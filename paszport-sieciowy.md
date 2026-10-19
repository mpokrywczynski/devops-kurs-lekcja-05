# Paszport sieciowy

Maszyna: WSL (Ubuntu 22.04) na Windows

## 1. Interfejsy

```
$ ip -br link
lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP>
eth0             UP             00:15:5d:91:9e:e2 <BROADCAST,MULTICAST,UP,LOWER_UP>
docker0          DOWN           26:10:4d:ab:09:5c <NO-CARRIER,BROADCAST,MULTICAST,UP>
```

Mam trzy interfejsy:
- `lo` – loopback, czyli komunikacja maszyny sama ze sobą.
- `eth0` – główna karta sieciowa, działa (`UP`). Jej MAC to `00:15:5d:91:9e:e2`.
- `docker0` – sieć Dockera, wyłączona (`DOWN`), bo nie mam uruchomionych kontenerów.

## 2. Adresy IP

```
$ ip -br addr
lo               UNKNOWN        127.0.0.1/8 ::1/128
eth0             UP             172.24.186.203/20 fe80::215:5dff:fe91:9ee2/64
docker0          DOWN           172.17.0.1/16
```

- Mój adres to `172.24.186.203/20`. To adres prywatny – nie jest widoczny w Internecie.
- `/20` oznacza, że moja sieć to `172.24.176.0 – 172.24.191.255`.
- `fe80::...` to adres IPv6, tworzy się automatycznie.

## 3. Tablica routingu

```
$ ip route show
default via 172.24.176.1 dev eth0 proto kernel
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
172.24.176.0/20 dev eth0 proto kernel scope link src 172.24.186.203
```

- Linia 1: wszystko, co idzie do Internetu, wysyłam przez bramę `172.24.176.1`.
- Linia 2: sieć Dockera – nieaktywna (`linkdown`).
- Linia 3: moja sieć lokalna – adresy z tego zakresu są osiągalne bezpośrednio, bez bramy.

## 4. Brama domyślna

```
$ ip route show default
default via 172.24.176.1 dev eth0 proto kernel

$ ip neigh show
172.24.176.1 dev eth0 lladdr 00:15:5d:61:0e:5f REACHABLE
```

Moja brama to `172.24.176.1`. Stan `REACHABLE` oznacza, że brama odpowiada.

## 5. Adres publiczny

```
$ curl https://api.ipify.org
31.178.229.170

$ curl https://ifconfig.me
31.178.229.170
```

Mój adres publiczny to `31.178.229.170`. Jest inny niż lokalny, bo router zamienia adres prywatny na publiczny (NAT).

## 6. Nasłuchujące porty

```
$ sudo ss -tulnp
Netid  State   Recv-Q  Send-Q  Local Address:Port  Peer Address:Port  Process
udp    UNCONN  0       0       127.0.0.53%lo:53    0.0.0.0:*          users:(("systemd-resolve",pid=1223,fd=13))
udp    UNCONN  0       0           127.0.0.1:323   0.0.0.0:*
udp    UNCONN  0       0               [::1]:323      [::]:*
tcp    LISTEN  0       4096    127.0.0.53%lo:53    0.0.0.0:*          users:(("systemd-resolve",pid=1223,fd=14))
```

| Port | Kto słucha | Dostępny z zewnątrz? |
|---|---|---|
| 53 | `systemd-resolve` (DNS) | nie, tylko `127.0.0.53` |
| 323 | nie wiadomo, `ss` nie pokazał procesu | nie, tylko `127.0.0.1` |

## 7. Wniosek

Żadna usługa nie jest dostępna z zewnątrz. Wszystkie porty nasłuchują tylko na adresach lokalnych (`127.x.x.x`), więc połączyć się z nimi może tylko ta sama maszyna.
