---
title: ftype
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f982f68f25a4decbc9c572b533fa1ecc5e893a8c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842708"
---
# <a name="ftype"></a>ftype



Muestra o modifica los tipos de archivo que se usan en las asociaciones de extensión de nombre de archivo. Si se utiliza sin un operador de asignación ( **=** ), **ftype** muestra la cadena de comando abierta actual para el tipo de archivo especificado. Si se usa sin parámetros, **ftype** muestra los tipos de archivo que tienen definidas cadenas de comandos abiertas.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<FileType >|Especifica el tipo de archivo que se va a mostrar o cambiar.|
|\<OpenCommandString >|Especifica la cadena de comandos abierta que se va a usar al abrir archivos del tipo de archivo especificado.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

En la tabla siguiente se describe cómo **ftype** sustituye las variables dentro de una cadena de comandos abierta:

|Variable|Valor de reemplazo|
|--------|-----------------|
|%0 o %1|Se sustituye por el nombre de archivo que se inicia a través de la asociación.|
|%*|Obtiene todos los parámetros.|
|%2, %3,...|Obtiene el primer parámetro (%2), el segundo parámetro (%3), etc.|
|%~\<N >|Obtiene todos los parámetros restantes a partir del parámetro *n*, donde *N* puede ser cualquier número comprendido entre 2 y 9.|

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar los tipos de archivo actuales que tienen definidas cadenas de comandos abiertas, escriba:
```
ftype
```
Para mostrar la cadena de comando abierta actual para el tipo de archivo *TXTfile* , escriba:
```
ftype txtfile
```
Este comando produce un resultado similar al siguiente:
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Para eliminar la cadena de comandos Open para un tipo de archivo llamado *example*, escriba:
```
ftype example=
```
Para asociar la extensión de nombre de archivo. pl con el tipo de archivo PerlScript y habilitar el tipo de archivo PerlScript para ejecutar PERL. EXE, escriba los siguientes comandos:
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Para eliminar la necesidad de escribir la extensión de nombre de archivo. pl al invocar un script Perl, escriba:
```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)