---
title: ksetup server
description: Artículo de referencia del comando ksetup Server, que le permite especificar un nombre para un equipo que ejecuta el sistema operativo Windows, de modo que los cambios realizados por el comando ksetup actualicen el equipo de destino.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99c708c3842d1d2d36783db09d60750bb2703670
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926098"
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
