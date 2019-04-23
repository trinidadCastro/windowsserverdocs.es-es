---
title: Componentes locales de inquilino
description: Describe los componentes de forma local en su implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: a01dbd12d76b1efa84e38f2ded38cfd613fb2ac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857406"
---
# <a name="tenant-on-premises-components"></a>Componentes locales de inquilino

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

La siguiente información describe los componentes locales que componen la implementación de hospedaje de escritorio.  
  
##  <a name="clients"></a>Clientes  
Para obtener acceso a los escritorios hospedados y aplicaciones, los usuarios deben usar a los clientes de escritorio remoto que admiten el protocolo de escritorio remoto (RDP) 7.1 o posterior. En concreto, el cliente debe admitir la puerta de enlace de escritorio remoto y el agente de conexión a Escritorio remoto. Para entregar aplicaciones en el escritorio local, el cliente también debe admitir la característica de RemoteApp. Para lograr la máxima escala de la puerta de enlace, el cliente debe admitir las conexiones de transporte HTTP puras a puerta de enlace de escritorio remoto.  
  
Información adicional:  
[Dispositivos habilitados para RemoteFX](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[Novedades de enlace de escritorio remoto de Windows Server 2012 R2](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Clientes de escritorio remoto de Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[Aplicación de escritorio remota para Windows en Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Escritorio remoto de Microsoft - aplicaciones Android en Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - escritorio remoto de Microsoft](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)  
[Escritorio remoto de Microsoft en la aplicación de Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory Domain Services  
Pueden elegir algunos inquilinos más grandes y más sofisticados hospedar un servidor de servicios de dominio de Active Directory (AD DS) en su entorno local. En este caso, el servidor de AD DS en el entorno del inquilino normalmente será una réplica del servidor de AD DS que sea local del inquilino. Esto es compatible con la creación de una red virtual en el entorno del inquilino y el uso de la VPN de Azure para crear una conexión de sitio a sitio desde la red del inquilino en el entorno local a la red virtual del inquilino en el centro de datos de Azure.  
  
Información adicional:  
[Introducción a Virtual Network de Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Crear un recurso en el Administrador de red virtual con una conexión VPN de sitio a sitio mediante el Portal de Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


