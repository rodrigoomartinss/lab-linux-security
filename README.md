# Lab: Linux Server Provisioning & Network Hardening

Este repositório documenta a criação e configuração inicial de um servidor Linux focado em boas práticas de infraestrutura e segurança.

## 🛠️ Tecnologias Utilizadas
* **Sistema Operacional:** Ubuntu Server 24.04 LTS
* **Virtualização:** Oracle VirtualBox (Rede em Modo Bridge)
* **Acesso Remoto:** OpenSSH Server configurado via Termius
* **Segurança:** UFW (Uncomplicated Firewall)

## 🔒 Camada 1 de Hardening: Firewall (UFW)
A política de segurança adotada foi o **Default Deny** (Negar tudo por padrão), abrindo apenas as portas estritamente necessárias para a operação e administração do servidor.

Comandos aplicados para a blindagem:
`sudo ufw default deny incoming`
`sudo ufw default allow outgoing`
`sudo ufw allow 22/tcp` 
`sudo ufw allow 80/tcp`
`sudo ufw enable`

### Evidência de Configuração
*(Adicione aqui o seu print mostrando o "Status: active" e as portas liberadas)*
