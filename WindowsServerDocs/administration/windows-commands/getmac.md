---
title: getmac
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c770f5da5159e0037af479f90fadb4cd83464c77
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375808"
---
# <a name="getmac"></a>getmac

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Devuelve la dirección Media Access Control (MAC) y la lista de protocolos de red asociados a cada dirección de todas las tarjetas de red de cada equipo, ya sea de forma local o a través de una red. 
## <a name="syntax"></a>Sintaxis
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Parámetros

|             Parámetro              |                                                                                          Descripción                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                       |
|        /u <Domain> @ no__t-1 @ no__t-2         | Ejecuta el comando con los permisos de cuenta del usuario especificado por user o Dominio\usuario. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
|           /p <Password>            |                                                     Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                     |
| /FO {TABLE &#124; List&#124; CSV} |                       Especifica el formato que se va a usar para los resultados de la consulta. Los valores válidos son **TABLE**, **List**y **CSV**. El formato predeterminado de la salida es **TABLE**.                        |
|                /NH                 |                                             Suprime el encabezado de columna en la salida. Válido cuando el parámetro **/FO** está establecido en **TABLE** o **CSV**.                                              |
|                 /v                 |                                                                    Especifica que el resultado muestra información detallada.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Comentarios
**getmac** puede ser útil si desea escribir la dirección Mac en un analizador de red o si necesita saber qué protocolos se están usando actualmente en cada adaptador de red de un equipo.
## <a name="BKMK_Examples"></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **getmac** :
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
