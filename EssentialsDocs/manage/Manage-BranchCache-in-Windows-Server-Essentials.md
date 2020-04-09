---
title: Administrar BranchCache en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a679973065a1d8b0033c0f00ca764aafe3546888
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852838"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Administrar BranchCache en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache puede ayudarle a optimizar el uso de Internet, mejorar el rendimiento de las aplicaciones en red y reducir el tráfico en la red de área extensa (WAN) cuando el servidor de Windows Server Essentials se encuentra de forma remota desde su oficina, o cuando los equipos cliente conectado a un servidor local use recursos basados en la nube, como las bibliotecas de SharePoint Online.  
  
 Con BranchCache habilitado, cuando un equipo cliente solicita contenido desde un servidor remoto de Windows Server Essentials, el contenido se almacena en caché en la oficina local. A continuación, otros equipos de la misma oficina pueden obtener el contenido localmente, en lugar de volver a descargar el contenido desde el servidor a través de la WAN. Esto puede mejorar el rendimiento de aplicaciones en red y reducir el consumo de ancho de banda a través de la WAN.  
  
 Tanto si el servidor de Windows Server Essentials es local como remoto, BranchCache puede mejorar los tiempos de respuesta de las carpetas compartidas del servidor y el contenido web que se hospeda en el servidor (por ejemplo, bibliotecas de SharePoint Online).  
  
 Dado que BranchCache no requiere hardware nuevo ni cambios en la topología de red, esta característica ofrece una manera sencilla de optimizar el uso del ancho de banda y mejorar los tiempos de respuesta de los servicios y los recursos a los que se tiene acceso a través de la WAN.  
  
## <a name="branchcache-scenarios"></a>Escenarios de BranchCache  
 Hay tres escenarios básicos para usar BranchCache con un servidor remoto:  
  
-   El servidor de Windows Server Essentials se hospeda en Microsoft Azure.  
  
-   El servidor de Windows Server Essentials se hospeda en el centro de información de un proveedor de servicios de terceros.  
  
-   El servidor de Windows Server Essentials se encuentra en otra oficina en una ubicación física diferente.  
  
## <a name="distributed-cache-mode"></a>Modo Caché distribuida  
 En Windows Server Essentials, BranchCache se implementa en *modo caché distribuida*, uno de los dos modos de caché disponibles en BranchCache. En modo Caché distribuida, la caché de contenido de la sucursal se distribuye entre los equipos cliente. Dado que no se necesita ningún hardware adicional ni cambios en la topología, este modo funciona bien para las pequeñas empresas que usan un servidor remoto o un servidor local para acceder a servicios en la nube, como SharePoint Online. Al activar BranchCache en Windows Server Essentials, se implementa el modo caché distribuida.  
  
> [!NOTE]
>  En las sucursales más grandes con más de una subred o con un gran número de empleados que usan aplicaciones en red, puede ser útil implementar BranchCache en el *modo Caché hospedada*. En el modo Caché hospedada, la caché de contenido se almacena en uno o más servidores de caché hospedados en la sucursal.
  
## <a name="requirements"></a>Requisitos  
 Para usar BranchCache en Windows Server Essentials, los equipos cliente y servidor deben cumplir los requisitos siguientes:  
  
-   El servidor debe ejecutar el sistema operativo Windows Server Essentials o el sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol experiencia con Windows Server Essentials.  
  
     En un servidor Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter, BranchCache se agrega al agregar el rol experiencia con Windows Server Essentials. Para activar BranchCache, deberá iniciar sesión en el panel de Windows Server Essentials con credenciales de administrador de dominio.  
  
-   Los equipos cliente deben ejecutar el sistema operativo Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Enterprise o Windows 8.1 Enterprise.  
  
-   En el modo Caché distribuida, todos los equipos cliente deben estar en la misma subred.  
  
    > [!NOTE]
    >  Si conectó los equipos cliente a su servidor de Windows Server Essentials sin unirlos al dominio, esos equipos se excluyen del almacenamiento en caché de forma predeterminada. Para incluir un equipo cliente que no está unido al dominio en el almacenamiento en caché, ejecute el cmdlet [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) de Windows PowerShell en el equipo cliente. Para obtener más información, consulte [Cmdlets de BranchCache en Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Activar BranchCache  
 Para activar BranchCache en modo caché distribuida, simplemente haga clic en un botón del panel de Windows Server Essentials. El almacenamiento en caché se inicia de inmediato y se realiza de manera transparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Activar BranchCache en Windows Server Essentials  
  
1.  Inicie sesión en el servidor de Windows Server Essentials con su cuenta de administrador.  
  
2.  En el panel de Windows Server Essentials, haga clic en **configuración**.  
  
     Se abre el Asistente para configuración.  
  
3.  Haga clic en **BranchCache**.  
  
4.  En la página **Configuración de BranchCache** , haga clic en **Activar**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Usar Windows PowerShell para activar o desactivar BranchCache  
 Puede usar Windows PowerShell para comprobar el estado de BranchCache (Habilitado o Deshabilitado) y para activar o desactivar BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Para activar o desactivar BranchCache usando Windows PowerShell  
  
1.  En el servidor, abra Windows PowerShell como administrador. En la pantalla **Inicio**, haga clic con el botón derecho en **Windows PowerShell**, haga clic en **Ejecutar como administrador** y, a continuación, haga clic en **Sí**.  
  
2.  Escriba cualquiera de los siguientes cmdlets en el símbolo del sistema.  
  
    -   Para comprobar el estado de BranchCache (Habilitado o Deshabilitado), escriba:  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   Para activar BranchCache, escriba:  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   Para desactivar BranchCache, escriba:  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>Vea también  
    
-   [Información general sobre BranchCache](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)
