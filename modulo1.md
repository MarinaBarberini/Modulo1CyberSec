# Relatório Técnico: Mapeamento de Rede Corporativa

**Projeto:** Formação em Cybersecurity - Módulo 1\
**Analista:** Marina Barberini\
**Data:** 18/07/2025

---

## 🖥️ 1. Rede Corporativa (corp_net - 10.10.10.0/24)

### Inventário:

| IP           | Hostname   | Serviços Detectados | Observações              |
| ------------ | ---------- | ------------------- | ------------------------ |
| 10.10.10.1   | (sem nome) | rpcbind (111/tcp)   | Provável gateway da rede |
| 10.10.10.2   | analyst    | Nenhum              | Máquina de análise       |
| 10.10.10.10  | WS_001     | Nenhum              | Estação de trabalho      |
| 10.10.10.101 | WS_002     | Nenhum              | Estação de trabalho      |
| 10.10.10.127 | WS_003     | Nenhum              | Estação de trabalho      |
| 10.10.10.222 | WS_004     | Nenhum              | Estação de trabalho      |

### Diagnóstico:

- Estações com portas fechadas: comportamento adequado.
- `rpcbind` ativo no IP 10.10.10.1: avaliar necessidade de exposição.

---

## 🛋️ 2. Rede de Infraestrutura (infra_net - 10.10.30.0/24)

### Inventário:

| IP           | Hostname      | Serviços Detectados       | Observações                           |
| ------------ | ------------- | ------------------------- | ------------------------------------- |
| 10.10.30.1   | (sem nome)    | rpcbind                   | Gateway da rede                       |
| 10.10.30.2   | analyst       | Nenhum                    | Máquina de análise                    |
| 10.10.30.10  | ftp-server    | FTP (porta 21)            | Sem criptografia                      |
| 10.10.30.11  | mysql-server  | MySQL 8.0.42 (porta 3306) | Acesso direto à base de dados         |
| 10.10.30.15  | samba-server  | Samba (139/tcp, 445/tcp)  | Compartilhamento de arquivos          |
| 10.10.30.17  | openldap      | LDAP (389), LDAPS (636)   | Autenticação com e sem criptografia   |
| 10.10.30.117 | zabbix-server | nginx (HTTP - porta 80)   | Interface web sem HTTPS               |
| 10.10.30.227 | legacy-server | Nenhum                    | Host ativo, mas sem serviços visíveis |

### Diagnóstico:

- Serviços essenciais bem identificados.
- FTP e HTTP expostos sem segurança: alto risco.
- `legacy-server` precisa de revisão.

---

## 👥 3. Rede de Visitantes (guest_net - 10.10.50.0/24)

### Inventário:

| IP         | Hostname        | Serviços Detectados | Observações                |
| ---------- | --------------- | ------------------- | -------------------------- |
| 10.10.50.1 | (sem nome)      | rpcbind             | Provável gateway           |
| 10.10.50.2 | laptop-luiz     | Nenhum              | Dispositivo pessoal seguro |
| 10.10.50.3 | laptop-vastro   | Nenhum              | Dispositivo pessoal seguro |
| 10.10.50.4 | macbook-aline   | Nenhum              | Dispositivo pessoal seguro |
| 10.10.50.5 | notebook-carlos | Nenhum              | Dispositivo pessoal seguro |
| 10.10.50.6 | analyst         | Nenhum              | Máquina de análise         |

### Diagnóstico:

- Dispositivos pessoais com portas fechadas: excelente.
- Gateway expõe `rpcbind`: revisar necessidade e controle de acesso.

---

## 🛠️ Plano de Ação 80/20

| Prioridade | Ação                                                          | Impacto | Facilidade |
| ---------- | ------------------------------------------------------------- | ------- | ---------- |
| Alta       | Substituir FTP por SFTP/FTPS no ftp-server                    | Alto    | Média      |
| Alta       | Ativar HTTPS na interface do zabbix-server                    | Alto    | Média      |
| Alta       | Restringir acesso ao mysql-server por firewall                | Alto    | Média      |
| Média      | Avaliar e limitar exposição do rpcbind nos gateways das redes | Médio   | Fácil      |
| Média      | Garantir que o OpenLDAP aceite apenas conexões criptografadas | Médio   | Média      |
| Média      | Revisar ou descomissionar o legacy-server                     | Médio   | Média      |

---

**Fim do Relatório**
