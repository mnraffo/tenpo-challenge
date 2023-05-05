<div align="center">
  <h1>Tenpo Challenge</h1>
  TENPO JVM API Challenge for Team Lead Backend Engineer
</div>
  
## Intro
El challenge se resolvió con una arquitectura orientada a microservicios, Redis como cache y PostgreSQL como base de datos relacional. A nivel aplicativo, se utilizó SpringBoot con Web MVC y WebClient, Resilience4J y Lombok. Para los test utilizamos TestContainers y quedo pendiente utilizar la libreria Karate con Jacoco para el coverage. Como package manager se utilizó Maven.


## Clone
El challenge cuenta con tres repositorios Git:

- tenpo-challenge: https://github.com/mnraffo/tenpo-challenge
- ms-fee: https://github.com/mnraffo/ms-fee
- ms-sum: https://github.com/mnraffo/ms-sum

#### Proyecto tenpo-challenge
Este proyecto solo contiene la documentación y el ejecutable final del challenge.
```
git clone https://github.com/mnraffo/tenpo-challenge
```

#### Proyecto ms-fee
Este proyecto contiene el microservicio que sirve el fee.
```
git clone https://github.com/mnraffo/ms-fee
```

#### Proyecto ms-sum
Este proyecto contiene el microservicio que sirven los endpoints principales de la solución y quien realiza el logging aplicativo.
```
git clone https://github.com/mnraffo/ms-sum
```


# Test
Para ejecutar los test:

#### ms-fee
```
cd ms-fee/
./mvnw test
```

#### ms-sum
```
cd ms-sum/
./mvnw test
```


## Build

#### ms-fee
```
cd ms-fee/
./mvnw clean package
```

#### ms-sum
```
cd ms-sum/
./mvnw clean package
```


## Run
Para ejecutar la aplicación es necesario tener instalado Docker.
```
cd tenpo-challenge/
docker-compose up
```

#### Endpoints:
Con un explorador web, navegar los Swaggers disponibles con los endpoints de cada ms:

##### ms-fee
```
http://localhost:9090/ms-fee/swagger-ui/index.html
```

##### ms-sum
```
http://localhost:9091/ms-sum/swagger-ui/index.html
```

#### Ejemplos:

- Obtener fee:
```
curl http://localhost:9090/ms-fee/fee
```

- Obtener la suma de dos numeros:
```
curl http://localhost:9091/ms-sum/sum?first=5&second=5
```

- Obtener historial de peticiones:
```
curl http://localhost:9091/ms-sum/audit-logs
```

## Disclaimers

- El microservicio ms-fee quedó expuesto a localhost, pero lo ideal seria que sea una api interna / no expuesta en el Api Manager. A fines prácticos quedó disponible para su uso y prueba.

- Sobre el apartado del historial de llamados a todos los endpoints, hubiese diseñado una estrategia diferente de raíz. Si bien quise mantenerme dentro de la premisa del ejercicio, una solución diferente a esa problemática sería contar con un sistema que atienda los CCC (Cross Cutting Concerns). Se puede hacer streaming de logs con FluentBit hacia una instancia de ELK, OpenSearch, etc. También es posible contar con algún sistema de telemetría como Micrometer u OpenTelemetry. En este caso, como el endpoint externo (Fee) podia o no estar mockeado, y, los logs debian quedar en una base de datos relacional, se opto por la implementación de un filtro y un servicio asincrónico. En caso de agregar el logging del ms-fee, habria que extraer la funcionalidad de logging hacia un nuevo microservicio, ejemplo ms-audit, quien sea el encargado. de recibir y guardar en la base de datos. No estaría bien compartir esa base entre dos microservicios dado que violaríamos el paradigma de "arquitectura orientada a microservicios". Sin duda, este punto deja una charla por delante porque hay muchas formas de mejorar al estrategia de logging.
