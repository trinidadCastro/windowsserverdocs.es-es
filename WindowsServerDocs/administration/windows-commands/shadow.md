---
title: shadow
description: Artículo de referencia del comando Shadow, que permite controlar de forma remota una sesión activa de otro usuario en un servidor host de sesión Escritorio remoto.
ms.topic: reference
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4689806b810fa46ccef3cd73b94c4d729c47f641
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718362"
---
# <a name="shadow"></a>shadow

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite controlar de forma remota una sesión activa de otro usuario en un servidor host de sesión de Escritorio remoto.

## <a name="syntax"></a>Sintaxis

```
shadow {<sessionname> | <sessionID>} [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<sessionname>` | Especifica el nombre de la sesión que desea controlar de forma remota. |
| `<sessionID>` | Especifica el identificador de la sesión que desea controlar de forma remota. Use el usuario de la **consulta** para mostrar la lista de sesiones y sus identificadores de sesión. |
| /server:`<servername>` | Especifica el servidor host de sesión de Escritorio remoto que contiene la sesión que desea controlar de forma remota. De forma predeterminada, se usa el servidor de Host4 de sesión Escritorio remoto actual. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Puede ver o controlar activamente la sesión. Si opta por controlar activamente una sesión de usuario, puede especificar acciones de teclado y mouse de entrada para la sesión.

- Siempre puede controlar de forma remota sus propias sesiones (excepto la sesión actual), pero debe tener el permiso control total o el permiso de acceso especial control remoto para controlar de forma remota otra sesión.

- También puede iniciar el control remoto mediante el administrador de Servicios de Escritorio remoto.

- Antes de que comience la supervisión, el servidor advierte al usuario de que la sesión está a punto de controlarse remotamente, a menos que está advertencia esté deshabilitada. Es posible que la sesión parezca detenida durante unos segundos mientras espera una respuesta del usuario. Para configurar el control remoto para usuarios y sesiones, use la herramienta de configuración de Servicios de Escritorio remoto o las extensiones de Servicios de Escritorio remoto a usuarios y grupos locales, y usuarios y equipos de Active Directory.

- Si la sesión no es compatible con la resolución de vídeo usada en la sesión que está controlando remotamente, la operación no se realiza correctamente.

- La sesión de consola no puede controlar de forma remota otra sesión ni se puede controlar de forma remota en otra sesión.

- Cuando desee finalizar el control remoto (sombreado), presione CTRL + `*` ( `*` solo con el teclado numérico).

## <a name="examples"></a>Ejemplos

Para Shadow *session 93*, escriba:

```
shadow 93
```

Para sombrear la sesión *ACCTG01*, escriba:

```
shadow ACCTG01
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
