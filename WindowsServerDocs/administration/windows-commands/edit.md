---
title: editar
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 096632005b3e42dd941ccc7c72c08ead1d291b53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873156"
---
# <a name="edit"></a>editar



Inicia el Editor de MS-DOS, que crea y cambia los archivos de texto ASCII.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName> [<FileName2> [...]]|Especifica la ubicación y el nombre de uno o varios archivos de texto ASCII. Si el archivo no existe, el Editor de MS-DOS lo crea. Si el archivo existe, el Editor de MS-DOS lo abre y muestra su contenido en la pantalla. *Nombre de archivo* puede contener caracteres comodín (**&#42;** y **?**). Separe varios nombres de archivo con espacios.|
|/b|Modo monocromático de fuerza, por lo que muestra el Editor de MS-DOS en blanco y negro.|
|/h|Muestra el número máximo de líneas posible para el monitor actual.|
|/r|Carga los archivos en modo de solo lectura.|
|/s|Fuerza el uso de nombres de archivo cortos.|
|\<NNN>|Carga de archivos binarios, ajustando líneas a *NNN* caracteres ancho.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Para obtener ayuda adicional, abra el Editor de MS-DOS y, a continuación, presione la tecla F1.
-   Algunos monitores no admiten la presentación de las teclas de método abreviado de forma predeterminada. Si el monitor no muestra las teclas de método abreviado, utilice **/b**.

## <a name="BKMK_examples"></a>Ejemplos

Para abrir el Editor de MS-DOS, escriba:
```
edit
```
Para crear y editar un archivo denominado newtextfile.txt en el directorio actual, escriba:
```
edit newtextfile.txt
```