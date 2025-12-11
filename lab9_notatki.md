### Routing Dynamiczny:

podział na wewnętrzne i zewnętrzne

AS - autonomus system (system autonomiczny) - sieć lub grupa sieci zarządzana przez jedną organizację.

Zewnętrzne - BGP (Border Gateway Protocol) (w dużej większości, innymi się nie zajmujemy)

Podział wewnętrznych ze względu na działanie:

- distance vector
- link state (stan łącza)

```bash
en
conf t
router rip
...
```

polecenie network - coś popsuło (ogłosiło sieć, włączyło wysyłanie i odbieranie komunikatów)

sh ip protocols - pokazuje jakie protokoły są włączone i jakie sieci są ogłaszane (pokazuje co się dzieje - fajnie)

#### Część praktyczna

Odwzorowywujamy sieć ze zdjęcia (tylko na początku bez switchy - czyli 2 komputery, 3 routery)

ustawianie routerów przez konsolę (dla każdego routera zrobić osobno)

```bash
en
conf t
int gi0/0
ip address 10.0.0.1 255.0.0.0
no shutdown
end

en
conf t
int gi0/1
ip address 11.0.0.1 255.0.0.0
no shutdown
end

sh ip int brief
sh ip route
```

ustawianie hosta
(dla każdego hosta osobno)

```bash
setip 10.0.0.2 255.0.0.0 10.0.0.1
ipconfig
```

po tym jak ustawimy wszystko powinniśmy mieć po wpisaniu komendy `sh ip route` na każdym routerze wpisy o sieciach bezpośrednio podłączonych
(zdjęcie z terminala)
pingi powinny działać między hostami

Teraz będziemy podłączać do całej sieci switche między hostami i routerami w sieciach 10.0.0.0 oraz 13.0.0.0
trzeba poczekać aż switche wstaną i powinno zadziałać samo (jak się pingnie to będzie g)

następnie psuje się sieć na routerze 13, bo został podpięty nowy router
Wyłączamy to na 13.0.0.0 i zamiast komendy `network` wrzucamy `redi conn`

```bash
no network 13.0.0.0
conf t
router rip
version 1
redi conn
end
```
