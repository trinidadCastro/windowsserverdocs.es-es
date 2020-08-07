---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS y AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: f531ffc8226fae9f515882e99da4620badc08611
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947714"
---
# <a name="dns-and-ad-ds"></a>DNS y AD DS

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) utiliza los servicios de resolución de nombres del sistema de nombres de dominio (DNS) para que los clientes puedan localizar controladores de dominio y los controladores de dominio que hospedan el servicio de directorio para comunicarse entre sí.

AD DS permite la integración sencilla del espacio de nombres de Active Directory en un espacio de nombres DNS existente. Características como las zonas DNS integradas en Active Directory facilitan la implementación de DNS mediante la eliminación de la necesidad de configurar zonas secundarias y, después, la configuración de las transferencias de zona.

Para obtener información sobre cómo DNS admite AD DS, consulte la sección [compatibilidad de DNS con Active Directory referencia técnica](/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10)).

> [!NOTE]
> Si implementa un espacio de nombres separado en el que el nombre de dominio de AD DS difiere del sufijo DNS principal que usan los clientes, AD DS la integración con DNS es más compleja. Para obtener más información, vea [espacio de nombres discontinuo](Disjoint-Namespace.md).

## <a name="in-this-section"></a>En esta sección

- [Ubicación del controlador de dominio](Domain-Controller-Location.md)
- [Zonas DNS integradas de Active Directory](Active-Directory-Integrated-DNS-Zones.md)
- [Nomenclatura de equipos](Computer-Naming.md)
- [Espacio de nombres separado](Disjoint-Namespace.md)
