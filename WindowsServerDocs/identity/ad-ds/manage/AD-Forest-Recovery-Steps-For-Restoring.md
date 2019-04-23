---
title: Recuperación de bosques de AD - pasos para restaurar el bosque
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 1712d3a636160f82495539afdd42ab2ee85ffae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861476"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Recuperación de bosques de AD - pasos para restaurar el bosque

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

En esta sección se proporciona información general de la ruta de acceso recomendada para la recuperación de un bosque. Los pasos de recuperación de bosque se describen en detalle más adelante.  
  
En la lista siguiente se resume los pasos de recuperación en un nivel alto:  
  
1. [Identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)  

   Trabajar con TI y Microsoft Support para determinar el ámbito del problema y las posibles causas y evaluar posibles soluciones con todas las partes interesadas del negocio. En muchos casos la recuperación de bosques total debe ser la última opción.  
  
2. [Decidir cómo recuperar el bosque](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Después de determinar que la recuperación del bosque es necesario, preliminar completa los pasos para preparar para él: determinar la estructura del bosque actual, identificar las funciones que cada controlador de dominio realiza, decidir qué controlador de dominio para restaurar para cada dominio y asegúrese de que todos los controladores de dominio grabables se realizan sin conexión.  

3. [Realizar recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  

   De forma aislada, recuperar un controlador de dominio para cada dominio, limpiarlos y volver a conectar los dominios. Restablecimiento de cuentas con privilegios y corregir problemas debidos a infracciones de seguridad en esta fase.  
  
4. [Volver a implementar restantes controladores de dominio](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Volver a implementar el bosque para devolverlo a su estado antes del error. Este paso deberá adaptarse a sus requisitos y diseño específico. Clonación del controlador de dominio virtualizados puede ayudar a acelerar este proceso.  

5. [Cleanup](AD-Forest-Recovery-Cleanup.md)  

   Una vez se haya restaurado la funcionalidad, volver a configurar la resolución de nombres según sea necesario y publique aplicaciones LOB para trabajar.  

Los pasos descritos en esta guía están diseñados para minimizar la posibilidad de volver a introducir datos peligrosos en el bosque recuperado. Es posible que deba modificar estos pasos para tener en cuenta factores tales como:  
  
- Escalabilidad  
- Capacidad de administración remota  
- Velocidad de recuperación  
