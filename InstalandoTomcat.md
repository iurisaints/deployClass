# Tutorial de instalação do Tomcat:

## 1) Instalando
Vamos iniciar com uma segurança básica do sistema:
``
sudo useradd -m -d /opt/tomcat -U -s /bin/false tomcat
``

Após isso vamos atualizar o sistema:
``
sudo apt update
``

E vamos verificar a versão do Java:
``
java -version
``

Vamos para a pasta /tmp:
``
cd /tmp
``

Agora baixe o tomcat:
``
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.20/bin/apache-tomcat-10.1.23.tar.gz 
``

E faça a extração pelo terminal:
``
sudo tar xzvf apache-tomcat-10*tar.gz -C /opt/tomcat --strip-components=1
``

Depois disso, você tem que garantir que o tomcat será instalado com esse código:
``
sudo chown -R tomcat:tomcat /opt/tomcat/
sudo chmod -R u+x /opt/tomcat/bin
``

## Configurando Usuários

Abra o arquivo de edição de usuários:
``
sudo nano /opt/tomcat/conf/tomcat-users.xml
``

Agora adicione essa parte antes do fim da tag de users e modifique a senha:
``
<role rolename="manager-gui" />
<user username="manager" password="manager_password" roles="manager-gui" />

<role rolename="admin-gui" />
<user username="admin" password="admin_password" roles="manager-gui,admin-gui" />
``


Precisamos remover a restrição da página de Manager:
``
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
``

E comentem a parte do Valve, tipo assim:

...
<Context antiResourceLocking="false" privileged="true" >
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
(comente essa parte com o comentário do html)  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> (finaliza o comentário aqui)
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.Csr>
</Context>


Salva e repete para o host manager através daqui:
``
sudo nano /opt/tomcat/webapps/host-manager/META-INF/context.xml
``

## 3) Criando um serviço systemd

O tomcat é basicamente java, só subir esse código:
``
sudo update-java-alternatives -l
``

Depois vamos criar um arquivo para o serviço do tomcat:
``
sudo nano /etc/systemd/system/tomcat.service
``
E adicione no arquivo:

[Unit]
Description=Tomcat
After=network.target

[Service]
Type=forking

User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target

-------------------------

Dê um reload no systemd:

``
sudo systemctl daemon-reload
``
E iniciar o serviço:

``
sudo systemctl start tomcat
``

E para verificar o status:

``
sudo systemctl status tomcat
``

Para sempre iniciar quando ligar o computador:
``
sudo systemctl enable tomcat
``

## 4) Acessando a interface web:

Colocar a porta que precisa:

``
sudo ufw allow 8080
``

E no seu navegador pode acessar utilizando:

``
http://your_server_ip:8080
``
