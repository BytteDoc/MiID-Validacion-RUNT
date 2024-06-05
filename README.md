# MiID Validación RUNT

Este servicio será expuesto para la validación del rostro frente a los registros del sistema Runt:

## Operaciones
    
* Login User.
* Validación Runt .


## Api Login User

Permite la Autenticación/Autorización del cliente en el sistema MiID

Url - Login User - POST https://servicesdev.miid.bio/Login/LoginUser

```json
{
  "emailAddress": "corre@prueba.com",
  "password": "pass.test"
}
```

**Variables a enviar:**

*  emailAddress : Es el correo que se le genera al cliente por parte de MiID. Este dato es entregado por MiID.
*  password : Es la Clave asignada previamente al cliente por parte de MiID. Este dato es entregado por MiID.

Si la información ingresada es correcta, el Api Retorna

```json
{
    "tokenType": "Bearer",
    "expiresIn": 3600,
    "extExpiresIn": 0,
    "accessToken": "eyJhbG....",
    "successOperation": true
}
```

**Variables de respuesta:**

* **tokenType** : Bearer
* **expiresIn** : Tiempo de Vigencia del Token en segundos
* **accessToken** : Token que será usado posteriormente para llamado del api *Validacion Runt*
* **successOperation** : Variable donde se indica si fue exitoso o no la autenticación

Si la información ingresada No es correcta, el Api Retorna

* status : 200

```json
{
    "expiresIn": 0,
    "extExpiresIn": 0,
    "successOperation": false,
    "message": "BadRequest Bad Request"
}
```

**Variables de respuesta:**

* **expiresIn** : Tiempo de Vigencia del Token en segundos
* **extExpiresIn** : Tiempo de Vigencia del Token en días
* **successOperation** : Variable donde se indica si fue exitoso o no la autenticación
* **message** : Mensaje del error

## Api Validación Runt

Url - Validación Runt - POST

https://servicesdev.miid.bio/Person/ValidacionRUNT

Cabeceras a enviar

* Se debe enviar el header Authorization, con el campo accessToken obtenido en el servicio Login User

<pre> <code>--header Authorization": Bearer eyJ0eXAi......."</code></pre>

**Tipo de cuerpo**

* Form/Data

**Variables Form a enviar**:

* **face** : Archivo binario con el insumo de la captura de rostro del usuario a validar.
* **documentnumber** :  Número de documento del usuario a validar.
* **documenttype** :  Tipo de documento del usuario a validar.
* **EnterpriseClientId** : Id del cliente creado en MiID. Dato entregado por MiID.

Si la información ingresada es correcta, el Api retorna

* status : 200

```json
{
    "descripcionRespuesta": "MATCH",
    "fechaTransaccion": "2022-08-17T13:44:20.929+0000",
    "primerNombre": "CARLOS",
    "segundoNombre": "ADOLFO",
    "primerApellido": "SIABATO",
    "segundoApellido": "COTAMO"
}
```

Si la información ingresada es correcta pero la imagen no es del usuario, el Api retorna

* status : 200

```json
{
    "descripcionRespuesta": "NO MATCH",
    "fechaTransaccion": "2022-08-17T13:44:20.929+0000",
    "primerNombre": "CARLOS",
    "segundoNombre": "ADOLFO",
    "primerApellido": "SIABATO",
    "segundoApellido": "COTAMO"
}
```

**Variables de respuesta:**

* **descripcionRespuesta** : Mensaje de respuesta del consumo del servicio
* **fechaTransaccion** : Fecha de transacción
* **primerNombre** : Primer nombre del usuario que se consulto
* **segundoNombre** : Segundo nombre del usuario que se consulto
* **primerApellido** : Primer apellido del usuario que se consulto
* **segundoApellido** : Segundo apellido del usuario que se consulto

Si la el token bearer es incorrecto o esta vencido.

* status : 201

Si la información ingresada es correcta, pero el cliente no existe en el RUNT el api retorna

* status : 200
```json
{
    "descripcionRespuesta": "El ciudadano no está registrado en el sistema RUNT",
    "fechaTransaccion": "2024-05-30T21:47:32.250+0000"
}
```