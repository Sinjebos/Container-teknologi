# Container-Teknologi-inlämning-2 #
- [Docker-compose](#Docker-compose)
- [Flask-app](#Flask-app)
- [Multi-Stage-Build](#Multi-Stage-Build)
- [Mysql-Persistent-Data](#Mysql-Persistent-Data)
- [Frågor](#Frågor)

## Docker-compose

```yaml
version: "3.8"          - Specificerar vilken version av Docker-compose
services:               - tillåter att köra tjänster under detta
 flask:                 - Namnet på första servicen, kan heta vad som
  container_name: flaskcontainer  - Ger containern namnet "flaskcontainer"
  build:                - Här specifieras att docker-compose ska börja bygga
    context: ./app      - Vilken fil docker-compose skall titta i för hitta sin Dockerfile
   dockerfile: Dockerfile.dev - pga att Docker filen inte heter bara "Dockerfile" så behövs det specifieras 
  ports:                - tillåter att öppna portar
   - "5000:5000"        - Gör så ens lokala lyssnar på port 5000 och containern lyssnar på 5000
  depends_on:           - Specifierar att db skall byggas först
   - db
  networks:             - Ger "flask" nätverket flask_app_net
    - flask_app_net
 db:                    - namn på andra servicenn
  container_name: dbcontainer   - Ger containern namnet "dbcontainr"
  image: postgres:latest        - hämtar senaste postgres image från dockerhub
  restart: always               - startar alltid om containern om den stannar
  environment:                  - variabler
   POSTGRES_DB: mydb            - db namn i postgres
   POSTGRES_PASSWORD: postgres  - lösenord till postgres
   POSTGRES_USER: postgres      - användarnamn till postgres
  volumes:  - länkar lokala files "postgres_data" till containern
   - postgres_data:/var/lib/postgresql/data/
  networks: - Ger "db" nätverket flask_app_net
   - flask_app_net
       
networks:  - nätverk
  flask_app_net:  - namnet på nätverket
      driver: bridge  - vilken bridge den skall ha

volumes:        - skapar lokal volume
 postgres_data: - namnet på lokala volume
```
# Del 2 praktiskt #

## Flask-app ##

Började med att skapa alla filer och sedan kopiera in allt som skulle med som kod till app.py och flask==2.0.1 till requirements.txt.
Skapade sedan Dockerfile efter vad som behövdes till att köra app.py koden

Körde sedan docker kommandon för att bygga imagen och starta containern
```python
Docker build -t sinjebos/flask-app .
Docker run -p 5000:5000 sinjebos/flask-app
```
Och besökte sedan localhost:5000 för att se "Hello World" som bilden nedan

![](img/5.png)

Efter allt funkade och fick upp "hello world" så gjorde jag
```python
docker push <image name>
```

![](img/1.png)

- [Back To Top](#Container-Teknologi-inlämning-2)

## Multi-Stage-Build

Här börja jag med att skapa Dockerfile och använda mig utav Node då jag jobbat med det som mest.


![](img/8.png)

Körde mer eller mindre samma kommandon som förra delen fast med annat image namn och andra portar för nginx

```python
Docker build .
Docker run -p 80:80 -v ${pwd}:/usr/share/nginx/html <image id>
```

När jag väl öppna localhost på port 80 så kunde jag ändra och live uppdatera innuti imagen med hjälp av volumes se bilderna nedan

![](img/6.png)



![](img/7.png)

## Mysql-Persistent-Data

jag börja med att skapa en docker-compose.yml och bygga den efter hur uppgiften lyder.

Viktigaste delen i det hela är att se hur volumes fungerar och hur det sparar allt i containern i det lokala docker filsystemet
![](img/3.png)

Som i bilden är jag även inne i containern redan och gått in i mysqls terminal med hjälp kommandon:
```python
docker-compose up -d
docker container ls
docker exec -it <container id> bash/sh
mysql -U myuser -pmypassword
```

Efter jag varit inne i containern och kört så skall jag stänga ner den och ta bort den existerande containern och det gör jag med och sedan starta en ny

```python
docker-compose down
docker-compose up -d
```

![](img/2.png)

Efter jag den har tagit bort den gamla och startat den nya containern så kan jag igen gå in i mysql och listar allt som finns i min db så finns "jack" kvar med hjälp av volumes

![](img/4.png)

# Del 3 Projekt



![](img/9.png)



![](img/10.png)



![](img/11.png)



![](img/12.png)


# Frågor

1. [x] Vad är Docker(plattformen)?


2. [x] Vad är en Docker Image och hur relaterar en sådan till Docker Containers?


3. [x] Vad innebär containerteknologi?


4. [x] Vad är Docker Registry?


5. [x] Hur lyder Docker Linux Kernels arbetsbeskrivning (vad den gör och hur det går till)?


6. [x] På vilket sätt kan Docker och Docker Containers jämföras med fartygstransporter?


7. [x] - Vad händer när vi har en Dockerfile och kör "docker build ."? Gå igenom build-processen