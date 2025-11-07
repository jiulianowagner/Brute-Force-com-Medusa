# Brute Force com Medusa
Este projeto documenta uma série de simulações de ataques de força bruta contra ambientes propositalmente vulneráveis (Metasploitable e DVWA - Damn Vulnerable Web Application). O objetivo primário é analisar o comportamento dos ataques, compreender as vulnerabilidades e exercitar a implementação, validação e documentação de medidas de prevenção e mitigação como parte essencial das operações de um Analista de SOC (Security Operations Center).

# Ambiente
| Componente | Função | Componente Chave |
|-------------|-----------|:-----------:|
| Kali Linux | Plataforma de Ataque. | Máquina virtual configurada para rede interna. |
| Metasploitable 2 | Alvo vulnerável (FTP) | Vários serviços desatualizados e abertos. |
| Medusa | Ferramenta de testes de força bruta. | Utilizada para automação de tentativas em massa. |
| DVWA (PHP/MySQL) | Alvo vulnerável (Formulário Web). | Nível de Segurança configurado para 'Low' (Baixo) para testes iniciais. |

# Preparação do ambiente
1 - Instalar VirtualBox

2- Instalar kali Linux

3- Instalar Metasploitable 
Metasploitable é um sistema operacional de máquina virtual intencionalmente vulnerável, projetado para uso em treinamento de segurança cibernética. Ele serve como um ambiente de laboratório seguro e legal para aprender e praticar técnicas de exploração de vulnerabilidades, desenvolvimento de exploits e testes de penetração sem o risco de prejudicar sistemas reais. 

4- Configurar em ambas Host Only (Network)

5- No Metasploitable (criar um snapshot): Maquina a ser atacada
O objetivo do snapshot é voltar ao estado original caso seja necessário

# Força Bruta em Serviço FTP
**Vulnerabilidade:** Serviço FTP sem limites de tentativas de login.

**Simulação do Ataque:** Uso do Medusa para testar uma lista de senhas contra a porta FTP (21) do Metasploitable 2.

**Estratégia de Mitigação (Foco SOC):**

**Política de Bloqueio:** Configuração do servidor FTP para bloquear o IP de origem após 3 tentativas de login falhas em 60 segundos.

**Monitoramento:** Análise dos logs (/var/log/auth.log) para identificar a string indicadora do ataque de força bruta e a regra de alerta correspondente.

# Automação de Tentativas em Formulário Web (DVWA)
**Vulnerabilidade:** Formulário de login sem rate limiting ou proteção CAPTCHA.

**Simulação do Ataque:** Uso do Medusa para automatizar o envio de credenciais via POST Request do formulário de login do DVWA.

**Estratégia de Mitigação (Foco SOC):**

**Controle de Taxa (Rate Limiting):** Implementação de regras no Web Application Firewall (WAF) ou no reverse proxy para limitar o número de requisições por IP a X por minuto na rota de login.

**Controle de Bots:** Adição de um mecanismo CAPTCHA após a detecção de um comportamento anômalo.

# Password Spraying em SMB com Enumeração de Usuários
**Vulnerabilidade:** Serviço SMB (445) que permite enumeração de usuários e não impõe bloqueio após tentativas falhas.

**Simulação do Ataque:**

**Enumeração:** Uso de ferramentas (como enum4linux ou Medusa) para coletar a lista de usuários válidos.

**Spraying:** Uso do Medusa para tentar uma única senha comum (simulada) contra a lista de usuários coletada.

**Estratégia de Mitigação (Foco SOC):**

Desabilitar Enumeração: Configuração do smb.conf para restringir a enumeração de usuários (visando dificultar a coleta de alvos).

Lista de Senhas Proibidas: Documentação da importância de implementar políticas de "senhas proibidas" no gerenciamento de identidades.

