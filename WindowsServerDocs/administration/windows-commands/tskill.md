---
title: tskill
description: Artículo de referencia del comando tskill, que finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión Escritorio remoto.
ms.topic: reference
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b281df4852bc5fbc0756e7b052d82ea2f9bab6d5
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156375"
---
# <a name="tskill"></a>tskill

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión Escritorio remoto.

> [!NOTE]
> Puede usar este comando para finalizar solo los procesos que le pertenecen, a menos que sea administrador. Los administradores tienen acceso total a todas las funciones de **tskill** y pueden finalizar los procesos que se ejecutan en otras sesiones de usuario.
>
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
tskill {<processID> | <processname>} [/server:<servername>] [/id:<sessionID> | /a] [/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<processID>` | Especifica el identificador del proceso que desea finalizar. |
| `<processname>` | Especifica el nombre del proceso que desea finalizar. Este parámetro puede incluir caracteres comodín. |
| /server:`<servername>` | Especifica el servidor de Terminal Server que contiene el proceso que desea finalizar. Si no se especifica **/Server** , se usa el servidor host de sesión escritorio remoto actual. |
| /ID`<sessionID>` | Finaliza el proceso que se está ejecutando en la sesión especificada. |
| /a | Finaliza el proceso que se está ejecutando en todas las sesiones. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Si se terminan todos los procesos ejecutados en una sesión, la sesión también termina.

- Si utiliza los `<processname>` `/server:<servername>` parámetros y, también debe especificar el `/id:<sessionID>` parámetro o **/a** .

## <a name="examples"></a>Ejemplos

Para finalizar el proceso 6543, escriba:

```
tskill 6543
```

Para finalizar el explorador de procesos que se ejecuta en la sesión 5, escriba:

```
tskill explorer /id:5
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
