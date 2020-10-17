---
title: tsdiscon
description: Artículo de referencia de tsdiscon, que desconecta una sesión de un servidor host de sesión de Escritorio remoto.
ms.topic: reference
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b7126e2e9d1f5185ea64566843c523bf0570891
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156381"
---
# <a name="tsdiscon"></a>tsdiscon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Desconecta una sesión de un servidor host de sesión de Escritorio remoto. Si no especifica un identificador de sesión o un nombre de sesión, este comando desconecta la sesión actual.

> [!IMPORTANT]
> Debe tener permiso de **acceso de control total** o desconectar el permiso de **acceso especial** para desconectar a otro usuario de una sesión.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
tsdiscon [<sessionID> | <sessionname>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<sessionID>` | Especifica el identificador de la sesión que se va a desconectar. |
| `<sessionname>` | Especifica el nombre de la sesión que se va a desconectar. |
| /server:`<servername>` | Especifica el servidor de Terminal Server que contiene la sesión que desea desconectar. De lo contrario, se usa el servidor host de sesión Escritorio remoto actual. Este parámetro solo es necesario si se ejecuta el comando **tsdiscon** desde un servidor remoto. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Las aplicaciones que se ejecutan cuando se desconecta la sesión se ejecutan automáticamente cuando se vuelve a conectar a esa sesión sin pérdida de datos. Puede usar el [comando restablecer sesión](reset-session.md) para finalizar las aplicaciones en ejecución de la sesión desconectada, pero esto puede producir la pérdida de datos en la sesión.

- No se puede desconectar la sesión de consola.

## <a name="examples"></a>Ejemplos

Para desconectar la sesión actual, escriba:

```
tsdiscon
```

Para desconectar la *sesión 10*, escriba:

```
tsdiscon 10
```

Para desconectar la sesión denominada *TERM04*, escriba:

```
tsdiscon TERM04
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)

- [comando de restablecimiento de sesión](reset-session.md)
