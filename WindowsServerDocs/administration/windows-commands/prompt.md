---
title: prompt
description: Obtenga información sobre cómo personalizar el símbolo del sistema.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2df80d3af6344644a68b1b2d01ba48fbf41f1581
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372022"
---
# <a name="prompt"></a>prompt



Cambia el símbolo del sistema cmd. exe. Si se usa sin parámetros, **prompt** restablece el símbolo del sistema a la configuración predeterminada, que es la letra de unidad y el directorio actuales seguidos del símbolo mayor que ( **>** ).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
prompt [<Text>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|@no__t 0Text >|Especifica el texto y la información que desea incluir en el símbolo del sistema.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Puede personalizar el símbolo del sistema para mostrar el texto que desee, incluida la información como el nombre del directorio actual, la fecha y la hora, y el número de versión de Microsoft Windows.

En la tabla siguiente se enumeran las combinaciones de caracteres que se pueden incluir en lugar de una o más cadenas de caracteres en el parámetro de *texto* . La lista incluye una breve descripción del texto o la información que cada combinación de caracteres agrega al símbolo del sistema.  

| óptico |                                 Descripción                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = (signo igual)                                |
|    $$     |                               $ (signo de dólar)                               |
|    $t     |                                Hora actual                                 |
|    $d     |                                Fecha actual                                 |
|    $p     |                           Unidad y ruta de acceso actuales                            |
|    $v     |                           Número de versión de Windows                            |
|    $n     |                                Unidad actual                                |
|    $g     |                            > (mayor que signo)                            |
|    $l     |                             < (menor que signo)                              |
|    $b     |                              \| (símbolo de barra vertical)                               |
|    $_     |                               ENTRAR-AVANCE DE LA                                |
|    $e     |                         Código de escape ANSI (código 27)                          |
|    $h     | Retroceso (para eliminar un carácter que se ha escrito en la línea de comandos) |
|    $a     |                                & (y comercial)                                |
|    $c     |                            ((paréntesis izquierdo)                             |
|    $f     |                            ) (paréntesis derecho)                            |
|    $s     |                                    Espacia                                    |

Cuando las extensiones de comando están habilitadas (es decir, el valor predeterminado), el comando **prompt** admite los siguientes caracteres de formato:  

|óptico|Descripción|
|---------|-----------|
|$+|Cero o más caracteres de signo más ( **+** ), en función de la profundidad de la pila de directorios **insertada** (un carácter por cada nivel insertado).|
|$m|Nombre remoto asociado a la letra de unidad actual o cadena vacía si la unidad actual no es una unidad de red.|

Si incluye el carácter **$p** en el parámetro de texto, el disco se lee después de escribir cada comando (para determinar la unidad y la ruta de acceso actuales). Esto puede tardar más tiempo, especialmente en el caso de las unidades de disquete.

## <a name="BKMK_examples"></a>Example

Para establecer un símbolo del sistema de dos líneas con la hora y fecha actuales en la primera línea y el signo mayor que en la línea siguiente, escriba:
```
prompt $d$s$s$t$_$g 
```
El mensaje se cambia como se indica a continuación, donde la fecha y la hora son actuales:
```
Fri 06/01/2007  13:53:28.91
>
```
Para establecer que el símbolo del sistema se muestre como una flecha (`-->`), escriba:
```
prompt --$g
```
Para cambiar manualmente el símbolo del sistema a la configuración predeterminada (la unidad y la ruta de acceso actuales seguidos del signo mayor que), escriba:
```
prompt $p$g
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
