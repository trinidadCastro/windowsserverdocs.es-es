---
title: "Recuperación de bosque de AD - diseñar un anuncio del bosque de Plan de recuperación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adfs
ms.openlocfilehash: 6754509ccac352895b9370e63adf1b69548953bf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Recuperación de bosque de AD - diseñar un anuncio del bosque de Plan de recuperación

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Dependiendo del entorno y los requisitos empresariales, podría o no podría tener que realizar los pasos descritos en esta guía para realizar una recuperación de bosque correcta. Dado que esta guía sirve solo como una plantilla para la recuperación de bosque, es fundamental que diseñar un plan de recuperación de bosque personalizados que se adapte a tu entorno y que cumpla con tus necesidades empresariales.  
  
 Por ejemplo, en el plan de recuperación de bosque, debe tener un mapa de la topología detallada de los bosques. El mapa debe mostrar toda la información sobre los controladores de dominio, como sus nombres, sus roles y estado de copia de seguridad y las relaciones de confianza entre ellos. Para una herramienta que puedes usar para crear un mapa de la topología, consulta [Microsoft Active Directory topología Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380).  
  
 Debería probar el plan de recuperación de bosque al menos una vez al año. Además, es una buena idea para realizar un detalle de recuperación de bosque cuando hay cambios de pertenencia al grupo Administradores de empresa o administradores de dominio. Esto ayuda a garantizar que el personal de TI información comprenda el diseño de recuperación del bosque.

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
