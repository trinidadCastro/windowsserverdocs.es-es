---
title: winsat mfmedia
description: Referencia de Mfmedia de WinSAT, que mide el rendimiento de la descodificación de vídeo (reproducción) mediante el marco de Media Foundation.
ms.topic: reference
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 889ef018e5803f9905100b5ae0b65f1bc0c4093e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038103"
---
# <a name="winsat-mfmedia"></a>winsat mfmedia



Mide el rendimiento de la descodificación de vídeo (reproducción) mediante el marco de Media Foundation.



## <a name="syntax"></a>Sintaxis

```
winsat mfmedia <parameters>
```

### <a name="parameters"></a>Parámetros

|Parámetros|Descripción|
|----------|-----------|
|-Input \<file name>|Requerido: especifique el archivo que contiene el clip de vídeo que se va a reproducir o codificar. El archivo puede estar en cualquier formato que se pueda representar mediante Media Foundation.|
|-dumpgraph|Especifique que el gráfico de filtro debe guardarse en un archivo compatible con GraphEdit antes de que se inicie la evaluación.|
|-NS|Especifica que el gráfico de filtros debe ejecutarse en la velocidad de reproducción normal del archivo de entrada. De forma predeterminada, el gráfico de filtros se ejecuta lo más rápido posible, omitiendo los tiempos de presentación.|
|-reproducir|Ejecutar la evaluación en modo de descodificación y reproducir cualquier contenido de audio proporcionado en el archivo especificado en la **entrada** mediante el dispositivo DirectSound predeterminado. De forma predeterminada, la reproducción de audio está deshabilitada.|
|-nopmp|No use el proceso Media Foundation canalización de medios protegidos (MFPMP) durante la evaluación.|
|-PMP|Haga siempre el uso del proceso MFPMP durante la evaluación.</br>Nota: Si no se especifica **-PMP** o **-nopmp** , se usará MFPMP solo cuando sea necesario.|
|-v|Envíe una salida detallada a STDOUT, incluida la información de estado y de progreso. Los errores también se escribirán en la ventana de comandos.|
|-XML \<file name>|Guarde el resultado de la evaluación como el archivo XML especificado. Si el archivo especificado existe, se sobrescribirá.|
|-idiskinfo|Guarde la información sobre los volúmenes físicos y los discos lógicos como parte de la **\<SystemConfig>** sección de la salida XML.|
|-iguid|Cree un identificador único global (GUID) en el archivo de salida XML.|
|-Nota texto de nota|Agregue el texto de la nota a la **\<note>** sección del archivo de salida XML.|
|-icn|Incluya el nombre del equipo local en el archivo de salida XML.|
|-eef|Enumerar información adicional del sistema en el archivo de salida XML.|

## <a name="examples"></a>Ejemplos

- Para ejecutar la evaluación con el archivo de entrada que se usa durante una evaluación **formal de WinSAT** , sin emplear el Media Foundation canalización de medios protegidos (MFPMP), en un equipo donde c:\Windows es la ubicación de la carpeta de Windows.
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Observaciones

-   La pertenencia al grupo local Administradores, o equivalente, es lo mínimo necesario para usar **WinSAT**. El comando debe ejecutarse desde una ventana del símbolo del sistema con privilegios elevados.
-   Para abrir una ventana del símbolo del sistema con privilegios elevados, haga clic en **Inicio**, **accesorios**, haga clic con el botón secundario en **símbolo del sistema**y haga clic en **Ejecutar como administrador**.

## <a name="additional-references"></a>Referencias adicionales

