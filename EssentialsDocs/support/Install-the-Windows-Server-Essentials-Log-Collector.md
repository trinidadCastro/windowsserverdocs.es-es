---
title: Instalar al recolector de registro de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ade18cec590392f35e7ad6b30d9a22ccdce44dcd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Instalar al recolector de registro de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

El Asistente para la instalación de selector de registro de Windows Server Essentials instala al recolector de registro como un Launchpad Add-in. Puedes instalar y usar el selector de registro en equipos de la red o el servidor o en ambos. Después de la instalación, el recolector de registro aparecerán en el panel.  
  
###  <a name="BKMK_ToInstall"></a>Para instalar al selector de registro  
  
1.  Descargar el paquete de instalación del selector de registro en cualquier equipo en la red o el servidor.  
  
    > [!NOTE]
    >  Puedes [descargar el paquete de instalación del recopilador del registro](https://go.microsoft.com/fwlink/p/?LinkId=255470) de Microsoft.  
  
2.  Haz doble clic en el icono de selector de registro.  
  
3.  Si se ejecuta la instalación desde un equipo de red, escribir sus credenciales de administrador del servidor cuando aparezca el mensaje.  
  
4.  Elige Aceptar los términos de licencia del Software de Microsoft.  
  
5.  Para instalar el selector de registro solo en el servidor, selecciona el **solo en el servidor** casilla de verificación. Para instalar el selector de registro en todos los equipos de red, seleccione la **en el servidor y en todos los equipos de la red** casilla de verificación.  
  
6.  Haz clic en **instalar el complemento**.  
  
###  <a name="BKMK_Reinstall"></a>Volver a instalar al selector de registro  
 Si es necesario volver a instalar el selector de registro, debe desinstalar y reinstalar el recolector de registro en el servidor y los equipos de red en la red. Al desinstalar el recolector de registro en el servidor en el panel, todos los equipos de red desinstalará automáticamente el selector de registro.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Desinstalar y reinstalar el recolector de registro  
  
1.  Abra el panel.  
  
2.  Haz clic en el **complemento**, selecciona **recopilador de registro** en la lista y, a continuación, haz clic en **desinstalar**.  
  

3.  Descargar e instalar el selector de registro, efectuando los pasos del procedimiento anterior, [para instalar el selector de registro](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Descargar e instalar el selector de registro, efectuando los pasos del procedimiento anterior, [para instalar el selector de registro](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Instalar manualmente el recolector de registro  
 Si el Asistente para la instalación no se pudo instalar al selector de registro, puedes instalar al selector de registro en un solo equipo mediante el siguiente procedimiento.  
  
##### <a name="to-manually-install-the-log-collector"></a>Instalar manualmente el recolector de registro  
  
1.  Cambia el nombre de la extensión del archivo de instalación descargado desde .wssx a .cab.  
  
2.  Haz doble clic en el nombre de archivo de instalación.  
  
3.  Haz clic en **Aceptar** si se le solicita.  
  
4.  Haz doble clic en el nombre de archivo que terminen en ˜.msi y selecciona una carpeta en la que extraerlo.  
  
5.  Navega a la carpeta con el archivo extraído y haz doble clic en el archivo de instalación para usar al Asistente para finalizar la instalación.
