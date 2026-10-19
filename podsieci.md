# Zeszyt podsieci

Metoda, której używam w każdym zadaniu:

1. Bity hosta = 32 − prefiks
2. Rozmiar bloku = 2^(bity hosta)
3. Dzielę odpowiednią liczbę adresu na bloki tej wielkości i sprawdzam, w którym bloku jest mój adres
4. Początek bloku = adres sieci, koniec bloku = broadcast, hosty = rozmiar bloku − 2

## Zadanie 1: 192.168.50.77/26

```
bity hosta = 32 − 26 = 6
blok       = 2^6 = 64          → maska 255.255.255.192
bloki w 4. liczbie: 0–63, 64–127, 128–191, 192–255
77 jest w bloku 64–127

adres sieci: 192.168.50.64
broadcast:   192.168.50.127
hosty:       192.168.50.65 – 192.168.50.126
liczba hostów: 64 − 2 = 62
```

## Zadanie 2: 10.10.10.200/25

```
bity hosta = 32 − 25 = 7
blok       = 2^7 = 128         → maska 255.255.255.128
bloki w 4. liczbie: 0–127, 128–255
200 jest w bloku 128–255

adres sieci: 10.10.10.128
broadcast:   10.10.10.255
hosty:       10.10.10.129 – 10.10.10.254
liczba hostów: 128 − 2 = 126
```

## Zadanie 3: 172.16.5.130/23

```
bity hosta = 32 − 23 = 9
blok       = 2^9 = 512 adresów → maska 255.255.254.0
512 adresów = 2 pełne "setki" po 256, więc bloki są w 3. liczbie co 2:
0–1, 2–3, 4–5, 6–7 ...
5 jest w bloku 4–5

adres sieci: 172.16.4.0
broadcast:   172.16.5.255
hosty:       172.16.4.1 – 172.16.5.254
liczba hostów: 512 − 2 = 510
```

## Zadanie 4: 192.0.2.9/30

```
bity hosta = 32 − 30 = 2
blok       = 2^2 = 4           → maska 255.255.255.252
bloki w 4. liczbie: 0–3, 4–7, 8–11, 12–15 ...
9 jest w bloku 8–11

adres sieci: 192.0.2.8
broadcast:   192.0.2.11
hosty:       192.0.2.9 – 192.0.2.10
liczba hostów: 4 − 2 = 2
```

## Zadanie 5: 192.168.200.0/24 na 8 podsieci

```
8 podsieci = 2^3 → potrzebuję 3 bity więcej → /24 + 3 = /27
bity hosta = 32 − 27 = 5
blok       = 2^5 = 32 adresy, 30 hostów → maska 255.255.255.224
```

| Podsieć | Adres sieci | Hosty | Broadcast |
|---|---|---|---|
| 1 | 192.168.200.0/27 | .1 – .30 | 192.168.200.31 |
| 2 | 192.168.200.32/27 | .33 – .62 | 192.168.200.63 |
| 3 | 192.168.200.64/27 | .65 – .94 | 192.168.200.95 |
| 4 | 192.168.200.96/27 | .97 – .126 | 192.168.200.127 |
| 5 | 192.168.200.128/27 | .129 – .158 | 192.168.200.159 |
| 6 | 192.168.200.160/27 | .161 – .190 | 192.168.200.191 |
| 7 | 192.168.200.192/27 | .193 – .222 | 192.168.200.223 |
| 8 | 192.168.200.224/27 | .225 – .254 | 192.168.200.255 |

Sprawdzenie: 8 × 32 = 256 – podsieci pokrywają całą pulę /24 i nie nachodzą na siebie.

## Zadanie 6: 10.50.0.0/16 na 4 podsieci

```
4 podsieci = 2^2 → potrzebuję 2 bity więcej → /16 + 2 = /18
bity hosta = 32 − 18 = 14
blok       = 2^14 = 16384 adresy = 64 "setki" po 256 → bloki w 3. liczbie co 64
maska 255.255.192.0, hostów w podsieci: 16384 − 2 = 16382
```

| Podsieć | Adres sieci | Hosty | Broadcast |
|---|---|---|---|
| 1 | 10.50.0.0/18 | 10.50.0.1 – 10.50.63.254 | 10.50.63.255 |
| 2 | 10.50.64.0/18 | 10.50.64.1 – 10.50.127.254 | 10.50.127.255 |
| 3 | 10.50.128.0/18 | 10.50.128.1 – 10.50.191.254 | 10.50.191.255 |
| 4 | 10.50.192.0/18 | 10.50.192.1 – 10.50.255.254 | 10.50.255.255 |

Sprawdzenie: 4 × 16384 = 65536 – podsieci pokrywają całą pulę /16 i nie nachodzą na siebie.
