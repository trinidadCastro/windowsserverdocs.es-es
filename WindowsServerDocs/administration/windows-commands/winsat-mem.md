---
title: winsat mem
description: Artículo de referencia de la memoria de WinSAT, que prueba el ancho de banda de la memoria del sistema de forma que se refleje la memoria grande en las copias de búfer de memoria, como se usa en el procesamiento multimedia.
winms.topic: reference
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 88a8ff7bf8cac5197532950d59d8c736d229f846
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641103"
---
# <a name="winsat-mem"></a>winsat mem



Comprueba el ancho de banda de la memoria del sistema de forma que se refleje la memoria grande en las copias de búfer de memoria, como se usa en el procesamiento multimedia.



## <a name="syntax"></a>Sintaxis

```
winsat mem <parameters>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|-arriba|Fuerce la prueba de memoria con un solo subproceso. El valor predeterminado es ejecutar un subproceso por CPU física o núcleo.|
|-RN|Especifica que los subprocesos de la evaluación deben ejecutarse con prioridad normal. El valor predeterminado es ejecutar con la prioridad 15.|
|-NC|Especifique que la evaluación debe asignar memoria y marcarla como sin almacenamiento en caché. Esto significa que las cachés del procesador se omitirán para las operaciones de copia. El valor predeterminado es ejecutar en el espacio en caché.|
|-do \<n>|Especifica la distancia, en bytes, entre el final del búfer de origen y el inicio del búfer de destino. El valor predeterminado es 64 bytes. El desplazamiento de destino máximo permitido es de 16 MB. Si se especifica un desplazamiento de destino no válido, se producirá un error.</br>Nota: cero es un valor válido para **\<n>** , pero no números negativos.|
|-menta \<n>|Especifique el tiempo de ejecución mínimo en segundos para la evaluación. El valor predeterminado es 2,0. El valor mínimo es 1,0. El valor máximo es 30,0.</br>Nota: si se especifica un valor **-menta** mayor que el valor **-maxt** cuando los dos parámetros se usan en combinación, se producirá un error.|
|-maxt \<n>|Especifique el tiempo de ejecución máximo en segundos para la evaluación. El valor predeterminado es 5,0. El valor mínimo es 1,0. El valor máximo es 30,0. Si se usa en combinación con el parámetro **-menta** , la evaluación comenzará a realizar comprobaciones estadísticas periódicas de los resultados después del período de tiempo especificado en **-menta**. Si se superan las comprobaciones estadísticas, la evaluación finalizará antes de que transcurra el período de tiempo especificado en **-maxt** . Si la evaluación se ejecuta durante el período de tiempo especificado en **-maxt** sin satisfacer las comprobaciones estadísticas, la evaluación finalizará en ese momento y devolverá los resultados que ha recopilado.|
|-buffersize \<n>|Especifique el tamaño de búfer que debe usar la prueba de copia de memoria. Dos veces esta cantidad se asignará por CPU, lo que determina la cantidad de datos copiados de un búfer a otro. El valor predeterminado es 16 MB. Este valor se redondea al límite de 4 KB más próximo. El valor máximo es 32 MB. El valor mínimo es 4 KB. Si se especifica un tamaño de búfer no válido, se producirá un error.|
|-v|Envíe una salida detallada a STDOUT, incluida la información de estado y de progreso. Los errores también se escribirán en la ventana de comandos.|
|-XML \<file name>|Guarde el resultado de la evaluación como el archivo XML especificado. Si el archivo especificado existe, se sobrescribirá.|
|-idiskinfo|Guarde la información sobre los volúmenes físicos y los discos lógicos como parte de la **\<SystemConfig>** sección de la salida XML.|
|-iguid|Cree un identificador único global (GUID) en el archivo de salida XML.|
|-Nota texto de nota|Agregue el texto de la nota a la **\<note>** sección del archivo de salida XML.|
|-icn|Incluya el nombre del equipo local en el archivo de salida XML.|
|-eef|Enumerar información adicional del sistema en el archivo de salida XML.|

## <a name="examples"></a>Ejemplos

- Para ejecutar la evaluación durante un mínimo de 4 segundos y no más de 12 segundos, use un tamaño de búfer de 32 MB y guarde los resultados en formato XML en el archivo **memtest.xml**.
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Observaciones

-   La pertenencia al grupo local Administradores, o equivalente, es lo mínimo necesario para usar **WinSAT**. El comando debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados.
-   Para abrir una ventana del símbolo del sistema con privilegios elevados, haga clic en **Inicio**, **accesorios**, haga clic con el botón secundario en **símbolo del sistema**y haga clic en **Ejecutar como administrador**.

## <a name="additional-references"></a>Referencias adicionales

