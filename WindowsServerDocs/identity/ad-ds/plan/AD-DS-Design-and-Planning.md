---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: "Diseño de AD DS y planificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 150e79f6ba89208e617ff0c487d64f415aac538e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-design-and-planning"></a>Diseño de AD DS y planificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Al implementar Windows Server Active Directory Domain Services (AD DS) en tu entorno, se puede aprovechar el modelo administrativa delegada centralizado y único inicio de sesión único (SSO) funcionalidad que proporciona AD DS. Después de identificar las tareas de implementación y el entorno actual para la organización, puedes crear la estrategia de implementación de AD DS que satisfaga las necesidades de la organización.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía proporciona recomendaciones que te ayudarán a desarrollar una estrategia de implementación de AD DS en función de los requisitos de la organización y el diseño particular que quieres crear. Esta guía está prevista para su uso por los especialistas de infraestructura o arquitectos de sistema. Antes de leer a esta guía, debe tener una buena comprensión de funcionamiento en un nivel funcional de AD DS. También debe tener una buena comprensión de los requisitos de la organización que se reflejarán en tu estrategia de implementación de AD DS.  
  
Esta guía describe conjuntos de tareas para varios puntos de partida posibles de una implementación de Windows Server 2008 AD DS. La guía ayuda a determinar la estrategia de implementación más adecuada para su entorno.  
  
Aunque las estrategias que se presentan en esta guía son adecuadas para casi todas las implementaciones de sistema operativo de servidor, se han probado y validado específicamente para entornos que contienen menos de 100.000 usuarios y menos de 1000 sitios, conexiones de red de un mínimo de 28,8 kilobits por segundo (Kbps). Si el entorno no cumple estos criterios, considera el uso de una empresa de consultoría que tiene la experiencia de implementación de AD DS en entornos más complejos.  
  
Para obtener más información sobre cómo probar el proceso de implementación de AD DS, consulta probar y verificar el proceso de implementación ([https://go.microsoft.com/fwlink/?LinkId=100206](https://go.microsoft.com/fwlink/?LinkId=100206)).  
  
> [!NOTE]  
> La información de esta guía también se aplica a Windows Server 2008 R2 AD DS.  
  
## <a name="in-this-guide"></a>En esta guía  
[Descripción de diseño de AD DS](Understanding-AD-DS-Design.md)  
  
[Identificar el diseño de AD DS y los requisitos de implementación](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)  
  
[Asignar los requisitos de a una estrategia de implementación de AD DS](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)  
  
[Diseñar la estructura lógica para Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)  
  
[Diseño de la topología de sitio para Windows Server 2008 AD DS](Designing-the-Site-Topology.md)  
  
[Habilitar las características avanzadas de AD DS](Enabling-Advanced-Features-for-AD-DS.md)  
  
[Evaluar los ejemplos de estrategia de implementación de AD DS](Evaluating-AD-DS-Deployment-Strategy-Examples.md)  
  
[Apéndice A: claves AD DS términos de revisión](Appendix-A--Reviewing-Key-AD-DS-Terms.md)  
  


