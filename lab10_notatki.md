OSPF
wewnętrzny protokół routingu typu link-state,

jak jest zmiana w sieci to wysyła informację do wszystkich routerów w AS (autonomous system) o stanie łącza bezpośrednio połączonych z nim sieci,

używa floodingu do rozgłaszania informacji o stanie łączy,
Ma swoje IP

Każdy router ma swój numer sekwencyjny po to żeby wiadomo było który update jest nowszy

Żeby zwiększyć skalowalność OSPF dzieli sieć (system autonomiczny) na obszary

-   Area 0 - backbone area (obszar szkieletowy) - wszystkie inne obszary muszą być z nim połączone

ASBR - autonomous system boundary router - router graniczny systemu autonomicznego - łączy różne systemy autonomiczne

## Realizacja ćwiczenia

### Połączenie routerów

3 routery połączone w trójkąt
![topologia](./images/lab10_topologia.png)
![zdjęcie](image.png)
^ tutaj zwrócić uwagę na to, że w R14 interfejs GA0/0/0 jest po prawej XD

Wszystko w sieci 202.0.0.64/26

Konfigurujemy tak, jak w VLSM.

-   R12:
    en
    conf t
    int fa0/0
    ip address 202.0.0.97 255.255.255.240
    no shutdown
    end

analogicznie sieci S0/2/0 i S0/2/1
adresy na rysunku

-   R13:
    en
    conf t
    int fa0/0
    ip address 202.0.0.65 255.255.255.224
    no shutdown
    end

analogicznie sieci S0/0/0 i S0/0/1
adresy na rysunku

-   R14:
    en
    conf t
    int S0/0/1
    ip address .....
    no shutdown
    end

K12:
setip 202.0.0.66 255.255.255.224 202.0.0.65
K21:
setip 202.0.0.98 255.255.255.240 202.0.0.97

#### Konfiguracja OSPF na routerach:

Na każdym routerze:
router ospf (process id)
network (adres sieci) (wildcard mask) area (area id)

gdzie:
process id - nie ma znaczenia, nie przekazują między sobą
wildcard mask - odwrotność maski podsieci
area id - musi być to samo dla całego obszaru, u nas wszystkie routery są w tym samym obszarze
Adres sieci i wildcard są wszędzie takie same, więc na każdym wpisujemy identycznie:

router ospf 14 (na przykład, podobno nie ma znaczenia)
network 202.0.0.64 0.0.0.63 area 6 (WSZĘDZIE MUSI BYĆ TO SAMO)
