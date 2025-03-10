# Install Hazelcast as a Linux systemd service

This repository holds sample scripts showing how to install Hazelcast
as a service on a Linux with systemd.

## What?
Install the Hazelcast as a Linux (systemd) service.

## How?

Clone this repo
```bash
git clone https://github.com/oifayulian/hazelcast-linux-service.git
```

Run following commands as the `root` (`sudo su`)

```bash
cd hazelcast-linux-service

# Create the hazelcast user/group
groupadd -r hazelcast
useradd -r -g hazelcast -d /opt/hazelcast -s /sbin/nologin hazelcast

# Install Hazelcast
HAZELCAST_VERSION=5.3.2
wget https://github.com/hazelcast/hazelcast/releases/download/v$HAZELCAST_VERSION/hazelcast-$HAZELCAST_VERSION.zip
unzip hazelcast-$HAZELCAST_VERSION.zip -d ./hazelcast-release
mv ./hazelcast-release/hazelcast-$HAZELCAST_VERSION /opt/hazelcast
# Change owner of the Hazelcast directories
chown -R hazelcast:hazelcast /opt/hazelcast

# Copy service and config files
rsync -r etc/ /etc

# Start and enable service
systemctl daemon-reload
systemctl enable hazelcast.service
systemctl start hazelcast.service
```

Check if the Hazelcast is running by calling REST URL:
```
curl http://localhost:5701/hazelcast/rest/cluster
```
