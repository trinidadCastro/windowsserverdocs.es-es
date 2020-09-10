---
title: eventcreate
description: Artículo de referencia para el comando eventcreate, que permite a un administrador crear un evento personalizado en un registro de eventos especificado.
ms.topic: reference
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f23ecbec564a5b578f5f7814314803c95d81bf13
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636084"
---
# <a name="eventcreate"></a>eventcreate

Permite a un administrador crear un evento personalizado en un registro de eventos especificado.

> [!IMPORTANT]
> Los eventos personalizados no se pueden escribir en el registro de seguridad.

## <a name="syntax"></a>Sintaxis

```
eventcreate [/s <computer> [/u <domain\user> [/p <password>]] {[/l {APPLICATION|SYSTEM}]|[/so <srcname>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <eventID> /d <description>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `<domain\user>` | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain\user>` . El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| l `{APPLICATION | SYSTEM}` | Especifica el nombre del registro de eventos donde se creará el evento. Los nombres de registro válidos son **aplicación** o **sistema**. |
| /so `<srcname>` | Especifica el origen que se va a usar para el evento. Un origen válido puede ser cualquier cadena y debe representar la aplicación o el componente que está generando el evento. |
| /t `{ERROR | WARNING | INFORMATION | SUCCESSAUDIT | FAILUREAUDIT}` | Especifica el tipo de evento que se va a crear. Los tipos válidos son **error**, **Warning**, **Information**, **SUCCESSAUDIT**y **FAILUREAUDIT**. |
| /ID `<eventID>` | Especifica el identificador del evento. Un identificador válido es cualquier número comprendido entre 1 y 1000. |
| /d. `<description>` | Especifica la descripción que se va a usar para el evento que se acaba de crear. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra cómo se puede usar el comando **eventcreate** :

```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
