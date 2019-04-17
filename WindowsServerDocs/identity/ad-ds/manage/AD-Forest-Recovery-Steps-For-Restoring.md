---
title: "Recuperación de bosque de AD - pasos para restaurar el bosque"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 29988c262897649173e039cc052bb64f38a1a0ca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
#<a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Recuperación de bosque de AD - pasos para restaurar el bosque 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Esta sección proporciona una visión general de la ruta de acceso recomendada para recuperar un bosque. Los pasos de recuperación de bosque se describen con detalle más adelante.  
  
 La siguiente lista resume los pasos de recuperación en un nivel alto:  
  
1.  [Identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)  
  
     Trabajar con TI y Microsoft Support para determinar el ámbito del problema y causas posibles y evaluar las soluciones posibles con todas las partes interesadas. En muchos casos, recuperación de bosque total debe ser la última opción.  
  
2.  [Decide cómo recuperar el bosque](AD-Forest-Recovery-Determine-how-to-Recover.md)  
  
     Después de determinar que es necesaria la recuperación de bosque, preliminar completa los pasos para preparar para ella: determinar la estructura del bosque actual, identificar las funciones que cada controlador de dominio realiza, decidir qué controlador de dominio para restaurar para cada dominio y garantizar que todos los controladores de dominio grabables se toman sin conexión.  
  
3.  [Realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
  
     De manera aislada, recuperar un controlador de dominio para cada dominio, limpiarlos y vuelve a conectar los dominios. Restablecer cuentas con privilegios y solucionar problemas causados por infracciones de seguridad en esta fase.  
  
4.  [Volver a implementar restante controladores de dominio](AD-Forest-Recovery-Restore-Additional-DCs.md)  
  
     Vuelve a implementar el bosque para devolverlo al estado que tenía antes del error. Este paso se deba adaptarse a tu diseño específico y los requisitos. Controlador de dominio virtualizada clonación puede ayudar a facilitar este proceso.  
  
5.  [Liberador de espacio](AD-Forest-Recovery-Cleanup.md)  
  
     Una vez restaurada la funcionalidad, volver a configurar la resolución de nombres según sea necesario y obtener aplicaciones de LOB trabajar.  

  
 Los pasos descritos en esta guía están diseñados para minimizar la posibilidad de reintroducir peligrosos datos en el bosque recuperado. Es posible que debas modificar estos pasos para tener en cuenta algunos factores, como:  
  
-   Escalabilidad  
-   Administración remota  
-   Velocidad de recuperación  

