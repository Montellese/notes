# WireGuard <!-- omit in toc -->

- [docker-compose](#docker-compose)
  - [Update docker-compose](#update-docker-compose)

## Command Line Interface

### List connected clients
```bash
sudo wg show
```

## Split Tunnel

Source: https://blog.crankshafttech.com/2021/03/how-to-setup-pihole-pivpn-unbound.html

Location: `/etc/wireguard/configs/foo.conf`
```
...

[Peer]
...
AllowedIPs = 10.6.0.1/32, 192.168.1.0/24
```
