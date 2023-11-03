# forticlient_vpn

Debian packages of FortiClient 7.x.x for Ubuntu. `forticlient_vpn_*.deb` packages are the free, non-EMS, VPN-only VPN client used for connecting to Fortinet SSL-VPN services. See the different distribution kinds on [Fortinet's Administration Guide](https://docs.fortinet.com/document/forticlient/7.2.2/administration-guide/666761/linux).

Install the `.deb` package with `apt`.

```sh
sudo apt install ./forticlient_vpn_7.2.1.0700_amd64.deb
```

Unlike the official EMS client, there's no `forticlient` binary available on PATH to run. To connect to the VPN, directly use the binary installed in `/opt/forticlient`.

```sh
sudo /opt/forticlient/vpn -s <your_vpn_server> -u <your_username> -p
```

It will ask for `password:` via standard input. If you're using a GUI, there should be some GUI binaries in `/opt/forticlient` too.

## For use in CircleCI

I originally researched this to configure a CircleCI job to deploy on a server behind a Fortinet SSL-VPN. You can find the very hard to find Debian packages in [the releases](https://github.com/purfectliterature/forticlient_vpn/releases). Thanks to GitHub, these are all permalinks.

## Official EMS clients

The regular, EMS clients are available officially on [Fortinet's APT repository](https://repo.fortinet.com/repo/forticlient/7.2/ubuntu/pool/multiverse/forticlient/). To install officially from this repository, add the GPG keys and install with `apt`.

```sh
curl -sS https://repo.fortinet.com/repo/forticlient/7.2/ubuntu/DEB-GPG-KEY | gpg --dearmor | sudo tee /usr/share/keyrings/repo.fortinet.com.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/repo.fortinet.com.gpg] https://repo.fortinet.com/repo/forticlient/7.2/ubuntu/ /stable multiverse" | sudo tee /etc/apt/sources.list.d/repo.fortinet.com.list
sudo apt update
sudo apt install forticlient
```

>[!NOTE]
>Fortinet's official guides to install FortiClient on Linux [here](https://www.fortinet.com/support/product-downloads/linux), [here](https://docs.fortinet.com/document/forticlient/7.2.2/linux-release-notes/213138/install-forticlient-linux-from-repo-fortinet-com), and [here](https://repo.fortinet.com/) are all bogus and will fail at `sudo apt update` because they are not in sync with the actual APT repository. There is no `non-free` distribution in the repository. There's `/stable/multiverse`. Also, `apt-key` is deprecated in later versions of Ubuntu.

Run `forticlient` to connect to the VPN.

>[!IMPORTANT]
>This this client will give you [this error](https://www.reddit.com/r/fortinet/comments/16rslds/fortinet_sslvpn_is_unavailable_forticlient_vpn/) on incompatible SSL-VPN services.
>```
>FortiClient VPN Trial has expired. Please contact your administrator or connect to EMS for license activation.
>```

## Starting and stopping the service

Use `systemctl` to start or stop the VPN (in both EMS and non-EMS clients).

```sh
systemctl start forticlient.service
systemctl stop forticlient.service
```
