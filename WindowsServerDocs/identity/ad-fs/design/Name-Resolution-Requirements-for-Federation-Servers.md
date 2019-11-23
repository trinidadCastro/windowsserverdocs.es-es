---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisitos de resolución de nombres para servidores de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 88ec418bd72a6389856deb1abd85641d8782bc30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408049"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisitos de resolución de nombres para servidores de federación

Cuando los equipos cliente de la red corporativa intentan obtener acceso a una aplicación o un servicio Web que está protegido por Servicios de federación de Active Directory (AD FS) \(AD FS\), primero deben autenticarse en un servidor de Federación. Una manera de autenticarse es que los clientes de la red corporativa tengan acceso a un servidor de Federación local a través de la autenticación integrada de Windows.  
  
## <a name="configure-corporate-dns"></a>Configurar un DNS corporativo  
Para que se pueda producir una resolución de nombres correcta mediante la autenticación integrada de Windows en servidores de Federación locales, el sistema de nombres de dominio \(DNS\) en la red corporativa del asociado de cuenta debe configurarse para un nuevo host \(un registro de recursos de\) que resuelva el nombre de dominio completo \(FQDN\) nombre de host del servidor de Federación en la dirección IP del clúster  
  
En la ilustración siguiente puedes ver cómo se lleva a cabo esta tarea en un escenario determinado. En este escenario, el equilibrio de carga de red de Microsoft \(NLB\) proporciona un nombre FQDN de clúster único y una dirección IP de clúster única para una granja de servidores de Federación existente.  
  
![requisitos de nombres](media/adfs2_deploy_single_fs.gif)  
  
Para obtener información sobre cómo configurar una dirección IP de clúster o un FQDN de clúster con NLB, vea [especificar los parámetros de clúster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Para obtener información sobre cómo configurar el DNS corporativo para un servidor de Federación, consulte [agregar &#40;un&#41; registro de recurso de host a a DNS corporativo para un servidor de Federación](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Para obtener información sobre cómo configurar los servidores proxy de Federación en la red perimetral, consulte [requisitos de resolución de nombres para servidores proxy de Federación](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
