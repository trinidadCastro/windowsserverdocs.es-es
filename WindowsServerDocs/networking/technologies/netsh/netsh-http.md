---
title: Comandos Netsh para Hypertext Transfer protocolo (HTTP)
description: Usar netsh http para consultar y configurar parámetros y configuración de HTTP.sys.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daecc6a385d7aa2c02d1bea02eb0a585a9b33185
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881796"
---
# <a name="netsh-http-commands"></a>Comandos http Netsh


Use **netsh http** para consultar y configurar las opciones de HTTP.sys y los parámetros.  

>[!TIP]
>Si usas Windows PowerShell en un equipo que ejecuta Windows Server 2016 o Windows 10, escriba **netsh** y presione ENTRAR. En el símbolo del sistema netsh, escriba **http** y presione ENTRAR para abrir el símbolo del sistema de netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Los comandos de netsh disponible http son:

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [Agregue el tiempo de espera](#add-timeout)
- [Agregar urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [eliminar el tiempo de espera](#delete-timeout)
- [delete urlacl](#delete-urlacl)
- [Vaciar logbuffer](#flush-logbuffer)
- [show cachestate](#show-cachestate)
- [Mostrar iplisten](#show-iplisten)
- [Mostrar servicestate](#show-servicestate)
- [Mostrar sslcert](#show-sslcert)
- [Mostrar tiempo de espera](#show-timeout)
- [Mostrar urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Agregar iplisten

Agrega una nueva dirección IP a la lista de escucha IP, sin incluir el número de puerto.

**Sintaxis**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parámetros**
| | | |
|---|---|---|
| **ipaddress** | Lista de escucha de la dirección IPv4 o IPv6 que se agregarán a la dirección IP. La lista de escucha IP se usa para definir el ámbito de la lista de direcciones al que está enlazado el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6. | Requerido |
---

**Ejemplos**

Estos son algunos ejemplos de cuatro de los **agregar iplisten** comando.

-   add iplisten ipaddress=fe80::1
-   Agregar iplisten ipaddress = 1.1.1.1
-   Agregar iplisten ipaddress = 0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>Agregar sslcert

Agrega un nuevo certificado de servidor SSL de enlace y el correspondiente directivas de certificados de cliente para una dirección IP y puerto.

**Sintaxis**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parámetros**

| | | |
|---|---|---|
| **ipport**                                   | Especifica la dirección IP y puerto para el enlace. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto.                                              | Requerido |
| **certhash**                                 | Especifica el hash SHA del certificado. Este hash tiene una longitud de 20 bytes y se especifica como una cadena hexadecimal.                                                                          | Requerido |
| **appid**                                    | Especifica el GUID para identificar la aplicación propietaria.                                                                                                                                   | Requerido |
| **certstorename**                            | Especifica el nombre del almacén del certificado. El valor predeterminado es MY. Certificado debe almacenarse en el contexto del equipo local.                                                                   | Opcional |
| **verifyclientcertrevocation**               | Especifica la activa o desactiva la comprobación de revocación de certificados de cliente.                                                                                                            | Opcional |
| **verifyrevocationwithcachedclientcertonly** | Especifica si el uso del certificado de cliente almacenada en caché solo para la comprobación de revocación está habilitado o deshabilitado.                                                                            | Opcional |
| **usagecheck**                               | Especifica si la comprobación de uso está habilitada o deshabilitada. Valor predeterminado está habilitado.                                                                                                            | Opcional |
| **revocationfreshnesstime**                  | Especifica el intervalo de tiempo en segundos, para que busque una lista de revocación actualizada de certificados (CRL). Si este valor es cero, la nueva CRL se actualiza sólo si expira la anterior. | Opcional |
| **urlretrievaltimeout**                      | Especifica el intervalo de tiempo de espera (en milisegundos) después de intentar recuperar la lista de revocación de certificados para la dirección URL remota.                                                       | Opcional |
| **sslctlidentifier**                         | Especifica la lista de los emisores de certificados que se puede confiar. Esta lista puede ser un subconjunto de los emisores de certificados que son de confianza del equipo.                                | Opcional |
| **sslctlstorename**                          | Especifica el nombre de almacén de certificados en equipo_local donde se almacena SslCtlIdentifier.                                                                                               | Opcional |
| **dsmapperusage**                            | Especifica si los asignadores de DS está habilitado o deshabilitado. Valor predeterminado es disabled.                                                                                                                | Opcional |
| **clientcertnegotiation**                    | Especifica si la negociación de certificado está habilitada o deshabilitada. Valor predeterminado es disabled.                                                                                            | Opcional |
---

**Ejemplos**

Este es un ejemplo de la **agregar sslcert** comando.

Agregar sslcert ipport = 1.1.1.1:443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>Agregue el tiempo de espera

Agrega un tiempo de espera global para el servicio.

**Sintaxis** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parámetros**
| | |
|---|---|
| **timeouttype** | Tipo de tiempo de espera para la configuración.                                                                        |
| **value**       | Valor de tiempo de espera (en segundos). Si el valor está en notación hexadecimal, a continuación, agregue el prefijo 0 x. |
---

**Ejemplos**

Estos son dos ejemplos de la **agregar tiempo de espera** comando.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>Agregar urlacl

Agrega una entrada de reserva del localizador uniforme de recursos (URL). Este comando reserva la URL para los usuarios sin privilegios de administrador y cuentas. La DACL se puede especificar mediante un nombre de cuenta de NT con los parámetros de escucha y el delegado o mediante una cadena SDDL.

**Sintaxis**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parámetros**
| | | |
|---|---|---|
| **url**       | Especifica el localizador uniforme de recursos (URL) completo.                                                                                    | Requerido |
| **user**      | Especifica el nombre de usuario o grupo de usuarios                                                                                                            | Requerido |
| **listen**    | Especifica uno de los siguientes valores: Sí: Permite al usuario registrar las direcciones URL. Este es el valor predeterminado. No: Denegar al usuario al registrar las direcciones URL. | Opcional |
| **delegate**  | Especifica uno de los siguientes valores: Sí: Permitir al usuario delegar no direcciones URL: Denegar al usuario de la delegación de las direcciones URL. Este es el valor predeterminado.   | Opcional |
| **sddl**      | Especifica una cadena SDDL que describe la DACL.                                                                                                | Opcional |
---

**Ejemplos**

Estos son algunos ejemplos de cuatro de los **agregar urlacl** comando.

-   Agregar urlacl url =https://+:80/MyUri usuario = DOMAIN\\usuario
-   Agregar urlacl url =https://www.contoso.com:80/MyUri usuario = DOMAIN\\usuario escucha = yes
-   Agregar urlacl url =https://www.contoso.com:80/MyUri usuario = DOMAIN\\delegado de usuario = no
-   Agregar urlacl url =https://+:80/MyUri sddl =...

---

## <a name="delete-cache"></a>Eliminar caché

Elimina todas las entradas, o una entrada especificada, el kernel de servicio caché de URI de HTTP.

**Sintaxis**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parámetros**
| | | |
|---|---|---|
| **url**       | Especifica el nombre completo localizador de recursos uniforme (URL) que desea eliminar.                                        | Opcional |
| **recursive** | Especifica si se quitan todas las entradas en la memoria caché la dirección url. **Sí**: quitar todas las entradas **ningún**: no quite todas las entradas | Opcional |
---

**Ejemplos**

Estos son dos ejemplos de la **eliminar caché** comando.

-   eliminar la dirección url de la memoria caché =https://www.contoso.com:80/myresource/ recursiva = yes
-   Eliminar caché

---

## <a name="delete-iplisten"></a>eliminar iplisten

Elimina una dirección IP de la lista de escucha IP. La lista de escucha IP se usa para definir el ámbito de la lista de direcciones al que está enlazado el servicio HTTP.

**Sintaxis**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parámetros**
| | | |
|---|---|---|
| **ipaddress** | Lista de escucha de la dirección IPv4 o IPv6 que se eliminará de la dirección IP. La lista de escucha IP se usa para definir el ámbito de la lista de direcciones al que está enlazado el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6. Esto no incluye el número de puerto. | Requerido |
---


**Ejemplos**

Estos son algunos ejemplos de cuatro de los **eliminar iplisten** comando.

-   delete iplisten ipaddress=fe80::1
-   eliminar iplisten ipaddress = 1.1.1.1
-   eliminar iplisten ipaddress = 0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>eliminar sslcert


Elimina enlaces de certificado de servidor SSL y directivas de certificados de cliente correspondiente para una dirección IP y puerto.

**Sintaxis**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parámetros**
| | | |
|---|---|---|
| **ipport** | Especifica la dirección IPv4 o IPv6 y el puerto para el que se eliminan los enlaces de certificados SSL. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto. | Requerido |
---


**Ejemplos**

Estos son algunos ejemplos de tres de los **eliminar sslcert** comando.

- eliminar sslcert ipport = 1.1.1.1:443
- eliminar sslcert ipport = 0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>eliminar el tiempo de espera

Elimina un tiempo de espera global y hace que el servicio de revertir a los valores predeterminados.

**Sintaxis**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parámetros**
| | | |
|---|---|---|
| **timeouttype** | Especifica el tipo de configuración de tiempo de espera. | Requerido |
---


**Ejemplos**

Estos son dos ejemplos de la **eliminar el tiempo de espera** comando.

-   delete timeout timeouttype=idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>delete urlacl

Elimina las reservas de direcciones URL.

**Sintaxis**

```powershell
delete urlacl [ url= ] URL
```

**Parámetros**
| | | |
|---|---|---|
| **url** | Especifica el nombre completo localizador de recursos uniforme (URL) que desea eliminar. | Requerido |
---


**Ejemplos**

Estos son dos ejemplos de la **delete urlacl** comando.

-   delete urlacl url=https://+:80/MyUri
-   delete urlacl url=https://www.contoso.com:80/MyUri

---

## <a name="flush-logbuffer"></a>Vaciar logbuffer

Vacía los búferes internos de los archivos de registro.

**Sintaxis**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Mostrar cachestate

Almacenar en caché las listas de recursos URI y sus propiedades asociadas. Este comando enumera todos los recursos y sus propiedades asociadas que se almacenan en caché en memoria caché de respuesta HTTP o muestran un único recurso y sus propiedades asociadas.

**Sintaxis**

```powershell
show cachestate [ [url= ] URL]
```

**Parámetros**
| | | |
|---|---|---|
| **url** | Especifica la dirección URL completa que se desea mostrar. Si no se especifica, se muestran todas las direcciones URL. La dirección URL también podría ser un prefijo para direcciones URL registradas. | Opcional |
---


**Ejemplos**

Estos son dos ejemplos de la **mostrar cachestate** comando:

-   Mostrar url cachestate =https://www.contoso.com:80/myresource
-   Mostrar cachestate

---

## <a name="show-iplisten"></a>Mostrar iplisten

Muestra todas las direcciones IP en la lista de escucha IP. La lista de escucha IP se usa para definir el ámbito de la lista de direcciones al que está enlazado el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6.

**Sintaxis**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Mostrar servicestate

Muestra una instantánea del servicio HTTP.

**Sintaxis**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parámetros**
| | | |
|---|---|---|
| **Vista**    | Especifica si desea ver una instantánea del estado de servicio HTTP basado en la sesión del servidor o en las colas de solicitud. | Opcional |
| **Verbose** | Especifica si se muestra información detallada que también se muestra información de la propiedad.                               | Opcional |
---

**Ejemplos**

Estos son dos ejemplos de la **mostrar servicestate** comando.

-   Mostrar servicestate view = "sesión"
-   Mostrar servicestate view = "requestq"

---

## <a name="show-sslcert"></a>Mostrar sslcert

Muestra los enlaces de certificados de servidor de capa de Sockets seguros (SSL) y directivas de certificados de cliente correspondiente para una dirección IP y puerto.

**Sintaxis**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parámetros**
| | | |
|---|---|---|
| **ipport** | Especifica la dirección IPv4 o IPv6 y el puerto que mostrar los enlaces de certificado SSL. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto. Si no especifica ipport, se muestran todos los enlaces. | Requerido |
---


**Ejemplos**

Estos son algunos ejemplos de cinco de los **mostrar sslcert** comando.

-   show sslcert ipport=[fe80::1]:443
-   Mostrar sslcert ipport = 1.1.1.1:443
-   Mostrar sslcert ipport = 0.0.0.0:443
-   show sslcert ipport=[::]:443
-   Mostrar sslcert

---

## <a name="show-timeout"></a>Mostrar tiempo de espera

Muestra, en segundos, el valor de tiempo de espera del servicio HTTP.

**Sintaxis**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>Mostrar urlacl

Control de acceso discrecional muestra listas (DACL) para la dirección URL reservada especificada o todas las direcciones URL reservadas.

**Sintaxis**

```powershell
show urlacl [ [url= ] URL]
```

**Parámetros**
| | | |
|---|---|---|
| **url** | Especifica la dirección URL completa que se desea mostrar. Si no especificada, se muestran todas las direcciones URL. | Opcional |
---


**Ejemplos**

Estos son algunos ejemplos de tres de los **mostrar urlacl** comando.

-   Mostrar urlacl url =https://+:80/MyUri
-   Mostrar urlacl url =https://www.contoso.com:80/MyUri
-   Mostrar urlacl

---