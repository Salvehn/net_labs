# Задание 1

![Networking](https://tva1.sinaimg.cn/large/007S8ZIlgy1gftdo5romtj30w40k4ab6.jpg)

Сети:

- 192.168.0.0
- 192.168.1.0
- 192.168.2.0

# Задание 2

### Первая сеть

Подсети:

#### 192.168.2.0/26

Узлов: 62 Broadcast: 192.168.2.63

#### 192.168.2.64/26

Узлов: 62 Broadcast: 192.168.2.127

#### 192.168.2.128/26

Узлов: 62 Broadcast: 192.168.2.191

### Вторая сеть

Подсети:

#### 192.168.1.0/25

Узлов: 126 Broadcast: 192.168.1.127

#### 192.168.1.128/26

Узлов: 62 Broadcast: 192.168.1.191

#### 192.168.1.192/26

Узлов: 62 Broadcast: 192.168.1.255

### Третья сеть

Подсети:

#### 192.168.0.0/28

Узлов: 14 Broadcast: 192.168.0.15

Самая большая подсеть: 192.168.1.0/25 Ошибок разбиения нет.



# Задание 3

**office1Server**

![Снимок экрана 2020-06-15 в 18.19.16](https://tva1.sinaimg.cn/large/007S8ZIlgy1gftdvu254nj30v609safi.jpg)

**office2Server**

![Снимок экрана 2020-06-15 в 18.20.37](https://tva1.sinaimg.cn/large/007S8ZIlgy1gftdx8jkd9j30v609k79o.jpg)

**centralServer**

![Снимок экрана 2020-06-15 в 18.50.32](https://tva1.sinaimg.cn/large/007S8ZIlgy1gftesdf7zuj30v60ecwmg.jpg)

# Задание 4

**Блокировка на 80 порт**
![Снимок экрана 2020-06-15 в 18.59.28](https://tva1.sinaimg.cn/large/007S8ZIlgy1gftflxgw9vj30v20aktd0.jpg)

**Блок ping**

![Снимок экрана 2020-06-16 в 10.08.23](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfu5bgvht7j31nw0u07wh.jpg)

# Задание 5

### port knocking

разрешаем соединения со статусами established и related 
`iptables -A INPUT -m state --state ESTABLISHED, RELATED -j ACCEPT`

разрешаем протокол icmp
`iptables -A INPUT -p icmp --icmp-type any -j ACCEPT`

создаем цепочку правил для PK
`iptables -N SSH_KNOCK`

создаем переход и цепочки INPUT в цепочку SSH_KNOCK
`iptables -A INPUT -j SSH_KNOCK`

Создаем цепочку правил для SSH_SET
`iptables -N SSH_SET`

предоставляем доступ, если наш хост есть в списке SSH_STEP2 менее 60 секунд
`iptables -A SSH_KNOCK -m state --state NEW -m tcp -p tcp -m recent --rcheck --seconds 60 --dport 22 --name SSH_STEP2 -j ACCEPT`

Если наш хост в списке более 60 секунд, то удаляем его
`iptables -A SSH_KNOCK -m state --state NEW -m tcp -p tcp -m recent --name SSH_STEP2 --remove -j DROP`

если наш хост стучался по порту 9966 и был в списке SSH_STEP1
`iptables -A SSH_KNOCK -m state --state NEW -m tcp -p tcp -m recent -rcheck --dport 9966 --name SSH_STEP1 -j SSH_SET`

то включаем его в список SSH_STEP2
`iptables -A SSH_SET -m recent --set --name SSH_STEP2 -j DROP`

иначе удаляем его из списка SSH_STEP1
`iptables -A SSH_KNOCK -m state --state NEW -m tcp -p tcp -m recent --name SSH_STEP1 --remove -j DROP`

если хост стучится по порту 6699, то добавить его в список SSH_STEP1
`iptables -A SSH_KNOCK -m state --state NEW -m tcp -p tcp -m recent --set --dport 6699 --name SSH_STEP1 -j DROP`

DROP по-умолчанию в PK
`iptables -A SSH_KNOCK -j DROP`

DROP по-умолчанию со стороны откуда будет идти проверка
`iptables -A INPUT -i eth1 -j DROP`



скрипт *knock.sh*

```HOST=$1
shift
for ARG in "$@"
do
		nmap -Pn --host-timeout 100 --max-retries 0 -p $ARG $HOST
done
```

![Снимок экрана 2020-06-17 в 11.34.56](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfvdfu7loij30yu0ludsx.jpg)