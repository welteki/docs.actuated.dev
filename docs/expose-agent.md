## Expose the agent's API over HTTPS

The agent serves HTTP traffic on `127.0.0.1:8081` which is not accessible over the Internet. We expect most pilot customers to be using hosts with public IP addresses, so all you need to do is to install [Caddy](https://github.com/caddyserver/caddy) to get a HTTPS URL.

1. For a host on a public cloud

    If you're running the agent on a host with a public IP, simply install Caddy server and create a DNS A record. That way you can get a TLS certificate for the agent's endpoint.

    We are looking to automate this process and make it easier, but for now, a bit of bash should do what you need.

    Edit the first two lines "export" and save the file as `install-caddy.sh`:
    
    ```bash
    export AGENT_DOMAIN=agent1.example.com
    export LE_EMAIL=webmaster@agent1.example.com

    CADDY_VER=v2.4.3
    arkade get --progress=false caddy -v ${CADDY_VER}

    SUDO=sudo
    if [ "$(id -u)" -eq 0 ]; then
        SUDO=
    fi

    $SUDO install -m 755 $HOME/.arkade/bin/caddy /usr/bin/caddy

    $SUDO curl -fSLs https://raw.githubusercontent.com/caddyserver/dist/master/init/caddy.service --output /etc/systemd/system/caddy.service

    $SUDO mkdir -p /etc/caddy
    $SUDO mkdir -p /var/lib/caddy
    
    if $(id caddy >/dev/null 2>&1); then
      echo "User caddy already exists."
    else
      $SUDO useradd --system --home /var/lib/caddy --shell /bin/false caddy
    fi

    $SUDO tee /etc/caddy/Caddyfile >/dev/null <<EOF
    {
        email "${LETSENCRYPT_EMAIL}"
    }
    ${AGENT_DOMAIN} {
        reverse_proxy 127.0.0.1:8081
    }
    EOF

    $SUDO chown --recursive caddy:caddy /var/lib/caddy
    $SUDO chown --recursive caddy:caddy /etc/caddy

    $SUDO systemctl enable caddy
    $SUDO systemctl start caddy
    ```

    Your agent's endpoint URL is going to be: `https://$AGENT_DOMAIN`.

2. You're running the agent on-premises, behind NAT or at home

    You'll need a way to expose the client to the Internet, which includes HTTPS encryption and a sufficient amount of connections/traffic per minute.

    [Inlets](https://inlets.dev/) provides a quick and secure solution here.

    The [inletsctl](https://github.com/inlets/inletsctl) tool will create a HTTPS tunnel server with you on your favourite cloud with a HTTPS certificate obtained from Let's Encrypt.
    
    ```bash
    export AGENT_DOMAIN=agent1.example.com
    export LE_EMAIL=webmaster@agent1.example.com

    arkade get inletsctl
    sudo mv $HOME/.arkade/bin/inletsctl /usr/local/bin/

    inletsctl create \
        --provider digitalocean \
        --region lon1 \
        --token-file $HOME/do-token \
        --letsencrypt-email $LE_EMAIL \
        --letsencrypt-domain $AGENT_DOMAIN
    ```

    Note down the tunnel's wss:// URL and token.

    Then run a HTTPS client to expose your agent:

    ```bash
    inlets-pro http client \
        --url $WSS_URL \
        --token $TOKEN \
        --upstream http://127.0.0.1:8081
    ```

    [Inlets is available on a monthly subscription](https://openfaas.gumroad.com/l/inlets-subscription), the "Personal (Essentials)" license can be used for actuated pilot customers.

    Your agent's endpoint URL is going to be: `https://$AGENT_DOMAIN`.
