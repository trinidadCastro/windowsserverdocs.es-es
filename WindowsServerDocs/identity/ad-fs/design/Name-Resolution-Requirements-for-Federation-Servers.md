---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisitos de resolución de nombres para servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845466"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisitos de resolución de nombres para servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando los equipos cliente en la red corporativa intentan tener acceso a una aplicación o servicio Web que está protegido por Active Directory Federation Services \(AD FS\), primero debe realizar la autenticación a un servidor de federación. Una manera de autenticar es que los clientes de la red corporativa obtener acceso a un servidor de federación local mediante la autenticación integrada de Windows.  
  
## <a name="configure-corporate-dns"></a>Configurar un DNS corporativo  
Para que puedan realizarse la resolución de nombres correcta mediante la autenticación integrada de Windows en servidores de federación local, sistema de nombres de dominio \(DNS\) en la red corporativa de la cuenta de socio debe estar configurado para un nuevo host \(A\) registro de recursos que resolverá el nombre de dominio completo \(FQDN\) nombre de host del servidor de federación a la dirección IP del clúster de servidores de federación.  
  
En la ilustración siguiente puedes ver cómo se lleva a cabo esta tarea en un escenario determinado. En este escenario, Microsoft Network Load Balancing \(NLB\) proporciona un nombre de FQDN de clúster único y una dirección IP de clúster única para una granja de servidores de federación existente.  
  
![requisitos de nombre](media/adfs2_deploy_single_fs.gif)  
  
Para obtener información acerca de cómo configurar una dirección IP o FQDN de clúster con NLB, consulte [especificando los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Para obtener información sobre cómo configurar DNS corporativo para un servidor de federación, vea [agrega un Host &#40;A&#41; registro de recurso al DNS corporativo para un servidor de federación](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Para obtener información sobre cómo configurar servidores proxy de federación en la red perimetral, consulte [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
