# Documentação Técnica: Planejamento VLSM

## 1. Visão Geral do Endereçamento
* **Bloco Total Disponível:** `192.168.0.0/22` (Abrangendo Matriz e Filiais)
* **Objetivo:** Divisão eficiente com base na demanda de hosts por VLAN, garantindo escalabilidade.

## 2. Detalhamento das Sub-redes (VLSM)

### Site 1: Matriz (192.168.1.0/24)
| Segmento | CIDR | Máscara | Host Mín/Máx | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| Engenharia | /26 | 255.255.255.192 | 192.168.1.1 - 1.62 | 192.168.1.1 |
| Wi-Fi Visitantes | /26 | 255.255.255.192 | 192.168.1.65 - 1.126 | 192.168.1.65 |
| Wi-Fi CORP | /27 | 255.255.255.224 | 192.168.1.129 - 1.158 | 192.168.1.129 |
| ADM | /27 | 255.255.255.224 | 192.168.1.161 - 1.190 | 192.168.1.161 |
| Financeiro | /28 | 255.255.255.240 | 192.168.1.193 - 1.206 | 192.168.1.193 |
| Contabilidade | /28 | 255.255.255.240 | 192.168.1.209 - 1.222 | 192.168.1.209 |
| Gerenciamento | /27 | 255.255.255.224 | 192.168.1.225 - 1.254 | 192.168.1.225 |

### Links WAN (Anel Roteado)
* **Rede:** `192.168.100.0/24`
* **Máscara:** `/30` (255.255.255.252) — *Otimizado para Ponto-a-Ponto*

| Link | Endereço de Rede | IPs Úteis |
| :--- | :--- | :--- |
| Matriz-F1 | 192.168.100.0 | .1 / .2 |
| F1-F2 | 192.168.100.4 | .5 / .6 |
| F2-Matriz | 192.168.100.8 | .9 / .10 |

## 3. Resumo da Estratégia de Máscaras
* **/26:** Sub-redes maiores (até 62 hosts) para departamentos densos.
* **/27:** Sub-redes médias (até 30 hosts) para setores administrativos e Wi-Fi CORP.
* **/28:** Sub-redes reduzidas (até 14 hosts) para departamentos pequenos.
* **/30:** Links WAN para máxima eficiência, ocupando apenas 2 IPs úteis por link.