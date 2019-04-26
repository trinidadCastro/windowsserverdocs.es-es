---
title: Instalación del compilador de registros de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837036"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Instalación del compilador de registros de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

El Asistente para instalación de Windows Server Essentials Log Collector instala al recopilador de registros como un Add en Launchpad. Puede instalar y usar el Compilador de registros en los equipos de la red, del servidor o de ambos. Después de la instalación, el Compilador de registros aparecerá en el panel.  
  
###  <a name="BKMK_ToInstall"></a> Para instalar al recopilador de registros  
  
1.  Descargue el paquete de instalación del Compilador de registros en cualquier servidor o equipo de la red.  
  
    > [!NOTE]
    >  También puede [descargar el paquete de instalación del Compilador de registros](https://go.microsoft.com/fwlink/p/?LinkId=255470) de Microsoft.  
  
2.  Haga doble clic en el icono del Compilador de registros.  
  
3.  Si ejecuta la instalación desde un equipo de red, escriba sus credenciales de administrador del servidor cuando se le pidan.  
  
4.  Acepte los Términos de licencia del software de Microsoft.  
  
5.  Para instalar el Compilador de registros solo en el servidor, seleccione la casilla **Sólo en el servidor** . Para instalar el Compilador de registros en todos los equipos de red, seleccione la casilla **En el servidor y en todos los equipos de la red** .  
  
6.  Haga clic en **Instalar el complemento**.  
  
###  <a name="BKMK_Reinstall"></a> Volver a instalar al recopilador de registros  
 Si fuera necesario reinstalar el Compilador de registros, debe desinstalar y reinstalarlo en el servidor y en los equipos de la red. Al desinstalar el Compilador de registros en el servidor desde el panel, todos los equipos de red desinstalarán automáticamente el Compilador de registros.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Para desinstalar y reinstalar el Compilador de registros  
  
1.  Abra el Panel.  
  
2.  Haga clic en la pestaña **Complemento** , seleccione **Compilador de registros** en la lista y, a continuación, haga clic en **Desinstalar**.  
  

3.  Descargue e instale el Compilador de registros siguiendo los pasos del procedimiento anterior [To install the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Descargue e instale el Compilador de registros siguiendo los pasos del procedimiento anterior [To install the Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Instalar manualmente el Compilador de registros  
 Si se produce un error en el asistente de instalación del Compilador de registros, puede instalarlo en un solo equipo con el siguiente procedimiento.  
  
##### <a name="to-manually-install-the-log-collector"></a>Para instalar manualmente el Compilador de registros  
  
1.  Cambie la extensión del archivo de instalación descargado desde .wssx a .cab.  
  
2.  Haga doble clic en el nombre del archivo de instalación.  
  
3.  Haga clic en **Aceptar** si se le pide.  
  
4.  Haga doble clic en el nombre de archivo que termina con ˜.msi y seleccione una carpeta en la que extraerlo.  
  
5.  Navegue hasta la carpeta con los archivos extraídos y haga doble clic en el archivo de instalación para utilizar al asistente para finalizar la instalación.
