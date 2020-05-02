---
title: finger
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec8040480a7cb75a5a42e051393e3db4a47f8e2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725616"
---
# <a name="finger"></a>finger

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
|    /?     |                                                               Muestra la ayuda en el símbolo del sistema.                                                                |

## <a name="remarks"></a>Observaciones
Se User@Host pueden especificar varios parámetros.
Debe **anteponer** un guion (-) en lugar de una barra diagonal (/).
Este comando solo está disponible si el Protocolo Protocolo de Internet (TCP/IP) se instala como componente en las propiedades de un adaptador de red en las conexiones de red.
Windows Server 2003 no proporciona un servicio de Finger.
## <a name="examples"></a>Ejemplos
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
