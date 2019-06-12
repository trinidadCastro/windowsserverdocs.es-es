---
title: llamada
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1f5253700f2932b2afa725163121e64ea4c1748d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434580"
---
# <a name="call"></a>llamada



Llama a un programa por lotes desde otro sin detener el programa principal. El **llamar** comando acepta etiquetas como destino de la llamada.

> [!NOTE]
> **Llamar a** no tiene ningún efecto en el símbolo del sistema cuando se utiliza fuera de un secuencia de comandos o archivo por lotes.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>Parámetros

|           Parámetro           |                                                                         Descripción                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:][<Path>]<FileName> | Especifica la ubicación y el nombre del programa por lotes que se desea llamar. El *FileName* el parámetro es obligatorio y debe tener una extensión .bat o. cmd. |
|      \<BatchParameters>       |                                            Especifica la información de línea de comandos requerido por el programa por lotes.                                             |
|           :\<Etiqueta >           |                                            Especifica la etiqueta que desee saltar a un control del programa por lotes.                                             |
|         \<argumentos >          |                     Especifica la información de línea de comandos que se pasarán a la nueva instancia del programa por lotes, comenzando por *: etiqueta.*                     |
|              /?               |                                                             Muestra la ayuda en el símbolo del sistema.                                                             |

## <a name="batch-parameters"></a>Parámetros del lote

Las referencias de argumentos de script por lotes ( **%0**, **%1**,...) se enumeran en las tablas siguientes.

**%** * en un lote script hace referencia a todos los argumentos (por ejemplo, **%1**, **%2**, **%3**...)

Puede usar las siguientes sintaxis opcionales como sustituciones de parámetros por lotes ( **%n**):

|Parámetro de lote|Descripción|
|---------------|-----------|
|%~1|Se expande **%1** y quita que rodean las comillas ("").|
|%~f1|Se expande **%1** a una ruta de acceso completa.|
|%~d1|Se expande **%1** a solo una letra de unidad.|
|%~p1|Se expande **%1** a solo una ruta de acceso.|
|%~n1|Se expande **%1** a solo un nombre de archivo.|
|%~x1|Se expande **%1** a sólo una extensión de nombre de archivo.|
|%~s1|Se expande **%1** a una ruta de acceso completa que contiene solo los nombres cortos.|
|%~a1|Se expande **%1** para los atributos de archivo.|
|%~t1|Se expande **%1** a la fecha y hora del archivo.|
|%~z1|Se expande **%1** al tamaño del archivo.|
|%~$PATH:1|Busca los directorios mostrados en la variable de entorno PATH y expande **%1** en el nombre completo de la primera se encuentra el directorio. Si el nombre de variable de entorno no está definido o no se encuentra el archivo en la búsqueda, este modificador se expande en una cadena vacía.|

En la tabla siguiente se muestra cómo se pueden combinar los modificadores con los parámetros del lote para resultados compuestos:

|Parámetro de proceso por lotes con el modificador|Descripción|
|-----------------------------|-----------|
|%~dp1|Se expande **%1** a una letra de unidad y ruta de acceso de solo.|
|% ~ nx1|Se expande **%1** sólo a un nombre de archivo y la extensión.|
|%~dp$PATH:1|Busca en los directorios mostrados en la variable de entorno PATH para **%1**y, a continuación, se expande a la letra de unidad y ruta de acceso de la primera se encuentra el directorio.|
|%~ftza1|Se expande **%1** para mostrar un resultado similar a la **dir** comando.|

En los ejemplos anteriores, **%1** y ruta de acceso puede ser reemplazado por otros valores válidos. El <strong>%~</strong> sintaxis termina con un número de argumento válido. El <strong>%~</strong> modificadores no se puede usar con ** %\\***.

## <a name="remarks"></a>Comentarios

-   Uso de parámetros de proceso por lotes

    Parámetros del lote pueden contener cualquier información que se puede pasar a un programa por lotes, incluidas las opciones de línea de comandos, los nombres de archivo, los parámetros del lote **%0** a través de **%9**y las variables (por ejemplo, **baudios %** ).
-   Mediante el *etiqueta* parámetro

    Mediante el uso de **llamar** con el *etiqueta* parámetro, cree un nuevo contexto de archivo por lotes y pasar el control a la instrucción después de la etiqueta especificada. La primera vez que se encuentra el final del archivo por lotes (es decir, después saltar a la etiqueta), el control vuelve a la instrucción después de la **llamar** instrucción. La segunda vez que se encuentra el final del archivo por lotes, se cierra la secuencia de comandos por lotes.
-   Mediante canalizaciones y los símbolos de redirección

    No utilice canalizaciones ( **|** ) y símbolos de redirección ( **<** o **>** ) con **llamar a**.
-   Realizar una llamada recursiva

    Puede crear un programa por lotes que llama a sí mismo. Sin embargo, debe proporcionar una condición de salida. En caso contrario, los programas de lote primarios y secundarios pueden bucle sin fin.
-   Trabajar con las extensiones de comando

    Si se habilitan las extensiones de comando, **llamar** acepta *etiqueta* como destino de la llamada. La sintaxis correcta es como sigue:

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>Ejemplos

Para ejecutar el programa Checknew.bat desde otro programa por lotes, escriba el siguiente comando en el programa principal:
```
call checknew
```
Si el programa principal acepta dos parámetros de proceso por lotes y desea que se puedan pasar los parámetros al programa Checknew.bat, escriba el siguiente comando en el programa principal:
```
call checknew %1 %2
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)