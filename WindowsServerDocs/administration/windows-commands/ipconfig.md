---
title: ipconfig
description: Artículo de referencia para el comando ipconfig, que muestra todos los valores de configuración de red TCP/IP actuales y actualiza la configuración del Protocolo de configuración dinámica de host (DHCP) y el sistema de nombres de dominio (DNS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3567f855a6066ed318f10daa22f1ca8de0d565c4
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924354"
---
# <a name="ipconfig"></a>ipconfig

Muestra todos los valores de configuración de red TCP/IP actuales y actualiza la configuración del Protocolo de configuración dinámica de host (DHCP) y el sistema de nombres de dominio (DNS). Si se usa sin parámetros, **ipconfig** muestra las direcciones IPv6 (Protocolo de Internet versión 4) e IPv6, la máscara de subred y la puerta de enlace predeterminada para todos los adaptadores.

## <a name="syntax"></a>Sintaxis

```
ipconfig [/allcompartments] [/all] [/renew [<adapter>]] [/release [<adapter>]] [/renew6[<adapter>]] [/release6 [<adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <adapter>] [/setclassid <adapter> [<classID>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /all | Muestra la configuración TCP/IP completa de todos los adaptadores. Los adaptadores pueden representar interfaces físicas, como adaptadores de red instalados o interfaces lógicas como conexiones de acceso telefónico. |
| comando/displaydns | Muestra el contenido de la memoria caché de la resolución del cliente DNS, que incluye las entradas cargadas previamente desde el archivo de hosts local y los registros de recursos que se han obtenido recientemente para las consultas de nombres resueltas por el equipo. El servicio cliente DNS usa esta información para resolver rápidamente los nombres consultados con frecuencia, antes de consultar sus servidores DNS configurados. |
| /flushdns | Vacía y restablece el contenido de la memoria caché de la resolución del cliente DNS. Durante la solución de problemas de DNS, puede usar este procedimiento para descartar las entradas de caché negativas de la memoria caché, así como cualquier otra entrada que se haya agregado dinámicamente. |
| /registerdns | Inicia el registro dinámico manual de los nombres DNS y las direcciones IP que se configuran en un equipo. Puede usar este parámetro para solucionar un error de registro de nombres DNS o resolver un problema de actualización dinámica entre un cliente y el servidor DNS sin reiniciar el equipo cliente. La configuración de DNS en las propiedades avanzadas del protocolo TCP/IP determina los nombres que se registran en DNS. |
| /Release`[<adapter>]` | Envía un mensaje DHCPRELEASE al servidor DHCP para liberar la configuración actual de DHCP y descartar la configuración de la dirección IP para todos los adaptadores (si no se especifica un adaptador) o para un adaptador específico, si se incluye el parámetro *adaptador* . Este parámetro deshabilita TCP/IP para los adaptadores configurados para obtener una dirección IP automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros. |
| /release6`[<adapter>]` | Envía un mensaje DHCPRELEASE al servidor DHCPv6 para liberar la configuración actual de DHCP y descartar la configuración de la dirección IPv6 para todos los adaptadores (si no se especifica un adaptador) o para un adaptador específico, si se incluye el parámetro *Adapter* . Este parámetro deshabilita TCP/IP para los adaptadores configurados para obtener una dirección IP automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros. |
| /Renew`[<adapter>]` | Renueva la configuración de DHCP para todos los adaptadores (si no se especifica un adaptador) o para un adaptador específico si se incluye el parámetro de *adaptador* . Este parámetro solo está disponible en equipos con adaptadores que estén configurados para obtener una dirección IP automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros. |
| /renew6`[<adapter>]` | Renueva la configuración de DHCPv6 para todos los adaptadores (si no se especifica un adaptador) o para un adaptador específico si se incluye el parámetro de *adaptador* . Este parámetro solo está disponible en equipos con adaptadores que estén configurados para obtener una dirección IPv6 automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros. |
| /setclassid`<adapter>[<classID>]` | Configura el identificador de clase DHCP para un adaptador especificado. Para establecer el identificador de clase de DHCP para todos los adaptadores, use el carácter comodín de asterisco (**&#42;**) en lugar del *adaptador*. Este parámetro solo está disponible en equipos con adaptadores que estén configurados para obtener una dirección IP automáticamente. Si no se especifica un identificador de clase DHCP, se quita el ID. de clase actual. |
| /showclassid`<adapter>` | Muestra el identificador de clase DHCP de un adaptador especificado. Para ver el identificador de clase de DHCP para todos los adaptadores, use el carácter comodín de asterisco (**&#42;**) en lugar del *adaptador*. Este parámetro solo está disponible en equipos con adaptadores que estén configurados para obtener una dirección IP automáticamente. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Este comando es muy útil en los equipos que están configurados para obtener una dirección IP automáticamente. Esto permite a los usuarios determinar qué valores de configuración de TCP/IP han sido configurados por DHCP, el direccionamiento IP privado automático (APIPA) o una configuración alternativa.

- Si el nombre proporcionado para el *adaptador* contiene espacios, utilice comillas alrededor del nombre del adaptador (por ejemplo, "nombre del adaptador").

- En el caso de los nombres de adaptador, **ipconfig** admite el uso del carácter comodín de asterisco (*) para especificar los adaptadores con nombres que comienzan por una cadena o adaptadores especificados con nombres que contienen una cadena especificada. Por ejemplo, `Local*` coincide con todos los adaptadores que comienzan con la cadena local y `*Con*` coincide con todos los adaptadores que contienen la cadena con.

### <a name="examples"></a>Ejemplos

Para mostrar la configuración básica de TCP/IP de todos los adaptadores, escriba:

```
ipconfig
```

Para mostrar la configuración TCP/IP completa de todos los adaptadores, escriba:

```
ipconfig /all
```

Para renovar una configuración de dirección IP asignada por DHCP solo para el adaptador de conexión de área local, escriba:

```
ipconfig /renew Local Area Connection
```

Para vaciar la memoria caché de la resolución DNS al solucionar problemas de resolución de nombres DNS, escriba:

```
ipconfig /flushdns
```

Para mostrar el ID. de clase de DHCP para todos los adaptadores cuyos nombres empiecen por local, escriba:

```
ipconfig /showclassid Local*
```

Para establecer el identificador de clase DHCP del adaptador de conexión de área local que se va a probar, escriba:

```
ipconfig /setclassid Local Area Connection TEST
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
