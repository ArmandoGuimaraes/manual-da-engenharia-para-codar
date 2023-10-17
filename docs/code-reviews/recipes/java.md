# Revisões de Código em Java

## Guia de Estilo Java

Os desenvolvedores devem seguir o [Guia de Estilo Java do Google](https://google.github.io/styleguide/javaguide.html).

## Análise de Código / Verificação de Estilo

Acreditamos firmemente que um estilo consistente aumenta a legibilidade e a manutenção de uma base de código. Portanto, recomendamos o uso de analisadores para impor regras de estilo e consistência.

Fazemos uso do [Checkstyle](https://github.com/checkstyle/checkstyle) usando a [mesma configuração usada no Azure Java SDK](https://github.com/Azure/azure-sdk-for-java/blob/master/eng/code-quality-reports/src/main/resources/checkstyle/checkstyle.xml).

[FindBugs](http://findbugs.sourceforge.net/) e [PMD](https://pmd.github.io/) também são comumente usados.

## Formatação Automática de Código

O Eclipse e outros IDEs Java oferecem suporte para formatação automática de código. Se estiver usando o Maven, alguns desenvolvedores também fazem uso do [formatter-maven-plugin](https://github.com/revelc/formatter-maven-plugin).

## Validação de Build

É importante impor o estilo de código e as regras na integração contínua (CI) para evitar que os membros da equipe façam merge de código que não esteja em conformidade com os padrões em seu repositório Git. Se estiver construindo usando o Azure DevOps, o Azure DevOps oferece suporte para tarefas de build do [Maven](https://learn.microsoft.com/azure/devops/pipelines/tasks/build/maven?view=azure-devops) e [Gradle](https://learn.microsoft.com/azure/devops/pipelines/tasks/build/gradle?view=azure-devops) usando ferramentas de análise de código [PMD](https://pmd.github.io/), [Checkstyle](https://checkstyle.sourceforge.io/) e [FindBugs](http://findbugs.sourceforge.net/) como parte de cada build.

Aqui está um exemplo de YAML para uma tarefa de build Maven com todas as três ferramentas de análise habilitadas:

```yaml
    - task: Maven@3
    displayName: 'Maven pom.xml'
    inputs:
        mavenPomFile: '$(Parameters.mavenPOMFile)'
        checkStyleRunAnalysis: true
        pmdRunAnalysis: true
        findBugsRunAnalysis: true
```

Aqui está um exemplo de YAML para uma tarefa de build Gradle com todas as três ferramentas de análise habilitadas:

```yaml
    - task: Gradle@2
    displayName: 'gradlew build'
    inputs:
        checkStyleRunAnalysis: true
        findBugsRunAnalysis: true
        pmdRunAnalysis: true
```

## Checklist de Revisão de Código

Além do [Checklist de Revisão de Código](../process-guidance/reviewer-guidance.md), você também deve procurar por itens específicos de revisão de código em Java:

* [ ] O projeto utiliza Lambdas para tornar o código mais limpo?
* [ ] A injeção de dependência (DI) é usada? Está configurada corretamente?
* [ ] Se o código usa o Spring Boot, você está usando @Inject em vez de @Autowire?
* [ ] O código lida com exceções corretamente?
* [ ] O [Azul Zulu OpenJDK](https://learn.microsoft.com/en-us/java/azure/jdk/java-jdk-install?view=azure-java-stable) está sendo usado?
* [ ] Uma ferramenta de automação de build e gerenciamento de pacotes (Gradle ou Maven) está sendo usada?
