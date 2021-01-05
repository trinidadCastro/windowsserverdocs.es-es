---
title: Instalación del compilador de registros de Windows Server Essentials
description: Aprenda a usar el Asistente para la instalación del recopilador de registros para instalar el compilador de registros como un complemento de Launchpad.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: a2ccb30620a8e0f03f29a006ecab999542f5552e
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810362"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Instalación del compilador de registros de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

El Asistente para la instalación del recopilador de registros de Windows Server Essentials instala el compilador de registros como un complemento de Launchpad. Puede instalar y usar el Compilador de registros en los equipos de la red, del servidor o de ambos. Después de la instalación, el Compilador de registros aparecerá en el panel.

###  <a name="to-install-the-log-collector"></a><a name="BKMK_ToInstall"></a> Para instalar el recopilador de registros

1.  Descargue el paquete de instalación del Compilador de registros en cualquier servidor o equipo de la red.

    > [!NOTE]
    > [Descargue el paquete de instalación del recopilador de registros de Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=34821).

2.  Haga doble clic en el icono del Compilador de registros.

3.  Si ejecuta la instalación desde un equipo de red, escriba sus credenciales de administrador del servidor cuando se le pidan.

4.  Acepte los Términos de licencia del software de Microsoft.

5.  Para instalar el Compilador de registros solo en el servidor, seleccione la casilla **Sólo en el servidor**. Para instalar el Compilador de registros en todos los equipos de red, seleccione la casilla **En el servidor y en todos los equipos de la red**.

6.  Haga clic en **Instalar el complemento**.

###  <a name="reinstalling-the-log-collector"></a><a name="BKMK_Reinstall"></a> Reinstalación del compilador de registros
 Si fuera necesario reinstalar el Compilador de registros, debe desinstalar y reinstalarlo en el servidor y en los equipos de la red. Al desinstalar el Compilador de registros en el servidor desde el panel, todos los equipos de red desinstalarán automáticamente el Compilador de registros.

##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Para desinstalar y reinstalar el Compilador de registros

1.  Abra el Panel.

2.  Haga clic en la pestaña **Complemento**, seleccione **Compilador de registros** en la lista y, a continuación, haga clic en **Desinstalar**.

3.  Descargue e instale el Compilador de registros siguiendo los pasos del procedimiento anterior [Para instalar el Compilador de registros](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).

### <a name="manually-install-the-log-collector"></a>Instalar manualmente el Compilador de registros
 Si se produce un error en el asistente de instalación del Compilador de registros, puede instalarlo en un solo equipo con el siguiente procedimiento.

##### <a name="to-manually-install-the-log-collector"></a>Para instalar manualmente el Compilador de registros

1.  Cambie el nombre de la extensión del archivo de instalación descargado de. WSSX a. cab.

2.  Haga doble clic en el nombre del archivo de instalación.

3.  Haga clic en **Aceptar** si se le pide.

4.  Haga doble clic en el nombre de archivo que termina con ". msi" y seleccione una carpeta en la que se va a extraer.

5.  Navegue hasta la carpeta con los archivos extraídos y haga doble clic en el archivo de instalación para utilizar al asistente para finalizar la instalación.
