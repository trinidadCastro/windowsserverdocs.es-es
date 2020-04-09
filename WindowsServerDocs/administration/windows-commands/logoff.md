---
title: logoff
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1661a9dd6cc89ea05980fd9085aa8fa67b8fe2c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840418"
---
# <a name="logoff"></a>logoff

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cierra la sesión de un usuario en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto) y elimina la sesión del servidor.
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

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
|          /?          |                                                                 Muestra la Ayuda en el símbolo del sistema.                                                                 |

## <a name="remarks"></a>Comentarios
- Siempre puede cerrar la sesión en la que ha iniciado sesión actualmente. Sin embargo, debe tener el permiso control total para cerrar la sesión de los usuarios de otras sesiones.
- El cierre de sesión de un usuario sin advertencia puede provocar la pérdida de datos en la sesión del usuario. Debe enviar un mensaje al usuario mediante el comando **MSG** para avisar al usuario antes de realizar esta acción.
- Si no se especifica <*SessionID*> o <*nombresesión*>, el **cierre** de sesión cierra la sesión del usuario en la sesión actual. Si especifica <*nombresesión*>, debe ser uno activo.
- Al cerrar la sesión de un usuario, finalizan todos los procesos y se elimina la sesión del servidor.
- No se puede cerrar la sesión de un usuario de la consola.
  ## <a name="examples"></a><a name=BKMK_examples></a>Example
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
