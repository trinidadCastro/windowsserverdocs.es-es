---
title: "Anuncio del bosque de recuperación: identificar el problema"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 1e9ede12dade0b5f1149eae784620520a8866e30
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="identify-the-problem"></a>Identificar el problema

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
  
 Cuando aparezcan los síntomas de un error de todo el bosque, como en los registros de eventos u otras soluciones de supervisión, trabajar con Microsoft Support para determinar la causa del error y evaluar las posibles soluciones.  
 
## <a name="examples-of-forest-wide-failures"></a>Ejemplos de errores de todo el bosque 
  
-   Todos los controladores de dominio se han dañado lógicamente o daños físicos a un punto que continuidad del negocio es imposible; Por ejemplo, todas las aplicaciones de negocio que dependen de AD DS son funcionales.  
  
-   Un administrador malintencionado haya puesto en peligro el entorno de Active Directory.  
  
-   Un atacante intencionadamente, o un administrador accidentalmente, ejecuta un script que se distribuye dañar los datos a través del bosque.  
  
-   Un atacante intencionadamente, o un administrador accidentalmente: extiende el esquema de Active Directory con los cambios de software malintencionados o en conflicto.  
  
-   Un atacante ha podido instalar software malintencionado en controladores de dominio y se haya notificado por Microsoft Support para recuperar el bosque de copia de seguridad.  
  
    > [!IMPORTANT]
    >  En este documento no abarca las recomendaciones de seguridad sobre cómo recuperar un bosque que ha sido pirateado o puesto en peligro. En general, se recomienda técnicas de mitigación sigue Pass-the-Hash para reforzar el entorno. Para obtener más información, consulta [ataques Mitigating Pass-the-Hash (PtH) y otras técnicas de robo de credenciales](https://www.microsoft.com/download/details.aspx?id=36036).  
  
-   Ninguno de los controladores de dominio puede replicar con sus asociados de replicación.  
  
-   No se pueden realizar cambios en AD DS en cualquier controlador de dominio.  
  
-   Nuevos controladores de dominio no se puede instalar en cualquier dominio.  
  
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
