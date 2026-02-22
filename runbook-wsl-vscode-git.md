# Runbook – WSL + VSCode + Git Setup

Guia prático para configurar um ambiente de desenvolvimento no Windows usando **WSL2 + Ubuntu + VSCode + Git (SSH)**.

---

## Índice

- [1. Instalação do WSL](#1-instalação-do-wsl)
- [2. Gerenciamento de distros](#2-gerenciamento-de-distros)
- [3. Hostname permanente no WSL](#3-hostname-permanente-no-wsl)
- [4. Backup e restore de distro](#4-backup-e-restore-de-distro)
- [5. Integração VSCode + WSL](#5-integração-vscode--wsl)
- [6. Git: configuração e fluxo básico](#6-git-configuração-e-fluxo-básico)
- [7. SSH para GitHub](#7-ssh-para-github)

---

## 1. Instalação do WSL

### Verificar status

Abra o **PowerShell**:

```powershell
wsl --status
```

Se não reconhecer o comando, habilite os recursos do Windows abaixo.

---

### Habilitar recursos necessários (via CLI)

No PowerShell (Administrador):

```powershell
dism /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Reinicie o Windows após habilitar.

---

### Definir WSL2 como padrão

```powershell
wsl --set-default-version 2
```

---

### Listar distros disponíveis

```powershell
wsl --list --online
```

---

### Instalar Ubuntu 22.04

```powershell
wsl --install -d Ubuntu-22.04
```

Durante a instalação será solicitado criar usuário e senha no Linux.

## 2. Gerenciamento de distros

### Listar distros instaladas

```powershell
wsl -l -v
```

---

### Entrar na distro padrão

```powershell
wsl
```

### Entrar em distro específica

```powershell
wsl -d Ubuntu-22.04
```

---

### Sair da distro

```bash
exit
```

---

### Parar todas as distros

```powershell
wsl --shutdown
```

### Parar distro específica

```powershell
wsl --terminate Ubuntu-22.04
```

---

### Definir distro padrão

```powershell
wsl --set-default Ubuntu-22.04
```

---

### Remover distro

```powershell
wsl --unregister NOME_DA_DISTRO
```

## 3. Hostname permanente no WSL

### Alterar hostname

Dentro do WSL:

```bash
sudo hostnamectl set-hostname devops-lab
hostname
```

---

### Tornar permanente após reinício

Edite o arquivo `/etc/wsl.conf`:

```bash
sudo vim /etc/wsl.conf
```

Adicione o seguinte conteúdo:

```ini
[network]
hostname=devops-lab
generateHosts=false
```

---

### Aplicar alterações

No PowerShell:

```powershell
wsl --shutdown
```

Depois entre novamente na distro:

```powershell
wsl -d Ubuntu-22.04
```

## 4. Backup e restore de distro

### Exportar (backup)

```powershell
wsl --export Ubuntu-22.04 ubuntu-backup.tar
```

---

### Importar (restore)

```powershell
wsl --import Ubuntu-22.04-restaurado C:\WSL\ ubuntu-backup.tar
```

---

## 5. Integração VSCode + WSL

### Método 1 (recomendado)

1. Instale o VSCode no Windows  
2. Instale a extensão **Remote - WSL**  
3. No VSCode pressione:

```
Ctrl + Shift + P
```

Digite:

```
WSL: Connect to WSL
```

No canto inferior esquerdo deve aparecer algo como:

```
WSL: Ubuntu-22.04
```

---

### Método 2 (garantir comando `code` no PATH)

Dentro do WSL:

```bash
echo 'export PATH="$PATH:/mnt/c/Users/SEU_USUARIO/AppData/Local/Programs/Microsoft VS Code/bin"' >> ~/.bashrc
source ~/.bashrc
which code
```

---

### Abrir pasta no VSCode

```bash
code .
```

## 6. Git: configuração e fluxo básico

### Instalar Git

```bash
sudo apt update
sudo apt install git -y
git --version
```

---

### Configurar identidade (obrigatório)

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@email.com"
git config --list
```

---

### Criar projeto versionado (exemplo)

```bash
mkdir -p ~/projects/devops-labs/git-basico
cd ~/projects/devops-labs/git-basico
echo "# Meu primeiro projeto Git" > README.md
```

---

### Entender o fluxo básico

Git trabalha com três áreas:

- **Working Directory** (seus arquivos)
- **Staging Area**
- **Commit (histórico)**

Comandos básicos:

```bash
git init
git add README.md
git commit -m "Primeiro commit"
git status
git log --oneline
```

---

### Renomear branch principal para main

```bash
git branch -m main
git branch
```

---

## 7. SSH para GitHub

### Verificar SSH

```bash
ssh -V
```

Se necessário, instalar:

```bash
sudo apt install openssh-client -y
```

---

### Gerar chave SSH (ed25519)

```bash
ssh-keygen -t ed25519 -C "seu-email-do-github@email.com"
ls ~/.ssh
```

Arquivos gerados:

- `id_ed25519` → chave privada  
- `id_ed25519.pub` → chave pública  

---

### Copiar chave pública

```bash
cat ~/.ssh/id_ed25519.pub
```

Adicionar no GitHub em:

**Settings → SSH and GPG keys → New SSH key**

---

### Testar conexão

```bash
ssh -T git@github.com
```

---

### Conectar repositório remoto e enviar commits

Dentro do projeto:

```bash
git remote add origin git@github.com:SEU_USUARIO/devops-lab.git
git remote -v
git push -u origin main
```

O parâmetro `-u` cria o vínculo entre a branch local e a remota, facilitando futuros `git push` e `git pull`.
