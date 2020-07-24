---
title: 'Recuperación del bosque de AD: recuperación de un solo dominio en un bosque de varios dominios'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: 84ce874ea85cce7ea562fcd5169902086732b4c4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960847"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Recuperación del bosque de AD: recuperación de un solo dominio en un bosque de varios dominios

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Puede haber ocasiones en las que sea necesario recuperar un solo dominio de un bosque que tenga varios dominios, en lugar de una recuperación de bosque completo. En este tema se tratan las consideraciones para recuperar un dominio individual y las posibles estrategias de recuperación.  
  
Una recuperación de un solo dominio presenta un desafío único para volver a generar los servidores del catálogo global (GC). Por ejemplo, si el primer controlador de dominio (DC) del dominio se restaura a partir de una copia de seguridad creada una semana antes, el resto de los catálogos globales del bosque tendrán datos más actualizados para ese dominio que el controlador de dominio restaurado. Para volver a establecer la coherencia de los datos de GC, hay un par de opciones:  
  
- Deshospede y, a continuación, vuelva a hospedar la partición de dominios recuperados de todos los catálogos globales del bosque, excepto los del dominio recuperado, al mismo tiempo.  
- Siga el proceso de recuperación del bosque para recuperar el dominio y, a continuación, quite los objetos persistentes de los GC de otros dominios.  
  
En las secciones siguientes se proporcionan consideraciones generales para cada opción. El conjunto completo de pasos que deben realizarse para la recuperación variará en función de los distintos entornos de Active Directory.  
  
## <a name="rehost-all-gcs"></a>Realojar todos los catálogos globales  

> [!WARNING]
> La contraseña de la cuenta de administrador de dominio para todos los dominios debe estar lista para usarse en caso de que un problema impida el acceso a un GC para el inicio de sesión.  

La rehospedación de todos los catálogos globales se puede realizar mediante los comandos repadmin/Unhost y repadmin/rehost (parte de repadmin/experthelp). Ejecutaría los comandos repadmin en cada GC de cada dominio que no se haya recuperado. Debe asegurarse de que todos los GC no contengan una copia del dominio recuperado. Para ello, primero debe desalojar la partición de dominio de todos los controladores de dominio en todos los dominios que no se recuperan del bosque. Después de que todos los catálogos globales no contengan la partición, puede volver a hospedarla. Al rehospedar, tenga en cuenta el sitio y la estructura de replicación del bosque; por ejemplo, finalice el rehost de un controlador de dominio por sitio antes de rehospedar los demás controladores de dominio de ese sitio.  
  
Esta opción puede ser ventajosa para una organización pequeña que tenga solo unos pocos controladores de dominio para cada dominio. Todos los catálogos globales se pueden volver a generar el viernes por la noche y, si es necesario, completar la replicación para todas las particiones de dominio de solo lectura antes del lunes de la mañana. Sin embargo, si necesita recuperar un dominio grande que abarque sitios de todo el mundo, el rehospedado de la partición de dominio de solo lectura en todos los catálogos globales de otros dominios puede afectar considerablemente a las operaciones y requerir un tiempo de inactividad.  
  
## <a name="remove-lingering-objects"></a>Quitar objetos persistentes

De forma similar al proceso de recuperación del bosque, restaure un controlador de dominio desde una copia de seguridad en el dominio que necesite recuperar, realice la limpieza de metadatos de los controladores de dominio restantes y, a continuación, vuelva a instalar AD DS para compilar el dominio. En los catálogos globales de todos los demás dominios del bosque, se quitan los objetos persistentes para la partición de solo lectura del dominio recuperado.  

El origen de la limpieza del objeto persistente debe ser un DC en el dominio recuperado. Para asegurarse de que el DC de origen no tiene ningún objeto persistente para las particiones de dominio, puede quitar el catálogo global si se trata de un GC.  

La eliminación de objetos persistentes es ventajosa para organizaciones de gran tamaño que no pueden arriesgar el tiempo de inactividad asociado a las otras opciones.  

Para obtener más información, vea [uso de repadmin para quitar objetos persistentes](/previous-versions/windows/it-pro/windows-server-2003/cc785298(v=ws.10)).

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
