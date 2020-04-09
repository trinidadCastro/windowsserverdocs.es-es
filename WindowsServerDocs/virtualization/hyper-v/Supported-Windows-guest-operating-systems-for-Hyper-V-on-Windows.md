---
title: Sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server
description: Enumera los sistemas operativos de Windows que se admiten para su uso como invitado en una máquina virtual. También proporciona vínculos a artículos similares para versiones anteriores de Hyper-V.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: 34183deefef3eea94c2b1da8dcb111c2c17efd8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857978"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Windows Server 2019

Hyper-V admite varias versiones de distribuciones de Windows Server, Windows y Linux que se ejecutan en máquinas virtuales, como sistemas operativos invitados. En este artículo se describen los sistemas operativos invitados de Windows Server y Windows. Para distribuciones de Linux y FreeBSD, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
    
Algunos sistemas operativos tienen integrado Integration Services. Otros requieren la instalación o actualización de Integration Services como paso independiente después de configurar el sistema operativo en la máquina virtual. Para obtener más información, vea las secciones siguientes y [Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).  
  
## <a name="supported-windows-server-guest-operating-systems"></a>Sistemas operativos invitados de Windows Server admitidos  

A continuación se muestran las versiones de Windows Server que se admiten como sistemas operativos invitados para Hyper-V en Windows Server 2016 y Windows Server 2019. 
  
|Sistema operativo invitado (servidor)|Número máximo de procesadores virtuales|Integration Services|Notas|  
|-------------------------------------|----------------------------------------|------------------------|---------| 
|Windows Server, versión 1909 |240 para la generación 2;<br>64 para la generación 1|Integrados|Una compatibilidad con el procesador virtual de más de 240 requiere sistemas operativos invitados de Windows Server, versión 1903 o posterior.| 
|Windows Server, versión 1903 |240 para la generación 2;<br>64 para la generación 1|Integrados||
|Windows Server, versión 1809 |240 para la generación 2;<br>64 para la generación 1|Integrados|| 
|Windows Server 2019 |240 para la generación 2;<br>64 para la generación 1|Integrados||
|Windows Server, versión 1803 |240 para la generación 2;<br>64 para la generación 1|Integrados|| 
|Windows Server 2016 |240 para la generación 2;<br>64 para la generación 1|Integrados|| 
|Windows Server 2012 R2 |64|Integrados||  
|Windows Server 2012 |64|Integrados||  
|Windows Server 2008 R2 con Service Pack 1 (SP 1)|64|Instale todas las actualizaciones críticas de Windows después de configurar el sistema operativo invitado.|Ediciones Datacenter, Enterprise, Standard y Web.|
|Windows Server 2008 con Service Pack 2 (SP2)|8|Instale todas las actualizaciones críticas de Windows después de configurar el sistema operativo invitado.|Ediciones Datacenter, Enterprise, Standard y Web (32 y 64 bits).|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>Sistemas operativos invitados de cliente Windows compatibles  

A continuación se muestran las versiones de cliente de Windows que se admiten como sistemas operativos invitados para Hyper-V en Windows Server 2016 y Windows Server 2019.
  
|Sistema operativo invitado (cliente)|Número máximo de procesadores virtuales|Integration Services|Notas|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|Integrados||  
|Windows 8.1|32|Integrados||  
|Windows 7 con Service Pack 1 (SP 1)|4|Actualice los servicios de integración después de configurar el sistema operativo invitado.|Ediciones Ultimate, Enterprise y Professional  (32 y 64 bits).|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Compatibilidad del sistema operativo invitado en otras versiones de Windows  

En la tabla siguiente se proporcionan vínculos a información acerca de los sistemas operativos invitados compatibles con Hyper-V en otras versiones de Windows.  
  
|Sistema operativo host|Tema|  
|-------------------------|---------|  
|Windows 10|[Sistemas operativos invitados admitidos para el cliente Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 y Windows 8.1|-   [sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server 2012 R2 y Windows 8.1](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />[Virtual Machines de -   Linux y FreeBSD en Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 y Windows 8|[Sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server 2012 y Windows 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 y Windows Server 2008 R2|[Acerca de los sistemas operativos Virtual Machines e invitados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Cómo Microsoft proporciona compatibilidad con los sistemas operativos invitados  

Microsoft ofrece compatibilidad para los sistemas operativos invitados de la siguiente manera:  
  
-   Los problemas encontrados en sistemas operativos y en servicios de integración de Microsoft se admiten en el soporte técnico de Microsoft.  
  
-   Los problemas encontrados en otros sistemas operativos cuya ejecución en Hyper-V se encuentra certificada por el proveedor del sistema operativo, se admiten en el soporte del proveedor.  
  
-   Microsoft envía los problemas detectados en otros sistemas operativos a la comunidad de soporte técnico de varios proveedores, [TSANet](https://www.tsanet.org/).  
  
## <a name="see-also"></a>Vea también  
  
-   [Máquinas virtuales Linux y FreeBSD en Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [Sistemas operativos invitados admitidos para el cliente Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



