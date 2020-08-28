---
title: auditpol get
description: Artículo de referencia del comando Auditpol get, que recupera la Directiva del sistema, la Directiva por usuario, las opciones de auditoría y el objeto de descriptor de seguridad de auditoría.
ms.topic: reference
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23de44ebc9bc91ad4db52ee7362b14d9c93648d8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029093"
---
# <a name="auditpol-get"></a>auditpol get

> Se aplica a: Windows Server (canal semianual), Windows Server, 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera la Directiva del sistema, la Directiva por usuario, las opciones de auditoría y el objeto de descriptor de seguridad de auditoría.

Para realizar operaciones *Get* en las directivas *por usuario* y *del sistema* , debe tener permiso de **lectura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones *Get* si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite obtener acceso adicional que no es necesario para realizar las operaciones *Get* generales.

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

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /User | Muestra la entidad de seguridad para la que se consulta la Directiva de auditoría por usuario. Se debe especificar el parámetro/Category o/subcategory. El usuario puede especificarse como un identificador de seguridad (SID) o un nombre. Si no se especifica ninguna cuenta de usuario, se consulta la Directiva de auditoría del sistema. |
| /categoría | Una o varias categorías de auditoría especificadas por el identificador único global (GUID) o el nombre. Se puede usar un asterisco (*) para indicar que se deben consultar todas las categorías de auditoría. |
| /subcategory | Una o más subcategorías de auditoría especificadas por el GUID o el nombre. |
| /SD | Recupera el descriptor de seguridad que se usa para delegar el acceso a la Directiva de auditoría. |
| /Option | Recupera la directiva existente para las opciones CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories. |
| /r | Muestra la salida en formato de informe, valor separado por comas (CSV). |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="remarks"></a>Observaciones

Todas las categorías y subcategorías se pueden especificar mediante el GUID o el nombre entre comillas ("). Los usuarios se pueden especificar por SID o nombre.

## <a name="examples"></a>Ejemplos

Para recuperar la Directiva de auditoría por usuario para la cuenta invitado y mostrar la salida de las categorías sistema, seguimiento detallado y acceso a objetos, escriba:

```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:System,detailed Tracking,Object Access
```

> [!NOTE]
> Este comando es útil en dos escenarios. 1) al supervisar una cuenta de usuario específica para actividades sospechosas, puede usar el `/get` comando para recuperar los resultados en categorías específicas mediante el uso de una directiva de inclusión para habilitar la auditoría adicional. 2) si la configuración de auditoría de una cuenta está registrando numerosos eventos superfluos, puede usar el `/get` comando para filtrar los eventos superfluos para esa cuenta con una directiva de exclusión. Para obtener una lista de todas las categorías, use el `auditpol /list /category` comando.

Para recuperar la Directiva de auditoría por usuario para una categoría y una subcategoría determinada, que notifica la configuración inclusiva y exclusiva de esa subcategoría en la categoría sistema de la cuenta invitado, escriba:

```
auditpol /get /user:guest /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Para mostrar el resultado en formato de informe e incluir el nombre del equipo, el destino de la Directiva, la subcategoría, el GUID de la subcategoría, la configuración de inclusión y la configuración de exclusión, escriba:

```
auditpol /get /user:guest /category:detailed Tracking /r
```

Para recuperar la Directiva de la categoría y subcategorías del sistema, que informa de la categoría y de la configuración de la Directiva de subcategoría de la Directiva de auditoría del sistema, escriba:

```
auditpol /get /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Para recuperar la Directiva de la categoría de seguimiento detallada y las subcategorías en formato de informe e incluir el nombre del equipo, el destino de la Directiva, la subcategoría, el GUID de la subcategoría, la configuración de inclusión y la configuración de exclusión, escriba:

```
auditpol /get /category:detailed Tracking /r
```

Para recuperar la Directiva de dos categorías con las categorías especificadas como GUID, que informa de todas las configuraciones de directiva de auditoría de todas las subcategorías en dos categorías, escriba:

```
auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Para recuperar el estado, habilitado o deshabilitado, de la opción AuditBaseObjects, escriba:

```
auditpol /get /option:AuditBaseObjects
```

Donde las opciones disponibles son AuditBaseObjects, AuditBaseOperations y FullprivilegeAuditing. Para recuperar el estado Enabled, Disabled o 2 de la opción CrashOnAuditFail, escriba:

```
auditpol /get /option:CrashOnAuditFail /r
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
