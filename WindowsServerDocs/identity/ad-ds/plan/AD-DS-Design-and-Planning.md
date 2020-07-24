---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: Planeación y diseño de AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 929530b8ee7dbb7b0486f3eb80a642cc5fdab20e
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962467"
---
# <a name="ad-ds-design-and-planning"></a>Planeación y diseño de AD DS

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mediante la implementación de Windows Server Active Directory Domain Services (AD DS) en su entorno, puede aprovechar las ventajas del modelo administrativo centralizado y delegado, y la capacidad de inicio de sesión único (SSO) que proporciona AD DS. Después de identificar las tareas de implementación y el entorno actual para su organización, puede crear la estrategia de implementación AD DS que satisfaga las necesidades de su organización.

## <a name="about-this-guide"></a>Acerca de esta guía

En esta guía se proporcionan recomendaciones para ayudarle a desarrollar una estrategia de implementación AD DS basada en los requisitos de su organización y en el diseño concreto que desea crear. Esta guía está pensada para especialistas en infraestructuras o arquitectos de sistemas. Antes de leer esta guía, debe tener una buena comprensión de cómo funciona AD DS en un nivel funcional. También debe tener una buena comprensión de los requisitos de la organización que se reflejarán en la estrategia de implementación de AD DS.

En esta guía se describen los conjuntos de tareas para varios puntos de partida posibles de una implementación de AD DS de Windows Server 2008. La guía le ayudará a decidir qué estrategia de implementación es la más adecuada para su entorno.

Aunque las estrategias que se presentan en esta guía son adecuadas para casi todas las implementaciones de sistema operativo de servidor, se han probado y validado específicamente para entornos que contienen menos de 100.000 usuarios y menos de 1.000 sitios, con conexiones de red de un mínimo de 28,8 kilobits por segundo (kbps). Si su entorno no cumple estos criterios, considere la posibilidad de usar una empresa de consultoría que tenga experiencia en la implementación de AD DS en entornos más complejos.

Para obtener más información acerca de cómo probar el proceso de implementación de AD DS, consulte el artículo [prueba y comprobación del proceso de implementación](/previous-versions/windows/it-pro/windows-server-2003/cc772722(v=ws.10)).

## <a name="in-this-guide"></a>En esta guía

[Descripción del diseño de AD DS](Understanding-AD-DS-Design.md)

[Determinación del diseño de AD DS y requisitos de implementación](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)

[Asignación de requisitos a una estrategia de implementación de AD DS](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)

[Diseñar la estructura lógica para Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)

[Diseño de la topología de sitio para Windows Server 2008 AD DS](Designing-the-Site-Topology.md)

[Habilitación de características avanzadas para AD DS](Enabling-Advanced-Features-for-AD-DS.md)

[Evaluación de ejemplos de estrategia de implementación de AD DS](Evaluating-AD-DS-Deployment-Strategy-Examples.md)

[Apéndice A: Revisión de términos clave de AD DS](Appendix-A--Reviewing-Key-AD-DS-Terms.md)
