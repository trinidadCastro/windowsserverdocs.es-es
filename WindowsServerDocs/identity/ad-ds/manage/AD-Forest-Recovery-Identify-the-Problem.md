---
title: 'Recuperación del bosque de AD: identificar el problema'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 73b7ef8dc6093ae28c4b5076e7b332b704090b4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823998"
---
# <a name="identify-the-problem"></a>Identificación del problema

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
  
Cuando aparezcan los síntomas de un error en todo el bosque, como en los registros de eventos u otras soluciones de supervisión, trabaje con Soporte técnico de Microsoft para determinar la causa del error y evalúe los posibles recursos.  

## <a name="examples-of-forest-wide-failures"></a>Ejemplos de errores en todo el bosque

- Todos los controladores de DC se han dañado lógicamente o están dañados físicamente hasta un punto en que no es posible la continuidad empresarial. por ejemplo, todas las aplicaciones empresariales que dependen de AD DS no son funcionales.  
- Un administrador no autorizado ha puesto en peligro el entorno de Active Directory.  
- Un atacante intencionadamente, o un administrador accidentalmente, ejecuta un script que distribuye los datos dañados en todo el bosque.  
- Un atacante, o un administrador accidentalmente, amplía el esquema de Active Directory con cambios malintencionados o en conflicto.  
- Un atacante ha administrado la instalación de software malintencionado en los controladores de seguridad y le ha informado Soporte técnico de Microsoft para recuperar el bosque de la copia de seguridad.  
  
   > [!IMPORTANT]
   >  En este documento no se tratan recomendaciones de seguridad sobre cómo recuperar un bosque que ha sido atacado o comprometido. En general, se recomienda seguir las técnicas de mitigación Pass-The-hash para reforzar el entorno. Para obtener más información, consulte [mitigar los ataques Pass-The-hash (PtH) y otras técnicas de robo de credenciales](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Ninguno de los controladores de DC puede replicarse con sus asociados de replicación.  
- No se pueden realizar cambios en AD DS en ningún controlador de dominio.  
- No se pueden instalar nuevos controladores de dominio en ningún dominio.  
  
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
