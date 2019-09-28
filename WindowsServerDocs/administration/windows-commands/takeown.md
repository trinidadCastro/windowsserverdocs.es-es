---
title: takeown
description: Obtenga información acerca de cómo obtener acceso a un archivo convirtiéndose en el propietario del archivo.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08804db36357c3d1d1efa7243b338bd85d5c48e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383758"
---
# <a name="takeown"></a>takeown

Permite que un administrador recupere el acceso a un archivo que anteriormente le era denegado, convirtiendo al administrador en el propietario del archivo.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<Computer >|Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificados en el comando.|
|/u [\<Domain > \] @ no__t-2|Ejecuta el script con los permisos de la cuenta de usuario especificada. El valor predeterminado es permisos del sistema.|
|/p [\<Password >]|Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .|
|/f @no__t-nombre de 0File >|Especifica el nombre de archivo o el patrón de nombre de directorio. Puede usar el carácter comodín * al especificar el patrón. También puede usar la sintaxis *ShareName*\*FileName *.|
|/a|Proporciona la propiedad al grupo de administradores en lugar de al usuario actual.|
|/r|Realiza una operación recursiva en todos los archivos del directorio y los subdirectorios especificados.|
|/d {Y \| N}|Suprime el mensaje de confirmación que se muestra cuando el usuario actual no tiene el permiso "lista de carpetas" en un directorio especificado y, en su lugar, utiliza el valor predeterminado especificado. Los valores válidos para la opción **/d** son los siguientes:</br>SÍ Tomar posesión del directorio.</br>N Omitir el directorio.</br>Tenga en cuenta que debe usar esta opción junto con la opción **/r** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Este comando se usa normalmente en archivos por lotes.
-   Si no se especifica el parámetro **/a** , se proporciona la propiedad del archivo al usuario que ha iniciado sesión actualmente en el equipo.
-   Modelos mixtos mediante ( **?** y **&#42;** ) no son compatibles con el comando **takeown** .
-   Después de eliminar el bloqueo con **takeown**, es posible que tenga que usar el explorador de Windows o el comando **cacls** para obtener permisos completos para los archivos y directorios antes de poder eliminarlos. Para obtener más información acerca de **cacls**, vea "referencias adicionales" al final de este tema.

## <a name="BKMK_examples"></a>Example

Para tomar posesión de un archivo denominado Lostfile, escriba:
```
takeown /f lostfile
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)