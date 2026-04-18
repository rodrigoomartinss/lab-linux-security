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
<img width="512" height="222" alt="image" src="https://github.com/user-attachments/assets/7fa34c0c-c70f-432a-8e6d-6a406fc2b742" />


# 🛡️ Laboratório: Hardening de SSH e Gestão de Chaves no Ubuntu 24.04

Este repositório documenta a implementação de uma camada avançada de segurança em um servidor Linux. O objetivo do projeto foi mitigar riscos de ataques de força bruta e dicionário, substituindo o acesso tradicional por senhas por autenticação via chaves criptográficas assimétricas.

## 🛠️ Tecnologias Utilizadas
* **OS Local:** Ubuntu 24.04 (Notebook Asus)
* **OS Servidor:** Ubuntu Server 24.04 LTS
* **Algoritmo de Criptografia:** Ed25519 (Curva Elíptica)
* **Serviço Alvo:** OpenSSH Server

## 🚀 Etapas do Projeto

### 1. Geração de Identidade Digital (Host Local)
Criação de um par de chaves assimétricas de alta performance e segurança no host do administrador. O algoritmo Ed25519 foi escolhido por oferecer maior segurança e chaves menores em comparação ao RSA tradicional.

### 2. Implementação de Confiança (Transferência)
Envio seguro da chave pública para o arquivo `authorized_keys` do servidor, permitindo que o host local seja reconhecido e autenticado automaticamente via token criptográfico.

### 3. Blindagem (Hardening) do Daemon SSH
Edição do arquivo de configuração principal do serviço SSH (`/etc/ssh/sshd_config`) para desativar globalmente o uso de senhas através do parâmetro `PasswordAuthentication no`.

### 4. Troubleshooting: O Arquivo Intruso
Durante os testes de invasão simulados, o servidor continuava exigindo senha. A investigação revelou que o Ubuntu 24.04 cria configurações em diretórios secundários que sobrescrevem a regra principal.
* **Solução:** O arquivo `/etc/ssh/sshd_config.d/50-cloud-init.conf` foi localizado via comando `grep` e removido, forçando o sistema a respeitar a política de bloqueio de senhas.

---

## 💻 Resumo de Comandos Utilizados

**No Host Local:**
```
# Geração do par de chaves Ed25519
ssh-keygen -t ed25519 -C "rodrigo-unifebe"

# Transferência da chave pública para o servidor
ssh-copy-id -i ~/.ssh/id_ed25519.pub rodrigo@192.168.0.240

# Teste de intrusão (Tentativa de forçar o uso de senha)
ssh -o PubkeyAuthentication=no rodrigo@192.168.0.240
```
🧪 Validação Final
Após o hardening completo, o servidor foi testado e agora recusa instantaneamente qualquer tentativa de login que não apresente a chave privada autorizada, retornando a seguinte saída de bloqueio:

rodrigo@192.168.0.240: Permission denied (publickey).


