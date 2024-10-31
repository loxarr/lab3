***Лабараторная работа №3***

Сперва я создал NAT Network в VirtualBox, используя терминал.(Почему-то в настройках не получалось это сделать)
```
## Check existing NAT Networks
$ VBoxManage list natnetworks

## Create a NAT Network
$ VBoxManage natnetwork add --netname NATNetwork101 --network "192.168.10.0/24" --enable

## Check the NAT Network
$ VBoxManage list natnetworks
NetworkName:    NATNetwork101
IP:             192.168.10.1 
Network:        192.168.10.0/24
IPv6 Enabled:   No
IPv6 Prefix:    fd17:625c:f037:2::/64
DHCP Enabled:   Yes     
Enabled:        Yes     
loopback mappings (ipv4)
        127.0.0.1=2     

## Enable or Disable DHCP for the network (on or off)
$ VBoxManage natnetwork modify --netname NATNetwork101 --dhcp on

## Start the NAT service
$ VBoxManage natnetwork start --netname NATNetwork101

## Enable Port Forwarding to connect to the VMs
## Forward localhost port 1022 to 192.168.10.5:22 (eg: SSH)
$ VBoxManage natnetwork modify --netname NATNetwork101 \
  --port-forward-4 "ssh:tcp:[]:1022:[192.168.10.5]:22"

## If you need to remove the NAT Network
$ VBoxManage natnetwork remove --netname NATNetwork101
```

Проверка доступа в сеть Интернет на машине А:

<img width="403" alt="Снимок экрана 2024-10-31 в 11 47 16" src="https://github.com/user-attachments/assets/83eb9c34-d1d2-4286-a577-65d9c0c2017b">

Проверка доступа из машины А в машину Б:

<img width="306" alt="Снимок экрана 2024-10-31 в 11 48 03" src="https://github.com/user-attachments/assets/1fb38e69-75a9-473d-8b7a-0ec628a28255">

Проверка доступа из машины А в машину В:

<img width="317" alt="Снимок экрана 2024-10-31 в 11 49 40" src="https://github.com/user-attachments/assets/62f0f8c1-c2e3-44ff-93ee-5be6a78fa235">

Затем прописал команду в машине Б доступ к машине В:
```
sudo iptables -A INPUT -s 192.168.10.5  -j DROP
```

Все три машины:
<img width="1512" alt="Снимок экрана 2024-10-31 в 11 58 46" src="https://github.com/user-attachments/assets/7e3cefea-d864-455f-8161-d92695ca4bd0">




