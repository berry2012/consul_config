# Consul Config Demo
Consul service discovery tool examples contributed by Andrew Puch

```
# Run these on all consul servers.
sudo su
apt-get update
apt-get install git unzip -y
cd /root
wget https://dl.bintray.com/mitchellh/consul/0.5.2_linux_amd64.zip
unzip 0.5.2_linux_amd64.zip
rm -f 0.5.2_linux_amd64.zip
mv consul /usr/bin/
git clone https://github.com/berry2012/consul_demo.git
```
# make sure it has execute access. 
chmod -R 775 /root/scripts should do it!

```
# Bootstrap / Web UI Server
---------------------------
wget https://dl.bintray.com/mitchellh/consul/0.5.2_web_ui.zip
unzip 0.5.2_web_ui.zip
rm -f 0.5.2_web_ui.zip
cd /root/consul_demo
consul keygen
cp bootstrap.json config.json

# Save this keygen! Note, if your key has a slash in it you need to escape them for setup.sh. Or just regenerate one until it doesn't have a slash :)
consul keygen 
```

```
# Non Boostrap Consul Server
---------------------------
cd /root/consul_demo
cp server.json config.json
```

```
# Consul Agent Server
---------------------------
apt-get install apache2 -y
cd /root/consul_demo
cp agent.json config.json
```

```
./setup.sh HOSTNAME ENCRYPT_KEY IP_OF_BOOTSTRAP IP_NON_BOOTSTRAP
nohup consul agent -config-dir /root/consul_demo/config.json &



# Now lets test on our agent server.
curl -X PUT -d 'test' http://localhost:8500/v1/kv/web/key1
curl http://localhost:8500/v1/kv/web/key1
curl http://localhost:8500/v1/kv/web/key1?raw
```
