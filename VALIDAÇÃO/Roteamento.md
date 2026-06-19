# Planejamento de Roteamento: Site-to-Site e WAN

## 1. Topologia de Roteamento
O projeto utiliza um roteamento dinâmico para garantir redundância e alta disponibilidade em toda a infraestrutura dos três sites (Matriz, Filial 1 e Filial 2).

### Protocolo OSPF (Open Shortest Path First)
O OSPF foi escolhido por ser um protocolo de estado de link (*link-state*), proporcionando convergência rápida e suporte a redes de médio e grande porte.

* **Area 0 (Backbone):** Todos os roteadores de borda dos três sites fazem parte da Área 0, garantindo o backbone da rede.
* **Redundância:** A topologia em anel roteado permite que, em caso de falha de um link WAN, a tabela de rotas seja recalculada instantaneamente, redirecionando o tráfego pelo caminho disponível.



## 2. Configuração de Interfaces e WAN
O planejamento de roteamento seguiu o endereçamento VLSM definido anteriormente para os links ponto-a-ponto.

* **Interfaces de Borda:** As interfaces seriais/Gigabit de WAN foram configuradas com máscaras `/30` para eliminar o desperdício de endereços IP.
* **Propagação de Redes:** Utilizou-se o comando `network` no processo OSPF para anunciar todas as sub-redes locais (VLANs) e os links de backbone para todos os roteadores do anel.

## 3. Roteamento Inter-VLAN
Para que os departamentos dentro de um mesmo site se comuniquem, o roteamento ocorre no roteador central através de subinterfaces:

* **Encapsulamento:** Implementação do padrão IEEE 802.1Q.
* **Gateways:** Cada subinterface (`G0/0.VLAN_ID`) possui o endereço IP definido no plano de endereçamento, atuando como o *Default Gateway* das estações de trabalho de cada VLAN.



## 4. Tabela de Roteamento (Resumo)
Para verificar a saúde da rede, o comando `show ip route` deve apresentar:
1. **Redes 'C' (Connected):** As VLANs locais e links WAN diretamente conectados.
2. **Redes 'O' (OSPF):** As redes de outros sites aprendidas dinamicamente pelo protocolo.