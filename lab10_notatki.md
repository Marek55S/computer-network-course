OSPF
wewnętrzny protokół routingu typu link-state,

jak jest zmiana w sieci to wysyła informację do wszystkich routerów w AS (autonomous system) o stanie łącza bezpośrednio połączonych z nim sieci,

używa floodingu do rozgłaszania informacji o stanie łączy,
Ma swoje IP

Każdy router ma swój numer sekwencyjny po to żeby wiadomo było który update jest nowszy

Żeby zwiększyć skalowalność OSPF dzieli sieć (system autonomiczny) na obszary

- Area 0 - backbone area (obszar szkieletowy) - wszystkie inne obszary muszą być z nim połączone

ASBR - autonomous system boundary router - router graniczny systemu autonomicznego - łączy różne systemy autonomiczne 