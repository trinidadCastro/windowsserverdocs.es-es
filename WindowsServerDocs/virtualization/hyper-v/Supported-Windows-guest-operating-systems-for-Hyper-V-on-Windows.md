---
title: Windows invitado sistemas operativos compatibles con Hyper-V en Windows Server
description: Se enumeran los sistemas de operativos Windows admitidos para su uso como un invitado en una máquina virtual. También ofrece vínculos a artículos similares para las versiones anteriores de Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: 5f0e91f3202f09d340154b49408c56752a9de577
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874216"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Windows invitado sistemas operativos compatibles con Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Windows Server 2019

Hyper-V admite varias versiones de distribuciones de Linux, Windows y Windows Server para ejecutarse en máquinas virtuales, como sistemas operativos invitados. Este artículo trata Windows Server compatible y los sistemas operativos invitados de Windows. Para las distribuciones de Linux y FreeBSD, consulte [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
    
Algunos sistemas operativos tienen los servicios de integración integrados. Otras requieren que instale o actualice los servicios de integración como un paso independiente después de configurar el sistema operativo en la máquina virtual. Para obtener más información, consulte las secciones siguientes y [Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services).  
  
## <a name="supported-windows-server-guest-operating-systems"></a>Sistemas de operativos de invitado de Windows Server  

A continuación es las versiones de Windows Server que se admiten como sistemas operativos invitados para Hyper-V en Windows Server 2016 y Windows Server 2019. 
  
|Sistema operativo invitado (servidor)|Número máximo de procesadores virtuales|Servicios de integración|Notas|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows Server 2019 |240 para la generación 2.<br>64 para la generación 1|Integrados|| 
|Windows Server 2016 |240 para la generación 2.<br>64 para la generación 1|Integrados|| 
|Windows Server 2012 R2 |64|Integrados||  
|Windows Server 2012 |64|Integrados||  
|Windows Server 2008 R2 con Service Pack 1 (SP 1)|64|Instale todas las actualizaciones críticas de Windows después de configurar el sistema operativo invitado.|Ediciones Datacenter, Enterprise, Standard y Web.|
|Windows Server 2008 con Service Pack 2 (SP2)|8|Instale todas las actualizaciones críticas de Windows después de configurar el sistema operativo invitado.|Ediciones Datacenter, Enterprise, Standard y Web (32 y 64 bits).|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>Sistemas de operativos de invitado de cliente de Windows compatibles  

Siguiente es las versiones de cliente de Windows que se admiten como sistemas operativos invitados para Hyper-V en Windows Server 2016 y Windows Server 2019.
  
|Sistema operativo invitado (cliente)|Número máximo de procesadores virtuales|Servicios de integración|Notas|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|Integrados||  
|Windows 8.1|32|Integrados||  
|Windows 7 con Service Pack 1 (SP 1)|4|Actualice los servicios de integración después de configurar el sistema operativo invitado.|Ediciones Ultimate, Enterprise y Professional  (32 y 64 bits).|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>Compatibilidad de sistema operativo invitado en otras versiones de Windows  

En la tabla siguiente proporciona vínculos a información sobre los sistemas operativos invitados admitidos para Hyper-V en otras versiones de Windows.  
  
|Sistema operativo host|Tema|  
|-------------------------|---------|  
|Windows 10|[Sistemas operativos invitados compatibles para cliente Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 y Windows 8.1|-   [Sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server 2012 R2 y Windows 8.1](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Linux y FreeBSD máquinas virtuales Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 y Windows 8|[Sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server 2012 y Windows 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 y Windows Server 2008 R2|[Acerca de las máquinas virtuales y sistemas operativos invitados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Cómo Microsoft ofrece soporte técnico para sistemas operativos invitados  

Microsoft ofrece compatibilidad para los sistemas operativos invitados de la siguiente manera:  
  
-   Los problemas encontrados en sistemas operativos y en servicios de integración de Microsoft se admiten en el soporte técnico de Microsoft.  
  
-   Los problemas encontrados en otros sistemas operativos cuya ejecución en Hyper-V se encuentra certificada por el proveedor del sistema operativo, se admiten en el soporte del proveedor.  
  
-   Microsoft envía los problemas detectados en otros sistemas operativos a la comunidad de soporte técnico de varios proveedores, [TSANet](https://www.tsanet.org/).  
  
## <a name="see-also"></a>Vea también  
  
-   [Linux y FreeBSD máquinas virtuales Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [Sistemas operativos invitados compatibles para cliente Hyper-V en Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



