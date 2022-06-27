nexus <br>
#nexus installation <br>
firest update repository : <br>
debian : (# sudo apt-get update) <br>
redhat : (# sudo yum update) <br>

# install java :<br>
sudo apt install openjdk-8-jre-headless <br>

# Download latest version of nexus
cd /opt <br>
sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz <br>
sudo tar -zxvf latest-unix.tar.gz <br>
rename folder :<br>
sudo mv nexus-3.x.x.x/ /opt/nexus<br>
add user with nexus name and set sudo access to it an NOPASSWD : <br>
sudo adduser nexus <br>
sudo vi  /etc/sudoers <br>
under root part : (User prilvilage spcification or Allow root to run any commands anywhere  ) add this part : <br>
nexus ALL=(ALL)NOPASSWD:ALL (save and exit )<br>
give access to folders and file : <br>
sudo chown -R nexus:nexus /opt/nexus <br>
sudo chown -R nexus:nexus /opt/sonatype-work <br>
# To run nexus as service at boot time <br>
 open /opt/nexus/bin/nexus.rc file T uncomment it and nexus user as shown below : <br>
sudo vi /opt/nexus/bin/nexus.rc <br>
change as below : <br>
run_as_user="nexus"
# for increase the nexus JVM Heap Size : <br>
sudo /opt/nexus/bin/nexus.vmoptions <br>
-xms 2703 --> 1024 <br>
# to run nexus as systemd service we should create unit file :
sudo vi /etc/systemd/system/nexus.service

[Unit]
Description=nexus service <br>
After=network.target <br>
  
[Service]
Type=forking <br>
LimitNOFILE=65536 <br>
ExecStart=/opt/nexus/bin/nexus start <br>
ExecStop=/opt/nexus/bin/nexus stop <br>
User=nexus <br>
Restart=on-abort <br>
TimeoutSec=600 <br>
  
[Install]
WantedBy=multi-user.target <br>
# start and Enable service
sudo systemctl start nexus <br>
sudo systemctl status nexus <br>
sudo systemctl enable nexus <br>

# open port 8081 for web access 
(ufw) sudo ufw allow 8081/tcp  <br>
(firewalld) sudo firewall-cmd --add-port=8081/tcp <br>

# web user and password 
cat /opt/sonatype-work/nexus3/admin.password <br>
(user will be admin and password will show in admin.password) <br>











