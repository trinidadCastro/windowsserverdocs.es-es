---
title: Configurar los resultados de WinSAT en el servidor
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 56769966cbfb6778c06abc4ba07d21f258e900a4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623346"
---
# <a name="set-the-winsat-score-on-the-server"></a>Configurar los resultados de WinSAT en el servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Debe establecer la puntuación de la CPU de WinSAT para un servidor que ejecute el sistema operativo Windows Server Essentials para optimizar la resolución de transmisión por secuencias de vídeo. Para ello, cree e instale el archivo .xml que contiene la información de los resultados de WinSAT.

## <a name="obtain-the-winsat-cpu-score"></a>Obtener el resultado de la CPU de WinSAT
 Dispone de un programa con OPK llamado WinServerSAT.exe que descubre los resultados de la CPU de WinSAT y coloca los datos en el archivo WinServerSAT.xml que el sistema operativo puede interpretar.

#### <a name="to-obtain-the-winsat-cpu-score"></a>Para obtener el resultado de la CPU de WinSAT

1. Copie el Resources\WinServerSAT \\ * en el medio de ADK en el equipo de referencia.

2. En el equipo de referencia, abra una ventana de símbolo del sistema con privilegios elevados.

3. Si la carpeta %ProgramFiles%\Windows Server\Bin\OEM no existe, escriba el siguiente comando y, a continuación, presione Entrar.

    **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**

4. Escriba el siguiente comando y, a continuación, presione Entrar.

    **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**

   En el ejemplo siguiente se muestra el contenido del archivo WinServerSAT.xml que se ha creado.

```

<?xml version="1.0" encoding="UTF-16"?>
<WinSAT>
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>
</WinSAT>
```

 Donde *WinSAT_Score* se sustituye por el valor detectado en el servidor.

> [!IMPORTANT]
>  Elimine los archivos WinServerSAT.exe, winsat.prx, winsat.wmv y WinSATEncode.wmv del equipo de referencia antes de capturar la imagen.
