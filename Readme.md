## Container-Teknologi inlämning 2 ##


```yaml
version: "3.8"
services: 
 flask:
  container_name: flaskcontainer
  build:
   context: ./app
   dockerfile: Dockerfile.dev
  ports:
   - "5000:5000"
  depends_on: 
   - db
  networks:
    - flask_app_net
 db:
  container_name: dbcontainer
  image: postgres:latest
  restart: always
  environment: 
   POSTGRES_DB: mydb
   POSTGRES_PASSWORD: postgres
   POSTGRES_USER: postgres
  volumes:
   - postgres_data:/var/lib/postgresql/data/
  networks:
   - flask_app_net
       
networks:   
  flask_app_net:
      driver: bridge

volumes:
 postgres_data:
```






















1. [x] Vad är Docker(plattformen)?


2. [x] Vad är en Docker Image och hur relaterar en sådan till Docker Containers?


3. [x] Vad innebär containerteknologi?


4. [x] Vad är Docker Registry?


5. [x] Hur lyder Docker Linux Kernels arbetsbeskrivning (vad den gör och hur det går till)?


6. [x] På vilket sätt kan Docker och Docker Containers jämföras med fartygstransporter?


7. [x] - Vad händer när vi har en Dockerfile och kör "docker build ."? Gå igenom build-processen