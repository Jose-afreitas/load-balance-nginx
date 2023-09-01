# verificar sobre(scale, cluster)

# Criando um load balance com nginx

## Criar um container do NGINX

## docker-compose exec nginx apk add bash nano

## docker-compose exec bash

# Arquivo principal do nginx que é o nginx.conf

# etc/nginx/nginx.conf

# nano /etc/nginx/conf.d/default.conf

# nesse aquivo você vai alterar

#

```
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}
```

# Após as alterações verifique o arquivo com: "nginx -t" feito isso verifique se os arquivos estão corretos, após faça o reload com: "nginx -s reload"

# Aqui você pode alterar as configurações, salve e veja se está correto o arquivo

# Use o comando nginx -t (para verificar se possui algum erro no arquivo)

```
upstream nodes_nginx {

server node-nginx1;
server node-nginx2;

}

server {
    listen       80;
    server_name  localhost;

    location / {
       proxy_pass http://nodes_nginx;
    }
}

```

# nginx -s reload

# para editar o htm você vai no seguinte arquivo

# cd /usr/share/nginx/html/ e edite o index.html

# Adicionando log

```
server {
    listen       80;
    server_name  localhost;

    location / {
       proxy_pass http://nodes_nginx;
    }
    access_log  /var/log/nginx/host.access.log  main;
}

```

# configure em cada container: => nano /etc/nginx/conf.d/default.conf

# Habilite a linha

# #access_log /var/log/nginx/host.access.log main;

# Reinicie com nginx -s reload

# para verificar o log passe tail -f /var/log/nginx/host.access.log

# pegando o Ip do usuário que está acessando o nginx, vamos corrigir indo no servidor principal e adicionando o seguinte.

```
location / {
       proxy_pass http://nodes_nginx;
       proxy_set_header X-Real-IP $remote_addr;
    }

```
