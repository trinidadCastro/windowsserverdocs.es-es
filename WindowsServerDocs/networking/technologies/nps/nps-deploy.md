---
title: Implementar el servidor de directivas de redes
description: En este tema se proporcionan vínculos a contenido de implementación del servidor de directivas de redes para Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.topic: article
ms.assetid: 6cfb50e0-7088-4295-97c5-14ff8776cbf8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b4f80d18b2facba7bb706bb566aed698c2a85c8d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937416"
---
# <a name="deploy-network-policy-server"></a>Implementar el servidor de directivas de redes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información acerca de cómo implementar el servidor de directivas de redes.

>[!NOTE]
>Para obtener documentación adicional sobre el servidor de directivas de redes, puede usar las siguientes secciones de la biblioteca.
>- [Tareas iniciales con el servidor de directivas de redes](nps-getstart-top.md)
>- [Planear el servidor de directivas de redes](nps-plan-top.md)
>- [Administrar el servidor de directivas de redes](nps-manage-top.md)

La guía de red principal de Windows Server 2016 incluye una sección sobre cómo planear e instalar el NPS del servidor de directivas de redes \( \) , y las tecnologías que se presentan en la guía sirven como requisitos previos para la implementación de NPS en un dominio de Active Directory. Para obtener más información, vea la sección "deploy NPS1" en la guía de [red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployNPS1)de Windows Server 2016.

## <a name="deploy-nps-certificates-for-vpn-and-8021x-access"></a>Implementación de certificados NPS para acceso VPN y 802.1 X

Si desea implementar métodos de autenticación como EAP de protocolo de autenticación extensible \( \) y EAP protegido que requieren el uso de certificados de servidor en su NPS, puede implementar certificados NPS con la guía de implementación de certificados [de servidor para implementaciones cableadas e inalámbricas de 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="deploy-nps-for-8021x-wireless-access"></a>Implementación de NPS para acceso inalámbrico de 802.1 X

Para implementar NPS para el acceso inalámbrico, puede usar la guía de [implementación de acceso inalámbrico autenticado mediante 802.1 x basada en contraseña](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).

## <a name="deploy-nps-for-windows-10-vpn-access"></a>Implementación de NPS para acceso VPN de Windows 10

Puede usar NPS para procesar solicitudes de conexión de Always On \( conexiones VPN de red privada virtual \) para empleados remotos que usan equipos y dispositivos que ejecutan Windows 10.

Para obtener más información, consulte la [Guía de implementación de VPN de acceso remoto Always on para Windows Server 2016 y Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

