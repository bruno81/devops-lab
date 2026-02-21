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