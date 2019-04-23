---
title: 'Recuperación del bosque de AD: identificar el problema'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: cb3cf45f778ff305c8fe5aad5e23f0257650d6f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865526"
---
# <a name="identify-the-problem"></a>Identificar el problema

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
  
Cuando se muestren síntomas de un error en todo el bosque, como en los registros de eventos o en otras soluciones de supervisión, trabajar con Microsoft Support para determinar la causa del error y evaluar las posibles soluciones.  

## <a name="examples-of-forest-wide-failures"></a>Ejemplos de errores de todo el bosque

- Todos los controladores de dominio se han dañado lógicamente o dañada físicamente a un punto que es imposible; continuidad del negocio Por ejemplo, todas las aplicaciones empresariales que dependen de AD DS son funcionales.  
- Un administrador no autorizado ha puesto en peligro el entorno de Active Directory.  
- Un atacante intencionadamente, o un administrador accidentalmente, se ejecuta un script que se propaga a daños en los datos en todo el bosque.  
- Un atacante intencionadamente, o un administrador accidentalmente, extiende el esquema de Active Directory con los cambios en conflicto o malintencionados.  
- Un atacante ha logrado instalar software malintencionado en los controladores de dominio y se haya notificado por Microsoft Support para recuperarse del bosque de copia de seguridad.  
  
   > [!IMPORTANT]
   >  Este documento no abarca las recomendaciones de seguridad acerca de cómo recuperar un bosque que ha sido atacado o puesto en peligro. En general, se recomienda para las técnicas de mitigación de seguimiento Pass-the-Hash para proteger el entorno. Para obtener más información, consulte [ataques Mitigating Pass-the-Hash (PtH) y otras técnicas de robo de credenciales](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Ninguno de los controladores de dominio puede replicar con sus asociados de replicación.  
- No se pueden realizar cambios en AD DS en cualquier controlador de dominio.  
- No se puede instalar nuevos controladores de dominio en cualquier dominio.  
  
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
