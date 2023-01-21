# Detector-de-sintomas-COVID-19
Repositorio con la programación de un detector de síntomas COVID, realizado en C++ para entorno .ino


Este repositorio detecta síntomas COVID con NodeRed, ESP32CAM, el sensor MAX30100, el sensor MLX90614 y MySQL.

Primero utilizaremos las bases de datos

Se empieza instalando mysql server con lo sisguientes comando:

'- sudo apt update' '- sudo apt install mysql-server'

Se entra a myqsl

-sudo myqsl

Se cre una base de datos con el siguiente comando:

-CREATE DATABASE detectorsintomas;

Seleccionamos la base de datos de la siguiente manera:

-use detectorsintomas;

Se crea un tabla con los campos necesarios de acuerdo al ejercicio.

CREATE TABLE registro (id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY, fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP, nombre CHAR (248) NOT NULL, correo CHAR (248) NOT NULL, temp FLOAT(4,2) NOT NULL, bpm INT(1) UNSIGNED NOT NULL, sp02 INT(1) UNSIGNED NOT NULL, protodiagnostico CHAR (248) NOT NULL);
De acuerdo con ello, para la base de datos necesitamos lo siguiente: Para este ejercicio necesesitamos lo siguiente

Base de datos

ID
Fecha
Nombre
Correo
Temperatura
BPM
SPO2
Protodiagnostico
Un programa para leer el sensor max30100

Un flow

Nota: el programa que se utilizo fue el anterior código

Los siguientes comandos se utilizan para poder ver la base de datos y tablas, así como también hacer uso de ellla.

El siguiente comando consiste en mostrar la lista de bases de datos.

show databases;

El comando siguiente muestra la lista de las tablas en la base de datos que se ha selaccionado.

show tables;

Para los detalles de una tabla de la base de datos seleccionada.

describe [nombre de tabla];

EL siguiente programa es para leer los sensores y enviar por MQTT:

https://github.com/codigo-iot/publicar-strings-json-mqtt-nodemcu-wifi/blob/main/nodemcu-mqtt-json/nodemcu-mqtt-json.ino

Tema MQTT pub: coidoIoT/detectorSintomas/flow

Tema MQTT sub: coidgoIoT/detectorSintomas/esp

Crearemos un flow para mostarar los sensores del detector de COVID.

Aseguraremos que la base de datos esté creada correctamente.

Agregar el código del sensor MLX90614 al programa del micro controlador

-Agregar biblioteca

-Agregar objeto

-Iniciar el sensor en void setup.

-Modificar el inicio del sensor para que tome los pines indicados manualmente .

-Agregar una nueva secuencia de tiempo no bloqueante.

-Modificar el envío por JSON.

Notas

Abrir el firwall de Ubuntu para Mosquitto

sudo ufw allow 1883
sudo ufw enable
Nodo Function Detector HR

if (msg.payload.hrv == 1) {
msg.payload = msg.payload.hr;


return msg;


}
Nodo Funcition Detector SPO2

if (msg.payload.spo2v == 1) {
msg.payload = msg.payload.spo2;


return msg;
}
Nodo Funcition

msg.payload = msg.payload.tir;


return msg;
Detector de sintomas COVID

Se realiza un boton diagnóstico.

Nodo function Protodiagnostico

if ((global.get ("temp") > 35.5 && global.get ("temp") < 36.5) && (global.get ("hr") > 60 && global.get ("hr") <90 ) && (global.get ("spo2") > 90))

{

msg.payload = "Signos vitales correctos";


global.set ("protodiagnostico", msg.payload);


msg.to = global.get ("correo");


msg.topic = "Proto diagnostico de covid - Ejercicio de CódigoIoT";


return msg;
}

else {

msg.payload = "Signos vitales incorrectos, ve al médico";


global.set ("protodiagnostico", msg.payload);
msg.to = global.get ("correo");


msg.topic = "Proto diagnostico de covid - Ejercicio de CódigoIoT";


return msg;


}
Para agregar nodos mysql:

-Entrar a la pestaña install del menú Mnage Palette -node-red-node-mysql

Posteriormente crea un nuevo usuario de Mysql

CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON . TO 'Teresa'@'localhost';
Agregar nodos MySQL

msg.topic = "INSERT INTO registro (nombre,correo,temp,bpm,sp02,protodiagnostico) VALUES ('" + global.get ("paciente") + "','" + global.get ("correo") + "'," + global.get ("temp") + "," + global.get ("hr") + "," + global.get ("spo2") + ",'" + global.get("protodiagnostico") + "');"; return msg;

Ejemplo de como quedaria si fuera una instrucción estática

Nodo Function Query

msg.topic = "INSERT INTO registro (nombre,correo,temp,bpm,sp02,protodiagnostico) VALUES ('Hugo Vargas','victor4salvador@outlook.es', 37.2,102,95,'Protodiagnostico');"; return msg;

Finalmente Agregar un enviador de correo

Instalar los nodos node-red-node-email
