# DIO_Activity
Desafio - DIO
Simulação de Ataques de Força Bruta com Kali Linux, Nmap e Medusa

Descrição do Projeto

Este projeto demonstra a construção de um ambiente controlado para simulação de ataques de força bruta contra sistemas propositalmente vulneráveis, utilizando Kali Linux como máquina atacante e Metasploitable 2 + DVWA como alvos.

Além dos ataques, foram realizadas etapas de reconhecimento e enumeração com Nmap, simulando um fluxo real de auditoria de segurança.

Todos os testes foram executados em ambiente isolado, exclusivamente para fins educacionais.

Objetivos

- Compreender o ciclo básico de um ataque de força bruta

- Realizar reconhecimento e enumeração de serviços

- Identificar credenciais fracas

- Explorar vulnerabilidades comuns

- Propor medidas de mitigação

- Documentar o processo técnico


Ferramentas Utilizadas:

- Kali Linux

- Metasploitable 2

- DVWA (Damn Vulnerable Web Application)

- Nmap

- Medusa

- VirtualBox


Configuração do Ambiente

Máquinas Virtuais:

Máquina	            Função	      Sistema

Kali Linux	      Atacante	    Linux

Metasploitable 2	Alvo	        Linux vulnerável


Rede:

Configuração: Rede Interna

Essa configuração garante que as máquinas se comuniquem apenas entre si.

Endereços IP

Kali Linux: 192.168.0.2

Metasploitable 2: 192.168.0.3


Fase 1 — Reconhecimento com Nmap

Antes dos ataques, foi realizado um scan para identificar serviços ativos.

Scan básico de portas
nmap -sS 192.168.0.2
Scan detalhado com detecção de serviços e versões
nmap -sV -A 192.168.0.2

Principais serviços identificados:

Porta	      Serviço	      Observação
21	        FTP	          vsftpd vulnerável
22	        SSH	          Acesso remoto
80	        HTTP	        DVWA disponível
139/445	    SMB	          Compartilhamentos ativos

Fase 2 — Enumeração de Serviços

Enumeração aprofunda a análise dos serviços descobertos.

Enumeração SMB
enum4linux 192.168.0.2

Informações obtidas:

- Lista de usuários

- Compartilhamentos de rede

- Informações do sistema

Verificação manual de acesso SMB
smbclient -L //192.168.0.2 -N

Fase 3 — Ataque de Força Bruta em FTP

Serviço alvo:

FTP ativo na porta 21.

Comando Medusa:
medusa -h 192.168.0.2 -u msfadmin -P wordlist.txt -M ftp

Resultado:

- Credencial válida encontrada:

Usuário: msfadmin

Senha: msfadmin

- Acesso FTP confirmado.


Fase 4 — Ataque a Formulário Web (DVWA)

Serviço alvo:

Aplicação DVWA hospedada na porta 80.

Acesso via navegador:

http://192.168.0.2/dvwa

Estratégia:

Tentativas automatizadas de login utilizando credenciais comuns.

Resultado:

Acesso administrativo obtido devido a senha fraca.


Fase 5 — Password Spraying em SMB

Password spraying testa uma única senha em múltiplos usuários.

Comando Medusa
medusa -h 192.168.0.2 -U users.txt -p senha123 -M smbnt

Resultado:

Usuários com senha fraca identificados.

Wordlists Utilizadas

- wordlist.txt:
123456
password
admin
msfadmin
senha123

- users.txt:
root
admin
user
msfadmin
guest

Recomendações de Mitigação:

- Utilizar senhas fortes e únicas

- Implementar bloqueio após tentativas falhas

- Habilitar autenticação multifator (MFA)

- Monitorar logs de autenticação

- Restringir acesso por firewall

- Desativar serviços desnecessários

- Aplicar atualizações de segurança regularmente

Conclusão:

Os testes demonstraram que sistemas com credenciais fracas e ausência de controles de segurança são altamente vulneráveis a ataques automatizados.

A inclusão de etapas de reconhecimento e enumeração evidencia como um invasor pode mapear o ambiente antes da exploração.
