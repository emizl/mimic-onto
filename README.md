# mimic-onto

This repository contains an ontology developed to semantically represent the MIMIC-IV database, enabling the integration of biomedical data and the execution of semantic queries. The project uses Ontology-Based Data Access (OBDA) concepts to connect a relational database to an ontological representation.

## Repository Structure

- `mimic-onto.owl`: Main ontology file in OWL format.
- `IAOImport.owl`: IAO ontology file.
- `bfo-core.owl`: BFO ontology file.
- `cob-full.owl`: COB ontology file.
- `mimic-onto.obda`: Mapping file to integrate the ontology with the relational database.

## Requirements

To test and explore this ontology, you will need:

1. **Protégé with Ontop Plugin**: Protégé is a tool for editing and exploring ontologies. The Ontop plugin allows you to connect the ontology with the database and execute SPARQL queries.
   - Download Protégé: [Protégé](https://protege.stanford.edu/). I used version 5.6.1.
   - Install the Ontop plugin directly in Protégé (available in the plugin menu).
2. **MIMIC-IV Database**: Ensure you have access to the properly configured MIMIC-IV database.
3. **Java**: A recent version to support Protégé and the Ontop plugin.

## How to Test

### Configuring the Ontop Plugin in Protégé

1. **Open Protégé**:
   - Load the ontology.owl file into Protégé. Ensure that the files for the other ontologies are placed in the same folder.

2. **Install and Configure the Ontop Plugin**:
   - In Protégé, go to the menu: File > Preferences > Plugins and install the Ontop plugin if it’s not already installed.
   - After installation, go to the "Ontop Mappings" tab. If it does not appear, enable it under VIEWS.

3. **Configure the Database Connection**:
   - Access the `Ontop Mapping Manager` and click on `Data Source Configuration`.
   - Fill in the connection details:
     - **JDBC Driver**: MySQL (or the appropriate driver for your version of MIMIC-IV).
     - **JDBC URL**: `jdbc:mysql://<your-host>:<port>/<your-database>`.
     - **Username** e **Password**: Enter the database credentials.
3. **Place the .obda file in the same folder and enable the Ontop Mappings tab. The mappings should appear there.**

4. **Running SPARQL Queries**:
   - Go to the Ontop SPARQL tab in Protégé.
   - Load one of the listed queries.
   - Click "Execute" to view the results.

### Example Queries

1. **List all patients with recorded diagnoses:**:
   ```sparql
   PREFIX : <http://www.semanticweb.org/emill/ontologies/2024/3/mimic-onto#>
   SELECT DISTINCT ?patient WHERE {
     ?patient a :SubjectOfCare ;
              :hasDiagnosis ?diagnosis .
   }
    ```
1. **Which patients, upon admission, underwent laboratory tests?:**:
   ```sparql
   PREFIX : <http://www.semanticweb.org/emill/ontologies/2024/3/mimic-onto#>
   PREFIX btl2: <http://purl.org/biotop/btl2.owl#>
   PREFIX iao: <http://www.semanticweb.org/filipesantana/ontologies/2024/2/IAOimport#>
   PREFIX cob: <http://purl.obolibrary.org/obo/COB_>
   SELECT DISTINCT ?patient WHERE { 
     ?admission a :Admission ;
          cob:0000072 ?lab . 
        ?lab a :LabMeasurements . 
     ?patient a :SubjectOfCare ;
        btl2:isPatientIn ?admission .
   } 


3. **Which medications are requested in a prescription and declared in the POE?:**:
   ```sparql
   PREFIX : <http://www.semanticweb.org/emill/ontologies/2024/3/mimic-onto#>
   PREFIX btl2: <http://purl.org/biotop/btl2.owl#>
   PREFIX cob: <http://purl.obolibrary.org/obo/COB_>
   SELECT DISTINCT ?medication ?prescription ?poe WHERE {
      ?prescription a :PrescriptionAct ;
                cob:0000070 ?poe ;
                cob:0000047 ?medication .
      ?medication a :MedicationAdministration .
      ?poe a :ProviderOrderEntry .
   } 
