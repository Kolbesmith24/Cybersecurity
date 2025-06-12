# What is it?
Routes all traffic coming from any command-line tool to any proxy we specify. `Proxychains` adds a proxy to any command-line tool and is hence the simplest and easiest method to route web traffic of command-line tools through our web proxies.
## Setup
To use `proxychains`, we first have to edit `/etc/proxychains4.conf`, comment out the final line and add the following line at the end of it:
``` bash
# socks4        127.0.0.1 9050
http 127.0.0.1 8080
```

We should also enable `Quiet Mode` to reduce noise by un-commenting `quiet_mode`. 
## How To Use
Once that's done, we can prepend `proxychains` to any command, and the traffic of that command should be routed through `proxychains` (i.e., our web proxy).

For example, let's try using `cURL`:
```bash
proxychains curl http://SERVER_IP:PORT
```

We see that it worked just as it normally would, with the additional `ProxyChains-3.1` line at the beginning, to note that it is being routed through `ProxyChains`. If we go back to our web proxy (Burp in this case), we will see that the request has indeed gone through it:
