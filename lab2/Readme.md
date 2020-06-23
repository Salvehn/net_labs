# Лабораторная работа №2

Задание:

- Развернуть 3 виртуальных машины;
- Объединить эти виртуальные машины разными виртуальными каналами;
- Настроить OSPFv2 между виртуальными машинами на базе quagga;

**Схема сети**

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2w58dbvcj31cr0u0gsh.jpg)
**запуск стенда**
`vagrant up`

...

`vagrant status`

![Снимок экрана 2020-06-21 в 22.39.31](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg0j4ftagjj30sq09278n.jpg)

### Общая настройка роутеров

```
cat /proc/sys/net/ipv4/ip_forward
sudo bash -c 'echo 1 > /proc/sys/net/ipv4/ip_forward'
sudo yum install nano -y	##nano > vi!
sudo nano /etc/sysctl.d/router.conf


net.ipv4.ip_forward = 1
net.ipv4.conf.all.forwarding = 1
net.ipv4.conf.all.rp_filter = 2


sudo yum install quagga -y
sudo touch /etc/quagga/ospfd.conf
sudo chown quagga:quagga /etc/quagga/ospfd.conf
##ubuntu - sudo chown quagga:quaggavty /etc/quagga/vtysh.conf
sudo chmod 640 /etc/quagga/ospfd.conf

sudo systemctl enable zebra
sudo systemctl enable ospfd
sudo systemctl start zebra
sudo systemctl start ospfd

systemctl status firewalld
sudo systemctl start firewalld
sudo firewall-cmd --add-protocol=ospf --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```


### Настройка Router 1

```
...
router ospf                  
router-id  10.0.0.1           
passive-interface default     
network 172.16.0.0/24 area 0  
no passive-interface eth1     
default-information originate
...
copy running-config startup-config
...
sudo iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
```

![Снимок экрана 2020-06-22 в 23.51.03](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1qt9zg6fj30vu0osnbu.jpg)

### Настройка Router 2

```
...
router ospf                   
router-id  10.0.0.2           
passive-interface default     
no passive-interface eth1     
network 172.16.0.0/24 area 0  
no passive-interface eth2     
network 172.16.1.0/24 area 1 
no passive-interface eth3     
network 172.31.0.0/24 area 0  
...
copy running-config startup-config
```

![Снимок экрана 2020-06-22 в 23.54.22](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1qwop6mvj30u010zau6.jpg)

### Настройка Router 3

```
...
router ospf                    
router-id  10.0.0.3            
passive-interface default      
no passive-interface eth1      
network 172.16.0.0/24 area 0   
no passive-interface eth2      
network 192.168.0.0/24 area 0 
no passive-interface eth3      
network 172.31.0.0/24 area 0   
...
copy running-config startup-config 
```

![Снимок экрана 2020-06-22 в 23.56.21](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1qyps546j30vu0tck8z.jpg)

### Настройка Router 4

```
...
router ospf                        роутера
router-id  10.0.0.4                
passive-interface default          
no passive-interface eth1          
network 172.16.1.0/24 area 1       
no passive-interface eth2          
network 192.168.1.0/24 area 1      
...
copy running-config startup-config 
```

![Снимок экрана 2020-06-22 в 23.58.03](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg1r0gypq8j30vu0oqk6l.jpg)