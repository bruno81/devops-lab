# 🚀 Runbook – WSL + VSCode + Git Setup

Guia completo para configurar ambiente de desenvolvimento no Windows utilizando:

- WSL2
- Ubuntu
- Visual Studio Code
- Git
- SSH para GitHub

---

# 📑 Índice

1. [Instalação do WSL](#-instalação-do-wsl)
2. [Gerenciamento de Distros](#-gerenciamento-de-distros)
3. [Configuração de Hostname](#-configuração-de-hostname)
4. [Backup e Restore de Distro](#-backup-e-restore-de-distro)
5. [Integração VSCode + WSL](#-integração-vscode--wsl)
6. [Configuração do Git](#-configuração-do-git)
7. [Configuração SSH para GitHub](#-configuração-ssh-para-github)

---

# 🧩 Instalação do WSL

## Verificar status

```powershell
wsl --status
```
Se não reconhecer o comando, habilite os recursos do Windows abaixo.

### Habilitar recursos necessários (recomendado via CLI)

No PowerShell (Administrador):

```powershell
dism /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Reinicie o Windows após habilitar.

### Definir WSL2 como padrão

```powershell
wsl --set-default-version 2
```

### Listar distros disponíveis

```powershell
wsl --list --online
```

### Instalar Ubuntu 22.04

```powershell
wsl --install -d Ubuntu-22.04
```