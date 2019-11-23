---
title: Comandos Netsh para el protocolo de transferencia de hipertexto (HTTP)
description: Use netsh http para consultar y configurar los parámetros y la configuración de HTTP. sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401875"
---
# <a name="netsh-http-commands"></a>Comandos http Netsh


Use **netsh http** para consultar y configurar los parámetros y la configuración de http. sys.  

>[!TIP]
>Si usa Windows PowerShell en un equipo que ejecuta Windows Server 2016 o Windows 10, escriba **netsh** y presione Entrar. En el símbolo del sistema de Netsh, escriba **http** y presione Entrar para obtener el símbolo del sistema netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Los comandos Netsh http disponibles son:

- [Agregar iplisten](#add-iplisten)
- [Agregar sslcert](#add-sslcert)
- [agregar tiempo de espera](#add-timeout)
- [Agregar urlacl](#add-urlacl)
- [eliminar caché](#delete-cache)
- [eliminar iplisten](#delete-iplisten)
- [eliminar sslcert](#delete-sslcert)
- [tiempo de espera de eliminación](#delete-timeout)
- [eliminar urlacl](#delete-urlacl)
- [vaciar logbuffer](#flush-logbuffer)
- [Mostrar cachestate](#show-cachestate)
- [Mostrar iplisten](#show-iplisten)
- [Mostrar servicestate](#show-servicestate)
- [Mostrar sslcert](#show-sslcert)
- [Mostrar tiempo de espera](#show-timeout)
- [Mostrar urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Agregar iplisten

Agrega una nueva dirección IP a la lista de escucha de IP, sin incluir el número de puerto.

**Sintáctica**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Los**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **DirIP** | Dirección IPv4 o IPv6 que se va a agregar a la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6. | Requerido |

---

**Example**

A continuación se muestran cuatro ejemplos del comando **Add iplisten** .

-   Agregue iplisten IPAddress = fe80:: 1
-   Agregar iplisten IPAddress = pág
-   Agregar iplisten IPAddress = 0.0.0.0
-   Agregar iplisten ipaddress =::

---

## <a name="add-sslcert"></a>Agregar sslcert

Agrega un nuevo enlace de certificado de servidor SSL y las directivas de certificado de cliente correspondientes para una dirección IP y un puerto.

**Sintáctica**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Los**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Especifica la dirección IP y el puerto para el enlace. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto.                        | Requerido |
|                 **certhash**                 |                                     Especifica el hash SHA del certificado. Este hash tiene una longitud de 20 bytes y se especifica como una cadena hexadecimal.                                      | Requerido |
|                  **appid**                   |                                                                  Especifica el GUID para identificar la aplicación propietaria.                                                                  | Requerido |
|              **certstorename**               |                                  Especifica el nombre del almacén del certificado. EL valor predeterminado es MY. El certificado debe almacenarse en el contexto del equipo local.                                  | Opcional |
|        **verifyclientcertrevocation**        |                                                      Especifica la comprobación activa/desactivada de la revocación de certificados de cliente.                                                       | Opcional |
| **verifyrevocationwithcachedclientcertonly** |                                      Especifica si el uso del certificado de cliente almacenado en memoria caché para la comprobación de revocación está habilitado o deshabilitado.                                       | Opcional |
|                **usagecheck**                |                                                      Especifica si la comprobación de uso está habilitada o deshabilitada. El valor predeterminado es habilitado.                                                       | Opcional |
|         **revocationfreshnesstime**          | Especifica el intervalo de tiempo, en segundos, para buscar una lista de revocación de certificados (CRL) actualizada. Si este valor es cero, la nueva CRL solo se actualizará si expira la anterior. | Opcional |
|           **urlretrievaltimeout**            |                            Especifica el intervalo de tiempo de espera (en milisegundos) después del intento de recuperar la lista de revocación de certificados para la dirección URL remota.                            | Opcional |
|             **sslctlidentifier**             |                Especifica la lista de emisores de certificados en los que se puede confiar. Esta lista puede ser un subconjunto de los emisores de certificados que son de confianza para el equipo.                 | Opcional |
|             **sslctlstorename**              |                                                Especifica el nombre del almacén de certificados en LOCAL_MACHINE donde se almacena SslCtlIdentifier.                                                | Opcional |
|              **dsmapperusage**               |                                                        Especifica si los asignadores de DS están habilitados o deshabilitados. El valor predeterminado es deshabilitado.                                                         | Opcional |
|          **clientcertnegotiation**           |                                              Especifica si la negociación del certificado está habilitada o deshabilitada. El valor predeterminado es deshabilitado.                                               | Opcional |

---

**Example**

El siguiente es un ejemplo del comando **Add sslcert** .

Add sslcert ipport = pág: 443 certhash = 0102030405060708090A0B0C0D0E0F1011121314 AppID = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>agregar tiempo de espera

Agrega un tiempo de espera global al servicio.

**Sintáctica** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Los**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo de tiempo de espera para la configuración.                                     |
|    **value**    | Valor del tiempo de espera (en segundos). Si el valor está en notación hexadecimal, agregue el prefijo 0x. |

---

**Example**

A continuación se muestran dos ejemplos del comando **Agregar tiempo de espera** .

-   agregar tiempo de espera timeouttype = valor de idleconnectiontimeout, = 120
-   Agregar timeout timeouttype = HeaderWaitTimeout valor = 0x40

---

## <a name="add-urlacl"></a>Agregar urlacl

Agrega una entrada de reserva de localizador uniforme de recursos (URL). Este comando reserva la dirección URL para usuarios y cuentas que no son administradores. La DACL puede especificarse mediante el uso de un nombre de cuenta de NT con los parámetros Listen y Delegate o mediante una cadena SDDL.

**Sintáctica**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Los**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Especifica el localizador uniforme de recursos (URL) completo.                                           | Requerido |
|   **user**   |                                                      Especifica el nombre de usuario o grupo de usuarios                                                       | Requerido |
|  **escuchar**  | Especifica uno de los siguientes valores: yes: permite al usuario registrar las direcciones URL. Este es el valor predeterminado. no: deniega al usuario el registro de las direcciones URL. | Opcional |
| **Delegado** |  Especifica uno de los siguientes valores: yes: permitir que el usuario delegue las direcciones URL no: denegar que el usuario delegue las direcciones URL. Este es el valor predeterminado.  | Opcional |
|   **SDDL**   |                                                Especifica una cadena SDDL que describe la DACL.                                                 | Opcional |

---

**Example**

A continuación se muestran cuatro ejemplos del comando **Add urlacl** .

- Agregar urlacl URL =https://+:80/MyUri usuario = dominio\\usuario
- Agregar urlacl URL =<https://www.contoso.com:80/MyUri> usuario = dominio\\usuario escuchar = sí
- Agregar urlacl URL =<https://www.contoso.com:80/MyUri> usuario = dominio\\usuario delegado = no
- Agregar urlacl URL =https://+:80/MyUri SDDL =...

---

## <a name="delete-cache"></a>eliminar caché

Elimina todas las entradas, o una entrada especificada, de la memoria caché de URI del kernel del servicio HTTP.

**Sintáctica**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Los**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Especifica el localizador uniforme de recursos (URL) completo que desea eliminar.                     | Opcional |
| **recursiva** | Especifica si se quitan todas las entradas de la caché de direcciones URL. **sí**: quitar todas las entradas **no**: no quitar todas las entradas | Opcional |

---

**Example**

A continuación se muestran dos ejemplos del comando **Delete cache** .

- Delete cache URL =<https://www.contoso.com:80/myresource/> Recursive = Yes
- eliminar caché

---

## <a name="delete-iplisten"></a>eliminar iplisten

Elimina una dirección IP de la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP.

**Sintáctica**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Los**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **DirIP** | Dirección IPv4 o IPv6 que se va a eliminar de la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6. No incluye el número de puerto. | Requerido |

---


**Example**

A continuación se muestran cuatro ejemplos del comando **Delete iplisten** .

-   Delete iplisten IPAddress = fe80:: 1
-   Delete iplisten IPAddress = pág
-   Delete iplisten IPAddress = 0.0.0.0
-   eliminar iplisten ipaddress =::

---

## <a name="delete-sslcert"></a>eliminar sslcert


Elimina los enlaces de certificado de servidor SSL y las directivas de certificado de cliente correspondientes para una dirección IP y un puerto.

**Sintáctica**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Los**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica la dirección IPv4 o IPv6 y el puerto para los que se eliminan los enlaces de certificado SSL. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto. | Requerido |

---


**Example**

A continuación se muestran tres ejemplos del comando **Delete sslcert** .

- Delete sslcert ipport = pág: 443
- Delete sslcert ipport = 0.0.0.0:443
- Delete sslcert ipport = [::]: 443

---

## <a name="delete-timeout"></a>tiempo de espera de eliminación

Elimina un tiempo de espera global y hace que el servicio vuelva a los valores predeterminados.

**Sintáctica**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Los**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Especifica el tipo de configuración de tiempo de espera. | Requerido |

---


**Example**

A continuación se muestran dos ejemplos del comando **eliminar tiempo de espera** .

-   Delete timeout timeouttype = idleconnectiontimeout,
-   Delete timeout timeouttype = HeaderWaitTimeout

---

## <a name="delete-urlacl"></a>eliminar urlacl

Elimina las reservas de direcciones URL.

**Sintáctica**

```powershell
delete urlacl [ url= ] URL
```

**Los**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Especifica el localizador uniforme de recursos (URL) completo que desea eliminar. | Requerido |

---


**Example**

A continuación se muestran dos ejemplos del comando **Delete urlacl** .

- Delete urlacl URL =https://+:80/MyUri
- Delete urlacl URL =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>vaciar logbuffer

Vacía los búferes internos de los archivos de registro.

**Sintáctica**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Mostrar cachestate

Enumera los recursos de URI en caché y sus propiedades asociadas. Este comando muestra todos los recursos y sus propiedades asociadas que se almacenan en caché en la caché de respuesta HTTP o muestra un único recurso y sus propiedades asociadas.

**Sintáctica**

```powershell
show cachestate [ [url= ] URL]
```

**Los**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica la dirección URL completa que desea mostrar. Si no se especifica, se muestran todas las direcciones URL. La dirección URL también puede ser un prefijo para las direcciones URL registradas. | Opcional |

---


**Example**

A continuación se muestran dos ejemplos del comando **Show cachestate** :

- Mostrar dirección URL de cachestate =<https://www.contoso.com:80/myresource>
- Mostrar cachestate

---

## <a name="show-iplisten"></a>Mostrar iplisten

Muestra todas las direcciones IP en la lista de escucha de IP. La lista de escucha de IP se utiliza para establecer el ámbito de la lista de direcciones a las que se enlaza el servicio HTTP. "0.0.0.0" significa cualquier dirección IPv4 y "::" significa cualquier dirección IPv6.

**Sintáctica**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Mostrar servicestate

Muestra una instantánea del servicio HTTP.

**Sintáctica**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Los**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Ver**   | Especifica si se va a ver una instantánea del estado del servicio HTTP en función de la sesión del servidor o de las colas de solicitudes. | Opcional |
| **Detallado** |                Especifica si se va a mostrar información detallada que también muestra información de propiedades.                | Opcional |

---

**Example**

A continuación se muestran dos ejemplos del comando **Show servicestate** .

-   Mostrar servicestate ver = "sesión"
-   Show servicestate View = "requestq"

---

## <a name="show-sslcert"></a>Mostrar sslcert

Muestra los enlaces de certificado de servidor Capa de sockets seguros (SSL) y las directivas de certificado de cliente correspondientes para una dirección IP y un puerto.

**Sintáctica**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Los**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica la dirección IPv4 o IPv6 y el puerto para los que se muestran los enlaces de certificado SSL. Un carácter de dos puntos (:) se utiliza como delimitador entre la dirección IP y el número de puerto. Si no especifica ipport, se muestran todos los enlaces. | Requerido |

---


**Example**

A continuación se muestran cinco ejemplos del comando **Show sslcert** .

-   Show sslcert ipport = [fe80:: 1]: 443
-   Mostrar sslcert ipport = pág: 443
-   Mostrar sslcert ipport = 0.0.0.0:443
-   Mostrar sslcert ipport = [::]: 443
-   Mostrar sslcert

---

## <a name="show-timeout"></a>Mostrar tiempo de espera

Muestra, en segundos, los valores de tiempo de espera del servicio HTTP.

**Sintáctica**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>Mostrar urlacl

Muestra las listas de control de acceso discrecional (DACL) de la dirección URL reservada especificada o de todas las direcciones URL reservadas.

**Sintáctica**

```powershell
show urlacl [ [url= ] URL]
```

**Los**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica la dirección URL completa que desea mostrar. Si no se ha dado, se muestran todas las direcciones URL. | Opcional |

---


**Example**

A continuación se muestran tres ejemplos del comando **Show urlacl** .

- Mostrar dirección URL de urlacl =https://+:80/MyUri
- Mostrar dirección URL de urlacl =<https://www.contoso.com:80/MyUri>
- Mostrar urlacl

---