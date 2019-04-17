---
title: "Virtualización de recuperación de bosque de AD"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 08b0c4a4c5ed230f91241fcfb67db1749935c5e2
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualización de recuperación de bosque de Active Directory

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Este tema describe la característica de clonación del controlador de dominio virtualizada de 2012, 2012 R2 y Windows Server 2016.  
 
## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Uso de clonación del controlador de dominio virtualizada para acelerar la recuperación de bosque  
 Controlador de dominio virtualizada clonación simplifica y acelera el proceso de instalación de controladores de dominio virtualizadas adicionales en un dominio, especialmente en ubicaciones centralizadas, como centros de datos donde se ejecutan varios controladores de dominio en hipervisores. Después de restaurar un controlador de dominio virtual en cada dominio de copia de seguridad, controladores de dominio adicionales en cada dominio rápidamente colocarse en línea utilizando el controlador de dominio virtualizada clonación proceso. Puedes preparar el primer controlador de dominio virtualizada que recuperar, apagarlo y, a continuación, copia ese disco duro virtual como tantas veces como sea necesario para crear clonados virtualizan los controladores de dominio para crear el dominio.  
  
 Los requisitos para clonar virtualizada de DC son:  
  
-   El hipervisor debe admitir VM-GenerationID. Hyper-V en Windows Server 2016, 2012 y Windows 8 es un ejemplo de un hipervisor que admite VM-GenerationID. Consulte con su proveedor de hipervisor si VM-GenerationID es compatible.  
  
-   El controlador de dominio virtualizada que se usa como origen para clonar debe ejecutar Windows Server 2016 o 2012 y ser miembro del grupo clonación controladores de dominio.  
  
-   El emulador PDC debe ejecutar Windows Server 2016 o 2012. Si se virtualiza los se puede duplicar emulador PDC.  
  
 Para obtener instrucciones paso a paso sobre cómo realizar virtualizan clonación del controlador de dominio, consulta [Introducción a la virtualización de los servicios de dominio de Active Directory (AD DS) (nivel 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Para obtener información detallada acerca de cómo virtualizado DC clonación funciona, consulta [virtualizados dominio controlador referencia técnica (nivel 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md).  

-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 

  
