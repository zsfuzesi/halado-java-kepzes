# 1. nap
## microservice
áhh, hagyjuk már, Magyarországon moduláris monolit
pár 100-1000 felhaszálóra 1 csapat teljesen antipattern
csak db-ből táblázatot szolgálunk ki, semmi értelme

ms design pattern ismerete nélkül csak gáz lesz az eredmény, inkább ne mááár

microservice.io - itt olvasható

database/ sevice, különben engedd el


kitekintő:
java26 a http kliens tud http3-at - handshake
keepalive
http3 nem tcp/ip kapcsolat
default van benne titkosítás, (https)


Spring modulithot használjunk. Jól definiált alkalmazás. Vicziának van a blogján  kis leírás.

Spring cloud - spring ms implementáció
van egy rakat cucc, ami elavult, zookeeper, 

### external configuration
12 factor
cloud esetén config szerverből szedi
konfigot resten keresztül átadja - git-ben tárolja, vagy hashicorp vault-ot tud
ha változik a konfig, értesítést küld, egy szimpi queue-n keresztül... na ekkor nyom az app restartot

na ez kubernetesben nem kell.... ezt viszi a kubernetes


# Édekességek
spring boot jdbc - immutable
az új jpa- kigenerálja java kóddá, lesz stacktrace, és native fordítás