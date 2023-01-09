<h1>CCDC Notes</h1>
Linux machines
<br />

<h3>1. Updating</h3>

- Update information about software (not actual software) : apt update
- Update actual software : apt dist-upgrade
- Enable automatic updates : apt install unattended-upgrades
  - Setup : dpkg-reconfigure --priority=low unattended-upgrades

<h3>2. Anti-malware/virus Downloads</h3>

- clamav : scans for viruses
- rkhunter : 
- chkrootkit : 

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

<h3>Determining who's logged in</h3>

- Show logged-on users : who -aH OR w
- List of recent logins : last
- History of logons to system : use Aureport
  - Install : sudo apt-get install aditd
  - Information about authenticaiton attempts : aureport -au
  - Failed login attempts from current day : aureport -au --failed --start today
  
<br>

<h3>Determining User Activity</h3>

- Find UID of given account name : id (name)
- Find username of given UID : getent passwd (UID)
- User account UID's begin with 500 or 100 (system accounts < 500)
- Provides list of commands ran by user : history

<br>

<h3>SSH</h3>

- Install : sudo apt-get install openssh-server
- View status : systemctl status ssh
- Configuration in : /etc/ssh/sshd_config
