---
title: takeown
description: Artículo de referencia del comando takeown, que permite a un administrador recuperar el acceso a un archivo que se denegó previamente.
ms.topic: reference
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 34fce5000a0a8c91123a2ebd4765bf4cbb00a289
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718242"
---
# <a name="takeown"></a>takeown

Permite que un administrador recupere el acceso a un archivo que anteriormente le era denegado, convirtiendo al administrador en el propietario del archivo. Este comando se usa normalmente en archivos por lotes.

## <a name="syntax"></a>Sintaxis

```
takeown [/s <computer> [/u [<domain>\]<username> [/p [<password>]]]] /f <filename> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el nombre del equipo local. Este parámetro se aplica a todos los archivos y carpetas especificados en el comando. |
| /u `[<domain>\]<username>` | Ejecuta el script con los permisos de la cuenta de usuario especificada. El valor predeterminado es permisos del sistema. |
| /p `[<[password>]` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /f `<filename>` | Especifica el nombre de archivo o el patrón de nombre de directorio. Puede usar el carácter comodín `*` al especificar el patrón. También puede usar la sintaxis `<sharename>\<filename>` . |
| /a | Proporciona la propiedad al grupo de administradores en lugar de al usuario actual. Si no especifica esta opción, la propiedad del archivo se asigna al usuario que ha iniciado sesión en el equipo. |
| /r | Realiza una operación recursiva en todos los archivos del directorio y los subdirectorios especificados. |
| /d. `{Y | N}` | Suprime el mensaje de confirmación que se muestra cuando el usuario actual no tiene el permiso **List Folder** en un directorio especificado y, en su lugar, utiliza el valor predeterminado especificado. Los valores válidos para la opción **/d** son:<ul><li>**Y** : toma la propiedad del directorio.</li><li>**N** -omitir el directorio.<p>**NOTA**:<br>Debe usar esta opción junto con la opción **/r** .</li></ul> |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

- Modelos mixtos mediante (**?** y **&#42;**) no son compatibles con el comando **takeown** .

- Después de eliminar el bloqueo con **takeown**, es posible que tenga que usar el explorador de Windows para conceder permisos completos a los archivos y directorios antes de poder eliminarlos.

## <a name="examples"></a>Ejemplos

Para tomar posesión de un archivo denominado *Lostfile*, escriba:

```
takeown /f lostfile
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
