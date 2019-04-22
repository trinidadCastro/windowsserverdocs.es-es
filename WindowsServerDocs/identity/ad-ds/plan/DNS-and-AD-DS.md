---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS y AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d75a78119d76a0f8380967292b1d0abc720597
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813146"
---
# <a name="dns-and-ad-ds"></a>DNS y AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servicios de dominio de Active Directory (AD DS) usa servicios de resolución de nombres del sistema de nombres de dominio (DNS) para hacer posible para los clientes buscar controladores de dominio y para los controladores de dominio que hospedan el servicio de directorio se comuniquen entre sí.  
  
AD DS permite facilitar la integración del espacio de nombres de Active Directory en un espacio de nombres DNS existente. Características tales como zonas DNS integradas en Active Directory que sea más fácil de implementar DNS, por lo que elimina la necesidad de configurar zonas secundarias y, a continuación, configurar las transferencias de zona.  
  
Para obtener información sobre la compatibilidad de DNS con AD DS, consulte la sección [compatibilidad de DNS con la referencia técnica de Active Directory](https://go.microsoft.com/fwlink/?LinkID=48147).  
  
> [!NOTE]  
> Si implementa un espacio de nombres no contiguo en el que el nombre de dominio de AD DS difiere el sufijo DNS principal que usan los clientes, la integración de AD DS con DNS es más compleja. Para obtener más información, consulte [discontinuas Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
- [Ubicación del controlador de dominio](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Zonas de DNS integrado en Active Directory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [Nombre del equipo](../../ad-ds/plan/Computer-Naming.md)  
- [Namespace no contiguo](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
