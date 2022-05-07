# Moonlight 🌕

The light takes you home 

## Server

use Ubuntu 16.04/20.04

For details: [基于 Nginx 的简单 TLS 分流](https://guide.v2fly.org/advanced/tls_routing_with_nginx.html#%E7%9B%AE%E7%9A%84)

### Just follow steps

#### Install V2ray & Configuration
1. Download & install v2ray
    ```bash
    bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
    ```
2. Copy `./etc/v2ray/config-server.json` to `/usr/local/etc/v2ray/config.json`
3. Remember to add UUID to `config.json` under Vmess protocol
    > You can get UUID from https://www.uuidgenerator.net/
    >
    > Or `cat /proc/sys/kernel/random/uuid`
4. Start v2ray
    ```bash
    sudo systemctl start v2ray
    ```

#### Install openresty & Configuration

1. [Install openresty](http://openresty.org/en/linux-packages.html#ubuntu)
    ```bash
    wget -O - https://openresty.org/package/pubkey.gpg | sudo apt-key add -

    echo "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main" \
        | sudo tee /etc/apt/sources.list.d/openresty.list

    sudo apt update
    sudo apt -y install openresty
    ```
2. Rsync conf folder
    ```bash
    # a is a folder, you edit conf/nginx.conf in a, and this won't impact files in original folder
    rsync -av /usr/local/openresty/nginx/{conf,html,logs} openresty-v2ray
    ```
3. Copy or Paste `./nginx/nginx.conf` to `openresty-v2ray/conf/nginx.conf`
4. Install ssl certificate, follow these steps: [acme.sh 说明](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
5. Assume your domain name is `domain.me`, Copy `~/.acme.sh/domain.me/domain.me.cert` to `/usr/local/etc/v2ray/v2ray.crt`
6. Copy `~/.acme.sh/domain.me/domain.me.key` to `/usr/local/etc/v2ray/v2ray.crt`
7. Reload configuration
    ```bash
    cd openresty-v2ray
    sudo openresty -p .
    sudo openresty -p . -s reload
    ```

## Client

### ClashX

#### Mine

+ [my ClashX Config](./etc/ClashX-Pro/clashx-config.yaml)
+ [my Home Rule](./etc/ClashX-Pro/my-ClashX-Ruleset.yaml)

#### Others

+ [ClashX pro](https://github.com/yichengchen/clashX#install)
+ [ClashX config template](https://github.com/Semporia/ClashX-Pro)
+ [ClashX rule](https://github.com/Loyalsoldier/v2ray-rules-dat)
