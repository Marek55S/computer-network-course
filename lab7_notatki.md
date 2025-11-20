Lab 7

(wklej zdjęcie notatek z tablicy)

Część praktyczna

DHCP konfiguracja serwera:

Konfiguracja intereface routera:
(dla jednego komputera maska 11.1.1.0/24, dla drugiego 13.3.3.0/24)

```bash
enable
config t
int fa0/0
ip address 11.1.1.1 255.255.255.0
no shutdown
end
```

to pierwsze pokazuje podgląd wszystkich interfejsów i ich stanów na urządzeniu, a drugie pokazuje tablicę routingu IP routera:

```bash
show ip interface brief
show ip route
```

Konfiguracja serwera DHCP:
(dla jednego komputera maska 11.1.1.0/24, dla drugiego 13.3.3.0/24 i wszystkie adresy tak zmieniamy)
(i tak samo dla drugiego będzie POOL_13)

```bash
enable
config t
service dhcp
ip dhcp pool PULA_11
network 11.1.1.0 /24
default-router 11.1.1.1
dns-server 11.1.1.2
exit
ip dhcp excluded-address 11.1.1.2 11.1.1.15
end
```

```bash
show ip dhcp pool
show ip dhcp binding
show ip dhcp server statistics
```

(wrzuć zdjęcie kabelków do tego momentu)

Konfiguracja interfejsu szeregowego:

(wrzuć zdjęcie podpięcia routerów kabelkem szeregowym)

```bash
enable
show ip interface brief
config t
int s0/0 ! tutaj w zależności co pokaże 2 komenda, port tutaj ustawiamy więc może być np serial0/0/0 itd
ip address 12.2.2.1 255.255.255.252 ! tutaj w ramach tego, że mamy 2 routery i maske /30 i nie możemy ustawić 0 ani 3 w ostatnim oktecie to ustawiamy 1 i 2
clock rate 125000 ! Tylko DCE !
no shutdown
end
```

trzeba sprawdzić: show controllers <serial (np. serial0/0/0)> jest po to, żeby sprawdzić który router ma DCE a który DTE, bo kabelek jest symetryczny tutaj akurat i z tego będziemy wiedzieć

sprawdzenie:

```bash
show ip interface brief
show controllers serial 0/0
show ip route
```

potem trzeba skonfigurować trasę statyczną na obu routerach:

```bash
enable
conf t
ip route <dokąd> <którędy> ! na jednym komputerze będzie to wyglądać tak jak niżej, na drugim zmieniamy adres podsieci i port routera przez który to idzie
ip route 13.3.3.0 255.255.255.0 12.2.2.2
end
```

na koniec można pinga zrobić żeby sprawdzić czy działa

DHCP centralizacja konfiguracji:

najpierw na jednym routerze wyłączamy sobie (chyba serwer DHCP na jednym routerze):

```bash
conf t
no ip dhcp pool PULA_13
```

```bash
enable
config t
service dhcp
ip dhcp pool PULA_13
network 13.3.3.0 /24
default-router 13.3.3.1
dns-server 13.3.3.2
exit
ip dhcp excluded-address 13.3.3.2 13.3.3.15
end
```

jeszcze na końcu trzeba:
(config-if)# ip helper-address 12.2.2.1
czyli:

```bash
enable
config t
int fa0/0
ip helper-address 12.2.2.1
```

na końcu sprawdzamy sobie jaki adres dostaliśmy z DHCP i pingujemy (to akurat w terminalu a nie w konsoli)

```bash
ipconfig
ping <to co pokazał config na drugim hoście>
```
