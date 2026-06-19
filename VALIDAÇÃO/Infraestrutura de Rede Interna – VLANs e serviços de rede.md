# Planejamento de Infraestrutura de Rede Cabeada

## 1. Infraestrutura de Rede Interna (VLANs)
A implementação de VLANs é o método padrão para dividir um domínio de broadcast único em múltiplos domínios lógicos, aumentando a segurança e a performance em ambientes cabeados.

### Diretrizes de Configuração:
* **Isolamento de Tráfego:** Cada departamento (Engenharia, ADM, Financeiro) possui uma VLAN dedicada para impedir que o tráfego de broadcast de um setor sobrecarregue a infraestrutura cabeada dos demais.
* **Segurança na Camada 2:** Uso de `Access Ports` para dispositivos finais e `Trunk Ports` (IEEE 802.1Q) para interconexão entre switches, garantindo que as tags de VLAN sejam preservadas entre a ponta e o núcleo da rede.
* **VLAN de Gerenciamento:** A VLAN de gerenciamento foi isolada (VLAN 99) para garantir que apenas administradores tenham acesso à configuração dos ativos de rede.



## 2. Roteamento Inter-VLAN
Para permitir que os dispositivos em VLANs diferentes se comuniquem através do cabeamento estruturado, utilizamos roteamento centralizado.

* **Router-on-a-stick:** Utiliza subinterfaces no roteador central. Cada subinterface é encapsulada com `encapsulation dot1Q [VLAN_ID]` e recebe um endereço IP que atua como o Gateway padrão para a respectiva rede.
* **Escalabilidade:** Esta arquitetura centralizada garante que todo o tráfego inter-departamental passe pelo filtro do roteador central, permitindo a aplicação de políticas de segurança.

## 3. Serviços de Rede
Para que a rede cabeada seja escalável e de fácil manutenção, os serviços críticos foram centralizados.

### A. DHCP Centralizado
* **Centralização:** Em vez de configurar DHCP em múltiplos dispositivos, utilizamos um Servidor DHCP único centralizado na Matriz.
* **Helper Address:** É o comando crítico (`ip helper-address [IP_SERVIDOR]`) aplicado nas subinterfaces dos roteadores, permitindo que requisições DHCP (que são broadcast) sejam encaminhadas para o servidor, garantindo que todos os computadores recebam IP automaticamente, independentemente da VLAN.



## 4. Segurança e Controle
* **ACLs (Access Control Lists):** Aplicadas nas subinterfaces dos roteadores para filtrar o tráfego entre departamentos, garantindo que setores sensíveis, como o Financeiro, fiquem isolados de acessos não autorizados.
* **Port Security:** Configuração em switches de acesso para limitar o número de endereços MAC por porta, mitigando ataques de inundação MAC e prevenindo que dispositivos não autorizados sejam conectados à rede cabeada.

## 5. Checklist de Verificação
- [ ] O roteador central possui rotas para todas as VLANs configuradas?
- [ ] Todas as portas de interconexão entre switches estão configuradas em modo `trunk`?
- [ ] O `ip helper-address` está apontando para o servidor DHCP central em todas as subinterfaces?
- [ ] A máscara de sub-rede foi aplicada de forma consistente em toda a topologia?
