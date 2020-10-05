---
title: Tapicfg show
description: Artículo de referencia del comando Tapicfg show, que muestra los nombres y las ubicaciones de las particiones de directorio de aplicaciones TAPI del dominio.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/29/2020
ms.openlocfilehash: b39021a1e7c76ee8db359b33f032bac11225e85c
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731179"
---
# <a name="tapicfg-show"></a>Tapicfg show

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra los nombres y las ubicaciones de las particiones de directorio de aplicaciones TAPI del dominio.

## <a name="syntax"></a>Sintaxis

```
tapicfg show [/defaultonly] [/domain:<domainname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| solo/default | Muestra los nombres y las ubicaciones de solo la partición del directorio de aplicaciones TAPI predeterminada en el dominio. |
| /Domain `<domainname>` | Especifica el nombre DNS del dominio para el que se muestran las particiones de directorio de aplicaciones TAPI. Si no se especifica el nombre de dominio, se usa el nombre del dominio local. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Esta herramienta de línea de comandos puede ejecutarse en cualquier equipo que sea miembro del dominio.

- El texto proporcionado por el usuario (como los nombres de las particiones de directorio de aplicaciones TAPI, los servidores y los dominios) con caracteres internacionales o Unicode solo se muestra correctamente si se instalan las fuentes y la compatibilidad con idiomas correspondientes.

- Puede seguir usando los servidores del servicio de ubicación de Internet (ILS) en su organización, si se necesita ILS para admitir determinadas aplicaciones, ya que los clientes TAPI que ejecuten Windows XP o un sistema operativo Windows Server 2003 pueden consultar servidores ILS o particiones de directorio de aplicaciones TAPI.

- Puede usar **Tapicfg** para crear o quitar puntos de conexión de servicio. Si se cambia el nombre de la partición del directorio de aplicaciones TAPI por cualquier motivo (por ejemplo, si cambia el nombre del dominio en el que reside), debe quitar el punto de conexión de servicio existente y crear uno nuevo que contenga el nuevo nombre DNS de la partición del directorio de la aplicación TAPI que se va a publicar. De lo contrario, los clientes TAPI no podrán encontrar ni tener acceso a la partición de directorio de aplicaciones TAPI. También puede quitar un punto de conexión de servicio para fines de mantenimiento o seguridad (por ejemplo, si no desea exponer datos TAPI en una partición de directorio de aplicaciones TAPI específica).

## <a name="example"></a>Ejemplo

Para mostrar el nombre de la partición de directorio de aplicaciones TAPI predeterminada para el nuevo dominio, escriba:

```
tapicfg show /defaultonly
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Tapicfg (instalación)](tapicfg-install.md)

- [Tapicfg Remove](tapicfg-remove.md)

- [Tapicfg publishscp](tapicfg-publishscp.md)

- [Tapicfg removescp](tapicfg-removescp.md)

- [Tapicfg makedefault](tapicfg-makedefault.md)
