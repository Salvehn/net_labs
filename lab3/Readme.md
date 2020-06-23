## Лабораторная работа 3

Задание:



- Проверить работу стенда
- Создать другой инстанс vrrp
- Настройка ACL
- Настройка логгирования haproxy
- Настройка отображения реального ip клиента в логгах nginx

Проверка работоспособности

![Снимок экрана 2020-06-23 в 20.50.42](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2r7wu9ehj314k09cwga.jpg)


![Снимок экрана 2020-06-23 в 21.29.59](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2scq6s1vj313w0skag5.jpg)

![Снимок экрана 2020-06-23 в 21.28.11](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2savq5ydj313w0skdlx.jpg)

**Имитация сбоя**

```
vagrant halt web1
lynx http://10.0.26.81
```

![Снимок экрана 2020-06-23 в 21.33.17](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2sg67k4lj313w0skdlx.jpg)

**haproxy2 keepalived.conf**

![Снимок экрана 2020-06-23 в 22.12.46](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2tl9yr3mj30wg0u0dkj.jpg)

**haproxy1 keepalived.conf**

![Снимок экрана 2020-06-23 в 22.13.35](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2tm46slnj30wg0u0dkj.jpg)

**haproxy2 tcpdump**

![telegram-cloud-photo-size-2-5445006504345317467-y](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2tmv9p42j30wg0u0k5h.jpg)

**Проверка**

```
lynx http://10.0.26.91
```

![Снимок экрана 2020-06-23 в 22.23.03](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2tvyjtgxj30wg0u0grr.jpg)



**Настройка ACL**

![Снимок экрана 2020-06-23 в 22.57.23](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2uvnhzw7j313w0m20w0.jpg)

```
lynx http:/10.0.26.81/index.html
```

![Снимок экрана 2020-06-23 в 22.59.32](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2uxx22ofj313w0sqjxl.jpg)

```
lynx http://10.0.26.81/status.html
```

![Снимок экрана 2020-06-23 в 23.00.03](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2uyf3jm6j313w0sqq4v.jpg)

**Настройка логгирования haproxy**

![Снимок экрана 2020-06-23 в 23.03.13](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2v1plezpj313w07o3zn.jpg)

**результат /var/log/messages**

![telegram-cloud-photo-size-2-5445006504345317518-y](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2v6yoomaj30zk0oan7t.jpg)



**Настройка отображения реального ip клиента в логах Nginx**

настройка Nginx.conf на web1

![Снимок экрана 2020-06-23 в 23.11.52](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2vapkmb4j313w0aa75y.jpg)

```
lynx http://10.0.26.81/index.html
```

![telegram-cloud-photo-size-2-5445006504345317526-y](https://tva1.sinaimg.cn/large/007S8ZIlgy1gg2voxe07yj30zk0kfjzv.jpg)