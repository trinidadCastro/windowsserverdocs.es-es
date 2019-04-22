---
title: prompt
description: Obtenga información sobre cómo personalizar el símbolo del sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 320e12fd30deda30ccc0da1ad6e5bea6f9a19d8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818446"
---
# <a name="prompt"></a>prompt



Cambia el símbolo del sistema Cmd.exe. Si se utiliza sin parámetros, **símbolo del sistema** restablece el símbolo del sistema a la configuración predeterminada, que es la letra de unidad actual y el directorio seguido por el símbolo mayor que (**>**).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
prompt [<Text>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<Text>|Especifica el texto y la información que desea incluir en el símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Puede personalizar el símbolo del sistema para mostrar el texto que desee, incluida información como el nombre del directorio actual, el tiempo y fecha y el número de versión de Microsoft Windows.

En la tabla siguiente se enumera las combinaciones de caracteres que se pueden incluir en lugar de, o además de uno o más cadenas de caracteres en el *texto* parámetro. La lista incluye una breve descripción de la información que cada combinación de caracteres que se agrega a la línea de comandos o de texto.  
|Carácter|Descripción|
|---------|-----------|
|$q|signo igual (=)|
|$$|$ (dólar)|
|$t|Hora actual|
|$d|Fecha actual|
|$p|Ruta de acceso y la unidad actual|
|$v|Número de versión de Windows|
|$n|Unidad actual|
|$g|> (mayor que el inicio de sesión)|
|$l|< (menor que el inicio de sesión)|
|$b|| (barra vertical)|
|$_|ESCRIBA O SALTO DE LÍNEA|
|$e|Código de escape ANSI (código 27)|
|$h|Retroceso (para eliminar un carácter que se ha escrito en la línea de comandos)|
|$a|& (y comercial)|
|$c|((paréntesis de apertura)|
|$f|) (paréntesis)|
|$s|Espacio|

Cuando se habilitan las extensiones de comando (es decir, el valor predeterminado) el **símbolo del sistema** comando admite los caracteres de formato siguientes:  

|Carácter|Descripción|
|---------|-----------|
|$+|Cero o más en el signo (**+**) caracteres, según la profundidad de la **pushd** pila de directorio (un carácter para cada nivel insertado).|
|$m|El nombre remoto asociado con la letra de unidad actual o una cadena vacía si la unidad actual no es una unidad de red.|

Si incluye el **$p** caracteres en el parámetro de texto, se lee el disco después de escribir cada comando (para determinar la unidad actual y la ruta de acceso). Esto puede tardar más tiempo, especialmente para las unidades de disco.

## <a name="BKMK_examples"></a>Ejemplos

Para establecer una línea de comandos de dos líneas con la fecha y hora actual en la primera línea y el signo mayor que en la línea siguiente, escriba:
```
prompt $d$s$s$t$_$g 
```
Cambia el símbolo del sistema como se indica a continuación, donde la fecha y hora están actualizadas:
```
Fri 06/01/2007  13:53:28.91
>
```
Para establecer el símbolo del sistema para mostrar como una flecha (`-->`), escriba:
```
prompt --$g
```
Para cambiar manualmente el símbolo del sistema para la configuración predeterminada (la unidad actual y la ruta de acceso seguido por el signo mayor que), escriba:
```
prompt $p$g
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)