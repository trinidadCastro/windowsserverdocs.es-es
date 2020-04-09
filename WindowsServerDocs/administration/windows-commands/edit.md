---
title: editar
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dfffefbe113bc5df242a00357c769aaa9c1c787
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845228"
---
# <a name="edit"></a>editar



Inicia el editor de MS-DOS, que crea y cambia archivos de texto ASCII.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> de unidad:] [<Path>]<FileName> [<FileName2> [...]]|Especifica la ubicación y el nombre de uno o más archivos de texto ASCII. Si el archivo no existe, el editor de MS-DOS lo crea. Si el archivo existe, el editor de MS-DOS lo abre y muestra su contenido en la pantalla. *Filename* puede contener caracteres comodín ( **&#42;** y **?** ). Separe varios nombres de archivo con espacios.|
|/b|Fuerza el modo monocromo, de modo que el editor de MS-DOS se muestre en blanco y negro.|
|/h|Muestra el número máximo de líneas posibles para el monitor actual.|
|/r|Carga archivos en modo de solo lectura.|
|/s|Fuerza el uso de nombres de archivo cortos.|
|\<NNN >|Carga los archivos binarios, ajustando las líneas a *nnn* caracteres de ancho.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para obtener ayuda adicional, abra el editor de MS-DOS y presione la tecla F1.
-   Algunos monitores no admiten la presentación de teclas de método abreviado de forma predeterminada. Si el monitor no muestra las teclas de método abreviado, use **/b**.

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para abrir el editor de MS-DOS, escriba:
```
edit
```
Para crear y editar un archivo denominado newtextfile. txt en el directorio actual, escriba:
```
edit newtextfile.txt
```