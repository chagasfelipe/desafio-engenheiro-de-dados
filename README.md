# desafio-engenheiro-de-dados



O desafio proposto consiste em criar pipelines ETL para consumo em Data Visualization.

Requisitos:
    Projeto GCP
    Chave Json do Usuário Proprietário do Projeto
    Conta com os privilégios(Papéis):
        Administrador do Dataflow
        Administrador do Composer
        Administrador do BigQuery
        Administrador de Ambiente e Objetos do Storage
        Administrador do Compute
        Trabalhador do Dataflow
        Usuário da Conta de Serviço



Ambiente:
    Google Cloud Platform:
        Cloud Storage (Data Lake).
        Dataflow com Python SDK (Ingestão de Dados).
        BigQuery (Data Warehouse).
        Data Studio (Data Visualization).
        Cloud Shell.
    Python.
    Apache Beam.



Modelo Conceitual dos Dados:
[imagem mer]

Cada arquivo foi representado como uma entidade:
tb_price_quote (Tabela Dimensional) [link price_quote]: Contém os preços das cotações dos fornecedores.
tb_bill_materiais (Tabela Fato) [link bill_of_materials]: Contém a registros sobre os tubos e seus compenentes que foram selecionados.
tb_components (Tabela Dimensional) [link comp_boss.csv]: Contém os detalhes dos componentes utilizados ou não na cotação de algum tubo.



Configurar Credenciais do GCP:
    Configure o gsutil para utilizar as credencias da conta GCP, digite no Cloud Shell o comando:
    gcloud auth login

Ao digitar o comando, será carregado um link para gerar uma chave de autenticação, acesse o link, autentique com sua conta GCP, copie a chave e cole no Cloud Shell



Criação Bucket (Data Lake):

    Strutura do Dataset:
    [diagrama estrutura dataset]

    Para criar o Bucket, digite o comando para criar o bucket:
        gsutil mb -p desafio-engenheiro-de-dados-data-lake -c STANDARD -l US-EAST1 -b on gs://bucket-desafio-engenheiro-dados
        Sintaxe: gsutil mb -p <ID DO PROJETO GCP> -c <CLASSE DE ARMAZENAMENTO> -l <REGIÃO> -b on gs://<NOME DO BUCKET>



Upload dos Arquivos no Data Lake:
    Acesso o Bucket criado e faça upload dos arquivos no Data lake criado bill_of_materials.csv, comp_boss.csv e price_quote.csv localizados no diretório data_files [link do diretório]

Instalação de Pacotes e Configuração do Ambiente Virtual:
    virtualenv -p python3 venv
    source venv/bin/activate
    pip install apache-beam[gcp]

    No terminal do Cloud Shell, crie a pasta para armazenar os recursos para criação dos pipelines com o comando:
        mkdir resources
        cd resources/

    Acesse o Editor do Cloud Shell, faça upload de todos o arquivos localizados na pasta scripts_and_support_files na pasta "resources" [link do diretório].

    Baixando a chave json do usuário:
        Acesse https://console.cloud.google.com/iam-admin/serviceaccounts 
        Na aba “Ações”, clique nas reticências > Gerenciar Chaves > Clique no botão “ADICIONAR CHAVE” > Criar nova Chave > Selecione JSON e clique no botão criar.
        O arquivo json da chave será baixada automaticamente para seu computador, acesse o Editor do Cloud Shell e faça upload do arquivo na pasta "resources".


Criação do Dataset no BigQuery(Data Warehouse):
Utilize o comando abaixo para criar o dataset "industrial_machine_product_data":
python3 create_bigquery_dataset.py



Estrutura do pipeline:
    [imagem diagrama pipeline]
    Os dados não processados são armazenados no Cloud Storage, o Python sdk extraí o arquivo que é processado pelo Dataflow e inserido no BigQuery para que sejam construídas as Views para consumo do Data Studio ou alguma Data Visualization Tool.



Execução dos Jobs:
    Para executar os pipelines no Dataflow para que os arquivos sejam ingeridos do Cloud Storage(Data Lake) para o BigQuery(Data Warehouse), utilize os comandos abaixo para cada arquivo:
        python3 job_load_bill_of_materials.py
        python3 job_load_price_quote.py
        python3 job_load_comp_boss.py



Criação das Views BigQuery:

    Abaixo os códigos para criação das Views para consumo no Data Studio para criação de relatórios:
    [código sql das views]



Data Visualization:
    Foram criados os seguintes relatórios:

    [Link Público do Relatório no Data Studio]

Repositório destinado ao Desafio Engenheiro de Dados - Dotz
