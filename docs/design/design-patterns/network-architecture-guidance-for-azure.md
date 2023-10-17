# Orientações de Arquitetura de Rede para o Azure

As seguintes são algumas das melhores práticas ao configurar e trabalhar com recursos de rede em ambientes de nuvem do Azure.

> **NOTA:** Ao trabalhar em um ambiente de nuvem existente, é importante entender quaisquer padrões atuais e como eles são usados antes de fazer alterações neles. Você também deve trabalhar com as partes interessadas relevantes para garantir que quaisquer novos padrões que você introduza proporcionem valor suficiente para justificar a mudança.

## Configuração de Rede e VNet

### Topologia de Concentrador e Filial

![imagem](images/spoke-spoke-routing.png)

A topologia de rede de concentrador e filial é um padrão de arquitetura comum usado no Azure para organizar e gerenciar recursos de rede. Baseia-se no conceito de um concentrador central que se conecta a várias redes de filiais. Esse modelo é particularmente útil para organizar recursos, manter a segurança e simplificar o gerenciamento de rede.

O modelo de concentrador e filial é implementado usando Redes Virtuais (VNet) e interconexão de VNets (VNet peering).

* O concentrador: A VNet central age como um concentrador, fornecendo serviços compartilhados como segurança de rede, monitoramento e conectividade com ambientes locais ou outras nuvens. Componentes comuns no concentrador incluem Dispositivos Virtuais de Rede (NVA), Firewall do Azure, Gateway VPN e Gateway ExpressRoute.

* As filiais: As VNets de filial representam unidades separadas ou aplicativos dentro de uma organização, cada um com seu próprio conjunto de recursos e serviços. Elas se conectam ao concentrador por meio de interconexão de VNets (VNet peering), o que permite a comunicação entre as VNets do concentrador e da filial.

A implementação de um modelo de concentrador e filial no Azure oferece várias vantagens:

* Isolamento e segmentação: Ao dividir recursos em VNets de filiais separadas, você pode isolar e segmentar cargas de trabalho, impedindo que possíveis problemas ou riscos de segurança afetem outras partes da rede.

* Gerenciamento centralizado: A VNet do concentrador atua como um ponto único de gerenciamento para serviços compartilhados, facilitando a manutenção, o monitoramento e a aplicação de políticas em toda a rede.

* Conectividade simplificada: A interconexão de VNets (VNet peering) permite a comunicação sem problemas entre as VNets do concentrador e da filial, sem a necessidade de roteamento complexo ou gateways adicionais, reduzindo a latência e a sobrecarga de gerenciamento.

* Escalabilidade: O modelo de concentrador e filial pode ser facilmente dimensionado para acomodar filiais adicionais à medida que a organização cresce ou à medida que novos aplicativos e serviços são introduzidos.

* Economia de custos: Centralizando serviços compartilhados no concentrador, as organizações podem reduzir os custos associados à implantação e ao gerenciamento de várias instâncias dos mesmos serviços em diferentes VNets.

Saiba mais sobre [topologia de concentrador e filial](https://learn.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?tabs=cli)

Ao implantar um modelo de concentrador/filial, é recomendável fazê-lo em conjunto com [zonas de pouso (landing zones)](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/). Isso garante consistência em todos os ambientes, bem como salvaguardas para garantir um alto nível de segurança, ao mesmo tempo em que oferece liberdade aos desenvolvedores nos ambientes de desenvolvimento.

### Firewall e Segurança

Ao usar uma topologia de concentrador e filial, é possível implantar um firewall centralizado no Hub que controle todo o tráfego de saída ou o tráfego de/para determinadas VNets. Isso permite proteção centralizada contra ameaças, minimizando os custos.

### DNS

As melhores práticas para o gerenciamento de DNS no Azure e em ambientes de nuvem em geral incluem o uso de serviços de DNS gerenciados. Alguns dos benefícios de usar serviços de DNS gerenciados são que os recursos são projetados para serem seguros, fáceis de implantar e configurar.

* **Encaminhamento de DNS:** Configure o encaminhamento

 de DNS entre seus servidores de DNS locais e os servidores de DNS do Azure para resolução de nomes em diferentes ambientes.

* **Use zonas DNS Privadas do Azure para recursos do Azure:** Configure zonas DNS Privadas do Azure para seus recursos do Azure para garantir que a resolução de nomes seja mantida dentro da rede virtual.

Saiba mais sobre [infraestrutura de DNS híbrido/multi-cloud](https://learn.microsoft.com/azure/architecture/hybrid/hybrid-dns-infra) e [infraestrutura de DNS do Azure](https://learn.microsoft.com/azure/dns/)

### Alocação de IP

Ao alocar espaços de endereço IP para Redes Virtuais do Azure (VNets), é essencial seguir as melhores práticas para um gerenciamento adequado e escalabilidade.

Aqui estão algumas recomendações para alocação de IP para VNets:

* **Reserve endereços IP:** Reserve endereços IP em seu espaço de endereço para recursos ou serviços críticos.

* **Alocação de IP pública:** Minimize o uso de endereços IP públicos e use o Azure Private Link sempre que possível para acessar serviços por meio de uma conexão privada.

* **Gerenciamento de endereços IP (IPAM):** Use soluções de IPAM para gerenciar e acompanhar a alocação de endereços IP em todo o ambiente híbrido.

* **Planeje seu espaço de endereço:** Escolha um espaço de endereço IP privado apropriado (a partir do RFC 1918) para suas VNets que seja grande o suficiente para acomodar o crescimento futuro. Evite sobreposição com redes locais ou de outras nuvens.

* **Use notação CIDR:** Use a notação CIDR (Classless Inter-Domain Routing) para definir o espaço de endereço da VNet, o que permite uma alocação mais eficiente e impede o desperdício de endereços IP.

* **Use sub-redes:** Divida suas VNets em sub-redes menores com base em requisitos de segurança, aplicação ou ambiente. Isso permite um melhor gerenciamento e segurança de rede.

* **Considere deixar um buffer entre as VNets:** Embora não seja estritamente necessário, deixar um buffer entre as VNets pode ser benéfico em alguns casos, especialmente quando você antecipa crescimento futuro ou a possibilidade de mesclar VNets. Isso pode ajudar a evitar conflitos de reendereçamento ao expandir ou mesclar redes.

* **Reserve endereços IP:** Reserve uma faixa de endereços IP dentro do espaço de endereço da sua VNet para recursos ou serviços críticos. Isso garante que eles tenham um endereço IP estático, essencial para serviços ou aplicativos específicos.

* **Planeje para cenários híbridos:** Se você estiver trabalhando em um ambiente híbrido com redes locais ou multi-cloud, certifique-se de planejar a alocação de endereços IP em todos os ambientes. Isso inclui evitar sobreposição de espaços de endereço e reservar endereços IP para recursos específicos, como gateways VPN ou circuitos ExpressRoute.

Saiba mais em [azure-best-practices/plan-for-ip-addressing](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-ip-addressing)

### Alocação de Recursos

Para alocação de recursos, as melhores práticas do [Guia de Design de Recursos de Nuvem](cloud-resource-design-guidance.md) devem ser seguidas.
