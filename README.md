# mimic-onto

Este repositório contém uma ontologia desenvolvida para representar semanticamente o banco de dados MIMIC-IV, permitindo a possibilidade de integração de dados biomédicos e a execução de consultas semânticas. O projeto utiliza conceitos de Ontologias Baseadas em Descrição (OBDA) para conectar um banco de dados relacional a uma representação ontológica.

## Estrutura do Repositório

- `mimic-onto.owl`: Arquivo principal da ontologia em formato OWL.
- `IAOImport.owl`: Arquivo da ontologia IAO.
- `bfo-core.owl`: Arquivo da ontologia BFO.
- `cob-full.owl`: Arquivo da ontologia COB.
- `mimic-onto.obda`: Arquivo de mapeamento para integrar a ontologia com o banco de dados relacional.

## Requisitos

Para testar e explorar esta ontologia, você precisará:

1. **Protégé com Ontop Plugin**: O Protégé é uma ferramenta para edição e exploração de ontologias. O plugin Ontop permite conectar a ontologia com o banco de dados e executar consultas SPARQL.
   - Faça o download do Protégé: [Protégé](https://protege.stanford.edu/). Eu utilizei a versão 5.6.1
   - Instale o plugin Ontop diretamente no Protégé (disponível no menu de plugins).
2. **Banco de Dados MIMIC-IV**: Certifique-se de ter acesso ao banco de dados MIMIC-IV devidamente configurado.
3. **Java**: Uma versão recente para suportar o Protégé e o plugin Ontop.

## Como Testar

### Configurando o Plugin Ontop no Protégé

1. **Abra o Protégé**:
   - Carregue o arquivo `ontology.owl` no Protégé. Certifique-se que os arquivos das outras ontologias sejam colocados na mesma pasta.

2. **Instale e Configure o Plugin Ontop**:
   - No Protégé, acesse o menu `File > Preferences > Plugins` e instale o plugin Ontop, caso ainda não esteja instalado.
   - Após a instalação, vá para a aba "Ontop Mappings". Caso ela não apareça, habilite em VIEWS.

3. **Configure a Conexão com o Banco de Dados**:
   - Acesse `Ontop Mapping Manager` e clique em `Data Source Configuration`.
   - Preencha as informações da conexão:
     - **JDBC Driver**: MySQL (ou o driver apropriado para a sua versão do mimic-iv).
     - **JDBC URL**: `jdbc:mysql://<seu-host>:<porta>/<seu-banco>`.
     - **Username** e **Password**: Insira as credenciais do banco de dados.
3. **Coloque o arquivo .obda na mesma pasta e habilite a aba OntopMappings. Lá deve aparecer os mapeamentos.**

4. **Executando Consultas SPARQL**:
   - Vá para a aba `Ontop SPARQL` no Protégé.
   - Carregue uma das consultas listadas.
   - Clique em `Execute` para visualizar os resultados.

### Exemplos de Consultas

1. **Listar todos os pacientes com diagnósticos registrados**:
   ```sparql
   PREFIX : <http://www.semanticweb.org/emill/ontologies/2024/3/mimic-onto#>
   SELECT DISTINCT ?patient WHERE {
     ?patient a :SubjectOfCare ;
              :hasDiagnosis ?diagnosis .
   }
    ```
1. **Quais são os pacientes que, ao participar de uma admissão, realizaram exames laboratoriais?**:
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


3. **Quais são as medicações solicitadas numa prescrição e declaradas na POE?**:
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
