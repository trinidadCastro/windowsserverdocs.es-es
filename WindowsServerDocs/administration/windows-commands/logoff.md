---
title: cerrar sesión
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86acf174bfdebdeab6db7476713dd2d91f21b1a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724263"
---
# <a name="logoff"></a>cerrar sesión

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cierra la sesión de un usuario en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto) y elimina la sesión del servidor.


> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                             Descripción                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Especifica el nombre de la sesión.                                                                  |
|     <SessionID>      |                                                 Especifica el identificador numérico que identifica la sesión en el servidor.                                                 |
| /server:<ServerName> | Especifica el servidor host de sesión de escritorio remoto que contiene la sesión cuyo usuario desea cerrar sesión. Si no se especifica, se usa el servidor en el que está activo actualmente. |
|          /v          |                                                       Muestra información acerca de las acciones que se llevan a cabo.                                                        |
|          /?          |                                                                 Muestra la ayuda en el símbolo del sistema.                                                                 |

## <a name="remarks"></a>Observaciones
- Siempre puede cerrar la sesión en la que ha iniciado sesión actualmente. Sin embargo, debe tener el permiso control total para cerrar la sesión de los usuarios de otras sesiones.
- El cierre de sesión de un usuario sin advertencia puede provocar la pérdida de datos en la sesión del usuario. Debe enviar un mensaje al usuario mediante el comando **MSG** para avisar al usuario antes de realizar esta acción.
- Si no se especifica <*SessionID*> o <*nombresesión*>, el **cierre** de sesión cierra la sesión del usuario en la sesión actual. Si especifica <*nombresesión*>, debe ser uno activo.
- Al cerrar la sesión de un usuario, finalizan todos los procesos y se elimina la sesión del servidor.
- No se puede cerrar la sesión de un usuario de la consola.
  ## <a name="examples"></a>Ejemplos
- Para cerrar la sesión de un usuario de la sesión actual, escriba:
  ```
  logoff
  ```
- Para cerrar la sesión de un usuario mediante el identificador de la sesión, por ejemplo, la sesión 12, escriba:
  ```
  logoff 12
  ```
- Para cerrar la sesión de un usuario mediante el nombre de la sesión y el servidor, por ejemplo, TERM04 de sesión en server1, escriba:
  ```
  logoff TERM04 /server:Server1
  ```

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
