---
title: 'Recuperación del bosque de AD: pasos para restaurar el bosque'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 537543bedd68bff002054f637d96240a71f75793
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823408"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Recuperación del bosque de AD: pasos para restaurar el bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

En esta sección se proporciona información general sobre la ruta de acceso recomendada para recuperar un bosque. Los pasos de recuperación del bosque se describen en detalle más adelante.  
  
En la lista siguiente se resumen los pasos de recuperación en un nivel alto:  
  
1. [Identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)  

   Trabaje con ella y Soporte técnico de Microsoft para determinar el ámbito del problema y las posibles causas, y evalúe posibles soluciones con todas las partes interesadas de la empresa. En muchos casos, la recuperación total del bosque debe ser la última opción.  
  
2. [Decidir cómo recuperar el bosque](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Después de determinar que la recuperación del bosque es necesaria, complete los pasos preliminares para prepararla: Determine la estructura del bosque actual, identifique las funciones que realiza cada controlador de dominio, decida qué DC restaurar para cada dominio y asegúrese de que todos los controladores de dominio que se pueden escribir están sin conexión.  

3. [Realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  

   En aislamiento, recupere un controlador de dominio para cada dominio, límpielo y vuelva a conectar los dominios. Restablezca las cuentas con privilegios y rectifique los problemas causados por las infracciones de seguridad en esta fase.  
  
4. [Volver a implementar los controladores de DC restantes](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Vuelva a implementar el bosque para devolverlo a su estado anterior al error. Este paso tendrá que adaptarse a su diseño y sus requisitos específicos. La clonación de controladores de dominio virtualizados puede ayudar a acelerar este proceso.  

5. [Reparación](AD-Forest-Recovery-Cleanup.md)  

   Una vez restaurada la funcionalidad, vuelva a configurar la resolución de nombres según sea necesario y consiga que las aplicaciones LOB funcionen.  

Los pasos de esta guía están diseñados para minimizar la posibilidad de reintroducir datos peligrosos en el bosque recuperado. Es posible que tenga que modificar estos pasos para tener en cuenta tales factores como:  
  
- Escalabilidad  
- Capacidad de administración remota  
- Velocidad de recuperación  
