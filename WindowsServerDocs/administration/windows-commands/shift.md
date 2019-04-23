---
title: shift
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52b3012a39409f9d48ae8aa7608e7bd0af5787d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858826"
---
# <a name="shift"></a>shift



Cambia la posición de parámetros por lotes en un archivo por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
shift [/n <N>]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/n \<N >|Especifica que comience el desplazamiento en el *N*argumento, donde *N* es cualquier valor entre 0 y 8. Requiere las extensiones de comando, que están habilitadas de forma predeterminada.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **MAYÚS** comando cambia los valores de los parámetros del lote **%0** a través de **%9** mediante la copia de cada parámetro en el anterior, el valor de **%1** se copia en **%0**, el valor de **%2** se copia en **%1**, y así sucesivamente. Esto es útil para escribir un archivo por lotes que realiza la misma operación en cualquier número de parámetros.
-   Si se habilitan las extensiones de comando, el **MAYÚS** comando admite la **/n** opción de línea de comandos. El **/n** opción especifica que comience el desplazamiento en el argumento n, donde **N** es cualquier valor entre 0 y 8. Por ejemplo, **MAYÚS /2** cambiaría **%3** a **%2**, **%4** a **%3**, y así sucesivamente y dejar **%0** y **%1** afectados. Las extensiones de comando están habilitadas de forma predeterminada.
-   Puede usar el **MAYÚS** comando para crear un archivo por lotes que acepte más de 10 parámetros por lotes. Si especifica más de 10 parámetros en la línea de comandos, aquellos que aparecen después del décimo (**%9**) será desplazadas en un momento **%9**.
-   El **MAYÚS** comando no tiene ningún efecto el **% \*** por lotes de parámetro.
-   Hay versiones anteriores no **MAYÚS** comando. Después de implementar el **MAYÚS** comando, no se puede recuperar el parámetro de lote (**%0**) que existían antes de la tecla MAYÚS.

## <a name="BKMK_examples"></a>Ejemplos

Las siguientes líneas de un archivo por lotes de ejemplo denominada Micopia.bat muestran cómo usar **MAYÚS** con cualquier número de parámetros del lote. En este ejemplo, Micopia.bat copia una lista de archivos en un directorio específico. Los parámetros del lote se representan mediante los argumentos de nombre de archivo y directorio.
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)