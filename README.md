# Read First

## Run POC

Build and Generate images
```sh
./build-and-generate-images.sh
```

Run
```sh
docker-compose up
```

Request HTTP
```sh
curl -X GET http://localhost:9999/customers
```


See distributed-traceId, in another terminal tab
```
docker-compose logs | grep [YOUR_TRACE_ID]
```

## Architecture and Design

Sync :: `GET /customers`
![image](https://github.com/diegolirio/spring-boot-3-observability/assets/3913593/9cd769b9-afd9-449a-9e6a-edef05b9ec87)

Async :: `POST /customers` and then `service1 produces message (customer) to topic and detail consumes and persist one`
![image](https://github.com/diegolirio/spring-boot-3-observability/assets/3913593/7049c6f8-a311-4517-95cd-c35e5378b9e0)



<h1 align="center">
  <span style="color: #890AF9;">zoe</span>
  <span style="color: #0000FF;">Docker</span>
</h1>

<p align="center">
<img src="./static/banner.gif" alt="Docker" width="700" height="200">
</p> 
<p align="center">
<img src="https://img.shields.io/badge/Docker-0747a6?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
<img src="https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white" alt="Markdown">
<img src="https://img.shields.io/badge/Bitbucket-0747a6?style=for-the-badge&logo=bitbucket&logoColor=white)" alt="Bitbucket">
<img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white" alt="Postgresql">
<img src="https://img.shields.io/badge/ORACLE-orange?style=for-the-badge&logo=oracle&logoColor=white" alt="Postgresql">
<img src="https://img.shields.io/badge/zoe-purple?style=for-the-badge&logo=oracle&logoColor=green" alt="zoe">
</p>
 
Este repositÃ³rio tem a premissa de tentar fornecer de uma maneira fÃ¡cil ou prÃ¡tica e consistente de subir ambientes local usando Docker. Ele usa imagens do Amazon Elastic Container Registry (ECR) para fornecer uma base confiÃ¡vel e segura para seus ambientes.

O mesmo Ã© estruturado para suportar vÃ¡rios cenÃ¡rios. Cada cenÃ¡rio Ã© representado por um arquivo de composiÃ§Ã£o Docker Compose, que define as configuraÃ§Ãµes para os contÃªineres Docker que compÃµem o ambiente. O Projeto nÃ£o preve todos os cenarios possÃ­veis, sedo necessÃ¡rio e obrigatÃ³rio a contribuiÃ§Ã£o entre todos, pois com isso, chegaremos o mais prÃ³ximo possivel para uma ferramenta cada vez mais consistente para uso.

Por exemplo, um cenÃ¡rio de desenvolvimento pode incluir um contÃªiner para o servidor web, um contÃªiner para o banco de dados e um contÃªiner para as ferramentas de desenvolvimento. O arquivo de composiÃ§Ã£o Docker Compose definiria as configuraÃ§Ãµes para cada contÃªiner, como a imagem Docker a ser usada, a porta a ser exposta e as variÃ¡veis de ambiente a serem definidas.

# Indice

- [ðŸ“˜ Estrutura do Projeto](#estrutura-projeto)
- [âš ï¸ PrÃ©-requisitos](#pre-requisitos)
- [ðŸ³ NÃ£o sei docker e agora?](#nao-sei-docker-e-agora)
- [ðŸ“” Como contribuir?](#como-contribuir)
- [ðŸ“” Como as imagens sÃ£o geradas?](#como-as-imagens-sao-geradas)
- [ðŸ“š Quais os cenÃ¡rios Mapeados?](#quais-os-cenarios-mapeados)
    - [ðŸ“– dev-dfe-client-emissao.yml](#dev-dfe-client-emissaoyml)
    - [ðŸ“– dev-dfe-client-gestao.yml](#dev-dfe-client-gestaoyml)
    - [ðŸ“– dev-emissao-bureau.yml](#dev-emissao-bureauyml)
    - [ðŸ“– dev-emissao-inhouse.yml](#dev-emissao-inhouseyml)
    - [ðŸ“– dev-engines.yml](#dev-enginesyml)
    - [ðŸ“– dev-extrator-oracle.yml](#dev-extrator-oracleyml)
    - [ðŸ“– dev-importa.yml](#dev-importayml)
    - [ðŸ“– dev-mde.yml](#dev-mdeyml)
    - [ðŸ“– dev-only-orc-amq.yml](#dev-only-orc-amqyml)
    - [ðŸ“– dev-only-psql-amq-orc.yml](#dev-only-psql-amq-orcyml)
    - [ðŸ“– dev-only-psql-amq.yml](#dev-only-psql-amqyml)
    - [ðŸ“– dev-orc-monitor.yml](#dev-orc-monitoryml)
    - [ðŸ“– dev-psql-monitor.yml](#dev-psql-monitoryml)
    - [ðŸ“– dev-recebe.yml](#dev-recebeyml)
    - [ðŸ“– dev-relatorio.yml](#dev-relatorioyml)
    - [ðŸ“– dev-rest.yml](#dev-restyml)
- [ðŸ“™ Quais sÃ£o as portas de acesso?](#quais-sao-as-portas-de-acesso)
- [ðŸ“™ Como subir o ambiente?](#como-subir-o-ambiente)
- [ðŸ“™ Como escolher a versÃ£o?](#como-escolher-a-versao)
- [ðŸ“™ Como editar a configuraÃ§Ã£o de um serviÃ§o?](#como-configurar-o-servico)
- [ðŸ“™ Como aplicar um fix/class/correÃ§Ã£o pontual para teste?](#como-aplicar-fix)
- [ðŸ“™ Como fazer o restore do dump POSTGRES?](#como-restore-postgres)
- [ðŸ“™ Como fazer testes de impressÃ£o?](#como-fazer-impressao)
- [ðŸŒ± Seeds?](#seeds)
- [ðŸ“œ Scripts](#scripts)

<a id="estrutura-projeto"></a>
# ðŸ“˜ Estrutura do Projeto

A estrutura do projeto foi organizada para proporcionar uma experiÃªncia de desenvolvimento eficiente e fÃ¡cil manutenÃ§Ã£o. A seguir, uma breve descriÃ§Ã£o de cada diretÃ³rio:

```
â”œâ”€â”€ dump
â”œâ”€â”€ compose
â”œâ”€â”€ configs
â”œâ”€â”€ scripts
â”œâ”€â”€ static
â””â”€â”€ volumes
```

- `dump`: ContÃ©m o dump base para o banco postgres, contendo alguns dados.
- `compose`: ContÃ©m os arquivos de composiÃ§Ã£o Docker Compose para diferentes cenÃ¡rios de desenvolvimento.
- `configs`: DiretÃ³rio para arquivos de configuraÃ§Ã£o adicionais e volumes, se necessÃ¡rio.
- `scripts`: Scripts auxiliares para o ambiente
- `static`: Arquivos de imagens utilizadas para a documentaÃ§Ã£o 
- `volumes`: DiretÃ³rio para mapeamento de volumes nos containers, especificamente pensando para serviÃ§os que usam o modulo [PDES - Veja o artigo na Wiki](http://zoe.wiki.br/index.php?title=PDES)
