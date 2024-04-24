Para subir sua aplicação Node.js no Tomcat no Ubuntu, siga estes passos:

**Pré-requisitos:**

* **Ubuntu Server:** Certifique-se de ter o Ubuntu Server instalado e configurado.
* **Node.js:** Instale o Node.js usando o seguinte comando:
    ```bash
    sudo apt install nodejs npm
    ```
* **Tomcat:** Instale o Tomcat usando o seguinte comando:
    ```bash
    sudo apt install tomcat9
    ```
* **Aplicação Node.js:** Tenha sua aplicação Node.js pronta para ser implantada.

**Passos:**

1. **Crie um diretório para sua aplicação:**
    ```bash
    sudo mkdir /opt/tomcat/webapps/minha-aplicacao-nodejs
    ```
    Substitua "minha-aplicacao-nodejs" pelo nome da sua aplicação.

2. **Copie os arquivos da sua aplicação:**
    ```bash
    cp -r /caminho/para/sua/aplicacao/nodejs/* /opt/tomcat/webapps/minha-aplicacao-nodejs
    ```
    Substitua "/caminho/para/sua/aplicacao/nodejs" pelo caminho do diretório da sua aplicação.

3. **Crie um arquivo `package.json`:**
    ```json
    {
      "name": "minha-aplicacao-nodejs",
      "main": "index.js",
      "scripts": {
        "start": "node index.js"
      }
    }
    ```
    Substitua "minha-aplicacao-nodejs" pelo nome da sua aplicação e "index.js" pelo nome do seu arquivo principal.

4. **Instale as dependências da sua aplicação:**
    ```bash
    cd /opt/tomcat/webapps/minha-aplicacao-nodejs
    npm install
    ```

5. **Crie um arquivo `server.xml`:**
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Server port="8080" shutdown="SHUTDOWN">
      <Connector port="8080" protocol="HTTP" connectionTimeout="60000" redirectPort="8443"/>
      <Service name="NodeJSApp" className="org.apache.catalina.core.StandardWrapper" path="/minha-aplicacao-nodejs">
        <Loader className="org.apache.catalina.loader.WebAppLoader" />
        <WebResourceRoot path="/opt/tomcat/webapps/minha-aplicacao-nodejs"  type="application" />
        <Valve className="org.apache.catalina.core.StandardContextValve" />
        <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
      </Service>
    </Server>
    ```
    Substitua "8080" pelas portas que você deseja usar para HTTP e HTTPS, e "minha-aplicacao-nodejs" pelo nome da sua aplicação.

6. **Salve o arquivo `server.xml` em `/opt/tomcat/conf/Catalina/localhost`.**

7. **Reinicie o Tomcat:**
    ```bash
    sudo systemctl restart tomcat9
    ```

Sua aplicação Node.js agora deve estar acessível em `http://localhost:8080/minha-aplicacao-nodejs`.

**Observações:**

* Você pode modificar o arquivo `server.xml` para configurar outras opções do Tomcat, como o contexto da sua aplicação e as portas que ela usa.
* Se sua aplicação Node.js usar um servidor web diferente do Express, você precisará configurar o Tomcat para usá-lo.
* Certifique-se de que sua aplicação Node.js esteja configurada para ser executada em um ambiente de produção.

**Recursos adicionais:**

* Documentação do Tomcat: [https://tomcat.apache.org/tomcat-8.5-doc/index.html](https://tomcat.apache.org/tomcat-8.5-doc/index.html)
* Documentação do Node.js: [https://nodejs.org/en/docs](https://nodejs.org/en/docs)
* Tutorial: Deploying a Node.js Application to Tomcat: [https://www.digitalocean.com/solutions/nodejs-hosting](https://www.digitalocean.com/solutions/nodejs-hosting)

**Lembre-se:**

* Este é um guia geral e pode ser necessário adaptá-lo à sua aplicação específica.
* Se você encontrar problemas, consulte a documentação do Tomcat e do Node.js ou procure ajuda em fóruns online.
