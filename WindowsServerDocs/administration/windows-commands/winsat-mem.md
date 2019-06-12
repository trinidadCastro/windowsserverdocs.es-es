---
title: WinSAT optimizado
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cdae81694a916905f36cdd9e941015e3ce5f15c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440084"
---
# <a name="winsat-mem"></a>WinSAT optimizado



Ancho de memoria de las pruebas del sistema de una manera brillante de copias de búfer de memoria a memoria grande, ya que se usan en el procesamiento multimedia.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
winsat mem <parameters>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-seguridad|Forzar la memoria de las pruebas con un solo subproceso. El valor predeterminado es ejecutar un subproceso por cada núcleo o CPU física.|
|-rn|Especificar que los subprocesos de la evaluación deben ejecutarse con una prioridad normal. El valor predeterminado es ejecutarse con prioridad 15.|
|-nc|Especificar que la evaluación debe asignar memoria y lo marca como no almacenada en caché. Esto significa que se omitirán las memorias caché del procesador para operaciones de copia. El valor predeterminado es ejecutar en el espacio en caché.|
|-do \<n>|Especificar la distancia, expresada en bytes, entre el final del búfer de origen y el principio del búfer de destino. El valor predeterminado es de 64 bytes. El desplazamiento de destino permitido máximo es 16MB. Especificar un desplazamiento de destino no válida producirá un error.</br>Nota: Cero es un valor válido para  **\<n >** , pero no los números negativos.|
|-mint \<n>|Especifique el mínimo tiempo de ejecución en segundos para la evaluación. El valor predeterminado es 2.0. El valor mínimo es 1.0. El valor máximo es 30,0.</br>Nota: Especificar un **-mint** valor mayor que el **- maxt** valor cuando los dos parámetros se usan en combinación tendrá como resultado un error.|
|-maxt \<n>|Especifique el máximo tiempo de ejecución en segundos para la evaluación. El valor predeterminado es 5.0. El valor mínimo es 1.0. El valor máximo es 30,0. Si se utiliza en combinación con la **-mint** parámetro, la evaluación comenzará a realizar comprobaciones periódicas de estadísticas de sus resultados después del período de tiempo especificado en **-mint**. Si la estadísticas comprobaciones son correctas, la evaluación finalizará antes del período de tiempo especificado en **- maxt** ha transcurrido. Si la evaluación se ejecuta durante el período de tiempo especificado en **- maxt** sin que satisfacen las comprobaciones de estadísticas, a continuación, la evaluación se finalizar en ese momento y devolver los resultados se ha recopilado.|
|-buffersize \<n >|Especifique el tamaño del búfer que debe usar la prueba de copia de memoria. Dos veces, se asignará esta cantidad por la CPU, que determina la cantidad de datos copiados de un búfer a otro. El valor predeterminado es 16MB. Este valor se redondea al límite de 4 KB más cercano. El valor máximo es 32MB. El valor mínimo es de 4 KB. Especifique un tamaño de búfer no válido provocará un error.|
|-v|Enviar resultado detallado a STDOUT, incluida la información de estado y el progreso. Los errores también se escribirán en la ventana de comandos.|
|-xml \<file name>|Guardar los resultados de la evaluación como el archivo XML especificado. Si el archivo especificado no existe, se sobrescribirá.|
|-idiskinfo|Guardar información acerca de los volúmenes físicos y los discos lógicos como parte de la  **\<SystemConfig >** sección en la salida XML.|
|-iguid|Crear un identificador único global (GUID) en el archivo de salida XML.|
|-Nota "texto de la nota"|Agregue el texto de nota a la  **\<Nota >** sección en el archivo de salida XML.|
|-icn|Incluir el nombre del equipo local en el archivo de salida XML.|
|-eef|Enumerar información de sistema adicionales en el archivo de salida XML.|

## <a name="BKMK_examples"></a>Ejemplos

- El ejemplo siguiente ejecuta la evaluación durante un período mínimo de 4 segundos y no más de 12 segundos, con un tamaño de búfer de 32MB y guardar los resultados en formato XML en el archivo **memtest.xml**.  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Comentarios

-   Pertenencia al grupo Administradores local, o equivalente, es lo mínimo necesario para usar **winsat**. El comando debe ejecutarse desde una ventana de símbolo del sistema con privilegios elevados.
-   Para abrir una ventana de símbolo del sistema con privilegios elevados, haga clic en **iniciar**, haga clic en **Accesorios**, haga clic en **símbolo**y haga clic en **ejecutar como administrador**.

#### <a name="additional-references"></a>Referencias adicionales

