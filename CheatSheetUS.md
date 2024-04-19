# Cheat Sheet for Ubuntu Server
Neste arquivo você encontrará alguns comandos para utilizar no seu Ubuntu Server.

### 1. Atualizar:
sudo apt-get update
-- caso queira atualizar o *sistema* (cuidado com isso, trocará a versão):
sudo apt-get upgrade

### 2. Atualização automática:
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades

### 3. Firewall
sudo apt install ufw
sudo ufw enable

### 4. Instalação do Docker
   - Execute os seguintes comandos para instalar o Docker:
     ```
     sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
     sudo apt-get update
     sudo apt-get install docker-ce
     ```
   - Adicione seu usuário ao grupo Docker para executar comandos Docker sem sudo: `sudo usermod -aG docker your_username`
   - Faça logout e login novamente para aplicar as alterações.
     
### 5. Instalação do Docker Compose:
   - Execute os seguintes comandos para instalar o Docker Compose:
     ```
     sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     sudo chmod +x /usr/local/bin/docker-compose
     ```
### 6. Instalação do Umbrel:
   - Clone o repositório Umbrel no GitHub: `git clone https://github.com/getumbrel/umbrel.git`
   - Navegue até o diretório umbrel: `cd umbrel`
   - Execute o script de instalação: `sudo ./scripts/install`

### 7. Gerenciamento do Ubuntu Server:
   - Para reiniciar o servidor: `sudo reboot`
   - Para desligar o servidor: `sudo shutdown -h now`
   - Para verificar o status dos serviços: `sudo systemctl status nome_do_serviço`

### 8. Habilitar SSH com UFW
sudo ufw allow ssh

### 9. Criando uma conta Não-Sudo
sudo adduser user1
(exigirá que preencha os dados para o cadastro da conta)
