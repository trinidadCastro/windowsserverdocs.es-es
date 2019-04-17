---
title: "Recuperación de bosque de AD - procedimientos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adfs
ms.openlocfilehash: 91e3954c05fe3cd12d35b5db91afd29fb3a31e00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2017
---
# <a name="ad-forest-recovery---procedures"></a>Recuperación de bosque de AD - procedimientos


>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Esta sección contiene procedimientos relacionados con el proceso de recuperación del bosque. Los procedimientos son aplicables a Windows Server 2016, 2012 R2, 2012 y también son aplicables a Windows Server 2008 R2 y 2008 con algunas excepciones menores. 

Procedimientos que incluyen pasos que varían de Windows Server 2003 se encuentran en [recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

La siguiente es una lista de procedimientos que se usan en la copia de seguridad y restauración de controladores de dominio y Active Directory.
  
-   [Hacer copias de seguridad completa del servidor](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
-   [Copia los datos de estado del sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
-   [Realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
-   [Realizar una sincronización autorizada de SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
-   [Realizar una restauración no autorizada de los servicios de dominio de Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  
  
     Estos pasos explica cómo realizar una restauración de SYSVOL al mismo tiempo.  
-   [Configurar el servicio de servidor DNS](AD-Forest-Recovery-Configure-DNS.md)  
-   [Eliminar el catálogo global](AD-Forest-Recovery-Remove-GC.md)  
-   [Generar el valor de grupos RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md)  
-   [Invalidar el grupo de RID actual](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
-   [Asumir un rol de maestro de operaciones](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
-   [Limpiar tras una restauración](AD-Forest-Recovery-Cleanup.md)
-   [Limpieza de metadatos de controladores de dominio grabable quitado](AD-Forest-Recovery-Cleaning-Metadata.md)  
-   [Restablecer la contraseña de cuenta de equipo del controlador de dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
-   [Restablecer la contraseña de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
-   [Restablecer una contraseña de confianza a un lado de la relación de confianza](AD-Forest-Recovery-Reset-Trust.md)  
-   [Agregar el catálogo global](AD-Forest-Recovery-Add-GC.md)  
-   [Recursos para comprobar la replicación funciona](AD-Forest-Recovery-Verify-Replication.md)  
  
  
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
