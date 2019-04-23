---
title: whoami
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6844ba001c2ebd7407b77f97204069a48a1b595b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840156"
---
# <a name="whoami"></a>whoami



Muestra información de usuario, grupo y los privilegios del usuario que ha iniciado sesión actualmente en el sistema local. Si se utiliza sin parámetros, **whoami** muestra el nombre de dominio y usuario actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/upn|Muestra el nombre de usuario en formato de nombre principal (UPN) del usuario.|
|/fqdn|Muestra el nombre de usuario en formato de nombre de dominio completo (FQDN) del nombre.|
|/LOGONID|Muestra el identificador de inicio de sesión del usuario actual.|
|/user|Muestra el nombre de dominio y usuario actual y el identificador de seguridad (SID).|
|/ Groups|Muestra los grupos de usuarios al que pertenece el usuario actual.|
|/priv|Muestra los privilegios de seguridad del usuario actual.|
|/Fo \<formato >|Especifica el formato de salida. Los valores válidos se incluyen:</br>**tabla** muestra la salida en una tabla. Este es el valor predeterminado.</br>**lista** muestra la salida en una lista.</br>**CSV** muestra la salida en formato de valores separados por comas (CSV).|
|/ all|Muestra toda la información en el token de acceso actual, incluidos el nombre de usuario actual, los identificadores de seguridad (SID), privilegios y grupos a los que pertenece el usuario actual.|
|/nh|Especifica que el encabezado de columna no debe mostrarse en la salida. Esto sólo es válido para los formatos CSV y de tabla.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar el nombre de usuario y dominio de la persona que ha iniciado sesión actualmente en este equipo, escriba:
```
whoami
```
Aparecerá un resultado similar al siguiente:
```
DOMAIN1\administrator
```
Para mostrar toda la información en el token de acceso actual, escriba:
```
whoami /all
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)