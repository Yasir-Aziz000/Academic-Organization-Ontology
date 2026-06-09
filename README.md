# Academic Organization Ontology (OWL 2.0)

## Project Overview
This ontology provides a formal representation of an Academic Organization. It defines the relationships between personnel (Professors, Students), organizational structures (Departments), and academic activities (Courses). 

**Developed by:** Aun Ali  
**Course:** Knowledge Representation  
**Professor:** Matteo Cristani  
**Tool used:** Protégé 5.x (OWL 2.0 Specification)

## Technical Features
The ontology implements several advanced Semantic Web features to ensure logical consistency and automated inference:

* **Class Hierarchy:** Organized under `owl:Thing`, featuring structured subclasses for `Person`, `Organization`, `Course`, and `Location`.
* **Disjointness Axioms:** Ensured that top-level classes are disjoint (e.g., a `Course` cannot be a `Person`).
* **Object Properties:**
    * `teaches` / `isTaughtBy`: An **Inverse Property** relationship allowing bidirectional inference.
    * `enrolledIn`: Links Students to Courses.
    * `worksIn`: Links Staff to Departments.
* **Property Chains:** Implemented `worksIn o isPartOf` to allow the reasoner to infer organizational membership across hierarchies.
* **Data Properties:** Used **Functional Properties** such as `hasFullName` and `hasStudentID` with `xsd` data typing (string/integer).

## Reasoning and Consistency
The ontology has been validated using the **HermiT Reasoner**. All classes are consistent, and the inverse properties successfully infer relationships between individuals (e.g., Professor Snape and Potions 101).

## How to View
1. Download the `Academic_Organization_Ontology.owl` file.
2. Open the file in **Protégé**.
3. Start the Reasoner (HermiT or Pellet) to view inferred relationships.
