---
title: Requisitos previos para la planeación de recuperación de bosque de Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c8945dd5ccccb27826dd96413b56a070a7452789
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842176"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Requisitos previos de recuperación de bosques de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

El siguiente documento se describen los requisitos previos que debe estar familiarizado con antes de idear un plan de recuperación del bosque o al intentar una recuperación.

## <a name="assumptions-for-using-this-guide"></a>Suposiciones para usar esta guía

1. Ha trabajado con un profesional de Microsoft Support y:
   - Determinar la causa del error de todo el bosque. Esta guía no sugerir una causa del error o recomienda los procedimientos descritos para evitar el error.
   - Evaluar las posibles soluciones.  
   - Finalizado, en colaboración con Microsoft Support, que restaurar todo el bosque a su estado antes de que se produjo el error es la mejor manera de recuperarse del error. En muchos casos, la recuperación del bosque debe ser la última opción.

2. Que ha seguido las recomendaciones de prácticas recomendadas de Microsoft para el uso del sistema de nombres de dominio (DNS) integrado en Active Directory. En concreto, debe haber una zona de DNS integrado en Active Directory para cada dominio de Active Directory. 
   - Si esto no es el caso, todavía puede usar los principios básicos de esta guía para realizar la recuperación del bosque. Sin embargo, deberá tomar medidas específicas para la recuperación DNS en función de su propio entorno. Para obtener más información sobre el uso de DNS integrado en Active Directory, consulte [crear un diseño de la infraestructura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

3. Aunque esta guía está pensada como una guía para la recuperación de bosque, genérica no se tratan todos los escenarios posibles. Por ejemplo, a partir de Windows Server 2008, hay una versión de Server Core, que es una versión completa de Windows Server sin una GUI completa. Aunque es ciertamente posible recuperarse de un bosque que consta de sólo los controladores de dominio que ejecutan Server Core, esta guía no tiene ningún instrucciones detalladas. Sin embargo, según las instrucciones que se tratan aquí podrá diseñar usted mismo las acciones de línea de comandos necesarias.  

> ![!NOTE]
> Aunque los objetivos de esta guía son recuperar el bosque y mantener o restaurar toda la funcionalidad DNS, puede provocar la recuperación en una configuración de DNS que ha cambiado la configuración antes del error. Una vez recuperado el bosque, puede revertir a la configuración de DNS original. Las recomendaciones de esta guía se describe cómo configurar los servidores DNS para llevar a cabo la resolución de nombres de otras partes del espacio de nombres corporativo donde hay zonas DNS que no se almacenan en AD DS.  

## <a name="concepts-for-using-this-guide"></a>Conceptos para utilizar esta guía

Antes de comenzar a planear la recuperación de un bosque de Active Directory, debe estar familiarizado con lo siguiente:  
  
- Conceptos básicos de Active Directory  
- La importancia de los roles de maestro de operaciones (también conocido como operaciones de maestro únicas flexible o FSMO). Estas funciones incluyen lo siguiente:  
   - Maestro de esquema
   - Maestro de nomenclatura de dominio
   - Maestro relativo (RID) de Id.
   - Maestro emulador PDC (controlador) de dominio principal
   - Maestro de infraestructura

Además, debe realizar una copia y restaurar AD DS y SYSVOL en un entorno de laboratorio de forma periódica. Para obtener más información, consulte [copia los datos de estado del sistema](AD-Forest-Recovery-Procedures.md) y [realizar una restauración no autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Procedures.md).

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
