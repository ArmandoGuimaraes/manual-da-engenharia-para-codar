# Avisos

Os seguintes avisos fornecem mais detalhes sobre como considerar o impacto de ações específicas recomendadas pelo [Checklist de Engenharia Sustentável](readme.md#sustainable-engineering-checklist).

## AÇÃO: Redimensionar máquinas físicas ou virtuais para melhorar a utilização

As recomendações de ferramentas de economia de custos geralmente estão alinhadas com a redução de carbono, mas, como a sustentabilidade não é o objetivo dessas ferramentas, economias de carbono não são garantidas. Como um provedor de nuvem ou data center gerencia a capacidade não utilizada também é um fator importante na determinação do quão impactante essa ação pode ser. Por exemplo:

O impacto sustentável de usar VMs menores na mesma família geralmente é benéfico ou neutro. Quando os núcleos não estão mais reservados, podem ser usados por outros em vez de trazer novos servidores online.

O impacto sustentável de mudar de famílias de VMs pode ser mais difícil de compreender porque o hardware subjacente e os núcleos reservados podem estar mudando junto com elas.

## AÇÃO: Migrar para um provedor de nuvem em escala hiper

As economias de carbono dos provedores de nuvem em escala hiper geralmente são atribuídas a quatro características-chave: eficiência operacional de TI, eficiência de equipamentos de TI, eficiência de infraestrutura de data center e eletricidade renovável. A Microsoft Cloud, por exemplo, é entre 22 e 93 por cento mais eficiente em termos de energia do que os data centers empresariais tradicionais, dependendo da comparação específica sendo feita. Quando se leva em consideração as compras de energia renovável, a Microsoft Cloud é entre 72 e 98 por cento mais eficiente em termos de carbono. [Fonte (PDF)](https://download.microsoft.com/download/7/3/9/739BC4AD-A855-436E-961D-9C95EB51DAF9/Microsoft_Cloud_Carbon_Study_2018.pdf)

## AÇÃO: Considerar executar um dispositivo de borda

Executar um dispositivo de borda anula muitos dos benefícios das instalações de computação em escala hiper, portanto, considerar a mistura de energia local e o cronograma típico das cargas de trabalho é importante para determinar se isso é benéfico no geral. Quanto maior o volume de dados que precisa ser transmitido, mais essa solução se torna atrativa. Por exemplo, o envio de grandes quantidades de conteúdo de áudio e vídeo para processamento.

## AÇÃO: Considerar o envio físico de dados para o provedor

O envio de itens físicos tem seu próprio impacto de carbono, dependendo do modo de transporte, o que precisa ser entendido antes de tomar essa decisão. Quanto maior o volume de dados que precisa ser transmitido, mais essa opção pode ser benéfica.

## AÇÃO: Considerar a eficiência energética das linguagens

Ao selecionar uma linguagem de programação, a linguagem de programação _mais_ eficiente em termos de energia nem sempre é a melhor escolha em termos de velocidade de desenvolvimento, manutenção, integração com sistemas dependentes e outros fatores do projeto. Mas ao decidir entre linguagens que atendem às necessidades do projeto, a eficiência energética pode ser uma consideração útil.

## AÇÃO: Usar políticas de cache

Um cache fornece armazenamento temporário de recursos solicitados por uma aplicação. O cache pode melhorar o desempenho da aplicação, reduzindo o tempo necessário para obter um recurso solicitado. O cache também pode melhorar a sustentabilidade, diminuindo a quantidade de tráfego de rede.

Embora o cache forneça esses benefícios, ele também aumenta o risco de que o recurso retornado para a aplicação esteja desatualizado, ou seja, não seja idêntico ao recurso que teria sido enviado pelo servidor se o cache não estivesse em uso. Isso pode criar experiências ruins para o usuário quando a precisão dos dados é fundamental.

Além disso, o cache pode permitir que usuários ou processos não autorizados leiam dados sensíveis. Uma resposta autenticada que está em cache pode ser recuperada do cache sem uma autorização adicional. Devido a preocupações de segurança como essa, o cache **não é recomendado** para cenários intermediários.

## AÇÃO: Considerar o cache de dados próximo aos usuários finais com uma CDN

Incluir CDNs na arquitetura de rede adiciona muitos servidores adicionais à sua infraestrutura de software, cada um com sua própria mistura de rede elétrica local. Os detalhes do hardware da CDN e o impacto da energia que o alimenta são importantes para determinar se as emissões de carbono ao executá-las são menores do que as emissões ao enviar os dados pela rede a partir de uma fonte mais distante. Quanto maior o volume de dados, a distância que precisa percorrer e a frequência das solicitações, mais essa solução se torna atraente.
