<h1>CCDC Notes</h1>

<br />

<h3>Updating</h3>

- Update server manually : apt update
- If update needed : apt dist-upgrade
- Enable automatic updates : apt install unattended-upgrades
  - Setup : dpkg-reconfigure --priority=low unattended-upgrades

<br>

<h3>Creating authentication key pair</h3>

- Store public keys : mkdir ~/.ssh && chmod 700 ~/.ssh
- Backout of server : logout
- Create new server keys: ssh-keygen -b 4096
- View keys : cd .ssh then ls
  - id_rsa is Private and id_rsa.pub is Public
- Upload public key to server
  - Windows :scp $env:USERPROFILE/.ssh/id_rsa.pub (name)@(server ip):~/.ssh/authorized_keys
  - Linux : ssh-copy-id (name)@(server IP)

<br>

<h3>Firewall</h3>

- See open ports : sudo ss -tulpn
- Install ufw : sudo apt install ufw
  - Put in door : sudo ufw all (port #)
  - Enable it : sudo ufw enable
- Hide server
  - Ping IP : ping (server IP) -t
  - Block ping :
    - Edit file : sudo nano /etc/ufw/before.rules
      - Under "ok icmp codes for input" enter line : -A ufw-before-input -p icmp —icmp-type echo-request -j DROP
      - Exit nano : ctrl+x, y, enter
    - Restart firewall : sudo ufw reboot
    
    <br>

<h3>Determinding who's logged in</h3>

- Show logged-on users : who -aH OR w
- List of recent logins : last
