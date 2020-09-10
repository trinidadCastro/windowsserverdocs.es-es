---
title: ipxroute
description: Artículo de referencia del comando ipxroute, que muestra y modifica información acerca de las tablas de enrutamiento utilizadas por el protocolo IPX.
ms.topic: reference
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dbfc2df421429d4265cbcb425d189c4fd69de99d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634428"
---
# <a name="ipxroute"></a>ipxroute

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra y modifica información acerca de las tablas de enrutamiento utilizadas por el protocolo IPX. Si se usa sin parámetros, **ipxroute** muestra la configuración predeterminada de los paquetes que se envían a direcciones desconocidas, de difusión y de multidifusión.

## <a name="syntax"></a>Sintaxis

```
ipxroute servers [/type=x]
ipxroute ripout <network>
ipxroute resolve {guid | name} {GUID | <adaptername>}
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]
ipxroute config
```

### <a name="parameters"></a>Parámetros
| Parámetro | Descripción |
| ------- | -------- |
| servidores`[/type=x]` | Muestra la tabla de punto de acceso de servicio (SAP) para el tipo de servidor especificado. **x** debe ser un entero. Por ejemplo, `/type=4` muestra todos los servidores de archivos. Si no especifica **/Type**, `ipxroute servers` muestra todos los tipos de servidores y los enumera por nombre de servidor. |
| resolver `{GUID | name}``{GUID | adaptername}` | Resuelve el nombre del GUID como su nombre descriptivo o el nombre descriptivo de su GUID. |
| panel = *n* | Especifica el adaptador de red para el que se van a consultar o establecer parámetros. |
| def | Envía paquetes a la difusión ALL ROUTEs. Si un paquete se transmite a una dirección de tarjeta de acceso a medios (MAC) única que no se encuentra en la tabla de enrutamiento de origen, **ipxroute** envía el paquete a la difusión de rutas únicas de forma predeterminada. |
| GbR | Envía paquetes a la difusión ALL ROUTEs. Si un paquete se transmite a la dirección de difusión (FFFFFFFFFFFF), **ipxroute** envía el paquete a la difusión de rutas únicas de forma predeterminada. |
| Maestro | Envía paquetes a la difusión ALL ROUTEs. Si un paquete se transmite a una dirección de multidifusión (C000xxxxxxxx), **ipxroute** envía el paquete a la difusión de rutas únicas de forma predeterminada. |
| Remove =*XXXXXXXXXXXX* | quita la dirección de nodo especificada de la tabla de enrutamiento de origen. |
| config | Muestra información acerca de todos los enlaces para los que se ha configurado IPX. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para mostrar los segmentos de red a los que está conectada la estación de trabajo, la dirección del nodo de estación de trabajo y el tipo de marco que se está usando, escriba:

```
ipxroute config
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
