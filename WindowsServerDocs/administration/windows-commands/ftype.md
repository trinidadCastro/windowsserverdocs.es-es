---
title: ftype
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c29ca0aa027d11fa8f981134e5367021227d3096
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881686"
---
# <a name="ftype"></a>ftype



Muestra o modifica los tipos de archivo que se usan en las asociaciones de extensión de nombre de archivo. Si se usa sin un operador de asignación (**=**), **ftype** muestra la cadena de comando Abrir actual para el tipo de archivo especificado. Si se utiliza sin parámetros, **ftype** muestra los tipos de archivo que tienen las cadenas de comando Abrir definidas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<FileType>|Especifica el tipo de archivo para mostrar o cambiar.|
|\<OpenCommandString>|Especifica la cadena de comando Abrir que se utilizará al abrir los archivos del tipo de archivo especificado.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

La tabla siguiente describe cómo **ftype** reemplaza las variables en una cadena de comando Abrir:

|Variable|Valor de reemplazo|
|--------|-----------------|
|%0 o %1|Se sustituye por el nombre de archivo que se inicia a través de la asociación.|
|%*|Obtiene todos los parámetros.|
|%2, %3, ...|Obtiene el primer parámetro (%2), el segundo parámetro (%3) y así sucesivamente.|
|%~\<N>|Obtiene todos los parámetros restantes a partir de la *N*parámetro donde *N* puede ser cualquier número de 2 a 9.|

## <a name="BKMK_examples"></a>Ejemplos

Para mostrar los tipos de archivo actual que tienen definidas de cadenas de comando Abrir, escriba:
```
ftype
```
Para mostrar la cadena de comando Abrir actual para el *txtfile* tipo de archivo, tipo:
```
ftype txtfile
```
Este comando genera una salida similar al siguiente:
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Para eliminar la cadena de comando Abrir para un tipo de archivo denominado *ejemplo*, tipo:
```
ftype example=
```
Para asociar la extensión de nombre de archivo .pl con el tipo de archivo PerlScript y habilitar el tipo de archivo PerlScript ejecutar PERL. EXE, escriba los siguientes comandos:
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Para eliminar la necesidad de escribir la extensión de nombre de archivo .pl al invocar una secuencia de comandos Perl, escriba:
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)