---
title: AuditPol resourceSACL
description: Tema de referencia del comando Auditpol resourceSACL, que configura las listas de control de acceso (SACL) del sistema de recursos globales.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: befb76a21880171740b051c987dfd4d9329ecc9e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719048"
---
# <a name="auditpol-resourcesacl"></a>AuditPol resourceSACL

> Se aplica a: Windows 7 y Windows Server 2008 R2

Configura las listas de control de acceso (SACL) del sistema de recursos globales.

Para realizar operaciones de *resourceSACL* , debe tener permisos de **control total** o de **escritura** para ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de *resourceSACL* si tiene el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege).

## <a name="syntax"></a>Sintaxis

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /set | Agrega una nueva entrada a o actualiza una entrada existente en la SACL de recursos para el tipo de recurso especificado. |
| /remove | Quita todas las entradas del usuario determinado en la lista de auditoría de acceso a objetos global. |
| /Clear | Quita todas las entradas de la lista de auditoría de acceso a objetos global.|
| /View | Muestra las entradas de auditoría de acceso a objetos global en una SACL de recurso. Los tipos de usuario y de recurso son opcionales. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="arguments"></a>Argumentos

| Argumento | Descripción |
| -------- | ----------- |
| /type | Recurso para el que se está configurando la auditoría de acceso a objetos. Los valores de argumento admitidos, con distinción de mayúsculas y minúsculas, son *archivo* (para directorios y archivos) y *clave* (para las claves del registro). |
| /Success | Especifica la auditoría de aciertos. |
| /Failure | Especifica la auditoría de errores. |
| /User | Especifica un usuario en uno de los siguientes formatos:<ul><li> DomainName\Account (por ejemplo, DOM\Administrators)</li><li>Cuenta StandaloneServer\Group (consulte la [función LookupAccountName](https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-lookupaccountnamea))</li><li>{S-1-x-x-x-x} (x se expresa en formato decimal y todo el SID debe ir entre llaves). Por ejemplo: {S-1-5-21-5624481-130208933-164394174-1001}<p>**Nota:** Si se usa el formulario de SID, no se realiza ninguna comprobación para comprobar la existencia de esta cuenta.</li></ul> |
| /access | Especifica una máscara de permisos que se puede especificar mediante:<p>Derechos de acceso genéricos, entre los que se incluyen:<ul><li>GA-GENÉRICO TODO</li><li>GR: LECTURA GENÉRICA</li><li>GW: ESCRITURA GENÉRICA</li><li>GX: EJECUCIÓN GENÉRICA</li></ul><p>Derechos de acceso para archivos, incluidos:<ul><li>FA-ARCHIVO TODO EL ACCESO</li><li>FR: LECTURA GENÉRICA DE ARCHIVO</li><li>ESCRITURA GENÉRICA DE ARCHIVO DE FW</li><li>FX-ARCHIVO GENÉRICO EXECUTE</li></ul><p>Derechos de acceso para las claves del registro, incluidos:<ul><li>KA-CLAVE TODOS ACCESO</li><li>KR-CLAVE DE LECTURA</li><li>KW-ESCRITURA DE CLAVES</li><li>EJECUCIÓN DE LA CLAVE KX</li></ul><p>Por ejemplo: `/access:FRFW` habilita los eventos de auditoría para las operaciones de lectura y escritura.<p>Un valor hexadecimal que representa la máscara de acceso (por ejemplo, 0x1200a9).<p>Esto resulta útil cuando se usan máscaras de bits específicas del recurso que no forman parte del estándar del lenguaje de definición de descriptores de seguridad (SDDL). Si se omite, se usa el acceso completo. |

## <a name="examples"></a>Ejemplos

Para establecer una SACL de recursos globales para auditar los intentos de acceso correctos de un usuario en una clave del registro:

```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```

Para establecer una SACL de recursos globales para auditar los intentos correctos y erróneos de un usuario para realizar funciones genéricas de lectura y escritura en archivos o carpetas:

```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```

Para quitar todas las entradas SACL de recursos globales de los archivos o carpetas:

```
auditpol /resourceSACL /type:File /clear
```

Para quitar todas las entradas de SACL de recursos globales para un usuario determinado de archivos o carpetas:

```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```

Para enumerar las entradas de auditoría de acceso a objetos globales establecidas en archivos o carpetas:

```
auditpol /resourceSACL /type:File /view
```

Para enumerar las entradas de auditoría de acceso a objetos globales para un usuario determinado que se establecen en archivos o carpetas:

```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
