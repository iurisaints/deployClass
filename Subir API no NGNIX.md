
**Pré-requisitos:**

* **API Node.js funcional:** Certifique-se de que sua API Node.js esteja funcionando e escutando em uma porta específica (por exemplo, 3000 ou 8080). Teste-a independentemente para verificar se funciona corretamente.
* **Servidor Nginx:** Tenha o Nginx instalado e configurado em seu sistema.

**Passos:**

1. **Configure sua aplicação Node.js:**
   * **Escolha da porta:** Escolha uma porta para sua API Node.js que não seja normalmente usada por outros serviços (por exemplo, 3000, 8080, etc.). Evite a porta 80, pois ela geralmente é usada pelo próprio Nginx.

2. **Configure o Nginx como um Proxy Reverso:**
   * **Acesse o arquivo de configuração:** Use `sudo nano /etc/nginx/nginx.conf` (ou o caminho apropriado para o seu sistema) para editar o arquivo de configuração do Nginx.

   * **Crie um bloco servidor:** Adicione um novo bloco servidor dedicado à sua API Node.js. Aqui está um exemplo:

     ```nginx
     server {
         listen 80;  # Ajuste se estiver usando uma porta diferente para o Nginx
         server_name seudominio.com;  # Substitua pelo seu nome de domínio real

         location /api {  # Este caminho será usado para acessar sua API
             proxy_pass http://localhost:3000;  # Substitua pela porta em que seu aplicativo Node.js está
             proxy_set_header Host $host;
             proxy_set_header X-Real-IP $remote_addr;
         }
     }
     ```

   - **Explicação:**
     - `listen 80`: Ajuste isso se o Nginx estiver escutando em uma porta diferente.
     - `server_name seudominio.com`: Substitua pelo seu nome de domínio real que aponta para este servidor.
     - `location /api`: Define o caminho dentro do seu domínio para acessar sua API (por exemplo, `http://seudominio.com/api`).
     - `proxy_pass http://localhost:3000`: Encaminha solicitações para o caminho `/api` para sua aplicação Node.js em `localhost` na porta `3000`. Ajuste esses valores conforme necessário.
     - `proxy_set_header Host $host;`: Garante que o hostname correto seja enviado para sua aplicação Node.js.
     - `proxy_set_header X-Real-IP $remote_addr;`: Define o cabeçalho `X-Real-IP` para o IP real do cliente, útil para fins de segurança.

3. **Salve e reinicie o Nginx:**
   - Salve o arquivo de configuração do Nginx (`Ctrl+O` e `Enter`).
   - Teste a configuração para erros de sintaxe: `sudo nginx -t`. Se não houver erros, reinicie o Nginx: `sudo systemctl restart nginx` (ou o comando apropriado para o seu sistema).

4. **Acesse sua API Node.js:**
   Agora você deve conseguir acessar sua API Node.js através do caminho que definiu no bloco `location` (por exemplo, `http://seudominio.com/api`).

**Considerações adicionais:**

* **Gerenciamento de processo:** Considere usar um gerenciador de processo como o PM2 para garantir que sua aplicação Node.js reinicie automaticamente em caso de falhas.
* **Segurança:** Para ambientes de produção, implemente medidas de segurança adequadas, como HTTPS e mecanismos de autenticação/autorização.
* **Configurações avançadas:** Explore recursos do Nginx como balanceamento de carga se você planeja escalar sua aplicação Node.js em vários servidores.
* **Customização:** Você pode personalizar ainda mais a configuração do Nginx para cache, registro e outros requisitos específicos.

Seguindo esses passos, você configurará com sucesso sua API Node.js para rodar perfeitamente atrás de um proxy reverso Nginx, melhorando o desempenho geral, a segurança e a escalabilidade.
