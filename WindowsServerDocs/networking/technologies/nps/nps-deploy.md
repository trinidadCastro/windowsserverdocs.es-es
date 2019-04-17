---
title: Implementar el servidor de directivas de red
description: Este tema proporciona vínculos a contenido de la implementación de servidor de directivas de red para Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: daf62e2a593014e16bcb5fd16542b2e9c75b9c1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-policy-server"></a>Implementar el servidor de directivas de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información acerca de cómo implementar el servidor de directivas de red.

>[!NOTE]
>Para obtener documentación adicional de servidor de directivas de red, puedes usar las siguientes secciones de la biblioteca.  
>- [Tareas iniciales con el servidor de directivas de red](nps-getstart-top.md)
>- [Planear el servidor de directivas de red](nps-plan-top.md)
>- [Administrar el servidor de directivas de red](nps-manage-top.md)

La Guía de red de Windows Server 2016 principal incluye una sección sobre planeación e instalación \(NPS\) el servidor de directivas de red, y las tecnologías presentadas en la guía actúan como requisitos previos para implementar NPS en un dominio de Active Directory. Para obtener más información, consulta la sección "Implementar NPS1" en el equipo con Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1).

## <a name="deploy-nps-server-certificates-for-vpn-and-8021x-access"></a>Implementar certificados de servidor NPS para VPN y acceso 802.1X

Si quieres implementar métodos de autenticación como protocolo de autenticación Extensible \(EAP\) y EAP protegido que requieren el uso de certificados de servidor en el servidor NPS, puedes implementar los certificados de servidor NPS con la guía [implementar certificados de servidor para implementaciones de conexión inalámbrica y cableadas 802.1X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implementar NPS de acceso inalámbrico 802.1X

Para implementar NPS de acceso inalámbrico, puedes usar la guía [basada en contraseña implementar 802.1X X autenticados acceso inalámbrico](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implementar NPS para el acceso VPN de Windows 10

Puedes usar NPS para procesar las solicitudes de conexión para las conexiones de \(VPN\) siempre en red privada Virtual para empleados remotos que usan equipos y dispositivos que ejecutan Windows 10.

Para obtener más información, consulta el [remoto acceso siempre en VPN implementación Guía para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

