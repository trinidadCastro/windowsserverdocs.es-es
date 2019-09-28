---
title: cerrar sesión
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d09b58823f12d0b26bf21c00638b58046119bdab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374231"
---
# <a name="logoff"></a>cerrar sesión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cierra la sesión de un usuario en un servidor de host de sesión de Escritorio remoto (host de sesión de escritorio remoto) y elimina la sesión del servidor.
Para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                             Descripción                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Especifica el nombre de la sesión.                                                                  |
|     <SessionID>      |                                                 Especifica el identificador numérico que identifica la sesión en el servidor.                                                 |
| /server:<ServerName> | Especifica el servidor host de sesión de escritorio remoto que contiene la sesión cuyo usuario desea cerrar sesión. Si no se especifica, se usa el servidor en el que está activo actualmente. |
|          /v          |                                                       Muestra información acerca de las acciones que se llevan a cabo.                                                        |
|          /?          |                                                                 Muestra la ayuda en el símbolo del sistema.                                                                 |

## <a name="remarks"></a>Comentarios
- Siempre puede cerrar la sesión en la que ha iniciado sesión actualmente. Sin embargo, debe tener el permiso control total para cerrar la sesión de los usuarios de otras sesiones.
- El cierre de sesión de un usuario sin advertencia puede provocar la pérdida de datos en la sesión del usuario. Debe enviar un mensaje al usuario mediante el comando **MSG** para avisar al usuario antes de realizar esta acción.
- Si no se especifica <*SessionID*> o <*nombresesión*>, el **cierre** de sesión cierra la sesión del usuario en la sesión actual. Si especifica <*nombresesión*>, debe ser uno activo.
- Al cerrar la sesión de un usuario, finalizan todos los procesos y se elimina la sesión del servidor.
- No se puede cerrar la sesión de un usuario de la consola.
  ## <a name="BKMK_examples"></a>Example
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

#### <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Referencia &#40;de&#41; comandos de Terminal Services de servicios de escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
