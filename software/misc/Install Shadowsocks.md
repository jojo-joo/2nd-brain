## Install Shadowsocks-libev Server on Ubuntu 20.04
the same as ubuntu 22.04, see followings.
## Install Shadowsocks-libev Server on Ubuntu 22.04
SSH into your remote Ubuntu server. `Shadowsocks-libev` is included in Ubuntu repository, so you can install it with:
```shell
sudo apt update
sudo apt install shadowsocks-libev
```

The sodium crypto library (`libsodium`) will be installed along with shadowsocks-libev. It’s a requirement if you want to use the secure and fast ChaCha20-Poly1305 encryption method. Once it’s installed, edit the configuration file.

``` shell
sudo vim /etc/shadowsocks-libev/config.json
```

The default contents of the file are as follows.

```json
{
    "server":["::", "127.0.0.1"],
    "mode":"tcp_and_udp",
    "server_port":8388,
    "local_port":1080,
    "password":"uDIkNMe2qhms",
    "timeout":60,
    "method":"chacha20-ietf-poly1305"
}
```

We need to change `127.0.0.1` to `0.0.0.0`, so Shadowsocks-libev server will listen on the public IP address. Then change `server_port` to other port numbers like 8888. The password was randomly generated, so you can leave it as it is.

Save and close the file. Then restart shadowsocks-libev service for the changes to take effect.

sudo systemctl restart shadowsocks-libev.service

Enable auto-start at boot time.
```shell
sudo systemctl enable shadowsocks-libev.service
```

Check its status. Make sure it’s running.

```shell
sudo systemctl status shadowsocks-libev.service
```

If you see the following error.

This system doesn't provide enough entropy to quickly generate high-quality random numbers. The service will not start until enough entropy has been collected.

You can fix this error by installing `rng-tools`.

sudo apt-get install rng-tools

Then run

sudo rngd -r /dev/urandom

Now you can start Shadowsocks-libev service.

## Install shadowsocks on ubuntu 14.04

1. Install shadowsocks：

    ```
    apt-get install python-pip
    pip install shadowsocks
    ```

2. Edit config file

    ```
    vim /etc/shadowsocks.json
    ```

    change the config file content as following:

    ```
    {
        "server":["[::0]","0.0.0.0"],
        "server_port":8388,
        "local_address": "127.0.0.1",
        "local_port":1080,
        "password":"your_password",
        "timeout":300,
        "method":"aes-256-cfb",
        "fast_open": false
    }
    ```
    > **NOTE**
    >
    > Although AES encryption is used, there is no guarantee of security and anonymity.
    If your VPS does not have an IPv6 address, please change the configuration to "server":"your VPS's IPv4 address".
    The port can be kept as default or changed by yourself, but make sure it does not conflict with existing ones.

3. Restart shadowsocks service

    ```
    ssserver -c /etc/shadowsocks.json -d start
    ssserver -c /etc/shadowsocks.json -d stop
    ```
