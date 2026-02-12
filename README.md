# Parte 5.2

## Despliega la aplicación, siguiendo esta guía, desde el repositorio: g Estúdiala en profundidad y realiza los pasos necesarios para levantarla en tu entorno local (Docker + Gradle). Ten en cuenta que puedes tener las dos aplicaciones en el mismo contenedor (cambiando el nombre del WAR y el contexto) o en contenedores separados.

Lo que he hecho para poderla subir al contenedor wildfly es:
- Primero me he clonado el repositorio:
![alt text](assets/clonacion.png)
- Luego he generado los ficheros gradlew usando este comando:
![alt text](assets/wrapper.png)
- Despues hice el build con gradlew para poder generar el war:
![alt text](assets/build.png)
- Luego copie el war generado con el contenedor:
![alt text](assets/copiar.png)


## Adjunta una captura de docker ps mostrando el contenedor wildfly activo.

Evidencia del comando docker ps en funcionamiento:

![alt text](assets/docker-ps.png)

## Adjunta evidencia de despliegue en logs (docker logs -f wildfly).

Evidencia de los logs del contendor wildfly:

![alt text](assets/logs.png)

## Realiza llamadas a la app y adjunta evidencias y sus respuestas (navegador y curl).

Prueba desde el navegador:

![alt text](assets/navegador.png)

Prueba usando el comando curl:

![alt text](assets/image.png)

## Explica qué componentes/servicios intervienen en tu despliegue de P5.2 y qué papel tiene cada uno (como mínimo: contenedor Docker, WildFly, aplicación WAR,puertos 8080/9990 y endpoint REST). Ten en cuenta el punto c) sobre el servidor web frontal.

Intervienen estos componentes y servicios:
- Docker: Es el encargado de permitir la virtualización del servicio de wildfly, permitiendole usarse sin necesidad de instarlo en la máquina anfitrión.
- Gradle: Es el encargado de la generación del war.
- Wildfly: Es el encargado del servidor de aplicaciones por lo que será el que desarrolle el papel de alojador de nuestras aplicaciones.
- Curl: Se encarga de probar si los endpoint funcionan correctamente. Desarrolla el papel de testeador.

Puertos:
- 8080: Se usa para el servidor de apliciones ya que por aqui irá el trafico de las peticiones para este.
- 9990: Es el puerto que expone wildfly y permite gestionar el servidor ya que es el puerto para la consola de administración.


Endpoints REST: La aplicación expone el endpoint /crud-file/api/tasks con los metodos GET,POST,PUT y DELETE. 


Servidor web frontal:
Se usa nginx para el servidor frontal que será el encargado de redireccionar las peticiones que vengan desde fuera al servidor de aplicaciones. 

Evidencias:

![alt text](assets/docker-ps.png)

![alt text](assets/wrapper.png)

![alt text](assets/build.png)


![alt text](assets/image.png)


## Identifica dentro de WildFly (en el contenedor) el/los archivo(s) de configuración principal(es) que gobiernan el servidor y señala:

El archivo principal es el `standalone.xml` que esta en `/opt/jboss/wildfly/standalone/configuration/standalone.xml`. Este archivo controla todo el servidor: las conexiones a bases de datos, el nivel de logs, los puertos, etc.

Tambien hay otros archivos importantes en esa carpeta:
- `mgmt-users.properties` — usuarios de administracion
- `application-users.properties` — usuarios de aplicacion
- `logging.properties` — configuracion de logs del arranque

En el `build.gradle` la dependencia de Jakarta EE esta marcada como `compileOnly` lo que quiere decir que solo se usa para compilar pero no se mete dentro del WAR ya que WildFly ya incluye esas librerias.

Evidencia del listado de la carpeta de configuracion:

![configuracion](assets/configuracion.png)
