# baseProject

Base de automação de testes end-to-end com **[Cypress](https://www.cypress.io/)**, organizada no padrão **Page Object Model**, com geração de massa de dados via **Faker** e configuração de ambiente através de um arquivo **YAML**.

O projeto foi pensado como um *template*/esqueleto inicial: os arquivos e pastas dentro de `cypress/` usam o placeholder `<page.name>` no nome, que deve ser substituído pelo nome real da página/funcionalidade sendo testada (ex: `home`, `login`, `checkout`).

## 📁 Estrutura do projeto

```
baseProject/
├── baseProject.yaml
├── README.md
├── .gitignore
└── cypress/
    ├── e2e/
    │   └── <page.name>/
    │       └── <page.name>Spec.cy.ts
    ├── fixtures/
    │   └── <page.name>/
    │       └── <page.name>LocatorsJson.json
    ├── pageObjects/
    │   ├── dataForTesting/
    │   │   └── <page.name>/
    │   │       └── <page.name>DataForTesting.ts
    │   ├── enums/
    │   │   └── <page.name>/
    │   │       ├── enumDataForTesting/
    │   │       │   └── <page.name>EnumDataForTesting.ts
    │   │       └── enumLocatorsJson/
    │   │           └── <page.name>EnumLocatorsJson.ts
    │   └── interfaces/
    │       └── <page.name>/
    │           ├── interfaceDataForTesting/
    │           │   └── <page.name>InterfaceDataForTesting.ts
    │           └── interfaceLocatorsJson/
    │               └── <page.name>InterfaceLocatorsJson.ts
    └── support/
        ├── commands.ts
        ├── e2e.ts
        └── faker/
            ├── fakerDataGenerator.ts
            └── <page.name>/
                └── <page.name>FakerDataGenerator.ts
```

### O que cada camada representa

| Pasta | Responsabilidade |
|---|---|
| `cypress/e2e/<page.name>/` | Specs de teste (`.cy.ts`) da página, escritos com a sintaxe padrão do Cypress. |
| `cypress/fixtures/<page.name>/` | Massa de dados estática em JSON, incluindo o JSON de **locators** da página. |
| `cypress/pageObjects/dataForTesting/<page.name>/` | Dados de teste usados pelos specs (Page Object de dados). |
| `cypress/pageObjects/enums/<page.name>/` | Enums que padronizam chaves de dados de teste (`enumDataForTesting`) e de locators (`enumLocatorsJson`), evitando strings soltas no código. |
| `cypress/pageObjects/interfaces/<page.name>/` | Interfaces TypeScript que tipam os dados de teste (`interfaceDataForTesting`) e os locators (`interfaceLocatorsJson`) da página. |
| `cypress/support/commands.ts` | Comandos customizados do Cypress (`Cypress.Commands.add`). |
| `cypress/support/e2e.ts` | Arquivo de suporte carregado antes de cada spec `e2e`. |
| `cypress/support/faker/` | Geradores de dados fake (baseado em [Faker.js](https://fakerjs.dev/)) — um genérico (`fakerDataGenerator.ts`) e um por página (`<page.name>FakerDataGenerator.ts`). |

## ⚙️ Configuração (`baseProject.yaml`)

O arquivo `baseProject.yaml`, na raiz do projeto, centraliza os parâmetros usados para gerar/nomear os arquivos da página e definir o ambiente de execução:

```yaml
locators:
  version: 1.00
page:
  name: home
environment:
  name: staging
```

| Chave | Descrição |
|---|---|
| `locators.version` | Versão do schema/arquivo de locators utilizado pela página. |
| `page.name` | Nome da página/funcionalidade. **Esse valor substitui o placeholder `<page.name>` em todos os nomes de pasta e arquivo dentro de `cypress/`.** |
| `environment.name` | Ambiente alvo da execução dos testes (ex: `staging`, `production`, `qa`). |

Ao criar uma nova página no projeto, basta:
1. Duplicar a estrutura de pastas/arquivos que contém `<page.name>`.
2. Atualizar o valor de `page.name` no `baseProject.yaml`.
3. Substituir o placeholder `<page.name>` pelo valor real em todos os nomes de pasta/arquivo (manualmente, via script, ou com o utilitário `replace-page-name.bat`, no Windows).

> ⚠️ **Atenção (Windows):** os caracteres `<` e `>` não são permitidos em nomes de arquivo/pasta no Windows. Se for extrair ou clonar o projeto em ambiente Windows, será necessário ajustar o placeholder para um padrão compatível (ex: `{page.name}`) ou renomear os arquivos antes de versionar.

## 🚀 Como começar

### Pré-requisitos
- [Node.js](https://nodejs.org/) instalado
- [Cypress](https://docs.cypress.io/guides/getting-started/installing-cypress) como dependência do projeto

### Instalação

```bash
npm install
```

### Executando os testes

Abrir o Cypress em modo interativo:

```bash
npx cypress open
```

Rodar os testes em modo headless (linha de comando):

```bash
npx cypress run
```

## 🧩 Convenções do projeto

- **Page Object Model**: cada página tem seus dados de teste, enums, interfaces e locators isolados em pastas próprias, favorecendo reuso e manutenção.
- **Tipagem forte**: interfaces em `pageObjects/interfaces/` garantem que os dados de teste e os locators sigam um contrato bem definido.
- **Enums em vez de strings soltas**: `enumDataForTesting` e `enumLocatorsJson` evitam erros de digitação ao referenciar chaves de dados/locators pelo código.
- **Dados dinâmicos com Faker**: os geradores em `support/faker/` produzem massa de dados aleatória/realista para os testes, complementando os dados estáticos das fixtures.

## 🤝 Contribuindo

1. Crie uma branch a partir da `main`.
2. Siga a convenção de nomenclatura `<page.name>` (ou o padrão adotado) ao adicionar uma nova página.
3. Abra um Pull Request descrevendo a página/funcionalidade testada.

## 📄 Licença

Não especificada.