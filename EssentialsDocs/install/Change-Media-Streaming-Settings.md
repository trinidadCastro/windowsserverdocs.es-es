---
title: "Cambiar la configuración de Streaming de multimedia"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d34d60e792fcda4d71a4509fe3d1b95fc6e74d0e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="change-media-streaming-settings"></a>Cambiar la configuración de Streaming de multimedia

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Existen varias opciones para cambiar la configuración de streaming multimedia. Las siguientes opciones están disponibles:  
  
-   [Ocultar el complemento de streaming de multimedia remotas](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Establecer el nombre de la biblioteca multimedia](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Establecer la calidad de transmisión por secuencias de vídeo](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Habilitar o deshabilitar el streaming multimedia mediante programación](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a>Ocultar el complemento de streaming de multimedia remotas  
 Puedes ocultar el streaming remoto complemento agregando una entrada en el registro.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Para ocultar el complemento de streaming multimedia de manera remota  
  
1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla y haz clic en **búsqueda**.  
  
2.  En la **búsqueda** , escriba **regedit**y, a continuación, haz clic en el **Regedit** aplicación.  
  
3.  En el panel izquierdo, expande hasta llegar a la siguiente clave del registro:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Haz clic en **DisabledAddIns**, elija **nueva**y, a continuación, haz clic en **valor DWORD**.  
  
5.  Para el nuevo valor, escribe **497796c6-9cc7-43be-aa26-4e6b5695370d**, que es el identificador para el complemento de streaming de multimedia remoto y, a continuación, presione **ENTRAR**.  
  
6.  Haga clic con el botón secundario del mouse en el valor y, a continuación, haz clic en **modificar**.  
  
7.  Tipo **1** para la información del valor y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_LibraryName"></a>Establecer el nombre de la biblioteca multimedia  
 Puedes usar una clase en el SDK de soluciones de Windows Server para establecer el nombre de la biblioteca multimedia. Para establecer el nombre de la biblioteca multimedia, puedes usar la **SetMediaLibraryName** método de la **MediaStreamingManager** clase la **Microsoft.WindowsServerSolutions.MediaStreaming** espacio de nombres. El siguiente ejemplo muestra cómo establecer el nombre de la biblioteca multimedia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Para obtener más información, consulta [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a>Establecer la calidad de transmisión por secuencias de vídeo  
 Establecer la calidad de streaming al obtener la puntuación de CPU de WinSAT y, a continuación, crear e instalar el archivo .xml que contiene la información de puntuaciones de WinSAT de vídeo. Si el archivo .xml que contiene la información de puntuaciones de WinSAT se ha instalado antes de que se ejecuta la configuración inicial, la interfaz de usuario de calidad de vídeo de configuración no aparecerá al cliente. Para obtener más información, consulta [establece la puntuación de WinSAT en el servidor](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a>Habilitar o deshabilitar el streaming multimedia mediante programación  
 Puedes usar una clase en el SDK de soluciones de Windows Server para habilitar o deshabilitar el streaming multimedia mediante programación. Para habilitar o deshabilitar el streaming multimedia, puedes usar la **SetMediaStreamingEnabled** método de la **MediaStreamingManager** clase la **Microsoft.WindowsServerSolutions.MediaStreaming** espacio de nombres. El siguiente ejemplo de código muestra cómo habilitar el streaming multimedia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 El siguiente ejemplo de código muestra cómo deshabilitar el streaming multimedia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)