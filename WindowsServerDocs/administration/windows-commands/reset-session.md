---
title: reset session
description: Artículo de referencia para el comando restablecer sesión, que permite restablecer una sesión en un servidor host de sesión Escritorio remoto.
ms.topic: reference
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 745a3ba51714ad3f5431dedbe9cebedf77e4ae72
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626915"
---
# <a name="reset-session"></a>reset session

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite restablecer (eliminar) una sesión en un servidor host de sesión Escritorio remoto. Debe restablecer una sesión sólo si no funciona correctamente o ha dejado de responder.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
reset session {<sessionname> | <sessionID>} [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<sessionname>` | Especifica el nombre de la sesión que desea restablecer. Para determinar el nombre de la sesión, use el [comando QUERY Session](query-session.md). |
| `<sessionID>` | Especifica el identificador de la sesión que se va a restablecer. |
| /server:`<servername>` | Especifica el servidor de Terminal Server que contiene la sesión que desea restablecer. De lo contrario, utiliza el servidor host de sesión Escritorio remoto actual. Este parámetro solo es necesario si usa este comando desde un servidor remoto. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="remarks"></a>Observaciones

- Siempre puede restablecer sus propias sesiones, pero debe tener permiso de acceso de **control total** para restablecer la sesión de otro usuario. Tenga en cuenta que el restablecimiento de una sesión de usuario sin advertir al usuario puede provocar la pérdida de datos en la sesión.

## <a name="examples"></a>Ejemplos

Para restablecer la sesión designada como *RDP-TCP # 6*, escriba:

```
reset session rdp-tcp#6
```

Para restablecer la sesión que utiliza el *ID. de sesión 3*, escriba:

```
reset session 3
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
