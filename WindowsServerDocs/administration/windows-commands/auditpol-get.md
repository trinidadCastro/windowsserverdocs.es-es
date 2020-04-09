---
title: obtención de Auditpol
description: Tema de comandos de Windows para **Auditpol Get**, que recupera la Directiva del sistema, la Directiva por usuario, las opciones de auditoría y el objeto de descriptor de seguridad de auditoría.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe2b1bd060f128e39fa1c687ec963c964798fe1b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851198"
---
# <a name="auditpol-get"></a>obtención de Auditpol

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera la Directiva del sistema, la Directiva por usuario, las opciones de auditoría y el objeto de descriptor de seguridad de auditoría.

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
| /category | Una o varias categorías de auditoría especificadas por el identificador único global (GUID) o el nombre. Se puede usar un asterisco (*) para indicar que se deben consultar todas las categorías de auditoría. |
| /subcategory | Una o más subcategorías de auditoría especificadas por el GUID o el nombre. |
| /SD | Recupera el descriptor de seguridad que se usa para delegar el acceso a la Directiva de auditoría. |
| /Option | Recupera la directiva existente para las opciones CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects o AuditBasedirectories. |
| /r | Muestra la salida en formato de informe, valor separado por comas (CSV). |
| /? | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

Todas las categorías y subcategorías se pueden especificar mediante el GUID o el nombre entre comillas ("). Los usuarios se pueden especificar por SID o nombre.
para todas las operaciones GET de la Directiva por usuario y la Directiva del sistema, debe tener permiso de lectura en ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones GET con el derecho de usuario **Administrar registro de seguridad y auditoría** (SeSecurityPrivilege). Sin embargo, este derecho permite obtener acceso adicional que no es necesario para realizar la operación get.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para recuperar la Directiva de auditoría por usuario para la cuenta invitado y mostrar la salida de las categorías sistema, seguimiento detallado y acceso a objetos, escriba:

```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:System,detailed Tracking,Object Access
```

> [!NOTE]
> Este comando es útil en dos escenarios. Al supervisar una cuenta de usuario específica para actividades sospechosas, puede usar el comando/Get para recuperar los resultados en categorías específicas mediante una directiva de inclusión para habilitar la auditoría adicional. O bien, si la configuración de auditoría de una cuenta está registrando numerosos eventos superfluos, puede usar el comando/Get para filtrar los eventos superfluos para esa cuenta con una directiva de exclusión. Para obtener una lista de todas las categorías, use el comando Auditpol/List/Category.

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
