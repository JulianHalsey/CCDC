<h1>CCDC Notes</h1>
Linux machines
<br />

<h3>1. Updating</h3>

- Update information about software (not actual software) : apt-get update
- Update actual software : apt dist-upgrade
- Enable automatic updates : apt install unattended-upgrades
  - Setup : dpkg-reconfigure --priority=low unattended-upgrades

<h3>2. Anti-malware/virus Software</h3>

- apt-get install clamav : install virus scanner
  - Update virus definitions : freshclam
  - Run scanner : clamscan -i (only show infected) r (recursively) / (where to scan)
- rkhunter -c : rootkit and exploitation scanner
- chkrootkit -q : shows what rootkit finds suspicious

<h3>3. Passwords</h3>

- Install password policy library (password authentication modules- PAM) : apt-get install libpam-cracklib
- /etc/pam.d : Set of libraries to manage user authentication
  - /commom-password : password configuration (length, complexity, etc.)
    - pam_unix.so
      - remeber=5
        - Can't reuse last 5 password
      - minlen=8
        - minimum password length
    - pam_cracklib.so
      - ucredit=1 lcredit=1 dcredit=1 ocredit=1
        - upper, lower, number, symbol # required
  - /common-auth
    - auth required pam_tally2.so deny=5 onerr=fail unlock_time=1800
      - Add to end of file
      - Number of allowed failed logins = 5
      - account lockout duration = 1800 seconds (30 min)
    - Don't get locked out
- /etc/login.defs : password expiry, not PAM
  - PASS_MAX_DAYS    90
    - Maximum passowrd duration
  - PASS_MIN_DAYS    10
    - Minimum duration before password expires
  - PASS_WARN_AGE    7
    - Days before expiration to warn users
    
      - minclass = minimum # of character types that must be used (i.e., uppercase, lowercase, digits, other)
      - maxrepeat = maximum number of times a single character may be repeated
      - maxclassrepeat = maximum number of characters in a row that can be in the same class
      

<h3>Apache Server</h3>

- Port 80 and 443
- Primary configuration file : /etc/apache2/apache2.conf
- Change name : /etc/localhost
- Change IP address : /etc/hosts
- Install : apt install apache2 apache2-doc apache2-utils
- Check status OR stop/start/restart : systemctl status/stop/start/restart apache2 
  - If disabled : systemctl enable apache2
  - Restarting drops all users so better to reload
- View available modules : apt search libapache2-mod
- (Ubuntu) Enable/Disable mod : a2enmod / a2dismod
- Enable/Disable a site : a2ensite / a2dissite
  - View sites : ls /etc/apache2/sites-available
  - Create new site : nano /etc/apache2/sites-available/name.net.conf
  
<h3>mySQL</h3>

- TCP/3306
- Install : apt install mysql-server
- Security:
  - To start : sudo mysql_secure_installation
  - Set password for mySQL
    - If error : sudo mysql then ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'SetRootPasswordHere';
  - Press y for rest of questions to make it more secure

<h3>Firewall</h3>

- See open ports : sudo ss -tulpn
- Install ufw : sudo apt install ufw
- View config file : nano /etc/default/ufw
- Do all configurations before enabling : ufw disable
- Reset policies : ufw reset
- Deny incoming connections : ufw default deny incoming
- Allow outgoing connecitons : ufw default allow outgoing
- If other systems not working : sudo systemctl stop ufw
- Allow services or ports : ufw allow ssh/http
  - Only allow certain IP to service : ufw allow from 10.0.0.1 to any port 22
- Allow certain IP address : ufw allow from 10.0.0.1
- View allowed ports : ufw status
- Enable it : sudo ufw enable
- View numbered rules : ufw status numbered
  - Delete rule : ufw delete #
- Only need allow for rules since default set to deny
- Hide server
  - Ping IP : ping (server IP) -t
  - Block ping :
    - Edit file : sudo nano /etc/ufw/before.rules
      - Under "ok icmp codes for input" enter line : -A ufw-before-input -p icmp —icmp-type echo-request -j DROP
      - Exit nano : ctrl+x, y, enter
    - Restart firewall : sudo ufw reboot
    
 <h3>Logging</h3>
 
 - All logs stored : ls -al /var/log/
 - Conatains info on failed login attempts : auth.log
   - To search for certain service : | grep -a "sshd"
 - List of recent logins and info : last -aiF
 - Bad-logon attempts to root : lastb -adF root
 - Find last login of specific user : lastlog -u root
 - Show logged-on users : who -aH OR w
 - View processes and reosurces running : htop
   - F6 to search and F9 to kill
 - Overview of entire system : glances
 - Monitor logged on users and what they're doing : whowatch
   - Ctrl+K to remotely kill process


<h3>Creating authentication key pair</h3>

- Store public keys : mkdir ~/.ssh && chmod 700 ~/.ssh
- Backout of server : logout
- Create new server keys: ssh-keygen -b 4096
- View keys : cd .ssh then ls
  - id_rsa is Private and id_rsa.pub is Public
- Upload public key to server
  - Windows :scp $env:USERPROFILE/.ssh/id_rsa.pub (name)@(server ip):~/.ssh/authorized_keys
  - Linux : ssh-copy-id (name)@(server IP)

   

<h3>Determining who's logged in</h3>

- History of logons to system : use Aureport
  - Install : sudo apt-get install aditd
  - Information about authenticaiton attempts : aureport -au
  - Failed login attempts from current day : aureport -au --failed --start today


<h3>Determining User Activity</h3>

- Find UID of given account name : id (name)
- Find username of given UID : getent passwd (UID)
- User account UID's begin with 500 or 100 (system accounts < 500)
- Provides list of commands ran by user : history

<h3>SSH</h3>

- Install : sudo apt-get install openssh-server
- View status : systemctl status ssh
- Configuration in : /etc/ssh/sshd_config
