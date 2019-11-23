---
title: llamada
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 0e5f9f2b0102c12ee0925bb434fdeddde85e34cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379721"
---
# <a name="call"></a>llamada



Llama a un programa por lotes desde otro sin detener el programa de Batch primario. El comando **Call** acepta etiquetas como destino de la llamada.

> [!NOTE]
> La **llamada** no tiene ningún efecto en el símbolo del sistema cuando se usa fuera de un script o un archivo por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>Parámetros

|           Parámetro           |                                                                         Descripción                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<> de unidad:] [\<ruta de acceso >]<FileName> | Especifica la ubicación y el nombre del programa por lotes al que desea llamar. El parámetro *filename* es obligatorio y debe tener una extensión. bat o. cmd. |
|      \<BatchParameters >       |                                            Especifica la información de línea de comandos requerida por el programa por lotes.                                             |
|           :\<etiqueta >           |                                            Especifica la etiqueta a la que desea que salte un control de programa por lotes.                                             |
|         \<argumentos >          |                     Especifica la información de la línea de comandos que se va a pasar a la nueva instancia del programa por lotes, a partir de *: etiqueta.*                     |
|              /?               |                                                             Muestra la ayuda en el símbolo del sistema.                                                             |

## <a name="batch-parameters"></a>Parámetros de Batch

En las tablas siguientes se enumeran las referencias de los argumentos del script por lotes ( **%0**, **%1**,...).

**%\*** de un script por lotes hace referencia a todos los argumentos (por ejemplo, **%1**, **%2**, **%3**...)

Puede usar las siguientes sintaxis opcionales como sustituciones para los parámetros de Batch ( **% n**):

|Parámetro batch|Descripción|
|---------------|-----------|
|% ~ 1|Expande **%1** y quita las comillas ("").|
|% ~ F1|Expande **%1** a una ruta de acceso completa.|
|% ~ D1|Expande **%1** solo a una letra de unidad.|
|% ~ P1|Expande **%1** a una ruta de acceso únicamente.|
|% ~ N1|Expande **%1** a un nombre de archivo únicamente.|
|% ~ x1|Expande **%1** a una extensión de nombre de archivo únicamente.|
|% ~ S1|Expande **%1** a una ruta de acceso completa que solo contiene nombres cortos.|
|% ~ a1|Expande **%1** a los atributos de archivo.|
|% ~ T1|Expande **%1** hasta la fecha y hora del archivo.|
|% ~ Z1|Expande **%1** hasta el tamaño del archivo.|
|% ~ $PATH: 1|Busca en los directorios que aparecen en la variable de entorno PATH y expande **%1** hasta el nombre completo del primer directorio encontrado. Si no se define el nombre de la variable de entorno o no se encuentra el archivo en la búsqueda, este modificador se expande a la cadena vacía.|

En la tabla siguiente se muestra cómo se pueden combinar modificadores con los parámetros de lote para resultados compuestos:

|Parámetro de lote con modificador|Descripción|
|-----------------------------|-----------|
|% ~ DP1|Expande **%1** a una letra de unidad y ruta de acceso solamente.|
|% ~ NX1|Expande **%1** a un nombre de archivo y a una extensión únicamente.|
|% ~ DP $ ruta de acceso: 1|Busca en los directorios que aparecen en la variable de entorno PATH de **%1**y, a continuación, se expande a la letra de unidad y la ruta de acceso del primer directorio encontrado.|
|% ~ ftza1|Expande **%1** para mostrar una salida similar al comando **dir** .|

En los ejemplos anteriores, **%1** y path se pueden reemplazar por otros valores válidos. La sintaxis de <strong>%~</strong> finaliza con un número de argumento válido. Los modificadores de <strong>%~</strong> no se pueden usar con **%\\\*** .

## <a name="remarks"></a>Observaciones

-   Usar parámetros de Batch

    Los parámetros de Batch pueden contener cualquier información que se pueda pasar a un programa por lotes, incluidas las opciones de línea de comandos, los nombres de archivo, los parámetros de lote **%0** a **%9**y las variables (por ejemplo, **% de% de baudios**).
-   Usar el parámetro *Label*

    Al usar **Call** con el parámetro *Label* , se crea un nuevo contexto de archivo por lotes y se pasa el control a la instrucción después de la etiqueta especificada. La primera vez que se encuentra el final del archivo por lotes (es decir, después de saltar a la etiqueta), el control vuelve a la instrucción después de la instrucción de **llamada** . La segunda vez que se encuentra el final del archivo por lotes, se sale del script por lotes.
-   Usar canalizaciones y símbolos de redireccionamiento

    No utilice canalizaciones ( **|** ) ni símbolos de redireccionamiento ( **<** o **>** ) con **llamada**.
-   Realización de una llamada recursiva

    Puede crear un programa por lotes que se llame a sí mismo. Sin embargo, debe proporcionar una condición de salida. De lo contrario, los programas de Batch primarios y secundarios pueden repetirse indefinidamente.
-   Trabajar con extensiones de comandos

    Si las extensiones de comando están habilitadas, **Call** acepta la *etiqueta* como destino de la llamada. La sintaxis correcta es la siguiente:

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>Example

Para ejecutar el programa Checknew. bat desde otro programa por lotes, escriba el siguiente comando en el programa de Batch primario:
```
call checknew
```
Si el programa de Batch primario acepta dos parámetros de batch y desea pasarlos a Checknew. bat, escriba el siguiente comando en el programa de Batch primario:
```
call checknew %1 %2
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
