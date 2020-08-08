---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Diseño de la estructura lógica
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1fca7f7b3166a6f9f9e9a9dc79e1b9a70fe832e4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943173"
---
# <a name="designing-the-logical-structure"></a>Diseño de la estructura lógica

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) permite a las organizaciones crear una infraestructura escalable, segura y administrable para la administración de usuarios y recursos. También les permite admitir aplicaciones habilitadas para el directorio.

Una estructura lógica Active Directory bien diseñada proporciona las siguientes ventajas:

-   Administración simplificada de redes basadas en Microsoft Windows que contienen un gran número de objetos

-   Una estructura de dominio consolidada y costos de administración reducidos

-   La capacidad de delegar el control administrativo sobre los recursos, según corresponda.

-   Impacto reducido en el ancho de banda de red

-   Uso compartido de recursos simplificado

-   Rendimiento óptimo de la búsqueda

-   Costo total de propiedad bajo

Una estructura lógica Active Directory bien diseñada facilita la integración eficaz de dichas características como directiva de grupo; bloqueo de escritorio; distribución de software; y la administración de usuarios, grupos, estaciones de trabajo y servidores en el sistema. Además, una estructura lógica diseñada cuidadosamente facilita la integración de aplicaciones y servicios de Microsoft y que no sean de Microsoft, como Microsoft Exchange Server, la infraestructura de clave pública (PKI) y un sistema de archivos distribuido (DFS) basado en dominio.

Al diseñar una estructura lógica Active Directory antes de implementar AD DS, puede optimizar el proceso de implementación para sacar el máximo partido de las características Active Directory. Para diseñar el Active Directory estructura lógica, el equipo de diseño primero identifica los requisitos de la organización y, en función de esta información, decide dónde colocar los límites del bosque y del dominio. Después, el equipo de diseño decide cómo configurar el entorno del sistema de nombres de dominio (DNS) para satisfacer las necesidades del bosque. Por último, el equipo de diseño identifica la estructura de la unidad organizativa (OU) necesaria para delegar la administración de los recursos de la organización.

## <a name="in-this-guide"></a>En esta guía

-   [Descripción del modelo lógico Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)

-   [Determinación de los participantes en el proyecto de implementación](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)

-   [Crear un diseño de bosque](../../ad-ds/plan/Creating-a-Forest-Design.md)

-   [Crear un diseño de dominios](../../ad-ds/plan/Creating-a-Domain-Design.md)

-   [Creación de un diseño de infraestructura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)

-   [Creación de diseño de unidad organizativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)

-   [Apéndice A: Inventario de DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)



