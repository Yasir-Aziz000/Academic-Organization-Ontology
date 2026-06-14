# Academic Organization Ontology — Project Report

**Author:** Yasir Aziz  
**Course:** Knowledge Representation  
**Professor:** Matteo Cristani  
**Tool:** Protégé 5.x (OWL 2.0) — validated with the HermiT reasoner

---

## 1. Objective
The goal of this project is to build a formal OWL 2.0 ontology that represents the
structure of an **academic organization** — its people, its organizational units, the
courses it offers, and the physical locations where teaching takes place — in a way that
allows a Description Logic reasoner to automatically **infer new knowledge** and **verify
logical consistency**.

## 2. Domain and Scope
The ontology models a university composed of:

- **People** — academic and administrative staff (Professors, Lecturers, Administrators,
  Deans) and students (Undergraduate, Graduate, PhD).
- **Organizations** — Faculties, Departments and Research Groups, arranged hierarchically.
- **Courses** — taught units that may depend on one another through prerequisites.
- **Locations** — Classrooms and Labs where courses are held.

## 3. Class Hierarchy
```
Person
├── Staff
│   ├── Professor        (⊑ teaches some Course)
│   ├── Lecturer
│   └── Administrator
│       └── Dean
└── Student              (⊑ exactly 1 hasStudentID)
    ├── Undergraduate
    └── Graduate
        └── PhD_Student

Organization → { Faculty, Department, Research_Group }
Course
Location → { Classroom, Lab }
```

## 4. Properties

### Object properties
| Property | Domain → Range | Characteristic |
|----------|----------------|----------------|
| `teaches` / `isTaughtBy` | Professor ↔ Course | inverse pair |
| `enrolledIn` / `hasStudent` | Student ↔ Course | inverse pair |
| `advisedBy` / `advises` | Student ↔ Professor | inverse pair |
| `isPartOf` | Organization → Organization | **transitive** |
| `hasPrerequisite` | Course → Course | **transitive** |
| `worksIn` | Staff → Organization | **property chain** `worksIn ∘ isPartOf` |
| `headedBy` | Department → Dean | **functional** |
| `locatedIn` | Course → Location | — |
| `memberOf` | Person → Research_Group | — |

### Data properties
| Property | Domain → Range | Characteristic |
|----------|----------------|----------------|
| `hasFullName` | Person → string | functional |
| `hasEmail` | Person → string | functional |
| `hasStudentID` | Student → integer | functional |
| `hasGPA` | Student → decimal | functional |
| `hasYear` | Student → integer | — |
| `hasCredits` | Course → integer | — |
| `hasCapacity` | Location → integer | — |

## 5. Axioms and Restrictions
- **Disjointness:** the four top classes `Person`, `Organization`, `Course`, `Location`
  are mutually disjoint; additionally `Staff`⊓`Student`, `Undergraduate`⊓`Graduate`,
  `Classroom`⊓`Lab`, and `Department`⊓`Faculty`⊓`Research_Group` are disjoint.
- **Existential restriction:** `Professor ⊑ teaches some Course`.
- **Qualified cardinality:** `Student ⊑ = 1 hasStudentID (integer)`.
- **Annotations:** `rdfs:label` and `rdfs:comment` document the main classes.

## 6. Reasoning Results (HermiT)
The ontology was validated with the **HermiT** reasoner. It is **consistent**, with no
unsatisfiable classes. The reasoner derived the following non-asserted facts, which
demonstrate the key OWL 2 constructs:

| Inferred fact | Mechanism |
|---------------|-----------|
| `Prof_Smith worksIn Faculty_of_Science` | property chain `worksIn ∘ isPartOf` |
| `Prof_Jones worksIn Faculty_of_Science` | property chain |
| `Advanced_Algorithms hasPrerequisite Intro_Programming` | transitivity of `hasPrerequisite` |
| `Intro_Programming isTaughtBy Prof_Smith` | inverse of `teaches` |
| `Carol` ⊑ `Graduate`, `Student`, `Person` | class-hierarchy subsumption |

## 7. Design Decisions
- **Staff and Student are disjoint.** This keeps the model clean and lets the reasoner
  flag any individual mistakenly typed as both. (If the domain required teaching assistants
  who are simultaneously students and staff, this axiom would be relaxed.)
- **`teaches` has domain `Professor`.** Asserting that someone teaches a course therefore
  lets the reasoner *infer* they are a Professor — a deliberate inference demonstration.
- **`worksIn` is defined through a property chain** rather than asserted at every level, so
  departmental membership automatically propagates up to the faculty.

## 8. How to Reproduce
1. Open `Academic_Organization.rdf` in **Protégé**.
2. Select **Reasoner → HermiT**, then **Start reasoner**.
3. Inspect the **inferred** (yellow) axioms on the individuals and the class hierarchy;
   confirm no class is highlighted red (inconsistent).

## 9. Files
| File | Description |
|------|-------------|
| `Academic_Organization.rdf` | The ontology (RDF/XML) |
| `Academic_Organization.owx` | The ontology (OWL/XML) |
| `Inferred_Output.owl` | Reasoner output containing all inferred axioms |
| `README.md` | Quick overview |
| `REPORT.md` | This report |
