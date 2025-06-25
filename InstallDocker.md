This is for the Jetson Orin Nano (Jetpack makes everything so hard)

Get the repositories:
```
sudo apt-get update && sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg
```

Get the DOWNGRADED VERSION OF DOCKER 5.27 (the most recent one does not work with Jetpack)
```
sudo apt-get install --allow-downgrades -y \\
docker-ce=5:27.5.1-1~ubuntu.24.04~noble \\
docker-ce-cli=5:27.5.1-1~ubuntu.24.04~noble \\
docker-buildx-plugin=0.20.0-1~ubuntu.24.04~noble \\
docker-compose-plugin=2.27.1-1~ubuntu.24.04~noble
sudo apt-mark hold docker-ce docker-ce-cli docker-buildx-plugin docker-compose-plugin
```

Switch to legacy iptables (nftables are incompatible with Docker)
```
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
```

Load and persist the kernal modules
```
echo -e 'xt_addrtype\niptable_nat\nnf_nat\nbr_netfilter'
```

Enable forwarding and bridge hooks
```
udo tee /etc/sysctl.d/99-docker.conf <<'EOF'
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF
sudo sysctl --system
```

Mount security nonsense for AppArmor
```
sudo mkdir -p /sys/kernel/security
grep -q securityfs /etc/fstab
```

Start and enable Docker services
```
sudo systemctl enable --now containerd docker
docker info --format '{{.ServerVersion}}'
```

Hold these packages so they don't show up as upgradable in apt update
```
sudo apt-mark hold docker-ce docker-ce-cli docker-buildx-plugin docker-compose-plugin
```

Add user to the docker group
```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

Test that it's working
```
docker run hello-world
```
