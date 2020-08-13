---
title: Componentes locales de inquilino
description: Describe los componentes locales en su implementación de RDS.
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: e2a06368bea88a9b746242da5a0281b465804c62
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958123"
---
# <a name="tenant-on-premises-components"></a>Componentes locales de inquilino

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

La siguiente información describe los componentes locales que componen la implementación de hospedaje de escritorio.

##  <a name="clients"></a>Clientes
Para acceder a los escritorios y aplicaciones hospedados, los usuarios deben usar los clientes de Escritorio remoto que admiten el Protocolo de escritorio remoto (RDP) 7.1 o posterior. En concreto, el cliente debe admitir la Puerta de enlace de Escritorio remoto y el Agente de conexión a Escritorio remoto. Para entregar aplicaciones al escritorio local, el cliente también debe admitir la característica RemoteApp. Para lograr la máxima escala de la puerta de enlace, el cliente debe admitir las conexiones de transporte HTTP puras a la Puerta de enlace de Escritorio remoto.

Información adicional: [Novedades de la Puerta de enlace de Escritorio remoto de Windows Server 2012 R2](https://techcommunity.microsoft.com/t5/microsoft-security-and/what-8217-s-new-in-windows-server-2012-remote-desktop-gateway/ba-p/247611)
[Clientes de Escritorio remoto de Microsoft](./clients/remote-desktop-clients.md)
[Aplicación de Escritorio remoto para Windows en Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)
[Escritorio remoto de Microsoft: aplicaciones Android en Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)
[Mac App Store: Escritorio remoto de Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)
[Escritorio remoto de Microsoft en App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)

##  <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory
Algunos inquilinos más grandes y más sofisticados pueden elegir hospedar un servidor de Active Directory Domain Services (AD DS) en su entorno local. En este caso, el servidor de AD DS en el entorno del inquilino normalmente será una réplica del servidor de AD DS que se encuentra en el entorno local del inquilino. Esto se admite al crear una red virtual en el entorno del inquilino y usar la VPN de Azure para crear una conexión de sitio a sitio desde la red local del inquilino a la red virtual del inquilino en el centro de datos de Azure.

Información adicional: [Información general de Microsoft Azure Virtual Network](/azure/virtual-network/virtual-networks-overview)
[Creación de una red virtual de Resource Manager con una conexión VPN de sitio a sitio mediante Azure Portal](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
