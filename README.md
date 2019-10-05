
![Logos](doc/source/images/LogoCampusParty.jpg)

# Workshop - Ensablemos un Chatbot para hacer pedidos de pizza

En este workshop haremos uso de las APIs de Watson de IBM, en particular las que nos habilitan a construir un Chatbot: **Watson Assistant**

Al terminar este workshop el participante comprenderá como hacer lo siguiente:

* Crear un dialogo usando Watson assistant
* Usar Assistant Slots para optimizar la vista de dialogos
* Usar una simple aplicación para validar el comportamiento del asistente

## Características del taller

* **Nivel de skill**: Cualquier nivel de skill

Se requiere conocimientos básicos de Git e IBM Cloud.

## Requisitos ##

* Cuenta en IBM Cloud - Crea tu IBMid desde este enlace https://ibm.biz/BdzJFA
* Cuenta en Github - Creat tu ususario desde este enlace https://github.com

Para todos los efectos del taller se debe usar la región _`Dallas (US-South)`_

## Introducción ##

Un asistente es un bot cognitivo que puede personalizar para adaptarlo a sus necesidades empresariales y desplegar en varios canales para ofrecer ayuda a los clientes donde y cuando la necesiten

Para nuestro ejemplo, crearemos un asistente que pueda tomar las órdenes de Pizzas para un restaurante. La arquitectura que utilizaremos para nuestro taller

!["Arquitecrura"](doc/source/images/architecture.png)

El flujo que se desarrolla es como sigue:

1. El cliente envia un mensaje a través de la Aplicación
2. La aplicación envia el mensaje al servicio de Watson Assistant, y se presenta en la pantalla de chat de nuestra aplicación Web
3. La API de Watson procesa el mensaje y genera la respuesta a enviar al cliente

Los componentes Empleados son:

* [IBM Watson Assistant](https://www.ibm.com/cloud/watson-assistant/)
* [Node.js](https://nodejs.org/)

## Paso a paso ##

### Preparación ###

Los siguientes pasos deben realizarse usando un usuarios con privilegios de administración

1. Descargar la CLI de IBMCloud siguiendo estas [instrucciones ](https://cloud.ibm.com/docs/cli?topic=cloud-cli-install-ibmcloud-cli)
2. Instalar NodeJs siguiendo las instrucciones de esta [página](https://nodejs.org/es/download/current/)
3. Validar que la CLI de IBM Cloud está operativa

```
$ ibmcloud -v
ibmcloud version 0.19.0+94101c85-2019-09-23T03:46:57+00:00
```

4. Validar que la CLI de NodeJs y NPM están operativas

```
$ node -v
v12.10.0
$ npm -v
6.10.3
```

### Instalación del Skill del asistente

1. Crea un servicio de Watson Assistant desde el dashboard de [IBM Cloud](https://cloud.ibm.com) con tu cuenta y colocale el siguiente nombre `wcsi-conversation-service`
2. Presiona el botón `Launch Watson Assistant` para acceder a la configuración de los servicios del asistente
3. Selecciona la pestaña `Skills`
4. Seleccionar `Create Skill`
5. Selecciona `Import Skill`
6. Selecciona carga desde archivo en esta dirección [`data/skill-WatsonPizzeria.json`](data/watson-pizzeria.json)
7. Asegurate que esté marcado la opción `Everything` y pulsa el botó `Import`

### Instalación Aplicación Web Linux ###

1. Crear directorio de trabajo (Por ejemplo ~/lab/)

```
$ cd ~
$ mkdir lab
```
2. Clonar el [repositorio](https://github.com/IBMInnovationLabUY/watson-assistant-slots-intro.git) para descargar los artefactos del taller

```
$ cd lab
$ git clone https://github.com/IBMInnovationLabUY/watson-assistant-slots-intro.git
```
3. Definir nombre único para la aplicación en el archivo [`manifest.yml`](manifest.yml).

``` yaml
---
declared-services:
  wcsi-conversation-service:
    label: conversation
    plan: free
applications:
- name: pizza-ya-campusprty
  command: npm start
  path: .
  memory: 256M
  instances: 1
  env:
    NPM_CONFIG_PRODUCTION: false
  buildpack: sdk-for-nodejs
```

4. configurar variables de entorno en `.env`

```yaml
# Replace the credentials here with your own.
# Rename this file to .env before running 'npm start'

WORKSPACE_ID=<put workspace id here>

ASSISTANT_IAM_APIKEY=<put assistant IAM apikey here>
ASSISTANT_URL=<put assistant url here>
```
El identificador del workspace lo obtienen de la siguiente forma:

!["Get Workspace ID"](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-assistant/assistantPostSkillGetID.gif)

Las credenciales de acceso al servicio de la siguiente forma:

!["Assistant Credentials"](https://raw.githubusercontent.com/IBM/pattern-utils/master/watson-assistant/watson_assistant_api_key.png)


4. Instalar paquete de BZIP2 para instalación de PhantomJs

```
$ sudo yum install bzip2
```

5. Instalar la app local usando NPM

```
$ npm install
```

### Ejecución Local ###

1. Levantamos nuestra Web app

```
$ npm start
```

2. Accedemos a la aplicación usando un browser a la siguiente dirección http://localhost:3000

3. Revisemos el comportamiento de nuestro asistente

### Subamos nuestra aplicación a la Nube ###

1. Iniciemos la CLI de IBM cloud

```
$ ibmcloud login
```

2. Definamos nuestro target a aplicaciones Cloud Foundry usando este comando

```
$ ibmcloud target --cf
```

3. Subamos nuestra app a IBM cloud

```
$ ibmcloud app push
```

4. A pedir Pizzas!

# Contactos #

Siguenos en twiiter [@IBMUruguay](https://twitter.com/IBMUruguay)
