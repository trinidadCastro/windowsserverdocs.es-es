---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS y AD DS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e7ee494c157396a7d58e9fd1b4b80060c4d99fc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="dns-and-ad-ds"></a>DNS y AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servicios de dominio de Active Directory (AD DS) usa servicios de resolución de nombres de sistema de nombres de dominio (DNS) para que sea posible para los clientes buscar controladores de dominio y para los controladores de dominio que hospedan el servicio de directorio para comunicarse entre sí.  
  
AD DS permite facilitar la integración del espacio de nombres de Active Directory en un espacio de nombres DNS existente. Características como zonas DNS integradas en Active Directory que sea más fácil de implementar DNS, ya que eliminan la necesidad de configurar zonas secundarias y, a continuación, configura las transferencias de zona.  
  
Para obtener información sobre cómo DNS es compatible con AD DS, consulta la compatibilidad de DNS para la referencia técnica de Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
> [!NOTE]  
> Si implementas un espacio de nombres desligada en el que el nombre de dominio de AD DS difiere el sufijo DNS principal que usan los clientes, la integración de AD DS con DNS es más compleja. Para obtener más información, consulta [discontinuas Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Ubicación del controlador de dominio](../../ad-ds/plan/Domain-Controller-Location.md)  
  
-   [Zonas de DNS Active Directory integrado](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
  
-   [Nombres de equipo](../../ad-ds/plan/Computer-Naming.md)  
  
-   [Namespace desligada](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
  


