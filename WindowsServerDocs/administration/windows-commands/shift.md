---
title: desplazamiento
description: Tema de referencia de Shift, que cambia la posición de los parámetros de lote en un archivo por lotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 870bda19de3426fd7007020efb2f3db39bf654c8
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433169"
---
# <a name="shift"></a>desplazamiento

Cambia la posición de los parámetros de lote en un archivo por lotes.



## <a name="syntax"></a>Sintaxis

```
shift [/n <N>]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/n \< n>|Especifica que se inicie el desplazamiento en el argumento *n*, donde *N* es cualquier valor de 0 a 8. Requiere extensiones de comando, que están habilitadas de forma predeterminada.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

- El **comando Shift** cambia los valores de los parámetros de **lote %0** a **%9** copiando cada parámetro en el anterior: el valor de **%1** se copia en **%0**, el valor de **%2** se copia en **%1**, y así sucesivamente. Esto resulta útil para escribir un archivo por lotes que realice la misma operación en cualquier número de parámetros.
- Si se habilitan las extensiones de comando, el comando **Shift** admite la opción de línea de comandos **/n** . La opción **/n** especifica que se inicie el desplazamiento en el argumento n, donde **n** es cualquier valor comprendido entre 0 y 8. Por ejemplo, **Shift/2** desplazaría **%3** a **%2**, **%4** a **%3**, etc., y deje **%0** y **%1** no afectados. Las extensiones de comando están habilitadas de forma predeterminada.
- Puede usar el comando **Shift** para crear un archivo por lotes que acepte más de 10 parámetros de lote. Si especifica más de 10 parámetros en la línea de comandos, los que aparecen después del décimo (**%9**) se desplazarán de uno en uno a **%9**.
- El comando **Shift** no tiene ningún efecto en el **%\*** parámetro batch.
- No hay ningún comando de **desplazamiento** hacia atrás. Después de implementar el comando **Shift** , no se puede recuperar el parámetro de lote (**%0**) que existía antes del cambio.

## <a name="examples"></a>Ejemplos

Las siguientes líneas de un archivo por lotes de ejemplo denominado "cópiela. bat" demuestran cómo usar **Shift** con cualquier número de parámetros de batch. En este ejemplo, el archivo de copia. bat copia una lista de archivos en un directorio específico. Los parámetros de lote se representan mediante los argumentos de nombre de archivo y directorio.
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if %1== goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
