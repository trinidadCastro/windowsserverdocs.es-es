---
title: eventcreate
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d575364adbeba9d9ea4da75a0a866bcc02acea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818716"
---
# <a name="eventcreate"></a>eventcreate



Permite a un administrador crear un evento personalizado en un registro de eventos especificado. Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<equipo >|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u \<dominio\nombre de usuario >|Ejecuta el comando con los permisos de cuenta del usuario especificado por \<usuario > o < DOMINIO\nombre de usuario >. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.|
|/p \<contraseña >|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/l {aplicación\|sistema}|Especifica el nombre del registro de eventos donde se creará el evento. Los nombres de registro válido son la aplicación y del sistema.|
|/so \<SrcName>|Especifica el origen que se usará para el evento. Un origen válido puede ser cualquier cadena y debe representar la aplicación o componente que genera el evento.|
|/t {ERROR\|advertencia\|información\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Especifica el tipo de evento va a crear. Los tipos válidos son ERROR, advertencia, información, SUCCESSAUDIT y FAILUREAUDIT.|
|/ ID \<EventID >|Especifica el Id. de evento para el evento. Un identificador válido es cualquier número comprendido entre 1 y 1000.|
|/d \<descripción >|Especifica la descripción que se utilizará para el evento recién creado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Eventos personalizados no se puede escribir en el registro de seguridad.

## <a name="BKMK_examples"></a>Ejemplos

Los ejemplos siguientes muestran cómo puede usar el comando eventcreate:
```
eventcreate /t error /id 100 /l application /d "Create event in application log"
eventcreate /t information /id 1000 /so winmgmt /d "Create event in WinMgmt source"
eventcreate /t error /id 2001 /so winword /l application /d "new src Winword in application log"
eventcreate /s server /t error /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t error /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d "Remote machine with partial user credentials"
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
