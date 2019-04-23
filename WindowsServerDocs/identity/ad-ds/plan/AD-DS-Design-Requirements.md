---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: Requisitos de diseño de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 79c694112f39adf5d37cd28f6bd7a770dedf3976
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857526"
---
# <a name="ad-ds-design-requirements"></a>Requisitos de diseño de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Diseñar la estructura lógica de Active Directory  
Antes de implementar Windows Server 2008 Active Directory Domain Services (AD DS), debe planear y diseñar la estructura lógica de AD DS para su entorno. La estructura lógica de AD DS determina cómo se organizan los objetos de directorio, y proporciona un método eficaz para administrar las cuentas de red y recursos compartidos. Al diseñar la estructura lógica de AD DS, definir una parte significativa de la infraestructura de red de su organización.  
  
Para diseñar la estructura lógica de AD DS, determinar el número de bosques que requiere la organización y, a continuación, cree diseños para dominios, la infraestructura de sistema de nombres de dominio (DNS) y unidades organizativas (OU). La siguiente ilustración muestra el proceso para diseñar la estructura lógica.  
  
![Requisitos de diseño de AD DS](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Para obtener más información, consulte [diseño lógico estructura para Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Diseñar la topología de sitios  
Después de diseñar la estructura lógica para su infraestructura de AD DS, debe diseñar la topología de sitios de la red. La topología de sitio es una representación lógica de la red física. Contiene información sobre la ubicación de sitios de AD DS, los controladores de dominio de AD DS dentro de cada sitio y los vínculos a sitios y puentes de vínculos de sitio que admiten la replicación de AD DS entre sitios. La siguiente ilustración muestra el proceso de diseño de topología de sitio.  
  
![Requisitos de diseño de AD DS](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Para obtener más información, consulte [diseñar el sitio de topología para Windows Server 2008 AD DS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Planeación de capacidad del controlador de dominio  
Para garantizar un mayor rendimiento de AD DS, debe determinar el número adecuado de controladores de dominio para cada sitio y comprobar que cumple con los requisitos de hardware para Windows Server 2008. Cuidadoso planeamiento de capacidad para los controladores de dominio garantiza que se subestimen los requisitos de hardware, lo que pueden producir el tiempo de respuesta de rendimiento y la aplicación de controlador de dominio deficiente. La siguiente ilustración muestra el proceso de planeamiento de capacidad del controlador de dominio.  
  
![Requisitos de diseño de AD DS](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>Habilitación de Windows Server 2008 de características de AD DS avanzadas  
Puede usar AD DS de Windows Server 2008 para introducir características avanzadas en su entorno al elevar el nivel funcional del dominio o bosque. Puede elevar el nivel funcional a Windows Server 2008 cuando todos los controladores de dominio en el bosque o dominio ejecutan Windows Server 2008.  
  
Para obtener más información, consulte [habilitar características avanzadas para AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


