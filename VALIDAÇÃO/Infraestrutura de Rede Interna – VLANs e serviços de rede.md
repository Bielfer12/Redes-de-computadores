# Infraestrutura de Rede Interna: VLANs e Serviços de Rede

## 1. Segmentação com VLANs (Virtual LANs)
A implementação de VLANs é o método padrão para dividir um domínio de broadcast único em múltiplos domínios lógicos, aumentando a segurança e a performance.

### Diretrizes de Configuração:
* **Isolamento de Tráfego:** Cada departamento (Engenharia, ADM, Financeiro) deve possuir uma VLAN dedicada para impedir que tráfego de broadcast de um setor sobrecarregue outro.
* **Segurança na Camada 2:** Uso de `Access Ports` para dispositivos finais e `Trunk Ports` (IEEE 802.1Q) para interconexão entre switches, garantindo que as tags de VLAN sejam preservadas.
* **VLAN Nativa:** Deve ser alterada da VLAN 1 (padrão) para uma VLAN de gerenciamento isolada, como a VLAN 99.



## 2. Roteamento Inter-VLAN
Para permitir que os dispositivos em VLANs diferentes se comuniquem, é necessário um dispositivo de Camada 3 (Roteador ou Switch L3).

* **Router-on-a-stick:** Utiliza subinterfaces no roteador central. Cada subinterface é encapsulada com `encapsulation dot1Q [VLAN_ID]` e recebe um endereço IP que atua como o Gateway daquela rede.
* **Switch Layer 3:** Quando a demanda de tráfego é alta, o roteamento deve ocorrer diretamente no switch core através de `Switch Virtual Interfaces (SVIs)`, reduzindo a latência.

## 3. Serviços Essenciais de Rede
A infraestrutura não sobrevive apenas com conectividade; ela depende de serviços que automatizam a gestão dos dispositivos.

### A. DHCP Centralizado
* **Centralização:** Em vez de configurar DHCP em cada roteador/antena, utilizamos um Servidor DHCP único.
* **Helper Address:** É o comando crítico (`ip helper-address [IP_SERVIDOR]`) aplicado nas subinterfaces dos roteadores, permitindo que requisições DHCP (que são broadcast) sejam encaminhadas para o servidor fora daquela sub-rede.

### B. Gestão de Acesso Sem Fio (Wireless)
* **Modo Bridge:** Em redes corporativas com switches PoE, APs (Access Points) devem operar em modo Bridge (transparente), mantendo o tráfego marcado com a VLAN correta.
* **Segurança:** Implementação de WPA2/WPA3-Enterprise (ou PSK com criptografia AES) para garantir que apenas dispositivos autorizados integrem a VLAN corporativa.



## 4. Segurança e Controle
* **ACLs (Access Control Lists):** Aplicadas nas subinterfaces dos roteadores para filtrar o tráfego. 
    * *Exemplo:* Bloquear o tráfego da VLAN 100 (Visitantes) para a sub-rede dos servidores, permitindo apenas o acesso à Internet.
* **Port Security:** Configuração em switches de acesso para limitar o número de endereços MAC por porta, mitigando ataques de inundação MAC.

## 5. Checklist de Verificação
- [ ] O roteador central possui rotas para todas as VLANs configuradas?
- [ ] As portas de interconexão entre switches estão em modo `trunk`?
- [ ] O `ip helper-address` está apontando para o servidor DHCP correto?
- [ ] A máscara de sub-rede é consistente em toda a topologia?