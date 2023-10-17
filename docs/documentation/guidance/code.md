# Código

Você provavelmente já ouviu mais de uma vez que **você deve escrever código autoexplicativo**. Isso não significa que você nunca deve comentar seu código.

Existem dois tipos de comentários de código: comentários de implementação e comentários de documentação.

## Comentários de Implementação

Eles são usados para documentação interna e destinam-se a qualquer pessoa que possa precisar manter o código no futuro, incluindo você mesmo.

Existem comentários de linha única e comentários de várias linhas (por exemplo, [Comentários em C#](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/language-specification/lexical-structure#comments)). Comentários são legíveis por humanos e não são executados, portanto, ignorados pelo compilador. Portanto, você pode potencialmente adicionar quantos desejar.

Agora, o uso desses comentários geralmente é considerado um indício de código problemático. Se você precisa esclarecer seu código, isso pode significar que o código está muito complexo. Portanto, você deve trabalhar para remover a necessidade de esclarecimento tornando o código mais simples, mais fácil de ler e entender. Ainda assim, esses comentários podem ser úteis para fornecer uma visão geral do código ou fornecer informações adicionais de contexto que não estão disponíveis no código em si.

Exemplos de comentários úteis:

- Comentário de linha única em C# que explica **por que** aquele trecho de código está ali (de um método privado em [System.Text.Json.JsonSerializer](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/JsonSerializer.Read.String.cs)):

```csharp
// Por questões de desempenho, evite obter a contagem real de bytes, a menos que o uso de memória seja maior que o limite.
Span<byte> utf8 = json.Length <= (ArrayPoolMaxSizeBeforeUsingNormalAlloc / JsonConstants.MaxExpansionFactorWhileTranscoding) ? ...
```

- Comentário de várias linhas em C# que fornece **contexto adicional** (de um método privado em [System.Text.Json.Utf8JsonReader](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Text.Json/src/System/Text/Json/Reader/Utf8JsonReader.cs)):

```csharp
// A transcodificação de UTF-16 para UTF-8 alterará o comprimento entre 1x e 3x.
// O desescape do valor do token reduzirá seu comprimento no máximo em 6x.
// Não faz sentido incorrer no custo de transcodificação/desescape/comparação se:
// - O valor do token for menor que charTextLength
// - O valor do token precisa ser transcodificado E desescapado e for mais de 6x maior que charTextLength
//      - Para caracteres UTF-16 ASCII, a transcodificação = 1x, o desescape = 6x => fator 6x
//      - Para caracteres UTF-16 não ASCII dentro do BMP, a transcodificação = 2-3x, mas são representados como um único valor hexadecimal escapado, \uXXXX => fator 6x
//      - Para caracteres UTF-16 não ASCII fora do BMP, a transcodificação = 4x, mas o par suplementar (2 caracteres) é representado por 16 bytes \uXXXX\uXXXX => fator 6x
// - O valor do token precisa ser transcodificado, mas NÃO desescapado e for mais de 3x maior que charTextLength
//      - Para caracteres UTF-16 ASCII, a transcodificação = 1x,
//      - Para caracteres UTF-16 não ASCII dentro do BMP, a transcodificação = 2-3x,
//      - Para caracteres UTF-16 não ASCII fora do BMP, a transcodificação = 2x, (pares suplementares - 2 caracteres transcodem para 4 bytes UTF-8)
if (sourceLength < charTextLength
    || sourceLength / (_stringHasEscaping ? JsonConstants.MaxExpansionFactorWhileEscaping : JsonConstants.MaxExpansionFactorWhileTranscoding) > charTextLength)
{
```

## Comentários de Documentação

Comentários de documentação (doc comments) são um tipo especial de comentário, adicionados acima da definição de qualquer tipo ou membro definido pelo usuário, e destinam-se a qualquer pessoa que possa precisar usar esses tipos ou membros em seu próprio código.

Se, por exemplo, você está construindo uma biblioteca ou framework, os comentários de documentação podem ser usados para gerar a documentação deles. Essa documentação deve servir como especificação de API e/ou guia de programação.

Os comentários de documentação não serão incluídos pelo compilador no executável final, assim como os comentários de linha única e de várias linhas.

Exemplo de um comentário de documentação em C# (do método Deserialize em [System.Text.Json.JsonSerializer](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/JsonSerializer.Read.String.cs)):

```csharp
/// <summary>
/// Analisa o texto que representa um único valor JSON em um <typeparamref name="TValue"/>.
/// </summary>
/// <returns>Uma representação de <typeparamref name="TValue"/> do valor JSON.</returns>
/// <param name="json">Texto JSON para análise.</param>
/// <param name="options">Opções para controlar o comportamento durante a análise.</param>
/// <exception cref="System.ArgumentNullException">
/// <paramref name="json"/> é <see langword="null"/>.
/// </exception>
/// <exception cref="JsonException">
/// O JSON é inválido.
///
/// -ou-
///
/// <typeparamref name="TValue" /> não é compatível com o JSON.
///
/// -ou-
///
/// Há dados remanescentes na string além de um único valor JSON.</exception>
/// <exception cref="NotSupportedException">
/// Não há um <see cref="System.Text.Json.Serialization.JsonConverter"/>
/// compatível com <typeparamref name="TValue"/> ou seus membros serializáveis.
/// </exception>
/// <remarks>Usar uma <see cref="string"/> não é tão eficiente quanto usar os
/// métodos UTF-8, pois a implementação usa nativamente UTF-8.
/// </remarks>
[RequiresUnreferencedCode(SerializationUnreferencedCodeMessage)]
public static TValue? Deserialize<TValue>(string json, JsonSerializerOptions? options = null)
{
```

Em **C#**, os comentários de documentação podem ser processados pelo compilador para gerar arquivos de documentação XML. Esses arquivos podem ser distribuídos junto com suas bibliotecas para que o Visual Studio e outras IDEs possam usar o IntelliSense para mostrar informações rá

pidas sobre tipos ou membros. Além disso, esses arquivos podem ser processados por ferramentas como [DocFx](https://dotnet.github.io/docfx/) para gerar sites de referência de API.

Mais informações:

- [Tags XML recomendadas para comentários de documentação em C#](https://learn.microsoft.com/dotnet/csharp/language-reference/xmldoc/recommended-tags).

Em outras linguagens, você pode precisar de ferramentas externas. Por exemplo, os comentários de documentação **Java** podem ser processados pela ferramenta Javadoc para gerar arquivos de documentação HTML.

Mais informações:

- [Como Escrever Comentários de Documentação para a Ferramenta Javadoc](https://www.oracle.com/technical-resources/articles/java/javadoc-tool.html)
- [Ferramenta Javadoc](https://www.oracle.com/java/technologies/javase/javadoc-tool.html#javadocdocuments)
