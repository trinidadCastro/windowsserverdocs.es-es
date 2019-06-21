---
title: DirectAccess
description: Puede usar este tema para obtener una introducción breve de DirectAccess en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11c5aa093ddd5aa4777e88c536195bb70bd846db
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281923"
---
# <a name="directaccess"></a>DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener una introducción breve de DirectAccess, incluido el servidor y los sistemas operativos cliente que son compatibles con DirectAccess, y vínculos a documentación adicional de DirectAccess para Windows Server 2016.  
  
> [!NOTE]  
> Además de este tema, está disponible la siguiente documentación de DirectAccess.  
>   
> -   [Rutas de acceso de implementación de DirectAccess en Windows Server](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [Requisitos previos para la implementación de DirectAccess](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [Configuraciones no compatibles de DirectAccess](DirectAccess-Unsupported-Configurations.md)  
> -   [Guías del laboratorio de pruebas de DirectAccess](DirectAccess-Test-Lab-Guides.md)  
> -   [Problemas conocidos de DirectAccess](DirectAccess-Known-Issues.md)  
> -   [Planeamiento de capacidad de DirectAccess](DirectAccess-Capacity-Planning.md) 
> -   [Unión a dominio sin conexión de DirectAccess](DirectAccess-Offline-Domain-Join.md)  
> -   [Solución de problemas de DirectAccess](Troubleshooting-DirectAccess.md)  
> -   [Implementar un único servidor de DirectAccess mediante el Asistente para introducción](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [Implementar un único servidor de DirectAccess con configuración avanzada](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [Agregar DirectAccess a una implementación de acceso remoto existente (VPN)](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
DirectAccess permite la conectividad a los usuarios remotos a los recursos de red de organización sin necesidad de establecer conexiones de red privada Virtual (VPN). Con las conexiones de DirectAccess, los equipos cliente remotos estén siempre conectados a su organización: no es necesario para los usuarios remotos iniciar y detener las conexiones, como es necesario con conexiones VPN. Además, los administradores de TI pueden administrar equipos cliente de DirectAccess, siempre que se están ejecutando y conectado a Internet.

>[!IMPORTANT]
>No intente implementar el acceso remoto en una máquina virtual \(VM\) en Microsoft Azure. No se admite el uso de acceso remoto en Microsoft Azure. No se puede usar el acceso remoto en una máquina virtual de Azure para implementar la VPN, DirectAccess o cualquier otra característica de acceso remoto en Windows Server 2016 o versiones anteriores de Windows Server. Para obtener más información, consulte [soporte de software de servidor de Microsoft para las máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).
  
DirectAccess proporciona únicamente soporte para los clientes Unidos a un dominio que incluyen compatibilidad con sistema operativo para DirectAccess.  
  
Los siguientes sistemas operativos de servidor son compatibles con DirectAccess.  
  
-   Puede implementar todas las versiones de Windows Server 2016 como un cliente de DirectAccess o un servidor de DirectAccess.  
  
-   Puede implementar todas las versiones de Windows Server 2012 R2 como un cliente de DirectAccess o un servidor de DirectAccess.  
  
-   Puede implementar todas las versiones de Windows Server 2012 como un cliente de DirectAccess o un servidor de DirectAccess.  
  
-   Puede implementar todas las versiones de Windows Server 2008 R2 como un cliente de DirectAccess o un servidor de DirectAccess.  
  
Los siguientes sistemas operativos de cliente son compatibles con DirectAccess.  
  
-   Windows 10 Enterprise  
  
-   Rama de mantenimiento de Windows 10 Enterprise 2015 largo plazo (LTSB)  
  
-   Windows 8 y 8.1 Enterprise  
  
-   Windows 7 Ultimate  
  
-   Windows 7 Enterprise
