---
title: finger
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e16120eb19ff2f194fe2c8bdeb3af80ca459ebe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377165"
---
# <a name="finger"></a>finger

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de un usuario o usuarios en un equipo remoto especificado (normalmente, un equipo que ejecuta UNIX) que está ejecutando el demonio o el servicio Finger. El equipo remoto especifica el formato y la salida de la pantalla de información del usuario. Cuando se usa sin parámetros, el **dedo** muestra la ayuda. 
## <a name="syntax"></a>Sintaxis
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>Parámetros

| Parámetro |                                                                            Descripción                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Muestra información de usuario en formato de lista largo.                                                           |
|  <User>   | Especifica el usuario sobre el que desea obtener información. Si omite el parámetro de *usuario* , el **dedo** muestra información acerca de todos los usuarios del equipo especificado. |
|  @<Host>  |        Especifica el equipo remoto que ejecuta el servicio Finger en el que busca información de usuario. Puede especificar un nombre de equipo o una dirección IP.        |
|    /?     |                                                               Muestra la ayuda en el símbolo del sistema.                                                                |

## <a name="remarks"></a>Comentarios
Se pueden especificar varios parámetros User@Host.
Debe **anteponer** un guion (-) en lugar de una barra diagonal (/).
Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.
Windows Server 2003 no proporciona un servicio de Finger.
## <a name="BKMK_Examples"></a>Example
Para mostrar información de user1 en el equipo users.microsoft.com, escriba:
```
finger user1@users.microsoft.com
```
Para mostrar información de todos los usuarios del equipo users.microsoft.com, escriba:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
