# Academic Organization Ontology (OWL 2.0)

## Project Overview
This ontology provides a formal representation of an Academic Organization. It models the relationships between personnel (Professors, Lecturers, Administrators, Deans, Students), organizational structures (Faculties, Departments, Research Groups), academic activities (Courses) and physical spaces (Classrooms, Labs).

**Developed by:** Yasir Aziz  
**Course:** Knowledge Representation  
**Professor:** Matteo Cristani  
**Tool used:** Protégé 5.x (OWL 2.0 Specification)

## Class Hierarchy
```
owl:Thing
├── Person
│   ├── Staff
│   │   ├── Professor        (≡ teaches some Course)
│   │   ├── Lecturer
│   │   └── Administrator
│   │       └── Dean
│   └── Student              (= exactly 1 hasStudentID)
│       ├── Undergraduate
│       └── Graduate
│           └── PhD_Student
├── Organization
│   ├── Faculty
│   ├── Department
│   └── Research_Group
├── Course
└── Location
    ├── Classroom
    └── Lab
```

## Technical Features
The ontology implements a wide range of Semantic Web / OWL 2 features to enable automated inference:

* **Class Hierarchy & Disjointness:** Top-level classes (`Person`, `Organization`, `Course`, `Location`) are mutually disjoint. Additional disjointness is asserted between `Staff`/`Student`, `Undergraduate`/`Graduate`, and `Classroom`/`Lab`.
* **Object Properties:**
    * `teaches` / `isTaughtBy` — **inverse** pair (bidirectional inference).
    * `advisedBy` / `advises` — **inverse** pair linking students and supervisors.
    * `enrolledIn` / `hasStudent` — **inverse** pair.
    * `isPartOf` — **transitive** (Department ⊑ Faculty membership propagates).
    * `hasPrerequisite` — **transitive** (prerequisite chains are inferred).
    * `worksIn` — uses the **property chain** `worksIn ∘ isPartOf` to infer membership across the org hierarchy.
    * `headedBy` — **functional** object property (a department has one dean).
    * `locatedIn`, `memberOf` — additional relations.
* **Data Properties:** **Functional** properties `hasFullName`, `hasEmail`, `hasStudentID`, `hasGPA`, plus `hasYear`, `hasCredits`, `hasCapacity`, all with `xsd` datatyping.
* **Restrictions:** `Professor ⊑ teaches some Course` (existential) and `Student ⊑ = 1 hasStudentID` (qualified cardinality).
* **Annotations:** `rdfs:label` and `rdfs:comment` on the main classes.

## Sample Inferences (run the reasoner to see these)
* `Prof_Smith worksIn ComputerScience_Dept`, and `ComputerScience_Dept isPartOf Faculty_of_Science` ⇒ **`Prof_Smith worksIn Faculty_of_Science`** (property chain).
* `Advanced_Algorithms hasPrerequisite Data_Structures hasPrerequisite Intro_Programming` ⇒ **`Advanced_Algorithms hasPrerequisite Intro_Programming`** (transitivity).
* `Prof_Smith teaches Intro_Programming` ⇒ **`Intro_Programming isTaughtBy Prof_Smith`** (inverse).
* `Carol` is a `PhD_Student` ⇒ inferred to be a `Graduate`, `Student`, and `Person`.

## Reasoning and Consistency
The ontology is designed to be **consistent** under the **HermiT** reasoner.

## How to View / Verify
1. Open `Academic_Organization.rdf` (or `Academic_Organization.owx`) in **Protégé**.
2. Go to **Reasoner → HermiT** and click **Start reasoner**.
3. Confirm there are **no red (inconsistent) classes** and inspect the inferred (yellow) axioms listed above.
