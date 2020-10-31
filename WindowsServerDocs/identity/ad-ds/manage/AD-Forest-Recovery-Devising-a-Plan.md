---
title: 'Recuperación del bosque de AD: diseño de un plan de recuperación de bosques de AD'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.openlocfilehash: 66c63483b118d9d1e04dc4067725fcc82fe8d082
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071337"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Recuperación del bosque de AD: diseño de un plan de recuperación de bosques de AD

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

En función de su entorno y de sus requisitos empresariales, puede que tenga que realizar todos los pasos descritos en esta guía para realizar una recuperación correcta del bosque. Dado que esta guía solo sirve como plantilla para la recuperación del bosque, es fundamental que diseñe un plan de recuperación de bosques personalizado que se ajuste a su entorno y satisfaga sus necesidades empresariales.

Por ejemplo, en el plan de recuperación del bosque, debe tener un mapa de topología detallado de los bosques. El mapa debe mostrar toda la información sobre los controladores de seguridad, como sus nombres, sus roles y el estado de la copia de seguridad, y las relaciones de confianza entre ellos. Para obtener una herramienta que puede usar para crear un mapa de topología, consulte [Microsoft Active Directory Topology](https://www.microsoft.com/download/details.aspx?id=13380).

Debe practicar el plan de recuperación del bosque al menos una vez al año. Además, se recomienda realizar una exploración de la recuperación del bosque cuando haya cambios de pertenencia al grupo administradores de organización o Admins. del dominio. Esto ayuda a garantizar que el personal de tecnologías de la información (TI) comprenda totalmente el plan de recuperación de bosques.

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
