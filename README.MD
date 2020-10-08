# TL;DR
{% highlight bash %}
### Install server
docker volume create --name privacyidea_config
docker volume create --name privacyidea_log
docker volume create --name privacyidea_data
docker volume create --name privacyidea_mysql_data
docker run -p 5000:80 --name privacyidea --restart=always -d \
       -v privacyidea_config:/etc/privacyidea \
       -v privacyidea_log:/var/log/privacyidea \
       -v privacyidea_data:/var/lib/privacyidea \
       -v privacyidea_mysql_data:/var/lib/mysql \
       -v /data/privacyidea_data:/etc/mydata \
       -e PRIVACYIDEA_ADMIN_USER=admin \
       -e PRIVACYIDEA_ADMIN_PASS=your_password \
       pasientskyhosting/ps-privacyidea

### Install client
wget https://files.pythonhosted.org/packages/5e/64/7ee0b5e7b68b4f7c70ce3e2945d86bfcb1579d042e41d4f64260b72b03d5/privacyideaadm-2.15.1.tar.gz
tar -xvzf privacyideaadm-2.15.1.tar.gz
cd privacyideaadm-2.15.1
python setup.py install
mkdir -p /etc/privacyidea
cat > /etc/privacyidea/authorizedkeyscommand <<EOF
[Default]
url=http://192.168.0.181:5000
admin=admin
password=your_password
nosslcheck=False
EOF

yum install epel-release -y
yum install python-pip -y && pip install requests
echo "AuthorizedKeysCommand /usr/bin/privacyidea-authorizedkeys" >> /etc/ssh/sshd_config
echo "AuthorizedKeysCommandUser root" >> /etc/ssh/sshd_config
systemctl restart sshd
privacyidea-authorizedkeys root


{% endhighlight %}
