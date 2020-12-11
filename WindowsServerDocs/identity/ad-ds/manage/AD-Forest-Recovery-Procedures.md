---
description: 'Más información acerca de la recuperación del bosque de AD: procedimientos'
title: 'Recuperación del bosque de AD: procedimientos'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.openlocfilehash: 9ab44e3dd8927387cc6dc2922eec75387c4b1631
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041753"
---
# <a name="ad-forest-recovery---procedures"></a>Recuperación del bosque de AD: procedimientos

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Esta sección contiene los procedimientos relacionados con el proceso de recuperación del bosque. Los procedimientos se aplican a Windows Server 2016, 2012 R2, 2012 y también son aplicables a Windows Server 2008 R2 y a 2008 con algunas excepciones menores.

Los procedimientos que incluyen pasos que varían en Windows Server 2003 se encuentran en la [recuperación de bosque con controladores de dominio de Windows server 2003](AD-Forest-Recovery-Windows-Server-2003.md).

A continuación se muestra una lista de los procedimientos que se usan para realizar copias de seguridad y restaurar controladores de dominio y Active Directory.

- [Copia de seguridad de un servidor completo](AD-Forest-Recovery-Backing-up-a-Full-Server.md)
- [Copia de seguridad de los datos de estado del sistema](AD-Forest-Recovery-Backing-up-System-State.md)
- [Realización de una recuperación completa del servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md)
- [Realización de una sincronización autoritativa de SYSVOL replicada en DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Realizar una restauración no autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md)

En estos pasos se explica cómo realizar una restauración autoritativa de SYSVOL al mismo tiempo.

- [Configuración del servicio servidor DNS](AD-Forest-Recovery-Configure-DNS.md)
- [Quitar el catálogo global](AD-Forest-Recovery-Remove-GC.md)
- [Aumentar el valor de los grupos de RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md)
- [Invalidar el grupo de RID actual](AD-Forest-Recovery-Invaildate-RID-Pool.md)
- [Asunción de una función de maestro de operaciones](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)
- [Limpiar después de una restauración](AD-Forest-Recovery-Cleanup.md)
- [Limpiar metadatos de controladores de dominio de escritura eliminados](AD-Forest-Recovery-Cleaning-Metadata.md)
- [Restablecer la contraseña de la cuenta de equipo del controlador de dominio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)
- [Restablecer la contraseña de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)
- [Restablecer una contraseña de confianza en un lado de la confianza](AD-Forest-Recovery-Reset-Trust.md)
- [Agregar el catálogo global](AD-Forest-Recovery-Add-GC.md)
- [Recursos para comprobar que la replicación funciona](AD-Forest-Recovery-Verify-Replication.md)

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)
- [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)
- [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
- [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)
- [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
