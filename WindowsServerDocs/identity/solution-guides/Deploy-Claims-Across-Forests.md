---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Implementación de notificaciones en bosques
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890656"
---
# <a name="deploy-claims-across-forests"></a>Implementación de notificaciones en bosques

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server 2012, un tipo de notificación es una aserción sobre el objeto con el que está asociada. Los tipos de notificación se definen por cada bosque en Active Directory. Hay muchos escenarios en los que puede ser necesario que una entidad de seguridad atraviese un límite de confianza para tener acceso a los recursos de un bosque de confianza. Transformación de notificaciones entre bosques en Windows Server 2012 le permite transformar las notificaciones de entrada y salida que atraviesan bosques para que las notificaciones se ha reconocido y aceptadas en los bosques que confían y de confianza. Algunos de los escenarios reales para la transformación de notificaciones son:  
  
-   Los bosques que confían pueden usar la transformación de notificaciones como una protección frente a la elevación de privilegios mediante el filtrado de las notificaciones entrantes con valores específicos.  
  
    Los bosques que confían también pueden emitir notificaciones para las entidades de seguridad que llegan a un límite de confianza si el bosque de confianza no admite ni emite las notificaciones.  
  
-   Los bosques de confianza pueden usar la transformación de notificaciones para evitar que determinados tipos de notificaciones y notificaciones con ciertos valores salgan del bosque que confía.  
  
-   También puedes usar la transformación de notificaciones para asignar tipos diferentes de notificaciones entre bosques que confían y de confianza. Esto puede servir para generalizar el tipo de notificación, el valor de la notificación o ambos aspectos. Sin esto, tendrás que normalizar los datos entre los bosques antes de poder usar las notificaciones. Generalizar las notificaciones entre los bosques que confían y de confianza reduce los costos de TI.  
  
## <a name="claim-transformation-rules"></a>Reglas de transformación de notificaciones  
La sintaxis del lenguaje de las reglas de transformación divide una sola regla en dos partes principales: una serie de instrucciones de condición y la instrucción del problema. Cada instrucción de condición tiene dos subcomponentes: el identificador de notificaciones y la condición. La instrucción del problema contiene palabras clave, delimitadores y una expresión de problema. Opcionalmente, la instrucción de condición comienza con una variable de identificador de notificación, que representa la notificación de entrada coincidente. La condición comprueba la expresión. Si la notificación de entrada no coincide con la condición, el motor de transformación omite la instrucción del problema y evalúa la siguiente notificación de entrada con la regla de transformación. Si todas las condiciones coinciden con la notificación de entrada, procesará la instrucción del problema.  
  
Para obtener información detallada sobre el lenguaje de reglas de notificación, consulta [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Vinculación de las directivas de transformación de notificaciones a los bosques  
Hay dos componentes implicados en la configuración de directivas de transformación de notificaciones: los objetos de directiva de transformación de notificaciones y el enlace de la transformación. Los objetos de directiva residen en el contexto de nomenclatura de configuración en un bosque y contienen información de asignación para las notificaciones. El vínculo especifica a qué bosques que confían y de confianza se aplica la asignación.  
  
Es importante comprender si el bosque es el bosque que confía o de confianza, porque esto es la base para vincular objetos de directiva de transformación. Por ejemplo, el bosque de confianza es el bosque que contiene las cuentas de usuario que requieren acceso. El bosque que confía es el bosque que contiene los recursos a los que quieres dar acceso a los usuarios. Las notificaciones viajan en la misma dirección que la entidad de seguridad que requiere acceso. Por ejemplo, si hay una confianza unidireccional desde el bosque contoso.com al bosque adatum.com, las notificaciones se desplazarán desde adatum.com hasta contoso.com, lo que permite a los usuarios de adatum.com tener acceso a los recursos de contoso.com.  
  
De forma predeterminada, un bosque de confianza permite a todas las notificaciones salientes que pasen y un bosque que confía coloca todas las notificaciones entrantes que recibe.  
  
## <a name="in-this-scenario"></a>En este escenario  
Para este escenario está disponible la guía siguiente:  
  
-   [Implementar notificaciones en bosques &#40;pasos de demostración&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Lenguaje de reglas de transformación de notificaciones](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Roles y características que se incluyen en este escenario  
En la tabla siguiente, se enumeran los roles y las características que forman parte de este escenario y se describe la manera en que son compatibles con él.  
  
|Rol/característica|Compatibilidad con este escenario|  
|-----------------|---------------------------------|  
|Active Directory Domain Services|En este escenario, tienes que configurar dos bosques de Active Directory con una confianza bidireccional. Tienes notificaciones en ambos bosques. También puedes establecer directivas de acceso central en el bosque que confía en el que residen los recursos.|  
|Rol de servicios de archivos y almacenamiento|En este escenario, se aplica la clasificación de datos a los recursos de los servidores de archivos. La directiva de acceso central se aplica a la carpeta donde quieres conceder el acceso al usuario. Después de la transformación, la notificación concede acceso de usuario a los recursos basándose en la directiva de acceso central que se aplica a la carpeta del servidor de archivos.|  
  


