
<div align="center" style="font-family: Arial, sans-serif; font-size: 20px; line-height: 1.5;">

# 🌟 **Curso AWS Developer - EDN** 🌟  
# 🌟 Developer Associate 🌟

<a href="https://escoladanuvem.org"><a href="https://aws.amazon.com/pt/?nc2=h_lg">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Amazon_Web_Services_Logo.svg/2560px-Amazon_Web_Services_Logo.svg.png" width="150" alt="AWS Logo">
</a>

<img src="https://raw.githubusercontent.com/pulumi/pulumi-aws-apigateway/main/assets/logo.png" width="80" alt="S3 Logo"/>-
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Amazon_Lambda_architecture_logo.svg/2048px-Amazon_Lambda_architecture_logo.svg.png" width="80" alt="Lambda Logo"/>-
<img src="https://github.com/HalleyVeras/Escola_da_Nuvem/blob/main/Documentos/download%20(4)_processed.png?raw=true" width="180" alt="EDN Logo">

</div>

# 🧪 Laboratório 6 - AWS Lambda com Aliases e API Gateway com Stages

## 📚 Visão Geral

Este laboratório demonstra uma prática essencial para gerenciar múltiplos ambientes (como desenvolvimento e produção) de forma organizada e segura em aplicações serverless, utilizando as funcionalidades de versionamento e direcionamento do Lambda e API Gateway.

> ⚠️ **Observação**: A interface do Console da AWS pode sofrer pequenas alterações visuais, mas os conceitos e estrutura geral permanecem consistentes.

---

## 🎯 Objetivos

Neste laboratório, você aprenderá a:

- Criar uma função AWS Lambda compatível com a integração Proxy do API Gateway.
- Publicar diferentes versões da função Lambda para representar estados distintos do código.
- Criar Aliases no Lambda (ex: `dev`, `prod`) apontando para versões específicas.
- Criar uma API REST no API Gateway.
- Configurar a integração do tipo Proxy entre a API Gateway e a função Lambda.
- Criar Stages no API Gateway (ex: Desenvolvimento, Producao) para representar ambientes.
- Integrar cada Stage com o Alias correspondente da função Lambda.
- Testar os endpoints para verificar o correto direcionamento das chamadas.

---

## 🧩 Cenário

Você está desenvolvendo uma API serverless que serve como backend de uma aplicação. É crucial testar novas funcionalidades em um ambiente isolado antes de liberar para produção. Utilizando Aliases do Lambda e Stages do API Gateway, você cria endpoints separados como `/dev/hello` e `/prod/hello`, cada um apontando para versões distintas do código.

---

## ✅ Pré-requisitos

- Conta AWS ativa
- Permissões IAM para criar e gerenciar funções Lambda e API Gateway

---

## 🛠️ Passo a Passo

### 🔸 Parte 1: Criação da Função Lambda

1. Acesse o [AWS Lambda Console](https://console.aws.amazon.com/lambda).
2. Clique em **Criar função** > **Criar do zero**.
3. Nome: `minha-funcao-proxy-lab-seunomesobrenome`.
4. Runtime: Python 3.9.
5. Arquitetura: x86_64.
6. Permissões: selecione **Criar nova função com permissões básicas do Lambda**.
7. Clique em **Criar função**.

#### Código da função (desenvolvimento):

8. Na aba **Código**, substitua pelo código do ambiente de desenvolvimento.
9. Clique em **Deploy**.

#### Testar função ($LATEST):

10. Aba **Testar** > **Criar novo evento**.
11. Nome: `teste-simulado`.
12. Modelo: `API Gateway AWS Proxy`.
13. No JSON, altere `requestContext.stage` para `"test-stage"`.
14. Clique em **Salvar** e depois **Testar**.

---

### 🔸 Parte 2: Versionamento e Aliases

#### Publicar Versão 1 (desenvolvimento)

1. Aba **Versões** > **Publicar nova versão**.
2. Descrição: `Versão inicial da função de desenvolvimento`.
3. Clique em **Publicar**.

#### Criar Alias "dev"

4. Aba **Aliases** > **Criar alias**.
5. Nome: `dev`, versão: `1`.
6. Clique em **Salvar**.

> 💡 Copie o ARN do Alias "dev".

#### Código da versão produção

7. Substitua o código pelo ambiente de produção.
8. Clique em **Deploy** e **Testar**.

#### Publicar Versão 2 (produção)

9. Aba **Versões** > **Publicar nova versão**.
10. Descrição: `Versão para ambiente de produção`.
11. Clique em **Publicar**.

#### Criar Alias "prod"

12. Aba **Aliases** > **Criar alias**.
13. Nome: `prod`, versão: `2`.
14. Clique em **Salvar**.

> 💡 Copie o ARN do Alias "prod".

---

### 🔸 Parte 3: API Gateway com Stages

#### Criar API REST

1. Vá para [API Gateway](https://console.aws.amazon.com/apigateway).
2. Clique em **Criar API** > **API REST** > **Compilar**.
3. Nome: `minha-api-proxy-lab-seunomesobrenome`.
4. Endpoint: **Regional** > **Criar API**.

#### Criar recurso `/hello`

1. Com a raiz `/` selecionada, clique em **Criar recurso**.
2. Nome: `hello` > **Criar recurso**.

#### Criar método GET com Proxy (desenvolvimento)

1. Selecione `/hello` > **Criar método**: GET.
2. Integração: Função Lambda (habilite Proxy).
3. Função: Cole o ARN do Alias **dev**.
4. Clique em **Criar método**.

#### Implantar Estágio "Desenvolvimento"

1. Clique em **Implantar API** > Novo estágio: `Desenvolvimento`.
2. Descrição: `API/Lambda - Versão Desenvolvimento` > **Implantar**.

#### Integrar com produção

1. Volte em **Recursos** > método GET > **Solicitação de integração** > **Editar**.
2. Altere ARN para o Alias **prod**.
3. Clique em **Salvar**.

#### Implantar Estágio "Producao"

1. Clique em **Implantar API** > Novo estágio: `Producao`.
2. Descrição: `API/Lambda - Producao` > **Implantar**.

---

### 🔸 Parte 4: Testes

1. Vá até **Estágios** > Expanda `Desenvolvimento` > GET.
2. Copie a **Invoke URL** e acesse no navegador.
3. Repita para o estágio `Producao`.

---



## 🚀 Conclusão

Utilizar **Aliases** no Lambda junto com **Stages** no API Gateway permite que você:

- Gerencie diferentes ambientes de forma clara e segura.
- Reduza erros em produção ao testar previamente em desenvolvimento.
- Mantenha um único ponto de entrada para a API com múltiplos comportamentos.
