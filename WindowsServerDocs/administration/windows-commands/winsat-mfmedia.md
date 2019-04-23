---
title: WinSAT mfmedia
description: Comandos de Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69ce4fac127a6af8a94f3800d62c45989cf7020b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845436"
---
# <a name="winsat-mfmedia"></a>WinSAT mfmedia



Mide el rendimiento de decodificación de vídeo (reproducción) mediante el marco de trabajo de Media Foundation.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>Parámetros

|Parámetros|Descripción|
|----------|-----------|
|-entrada \<nombre de archivo >|Obligatorio: Especifique el archivo que contiene el clip de vídeo que se va a reproducir o codificado. El archivo puede ser cualquier formato que se puede representar por Media Foundation.|
|-dumpgraph|Especifique que se debe guardar el gráfico de filtro a un archivo GraphEdit compatible antes de que comience la evaluación.|
|-ns|Especifica que debe ejecutarse el gráfico de filtro a la velocidad de reproducción normal del archivo de entrada. De forma predeterminada, el gráfico de filtro se ejecuta tan rápido como sea posible, omitiendo los tiempos de presentación.|
|-play|Ejecución en la evaluación de descodificar modo y reproducir cualquier proporcionado en el archivo especificado en el contenido de audio **-entrada** con el dispositivo de DirectSound predeterminado. De forma predeterminada, está deshabilitada la reproducción de audio.|
|-nopmp|No hacer uso de la Media Foundation Protected Media canalización (MFPMP) proceso durante la evaluación.|
|-pmp|Siempre hacen uso de la MFPMP procesos durante la evaluación.</br>Nota: Si **- pmp** o **- nopmp** no se especifica, se usará MFPMP solo cuando sea necesario.|
|-v|Enviar resultado detallado a STDOUT, incluida la información de estado y el progreso. Los errores también se escribirán en la ventana de comandos.|
|-xml \<file name>|Guardar los resultados de la evaluación como el archivo XML especificado. Si el archivo especificado no existe, se sobrescribirá.|
|-idiskinfo|Guardar información acerca de los volúmenes físicos y los discos lógicos como parte de la  **\<SystemConfig >** sección en la salida XML.|
|-iguid|Crear un identificador único global (GUID) en el archivo de salida XML.|
|-Nota "texto de la nota"|Agregue el texto de nota a la  **\<Nota >** sección en el archivo de salida XML.|
|-icn|Incluir el nombre del equipo local en el archivo de salida XML.|
|-eef|Enumerar información de sistema adicionales en el archivo de salida XML.|

## <a name="BKMK_examples"></a>Ejemplos

-   En el siguiente ejemplo se ejecuta la evaluación con el archivo de entrada que se usa durante una **winsat formal** evaluación, sin emplear la Media Foundation Protected Media canalización (MFPMP), en un equipo donde c:\windows es la ubicación de la carpeta Windows.  
    ```
    winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
    ```

## <a name="remarks"></a>Comentarios

-   Pertenencia al grupo Administradores local, o equivalente, es lo mínimo necesario para usar **winsat**. El comando debe ejecutarse desde una ventana de símbolo del sistema con privilegios elevados.
-   Para abrir una ventana de símbolo del sistema con privilegios elevados, haga clic en **iniciar**, haga clic en **Accesorios**, haga clic en **símbolo**y haga clic en **ejecutar como administrador**.

#### <a name="additional-references"></a>Referencias adicionales

