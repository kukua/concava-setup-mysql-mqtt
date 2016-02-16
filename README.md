# ConCaVa with MySQL and MQTT

> This setup uses MySQL for authentication & metadata storage, MongoDB for data storage and MQTT for publishing the data.

## Requirements

- [Docker](https://docs.docker.com/engine/installation/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## How to use

```bash
git clone https://github.com/kukua/concava-setup-mysql-mqtt
cd concava-setup-mysql-mqtt
cp .env.sample .env
# > Edit configuration in .env
docker-compose up -d

# Migrate database
docker-compose run mariadb bash -c 'TERM=dumb mysql --host=mariadb --user="$MYSQL_USER" --password="$MYSQL_PASSWORD" --database="$MYSQL_DATABASE" < /database.sql'
```

**Note:** The MQTT server is lazy loaded, so it becomes available after processing the first sensor data.

## Test

```bash
# 1337 (base 10) = 00000539 (base 16)
echo '00000539' | xxd -r -p | \
    curl -i -XPUT 'http://<container IP>:3000/v1/sensorData/0000000000000001' \
    -H 'Authorization: Token abcdef0123456789abcdef0123456789' \
    -H 'Content-Type: application/octet-stream' --data-binary @-

# TODO(mauvm): Add script for debugging published MQTT data
```