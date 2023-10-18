# Acessibilidade

A acessibilidade é um componente crítico de qualquer projeto bem-sucedido e garante que as soluções que construímos sejam utilizáveis e apreciadas pelo maior número possível de pessoas. Embora o cumprimento das normas de acessibilidade seja obrigatório, a acessibilidade vai muito além do mero cumprimento. A acessibilidade é sobre usar técnicas como design inclusivo para infundir diferentes perspectivas e toda a gama de diversidade humana nos produtos que construímos. Ao incorporar acessibilidade em seu projeto desde a visão inicial até o MVP e além, você está promovendo um ambiente mais inclusivo para sua equipe e ajudando a fechar o "Divisão de Deficiência" que existe para muitas pessoas que vivem com deficiências.

## Começando

Se você é novo em acessibilidade ou está procurando uma visão geral dos fundamentos da acessibilidade, o Microsoft Learn oferece um ótimo curso de treinamento que abrange uma ampla gama de tópicos, desde a criação de conteúdo acessível no Office até o design de recursos de acessibilidade em seus próprios aplicativos. Você pode aprender mais sobre o curso ou começar em [Microsoft Learn: Fundamentos de Acessibilidade](https://learn.microsoft.com/en-us/learn/paths/accessibility-fundamentals/).

## Design Inclusivo

O design inclusivo é uma metodologia que abraça toda a gama de diversidade humana como um recurso para ajudar a construir melhores produtos e serviços. O design inclusivo complementa a acessibilidade, indo além das normas de conformidade de acessibilidade para garantir que os produtos sejam utilizáveis e apreciados por todas as pessoas. Ao aproveitar a metodologia de design inclusivo no início de um projeto, você pode esperar uma solução mais inclusiva e melhor para todos. O site [Microsoft Design Inclusivo](https://www.microsoft.com/design/inclusive/) oferece uma variedade de recursos para incorporar design inclusivo em seus projetos, incluindo atividades de design inclusivo que podem ser usadas em sessões de visão e design de arquitetura.

A metodologia de Design Inclusivo da Microsoft inclui os seguintes princípios:

### Reconhecer a exclusão

Projetar para inclusividade não apenas abre nossos produtos e serviços para mais pessoas, mas também reflete como as pessoas realmente são. Todos os seres humanos crescem e se adaptam ao mundo ao seu redor, e queremos que nossos designs reflitam isso.

### Resolver para um, estender para muitos

Todos têm habilidades e limites para essas habilidades. Projetar para pessoas com deficiências permanentes realmente resulta em designs que beneficiam as pessoas universalmente. Restrições são uma coisa linda.

### Aprender com a diversidade

Os seres humanos são os verdadeiros especialistas em se adaptar à diversidade. O design inclusivo coloca as pessoas no centro desde o início do processo, e essas perspectivas frescas e diversas são a chave para uma verdadeira percepção.

## Ferramentas

### Insights de Acessibilidade

[Insights de Acessibilidade](https://accessibilityinsights.io/) é uma solução gratuita e de código aberto para identificar problemas de acessibilidade em aplicativos Windows, Android e web. Insights de Acessibilidade podem identificar uma ampla gama de problemas de acessibilidade, incluindo problemas com tags de imagem alternativas ausentes, organização de cabeçalhos, ordem de tabulação, contraste de cores e muito mais. Além disso, você pode usar Insights de Acessibilidade para simular daltonismo para garantir que sua interface do usuário seja acessível àqueles que experimentam alguma forma de daltonismo. Você pode baixar Insights de Acessibilidade aqui: [https://accessibilityinsights.io/downloads/](https://accessibilityinsights.io/downloads/)

### Linter de Acessibilidade

[A Deque Systems](https://www.deque.com/) é uma empresa especializada em acessibilidade na web que oferece treinamento e ferramentas de acessibilidade para diversas organizações, incluindo a Microsoft. Uma das muitas ferramentas oferecidas pela Deque é o [axe Accessibility Linter para o VS Code](https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter). Esta extensão do VS Code utiliza o motor de regras [axe-core](https://github.com/dequelabs/axe-core/blob/develop/doc/rule-descriptions.md#:~:text=WCAG%202.0%20Level%20A%20%26%20AA%20Rules%20,%20%20%20%2011%20more%20rows%20?msclkid=604d209ed16411eca3c4c2af8c378e89) para identificar problemas de acessibilidade em HTML, Angular, React, Markdown e Vue. Utilizar um verificador de acessibilidade pode ajudar a garantir que os problemas de acessibilidade sejam abordados no início do ciclo de desenvolvimento.

## Práticas

### Testes de Acessibilidade

Os testes de acessibilidade são uma subcategoria especializada de testes de software e incluem ferramentas automatizadas e processos de teste manuais que variam de projeto para projeto. Além das ferramentas como Accessibility Insights mencionadas anteriormente, existem muitas outras soluções para testes de acessibilidade. A W3C fornece uma lista abrangente de ferramentas de avaliação e teste em seu site em [https://www.w3.org/WAI/ER/tools/](https://www.w3.org/WAI/ER/tools/).

Se você deseja adicionar testes automatizados aos seus Pipelines do Azure, pode considerar a [extensão Accessibility Testing](https://marketplace.visualstudio.com/items?itemName=DrewLewis.Accessibility) criada por Drew Lewis, um ex-funcionário da Microsoft.

É importante lembrar que apenas a automação das ferramentas não é suficiente; certifique-se de complementar seus testes automatizados com testes manuais. O Accessibility Insights (link acima) pode orientar os usuários em algumas etapas de teste manual.

### Fundamentos de Código e Documentação

Antes de começar os testes, você pode fazer algumas pequenas alterações na forma como escreve o código e a documentação.

- Documente! Além da documentação em texto, isso também significa comentários de código, nomes claros de variáveis e arquivos, e saídas de pipeline ou scripts que relatem claramente o sucesso ou a falha e forneçam detalhes.
- Evite letras minúsculas em nomes de variáveis e arquivos, hashtags, neologismos, etc. Use camelCase, snake_case ou outros métodos de separação de palavras.
- Introduza abreviações soletrando o termo completo e, em seguida, a abreviação entre parênteses.
- Use cabeçalhos de forma eficaz para dividir o conteúdo por tópico. Não use mais de um h1 por página e não pule níveis (por exemplo, use um h3 diretamente abaixo de um h1). Evite usar formatação para fazer algo *parecer* um cabeçalho quando não é.
- Use texto de link descritivo. Evite associar um link a frases como "Leia mais" e certifique-se de que o texto informe diretamente para onde o link aponta. O texto do link deve ser autoexplicativo.
- Ao incluir imagens ou diagramas, adicione texto alternativo (alt text). Isso nunca deve ser apenas "Imagem" ou "Diagrama" (ou similar). Em sua descrição, destaque o objetivo da imagem ou diagrama na página e o que ele se destina a transmitir.
- Prefira guias a espaços sempre que possível. Isso permite que os usuários usem sua largura de guia preferida, para que usuários com uma variedade de visões possam visualizar o código facilmente.

## Recursos Adicionais

* [Tecnologia e Ferramentas de Acessibilidade da Microsoft](https://www.microsoft.com/accessibility)
* [Diretrizes de Conteúdo Web para Acessibilidade (WCAG)](https://www.w3.org/TR/WCAG20/#intro)
* [Diretrizes e Requisitos de Acessibilidade | Guia de Estilo da Microsoft](https://learn.microsoft.com/en-us/style-guide/accessibility/accessibility-guidelines-requirements)
* [Guia de Estilo para Desenvolvedores do Google: Escrever Documentação Acessível](https://developers.google.com/style/accessibility)
