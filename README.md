# zabbix-docker

## Criar mysql
copie o conteudo abaixo, crie um arquivo no linux chamado mysql-zabbix.sh e cole o conteudo



```sh
docker run -d --name zabbix-mysql \
--restart always \
-p 3306:3306 \
-v /docker/zabbix/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=secret \
-e MYSQL_DATABASE=zabbix \
-e MYSQL_USER=zabbix \
-e MYSQL_PASSWORD=zabbix \
--network=zabbix \
mysql:8 \
--default-authentication-plugin=mysql_native_password \
--character-set-server=utf8 \
--collation-server=utf8_bin
```

em seguida execute o comando abaixo

```sh
chmod +x mysql-zabbix.sh
```
