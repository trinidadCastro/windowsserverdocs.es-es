---
title: cerrar sesión
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 88b3cbbbd52a965fd0a0de3c164e8dbc1f90d053
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437526"
---
# <a name="logoff"></a>cerrar sesión

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cierra un usuario desde una sesión en un servidor Host de sesión de escritorio remoto (Host de sesión de rd) y elimina la sesión del servidor.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                             Descripción                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Especifica el nombre de la sesión.                                                                  |
|     <SessionID>      |                                                 Especifica el identificador numérico que identifica la sesión en el servidor.                                                 |
| /server:<ServerName> | Especifica el servidor de Host de sesión de escritorio remoto que contiene la sesión de usuario que desea cerrar la sesión. Si no se especifica, se usa el servidor en el que están activas actualmente. |
|          /v          |                                                       Muestra información sobre las acciones que se va a realizar.                                                        |
|          /?          |                                                                 Muestra la ayuda en el símbolo del sistema.                                                                 |

## <a name="remarks"></a>Comentarios
- Siempre puede cerrar la sesión a la que han iniciado sesión. Sin embargo, debe tener el permiso Control total a los usuarios de otras sesiones.
- Cierra la sesión un usuario desde una sesión sin previo aviso puede dar lugar a pérdida de datos en la sesión del usuario. Debe enviar un mensaje al usuario mediante el **msg** comando para advertir al usuario antes de realizar esta acción.
- Si <*SessionID*> o <*SessionName*> no se especifica, **logoff** cierra al usuario de la sesión actual. Si se especifica <*SessionName*>, debe estar activo.
- Al cerrar un usuario, finalizan todos los procesos y la sesión se elimina del servidor.
- No puede registrar un usuario de la sesión de consola.
  ## <a name="BKMK_examples"></a>Ejemplos
- Para cerrar la sesión un usuario de la sesión actual, escriba:
  ```
  logoff
  ```
- Para cerrar una sesión de usuario con el identificador de sesión, por ejemplo, 12 de la sesión, escriba:
  ```
  logoff 12
  ```
- Para cerrar una sesión de usuario con el nombre de la sesión y el servidor, por ejemplo sesión TERM04 en Server1, escriba:
  ```
  logoff TERM04 /server:Server1
  ```

#### <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-   [Servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
