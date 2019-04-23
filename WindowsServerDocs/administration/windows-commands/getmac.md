---
title: getmac
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e356354e63a057201582db0fb74933e1b3ef8d8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851046"
---
# <a name="getmac"></a>getmac

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Devuelve los medios tener acceso a la dirección MAC (control) y la lista de protocolos de red asociados con cada dirección de todas las tarjetas de red en cada equipo, ya sea localmente o a través de una red. 
## <a name="syntax"></a>Sintaxis
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/s <computer>|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u <Domain>\\<User>|Ejecuta el comando con los permisos de cuenta del usuario especificado por el usuario o dominio\usuario. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.|
|/p <Password>|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/FO {tabla &#124; lista&#124; CSV}|Especifica el formato que se usará para la salida de la consulta. Los valores válidos son **tabla**, **lista**, y **CSV**. El formato predeterminado para la salida es **tabla**.|
|/nh|Suprime el encabezado de columna de salida. Es válido cuando el **/fo** parámetro está establecido en **tabla** o **CSV**.|
|/v|Especifica que la salida muestra información detallada.|
|/?||
## <a name="remarks"></a>Comentarios
**GETMAC** puede ser útil cuando desea escribir la dirección MAC en un analizador de red o cuando necesite saber qué protocolos están actualmente en uso en cada adaptador de red en un equipo.
## <a name="BKMK_Examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **getmac** comando:
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
