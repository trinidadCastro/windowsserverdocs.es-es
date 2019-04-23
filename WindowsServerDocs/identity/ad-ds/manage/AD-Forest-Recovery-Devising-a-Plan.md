---
title: Plan de recuperación del bosque de recuperación de bosques de AD - idear un anuncio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: edfc874fe030c6394bc8bda95c49e61951e78f43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881786"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Plan de recuperación del bosque de recuperación de bosques de AD - idear un anuncio

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Según el entorno y los requisitos empresariales, podría o no podría tener que realizar los pasos descritos en esta guía para realizar una recuperación correcta del bosque. Dado que esta guía sirve como plantilla para la recuperación de bosque, es fundamental que diseñe un plan de recuperación personalizada del bosque que se adapte a su entorno y satisfaga sus necesidades empresariales.  
  
Por ejemplo, en el plan de recuperación de bosque, debe tener un mapa de topología detallada de los bosques. La asignación debe enumerar toda la información sobre los controladores de dominio, como sus nombres, sus roles y estado de copia de seguridad y las relaciones de confianza entre ellos. Para obtener una herramienta que puede usar para crear un mapa de topología, consulte [Diagrammer de topología de Active Directory de Microsoft](https://www.microsoft.com/download/details.aspx?id=13380).  
  
Debe poner en práctica el plan de recuperación de bosque al menos una vez al año. Además, es una buena idea para realizar un simulacro de recuperación del bosque cuando hay cambios de pertenencia al grupo Administradores de organización o Admins. del dominio. Esto ayuda a garantizar que su personal de información (TI) de la tecnología comprenda completamente el plan de recuperación del bosque.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de bosques de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosques de AD - idear un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosques de AD: identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación de bosques de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosques de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosques de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosques de AD - recuperar un único dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosques de AD: recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
