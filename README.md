# mimic-onto

Este repositório contém uma ontologia desenvolvida para representar semanticamente o banco de dados MIMIC-IV, permitindo a possibilidade de integração de dados biomédicos e a execução de consultas semânticas. O projeto utiliza conceitos de Ontologias Baseadas em Descrição (OBDA) para conectar um banco de dados relacional a uma representação ontológica.

## Estrutura do Repositório

- `mimic-onto.owl`: Arquivo principal da ontologia em formato OWL.
- `IAOImport.owl`: Arquivo da ontologia IAO.
- `bfo-core.owl`: Arquivo da ontologia BFO.
- `cob-full.owl`: Arquivo da ontologia COB.
- `mimic-onto.obda`: Arquivo de mapeamento para integrar a ontologia com o banco de dados relacional.
- `queries/`: Pasta contendo exemplos de consultas SPARQL.

## Requisitos

Para testar e explorar esta ontologia, você precisará:

1. **Protégé com Ontop Plugin**: O Protégé é uma ferramenta para edição e exploração de ontologias. O plugin Ontop permite conectar a ontologia com o banco de dados e executar consultas SPARQL.
   - Faça o download do Protégé: [Protégé](https://protegeproject.github.io/). Eu utilizei a versão 5.6.1
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
     - **JDBC URL**: `jdbc:postgresql://<seu-host>:<porta>/<seu-banco>`.
     - **Username** e **Password**: Insira as credenciais do banco de dados.
3. **Coloque o arquivo .obda na mesma pasta e habilite a aba OntopMappings. Lá deve aparecer os mapeamentos.**

4. **Executando Consultas SPARQL**:
   - Vá para a aba `Ontop SPARQL` no Protégé.
   - Carregue uma das consultas da pasta `queries/` ou insira manualmente.
   - Clique em `Execute` para visualizar os resultados.

### Exemplos de Consultas

1. **Listar todos os pacientes com diagnósticos registrados**:
   ```sparql
   PREFIX : <http://www.semanticweb.org/emill/ontologies/2024/3/mimic-onto#>
   SELECT DISTINCT ?patient WHERE {
     ?patient a :SubjectOfCare ;
              :hasDiagnosis ?diagnosis .
   }
