---
title: ipconfig
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15071c2c-4815-4893-93b2-ab30232e312e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fff4e5088e3e08cf2e9e742d8fb6aa2187bb9e83
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438135"
---
# <a name="ipconfig"></a>ipconfig



Muestra todos los valores de configuración de red TCP/IP actuales y actualiza la configuración de protocolo de configuración dinámica de Host (DHCP) y sistema de nombres de dominio (DNS). Se utiliza sin parámetros, **ipconfig** muestra el protocolo de Internet versión 4 (IPv4) y IPv6 direcciones, la máscara de subred y puerta de enlace predeterminada para todos los adaptadores.

## <a name="syntax"></a>Sintaxis

```
ipconfig [/allcompartments] [/all] [/renew [<Adapter>]] [/release [<Adapter>]] [/renew6[<Adapter>]] [/release6 [<Adapter>]] [/flushdns] [/displaydns] [/registerdns] [/showclassid <Adapter>] [/setclassid <Adapter> [<ClassID>]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/ all|Muestra la configuración de TCP/IP completa de todos los adaptadores. Los adaptadores pueden representar interfaces físicas, como adaptadores de red instalados o interfaces lógicas como conexiones de acceso telefónico.|
|/allcompartments|Muestra la configuración de TCP/IP completa de todos los compartimientos.|
|/displaydns|Muestra el contenido de la caché de resolución de cliente DNS, que incluye las entradas cargadas previamente desde el archivo Hosts local y cualquier obtenido recientemente los registros de recursos para las consultas de nombres resueltos por el equipo. El servicio cliente DNS usa esta información para resolver nombres consultados con más frecuencia rápidamente, antes de consultar sus servidores DNS configurados.|
|/flushdns|Vacía y restablece el contenido de la caché de resolución de cliente DNS. Durante la solución de problemas de DNS, puede usar este procedimiento para descartar las entradas de caché negativas de la memoria caché, así como otras entradas que se han agregado dinámicamente.|
|/registerdns|Inicia el registro dinámico manual para los nombres DNS y direcciones IP que están configuradas en un equipo. Puede usar este parámetro para solucionar problemas de un registro de nombres DNS con errores o resolver un problema de actualización dinámica entre un cliente y el servidor DNS sin tener que reiniciar el equipo cliente. La configuración de DNS en las propiedades avanzadas del protocolo TCP/IP determina qué nombres están registrados en DNS.|
|/ versión [\<adaptador >]|Envía un mensaje DHCPRELEASE al servidor DHCP para liberar la configuración de DHCP actual y descartar la configuración de dirección IP para todos los adaptadores (si no se especifica un adaptador) o de un adaptador específico si el *adaptador* se incluye un parámetro. Este parámetro deshabilita TCP/IP para los adaptadores configurados para obtener una dirección IP automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros.|
|/RELEASE6 [\<adaptador >]|Envía un mensaje DHCPRELEASE al servidor DHCPv6 para liberar la configuración de DHCP actual y descartar la configuración de direcciones IPv6 para todos los adaptadores (si no se especifica un adaptador) o de un adaptador específico si el *adaptador* se incluye un parámetro. Este parámetro deshabilita TCP/IP para los adaptadores configurados para obtener una dirección IP automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros.|
|/ renew [\<adaptador >]|Renueva la configuración de DHCP para todos los adaptadores (si no se especifica un adaptador) o de un adaptador específico si el *adaptador* se incluye el parámetro. Este parámetro está disponible solo en equipos con los adaptadores que están configurados para obtener una dirección IP automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros.|
|/renew6 [\<adaptador >]|Renueva la configuración de DHCPv6 para todos los adaptadores (si no se especifica un adaptador) o de un adaptador específico si el *adaptador* se incluye el parámetro. Este parámetro está disponible solo en equipos con los adaptadores que están configurados para obtener una dirección IPv6 automáticamente. Para especificar un nombre de adaptador, escriba el nombre del adaptador que aparece cuando se usa **ipconfig** sin parámetros.|
|/setclassid \<adaptador > [ <ClassID>]|Configura el ID. Para establecer el identificador de clase DHCP para todos los adaptadores, utilice el asterisco ( **&#42;** ) caracteres comodín en lugar de *adaptador*. Este parámetro está disponible solo en equipos con los adaptadores que están configurados para obtener una dirección IP automáticamente. Si no se especifica un identificador de clase DHCP, se quita el identificador de clase actual.|
|/showclassid \<adaptador >|Muestra el ID. Para ver el identificador de clase DHCP para todos los adaptadores, use el asterisco ( **&#42;** ) caracteres comodín en lugar de *adaptador*. Este parámetro está disponible solo en equipos con los adaptadores que están configurados para obtener una dirección IP automáticamente.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

- Este comando es muy útil en los equipos que están configurados para obtener una dirección IP automáticamente. Esto permite a los usuarios a determinar qué valores de configuración de TCP/IP se han configurado por DHCP, direcciones IP privada automática (APIPA) o una configuración alternativa.
- Si el nombre que se suministran a *adaptador* contiene espacios, utilice comillas alrededor del nombre de adaptador (ejemplo: **"** <em>Nombre de adaptador</em> **"** ).
- Para los nombres de adaptador, **ipconfig** admite el uso del asterisco ( *) carácter comodín para especificar los adaptadores con nombres que comienzan con una cadena especificada o los adaptadores con nombres que contienen la cadena especificada. Por ejemplo, **Local\\** *   coincide con todos los adaptadores que comienzan con la cadena Local y  **\*Con\\** * coincide con todos los adaptadores que contienen el cadena Con.

## <a name="examples"></a>Ejemplos

Para mostrar la configuración básica de TCP/IP para todos los adaptadores, escriba:
```
ipconfig
```
Para mostrar la configuración de TCP/IP completa de todos los adaptadores, escriba:
```
ipconfig /all
```
Para renovar una configuración de dirección IP asignada por DHCP para que sólo el adaptador de conexión de área Local, escriba:
```
ipconfig /renew "Local Area Connection"
```
Para vaciar la memoria caché de resolución DNS al solucionar problemas de resolución de nombres DNS, escriba:
```
ipconfig /flushdns
```
Para mostrar el identificador de clase DHCP para todos los adaptadores con nombres que empiecen por Local, escriba:
```
ipconfig /showclassid Local*
```
Para establecer el identificador de clase DHCP para el adaptador de conexión de área Local para pruebas, escriba:
```
ipconfig /setclassid "Local Area Connection" TEST
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)