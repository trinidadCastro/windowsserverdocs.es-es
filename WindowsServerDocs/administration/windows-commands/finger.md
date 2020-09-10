---
title: finger
description: Artículo de referencia del comando Finger, que muestra información acerca de los usuarios de un equipo remoto especificado que ejecuta el servicio Finger o el demonio.
ms.topic: reference
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e2c631fe02b22ea0fc57a9e338f80ac15b00873f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634926"
---
# <a name="finger"></a>finger

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de los usuarios de un equipo remoto especificado (normalmente, un equipo que ejecuta UNIX) que está ejecutando el servicio Finger o el demonio. El equipo remoto especifica el formato y la salida de la pantalla de información del usuario. Cuando se usa sin parámetros, el **dedo** muestra la ayuda.

> [!IMPORTANT]
> Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.

## <a name="syntax"></a>Sintaxis

```
finger [-l] [<user>] [@<host>] [...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -l | Muestra información de usuario en formato de lista largo. |
| `<user>` | Especifica el usuario sobre el que desea obtener información. Si omite el parámetro *User* , este comando muestra información acerca de todos los usuarios del equipo especificado. |
| `@<host>` | Especifica el equipo remoto que ejecuta el servicio Finger en el que busca información de usuario. Puede especificar un nombre de equipo o una dirección IP. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Debe **anteponer** un guion (-) en lugar de una barra diagonal (/).

- `user@host`Se pueden especificar varios parámetros.

### <a name="examples"></a>Ejemplos

Para mostrar información de *user1* en el equipo *users.Microsoft.com*, escriba:

```
finger user1@users.microsoft.com
```

Para mostrar información de *todos los usuarios* del equipo *users.Microsoft.com*, escriba:

```
finger @users.microsoft.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
