# FAQ

**Q:** The script has been updated since I installed OpenVPN. How do I update?

**A:** You can't. Managing updates and new features from the script would require way too much work. Your only solution is to uninstall OpenVPN and reinstall with the updated script.

You can, of course, it's even recommended, update the `openvpn` package with your package manager.

---

**Q:** What syctl and iptables changes are made by the script?

**A:** Iptables rules are saved at `/etc/iptables/add-openvpn-rules.sh` and `/etc/iptables/rm-openvpn-rules.sh`. They are managed by the service `/etc/systemd/system/iptables-openvpn.service`

Sysctl options are at `/etc/sysctl.d/20-openvpn.conf`

---

**Q:** How can I access other clients connected to the same OpenVPN server?

**A:** Add `client-to-client` to your `server.conf`

---

**Q:** My router can't connect

**A:**

- `Options error: No closing quotation (") in config.ovpn:46` :

  type `yes` when asked to customize encryption settings and choose `tls-auth`

**Q:** How can I access computers the OpenVPN server's remote LAN?

**A:** Add a route with the subnet of the remote network to `/etc/openvpn/server.conf` and restart openvpn. Example: `push "route 192.168.1.0 255.255.255.0"` if the server's LAN is `192.168.1.0/24`

---

**Q:** How can I add multiple users in one go?

**A:** Here is a sample bash script to achieve this:

```sh
userlist=(user1 user2 user3)

for i in ${userlist[@]};do
   MENU_OPTION=1 CLIENT=$i PASS=1 ./openvpn-install.sh
done
```

From a list in a text file:

```sh
while read USER
    do MENU_OPTION="1" CLIENT="$USER" PASS="1" ./openvpn-install.sh
done < users.txt
```

---

**Q:** How do I change the default `.ovpn` file created for future clients?

**A:** You can edit the template out of which `.ovpn` files are created by editing `/etc/openvpn/client-template.txt`

---

**Q:** For my clients - I want to set my internal network to pass through the VPN and the rest to go through my internet?

**A:** You would need to edit the `.ovpn` file. You can edit the template out of which those files are created by editing `/etc/openvpn/client-template.txt` file and adding

```sh
route-nopull
route 10.0.0.0 255.0.0.0
```

So for example - here it would route all traffic of `10.0.0.0/8` to the vpn. And the rest through the internet.
