
ProxyChains is a UNIX program, that hooks network-related libc functions in dynamically linked programs via a preloaded DLL and redirects the connections through SOCKS4a/5 or HTTP proxies.

## install

```bash
sudo apt-fast install proxychains
```

## config

`sudo vi /etc/proxychains.conf`

```bash
# shift+g to bottom
...
...
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
#socks4 	127.0.0.1 9050
socks5 	127.0.0.1 10808
socks4 	127.0.0.1 10808
http 	127.0.0.1 10808
https 	127.0.0.1 10808
```

## alias

`vi .zshrc`
```bash
alias pc="proxychains"
```

## usage

```bash
# pc application
pc firefox
```

or start with icon, edit config, add proxychains command.

example:
```bash
[Desktop Entry]
Name=BingGPT
Comment=AI-powered copilot
GenericName=BingGPT
# add proxychains
Exec=proxychains binggpt %U
Icon=binggpt
Type=Application
StartupNotify=true
Categories=Utility;
```