---
title: typeperf
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1337066f547367b381a531dbab6627ad78d280ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848456"
---
# <a name="typeperf"></a>typeperf



El **typeperf** comando escribe los datos de rendimiento a la ventana de comandos o en un archivo de registro. Para detener **typeperf**, presione CTRL+C.

Para obtener ejemplos de cómo usar **typeperf**, consulte [ejemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxis

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|\<counter [counter […]]>|Especifica los contadores de rendimiento para supervisar.|

> [!NOTE]
> **\<contador >** es el nombre completo de un contador de rendimiento en  *\\ \\Computer\Object (instancia) \Counter* dar formato, como  **\\ \\Server1\ Processor(0)\% tiempo de usuario**.

## <a name="options"></a>Opciones

|Opción|Descripción|
|---------|-----------|
|-?|Contextual muestra la Ayuda.|
|-f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL >|Especifica el formato de archivo de salida. El valor predeterminado es CSV.|
|-cf \<filename>|Especifica un archivo que contiene una lista de contadores de rendimiento para supervisar, con un contador por línea.|
|-si <[[hh:]mm:]ss>|Especifica el intervalo de muestra. El valor predeterminado es un segundo.|
|-o \<filename>|Especifica la ruta de acceso para el archivo de salida o la base de datos SQL. El valor predeterminado es STDOUT (escrito en la ventana de comandos).|
|-q [object]|Mostrar una lista de contadores instalados (sin instancias). Para enumerar los contadores de un objeto, incluya el nombre del objeto. EJEMPLO|
|-qx [object]|Mostrar una lista de contadores instalados con instancias. Para enumerar los contadores de un objeto, incluya el nombre del objeto.|
|-sc \<ejemplos >|Especifica el número de muestras que desea recopilar. El valor predeterminado es recopilar los datos hasta que se presiona CTRL+C.|
|-config \<nombreDeArchivo >|Especifica un archivo de configuración que contiene las opciones de comando.|
|-s \<computer_name>|Especifica un equipo remoto para supervisar si se especifica ningún equipo en la ruta de acceso de contador.|
|-y|Responda Sí a todas las preguntas sin preguntar.|

## <a name="BKMK_EXAMPLES"></a>Ejemplos

-   El ejemplo siguiente escribe los valores de contador de rendimiento del equipo local  **\\ \\procesador (_Total)\% tiempo de procesador** a la ventana de comandos en un intervalo de muestreo predeterminado de 1 segundo hasta que se presiona CTRL+C.  
    ```
    typeperf "\Processor)_Total)\% Processor Time"
    ```  
-   El ejemplo siguiente escribe los valores de la lista de contadores en el archivo **counters.txt** en el archivo delimitado por tabuladores **dominio2.tsv** en un intervalo de muestra de 5 segundos hasta que se han recopilado 50 muestras.  
    ```
    typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
    ```  
-   El ejemplo siguiente se consulta contadores instalados con instancias del objeto de contador **PhysicalDisk** y escribe la lista resultante al archivo **counters.txt**.  
    ```
    typeperf -qx PhysicalDisk -o counters.txt
    ```
