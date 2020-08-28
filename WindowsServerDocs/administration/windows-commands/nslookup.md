---
title: nslookup
description: Artículo de referencia del comando Nslookup, que muestra información que puede usar para diagnosticar la infraestructura del sistema de nombres de dominio (DNS).
ms.topic: reference
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c06121384ec18a879eaccbb34c926ce68cff9f49
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032783"
---
# <a name="nslookup"></a>nslookup

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información que puede usar para diagnosticar la infraestructura del sistema de nombres de dominio (DNS). Antes de usar esta herramienta, debe estar familiarizado con el funcionamiento de DNS. La herramienta de línea de comandos nslookup solo está disponible si se ha instalado el protocolo TCP/IP.

La herramienta de línea de comandos nslookup tiene dos modos: interactivo y no interactivo.

Si solo necesita buscar una parte de los datos, se recomienda usar el modo no interactivo. En el primer parámetro, escriba el nombre o la dirección IP del equipo que desea buscar. En el segundo parámetro, escriba el nombre o la dirección IP de un servidor de nombres DNS. Si omite el segundo argumento, **nslookup** usa el servidor de nombres DNS predeterminado.

Si necesita buscar más de un fragmento de datos, puede usar el modo interactivo. Escriba un guión (-) para el primer parámetro y el nombre o la dirección IP de un servidor de nombres DNS para el segundo parámetro. Si omite ambos parámetros, la herramienta utiliza el servidor de nombres DNS predeterminado. Al usar el modo interactivo, puede:

- Interrumpa los comandos interactivos en cualquier momento presionando CTRL + B.

- Salga de; para ello, escriba **Exit**.

- Trate un comando integrado como un nombre de equipo, anteponiendo el carácter de escape ( \) . Un comando desconocido se interpreta como un nombre de equipo.

## <a name="syntax"></a>Sintaxis

```
nslookup [exit | finger | help | ls | lserver | root | server | set | view] [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [salir de nslookup](nslookup-exit-command.md) | Sale de la herramienta de línea de comandos nslookup. |
| [dedo de nslookup](nslookup-finger-command.md) | Se conecta con el servidor Finger en el equipo actual. |
| [nslookup help](nslookup-help.md) | Muestra un breve resumen de los subcomandos. |
| [nslookup ls](nslookup-ls.md) | Muestra información de un dominio DNS. |
| [nslookup lserver](nslookup-lserver.md) | Cambia el servidor predeterminado al dominio DNS especificado. |
| [nslookup root](nslookup-root.md) | Cambia el servidor predeterminado al servidor para la raíz del espacio de nombres de dominio DNS. |
| [nslookup server](nslookup-server.md) | Cambia el servidor predeterminado al dominio DNS especificado. |
| [nslookup set](nslookup-set.md) | Cambia los valores de configuración que afectan a cómo funcionan las búsquedas. |
| [nslookup set all](nslookup-set-all.md) | Imprime los valores actuales de las opciones de configuración. |
| [nslookup set class](nslookup-set-class.md) | Cambia la clase de consulta. La clase especifica el grupo de protocolos de la información. |
| [nslookup set d2](nslookup-set-d2.md) | Activa o desactiva el modo de depuración exhaustiva. Se imprimen todos los campos de cada paquete. |
| [nslookup set debug](nslookup-set-debug.md) | Activa o desactiva el modo de depuración. |
| [nslookup set domain](nslookup-set-domain.md) | Cambia el nombre de dominio DNS predeterminado por el nombre especificado. |
| [nslookup set port](nslookup-set-port.md) | Cambia el puerto del servidor de nombres DNS TCP/UDP predeterminado al valor especificado. |
| [nslookup set querytype](nslookup-set-querytype.md) | Cambia el tipo de registro de recursos de la consulta. |
| [nslookup set recurse](nslookup-set-recurse.md) | Indica al servidor de nombres DNS que consulte a otros servidores si no tiene la información. |
| [nslookup set retry](nslookup-set-retry.md) | Establece el número de reintentos. |
| [nslookup set root](nslookup-set-root.md) | Cambia el nombre del servidor raíz que se usa para las consultas. |
| [nslookup set search](nslookup-set-search.md) | Anexa los nombres de dominio DNS de la lista de búsqueda de dominios DNS a la solicitud hasta que se reciba una respuesta. Esto se aplica cuando el conjunto y la solicitud de búsqueda contienen al menos un punto, pero no finalizan con un punto final. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Cambia el nombre de dominio DNS y la lista de búsqueda predeterminados. |
| [nslookup set timeout](nslookup-set-timeout.md) | Cambia el número inicial de segundos de espera para una respuesta a una solicitud. |
| [nslookup set type](nslookup-set-type.md) | Cambia el tipo de registro de recursos de la consulta. |
| [nslookup set vc](nslookup-set-vc.md) | Especifica que se use o no un circuito virtual al enviar solicitudes al servidor. |
| [nslookup view](nslookup-view.md) | Ordena y muestra la salida del subcomando o los comandos **LS** anteriores. |

### <a name="remarks"></a>Observaciones

- Si *computerTofind* es una dirección IP y la consulta es para un tipo de registro de recursos **A** o **ptr** , se devuelve el nombre del equipo.

- Si *computerTofind* es un nombre y no tiene un punto final, el nombre de dominio DNS predeterminado se anexa al nombre. Este comportamiento depende del estado de los siguientes subcomandos **set** : **Domain**, **srchlist**, **defname**y **Search**.

- Si escribe un guión (-) en lugar de *computerTofind*, el símbolo del sistema cambia al modo interactivo de **nslookup** .

- Si se produce un error en la solicitud de búsqueda, la herramienta de línea de comandos proporciona un mensaje de error, que incluye:

  | Mensaje de error | Descripción |
  | ------------- | ----------- |
  | se agotó el tiempo de espera |El servidor no respondió a una solicitud después de un determinado período de tiempo y cierto número de reintentos. Puede establecer el período de tiempo de espera con el comando [nslookup Set timeout](nslookup-set-timeout.md) . Puede establecer el número de reintentos con el comando [nslookup set retry](nslookup-set-retry.md) . |
  | No hay respuesta del servidor | No se está ejecutando ningún servidor de nombres DNS en el equipo servidor. |
  | No hay registros | El servidor de nombres DNS no tiene registros de recursos del tipo de consulta actual para el equipo, aunque el nombre del equipo es válido. El tipo de consulta se especifica con el comando [nslookup Set QueryType](nslookup-set-querytype.md) . |
  | Dominio inexistente | El nombre del equipo o dominio DNS no existe. |
  | Conexión rechazada o no se puede tener acceso a la red | No se pudo establecer la conexión con el servidor de nombres DNS o el servidor Finger. Este error suele producirse con las solicitudes **LS** y **Finger** . |
  | Error de servidor | El servidor de nombres DNS encontró una incoherencia interna en su base de datos y no pudo devolver una respuesta válida. |
  | Dar | El servidor de nombres DNS rechazó el servicio de la solicitud. |
  | error de formato | El servidor de nombres DNS detectó que el paquete de solicitud no tenía el formato correcto. Puede indicar un error en **nslookup**. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
