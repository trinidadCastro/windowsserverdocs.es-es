---
title: "Requisitos previos para planear para la recuperación de bosque de Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 672e1f4d0de9bfb2cbe291c5ed715814c8acacd0
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
#<a name="active-directory-forest-recovery-prerequisites"></a>Requisitos previos de recuperación de bosque de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

El siguiente documento Explique requisitos previos que debes estar familiarizado con antes de diseñar un plan de recuperación de bosque o tratar de realizar una recuperación.

## <a name="assumptions-for-using-this-guide"></a>Supuestos de uso de esta guía 

1.  Ha trabajado con un profesional de Microsoft Support y:

    - Determinar la causa del error de todo el bosque. Esta guía no sugerir una causa del error ni recomienda los procedimientos para evitar el error.  
    - Evalúa las posibles soluciones.  
    - Celebrados en colaboración con Microsoft Support, ese restaurar todo el bosque a su estado antes de que se produjo el error es la mejor forma recuperarse del error. En muchos casos, recuperación de bosque debe ser la última opción.  </br></br>

2. Haber seguido los procedimientos recomendados de Microsoft para usar el sistema de nombres de dominio (DNS) integrado en Active Directory. Específicamente, debe haber una zona DNS integrado en Active Directory para cada dominio de Active Directory. Si este no es el caso, aún puede usar los principios básicos de esta guía para realizar la recuperación de bosque. Sin embargo, deberás medidas específicas en función de su propio entorno de recuperación DNS. Para obtener más información sobre el uso de DNS integradas en Active Directory, consulte [crear un diseño de la infraestructura de DNS ](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).
3. Aunque esta guía sirve como guía genérica para la recuperación de bosque, no se cubren todos los escenarios posibles. Por ejemplo, a partir de Windows Server 2008, hay una versión de Server Core, que es una versión completa de Windows Server sin una interfaz gráfica de usuario completa. Aunque es totalmente factible recuperar un bosque que conste de tan solo los controladores de dominio que se ejecutan Server Core, esta guía no tiene ningún instrucciones detalladas. Sin embargo, en función de las instrucciones que se tratan aquí podrás diseñar las acciones necesarias de línea de comandos tú mismo.  
 
>![!NOTE]
> A pesar de los objetivos de esta guía recuperar el bosque y mantener o restaurar toda la funcionalidad DNS, puede provocar la recuperación en una configuración de DNS que ha cambiado desde la configuración antes del error. Una vez recuperado el bosque, puedes revertir a la configuración de DNS original. Las recomendaciones indicadas en esta guía se describe cómo configurar servidores DNS para realizar la resolución de nombres de otras partes del espacio de nombres corporativa donde hay zonas DNS que no se almacenan en AD DS.  

## <a name="concepts-for-using-this-guide"></a>Conceptos para usar esta guía
 Antes de comenzar con el diseño para la recuperación de un bosque de Active Directory, deberías conocer lo siguiente:  
  
-   Conceptos fundamentales de Active Directory  
  
-   La importancia de los roles de maestro de operaciones (también conocido como operaciones de maestro único flexibles o FSMO). Estas funciones incluyen lo siguiente:  
  
    -   Maestro de esquema  
  
    -   Patrón de nomenclatura de dominio  
  
    -   Maestro relativos ID (RID)  
  
    -   Maestro emulador de dominio principal (PDC) del controlador  
  
    -   Maestro de infraestructura  
  
 Además, debe haber una copia de seguridad y restaurar AD DS y SYSVOL en un entorno de laboratorio de forma regular. Para obtener más información, consulta [copia los datos de estado del sistema](AD-Forest-Recovery-Procedures.md) y [realizar una restauración de servicios de dominio de Active Directory no autorizada ](AD-Forest-Recovery-Procedures.md).

## <a name="next-steps"></a>Pasos siguientes
-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
