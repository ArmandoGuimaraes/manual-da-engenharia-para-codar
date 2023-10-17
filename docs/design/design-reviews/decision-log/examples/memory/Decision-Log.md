# Registro de Decisões

Este documento é usado para rastrear as principais decisões tomadas durante o curso do projeto.
Isso pode ser usado em um estágio posterior para entender por que decisões foram tomadas e por quem.

| **Decisão**                      | **Data**     | **Alternativas Consideradas**                                    | **Justificativa**                                                                                                                                          | **Documento Detalhado**                              | **Tomada Por** | **Trabalho Requerido** |
|-----------------------------------|--------------|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|-------------|-----------------------|
| Usar Registros de Decisões de Arquitetura | 25/01/2021 | Documentos de Design Padrão                                    | Uma solução fácil e de baixo custo para rastrear as decisões arquitetônicas ao longo da vida de um projeto.                                          | Registro de Decisões de Arquitetura                 | Equipe de Desenvolvimento    | #21654                |
| Usar ArgoCD                       | 26/01/2021 | FluxCD                                                           | ArgoCD é mais rico em recursos, suportará mais cenários e será uma melhor ferramenta para colocar em nossos cintos de ferramentas. Por isso, decidimos neste ponto usar o ArgoCD. | [Estudo de Trade GitOps](Trade-Studies/GitOps.md) | Equipe de Desenvolvimento    | #21672                |
| Usar Helm                         | 28/01/2021 | Kustomize, Kubes, Gitkube, Draft                                | Maturidade da plataforma, modelagem, suporte ao ArgoCD                                                                                                     | Estudo de Gerenciamento de Pacotes K8s              | Equipe de Desenvolvimento    | #21674                |
| Usar o CosmosDB                   | 29/01/2021 | Blob Storage, CosmosDB, SQL Server, Neo4j, JanusGraph, ArangoDB | O CosmosDB tem uma melhor integração com o Azure, identidade gerenciada e a API Gremlin é poderosa.                                                      | Estudo e Decisão de Armazenamento de Gráfico         | Equipe de Desenvolvimento    | #21650                |
| Usar o Azure Traffic Manager | 02/02/2021 | Azure Front Door | Uma solução leve para rotear o tráfego entre múltiplos clusters regionais do k8s | Estudo de Roteamento | Equipe de Desenvolvimento | #21673 |
| Usar Linkerd + Contour | 02/02/2021 | Istio, Consul, Ambassador, Traefik | Uma pilha nativa da nuvem CNCF para fornecer service mesh, gateway de API e ingress | Estudo de Roteamento | Equipe de Desenvolvimento | #21673 |
| Usar Modelos ARM | 02/02/2021 | Terraform, Pulumi, Az CLI | Azure Native, Suporte ao Monitoramento Az e atualizações incrementais | Estudo de Implantação Automatizada | Equipe de Desenvolvimento | #21651 |
| Usar 99designs/gqlgen | 04/02/2021 | graphql, graphql-go, thunder | Segurança de tipo, registro automático e geração de código | Estudo de GraphQL Golang | Equipe de Desenvolvimento | #21775 |
| Criar um modelo de dados de papel normalizado | 25/03/2021 | Perfis de Estágio de Carreira (CSP), Microsoft Role Library | Requer um modelo de dados que suporte os requisitos de dados de ambos os sistemas de função | Esquema de Modelo de Dados de Função | Equipe de Desenvolvimento | #22035 |
| Projetar para arestas e vértices | 25/03/2021 | N/A | N/A | Modelo de Dados | Equipe de Desenvolvimento | #21976 |
| Usar o grammes | 29/03/2021 | Gremlin, gremgo, gremcos | Equilíbrio entre documentação e maturidade | Estudo de Biblioteca de API do Gremlin | Equipe de Desenvolvimento | #21870 |
| Projetar para implementação do Gremlin | 02/04/2021 | N/A | N/A | Gremlin | Equipe de Desenvolvimento | #21980 |
| Projetar para implementação do Gremlin | 02/04/2021 | N/A | N/A | Gremlin | Equipe de Desenvolvimento | #21980 |
| Expor um modelo de dados 1:1 da API para o BD | 02/04/2021 | Expondo uma versão simplificada do contrato do modelo de dados | A equipe decidiu que não há peças de dados que possamos descartar como úteis. Será atualizado se o modelo de dados se tornar muito complexo | README da API | Equipe de Desenvolvimento | #21658 |
| Descontinuar o SonarCloud | 05/04/

2021 | Checkstyle, PMD, FindBugs | Requer um plano pago para uso em um repositório privado | Qualidade e Segurança de Código | Equipe de Desenvolvimento | #22090 |
| Adotou Estratégia de Marcação Estável | 08/04/2021 | N/A | A equipe alinhou-se com a estratégia de marcação de contêineres Docker proposta | Estratégia de Marcação | Equipe de Desenvolvimento | #22005 |
