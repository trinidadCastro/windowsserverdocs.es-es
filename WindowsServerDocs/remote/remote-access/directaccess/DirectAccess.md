---
title: DirectAccess
description: Puede usar este tema para obtener una breve introducción a DirectAccess en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 166d56da7127c19636e08b4ac8fe5912530d0c37
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947071"
---
# <a name="directaccess"></a>DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener una breve introducción a DirectAccess, incluidos los sistemas operativos de servidor y cliente que admiten DirectAccess, y para obtener vínculos a documentación adicional de DirectAccess para Windows Server 2016.

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
> -   [Implementar un único servidor de DirectAccess con el Asistente para Introducción](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)
> -   [Implementar un único servidor de DirectAccess con configuración avanzada](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)
> -   [Agregar DirectAccess a una implementación de acceso remoto existente (VPN)](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)

DirectAccess permite la conectividad de los usuarios remotos con los recursos de red de la organización sin necesidad de conexiones de red privada virtual (VPN) tradicionales. Con las conexiones de DirectAccess, los equipos cliente remotos siempre están conectados a la organización; no es necesario que los usuarios remotos inicien y detengan las conexiones, como es necesario con las conexiones VPN. Además, los administradores de TI pueden administrar los equipos cliente de DirectAccess siempre que ejecuten y estén conectados a Internet.

>[!IMPORTANT]
>No intente implementar el acceso remoto en una \( VM de máquina virtual \) en Microsoft Azure. No se admite el uso de acceso remoto en Microsoft Azure. No se puede usar el acceso remoto en una máquina virtual de Azure para implementar VPN, DirectAccess o cualquier otra característica de acceso remoto en Windows Server 2016 o versiones anteriores de Windows Server. Para obtener más información, vea [compatibilidad de software de servidor de Microsoft con máquinas virtuales de Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

DirectAccess proporciona compatibilidad solo para clientes Unidos a un dominio que incluyen compatibilidad con el sistema operativo para DirectAccess.

Los siguientes sistemas operativos de servidor son compatibles con DirectAccess.

-   Puede implementar todas las versiones de Windows Server 2016 como un cliente de DirectAccess o un servidor de DirectAccess.

-   Puede implementar todas las versiones de Windows Server 2012 R2 como un cliente de DirectAccess o un servidor de DirectAccess.

-   Puede implementar todas las versiones de Windows Server 2012 como un cliente de DirectAccess o un servidor de DirectAccess.

-   Puede implementar todas las versiones de Windows Server 2008 R2 como un cliente de DirectAccess o un servidor de DirectAccess.

Los siguientes sistemas operativos de cliente son compatibles con DirectAccess.

-   Windows 10 Enterprise

-   Windows 10 Enterprise 2015 Rama de mantenimiento a largo plazo (LTSB)

-   Windows 8 y 8,1 Enterprise

-   Windows 7 Ultimate

-   Windows 7 Enterprise
