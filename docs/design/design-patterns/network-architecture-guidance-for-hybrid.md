# Orientações de Arquitetura de Rede para Ambientes Híbridos

As seguintes são as melhores práticas sobre como projetar e configurar recursos usados em ambientes híbridos e multi-cloud.

> **NOTA:** Ao trabalhar em um ambiente híbrido existente, é importante entender quaisquer padrões atuais e como eles são usados antes de fazer qualquer alteração.

## Topologia de Concentrador e Filial

A topologia de concentrador e filial não muda muito ao usar nuvem/híbrido se configurada corretamente. A principal diferença é que a VNet do concentrador está interconectada com a rede local por meio de um ExpressRoute e que todo o tráfego do Azure pode sair via ExpressRoute e a conexão com a Internet local.

As melhores práticas gerais estão em [Orientações de Arquitetura de Rede para Azure#Topologia de Concentrador e Filial](network-architecture-guidance-for-azure.md#topologia-de-concentrador-e-filial)

### Alocação de IP

Ao trabalhar com implantações híbridas, tome cuidado extra ao planejar a alocação de IPs, pois existe um risco muito maior de sobreposição de intervalos de rede.

As melhores práticas gerais estão disponíveis em [Orientações de Arquitetura de Rede para Azure#Alocação de IP](network-architecture-guidance-for-azure.md#ip-allocation)

Saiba mais sobre isso em [Melhores Práticas do Azure para Planejamento de Endereçamento IP](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-ip-addressing)

### ExpressRoute

Ambientes que usam o ExpressRoute frequentemente direcionam todo o tráfego do Azure por meio de uma conexão privada (ExpressRoute) para um local local. Isso impõe alguns problemas ao trabalhar com ofertas de PAAS, pois nem todas elas se conectam por meio de seu respectivo ponto de extremidade privado e, em vez disso, usam um IP externo para conexões de saída, ou algum tráfego de PAAS para PAAS ocorre internamente no Azure e não funcionará com redes públicas desativadas.

Dois serviços notáveis aqui são cópias dos planos de dados de contas de armazenamento e muitos dos serviços não suportam pontos de extremidade privados.

Escolha o circuito ExpressRoute certo: selecione um SKU (Padrão ou Premium) e largura de banda apropriados com base nos requisitos da sua organização.
Redundância: garanta redundância provisionando dois circuitos ExpressRoute em locais de interconexão diferentes.
Monitoramento: use o Azure Monitor e o Network Performance Monitor (NPM) para monitorar a saúde e o desempenho de seus circuitos ExpressRoute.

### DNS

As melhores práticas gerais estão disponíveis em [Orientações de Arquitetura de Rede para Azure#DNS](network-architecture-guidance-for-azure.md#dns)

Ao usar o Azure DNS em um ambiente híbrido ou multi-cloud, é importante garantir uma configuração de DNS e encaminhamento consistente que garanta que os registros sejam atualizados automaticamente e que todos os servidores DNS estejam cientes uns dos outros e saibam qual servidor é o autorizado para os diferentes registros.

Saiba mais sobre [Infraestrutura de DNS Híbrida/Multi-Cloud](https://learn.microsoft.com/azure/architecture/hybrid/hybrid-dns-infra)

### Alocação de Recursos

Para alocação de recursos, as melhores práticas do [Guia de Design de Recursos de Nuvem](cloud-resource-design-guidance.md) devem ser seguidas.
