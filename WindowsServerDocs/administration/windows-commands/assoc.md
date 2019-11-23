---
title: assoc
description: 'Temas de comandos de Windows para **Assoc** : muestra o modifica las asociaciones de extensión de nombre de archivo.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e4a6fd700cbe66897a24f01f66387e76e07b568b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382670"
---
# <a name="assoc"></a>assoc



Muestra o modifica las asociaciones de extensión de nombre de archivo. Si se usa sin parámetros, **Assoc** muestra una lista de todas las asociaciones de extensión de nombre de archivo actuales.

> [!NOTE]
> Este comando solo se admite en CMD. EXE y no está disponible en PowerShell.
>

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|<. ext >|Especifica la extensión de nombre de archivo.|
|\<FileType >|Especifica el tipo de archivo que se va a asociar a la extensión de nombre de archivo especificada.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

-   Para quitar la Asociación de tipo de archivo para una extensión de nombre de archivo, agregue un espacio en blanco después del signo igual presionando la barra ESPACIAdora.
-   Para ver los tipos de archivo actuales que tienen definidas cadenas de comandos abiertas, use el comando **ftype** .
-   Para redirigir la salida de **Assoc** a un archivo de texto, utilice el operador de redireccionamiento de **>** .

## <a name="BKMK_examples"></a>Example

Para ver la Asociación de tipo de archivo actual para la extensión de nombre de archivo. txt, escriba:
```
assoc .txt
```
Para quitar la Asociación de tipo de archivo para la extensión de nombre de archivo. bak, escriba:
```
assoc .bak= 
```

> [!NOTE]
> Asegúrese de agregar un espacio después del signo igual.

Para ver la salida de **Assoc** en una pantalla a la vez, escriba:
```
assoc | more
```
Para enviar la salida de **Assoc** al archivo Assoc. txt, escriba:
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
