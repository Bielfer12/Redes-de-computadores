# Documentação Técnica: Planejamento VLSM

## 1. Visão Geral do Endereçamento
A topologia lógica foi desenvolvida utilizando segmentação avançada, separando o tráfego corporativo, tráfego industrial e redes de acesso sem fio.

**Padrão de Sub-redes (Opção de /24 por Site):**
- **Site 1 (Matriz):** `192.168.1.0/24`
- **Site 2 (Filial 1 - Produção):** `192.168.2.0/24`
- **Site 3 (Filial 2 - Comercial):** `192.168.3.0/24`
- **Anel WAN (Roteadores):** `192.168.100.0/24`

## 2. Detalhamento das Sub-redes (VLSM)

### 1. SITE 1: Matriz (Office Corporativo)
Bloco principal: `192.168.1.0/24`

| Departamento / VLAN | ID | Rede (Subnet) | Máscara | Range IPs Úteis | IP do Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Engenharia** | 2 | `192.168.1.0` | `/26` (255.255.255.192) | `192.168.1.1` até `1.62` | `192.168.1.1` |
| **Wi-Fi Visitantes** | 100 | `192.168.1.64` | `/26` (255.255.255.192) | `192.168.1.65` até `1.126` | `192.168.1.65` |
| **Wi-Fi CORP** | 110 | `192.168.1.128` | `/27` (255.255.255.224) | `192.168.1.129` até `1.158` | `192.168.1.129` |
| **ADM** | 12 | `192.168.1.160` | `/27` (255.255.255.224) | `192.168.1.161` até `1.190` | `192.168.1.161` |
| **Financeiro** | 5 | `192.168.1.192` | `/28` (255.255.255.240) | `192.168.1.193` até `1.206` | `192.168.1.193` |
| **Contabilidade** | 6 | `192.168.1.208` | `/28` (255.255.255.240) | `192.168.1.209` até `1.222` | `192.168.1.209` |
| **Gerenciamento** | 99 | `192.168.1.224` | `/27` (255.255.255.224) | `192.168.1.225` até `1.254` | `192.168.1.225` |

### 2. SITE 2: Filial 1 (Produção / Fábrica)
Bloco principal: `192.168.2.0/24` 
*Nota: Segmentação industrial isolada em Máquinas de Conversão e Máquinas de Impressão para mitigar ruídos de rede (broadcast). Setor Administrativo local focado em PCP e Logística.*

| Departamento / VLAN | ID | Rede (Subnet) | Máscara | Range IPs Úteis | IP do Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Conversão** | 21 | `192.168.2.0` | `/26` (255.255.255.192) | `192.168.2.1` até `2.62` | `192.168.2.1` |
| **Impressão** | 22 | `192.168.2.64` | `/26` (255.255.255.192) | `192.168.2.65` até `2.126` | `192.168.2.65` |
| **Wi-Fi Visitantes** | 100 | `192.168.2.128` | `/27` (255.255.255.224) | `192.168.2.129` até `2.158` | `192.168.2.129` |
| **Wi-Fi CORP** | 110 | `192.168.2.160` | `/27` (255.255.255.224) | `192.168.2.161` até `2.190` | `192.168.2.161` |
| **ADM (PCP/Logística)** | 12 | `192.168.2.192` | `/27` (255.255.255.224) | `192.168.2.193` até `2.222` | `192.168.2.193` |
| **Gerenciamento** | 99 | `192.168.2.224` | `/27` (255.255.255.224) | `192.168.2.225` até `2.254` | `192.168.2.225` |

---

### 3. SITE 3: Filial 2 (Escritório Comercial)
Bloco principal: `192.168.3.0/24`

| Departamento / VLAN | ID | Rede (Subnet) | Máscara | Range IPs Úteis | IP do Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Comercial** | 30 | `192.168.3.0` | `/26` (255.255.255.192) | `192.168.3.1` até `3.62` | `192.168.3.1` |
| **Wi-Fi Visitantes** | 100 | `192.168.3.64` | `/26` (255.255.255.192) | `192.168.3.65` até `3.126` | `192.168.3.65` |
| **Wi-Fi CORP** | 110 | `192.168.3.128` | `/27` (255.255.255.224) | `192.168.3.129` até `3.158` | `192.168.3.129` |
| **ADM** | 12 | `192.168.3.160` | `/27` (255.255.255.224) | `192.168.3.161` até `3.190` | `192.168.3.161` |
| **Gerenciamento** | 99 | `192.168.3.192` | `/26` (255.255.255.192) | `192.168.3.193` até `3.254` | `192.168.3.193` |


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
