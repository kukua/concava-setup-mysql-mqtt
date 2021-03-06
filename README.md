# ConCaVa with MySQL and MQTT

> This setup uses MySQL for authentication & metadata storage, MongoDB for temporary data storage and MQTT for publishing the data.

## How to use

```bash
git clone https://github.com/kukua/concava-setup-mysql-mqtt
cd concava-setup-mysql-mqtt
cp .env.example .env
chmod 600 .env
# > Edit configuration in .env
# > Change path to log (by default: './concava.log') in docker-compose.yml
# > Create log file:
touch ./concava.log
docker-compose up -d
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

## License

This software is licensed under the [MIT license](https://github.com/kukua/concava-setup-mysql-mqtt/blob/master/LICENSE).

© 2016 Kukua BV
