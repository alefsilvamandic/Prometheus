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
mkdir /var/lib/prometheus
```

crie um usuário prometheus para gerenciar o daemon do prometheus. obs: será criado sem o diretório home e sem acesso ssh, apenas para o daemon.

```
useradd --no-create-home --shell /bin/false prometheus
```
download do prometheus fix/release

curl -LO https://github.com/prometheus/prometheus/releases/donwload/v2.0.0/prometheus-2.0.0.linux-amd64.tar.gz

tar -xvzf prometheus-2.0.0.linux-amd64.tar.gz

