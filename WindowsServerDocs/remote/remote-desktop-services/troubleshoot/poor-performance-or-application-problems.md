---
title: Problemas de bajo rendimiento o de aplicaciones durante la conexión a Escritorio remoto
description: Solución de problemas de bajo rendimiento o de aplicaciones durante la conexión a Escritorio remoto.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: c65683a69633a950630b7fd74e1181da767ae35b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870581"
---
# <a name="poor-performance-or-application-problems-during-remote-desktop-connection"></a>Problemas de bajo rendimiento o de aplicaciones durante la conexión a Escritorio remoto

En este artículo se abordan varios problemas comunes que los usuarios pueden experimentar al usar la funcionalidad de Escritorio remoto.

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemas intermitentes con nuevas máquinas virtuales de Microsoft Azure

Este problema afecta a las máquinas virtuales que se han aprovisionado recientemente. Después de que el usuario se conecte a la máquina virtual, la sesión de escritorio remoto no carga toda la configuración del usuario correctamente.

Para solucionar este problema, desconecte la máquina virtual, espere al menos 20 minutos y vuelva a conectarla.

Para resolver este problema, aplique las siguientes actualizaciones a las maquinas virtuales, como sea apropiado:

  - Windows 10 y Windows Server 2016: KB 4343884, [30 de agosto de 2018: KB4343884 (compilación del sistema operativo 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018: KB4343891 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemas de reproducción de vídeo en Windows 10 versión 1709

Este problema se produce cuando los usuarios se conectan a equipos remotos que ejecutan Windows 10, versión 1709. Cuando estos usuarios reproducen vídeo mediante el códec VMR9 (Video Mixing Renderer 9), el reproductor muestra solo una ventana en negro.

Se trata de un problema conocido en Windows 10, versión 1709. El problema no se produce en la versión 1703 de Windows 10.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemas de uso compartido de escritorio en Windows 10

Este problema se produce cuando el usuario tiene un perfil de usuario de solo lectura (y el subárbol del Registro asociado), como en un escenario de quiosco multimedia. Cuando ese usuario se conecta a un equipo remoto en el que se ejecuta la versión 1803 de Windows 10, no puede compartir su escritorio.

Para corregir este problema, aplique la actualización 4340917 de Windows 10, [24 de julio de 2018: KB4340917 (compilación de sistema operativo 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemas de rendimiento al mezclar versiones de Windows 10, si se deshabilita NLA

Este problema se produce cuando los equipos cliente de Escritorio remoto que ejecutan Windows 10 se conectan a escritorios remotos que ejecutan otras versiones de Windows 10 mientras NLA está habilitado. Los usuarios de clientes de Escritorio remoto en equipos en los que se ejecuta la versión 1709 de Windows 10, o una versión anterior, experimentan un rendimiento deficiente al conectarse a escritorios remotos en los que se ejecuta la versión 1803 de Windows 10 o posterior.

Esto se debe a que, cuando NLA se deshabilita, los equipos cliente anteriores usan un protocolo más lento cuando se conectan a Windows 10, versión 1803, o a cualquier versión posterior.

Para resolver este problema, aplique el KB 4340917, [24 de julio de 2018: KB4340917 (compilación del sistema operativo 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema de la pantalla en negro

Este problema se produce en Windows 8.0, Windows 8.1, Windows 10 RTM y Windows Server 2012 R2. Un usuario inicia varias aplicaciones en un equipo de escritorio remoto y luego se desconecta de la sesión. De forma periódica, el usuario se vuelve a conectar con el Escritorio remoto para interactuar con las aplicaciones y, después, se vuelve a desconectar. En algún momento, cuando el usuario se vuelve a conectar, la sesión de Escritorio remoto solo muestra una pantalla en negro. Para que la sesión se vuelva a mostrar correctamente, el usuario debe finalizar su sesión desde la consola del equipo remoto o la consola del servidor RDSH, y detener las aplicaciones de la sesión.

Para resolver este problema, aplica las siguientes actualizaciones, según sea pertinente:

  - Windows 8 y Windows Server 2012: KB4103719, [17 de mayo de 2018: KB4103719 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 y Windows Server 2012 R2: KB4103724, [17 de mayo de 2018: KB4103724 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724) y KB 4284863, [21 de junio de 2018: KB4284863 (versión preliminar del paquete acumulativo mensual)](https://support.microsoft.com/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Se ha corregido en al KB4284860, [12 de junio de 2018: KB4284860 (compilación del sistema operativo 10240.17889)](https://support.microsoft.com/help/4284860/windows-10-update-kb4284860)
