# Prometheus

link para explicação sobre a arquitetura do sistema prometheus

https://developers.soundcloud.com/blog/prometheus-monitoring-at-soundcloud

possui um exporter, como node, para maysql, nginx, apache e etc.


Basta baixar um binário e executar que ira começar a coletar a métrica sobre o app. {chave - valor}.

De tempos em tempos coleta a métrica e armazena em timeseries database. 

# Instalando o prometheus


- ubuntu
  ```
  apt-get update && apt-get install -y curl wget
  ```
- centos
  ```
  yum install -y curl wget
  ```

crie os diretórios

```
mkdir /etc/prometheus
```

```
mkdir /var/lib/prometheus
```

crie um usuário prometheus para gerenciar o daemon do prometheus. obs: será criado sem o diretório home e sem acesso ssh, apenas para o daemon.

```
useradd --no-create-home --shell /bin/false prometheus
```
download do prometheus fix/release
```
curl -LO https://github.com/prometheus/prometheus/releases/donwload/v2.3.0/prometheus-2.3.0.linux-amd64.tar.gz
```
descompacte o arquivo 
```
tar -xvzf prometheus-2.3.0.linux-amd64.tar.gz
```
```
cd prometheus-2.0.0.linux-amd64

cp prometheus /usr/local/bin

cp promtool /usr/local/bin
```
mude as permissões dos binarios para o usuario prometheus criado

```
chown prometheus:prometheus /usr/local/bin/prometheus

chown prometheus:prometheus /usr/local/bin/promtool
```
copie os diretórios e libs para o diretório prometheus criado

```
cp -R consoles /etc/prometheus

cp -R consoles_libraries /etc/prometheus
```
para criar, colar e editar as configurações do prometheus

```
vim /etc/prometheus/prometheus.yml
```
crie as rules do proejeto

```
vim /etc/prometheus/alert.rules
```
altere as permissoes de todos os arquivos do prometheus para o usuário criado

```
chown prometheus:prometheus /etc/prometheus -R

chown prometheus:prometheus /var/lib/prometheus -R
```

crie o service e configure para executar o prometheus

```
vim /etc/systemd/system/prometheus.service
```

```
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ 
#    --web.console.template=etc/prometheus/console \
#    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
