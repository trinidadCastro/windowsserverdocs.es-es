---
title: finger
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78313fc4980b32e3aeb6d1611ef80d7eb6831fc1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844608"
---
# <a name="finger"></a>finger

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de un usuario o usuarios en un equipo remoto especificado (normalmente, un equipo que ejecuta UNIX) que está ejecutando el demonio o el servicio Finger. El equipo remoto especifica el formato y la salida de la pantalla de información del usuario. Cuando se usa sin parámetros, el **dedo** muestra la ayuda. 
## <a name="syntax"></a>Sintaxis
```
finger [-l] [<User>] [@<Host>] [...]
```
#### <a name="parameters"></a>Parámetros

| Parámetro |                                                                            Descripción                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Muestra información de usuario en formato de lista largo.                                                           |
|  <User>   | Especifica el usuario sobre el que desea obtener información. Si omite el parámetro de *usuario* , el **dedo** muestra información acerca de todos los usuarios del equipo especificado. |
|  @<Host>  |        Especifica el equipo remoto que ejecuta el servicio Finger en el que busca información de usuario. Puede especificar un nombre de equipo o una dirección IP.        |
|    /?     |                                                               Muestra la Ayuda en el símbolo del sistema.                                                                |

## <a name="remarks"></a>Comentarios
Se pueden especificar varios parámetros de User@Host.
Debe **anteponer** un guion (-) en lugar de una barra diagonal (/).
Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.
Windows Server 2003 no proporciona un servicio de Finger.
## <a name="examples"></a><a name=BKMK_Examples></a>Example
Para mostrar información de user1 en el equipo users.microsoft.com, escriba:
```
finger user1@users.microsoft.com
```
Para mostrar información de todos los usuarios del equipo users.microsoft.com, escriba:
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
