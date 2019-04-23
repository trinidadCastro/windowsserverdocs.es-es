---
title: cmdkey
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7080a3ae28fed722f17cf8311aa1e2fd3bece41f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854616"
---
# <a name="cmdkey"></a>cmdkey

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea, enumera y elimina los nombres de usuario y las contraseñas o credenciales almacenados.

## <a name="syntax"></a>Sintaxis
```
cmdkey [{/add:<TargetName>|/generic:<TargetName>}] {/smartcard|/user:<UserName> [/pass:<Password>]} [/delete{:<TargetName>|/ras}] /list:<TargetName>
```
## <a name="parameters"></a>Parámetros
|Parámetros|Descripción|
|-------|--------|
|/add:<TargetName>|Agrega un nombre de usuario y una contraseña a la lista.<br /><br />Requiere que el parámetro de <TargetName> que identifica el nombre de equipo o dominio que se asociará esta entrada.|
|/ genérico:<TargetName>|Agrega credenciales genéricas a la lista.<br /><br />Requiere que el parámetro de <TargetName> que identifica el nombre de equipo o dominio que se asociará esta entrada.|
|/smartcard|Recupera las credenciales de una tarjeta inteligente.|
|/ User:<UserName>|Especifica el usuario o el nombre de cuenta para almacenar con esta entrada. Si *UserName* no es se proporciona, se volverá a solicitarse.|
|/PASS:<Password>|Especifica la contraseña para almacenar con esta entrada. Si *contraseña* no es se proporciona, se volverá a solicitarse.|
|/delete{:<TargetName> &#124; /ras}|elimina un nombre de usuario y una contraseña de la lista. Si *TargetName* se especifica que la entrada se eliminará. Si se especifica /ras, se eliminará la entrada de acceso remoto almacenada.|
|/List:<TargetName>|Muestra la lista de los nombres de usuario y las credenciales. Si *TargetName* no es los nombres de usuario especificado, todo ello almacenado y se mostrarán las credenciales.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Si se encuentra más de una tarjeta inteligente en el sistema cuando se usa la opción de línea de comandos /smartcard, **cmdkey** mostrará información sobre todas las tarjetas inteligentes disponibles y, a continuación, pedir al usuario especificar cuál desea usar.
-   Las contraseñas no se mostrará una vez que se almacenan.
## <a name="BKMK_examples"></a>Ejemplos
Para mostrar una lista de todos los nombres de usuario y las credenciales almacenadas, escriba:
```
cmdkey /list
```
Para agregar un nombre de usuario y contraseña de usuario mikedan tenga acceso al equipo SERVIDOR01 con la contraseña Kleo, escriba:
```
cmdkey /add:server01 /user:mikedan /pass:Kleo
```
Para agregar un nombre de usuario y contraseña de usuario mikedan tenga acceso al equipo SERVIDOR01 y pedir confirmación de la contraseña cada vez que se tiene acceso a Server01, escriba:
```
cmdkey /add:server01 /user:mikedan
```
Para eliminar la credencial que ha almacenado el acceso remoto, escriba:
```
cmdkey /delete /ras
```
Para eliminar la credencial que se almacena para Server01, escriba:
```
cmdkey /delete:Server01
```
## <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
