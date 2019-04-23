---
title: Recuperación de bosques de AD - recuperar un único dominio en un bosque con varios dominios
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: fae2cc40af0b43dd38d72c2622720a6bb17b0a66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863476"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Recuperación de bosques de AD - recuperar un único dominio en un bosque con varios dominios

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Puede haber ocasiones en que es necesario recuperar un solo dominio de un bosque que tiene varios dominios, en lugar de una recuperación del bosque completo. En este tema cubre las consideraciones para recuperar un único dominio y las posibles estrategias de recuperación.  
  
Recuperación de un único dominio presenta un desafío único para volver a generar servidores de catálogo global (GC). Por ejemplo, si el primer controlador de dominio (DC) para el dominio se restaura desde una copia de seguridad que se creó una semana antes, a continuación, todos los otros catálogos globales del bosque tendrán más datos actualizados para el dominio que el controlador de dominio restaurado. Para volver a establecer la coherencia de datos de catálogo global, hay un par de opciones:  
  
- Unhost y, a continuación, vuelva a alojar la partición de dominios recuperados de todos los catálogos globales del bosque, excepto en el dominio recuperado, al mismo tiempo.  
- Siga el proceso de recuperación de bosque para recuperar el dominio y, a continuación, quitar los objetos persistentes de catálogos globales en otros dominios.  
  
Las secciones siguientes proporcionan consideraciones generales para cada opción. El conjunto completo de pasos que deben realizarse para la recuperación puede variar para diferentes entornos de Active Directory.  
  
## <a name="rehost-all-gcs"></a>Rehospedar todos los catálogos globales  

> [!WARNING]
> La contraseña de la cuenta de administrador de dominio para todos los dominios debe estar lista para su uso en caso de un problema impide el acceso a un catálogo global para el inicio de sesión.  

Se puede realizar el rehospedaje todos los catálogos globales con repadmin /unhost y comandos de repadmin /rehost (parte de repadmin /experthelp). Los comandos repadmin ejecutaría en cada catálogo global en cada dominio que no se ha recuperado. Debe garantizarse, que todos los catálogos globales no contienen una copia del dominio recuperada ya. Para lograr esto, unhost la partición de dominio en primer lugar de todos los controladores de dominio en todos los dominios recuperada ninguno del bosque en primer lugar. Después de que todos los catálogos globales ya no contiene la partición, se puede rehost. Cuándo el rehospedaje, considere la estructura sitio y replicación del bosque, por ejemplo, finalice el rehospedaje de un controlador de dominio por sitio antes de rehospedaje los otros controladores de dominio de ese sitio.  
  
Esta opción puede ser conveniente para una organización pequeña que tenga solo unos pocos controladores de dominio para cada dominio. Todos los catálogos globales se podrían volver a generar un viernes por la noche y, si es necesario, completar la replicación de dominio de solo lectura todas las particiones antes del lunes por la mañana. Pero si necesita recuperar un dominio grande que cubre los sitios en todo el mundo, rehospedaje la partición de dominio de solo lectura en todos los catálogos globales de otros dominios pueden significativamente las operaciones de impacto y potencialmente requieren tiempo de inactividad.  
  
## <a name="remove-lingering-objects"></a>Quitar los objetos persistentes

Al igual que el proceso de recuperación del bosque, restaurar un controlador de dominio de copia de seguridad en el dominio que necesite recuperar, realizar la limpieza de metadatos de controladores de dominio restantes y, a continuación, volver a instalar AD DS para crear el dominio. En los catálogos globales de todos los demás dominios del bosque, quite los objetos persistentes para la partición de solo lectura del dominio recuperada.  

El origen para la limpieza de objetos persistentes debe ser un controlador de dominio en el dominio recuperado. Para estar seguro de que el DC de origen no tiene todos los objetos persistentes para las particiones de dominio, puede quitar el catálogo global si fuese un GC.  

Es una ventaja para las organizaciones más grandes que el tiempo de inactividad asociado con las otras opciones de riesgo de no se pueden quitar objetos persistentes.  

Para obtener más información, consulte [Utilice Repadmin para quitar objetos persistentes](https://technet.microsoft.com/library/cc785298.aspx).

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
