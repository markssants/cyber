# 💻 Desafio de Simulação e Prevenção de Ataques de Força Bruta

## 1. Visão Geral do Projeto

Este projeto documenta a configuração de um ambiente de teste isolado e a análise de vulnerabilidades de **ataque de força bruta** em serviços comuns (FTP, SMB e aplicações Web) utilizando o Kali Linux e a ferramenta Medusa.

O objetivo principal é exercitar medidas de **segurança defensiva**, entendendo como os ataques são realizados para implementar estratégias de **prevenção e mitigação**.

**Tecnologias Utilizadas:**
* **Ataque/Teste:** Kali Linux, Medusa (Ferramenta de Brute Force)
* **Alvo:** Metasploitable 2 (Servidor vulnerável com FTP, SMB, DVWA)
* **Virtualização:** Oracle VirtualBox
* **Rede:** Configuração **Host-Only** (Rede Interna Isolada)

---

## 2. Configuração do Ambiente de Teste

O ambiente foi configurado com duas Máquinas Virtuais (VMs) isoladas, conectadas por uma rede Host-Only, garantindo que o tráfego simulado fique contido.

| VM | Sistema Operacional | Função | Endereço IP (Exemplo) |
| :--- | :--- | :--- | :--- |
| **Kali Linux** | Debian (Rolling) | Máquina de Teste (Attacker) | 192.168.56.101 |
| **Metasploitable 2** | Linux | Máquina Alvo (Target) | 192.168.56.102 |

****

**Configuração de Rede:**
As VMs utilizaram a configuração **Rede Interna (Host-Only)** no VirtualBox, garantindo que o tráfego simulado de ataque não interfira ou vaze para redes externas.

---

## 3. Cenários de Ataque Simulados e Análise

Os testes foram realizados utilizando a ferramenta **Medusa** para validar a exposição de serviços a ataques de dicionário e *password spraying*.

| Serviço Alvo | Ferramenta Utilizada | Análise de Vulnerabilidade | Mitigação Abordada |
| :--- | :--- | :--- | :--- |
| **A. FTP (Porta 21)** | Medusa (`-M ftp`) | **Vulnerabilidade:** Permite tentativas de login ilimitadas sem bloqueio de IP ou atraso. | Bloqueio de IP após `N` falhas, Desativação de FTP Anônimo. |
| **B. Formulário Web (DVWA)** | Medusa/Script Customizado | **Vulnerabilidade:** Falha na implementação de **Rate Limiting** e ausência de **CAPTCHA** no login. | Implementação de CAPTCHA e 2FA. |
| **C. SMB (Password Spraying)** | Medusa (`-M smb`) | **Vulnerabilidade:** Falha ao forçar a complexidade das senhas, permitindo que uma senha simples funcione para muitos usuários. | Monitoramento de múltiplos bloqueios, Política de Senha Forte. |

---

## 4. Documentação dos Testes e Artefatos

* **Comandos Utilizados (Estrutura Genérica):**
    A sintaxe básica para os testes de brute force seguiu o formato:
    ```bash
    # Exemplo: Medusa para teste em serviços de rede
    medusa -h [IP_ALVO] -u [ARQUIVO_USUARIOS] -P [ARQUIVO_SENHAS] -M [MODULO] -n [PORTA]
    ```
* **Wordlists:** (Arquivos localizados na pasta `\wordlists`)
    * `wordlists/usernames.txt`: Lista simples de usuários comuns do Metasploitable 2 (ex: `msfadmin`, `user`, `postgres`).
    * `wordlists/passwords_small.txt`: Lista de senhas extremamente fracas para validação inicial (ex: `password`, `123456`, `test`).
* **Capturas de Tela:** (Localizadas na pasta `/images`)
    * `images/01_network_config.png`: Captura da configuração de rede Host-Only.
    * `images/02_login_attempt_log.png`: Exemplo de log do servidor Metasploitable 2 mostrando tentativas de login fracassadas.

---

## 5. Recomendações de Mitigação e Defesa (Foco Principal) 🛡️

Com base nas vulnerabilidades expostas, as seguintes medidas de segurança são essenciais para prevenir ataques de força bruta:

| Área de Defesa | Estratégia de Mitigação | Detalhes e Implementação |
| :--- | :--- | :--- |
| **Acesso e Autenticação** | **Autenticação Multifator (MFA/2FA)** | Exigir um segundo fator (código TOTP, push notification) após o login, tornando o ataque de força bruta inútil, pois ele não pode adivinhar o token temporário. |
| **Limitação de Tentativas** | **Rate Limiting e Bloqueio Temporário** | Aplicar regras em firewalls ou no servidor de aplicação (ex: `iptables`, Cloudflare) que bloqueiem o endereço IP de origem por um tempo após um número pequeno de tentativas de login malsucedidas (ex: 5 falhas). |
| **Gerenciamento de Senhas** | **Política de Senhas Robustas** | Exigir comprimento mínimo de **12 caracteres** e utilizar **hashing seguro** (preferencialmente **Argon2** ou **bcrypt**) para armazenamento, tornando a quebra de senhas offline inviável. |
| **Defesa Web (DVWA)** | **CAPTCHA Inteligente** | Utilizar mecanismos de desafio (CAPTCHA) após a primeira falha de login, dificultando a automação por scripts. |
| **Monitoramento** | **Alertas e SIEM** | Configurar ferramentas de Monitoramento de Segurança (SIEM/IDS) para emitir alertas imediatos quando detectarem *password spraying* (ex: vários bloqueios de conta diferentes em um curto intervalo de tempo). |

---

## Conclusão

O desafio confirmou a eficácia da ferramenta Medusa em explorar serviços sem controles de segurança robustos. A parte mais valiosa do exercício foi a estruturação e documentação das defesas, reforçando que a **prevenção** por meio de **MFA**, **Rate Limiting** e **políticas de senha forte** é a única forma eficaz de neutralizar essa classe de ameaças.
