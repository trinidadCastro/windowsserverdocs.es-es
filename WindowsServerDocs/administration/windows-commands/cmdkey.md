---
title: cmdkey
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fcd68ee-a14a-4b71-9300-c3f5c5d31e8e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2b12cb53eef930d05c1e291de5574a8ba94306
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379312"
---
# <a name="cmdkey"></a>cmdkey

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, enumera y elimina los nombres de usuario y las contraseñas o credenciales almacenados.

## <a name="syntax"></a>Sintaxis
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Parámetros

|             Parámetros             |                                                                                    Descripción                                                                                     |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /Add: <TargetName>          | agrega un nombre de usuario y una contraseña a la lista.<br /><br />Requiere el parámetro de <TargetName>, que identifica el nombre del equipo o dominio con el que se asociará esta entrada. |
|       /Generic: <TargetName>        |   agrega credenciales genéricas a la lista.<br /><br />Requiere el parámetro de <TargetName>, que identifica el nombre del equipo o dominio con el que se asociará esta entrada.    |
|             /SmartCard             |                                                                    Recupera la credencial de una tarjeta inteligente.                                                                     |
|          /User: <UserName>          |                                 Especifica el nombre de usuario o de cuenta que se va a almacenar con esta entrada. Si no se proporciona el *nombre de usuario* , se le solicitará.                                  |
|          /Pass: <Password>          |                                       Especifica la contraseña que se va a almacenar con esta entrada. Si no se proporciona la *contraseña* , se le solicitará.                                        |
| /Delete{: <TargetName> &#124; /ras} |  elimina un nombre de usuario y una contraseña de la lista. Si se especifica *targetName* , se eliminará dicha entrada. Si se especifica/ras, se eliminará la entrada de acceso remoto almacenada.   |
|         /List: <TargetName>         |                  Muestra la lista de nombres de usuario y credenciales almacenados. Si no se especifica *targetName* , se mostrarán todos los nombres de usuario y las credenciales almacenados.                   |
|                 /?                 |                                                                        Muestra la ayuda en el símbolo del sistema.                                                                        |

## <a name="remarks"></a>Comentarios
- Si se encuentra más de una tarjeta inteligente en el sistema cuando se usa la opción de línea de comandos/SmartCard, **cmdkey** mostrará información acerca de todas las tarjetas inteligentes disponibles y, a continuación, solicitará al usuario que especifique cuál desea usar.
- Las contraseñas no se mostrarán una vez almacenadas.
  ## <a name="BKMK_examples"></a>Example
  Para mostrar una lista de todos los nombres de usuario y las credenciales que se almacenan, escriba:
  ```
  cmdkey /list
  ```
  Para agregar un nombre de usuario y una contraseña para el usuario Mikedan para acceder al equipo Server01 con la contraseña KLEO, escriba:
  ```
  cmdkey /add:server01 /user:mikedan /pass:Kleo
  ```
  Para agregar un nombre de usuario y una contraseña para el usuario Mikedan para acceder al equipo Server01 y solicitar la contraseña cada vez que se tenga acceso a Server01, escriba:
  ```
  cmdkey /add:server01 /user:mikedan
  ```
  Para eliminar la credencial que ha almacenado el acceso remoto, escriba:
  ```
  cmdkey /delete /ras
  ```
  Para eliminar la credencial almacenada para Server01, escriba:
  ```
  cmdkey /delete:Server01
  ```
  ## <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
