# Desafio3AWSCloudFormation
# README.md — Laboratório: AWS CloudFormation

## Sumário

1. Visão geral
2. Objetivos do laboratório
3. Arquitetura

4. Descrição do template CloudFormation
5. Passo a passo — deploy
6. Como validar que deu certo

7. Limpeza e custos


---

## 1. Visão geral

Este laboratório mostra como **projetar, criar e operar uma stack AWS usando CloudFormation**. O objetivo é aprender a criar uma stack utilizando um template.

Cenário prático: receber um arquivo CSV (enviado por uma máquina física ou cliente) em um **S3 bucket**, disparar processamento via **Lambda**, e mostrar os registros que estão nesse documento. No caso será um documento CSV. Tudo orquestrado por um template CloudFormation.

## 2. Objetivos do laboratório

Ao final teremos a seguinte capacidade:

* Entender a estrutura de um template CloudFormation
* Criar stacks via Console e AWS CLI
* Validar recursos criados (S3, Lambda)

## 3. Arquitetura

Descrição sequencial:

1. Usuário / máquina física faz upload de `arquivo.csv` para o S3 bucket público/privado definido.
2. Evento S3 (`s3:ObjectCreated:*`) aciona uma função Lambda via trigger configurada.
3. Lambda processa o CSV e mostra os registros.


> Diagrama (simplificado):

```
[Cliente] --> (upload CSV) --> [S3 Bucket] --> (evento ObjectCreated) --> [Lambda] --> {mostra registro}
```



## 4. Descrição do template CloudFormation

O `template.yaml` inclui as seções tipicamente usadas:

* **Parameters** — variáveis passadas no deploy (ex.: Nome do stack, ambiente, prefixo de nomes).
* **Mappings** — mapeamentos (ex.: AMI por região, caso use EC2).
* **Resources** — definição dos recursos (S3 Bucket, Lambda Function, DynamoDB Table, IAM Roles/Policies, Event Notification, CloudWatch Log Group, SNS Topic opcional).
* **Outputs** — informações úteis (ex.: ARN do bucket, nome da tabela DynamoDB, URL do console).

### Recursos (exemplo mínimo)

* `S3BucketUpload`: Bucket S3 com versão desativada (ou ativada conforme exercício), notificação para Lambda.
* `LambdaExecutionRole`: Role com políticas mínimas — `s3:GetObject`, `dynamodb:PutItem`, `logs:CreateLogStream`, `logs:PutLogEvents`.
* `ProcessorLambda`: Função Lambda (Python 3.11/3.13) empacotada em object S3 (ou referência inline para testes simples).

## 5. Passo a passo — deploy

### Opção A — Console

1. Acesse o Console AWS → CloudFormation.
2. Clique em **Create stack** → **With new resources (standard)**.
3. Selecione **Template file** (envie `AWSTemplateFormatVersion 2010-09-09.yaml`).
4. Clique em **Create stack** e aguarde.


## 6. Como validar que deu certo

1. Verifique status da stack:
  Status esperado: `CREATE_COMPLETE`.
2. Verifique o bucket S3 criado (nome no output):
3. Faça upload de um CSV de teste (local):



## 7. Limpeza e custos

Para não pagar por recursos após o laboratório, remova a stack:


Verifique se todos os recursos foram removidos (às vezes, buckets não são apagados automaticamente se não estiverem vazios). Se um bucket restante impedir a remoção, esvazie-o manualmente:


