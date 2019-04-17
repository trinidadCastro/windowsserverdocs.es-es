---
title: "Establece la puntuación de WinSAT en el servidor"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 77866acccac13ac48da8779700c8654f2c7f3277
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="set-the-winsat-score-on-the-server"></a>Establece la puntuación de WinSAT en el servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Debes establecer la puntuación de CPU de WinSAT para un servidor que ejecuta el sistema operativo de Windows Server Essentials para optimizar la resolución de transmisión por secuencias de vídeo. Para hacer esto, crear e instalar el archivo .xml que contiene la información de puntuaciones de WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obtener la puntuación de CPU de WinSAT  
 Se proporcionan un programa con el OPK llamado WinServerSAT.exe que detecta la puntuación de CPU de WinSAT y coloca esa información en el archivo WinServerSAT.xml que lee el sistema operativo.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Para obtener la puntuación de CPU de WinSAT  
  
1.  Copia Resources\WinServerSAT\\ * en medios ADK en el equipo de referencia.  
  
2.  En el equipo de referencia, abre una ventana de símbolo del sistema con privilegios elevados.  
  
3.  Si no existe la carpeta de %ProgramFiles%\Windows Server\Bin\OEM, escribe el siguiente comando y, a continuación, presione ENTRAR.  
  
     **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4.  Escribe el siguiente comando y, a continuación, presione ENTRAR.  
  
     **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
 El siguiente ejemplo muestra el contenido XML del archivo WinServerSAT.xml que se crea.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 Donde *WinSAT_Score* se reemplaza con el valor que se detecta en el servidor.  
  
> [!IMPORTANT]
>  Debes quitar WinServerSAT.exe, winsat.prx, winsat.wmv y WinSATEncode.wmv archivos desde el equipo de referencia antes de capturar la imagen.
