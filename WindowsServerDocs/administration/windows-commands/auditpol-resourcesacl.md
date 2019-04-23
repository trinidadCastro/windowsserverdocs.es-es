---
title: resourceSACL Auditpol
description: Tema de los comandos de Windows para **uditpol resourceSACL** -configura listas de control de acceso de recursos globales del sistema (SACL).
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 375f37250404dd6740027cb18959697626c1ffc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837496"
---
# <a name="auditpol-resourcesacl"></a>resourceSACL Auditpol



Configura las listas de control de acceso de recursos globales del sistema (SACL).

> [!NOTE]
> Solo se aplica a Windows 7 y Windows Server 2008 R2.

Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

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
|/set|Agrega una nueva entrada a o actualiza una entrada existente en la SACL de recursos para el recurso de tipo especificado.|
|/remove|Quita todas las entradas del usuario determinado en la lista de auditoría el acceso a objetos global.|
|/ clear|Quita todas las entradas de la auditoría de la lista el acceso a objetos global.|
|/View|Enumera las entradas de auditoría de acceso a objetos global en un recurso de la SACL. Los tipos de usuario y los recursos son opcionales.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="arguments"></a>Argumentos

|Argumento|Descripción|
|--------|-----------|
|/type|El recurso para el objeto de auditoría de acceso se está configurando. Los valores de argumento admitidos son archivo (para archivos y directorios) y la clave (para las claves del registro).</br>Nota: Los valores de archivo y la clave distinguen mayúsculas de minúsculas.|
|/success|Especifica la auditoría de acierto.|
|/failure|Especifica la auditoría de errores.|
|/user|Especifica un usuario en una de las formas siguientes:</br>-DomainName\Account (por ejemplo, DOM\Administrators)</br>-StandaloneServer\Group cuenta (consulte [función LookupAccountName](https://msdn.microsoft.com/library/windows/desktop/aa379159(v=vs.85).aspx))</br>-{S-1-x-x-x-x} (x se expresa en formato decimal, y el SID completo debe incluirse entre llaves); Por ejemplo: {S-1-5-21-5624481-130208933-164394174-1001}</br>    Nota:     Si se usa el formulario de SID, no se realiza ninguna comprobación para comprobar la existencia de esta cuenta.|
|/access|Especifica una máscara de permisos que se puede especificar en uno de dos formas:</br>-Una secuencia de permisos sencillas:</br>    -Derechos de acceso genéricos:</br>        -GA - TODO GENÉRICO</br>        -GR - GENÉRICO DE LECTURA</br>        -ESCRIBIR GW - GENÉRICO</br>        -EJECUTAR GX - GENÉRICO</br>    -Derechos de acceso para archivos:</br>        -FA - TODO EL ACCESO DE ARCHIVO</br>        LEER ARCHIVO - FR - GENÉRICO</br>        -EC: ESCRITURA GENÉRICA EN ARCHIVO</br>        EJECUTAR - FX - ARCHIVO GENÉRICO</br>    -Derechos de acceso para las claves del registro:</br>        -KA - TODO EL ACCESO DE CLAVE</br>        CLAVE - KR - LEER</br>        -KW - CLAVE ESCRITURA</br>        -   KX - KEY EXECUTE</br>    Por ejemplo: ' / FRFW: acceso a "habilitará los eventos de auditoría para lectura y las operaciones de escritura</br>-Un valor hexadecimal que representa la máscara de acceso (por ejemplo, 0x1200a9)</br>    Esto es útil cuando se usa la máscara de bits de específicos de los recursos que no forman parte de lo estándar de (seguridad SDDL) de lenguaje de definición de descriptor de seguridad. Si se omite, se utiliza acceso completo.|

## <a name="remarks"></a>Comentarios

Para las operaciones de resourceSACL, debe tener permiso de escritura o Control total en ese objeto establecido en el descriptor de seguridad. También se pueden realizar operaciones resourceSACL que poseen el **Administrar registro de auditoría y seguridad** derecho de usuario (SeSecurityPrivilege). Sin embargo, este derecho permite acceso adicional que no es necesario realizar la operación de eliminación.

## <a name="BKMK_Examples"></a>Ejemplos

Para establecer un recurso global SACL para auditar el acceso correcto intentos por el usuario en una clave del registro:
```
auditpol /resourceSACL /set /type:Key /user:MYDOMAIN\myuser /success
```
Para establecer un recurso global SACL para auditar los intentos correctos e incorrectos de un usuario a realizar la lectura genérica y escribir funciones en archivos o carpetas:
```
auditpol /resourceSACL /set /type:File /user:MYDOMAIN\myuser /success /failure /access:FRFW
```
Para quitar todas las entradas de la SACL de recursos globales de archivos o carpetas:
```
auditpol /resourceSACL /type:File /clear
```
Para quitar todas las entradas de la SACL de recursos global para un usuario determinado de archivos o carpetas:
```
auditpol /resourceSACL /remove /type:File /user:{S-1-5-21-56248481-1302087933-1644394174-1001}
```
Para mostrar el acceso a objetos global establecido en archivos o carpetas de entradas de auditoría:
```
auditpol /resourceSACL /type:File /view
```
Para obtener una lista del objeto global, tener acceso a las entradas de auditoría para un usuario determinado que se establecen en los archivos o carpetas:
```
auditpol /resourceSACL /type:File /view /user:MYDOMAIN\myuser
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)