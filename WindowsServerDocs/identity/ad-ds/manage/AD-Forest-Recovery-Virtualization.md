---
title: Virtualización de recuperación del bosque de AD
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 23317f55fdce18e78ac3e7e1490f6fc4937fd062
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858456"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualización de la recuperación de bosques de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Este tema describe la característica de clonación del controlador de dominio virtualizados en Windows Server 2016, 2012 R2 y 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Uso de clonación del controlador de dominio virtualizados para acelerar la recuperación de bosques

Clonación del controlador (DC) de dominio virtualizados, simplifica y acelera el proceso de instalación de controladores de dominio virtualizados adicionales en un dominio, especialmente en las ubicaciones centralizadas como centros de datos donde se ejecutan varios controladores de dominio en los hipervisores. Después de restaurar un controlador de dominio virtual en cada dominio de copia de seguridad, los controladores de dominio adicionales en cada dominio rápidamente poderse conectar utilizando el proceso de clonación de controlador de dominio virtualizado. Puede preparar el primer controlador de dominio virtualizado que recuperar, cerrarlo y, a continuación, copie este disco duro virtual como tantas veces como sea necesario para crear clonado virtualizar los controladores de dominio para crear el dominio.  
  
Los requisitos para la clonación de controlador de dominio virtualizados son:  
  
- El hipervisor debe admitir el VM-GenerationID. Hyper-V en Windows Server 2016, 2012 y Windows 8 es un ejemplo de un hipervisor que admita VM-GenerationID. Si se admite VM-GenerationID, compruebe con el fabricante del hipervisor.  
- El controlador de dominio virtualizado que se usa como origen para la clonación debe ejecutar Windows Server 2016 o 2012 y ser miembro del grupo de dominio clonables. 
- El emulador de PDC debe ejecutar Windows Server 2016 o 2012. Emulador de PDC se puede clonar si está virtualizado.  
  
Para obtener instrucciones paso a paso sobre cómo realizar virtualizado clonación del controlador de dominio, consulte [Introducción a la virtualización de servicios de dominio de Active Directory (AD DS) (nivel 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Para obtener información detallada acerca de cómo virtualizado funciona la clonación del controlador de dominio, consulte [virtualizados dominio referencia técnica de controladores (nivel 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
