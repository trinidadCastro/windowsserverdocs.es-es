---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: "Diseñar la estructura lógica"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d1b9c27f05faef49f7fd4228f4ebe689b75d30f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-logical-structure"></a>Diseñar la estructura lógica

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servicios de dominio de Active Directory (AD DS) permite a las organizaciones crear una infraestructura escalable, segura y administrable de usuario y la administración de recursos. También permite que admita aplicaciones habilitadas para directorio.  
  
Una estructura lógica de Active Directory bien diseñada ofrece las siguientes ventajas:  
  
-   Administración simplificada de redes basadas en Windows de Microsoft que contengan grandes cantidades de objetos  
  
-   Una estructura de dominio consolidado y menores costos de administración  
  
-   La capacidad para delegar control administrativo sobre recursos, según corresponda  
  
-   Impacto reducido en el ancho de banda de red  
  
-   Uso compartido de recursos simplificada  
  
-   Rendimiento óptimo de búsqueda  
  
-   Bajo costo total de propiedad  
  
Una estructura lógica de Active Directory bien diseñada facilita la integración eficaz de características como la directiva de grupo. bloquear el escritorio. distribución de software; y administración de usuario, grupo, estación de trabajo y servidor en el sistema. Además, una estructura lógica cuidadosamente diseñada facilita la integración de Microsoft y aplicaciones ajenas a Microsoft y servicios, como Microsoft Exchange Server, la infraestructura de clave pública (PKI) y basados en dominio distribuyen (DFS) del sistema de archivos.  
  
Al diseñar una estructura lógica de Active Directory antes de implementar AD DS, puedes optimizar el proceso de implementación para aprovechar mejor las características de Active Directory. Para diseñar la estructura lógica de Active Directory, el equipo de diseño primero identifica los requisitos para la organización y, según esta información, decide dónde colocar los límites de bosque y dominio. A continuación, el equipo de diseño decide cómo configurar el entorno de sistema de nombres de dominio (DNS) para satisfacer las necesidades del bosque. Por último, el equipo de diseño identifica la estructura de unidad organizativa (OU) que se necesita para delegar la administración de recursos de la organización.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Descripción del modelo lógico de Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificar a los participantes del proyecto de implementación](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Creación de un diseño de bosque](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Creación de un diseño de dominio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Crear un diseño de la infraestructura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Crear un diseño de la unidad organizativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Apéndice A: DNS inventario](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


