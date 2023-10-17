# Revisões de Código em JavaScript/TypeScript

## Guia de Estilo

Os desenvolvedores devem usar o [Prettier](https://prettier.io/) para formatação de código em JavaScript.

O uso de uma ferramenta automatizada de formatação de código, como o Prettier, impõe um guia de estilo amplamente aceito que foi colaborativamente desenvolvido por uma ampla gama de empresas, incluindo Microsoft, Facebook e AirBnB.

Para orientação de estilo de nível superior não abrangida pelo Prettier, seguimos o [Guia de Estilo AirBnB](https://github.com/airbnb/javascript).

## Análise de Código / Verificação de Estilo

### eslint

De acordo com as orientações delineadas no [roadmap TSLint da Palantir de 2019](https://medium.com/palantir/tslint-in-2019-1a144c2317a9),
o código TypeScript deve ser lintado com o [ESLint](https://github.com/eslint/eslint). Consulte a documentação do [typescript-eslint](https://typescript-eslint.io/) para obter mais informações sobre a verificação de código TypeScript com o ESLint.

Para [instalar e configurar a verificação de código com o ESLint](https://typescript-eslint.io/),
instale os seguintes pacotes como dependências de desenvolvimento:

```bash
npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

Adicione um arquivo `.eslintrc.js` à raiz do seu projeto:

```javascript
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  plugins: [
    '@typescript-eslint',
  ],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/eslint-recommended',
    'plugin:@typescript-eslint/recommended',
  ],
};
```

Adicione o seguinte ao `scripts` do seu `package.json`:

```json
"scripts": {
    "lint": "eslint . --ext .js,.jsx,.ts,.tsx --ignore-path .gitignore"
}
```

Isso vai lintar todos os arquivos `.js`, `.jsx`, `.ts`, `.tsx` em seu projeto e omitir quaisquer arquivos ou
diretórios especificados no seu `.gitignore`.

Você pode executar a verificação de lintagem com:

```bash
npm run lint
```

## Configuração do Prettier

[Prettier](https://prettier.io/docs/en/) é um formatador de código com opiniões.

[Guia de início](https://prettier.io/docs/en/integrating-with-linters.html).

Instale com `npm` como uma dependência de desenvolvimento:

```bash
npm install -D prettier eslint-config-prettier eslint-plugin-prettier
```

Adicione `prettier` ao seu `.eslintrc.js`:

```javascript
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  plugins: [
    '@typescript-eslint',
  ],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/eslint-recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier/@typescript-eslint',
    'plugin:prettier/recommended',
  ],
};
```

Isso aplicará o conjunto de regras `prettier` ao fazer a lintagem com o ESLint.

## Formatação Automática com o VS Code

O VS Code pode ser configurado para executar automaticamente `eslint --fix` ao salvar.

Crie uma pasta `.vscode` na raiz do seu projeto e adicione o seguinte ao seu
`.vscode/settings.json`:

```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
}
```

Por padrão, usamos as seguintes substituições que devem ser adicionadas à configuração do VS Code para padronizar as aspas simples, uma indentação de quatro espaços e para realizar a verificação de linting com ESLint:

```json
{
    "prettier.singleQuote": true,
    "prettier.eslintIntegration": true,
    "prettier.tabWidth": 4
}
```

## Configuração de Testes

Recomenda-se fortemente a configuração do Playwright em um projeto. É uma suíte de testes de código aberto criada pela Microsoft.

Para instalá-lo, use o seguinte comando:

```bash
npm install playwright
```

Como o Playwright mostra os testes no navegador, você deve escolher qual navegador deseja executar, a menos que esteja usando o Chrome, que é o padrão. Você pode fazer isso
## Validação de Build

Para automatizar esse processo no Azure DevOps, você pode adicionar o seguinte trecho ao seu arquivo de definição de pipeline yaml. Isso realizará a lintagem de quaisquer scripts na pasta `./scripts/`.

```yaml
- task: Npm@1
  displayName: 'Lint'
  inputs:
    command: 'custom'
    customCommand: 'run lint'
    workingDir: './scripts/'
```

## Hooks Pré-Commit

Todos os desenvolvedores devem executar o `eslint` em um hook pré-commit para garantir a formatação padrão. Recomendamos fortemente o uso de uma integração com o editor, como o [vscode-eslint](https://github.com/Microsoft/vscode-eslint), para fornecer feedback em tempo real.

1. Em `.git/hooks`, renomeie `pre-commit.sample` para `pre-commit`
1. Remova o código de exemplo existente nesse arquivo
1. Existem muitos exemplos de scripts para isso no gist, como [pre-commit-eslint](https://gist.github.com/linhmtran168/2286aeafe747e78f53bf)
1. Modifique de acordo para incluir arquivos TypeScript (inclua a extensão ts e certifique-se de que typescript-eslint esteja configurado)
1. Torne o arquivo executável: `chmod +x .git/hooks/pre-commit`

Como alternativa, [husky](https://github.com/typicode/husky) pode ser considerado para simplificar os hooks pré-commit.

## Checklist de Revisão de Código



Além do [Checklist de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve verificar esses itens específicos de revisão de código em JavaScript e TypeScript.

### Checklist de JavaScript / TypeScript

* [ ] O código segue nossos padrões de formatação e código? Executar prettier e ESLint no código não deve gerar avisos ou erros, respectivamente?
* [ ] A mudança re-implementa código que seria melhor servido por um módulo bem conhecido do ecossistema?
* [ ] É usado `"use strict";` para reduzir erros com variáveis não declaradas?
* [ ] São usados testes unitários sempre que possível, também para APIs?
* [ ] Os testes são organizados corretamente no padrão **Arrange/Act/Assert** e devidamente documentados dessa forma?
* [ ] São seguidas as melhores práticas para tratamento de erros, bem como as declarações `try catch finally`?
* [ ] As chamadas assíncronas seguem corretamente o padrão `doWork().then(doSomething).then(checkSomething)`, incluindo `expect`, `done`?
* [ ] Em vez de usar strings brutas, são usadas constantes na classe principal? Ou, se essas strings são usadas em vários arquivos/classes, existe uma classe estática para as constantes?
* [ ] Números mágicos são explicados? Não deve haver números no código sem pelo menos um comentário explicando por que estão lá. Se o número for repetitivo, existe uma constante/enum ou equivalente?
* [ ] Se houver um método assíncrono, o nome do método termina com o sufixo `Async`?
* [ ] Existe um nível mínimo de registro de eventos? Os níveis de registro usados são sensatos?
* [ ] A manipulação do fragmento de documento é limitada quando é necessário manipular vários subelementos?
* [ ] O código TypeScript é compilado sem gerar erros de lintagem?
* [ ] Em vez de usar strings brutas, são usadas constantes na classe principal? Ou, se essas strings são usadas em vários arquivos/classes, existe uma classe estática para as constantes?
* [ ] Números mágicos são explicados? Não deve haver números no código sem pelo menos um comentário explicando por que estão lá. Se o número for repetitivo, existe uma constante/enum ou equivalente?
* [ ] Existem comentários apropriados `/* */` nas várias classes e métodos?
* [ ] Operações pesadas são implementadas no backend, mantendo o controlador o mais leve possível?
* [ ] O tratamento de eventos no HTML é eficiente?
