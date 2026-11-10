---
Version: 1.1.0
Title: Roteiro de Configuração de Ambiente e Ferramentas — Mini-Curso IA/OpenCode
Status: aprovado
Data: 10/11/2026
Vinculado a: mini-curso-ementa.md
---

# Roteiro de Configuração de Ambiente e Ferramentas

Manual intensivo, objetivo e direto, para nível técnico básico. Idioma: PT-BR. Comandos organizados para copy-paste. Janela de ação: **terminal Windows (PowerShell)**; após o WSL, os comandos passam a rodar dentro do Linux.

> Máquinas sem acesso de administrador: alguns comandos exigem admin e podem falhar — isso é esperado. Os principais passos (Scoop, nvm, npm, pacman dentro do WSL com sudo) estão liberados para o usuário comum.

---

## Sequência preferencial de ações

### Passo 1 — PowerShell: política de execução e Scoop

Abra o PowerShell como usuário comum (não-admin).

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex
```

Validação:
```powershell
scoop --version
```

### Passo 2 — Windows Terminal via Scoop (opcional, recomendado)

```powershell
scoop bucket add extras
scoop install windows-terminal
```

Atualizar depois:
```powershell
scoop update windows-terminal
```

### Passo 3 — WSL com Arch Linux

Requer PowerShell **como administrador** (clique com o botão direito → "Executar como administrador") e reinício do computador.

```powershell
wsl --install -d Arch
wsl --set-default-version 2
wsl --set-default Arch
```

Após reiniciar, abra o terminal do Arch (WSL) e configure usuário/senha quando solicitado.

Verificar versão:
```bash
wsl.exe --list --verbose
```

### Passo 3.1 — Usuário default não-root no WSL/Arch

No terminal do Arch (WSL), rode como o usuário que o WSL criou na instalação.

**1. Definir senha do root** (garante que o root já esteja configurado):
```bash
sudo passwd root
```

**2. Criar o usuário não-root com diretório /home e grupos necessários** (substitua `<usuario>` pelo nome desejado):
```bash
sudo useradd -m -G wheel,audio,video,optical,storage,input <usuario>
sudo passwd <usuario>
```

**3. Fazer o WSL iniciar sempre com este usuário (não-root)**:
```bash
sudo tee /etc/wsl.conf > /dev/null <<'EOF'
[user]
default=<usuario>
EOF
```

**4. Dar permissões sudo ao usuário** (roda sudo sem senha — adequado a ambiente de aula):
```bash
echo "<usuario> ALL=(ALL:ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/<usuario>
sudo chmod 0440 /etc/sudoers.d/<usuario>
```

**5. Aplicar a mudança**: no PowerShell do Windows, reinicie o WSL e reabre o terminal do Arch:
```powershell
wsl --shutdown
```
Depois abra novamente o terminal do Arch; ele deverá iniciar logado como `<usuario>`.

Validação:
```bash
whoami
ls -la /home/<usuario>
```

> **Nota:** no Arch, o grupo `wheel` equivale ao `sudo` de outras distros. O arquivo `/etc/sudoers.d/<usuario>` concede privilégios de administrador ao usuário comum.

### Passo 4 — Dentro do WSL/Arch: sistema, pacotes base e ferramentas essenciais

No terminal do Arch (WSL):

```bash
sudo pacman -Syu
sudo pacman -S --needed base-devel git python python-pip vim curl jq gh
```

Configurar sudoers (permitir que seu usuário rode sudo sem senha — simples, para ambiente de aula):
```bash
echo "$USER ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/$USER
sudo chmod 0440 /etc/sudoers.d/$USER
```

Validações:
```bash
python --version
pip --version
node --version 2>/dev/null || echo "node ainda não instalado (passo 6)"
gh --version
```

### Passo 5 — yay (helper do AUR)

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
cd ..
rm -rf yay
```

Validação:
```bash
yay --version
```

### Passo 6 — NVM e Node.js

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.41.0/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
nvm install --lts
nvm use --lts
```

Validação:
```bash
node --version
npm --version
```

### Passo 7 — Uvicorn (via pip)

```bash
pip install uvicorn
```

Validação:
```bash
uvicorn --version
```

### Passo 8 — Docker (opcional)

Docker Desktop com WSL 2 backend (mais simples, GUI) ou Docker Engine dentro do WSL. Teste:

```bash
docker run hello-world
```

Se precisar rodar sem sudo no WSL:
```bash
sudo usermod -aG docker $USER
```
Depois reinicie o terminal WSL (`wsl --shutdown` no PowerShell, então reabrir).

### Passo 9 — Tailscale (opcional)

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```
Acesse a URL exibida no navegador para autenticar.

### Passo 10 — OpenCode

```bash
npm install -g @opencode/cli
```

Validação:
```bash
opencode --version
```

---

## Validação final de entrada no curso

Confirme no terminal Windows/WSL antes do Bloco B:

```bash
wsl --list --verbose
git --version
python --version
pip --version
node --version
npm --version
opencode --version
```

Checklist:
- [ ] Terminal WSL/Arch aberto e funcional
- [ ] Git, Python, pip, Node.js, npm respondem aos comandos de versão
- [ ] OpenCode instalado (`opencode --version`)
- [ ] Acesso à internet funcional
- [ ] Conta GitHub com e-mail cadastrado

---

*Roteiro de ambiente vinculado à ementa do mini-curso. Versão: 1.1.0 | 10/11/2026*