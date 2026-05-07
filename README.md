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

Kulcson belül sorrendtartó. Azonos kulcsú elemek ugyanabba a topicba mennek.

Hint:
egy microservice-nek legyen 3 topicja:
- input topic: ide jönnek a kérések, amiket a service feldolgoz
- output topic: ide kerülnek a válaszok, amiket a service a
- event topic: ide kerülnek a service által generált események, amikre más service-ek feliratkozhatnak

Kérdés-válasz összekapcsolása, headerben egy correlation id, amivel a válasz visszakereshető lesz.
Ez egy egyszerű header paraméter.

defaultból ha egy objektumot adunk át kaka-nak, nem lesz alapból json...
ehhez kell egy serializer, ami json-t csinál belőle, és egy deserializer, ami visszaalakítja a json-t objektummá.
kafka.producer.value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
a consumer dettó

ha a json lassú, akkor a bináris avro a jó, kafka default támogatja..

IDEA pliugin= confluent :D
Ha olvasni akarok, akkor egy consumert kell létrehozni

Microservice eseén kódmegosztás antipattern.. contract... azaz jsonschema alapján
közös kód interfész esetén nem szabad

Van egy biztonsági rés:
consumer.properties:
    spring.json.trusted.packages: 'employees'

# 2. nap
## Spring Boot Stream
EDA támogatás, magasabb absztrakciós szint, mint a sima kafka client
fejesztési elv - egyetlen egy metódus, amit ad, a többi létező, pl: funkcionális interfészek

https://spring.io/projects/spring-cloud-stream
itt található sok binder, pl a kafkás is

avro formátumra átállítjuk, nyelvfüggetlen, bináris

### resilience4j - circuit breaker


# Édekességek
spring boot jdbc - immutable
az új jpa- kigenerálja java kóddá, lesz stacktrace, és native fordítás

Configuration annotációnál -> (proxyBeanMethods = false)
gyorsabban indul a spring

contract first approach: json schema alapján generálunk kódot, ez elterjedt
schema registry - springben is van ilyen pl, egy leírót, contractot le tudjuk kérni

chaos monkey- lekapcsolja a hálózati elemeket, és monitorozni lehet, hogy mikor, mennyi idő alatt
áll helyre, önjavító

van ilyen springre is, chaos monkey for spring
post üzenetekkel be tudom kapcsolni a hibákat

spring boot admin - alkalmazásokat monitorozgat, actuatoron keresztül minent elér, konfigok elérhetők, spring cuccok, log level állítgatás

backend for frontend - van 100 microservice, és a web kliens.. na a kliensemet nem ersztem rá mind a 100 rendszerre, hanem kapnak egy külön backendet, na majd a beszélgessen a 100 servicemmel
na meg más kell a web-nek meg a mobilnak, ezért külön backend for frontend van
ezt meg a frontendes node-ban megírja
