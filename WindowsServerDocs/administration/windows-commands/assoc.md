---
title: assoc
description: Tema de los comandos de Windows para **assoc** -muestra o modifica las asociaciones de extensión de nombre de archivo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d893167081b66c81366b59613c52182a4ddba370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816646"
---
# <a name="assoc"></a>assoc



Muestra o modifica las asociaciones de extensión de nombre de archivo. Si se utiliza sin parámetros, **assoc** muestra una lista de todos los nombre de la extensión asociaciones de archivo actuales.

> [!NOTE]
> Este comando solo se admite dentro de CMD. EXE y no está disponible en PowerShell.
>

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|<.ext>|Especifica la extensión de nombre de archivo.|
|\<FileType>|Especifica el tipo de archivo para asociarlo con la extensión de nombre de archivo especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para quitar la asociación de tipo de archivo para una extensión de nombre de archivo, agregue un espacio en blanco después del signo igual presionando la barra espaciadora.
-   Para ver los tipos de archivo actual que tienen las cadenas de comando Abrir definidas, use el **ftype** comando.
-   Para redirigir la salida de **assoc** en un archivo de texto, use la **>** operador de redirección.

## <a name="BKMK_examples"></a>Ejemplos

Para ver la asociación de tipo de archivo actual de la extensión de nombre de archivo .txt, escriba:
```
assoc .txt
```
Para quitar la asociación de tipo de archivo para la extensión de nombre de archivo .bak, escriba:
```
assoc .bak= 
```

> [!NOTE]
> No olvide agregar un espacio después del signo igual.

Para ver la salida de **assoc** una pantalla a la vez, tipo:
```
assoc | more
```
Para enviar el resultado de **assoc** a assoc.txt de archivo, escriba:
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
