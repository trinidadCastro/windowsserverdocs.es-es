---
title: 'Recuperación del bosque de AD: procedimientos'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: da45e3b20c370a2a37b0eab31a78216434dd60be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827726"
---
# <a name="ad-forest-recovery---procedures"></a>Recuperación del bosque de AD: procedimientos

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Esta sección contiene procedimientos relacionados con el proceso de recuperación del bosque. Los procedimientos son aplicables a Windows Server 2016, 2012 R2, 2012 y también son aplicables a Windows Server 2008 R2 y 2008 con algunas excepciones menores.

Los procedimientos que se incluyen los pasos que varían para Windows Server 2003 se encuentran en [recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

La siguiente es una lista de procedimientos que se usan en la copia de seguridad y restauración de controladores de dominio y Active Directory.

- [Copia de seguridad completa del servidor](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [Copia los datos de estado del sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
- [Realizar una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [Realizar una sincronización autoritativa de SYSVOL replicado mediante DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Realizar una restauración no autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

Estos pasos explica cómo realizar una restauración autoritativa de SYSVOL al mismo tiempo.  

- [Configurar el servicio servidor DNS](AD-Forest-Recovery-Configure-DNS.md)  
- [Quitar el catálogo global](AD-Forest-Recovery-Remove-GC.md)  
- [Generar el valor de los grupos de RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [Invalidar el grupo RID actual](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [Asumir una función de maestro de operaciones](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [La limpieza tras una restauración](AD-Forest-Recovery-Cleanup.md)
- [Limpieza de metadatos de controladores de dominio grabables quitados](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [Restablecer la contraseña de cuenta de equipo del controlador de dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [Restablecer la contraseña de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [Restablecer una contraseña de confianza en un lado de la relación de confianza](AD-Forest-Recovery-Reset-Trust.md)  
- [Adición del catálogo global](AD-Forest-Recovery-Add-GC.md)  
- [Recursos para comprobar que funciona la replicación](AD-Forest-Recovery-Verify-Replication.md)  

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
