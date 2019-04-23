---
title: conjunto Auditpol
description: 'Tema de los comandos de Windows para **auditpol set** : establece la directiva de auditoría por usuario, la directiva de auditoría del sistema, o bien opciones de auditoría.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9c0fb17620147d2de5b991c1a9a0fb95e782677
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826996"
---
# <a name="auditpol-set"></a>conjunto Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Establece el usuario por la directiva de auditoría, directiva de auditoría del sistema o las opciones de auditoría.

## <a name="syntax"></a>Sintaxis
```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/user|Se establece la entidad de seguridad para los que la directiva especificada por la categoría o subcategoría de auditoría por el usuario. La categoría o subcategoría opción debe especificarse como un nombre o identificador de seguridad (SID).|
|/include|Especificado con/User; indica que la directiva del usuario por usuario provocará una auditoría que se generará incluso si no se especifica mediante la directiva de auditoría del sistema. Esta configuración es el valor predeterminado y se aplica automáticamente si no la / incluye ni /exclude parámetros se especifican explícitamente.|
|/exclude|Especificado con/User; indica que la directiva del usuario por usuario provocará una auditoría se supriman independientemente de la directiva de auditoría del sistema. Este valor se omite para los usuarios que son miembros del grupo Administradores local.|
|/Category|Una o varias categorías de auditoría especificadas por nombre o identificador único global (GUID). Si no se especifica ningún usuario, se establece la directiva del sistema.|
|/subcategory|Una o varias subcategorías de auditoría especificadas por nombre o GUID. Si no se especifica ningún usuario, se establece la directiva del sistema.|
|/success|Especifica la auditoría de acierto. Esta configuración es el valor predeterminado y se aplica automáticamente si se especifican explícitamente el /success ni /failure parámetros. Esta opción debe utilizarse con un parámetro que indica si se habilita o deshabilita a la configuración.|
|/failure|Especifica la auditoría de errores. Esta opción debe utilizarse con un parámetro que indica si se habilita o deshabilita a la configuración.|
|/Option|Establece la directiva de auditoría para las opciones CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories.|
|/sd|Establece el descriptor de seguridad que se usa para delegar el acceso a la directiva de auditoría. El descriptor de seguridad debe especificarse mediante el lenguaje de definición de descriptores de seguridad (SDDL). El descriptor de seguridad debe tener una lista de control de acceso discrecional (DACL).|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
todas las operaciones de conjunto de la directiva de por usuario y la directiva del sistema, se debe escribir o permiso Control total sobre ese objeto se establece en el descriptor de seguridad. También se pueden realizar operaciones de conjunto que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación de establecimiento.
## <a name="BKMK_examples"></a>Ejemplos
### <a name="examples-for-the-per-user-audit-policy"></a>Ejemplos de la directiva de auditoría por usuario
Para establecer el usuario por la directiva de auditoría para todas las subcategorías en la categoría de seguimiento detallada para el usuario de Migueldom para que todos los intentos correctos del usuario que se va a auditar, escriba:
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
Para establecer la directiva de auditoría por usuario para las categorías especificadas por nombre y GUID y subcategorías especificadas por el GUID que se debe suprimir la auditoría para los intentos correctos o con errores, escriba:
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
Para establecer la directiva de auditoría por usuario para el usuario especificado para todas las categorías para la supresión de la auditoría de todos excepto intentos correctos, escriba:
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>Ejemplos de la directiva de auditoría del sistema
Para establecer la directiva de auditoría del sistema para todas las subcategorías en la categoría de seguimiento detallada para incluir la auditoría de intentos correctos solo, escriba:
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> No se modifica la configuración de error.
Para establecer la directiva de auditoría del sistema para las categorías de acceso a objetos y del sistema (que está implícito debido a que se enumeran las subcategorías) y las subcategorías especificadas por el GUID para la supresión de intentos erróneos y auditar los intentos correctos, escriba:
```
auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
```
### <a name="example-for-auditing-options"></a>Ejemplo de opciones de auditoría
Para establecer las opciones de auditoría en el estado habilitado para la opción CrashOnAuditFail, escriba:
```
auditpol /set /option:CrashOnAuditFail /value:enable
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
