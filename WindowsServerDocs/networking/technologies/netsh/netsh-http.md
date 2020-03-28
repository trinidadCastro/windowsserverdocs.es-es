---
title: Comandos Netsh para el Protocolo de transferencia de hipertexto (HTTP)
description: Usa netsh http para consultar y configurar los parámetros y las opciones de HTTP.sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2c1032812f52eb32d9ae12310c8f2cff03f70aa5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309388"
---
# <a name="netsh-http-commands"></a>Comandos netsh http


Usa **netsh http** para consultar y configurar los parámetros y las opciones de HTTP.sys.  

>[!TIP]
>Si usas Windows PowerShell en un equipo que ejecuta Windows Server 2016 o Windows 10, escribe **netsh** y presiona Entrar. En el símbolo del sistema de netsh, escribe **http** y presiona Entrar para obtener el símbolo del sistema de netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Los comandos netsh http disponibles son:

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [add timeout](#add-timeout)
- [add urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [delete timeout](#delete-timeout)
- [delete urlacl](#delete-urlacl)
- [flush logbuffer](#flush-logbuffer)
- [show cachestate](#show-cachestate)
- [show iplisten](#show-iplisten)
- [show servicestate](#show-servicestate)
- [show sslcert](#show-sslcert)
- [show timeout](#show-timeout)
- [show urlacl](#show-urlacl)

## <a name="add-iplisten"></a>add iplisten

Agrega una nueva dirección IP a la lista de escucha de IP, sin incluir el número de puerto.

**Sintaxis**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parámetros**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Dirección IPv4 o IPv6 que se va a agregar a la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6. | Requerido |

---

**Ejemplos**

A continuación se muestran cuatro ejemplos del comando **add iplisten**.

-   add iplisten ipaddress=fe80::1
-   add iplisten ipaddress=1.1.1.1
-   add iplisten ipaddress=0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>add sslcert

Agrega un nuevo enlace de certificado de servidor SSL y las directivas de certificado de cliente correspondientes para una dirección IP y un puerto.

**Sintaxis**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parámetros**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Especifica la dirección IP y el puerto para el enlace. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto.                        | Requerido |
|                 **certhash**                 |                                     Especifica el hash SHA del certificado. Este hash tiene una longitud de 20 bytes y se especifica como una cadena hexadecimal.                                      | Requerido |
|                  **appid**                   |                                                                  Especifica el GUID para identificar la aplicación propietaria.                                                                  | Requerido |
|              **certstorename**               |                                  Especifica el nombre del almacén del certificado. El valor predeterminado es MY. El certificado debe almacenarse en el contexto del equipo local.                                  | Opcional |
|        **verifyclientcertrevocation**        |                                                      Especifica la activación o desactivación de la comprobación de la revocación de certificados de cliente.                                                       | Opcional |
| **verifyrevocationwithcachedclientcertonly** |                                      Especifica si el uso únicamente del certificado de cliente almacenado en memoria caché para la comprobación de la revocación está habilitado o deshabilitado.                                       | Opcional |
|                **usagecheck**                |                                                      Especifica si la comprobación del uso está habilitada o deshabilitada. Está habilitada de forma predeterminada.                                                       | Opcional |
|         **revocationfreshnesstime**          | Especifica el intervalo de tiempo, en segundos, para buscar una lista de revocación de certificados (CRL) actualizada. Si este valor es cero, la nueva CRL solo se actualizará si expira la anterior. | Opcional |
|           **urlretrievaltimeout**            |                            Especifica el intervalo de tiempo de espera (en milisegundos) después del intento de recuperar la lista de revocación de certificados para la dirección URL remota.                            | Opcional |
|             **sslctlidentifier**             |                Especifica la lista de emisores de certificados en los que se puede confiar. Esta lista puede ser un subconjunto de los emisores de certificados que son de confianza para el equipo.                 | Opcional |
|             **sslctlstorename**              |                                                Especifica el nombre del almacén de certificados en LOCAL_MACHINE donde se almacena SslCtlIdentifier.                                                | Opcional |
|              **dsmapperusage**               |                                                        Especifica si los asignadores de DS están habilitados o deshabilitados. De forma predeterminada, están deshabilitados.                                                         | Opcional |
|          **clientcertnegotiation**           |                                              Especifica si la negociación del certificado está habilitada o deshabilitada. De forma predeterminada, está deshabilitada.                                               | Opcional |

---

**Ejemplos**

El siguiente es un ejemplo del comando **add sslcert**.

add sslcert ipport=1.1.1.1:443 certhash=0102030405060708090A0B0C0D0E0F1011121314 appid={00112233-4455-6677-8899- AABBCCDDEEFF}

---

## <a name="add-timeout"></a>add timeout

Agrega un tiempo de espera global al servicio.

**Sintaxis** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parámetros**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo de tiempo de espera del valor de configuración.                                     |
|    **value**    | Valor del tiempo de espera (en segundos) Si el valor está en notación hexadecimal, agrega el prefijo 0x. |

---

**Ejemplos**

A continuación se muestran dos ejemplos del comando **add timeout**.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>add urlacl

Agrega una entrada de reserva de localizador uniforme de recursos (URL). Este comando reserva la dirección URL para usuarios y cuentas que no son administradores. La lista DACL puede especificarse mediante un nombre de cuenta NT con los parámetros listen y delegate o mediante una cadena SDDL.

**Sintaxis**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parámetros**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Especifica el localizador uniforme de recursos (URL) completo.                                           | Requerido |
|   **user**   |                                                      Especifica el nombre del usuario o el grupo de usuarios.                                                       | Requerido |
|  **listen**  | Especifica uno de los siguientes valores: yes, permite al usuario registrar direcciones URL. Este es el valor predeterminado. no: deniega al usuario el registro de direcciones URL. | Opcional |
| **delegate** |  Especifica uno de los siguientes valores: yes, permite al usuario delegar direcciones URL. no: deniega al usuario la delegación de direcciones URL. Este es el valor predeterminado.  | Opcional |
|   **sddl**   |                                                Especifica una cadena SDDL que describe la lista DACL.                                                 | Opcional |

---

**Ejemplos**

A continuación se muestran cuatro ejemplos del comando **add urlacl**.

- add urlacl url=https://+:80/MyUri user=DOMAIN\\ user
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user listen=yes
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user delegate=no
- add urlacl url=https://+:80/MyUri sddl=...

---

## <a name="delete-cache"></a>delete cache

Elimina todas las entradas, o una entrada especificada, de la memoria caché de URI del kernel del servicio HTTP.

**Sintaxis**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parámetros**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Especifica el localizador uniforme de recursos (URL) completo que quieres eliminar.                     | Opcional |
| **recursive** | Especifica si se quitan todas las entradas de la caché de direcciones URL. **yes**: quita todas las entradas. **no**: no quita todas las entradas. | Opcional |

---

**Ejemplos**

A continuación se muestran dos ejemplos del comando **delete cache**.

- delete cache url=<https://www.contoso.com:80/myresource/> recursive=yes
- delete cache

---

## <a name="delete-iplisten"></a>delete iplisten

Elimina una dirección IP de la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP.

**Sintaxis**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parámetros**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | Dirección IPv4 o IPv6 que se va a eliminar de la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6. No incluye el número de puerto. | Requerido |

---


**Ejemplos**

A continuación se muestran cuatro ejemplos del comando **delete iplisten**.

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>delete sslcert


Elimina los enlaces de certificado de servidor SSL y las directivas de certificado de cliente correspondientes para una dirección IP y un puerto.

**Sintaxis**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parámetros**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica la dirección IPv4 o IPv6 y el puerto para los que se eliminan los enlaces de certificado SSL. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto. | Requerido |

---


**Ejemplos**

A continuación se muestran tres ejemplos del comando **delete sslcert**.

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>delete timeout

Elimina un tiempo de espera global y hace que el servicio vuelva a los valores predeterminados.

**Sintaxis**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parámetros**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Especifica el tipo de opción de tiempo de espera. | Requerido |

---


**Ejemplos**

A continuación se muestran dos ejemplos del comando **delete timeout**.

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

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Especifica el localizador uniforme de recursos (URL) completo que quieres eliminar. | Requerido |

---


**Ejemplos**

A continuación se muestran dos ejemplos del comando **delete urlacl**.

- delete urlacl url=https://+:80/MyUri
- delete urlacl url=<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>flush logbuffer

Vacía los búferes internos de los archivos de registro.

**Sintaxis**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>show cachestate

Enumera los recursos de URI en caché y sus propiedades asociadas. Este comando muestra todos los recursos y sus propiedades asociadas que se han almacenados en la caché de respuesta HTTP o muestra un único recurso y sus propiedades asociadas.

**Sintaxis**

```powershell
show cachestate [ [url= ] URL]
```

**Parámetros**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica la dirección URL completa que quieres mostrar. Si no se especifica, se muestran todas las direcciones URL. La dirección URL también puede ser un prefijo para las direcciones URL registradas. | Opcional |

---


**Ejemplos**

A continuación se muestran dos ejemplos del comando **show cachestate**:

- show cachestate url=<https://www.contoso.com:80/myresource>
- show cachestate

---

## <a name="show-iplisten"></a>show iplisten

Muestra todas las direcciones IP en la lista de escucha IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6.

**Sintaxis**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>show servicestate

Muestra una instantánea del servicio HTTP.

**Sintaxis**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parámetros**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Ver**   | Especifica si se va a ver una instantánea del estado del servicio HTTP en función de la sesión del servidor o de las colas de solicitudes. | Opcional |
| **Verbose** |                Especifica si se va a mostrar información detallada con información sobre propiedades.                | Opcional |

---

**Ejemplos**

A continuación se muestran dos ejemplos del comando **show servicestate**.

-   show servicestate view="session"
-   show servicestate view="requestq"

---

## <a name="show-sslcert"></a>show sslcert

Muestra enlaces de certificado de servidor de Capa de sockets seguros (SSL) y las directivas de certificado de cliente correspondientes para una dirección IP y un puerto.

**Sintaxis**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parámetros**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica la dirección IPv4 o IPv6 y el puerto para los que se muestran los enlaces de certificado SSL. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto. Si no especificas ipport, se muestran todos los enlaces. | Requerido |

---


**Ejemplos**

A continuación se muestran cinco ejemplos del comando **show sslcert**.

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   show sslcert ipport=0.0.0.0:443
-   show sslcert ipport=[::]:443
-   show sslcert

---

## <a name="show-timeout"></a>show timeout

Muestra los valores de tiempo de espera en segundos del servicio HTTP.

**Sintaxis**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>show urlacl

Muestra las listas de control de acceso discrecional (DACL) de la dirección URL reservada especificada o de todas las direcciones URL reservadas.

**Sintaxis**

```powershell
show urlacl [ [url= ] URL]
```

**Parámetros**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica la dirección URL completa que quieres mostrar. Si no se especifica, se muestran todas las direcciones URL. | Opcional |

---


**Ejemplos**

A continuación se muestran tres ejemplos del comando **show urlacl**.

- show urlacl url=https://+:80/MyUri
- show urlacl url=<https://www.contoso.com:80/MyUri>
- show urlacl

---