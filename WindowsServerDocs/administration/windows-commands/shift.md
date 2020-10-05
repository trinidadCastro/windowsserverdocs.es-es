---
title: desplazamiento
description: Artículo de referencia del comando Shift, que cambia la posición de los parámetros de lote en un archivo por lotes.
ms.topic: reference
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4553826aa7c3def335ae1d9ec9594ee9d878fc63
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718342"
---
# <a name="shift"></a>desplazamiento

Cambia la posición de los parámetros de lote en un archivo por lotes.

## <a name="syntax"></a>Sintaxis

```
shift [/n <N>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /n `<N>` | Especifica que se inicie el desplazamiento en el argumento *n*, donde *N* es cualquier valor de *0* a *8*. Requiere extensiones de comando, que están habilitadas de forma predeterminada. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Observaciones

- El **comando Shift** cambia los valores de los parámetros de **lote %0** a **%9** copiando cada parámetro en el anterior: el valor de **%1** se copia en **%0**, el valor de **%2** se copia en **%1**, y así sucesivamente. Esto resulta útil para escribir un archivo por lotes que realice la misma operación en cualquier número de parámetros.

- Si se habilitan las extensiones de comando, el comando **Shift** admite la opción de línea de comandos **/n** . La opción **/n** especifica que se inicie el desplazamiento en el argumento n, donde **n** es cualquier valor comprendido entre 0 y 8. Por ejemplo, **Shift/2** desplazaría **%3** a **%2**, **%4** a **%3**, etc., y deje **%0** y **%1** no afectados. Las extensiones de comando están habilitadas de forma predeterminada.

- Puede usar el comando **Shift** para crear un archivo por lotes que acepte más de 10 parámetros de lote. Si especifica más de 10 parámetros en la línea de comandos, los que aparecen después del décimo (**%9**) se desplazarán de uno en uno a **%9**.

- El comando **Shift** no tiene ningún efecto en el **%\*** parámetro batch.

- No hay ningún comando de **desplazamiento** hacia atrás. Después de implementar el comando **Shift** , no se puede recuperar el parámetro de lote (**%0**) que existía antes del cambio.

## <a name="examples"></a>Ejemplos

Para usar un archivo por lotes, denominado *Mycopy.bat*, para copiar una lista de archivos en un directorio específico, escriba:

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
