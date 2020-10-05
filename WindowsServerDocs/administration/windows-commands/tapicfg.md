---
title: tapicfg
description: Artículo de referencia para los comandos Tapicfg, que crea, quita o muestra una partición de directorio de aplicaciones TAPI, o establece una partición de directorio de aplicaciones TAPI predeterminada.
ms.topic: reference
ms.assetid: c0e642ce-5d98-4edb-9a65-1dff09aef4e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 17b596036251d6ea8588de3b70359ea161b67c58
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718132"
---
# <a name="tapicfg"></a>tapicfg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Crea, quita o muestra una partición de directorio de aplicaciones TAPI, o establece una partición de directorio de aplicaciones TAPI predeterminada. Los clientes TAPI 3,1 pueden usar la información de esta partición de directorio de aplicaciones con el servicio de localizador de servicios de directorio para buscar y comunicarse con directorios TAPI. También puede usar **Tapicfg** para crear o quitar puntos de conexión de servicio, que permiten a los clientes TAPI localizar de forma eficaz las particiones de directorio de aplicaciones TAPI en un dominio.

Esta herramienta de línea de comandos puede ejecutarse en cualquier equipo que sea miembro del dominio.

## <a name="syntax"></a>Sintaxis

```
tapicfg install
tapicfg remove
tapicfg publishscp
tapicfg removescp
tapicfg show
tapicfg makedefault
```

### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
|--|--|
| [Tapicfg (instalación)](tapicfg-install.md) | Crea una partición de directorio de aplicaciones TAPI. |
| [Tapicfg Remove](tapicfg-remove.md) | Quita una partición de directorio de aplicaciones TAPI.|
| [Tapicfg publishscp](tapicfg-publishscp.md) | Crea un punto de conexión de servicio para publicar una partición de directorio de aplicaciones TAPI. |
| [Tapicfg removescp](tapicfg-removescp.md) | Quita un punto de conexión de servicio para una partición de directorio de aplicaciones TAPI. |
| [Tapicfg show](tapicfg-show.md) | Muestra los nombres y las ubicaciones de las particiones de directorio de aplicaciones TAPI del dominio. |
| [Tapicfg makedefault](tapicfg-makedefault.md) | Establece la partición del directorio de aplicaciones TAPI predeterminado para el dominio. |

#### <a name="remarks"></a>Observaciones

- Debe ser miembro del grupo administradores de **empresas** en Active Directory para ejecutar **Tapicfg install** (para crear una partición de directorio de aplicaciones TAPI) o **Tapicfg Remove** (para quitar una partición de directorio de aplicaciones TAPI).

- El texto proporcionado por el usuario (como los nombres de las particiones de directorio de aplicaciones TAPI, los servidores y los dominios) con caracteres internacionales o Unicode solo se muestra correctamente si se instalan las fuentes y la compatibilidad con idiomas correspondientes.

- Puede seguir usando los servidores del servicio de ubicación de Internet (ILS) en su organización, si se necesita ILS para admitir determinadas aplicaciones, ya que los clientes TAPI que ejecuten Windows XP o un sistema operativo Windows Server 2003 pueden consultar servidores ILS o particiones de directorio de aplicaciones TAPI.

- Puede usar **Tapicfg** para crear o quitar puntos de conexión de servicio. Si se cambia el nombre de la partición del directorio de aplicaciones TAPI por cualquier motivo (por ejemplo, si cambia el nombre del dominio en el que reside), debe quitar el punto de conexión de servicio existente y crear uno nuevo que contenga el nuevo nombre DNS de la partición del directorio de la aplicación TAPI que se va a publicar. De lo contrario, los clientes TAPI no podrán encontrar ni tener acceso a la partición de directorio de aplicaciones TAPI. También puede quitar un punto de conexión de servicio para fines de mantenimiento o seguridad (por ejemplo, si no desea exponer datos TAPI en una partición de directorio de aplicaciones TAPI específica).

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Tapicfg (instalación)](tapicfg-install.md)

- [Tapicfg Remove](tapicfg-remove.md)

- [Tapicfg publishscp](tapicfg-publishscp.md)

- [Tapicfg removescp](tapicfg-removescp.md)

- [Tapicfg show](tapicfg-show.md)

- [Tapicfg makedefault](tapicfg-makedefault.md)
