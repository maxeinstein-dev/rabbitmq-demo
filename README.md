# RabbitMQ Demo

Repositório criado para estudo prático de mensageria com RabbitMQ usando Spring Boot.

A ideia aqui é entender o fluxo de uma aplicação orientada a eventos, saindo de uma mensagem publicada em fila, passando pela persistência no banco e chegando ao envio de e-mail. O foco principal hoje está no módulo `email`, que consome mensagens da fila `email-queue`, registra o processamento com JPA e usa SMTP para disparar o e-mail. O módulo `user` existe como base para evoluções futuras e para simular a integração entre serviços.

## Objetivo do projeto

Este projeto foi montado para fins de estudo e experimentação. Ele serve para praticar:

* consumo de mensagens com RabbitMQ;
* persistência com Spring Data JPA e PostgreSQL;
* envio de e-mails com Spring Mail;
* organização de um projeto em múltiplos módulos.

## Estrutura

* `email/` - serviço responsável por receber mensagens, persistir dados do e-mail e executar o envio.
* `user/` - serviço base para futuras integrações e cenários de produção de mensagens.

## Tecnologias

* Java
* Spring Boot
* RabbitMQ
* PostgreSQL
* Spring Data JPA
* Spring Mail
* Maven

## Como executar

### 1. Subir o banco do módulo de e-mail

Na pasta `email/`, execute:

```bash
docker compose up -d
```

Isso sobe o PostgreSQL local usado pelo serviço de e-mail.

### 2. Configurar as variáveis de ambiente

O envio de e-mail depende das variáveis:

* `EMAIL_USERNAME`
* `EMAIL_PASSWORD`

Também vale revisar o arquivo `email/src/main/resources/application.yml` para ajustar banco, RabbitMQ e SMTP ao seu ambiente.

### 3. Rodar os serviços

Você pode iniciar cada módulo separadamente com Maven a partir da raiz do repositório:

```bash
./mvnw -pl email spring-boot:run
./mvnw -pl user spring-boot:run
```

No Windows, use:

```bash
mvnw.cmd -pl email spring-boot:run
mvnw.cmd -pl user spring-boot:run
```

## Fluxo atual

1. Uma mensagem é publicada na fila `email-queue`.
2. O consumidor do módulo `email` recebe a mensagem.
3. O conteúdo é persistido no PostgreSQL.
4. O serviço tenta enviar o e-mail usando SMTP.

## Observações

* Este repositório não foi pensado como produto final, e sim como laboratório de estudo.
* A configuração de infraestrutura pode ser adaptada conforme o broker, o banco e o provedor de e-mail utilizados no seu ambiente.