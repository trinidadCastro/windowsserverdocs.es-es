---
title: get Auditpol
description: Tema de los comandos de Windows para **auditpol obtener** -recupera la directiva de sistema, la directiva por usuario, la auditoría de las opciones y objeto de descriptor de seguridad de auditoría.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83aa1d9d193db977dfe3d375476106d7d6f81e85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881546"
---
# <a name="auditpol-get"></a>get Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera la directiva de sistema, la directiva por usuario, la auditoría de las opciones y objeto de descriptor de seguridad de auditoría.

## <a name="syntax"></a>Sintaxis
```
auditpol /get 
[/user[:<username>|<{sid}>]]
[/category:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/subcategory:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/option:<option name>]
[/sd]
[/r]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/user|Muestra a la entidad de seguridad para los que se consulta la directiva de auditoría por usuario. Debe especificarse el parámetro /category o/SubCategory. El usuario puede especificarse como un nombre o identificador de seguridad (SID). Si no se especifica ninguna cuenta de usuario, se consulta la directiva de auditoría del sistema.|
|/Category|Una o varias categorías de auditoría especificadas por nombre o identificador único global (GUID). Un asterisco (*) puede utilizarse para indicar que se deben consultar todas las categorías de auditoría.|
|/subcategory|Una o varias subcategorías de auditoría especificadas por nombre o GUID.|
|/sd|Recupera el descriptor de seguridad que se usa para delegar el acceso a la directiva de auditoría.|
|/Option|Recupera la directiva existente para las opciones CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories.|
|/r|Muestra la salida en formato de informe, valores separados por comas (CSV).|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
Todas las categorías y subcategorías pueden especificarse mediante el GUID o nombre entre comillas. Los usuarios se pueden especificar por nombre o SID.
todas las operaciones get de la directiva de por usuario y la directiva del sistema, debe tener permiso lectura en ese objeto establecido en el descriptor de seguridad. También se pueden realizar las operaciones get que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación get.
## <a name="BKMK_examples"></a>Ejemplos
### <a name="examples-for-the-per-user-audit-policy"></a>Ejemplos de la directiva de auditoría por usuario
Para recuperar la directiva de auditoría por usuario para la cuenta de invitado y mostrar la salida para el sistema de seguimiento detallado y categorías de acceso a objetos, escriba:
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> Este comando es útil en dos escenarios. Al supervisar una cuenta de usuario para la actividad sospechosa, puede usar el comando /get para recuperar los resultados en las categorías específicas mediante el uso de una directiva de inclusión para habilitar la auditoría adicional. O bien, si auditar la configuración en una cuenta de la sesión numerosos pero superfluos eventos, puede usar el comando /get para filtrar los eventos extraños para esa cuenta con una directiva de exclusión. Para obtener una lista de todas las categorías, use el comando de /category auditpol /list.
Para recuperar la directiva de auditoría por usuario para una categoría y una subcategoría particular, que informa de la configuración inclusivo y exclusiva para esa subcategoría en la categoría del sistema para la cuenta de invitado, escriba:
```
auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```
Para mostrar la salida en formato de informe e incluir el nombre del equipo, destino de la directiva, subcategoría, subcategoría GUID, configuración de inclusión y configuración de exclusión, escriba:
```
auditpol /get /user:guest /category:detailed Tracking" /r
```
### <a name="examples-for-the-system-audit-policy"></a>Ejemplos de la directiva de auditoría del sistema
Para recuperar la directiva para el sistema de categorías y subcategorías, que informa de la configuración de directiva de categoría y subcategoría de la directiva de auditoría del sistema, escriba:
```
auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```
Para recuperar la directiva para la categoría de seguimiento detallado y subcategorías en formato de informe e incluir el nombre del equipo, destino de la directiva, subcategoría, subcategoría GUID, la configuración de inclusión y configuración de exclusión, tipo:
```
auditpol /get /category:"detailed Tracking" /r
```
Para recuperar la directiva para dos categorías con las categorías especifica como GUID, que informa de todas las opciones de directiva de auditoría de todas las subcategorías en dos categorías, tipo:
```
auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```
### <a name="examples-for-auditing-options"></a>Ejemplos de opciones de auditoría
Para recuperar el estado, habilitado o deshabilitado, de la opción AuditBaseObjects, escriba:
```
auditpol /get /option:AuditBaseObjects
```
> [!NOTE]
> Las opciones disponibles son AuditBaseObjects, AuditBaseOperations y FullprivilegeAuditing.
Para recuperar el estado habilitado, deshabilitado o 2 de la opción CrashOnAuditFail, escriba:
```
auditpol /get /option:CrashOnAuditFail /r
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
