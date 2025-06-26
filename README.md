# Passaporte Competências Digitais (pcd-v3)

[![Status do Deploy](https://github.com/your-org/pcd-v3/actions/workflows/firebase-hosting-merge.yml/badge.svg)](https://github.com/your-org/pcd-v3/actions/workflows/firebase-hosting-merge.yml)

Bem-vindo ao repositório oficial da aplicação "Passaporte Competências Digitais" v3. Este documento é o ponto de partida essencial para todos os developers e membros da equipa.

## 📖 Tabela de Conteúdos

1.  [O Projeto](#-o-projeto)
2.  [Arquitetura Central](#-arquitetura-central-monorepo-gerido)
3.  [Stack Tecnológica](#-stack-tecnológica)
4.  [🚀 Começar a Desenvolver (Getting Started)](#-começar-a-desenvolver-getting-started)
    *   [Pré-requisitos](#pré-requisitos)
    *   [Configuração no GitHub Codespaces](#configuração-no-github-codespaces)
5.  [📂 Estrutura do Projeto](#-estrutura-do-projeto)
6.  [⚙️ Scripts Disponíveis](#️-scripts-disponíveis)
7.  [🛠️ Workflow de Desenvolvimento](#️-workflow-de-desenvolvimento)
8.  [🔄 Pipeline de CI/CD](#-pipeline-de-cicd)
9.  [🔑 Variáveis de Ambiente](#-variáveis-de-ambiente)
10. [🧠 Decisões Arquiteturais Chave](#-decisões-arquiteturais-chave)

---

## 📍 O Projeto

O "Passaporte Competências Digitais" (PCD) é um ecossistema de aprendizagem digital desenhado para ser acolhedor, motivador e eficaz. O projeto utiliza uma arquitetura moderna para proporcionar uma experiência de utilizador rica e interativa, focada na gamificação e na aprendizagem social.

Esta versão (`v3`) foi construída de raiz para resolver os desafios técnicos encontrados em iterações anteriores, adotando as melhores práticas da indústria para garantir estabilidade, escalabilidade e segurança.

## 🏛️ Arquitetura Central: Monorepo Gerido

O problema fundamental do `pcd-firebase-v1` foi a ausência de isolamento entre o frontend e o backend. Para resolver isto, `pcd-v3` é um **monorepo formal**, gerido com **pnpm Workspaces**.

Isto significa que a aplicação Next.js (`web`) e as Cloud Functions (`functions`) são pacotes completamente independentes, cada um com as suas próprias dependências e configurações, eliminando a "contaminação de configuração" que causou conflitos no passado.

## 💻 Stack Tecnológica

| Categoria   | Tecnologia                                                                                              |
| :---------- | :------------------------------------------------------------------------------------------------------ |
| **Frontend**  | [**Next.js 14**](https://nextjs.org/) (App Router), [**React 18**](https://react.dev/), [**TypeScript**](https://www.typescriptlang.org/) |
| **Backend**   | [**Firebase**](https://firebase.google.com/) (Authentication, Firestore, Cloud Functions v2, Storage)   |
| **Estilização** | [**Tailwind CSS**](https://tailwindcss.com/) & [**HeroUI**](https://www.heroui.com/)                      |
| **Gestão de Estado** | [**Zustand**](https://zustand-demo.pmnd.rs/)                                                              |
| **Tooling**   | [**pnpm Workspaces**](https://pnpm.io/workspaces), [**ESLint**](https://eslint.org/), [**GitHub Actions**](https://github.com/features/actions) |
| **Ambiente**  | [**GitHub Codespaces**](https://github.com/features/codespaces)                                         |

---

## 🚀 Começar a Desenvolver (Getting Started)

Este guia assume a utilização do **GitHub Codespaces**, que é o nosso ambiente de desenvolvimento padrão para garantir consistência.

### Pré-requisitos

1.  Uma conta **GitHub**.
2.  Acesso ao projeto Firebase `pcd-v3` na [Consola da Firebase](https://console.firebase.google.com/).

### Configuração no GitHub Codespaces

1.  **Lançar o Codespace:**
    *   Navegue até à página principal do repositório `pcd-v3`.
    *   Clique no botão verde `<> Code`, selecione o separador `Codespaces` e clique em `Create codespace on main`.
    *   Aguarde alguns minutos. O ambiente será criado com todas as ferramentas necessárias (Node.js, pnpm, Firebase CLI) graças ao nosso ficheiro `.devcontainer/devcontainer.json`.

2.  **Autenticar na Firebase:**
    *   No terminal do VS Code que se abriu, execute o seguinte comando. A flag `--no-localhost` é **essencial** para que funcione no Codespaces.
        ```bash
        firebase login --no-localhost
        ```
    *   Siga as instruções: copie o URL, cole-o no seu browser local, faça login na sua conta Google e copie o código de autorização de volta para o terminal.

3.  **Associar o Projeto Firebase:**
    *   Verifique se o projeto `pcd-v3` está acessível e defina-o como o projeto ativo para este diretório.
        ```bash
        firebase projects:list
        firebase use pcd-v3
        ```

4.  **Instalar as Dependências:**
    *   Este único comando irá ler o `pnpm-workspace.yaml` e instalar as dependências para **todos os pacotes** do monorepo (web, functions, etc.).
        ```bash
        pnpm install
        ```

5.  **Iniciar o Servidor de Desenvolvimento:**
    *   Execute o script `dev` a partir da raiz do projeto.
        ```bash
        pnpm dev
        ```
    *   O Codespaces irá detetar que a porta `3000` está a ser utilizada e irá fazer o *forward* automaticamente. Um pop-up aparecerá a sugerir "Abrir no Browser". Pode também encontrar o URL no separador `Ports` do VS Code.

Parabéns! O seu ambiente de desenvolvimento está pronto e a correr.

---

## 📂 Estrutura do Projeto

A estrutura do monorepo é desenhada para uma clara separação de responsabilidades.

```
pcd-v3/
├── .devcontainer/       # Configuração para o ambiente do GitHub Codespaces
├── .github/             # Workflows de CI/CD com GitHub Actions
│   └── workflows/
├── apps/                # Aplicações implementáveis
│   └── web/             # A aplicação frontend Next.js
│       ├── src/
│       ├── next.config.mjs
│       └── package.json
├── packages/            # Pacotes partilhados (código, tipos, configurações)
│   ├── functions/       # O backend com as Cloud Functions (totalmente isolado)
│   │   ├── src/
│   │   └── package.json
│   ├── types/           # Definições de tipos TypeScript partilhadas
│   └── eslint-config-custom/ # Configuração base do ESLint para consistência
├── package.json         # O package.json da raiz (para scripts globais)
├── pnpm-workspace.yaml  # Ficheiro de definição do monorepo
└── tsconfig.base.json   # Configuração base do TypeScript
```

---

## ⚙️ Scripts Disponíveis

Execute estes scripts a partir da **raiz** do projeto.

| Comando      | Descrição                                                                                                                              |
| :----------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| `pnpm dev`     | Inicia o servidor de desenvolvimento do Next.js (`apps/web`) em modo de *hot-reloading*.                                                   |
| `pnpm build`   | Constrói a aplicação Next.js para produção. É executado automaticamente pelo nosso pipeline de CI/CD.                                      |
| `pnpm lint`    | Executa o ESLint em todos os pacotes para verificar a consistência do código.                                                              |
| `pnpm type-check` | Executa o compilador TypeScript em modo de verificação para encontrar erros de tipo. |

---

## 🛠️ Workflow de Desenvolvimento

1.  **Crie um Ramo (Branch):** A partir do ramo `main`, crie um novo ramo para a sua *feature* ou correção. Use uma convenção clara, por exemplo: `feature/login-page` ou `fix/button-style`.
    ```bash
    git checkout main
    git pull
    git checkout -b feature/nome-da-feature
    ```
2.  **Desenvolva:** Faça as suas alterações no Codespace. Faça *commits* pequenos e atómicos.
3.  **Crie um Pull Request (PR):** Assim que tiver algum progresso, crie um *Pull Request* no GitHub, mesmo que seja um *draft*. Isto permite que a equipa veja o seu trabalho.
4.  **Revisão com Preview Channels:** O nosso pipeline de CI/CD irá automaticamente construir a sua alteração e publicar um comentário no PR com um **URL de preview único**. Partilhe este link com a equipa (designers, PMs, etc.) para feedback visual e funcional.
5.  **Merge:** Após a aprovação, faça o *merge* do seu PR para o ramo `main`. Isto irá acionar o deploy para produção.

---

## 🔄 Pipeline de CI/CD

Utilizamos **GitHub Actions** para automação. A configuração encontra-se em `.github/workflows`.

*   **`firebase-hosting-pull-request.yml`**:
    *   **Gatilho:** Em cada *push* para um Pull Request.
    *   **Ação:** Instala dependências, constrói a aplicação e faz o deploy para um **canal de preview** temporário do Firebase Hosting.

*   **`firebase-hosting-merge.yml`**:
    *   **Gatilho:** Em cada *merge* ou *push* para o ramo `main`.
    *   **Ação:** Instala dependências, constrói a aplicação e faz o deploy para o **canal `live`** (produção) do Firebase Hosting.

---

## 🔑 Variáveis de Ambiente

**NUNCA FAÇA COMMIT DE FICHEIROS `.env` OU DE CHAVES SECRETAS!**

*   **Frontend (Next.js):**
    *   Para expor variáveis de ambiente ao browser, crie um ficheiro `.env.local` em `apps/web/`.
    *   As variáveis **têm** de começar com o prefixo `NEXT_PUBLIC_`.
    *   **Exemplo (`apps/web/.env.local`):**
        ```env
        NEXT_PUBLIC_FIREBASE_API_KEY="AIza..."
        NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN="pcd-v3.firebaseapp.com"
        # ... restantes chaves de configuração do Firebase
        ```

*   **Backend (CI/CD):**
    *   A chave da conta de serviço da Firebase (`FIREBASE_SERVICE_ACCOUNT_PCD_V3`) é gerida como um **Encrypted Secret** no GitHub. Foi configurada automaticamente pelo comando `firebase init hosting:github` e é utilizada de forma segura pelo nosso pipeline. Não precisa de a gerir manualmente.

---

## 🧠 Decisões Arquiteturais Chave

Este projeto foi construído sobre as lições aprendidas. As seguintes decisões são fundamentais para a sua estabilidade:

*   **`pnpm Workspaces`**: Garante o isolamento total entre o frontend e o backend, eliminando a causa raiz de erros de compilação e linting passados.
*   **`Firebase CLI webframeworks`**: A variável `FIREBASE_CLI_EXPERIMENTS: webframeworks` no nosso pipeline simplifica drasticamente o deploy, permitindo que a Firebase CLI lide com a complexidade de empacotar uma aplicação Next.js.
*   **Segurança `deny-by-default`**: As regras do Firestore serão escritas para negar todas as escritas por defeito, forçando todas as mutações de dados a passar por Cloud Functions validadas e seguras.
*   **`Zustand` para Estado Global**: Uma solução leve e poderosa para gerir o estado da UI (ex: utilizador autenticado), evitando a complexidade do Redux ou as ineficiências do Context API para estados globais.
