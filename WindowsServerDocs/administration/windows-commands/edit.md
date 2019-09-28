---
title: editar
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a51a81a0ed2d28a30e8ec221d5719968dce48ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377621"
---
# <a name="edit"></a>editar



Inicia el editor de MS-DOS, que crea y cambia archivos de texto ASCII.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive >:] [<Path>] <FileName> [<FileName2> [...]]|Especifica la ubicación y el nombre de uno o más archivos de texto ASCII. Si el archivo no existe, el editor de MS-DOS lo crea. Si el archivo existe, el editor de MS-DOS lo abre y muestra su contenido en la pantalla. *Filename* puede contener caracteres comodín ( **&#42;** y **?** ). Separe varios nombres de archivo con espacios.|
|b|Fuerza el modo monocromo, de modo que el editor de MS-DOS se muestre en blanco y negro.|
|/h|Muestra el número máximo de líneas posibles para el monitor actual.|
|/r|Carga archivos en modo de solo lectura.|
|/s|Fuerza el uso de nombres de archivo cortos.|
|@NO__T 0NNN >|Carga los archivos binarios, ajustando las líneas a *nnn* caracteres de ancho.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para obtener ayuda adicional, abra el editor de MS-DOS y presione la tecla F1.
-   Algunos monitores no admiten la presentación de teclas de método abreviado de forma predeterminada. Si el monitor no muestra las teclas de método abreviado, use **/b**.

## <a name="BKMK_examples"></a>Example

Para abrir el editor de MS-DOS, escriba:
```
edit
```
Para crear y editar un archivo denominado newtextfile. txt en el directorio actual, escriba:
```
edit newtextfile.txt
```