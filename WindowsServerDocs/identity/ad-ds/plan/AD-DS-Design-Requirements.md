---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: "Requisitos de AD DS diseño"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 17338cd00fecec098865095dd9613f62beb3a457
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="ad-ds-design-requirements"></a>Requisitos de AD DS diseño

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Diseñar la estructura lógica de Active Directory  
Antes de implementar Windows Server 2008 Active Directory Domain Services (AD DS), debe planear y diseñar la estructura lógica de AD DS para su entorno. La estructura lógica de AD DS determina cómo se organizan los objetos de directorio y proporciona un método eficaz para la administración de las cuentas de redes y recursos compartidos. Al diseñar la estructura lógica de AD DS, defines una parte importante de la infraestructura de red de la organización.  
  
Para diseñar la estructura lógica de AD DS, determinar el número de bosques que necesita la organización y, a continuación, crear diseños para dominios, la infraestructura de sistema de nombres de dominio (DNS) y unidades organizativas (OU). La siguiente ilustración muestra el proceso de la estructura lógica de diseño.  
  
![Requisitos de diseño de AD DS](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Para obtener más información, consulta [diseñar la lógica estructura para Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Diseño de la topología de sitio  
Después de diseñar la estructura lógica de la infraestructura de AD DS, debes diseñar la topología de sitio para la red. La topología de sitios es una representación lógica de la red física. Contiene información sobre la ubicación de los sitios de AD DS, los controladores de dominio de AD DS dentro de cada sitio y los vínculos a sitios y puentes que admiten la replicación de AD DS entre sitios. La siguiente ilustración muestra el proceso de diseño de la topología de sitio.  
  
![Requisitos de diseño de AD DS](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Para obtener más información, consulta [diseñar el sitio topología para Windows Server 2008 AD DS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Planear la capacidad del controlador de dominio  
Para garantizar un rendimiento eficaz de AD DS, debes determinar el número adecuado de controladores de dominio para cada sitio y comprobar que cumplen los requisitos de hardware para Windows Server 2008. Diseño de los controladores de dominio de capacidad cuidadosa garantiza que no subestimes requisitos de hardware, lo que pueden hacer que el tiempo de respuesta de rendimiento y la aplicación de controlador de dominio deficiente. La siguiente ilustración muestra el proceso de planificación de la capacidad de controlador de dominio.  
  
![Requisitos de diseño de AD DS](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>Habilitar Windows Server 2008 avanzada características de AD DS  
Puedes usar Windows Server 2008 AD DS para introducir características avanzadas en el entorno levantando el nivel funcional del dominio o bosque. Puedes generar el nivel funcional de Windows Server 2008 cuando todos los controladores de dominio en el dominio o bosque ejecutan Windows Server 2008.  
  
Para obtener más información, consulta [habilitar características avanzadas de AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


