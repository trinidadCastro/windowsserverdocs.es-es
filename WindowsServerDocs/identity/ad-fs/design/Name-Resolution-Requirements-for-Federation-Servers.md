---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: "Requisitos de resolución de nombres para los servidores de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisitos de resolución de nombres para los servidores de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cuando los equipos cliente en la red corporativa intentan acceder a una aplicación o servicio Web que está protegido por los servicios de federación de Active Directory \(AD FS\), deben autenticarse primero a un servidor de federación. Un modo de autenticación es que los clientes de la red corporativa acceso a un servidor de federación local mediante la autenticación integrada de Windows.  
  
## <a name="configure-corporate-dns"></a>Configurar DNS corporativa  
Para que pueda producirse resolución de nombres correcta mediante la autenticación de Windows integrada en los servidores de federación local, \(DNS\) sistema de nombres de dominio en la red corporativa de asociado de la cuenta debe estar configurada para un nuevo registro de recursos \(A\) host que resuelva el nombre de host \(FQDN\) de nombre de dominio completo del servidor de federación de a la dirección IP del clúster de servidor de federación.  
  
En la siguiente ilustración, puedes ver cómo se realiza esta tarea para un escenario dado. En este escenario, \(NLB\) equilibrio de carga de red de Microsoft proporciona un nombre de clúster único FQDN y una sola dirección IP para un conjunto de servidor de federación existente.  
  
![Requisitos de nombre](media/adfs2_deploy_single_fs.gif)  
  
Para obtener información acerca de cómo configurar una dirección IP o FQDN mediante NLB del clúster, vea [especificar los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Para obtener información sobre cómo configurar DNS corporativa para un servidor de federación, vea [agregar un Host & #40; Un & #41; Registro de recursos corporativos DNS para un servidor de federación](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Para obtener información sobre cómo configurar servidores proxy de servidor de federación de la red perimetral, consulte [requisitos de resolución de nombre de Proxies de servidor de federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
