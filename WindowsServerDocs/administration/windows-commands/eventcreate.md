---
title: eventcreate
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797298622ba1021caef3d04e2f2f06f016ef6a70
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725761"
---
# <a name="eventcreate"></a>eventcreate



Permite a un administrador crear un evento personalizado en un registro de eventos especificado. 

## <a name="syntax"></a>Sintaxis

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<equipo>|Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u \<dominio\usuario>|Ejecuta el comando con los permisos de cuenta del usuario especificado por \<> de usuario o <dominio\usuario>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.|
|/p \<contraseña>|Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .|
|/l {APPLICATION\|System}|Especifica el nombre del registro de eventos donde se creará el evento. Los nombres de registro válidos son aplicación y sistema.|
|/so \<SrcName>|Especifica el origen que se va a usar para el evento. Un origen válido puede ser cualquier cadena y debe representar la aplicación o el componente que está generando el evento.|
|/t {información\|de\|ADVERTENCIA de error\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Especifica el tipo de evento que se va a crear. Los tipos válidos son ERROR, WARNING, INFORMATION, SUCCESSAUDIT y FAILUREAUDIT.|
|/ID \<EventID>|Especifica el identificador del evento. Un identificador válido es cualquier número comprendido entre 1 y 1000.|
|/d \<Descripción>|Especifica la descripción que se va a usar para el evento que se acaba de crear.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Los eventos personalizados no se pueden escribir en el registro de seguridad.

## <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra cómo se puede usar el comando eventcreate:
```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

#### <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
