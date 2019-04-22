---
title: Implementar el servidor de directivas de redes
description: En este tema se proporciona vínculos a contenido de la implementación de servidor de directivas de red para Windows Server 2016 e incluye vínculos a guías adicionales acerca de NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8da8951a9c6ed5022c892bbf01b33614d38abc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814846"
---
# <a name="deploy-network-policy-server"></a>Implementar el servidor de directivas de redes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre la implementación de servidor de directivas de red.

>[!NOTE]
>Para obtener documentación adicional de servidor de directivas de red, puede usar las siguientes secciones de la biblioteca.  
>- [Introducción a servidor de directivas de red](nps-getstart-top.md)
>- [Planear el servidor de directivas de red](nps-plan-top.md)
>- [Administrar el servidor de directivas de red](nps-manage-top.md)

La Guía de red de Windows Server 2016 Core incluye una sección sobre planeamiento e instalación de servidor de directivas de red \(NPS\), y las tecnologías que se presentan en la guía que sirven como requisitos previos para implementar NPS en un dominio de Active Directory. Para obtener más información, vea la sección "Implementar NPS1" en Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Implementación de certificados NPS para acceso 802.1X y VPN

Si desea implementar métodos de autenticación como el protocolo de autenticación Extensible \(EAP\) y EAP protegido que requieren el uso del servidor de certificados en el NPS, puede implementar certificados de NPS con la Guía de [ Implementar certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implementación de NPS para acceso inalámbrico 802.1X

Para implementar NPS para acceso inalámbrico, puede usar la guía [basado en contraseña implementar autentica el acceso inalámbrico 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implementación de NPS para acceso VPN de Windows 10

Puede usar NPS para procesar las solicitudes de conexión para siempre en redes privadas virtuales \(VPN\) conexiones para los empleados remotos que usan los equipos y dispositivos que ejecutan Windows 10.

Para obtener más información, consulte el [remoto acceso siempre en VPN Deployment Guide para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

