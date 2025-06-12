# DNS Records
We got a message saying `Admin panel moved to academy.htb`. If we visit the website in our browser, we get `can’t connect to the server at www.academy.htb`.

This is because the exercises are not public websites that can be accessed by anyone but local websites within HTB.

Browsers only understand how to go to IPs, and if we provide them with a URL, they try to map the URL to an IP by looking into the local `/etc/hosts` file and the public DNS `Domain Name System`. If the URL is not in either, it would not know how to connect to it.

If we visit the IP directly, the browser goes to that IP directly and knows how to connect to it. But in this case, we tell it to go to `academy.htb`, so it looks into the local `/etc/hosts` file and doesn't find any mention of it.

So, to connect to `academy.htb`, we would have to add it to our `/etc/hosts` file. We can achieve that with the following command:
```bash
sudo sh -c 'echo "SERVER_IP  academy.htb" >> /etc/hosts'
```

Now we can visit the website (don't forget to add the PORT in the URL)

[[Cybersecurity/CBBH Modules/1. Recon/2. FFUF/Reference|Reference]]