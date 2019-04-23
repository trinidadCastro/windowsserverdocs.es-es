---
title: takeown
description: Obtenga información sobre cómo obtener acceso a un archivo convirtiéndose en el propietario del archivo.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b5a4874edf9fa4406d4643e686fed2b725699dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854366"
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
|/s \<equipo >|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local. Este parámetro se aplica a todos los archivos y carpetas especificadas en el comando.|
|/u [\<Domain>\]<User name>|Ejecuta la secuencia de comandos con los permisos de la cuenta de usuario especificado. El valor predeterminado es permisos del sistema.|
|/p [\<Password>]|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/f \<nombre de archivo >|Especifica el nombre de archivo o directorio patrón. Puede usar el carácter comodín * al especificar el patrón. También puede usar la sintaxis *ShareName*\*FileName *.|
|/a|Asigna la propiedad al grupo de administradores en lugar del usuario actual.|
|/r|Realiza una operación recursiva en todos los archivos del directorio especificado y los subdirectorios.|
|/d {Y \| N}|Suprime el mensaje de confirmación que se muestra cuando el usuario actual no tiene el permiso "Mostrar lista de carpetas" en un directorio especificado y en su lugar utiliza el valor predeterminado especificado. Los valores válidos para el **/d** opción son los siguientes:</br>-Y: Tomar posesión del directorio.</br>-N: Omitir el directorio.</br>Tenga en cuenta que debe usar esta opción junto con la **/r** opción.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Este comando se usa normalmente en archivos por lotes.
-   Si el **/a** parámetro no se especifica, la propiedad de archivo se otorga al usuario que ha iniciado sesión actualmente en el equipo.
-   ¿Patrones mixtos mediante (**?** y **&#42;**) no son compatibles con **takeown** comando.
-   Después de eliminar el bloqueo con **takeown**, es posible que deba usar el Explorador de Windows o el **cacls** comando para el Concédase permisos completos para los archivos y directorios antes de que se pueden eliminar. Para obtener más información acerca de **cacls**, consulte "Referencias adicionales" al final de este tema.

## <a name="BKMK_examples"></a>Ejemplos

Para tomar posesión de un archivo denominado Archivo_perdido, escriba:
```
takeown /f lostfile
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)