---
title: Cambiar la configuración de transmisión por secuencias
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823406"
---
# <a name="change-media-streaming-settings"></a>Cambiar la configuración de transmisión por secuencias

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Existen varias opciones para cambiar la configuración de la transmisión por secuencias de multimedia. Están disponibles las opciones siguientes:  
  
-   [Ocultar el complemento de transmisión por secuencias de multimedia remoto](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Establezca el nombre de la biblioteca multimedia](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Establecer la calidad de transmisión por secuencias de vídeo](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Habilitar o deshabilitar el streaming multimedia mediante programación](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a> Ocultar el complemento de transmisión por secuencias de multimedia remoto  
 La transmisión por secuencias de multimedia se puede ocultar de manera remota agregando una entrada al Registro.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Para ocultar el complemento de transmisión por secuencias de multimedia de manera remota  
  
1.  En el servidor, mueva el ratón a la esquina superior derecha de la pantalla y haga clic en **Buscar**.  
  
2.  En el cuadro **de búsqueda**, escriba **regedit**, y después haga clic en la aplicación **Regedit**.  
  
3.  En el panel izquierdo, expanda hacia abajo hasta la siguiente entrada del Registro:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Haga clic con el botón secundario en **DisabledAddIns**, seleccione **Nuevo**y a continuación haga clic en **Valor DWORD**.  
  
5.  Para el nuevo valor, escriba **497796c6-9cc7-43be-aa26-4e6b5695370d**, que es el identificador del complemento de transmisión por secuencias de multimedia de manera remota y después presione **Enter**.  
  
6.  Haga clic con el botón secundario del mouse en el valor y, a continuación, haga clic en **Modificar**.  
  
7.  Escriba **1** en los datos de valor y, a continuación, haga clic en **Aceptar**.  
  
##  <a name="BKMK_LibraryName"></a> Establezca el nombre de la biblioteca multimedia  
 Para establecer el nombre de la biblioteca multimedia, puede usar una clase de SDK de Soluciones de Windows Server. Para establecer el nombre de la biblioteca multimedia puede usar el método **SetMediaLibraryName** de la clase **MediaStreamingManager** del espacio de nombres **Microsoft.WindowsServerSolutions.MediaStreaming** . En el siguiente ejemplo se muestra cómo configurar el nombre de la biblioteca multimedia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Para obtener más información, consulte el [SDK de Soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a> Establecer la calidad de transmisión por secuencias de vídeo  
 Para establecer la calidad de transmisión por secuencias de vídeo, obtenga los resultados de la CPU de WinSAT y, a continuación, instale el archivo .xml que contenga la información sobre los resultados de WinSAT. Si el archivo .xml que contiene la información sobre los resultados de WinSAT se instala antes de que se ejecute la configuración inicial, el cliente no verá la interfaz de usuario para configurar la calidad del vídeo. Para obtener más información, consulte [Configurar los resultados de WinSAT en el servidor](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a> Habilitar o deshabilitar el streaming multimedia mediante programación  
 Para habilitar o deshabilitar la transmisión por secuencias de multimedia mediante programación, puede usar una clase de SDK de Soluciones de Windows Server. Para habilitar o deshabilitar la transmisión por secuencias de multimedia puede usar el método **SetMediaStreamingEnabled** de la clase **MediaStreamingManager** del espacio de nombres **Microsoft.WindowsServerSolutions.MediaStreaming**. En el ejemplo de código siguiente se muestra cómo habilitar la transmisión por secuencias de multimedia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 En el código de ejemplo que aparece a continuación se muestra cómo deshabilitar la transmisión por secuencias multimedia:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Vea también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)