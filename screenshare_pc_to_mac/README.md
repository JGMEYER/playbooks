# Secure Remote Connection from PC to MacOS with VNC and SSH Tunneling

## Setup:

```
NOTE: These items are ordered in such a way to provide the best opportunities
for troubleshooting issues as you follow the steps.
```

- [MAC] Enable "Screen Sharing" and "Remote Login" under "Sharing" in System Preferences.
- [MAC] Under "Screen Sharing">"Computer Settings...", check "VNC viewers may control screen with password" and include a simple password.
- [PC] Generate pub/private keys with [puttygen.exe](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Create a name and a secure password, then "save private key" (for example to C:/SSH/key-for-use-with-macbook-pro-20190904.ppk). Hold on to the public key.
- [MAC] Copy/paste public key from Windows to `~/.ssh/authorized_keys`. Add this all on one line and even include the "ssh-rsa" at the beginning.
```
mkdir ~/.ssh
chmod 700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chown -R $USER:staff ~/.ssh
sudo launchctl stop com.openssh.sshd
sudo launchctl start com.openssh.sshd
```
- [PC] In [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html):
  - `Connection>SSH>Auth`: upload private key file to bottom-most field.
  - `Connection>SSH>Tunnels`: forward port 5901 to "localhost:5900". This means all traffic from 5901 on the PC will be pushed through the 5900 port on the MAC.
  - `Session`: configure session to connect to `<MACBOOK_IP>:22`. This will connect you to the macbook via SSH and ultimately securely forward information from PC:5901 to MAC:5900.


## To connect:
- [PC] Run your PuTTY session. You'll need to provide the username on the MAC and the password you set for the private key on your PC.
- [PC] In [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/), or another VNC client, point to localhost:5901 and connect. You'll need to provide the password you set on your MAC under `"Sharing">"Screen Sharing">"Computer Settings..."`. The connection should be secure, despite the notification. See the explanation why [here](https://community.spiceworks.com/topic/603622-how-to-vnc-over-ssh-from-win7-to-os-x-mavericks).

## Troubleshooting:
- Confirm you can establish an unsecured connection (AT YOUR OWN RISK!) to your Mac by pointing your VNC client to `<MACBOOK_IP>:5900`.
- Confirm you can connect to your Mac through SSH by running your PuTTY configuration and logging in using your MAC username and private key password.

## References:
- https://www.howtogeek.com/214220/how-to-access-your-macs-screen-from-windows-and-vice-versa/
- https://superuser.com/a/292096
- https://blog.djmnet.org/2009/05/11/macos-remote-desktop-over-the-internet-using-ssh-secure-shell/
- https://askubuntu.com/questions/306798/trying-to-do-ssh-authentication-with-key-files-server-refused-our-key
- https://apple.stackexchange.com/questions/157056/windows-putty-connect-to-osx-ssh-server-with-ssh-key
- https://superuser.com/questions/215504/permissions-on-private-key-in-ssh-folder
- https://community.spiceworks.com/topic/603622-how-to-vnc-over-ssh-from-win7-to-os-x-mavericks
