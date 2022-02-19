# java-docker
Java single and multistage dockerized app with my mate <a href="https://github.com/nicolaujoan/">Joan Nicolau</a>

### Multi stage - OpenJDK JRE
<hr>

Crear imagen con el JDK y JRE para empaquetar la app con los jar de la app:

Dockerfile

``` Dockerfile 
FROM maven:3.8.4-openjdk-11-slim AS build

WORKDIR /home/app

COPY src src
COPY pom.xml pom.xml
RUN mvn -f /home/app/pom.xml clean package -q

FROM openjdk:11.0-jre-slim-buster

LABEL "edu.poniperro.gilded-dockerized"="gilded-dockerized"
LABEL version=1.0-SNAPSHOT
LABEL mantainer="jnicolausantandreu@cifpfbmoll.eu"

COPY --from=build /home/app/target/gilded-dockerized-1.0-SNAPSHOT.jar /usr/local/lib/gilded-dockerized.jar
CMD ["java","-jar","/usr/local/lib/gilded-dockerized.jar"]
```

Creación de la imagen

``` bash
$ docker build -t gildedrose


$ docker images

REPOSITORY    TAG                     IMAGE ID       CREATED          SIZE
gildedrose    latest                  e99023abd58e   13 minutes ago   221MB
openjdk       11.0-jre-slim-buster    706a0cb06cd5   7 days ago       221MB

```

Crear contenedor y ejecutarlo a partir de la imagen descargada

```
bash
$ docker run -it --name gildedrose gildedrose:latest

--- Bienvenido a Gilded Rose ---

## Actualizando items... ##



### day 1 ###

+5 Dexterity Vest, 9, 19
Aged Brie, 1, 1
Elixir of the Mongoose, 4, 6
Sulfuras, Hand of Ragnaros, 0, 80
Sulfuras, Hand of Ragnaros, -1, 80
Backstage passes to a TAFKAL80ETC concert, 14, 21
Backstage passes to a TAFKAL80ETC concert, 9, 50
Backstage passes to a TAFKAL80ETC concert, 4, 50
Conjured Mana Cake, 2, 5

### day 2 ###

+5 Dexterity Vest, 8, 18
Aged Brie, 0, 2
Elixir of the Mongoose, 3, 5
Sulfuras, Hand of Ragnaros, 0, 80
Sulfuras, Hand of Ragnaros, -1, 80
Backstage passes to a TAFKAL80ETC concert, 13, 22
Backstage passes to a TAFKAL80ETC concert, 8, 51
Backstage passes to a TAFKAL80ETC concert, 3, 51
Conjured Mana Cake, 1, 4

### day 3 ###

+5 Dexterity Vest, 7, 17
Aged Brie, -1, 4
Elixir of the Mongoose, 2, 4
Sulfuras, Hand of Ragnaros, 0, 80
Sulfuras, Hand of Ragnaros, -1, 80
Backstage passes to a TAFKAL80ETC concert, 12, 23
Backstage passes to a TAFKAL80ETC concert, 7, 52
Backstage passes to a TAFKAL80ETC concert, 2, 52
Conjured Mana Cake, 0, 3
...

$ docker ps -a 

CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS                      PORTS     NAMES
300f3154a6a0   gildedrose:latest   "java -jar /usr/loca…"   13 seconds ago   Exited (0) 12 seconds ago             gildedrose
```
