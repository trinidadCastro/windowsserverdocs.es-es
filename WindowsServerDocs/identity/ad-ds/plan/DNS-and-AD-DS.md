---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS y AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0eaddb6e94011a14b352b813cf508062116001f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822728"
---
# <a name="dns-and-ad-ds"></a>DNS y AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) utiliza los servicios de resolución de nombres del sistema de nombres de dominio (DNS) para que los clientes puedan localizar controladores de dominio y los controladores de dominio que hospedan el servicio de directorio para comunicarse entre sí.  
  
AD DS permite la integración sencilla del espacio de nombres de Active Directory en un espacio de nombres DNS existente. Características como las zonas DNS integradas en Active Directory facilitan la implementación de DNS mediante la eliminación de la necesidad de configurar zonas secundarias y, después, la configuración de las transferencias de zona.  
  
Para obtener información sobre cómo DNS admite AD DS, consulte la sección [compatibilidad de DNS con Active Directory referencia técnica](https://go.microsoft.com/fwlink/?LinkID=48147).  
  
> [!NOTE]  
> Si implementa un espacio de nombres separado en el que el nombre de dominio de AD DS difiere del sufijo DNS principal que usan los clientes, AD DS la integración con DNS es más compleja. Para obtener más información, vea [espacio de nombres discontinuo](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
- [Ubicación del controlador de dominio](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Zonas DNS integradas de Active Directory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [Nomenclatura de equipos](../../ad-ds/plan/Computer-Naming.md)  
- [Espacio de nombres separado](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
