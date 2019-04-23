---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Diseño de la estructura lógica
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 82184fe678c05b1d7458584de8eecd0c07ece02f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832336"
---
# <a name="designing-the-logical-structure"></a>Diseño de la estructura lógica

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servicios de dominio de Active Directory (AD DS) permite a las organizaciones a crear una infraestructura escalable, segura y administrable para la administración de usuarios y recursos. También permite que admita aplicaciones habilitadas para directorio.  
  
Una estructura lógica de Active Directory bien diseñada ofrece las siguientes ventajas:  
  
-   Administración simplificada de redes basadas en Windows de Microsoft que contienen grandes cantidades de objetos  
  
-   Una estructura de dominio consolidado y menores costos de administración  
  
-   La capacidad de delegar control administrativo sobre los recursos, según corresponda  
  
-   Menor repercusión en el ancho de banda de red  
  
-   Uso compartido de recursos simplificada  
  
-   Rendimiento de la búsqueda óptimo  
  
-   Costo total de propiedad reducido  
  
Una estructura lógica de Active Directory bien diseñada facilita la integración eficaz de características como la directiva de grupo. bloquear el escritorio. distribución de software; y el usuario, grupo, estación de trabajo y servidor de administración en el sistema. Además, una estructura lógica cuidadosamente diseñada facilita la integración de Microsoft y las aplicaciones que no sean de Microsoft y servicios, como Microsoft Exchange Server, la infraestructura de clave pública (PKI), y basado en dominio distribuido (DFS) del sistema de archivos.  
  
Al diseñar una estructura lógica de Active Directory antes de implementar AD DS, puede optimizar el proceso de implementación para sacar el máximo provecho de las características de Active Directory. Para diseñar la estructura lógica de Active Directory, el equipo de diseño primero identifica los requisitos de su organización y, según esta información, decide dónde colocar los límites del bosque y dominio. A continuación, el equipo de diseño decide cómo configurar el entorno de sistema de nombres de dominio (DNS) para satisfacer las necesidades del bosque. Por último, el equipo de diseño identifica la estructura de unidad organizativa (OU) que se requiere para delegar la administración de recursos de la organización.  
  
## <a name="in-this-guide"></a>En esta guía  
  
-   [Descripción del modelo lógico de Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identificar a los participantes del proyecto de implementación](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Crear un diseño de bosque](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Crear un diseño de dominio](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Crear un diseño de la infraestructura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Creación de un diseño de unidad organizativa](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Apéndice A: Inventario DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


