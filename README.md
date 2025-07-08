# leds-conectafapes-otel
Infraestrutura do SigNoz com suporte a OpenTelemetry Collector externo para observabilidade distribuída.

## Sobre o projeto
Este repositório contém a estrutura necessária para executar:

- **SigNoz**: uma plataforma de observabilidade open source que permite monitorar métricas, logs e traces.
- **Collector externo (OpenTelemetry Collector)**: um componente separado que coleta dados de aplicações instrumentadas e os envia para o SigNoz.

Essa separação permite mais flexibilidade, isolamento por ambiente ou aplicação, e facilita o escalonamento horizontal da coleta de dados.

> Para uso em ambiente de **desenvolvimento**, apenas o SigNoz precisa ser executado.  
> O collector externo é opcional e recomendado apenas em ambientes distribuídos ou mais complexos. 

## Pré-requisitos
Antes de começar, certifique-se de que você tem os seguintes requisitos instalados:

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/install/)

---

## Instalação

### 1. Subindo o ambiente principal do SigNoz

O SigNoz é responsável por receber, processar e exibir métricas, logs e traces.  
Ele utiliza o ClickHouse como base de dados principal.

```bash
cd signoz/deploy/docker
docker compose up -d
````

O painel do SigNoz ficará disponível em:

```
http://localhost:8080
```

### 2. Subindo o OpenTelemetry Collector externo

O Collector externo é responsável por receber os dados enviados pelas aplicações via OTLP (HTTP ou gRPC) e encaminhá-los para o SigNoz.

```bash
cd external-otel
docker compose up -d
```

Esse serviço escuta nas portas:

* `4317` para OTLP gRPC
* `4318` para OTLP HTTP

#### Variáveis de ambiente

Você pode configurar as seguintes variáveis no `docker-compose.yaml`:

* `CLICKHOUSE_HOST`: endereço IP ou hostname do ClickHouse (ex: `10.128.128.18`)
* `ALLOWED_ORIGINS`: origem permitida para CORS (ex: `http://localhost:5173`)

---

## Estrutura de diretórios

```
leds-conectafapes-otel/
├── external-otel/
│   ├── docker-compose.yaml
│   └── otel-collector-config.yaml
├── signoz/
│   └── deploy/
│       └── docker/
│           └── docker-compose.yaml
└── README.md
```

* `signoz/`: ambiente principal com SigNoz, ClickHouse, Zookeeper e outros serviços internos.
* `external-otel/`: OpenTelemetry Collector separado, configurado para enviar dados para o SigNoz via OTLP.
