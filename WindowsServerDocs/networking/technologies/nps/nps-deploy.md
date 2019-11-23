---
title: Implementar el Servidor de directivas de redes
description: En este tema se proporcionan vínculos a contenido de implementación del servidor de directivas de redes para Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33cada472c314088bc1485bab6d9631226b0ffaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405420"
---
# <a name="deploy-network-policy-server"></a>Implementar el Servidor de directivas de redes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de cómo implementar el servidor de directivas de redes.

>[!NOTE]
>Para obtener documentación adicional sobre el servidor de directivas de redes, puede usar las siguientes secciones de la biblioteca.  
>- [Introducción con el servidor de directivas de redes](nps-getstart-top.md)
>- [Planear el servidor de directivas de redes](nps-plan-top.md)
>- [Administrar el servidor de directivas de redes](nps-manage-top.md)

La guía de red principal de Windows Server 2016 incluye una sección sobre cómo planear e instalar el servidor de directivas de redes \(\)de NPS y las tecnologías que se presentan en la guía sirven como requisitos previos para la implementación de NPS en un dominio de Active Directory. Para obtener más información, vea la sección "deploy NPS1" en la guía de [red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)de Windows Server 2016.

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Implementación de certificados NPS para acceso VPN y 802.1 X

Si desea implementar métodos de autenticación como el protocolo de autenticación extensible \(EAP\) y EAP protegido que requieren el uso de certificados de servidor en su NPS, puede implementar certificados NPS con la guía de [implementación de certificados de servidor para implementaciones cableadas e inalámbricas de 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implementación de NPS para acceso inalámbrico de 802.1 X

Para implementar NPS para el acceso inalámbrico, puede usar la guía de [implementación de acceso inalámbrico autenticado mediante 802.1 x basada en contraseña](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implementación de NPS para acceso VPN de Windows 10

Puede usar NPS para procesar solicitudes de conexión de Always On red privada virtual \(VPN\) conexiones para empleados remotos que usan equipos y dispositivos que ejecutan Windows 10.

Para obtener más información, consulte la [Guía de implementación de VPN de acceso remoto Always on para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

