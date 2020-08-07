---
title: whoami
description: Artículo de referencia para whoami, que muestra información de usuarios, grupos y privilegios para el usuario que ha iniciado sesión actualmente en el sistema local.
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdcef4072fe692f2717fe79814af926a2c151636
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896491"
---
# <a name="whoami"></a>whoami



Muestra información de usuarios, grupos y privilegios para el usuario que ha iniciado sesión actualmente en el sistema local. Si se usa sin parámetros, **whoami** muestra el dominio y el nombre de usuario actuales.



## <a name="syntax"></a>Sintaxis

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/upn|Muestra el nombre de usuario en formato de nombre principal de usuario (UPN).|
|/fqdn|Muestra el nombre de usuario en formato de nombre de dominio completo (FQDN).|
|/logonid|Muestra el ID. de inicio de sesión del usuario actual.|
|/User|Muestra el dominio y el nombre de usuario actuales y el identificador de seguridad (SID).|
|/groups|Muestra los grupos de usuarios a los que pertenece el usuario actual.|
|/priv|Muestra los privilegios de seguridad del usuario actual.|
|/FO\<Format>|Especifica el formato de salida. Los valores válidos son:</br>**tabla** de Muestra la salida en una tabla. Este es el valor predeterminado.</br>**lista** de Muestra la salida en una lista.</br>**CSV** Muestra la salida en formato de valores separados por comas (CSV).|
|/all|Muestra toda la información del token de acceso actual, incluidos el nombre de usuario actual, los identificadores de seguridad (SID), los privilegios y los grupos a los que pertenece el usuario actual.|
|/NH|Especifica que el encabezado de columna no debe mostrarse en la salida. Esto solo es válido para los formatos de tabla y CSV.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para mostrar el dominio y el nombre de usuario de la persona que ha iniciado sesión actualmente en este equipo, escriba:
```
whoami
```
Verá un resultado similar al siguiente:
```
DOMAIN1\administrator
```
Para mostrar toda la información del token de acceso actual, escriba:
```
whoami /all
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)