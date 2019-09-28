---
title: AuditPol resourceSACL
description: 'Temas de comandos de Windows para **Uditpol resourceSACL** : configura las listas de control de acceso (SACL) del sistema de recursos globales.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28771ba7-967a-45e9-9bf0-b2a2673070f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2acffd75298f0f36a9c15e0622816feaae57cb64
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382428"
---
# <a name="auditpol-resourcesacl"></a>AuditPol resourceSACL



Configura las listas de control de acceso (SACL) del sistema de recursos globales.

> [!NOTE]
> Solo se aplica a Windows 7 y Windows Server 2008 R2.

Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
auditpol /resourceSACL
[/set /type:<resource> [/success] [/failure] /user:<user> [/access:<access flags>]]
[/remove /type:<resource> /user:<user> [/type:<resource>]]
[/clear [/type:<resource>]]
[/view [/user:<user>] [/type:<resource>]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/Set|Agrega una nueva entrada a o actualiza una entrada existente en la SACL de recursos para el tipo de recurso especificado.|
|/remove|Quita todas las entradas del usuario determinado en la lista de auditoría de acceso a objetos global.|
|/Clear|Quita todas las entradas de la lista de auditoría de acceso a objetos global.|
|/View|Muestra las entradas de auditoría de acceso a objetos global en una SACL de recurso. Los tipos de usuario y de recurso son opcionales.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="arguments"></a>Argumentos

|Argumento|Descripción|
|--------|-----------|
|/Type|Recurso para el que se está configurando la auditoría de acceso a objetos. Los valores de argumento admitidos son archivo (para directorios y archivos) y clave (para las claves del registro).</br>Nota: Los valores de archivo y clave distinguen mayúsculas de minúsculas.|
|/Success|Especifica la auditoría de aciertos.|
|/Failure|Especifica la auditoría de errores.|
|/User|Especifica un usuario en uno de los siguientes formatos:</br>-DomainName\Account (por ejemplo, DOM\Administrators)</br>-StandaloneServer\Group cuenta (consulte la [función LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x se expresa en formato decimal y todo el SID debe ir entre llaves); por ejemplo: {S-1-5-21-5624481-130208933-164394174-1001}</br>    Nota:     Si se usa el formulario de SID, no se realiza ninguna comprobación para comprobar la existencia de esta cuenta.|
|/access|Especifica una máscara de permisos que se puede especificar en una de las dos formas siguientes:</br>-Una secuencia de derechos simples:</br>    -Derechos de acceso genéricos:</br>        -GA-GENÉRICO TODO</br>        -GR: LECTURA GENÉRICA</br>        -GW-ESCRITURA GENÉRICA</br>        -GX-EJECUCIÓN GENÉRICA</br>    -Derechos de acceso para archivos:</br>        -FA-ARCHIVO TODO EL ACCESO</br>        -FR-LECTURA DE ARCHIVO GENÉRICO</br>        -ESCRITURA GENÉRICA DE ARCHIVO DE FW</br>        -FX-ARCHIVO GENÉRICO EXECUTE</br>    -Derechos de acceso para las claves del registro:</br>        -KA-CLAVE TODO EL ACCESO</br>        -KR-CLAVE LEÍDA</br>        -KW-ESCRITURA DE CLAVES</br>        -KX-CLAVE EXECUTE</br>    Por ejemplo: '/Access: FRFW ' habilitará los eventos de auditoría para las operaciones de lectura y escritura</br>: Un valor hexadecimal que representa la máscara de acceso (por ejemplo, 0x1200a9).</br>    Esto resulta útil cuando se usan máscaras de bits específicas del recurso que no forman parte del estándar del lenguaje de definición de descriptores de seguridad (SDDL). Si se omite, se usa el acceso completo.|

## <a name="remarks"></a>Comentarios

En el caso de las operaciones de resourceSACL, debe tener el permiso de control total o de escritura en ese objeto establecido en el descriptor de seguridad. También puede realizar operaciones de resourceSACL con el derecho de usuario **Administrar registro de auditoría y de seguridad** (SeSecurityPrivilege). Sin embargo, este derecho permite el acceso adicional que no es necesario para realizar la operación de eliminación.

## <a name="BKMK_Examples"></a>Example

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

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)