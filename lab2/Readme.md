**with provisioned vagrant file**

# Лабораторная работа №2

Задание:

- Развернуть 3 виртуальных машины;
- Объединить эти виртуальные машины разными виртуальными каналами;
- Настроить OSPFv2 между виртуальными машинами на базе quagga;
- Настроить асимметричную маршрутизацию;

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

