---
title: Recuperación de bosque de AD - recuperar un solo dominio en un bosque de varios dominios
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adfs
ms.openlocfilehash: 3a1c9d0671a732eee83aa707e061afdbbf106fa5
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2018
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Recuperación de bosque de AD - recuperar un solo dominio en un bosque de varios dominios

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Puede haber ocasiones en que es necesario recuperar un solo dominio dentro de un bosque que tiene varios dominios, en lugar de una recuperación completa del bosque. Este tema tratan las consideraciones para la recuperación de un solo dominio y posibles estrategias de recuperación.  
  
 Recuperación de un solo dominio presenta un desafío único para la reconstrucción de servidores de catálogo global (GC). Por ejemplo, si el primer controlador de dominio (corriente continua) para el dominio se restaura desde una copia de seguridad que se creó una semana anteriormente y, a continuación, todos los otros catálogos globales en el bosque tendrán más datos actualizados para ese dominio que el controlador de dominio restaurado. Para volver a establecer la coherencia de los datos GC, hay un par de opciones:  
  
-   Unhost y, a continuación, vuelva a alojar la partición de dominios recuperados desde todos los catálogos globales en el bosque, excepto las que están en el dominio recuperado, al mismo tiempo.  
  
-   Sigue el proceso de recuperación de bosque para recuperar el dominio y, a continuación, quite los objetos persistentes GC de otros dominios.  
  
 Las secciones siguientes proporcionan consideraciones generales de cada opción. El conjunto completo de los pasos que deben realizarse para la recuperación varían en función de diferentes entornos de Active Directory.  
  
## <a name="rehost-all-gcs"></a>Vuelva a alojar todos los catálogos globales  

> [!WARNING]
>  La contraseña de la cuenta de administrador de dominio para todos los dominios debe estar lista para su uso en caso de un problema que impide el acceso a un catálogo global para el inicio de sesión.  

 Todos los catálogos globales de cambio de host puede hacerse con repadmin//unhost y comandos de repadmin /rehost (parte de repadmin /experthelp). Los comandos repadmin ejecutaría en cada GC en cada dominio que no se recupera. Debe garantizarse, que todos los catálogos globales no contienen una copia del dominio recuperado ya. Para lograrlo, unhost la partición de dominio primero todos los controladores de dominio en todos los dominios recuperado ninguno del bosque en primer lugar. Después de que todos los catálogos globales no contienen ya la partición, puede vuelva a alojar. Cuando cambio de host, considera la posibilidad de la estructura sitio y replicación del bosque, por ejemplo, finalizar la rehost de un controlador de dominio por sitio antes de cambio de host de los otros controladores de dominio de ese sitio.  
  
 Esta opción puede ser muy ventajosa para una organización pequeña que tiene solo algunos controladores de dominio para cada dominio. Todos los GC podrían generarse un viernes por la noche y, si es necesario, replicación completa para el dominio de solo lectura todas las particiones antes del lunes por la mañana. Pero si es necesario recuperar un dominio grande que abarca los sitios en todo el mundo, la partición de dominio de solo lectura en todos los catálogos globales de cambio de host de otros dominios pueden significativamente las operaciones de impacto y potencialmente requieren tiempo de inactividad.  
  
## <a name="remove-lingering-objects"></a>Quitar los objetos persistentes  
 Similar al proceso de recuperación de bosque, restaurar un controlador de dominio de copia de seguridad del dominio que debes recuperar, realice la limpieza de metadatos de controladores de dominio restantes y, a continuación, volver a instalar AD DS para crear el dominio. En los catálogos globales de todos los demás dominios del bosque, quitarán los objetos persistentes para la partición de solo lectura del dominio recuperado.  
  
 El origen para la limpieza de objeto persistentes debe ser un controlador de dominio en el dominio recuperado. Para asegurarse de que el origen de controlador de dominio no tiene todos los objetos persistentes para las particiones de dominio, puedes quitar el catálogo global si fuera un catálogo global.  
  
 Eliminación de objetos persistentes resulta ventajoso para organizaciones grandes que no se el riesgo del tiempo de inactividad asociado con las otras opciones.  
  
 Para obtener más información, consulta [Repadmin de uso para quitar los objetos persistentes](https://technet.microsoft.com/library/cc785298.aspx).

## <a name="next-steps"></a>Pasos siguientes
-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
-   [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
