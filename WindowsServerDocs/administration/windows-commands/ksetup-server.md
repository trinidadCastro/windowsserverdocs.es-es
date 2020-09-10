---
title: ksetup server
description: Artículo de referencia del comando ksetup Server, que le permite especificar un nombre para un equipo que ejecuta el sistema operativo Windows, de modo que los cambios realizados por el comando ksetup actualicen el equipo de destino.
ms.topic: reference
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4be82315bc0d683b5399350c618aa87e615067ba
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639580"
---
# <a name="ksetup-server"></a>ksetup server

Permite especificar un nombre para un equipo que ejecuta el sistema operativo Windows, de modo que los cambios realizados por el comando **ksetup** actualicen el equipo de destino.

El nombre del servidor de destino se almacena en el registro en `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos` . Esta entrada no se muestra cuando se ejecuta el comando **ksetup** .

> [!IMPORTANT]
> No hay ninguna manera de quitar el nombre del servidor de destino. En su lugar, puede volver a cambiarlo al nombre del equipo local, que es el valor predeterminado.

## <a name="syntax"></a>Sintaxis

```
ksetup /server <servername>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<servername>` | Especifica el nombre completo del equipo en el que la configuración será efectiva, como *IPops897.Corp.contoso.com*.<p>Si se especifica un nombre de equipo de dominio completo completo, se producirá un error en el comando. |

### <a name="examples"></a>Ejemplos

Para que las configuraciones de **ksetup** sean eficaces en el equipo *IPops897* , que está conectado en el dominio Contoso, escriba:

```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)
