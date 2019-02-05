Microservicios - _SpringBoot_
=============================
https://github.com/dperezg2017/in28minutes.com/blob/master/_posts/2017-10-16-spring-micro-services.md

### Actuator ##
###### Paso1: en el properties => management.endpoints.web.exposure.include=*
###### Paso2: pom.xml: 
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-rest-hal-browser</artifactId>
		</dependency>
###### Paso3: - Si se usa SprinbBoot 2.0.0 a (+) =>  POST: http://localhost:8080/actuator/refresh 
- Si se usa SprinbBoot 1.x a (+) =>  POST: http://localhost:8080/refresh ó http://localhost:8080/application/refresh
* Opcionales
Si en caso se muestra problema de authorizacion, agregar en el properties => management.security.enabled=false.

### Spring Cloud Bus ##
Se debe implementar con MQRabbit - 
###### Paso1: Iniciar MQRabbit (Revisar procedimiento lineas mas abajo)
###### Paso2: Agregar la dependecia en "Limits-Service" y "Spring-cloud-config-server"
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-bus-amqp</artifactId>
		</dependency>
###### Paso3: - Si se usa SprinbBoot 2.0.0 a (+) =>  POST: http://localhost:8080/actuator/bus-refresh
       - Si se usa SprinbBoot 1.x a (+) =>  POST: http://localhost:8080/bus/refresh
##### Opcionales
###### El Bus, esta estable en la version de SpringBoot => "2.0.2.RELEASE"
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.2.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<!-- for 2.1.0 <spring-cloud.version>Greenwich.M1</spring-cloud.version> -->
		<spring-cloud.version>Finchley.SR1</spring-cloud.version>
	</properties>
	
### Hystrix ###
###### Paso1: Agregar la dependencia: 
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
<!-- 			<artifactId>spring-cloud-starter-hystrix</artifactId> -->
		</dependency>
###### Paso2: Agregar la anotacion "@EnableHystrix" en la clase Main del servicio "limits-service"
###### Paso 3 Agregar en el Controller, una excepcion.
  @GetMapping("/fault-tolerance-example")
  @HystrixCommand(fallbackMethod="fallbackRetrieveConfiguration")
  public LimitConfiguration retrieveConfiguration() {
    throw new RuntimeException("Not available");
  }

  public LimitConfiguration fallbackRetrieveConfiguration() {
    return new LimitConfiguration(999, 9);
  }
######  Explicacion: Si no se agrega la linea:  @HystrixCommand(fallbackMethod="fallbackRetrieveConfiguration"), se mostrara la excepcion, pero le estamos diciendo, que si ocurre excepcion, vaya al metodo X. para abstener a una excepcion en ejecucion.

_GIT HUB_ - deben estar en la ruta del proyecto
===============================================
###### Subir Proyecto de cero: 
- git init
- git add *
- git status 
- git commit -m 'Subo la estructura del proyecto al repositorio de GitHub'
- git remote add origin https://github.com/dperezg2017/Help_Dev.git
- git push -u origin master
###### Subir cambios de Proyecto : 
- git add *
- git status 
- git commit -m 'Subo la estructura del proyecto al repositorio de GitHub'
- git push
###### Actualizar Proyecto:
- git pull
###### Descargar proyecto de gitHub:
- git clone https://github.com/dperezg2017/Help_Dev.git


VM _Argumentos_ 
===============
-Dserver.port=8001 

RabbitMQ _Colas_ 
================
######Instalar RabbitMQ
######Instalar 1: http://www.erlang.org/downloads
######Instalar 2: https://www.rabbitmq.com/install-windows.html
######Video: https://www.youtube.com/watch?v=gKzKUmtOwR4


######paso1: https://zipkin.io/pages/quickstart.html  
######paso2: click ultima version para descargar "zipkin-server-2.12.0-exec.jar" ,varia la version
######paso3: abrir el CMD, ubicarte donde se encuentra el .Jar y escribir lo siguiente:
######set RABBIT_URI=amqp://localhost
######java -jar zipkin-server-2.12.0-exec.jar
######paso4: ingresar: http://localhost:9411/zipkin/   
######Recomendacion:
######Para ver que los servicios esten en el zipkin, debera agregar las dependencias:
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-zipkin</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.amqp</groupId>
			<artifactId>spring-rabbit</artifactId>
		</dependency>
#####Opcionales:
Para ver la interfaz: 
######Paso1: en el CMD, escribir => rabbitmq-plugins enable rabbitmq_management
######Paso2: ir a = > http://localhost:15672/#/queues
######Paso3: registrarse con usuario y clave => guest

### Rabbit Simulador  - http://tryrabbitmq.com/?queue_id=cola2&queue_name=cola3

Se realizaron las pruebas para ver la diferencia de Exchange: topic,fanout,direct 
#####direct: tienes que enviarle exactamente como se llame el "routing key" para que pueda pasar por dicha cola. 
	example: Binding1 { "routing key": orange }    Binding2 { "routing key": orange.big }
		 Producer {"routing key": orange }
#####fanout: ovbia el nombre de los "routing key". igual se lleva enviar los mensaje a los consumers
	example: Binding1 { "routing key": orange }    Binding2 { "routing key": orange.big }
		 Producer {"routing key":  }
#####topic: tiene el #: remplasa 0 o letras, *:remplaza letras, en el ejemplo pasa por los dos binding: une el exchange con la cola. es lo diferente al topic.
	example: Binding1 { "routing key": orange.* }    Binding2 { "routing key": orange.big }
		 Producer {"routing key": orange.big }		 

Angular _version6_ 
==================
http://www.tic2.org/WebTecnica/Programas/SOperativos/Linux/Comandos/LinuxComandosEquivalencias.htm

- Instalar NodeJS
- npm install -g @angular/cli
- ng new my-dream-app
- cd my-dream-app
- ng serve

##### Si te falta librerias: npm update   ó  (dependiendo)   npm install --save-dev @angular-devkit/build-angular
## Proyecto
######Para crear nuevo proyecto :  "ng new SPA" y levantarlo => "ng serve -o" en el CMD
######Para generar nuevo componentes : "ng g c navbar"  ,donde g:generate y c:component
######Pa agregar Boostrap: "npm install bootstrap --save" , "npm install jquery --save" y "npm install popper,js --save"
######Modificar file: "angular.json"
            "styles": [
              "src/styles.css",
              "node_modules/bootstrap/dist/css/bootstrap.min.css"
            ],
            "scripts": [
              "node_modules/jquery/dist/jquery.slim.min.js",
              "node_modules/popper.js/dist/umd/popper.min.js",
              "node_modules/bootstrap/dist/js/bootstrap.min.js"
            ]
######Para no agrega Styles en mi componente: "ng g c heroes -is" se agrega la palabra "-is"


_Docker_
========
https://onedrive.live.com/?authkey=%21ANAqKS_syP3u2Os&id=2F5823B4594339C3%2116706&cid=2F5823B4594339C3 - virtual ubuntu con docker





