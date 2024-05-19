# Event API (CC58D-1-2024)

API desenvolvida como requisito de nota na disciplina de Tópicos Avançados em Computação da Universidade Tecnológica Federal do Paraná.
Consiste em uma API para registro de eventos significativos no ambiente ou alarmes gerados pelos dispositivos IoT. Isso pode incluir eventos como falha de comunicação, leitura fora dos limites esperados, detecção de intrusão, etc.

Para isso foi utilizado o Node com o framework Express e banco de dados NoSQL MongoDB para persistência de dados com o auxílio da biblioteca Mongoose.

Para esta API foi implementado um sistema de autenticação utilizando JWT, favorecendo a segurança das requisições da API. 

## Preparando o ambiente

Instale as dependências do projeto por meio do comando `npm install`.

Crie uma conta no MongoDB Atlas e inicialize um cluster, ou inicialize um banco de dados da sua preferência (para isso, será necessário refatorar o código para atender às peculiaridades do banco em questão).

## Rodando

Para executar o projeto, no terminal digite `npm run dev`.

- O projeto pode ser acessado em `http://localhost:3000`.

- A aplicação está rodando. Aproveite!!!