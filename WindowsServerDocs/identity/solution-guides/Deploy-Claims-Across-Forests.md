---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Implementar notificaciones entre bosques
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e47804b9e194706d3b30787a1d6ae4b907467af1
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="deploy-claims-across-forests"></a>Implementar notificaciones entre bosques

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En Windows Server 2012, un tipo de notificación es una aserción sobre el objeto con el que está asociado. Los tipos de notificación se definen por bosque de Active Directory. Existen muchos escenarios donde una entidad de seguridad posible que tengas que recorrer un límite de confianza para acceder a recursos en un bosque de confianza. Transformación entre bosques notificaciones en Windows Server 2012 le permite transformar reclamaciones de salida y entrada que recorren bosques para que las notificaciones reconoce y acepta en los bosques de confianza y que confían. Algunos de los escenarios reales de transformación de una reclamación son:  
  
-   Bosques de confianza pueden utilizar reclamación transformación como una protección contra ataques de elevación de privilegios filtrando las notificaciones entrantes con valores específicos.  
  
    Bosques de confianza, también pueden emitir reclamaciones por entidades próximamente sobre un límite de confianza si el bosque de confianza no admite o emitir cualquier reclamación.  
  
-   Bosques de confianza pueden utilizar la transformación de notificación para evitar determinados tipos de notificaciones y notificaciones con ciertos valores de destinada al bosque de confianza.  
  
-   También puedes usar la solicitud de notificación de transformación para asignar diferentes tipos entre bosques de confianza y que confían. Esto puede usarse para generalizar el tipo de notificación, el valor de notificación o ambos. Sin esto, debes normalizar los datos entre los bosques antes de poder usar las notificaciones. Generalización de reclamaciones entre los bosques de confianza y que confían reduce los costos de TI.  
  
## <a name="claim-transformation-rules"></a>Reglas de transformación de notificación  
La sintaxis de lenguaje de transformación regla divide una sola regla en dos partes principales: una serie de instrucciones condicionales y la declaración del problema. Cada instrucción condicional tiene dos subcomponentes: el identificador de solicitud y la condición. La declaración del problema contiene palabras clave, delimitadores y una expresión del problema. La instrucción condicional, opcionalmente, comienza con una variable de identificador de notificación, que representa la solicitud de entrada coincidente. Comprueba la condición de la expresión. Si la solicitud de entrada no coincide con la condición, el motor de transformación omite la declaración del problema y evalúa la siguiente solicitud de entrada con la regla de transformación. Si todas las condiciones coincide con la solicitud de entrada, se procesa la declaración del problema.  
  
Para obtener información detallada sobre el lenguaje de reglas de notificación, consulta [reglas de lenguaje de transformación de reclamaciones](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Vinculación de directivas de transformación de notificación a los bosques  
Hay dos componentes implicados en la configuración de directivas de la transformación de Reclamación: reclamar objetos de directiva de transformación y el vínculo de transformación. Los objetos de directiva live en el contexto de nomenclatura de configuración en un bosque y contienen información de asignación para las notificaciones. Especifica el vínculo que confiar y bosques de confianza se aplica la asignación.  
  
Es importante determinar si el bosque es el bosque de confianza o que confían porque se trata de base para vincular objetos de directiva de transformación. Por ejemplo, el bosque de confianza es el bosque que contiene las cuentas de usuario que requieren acceso. El bosque de confianza es el bosque que contiene los recursos que quieras dar a los usuarios acceso a. Reclamaciones de viajes en la misma dirección que la entidad de seguridad que requiere acceso a la seguridad. Por ejemplo, si hay una confianza unidireccional del bosque contoso.com al bosque adatum.com, las notificaciones fluya de adatum.com a contoso.com, que permite a los usuarios de adatum.com acceder a recursos en contoso.com.  
  
De manera predeterminada, un bosque de confianza permite todas las notificaciones salientes a pasar y un bosque de confianza quita todas las notificaciones entrantes que recibe.  
  
## <a name="in-this-scenario"></a>En este escenario  
La siguiente guía está disponible para este escenario:  
  
-   [Implementar notificaciones entre bosques & #40; pasos de demostración & #41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Lenguaje de las reglas de transformación de notificaciones](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Roles y características incluidas en este escenario  
La siguiente tabla enumera los roles y características que forman parte de este escenario y describe cómo apoyan.  
  
|Rol o característica|¿Cómo admite este escenario|  
|-----------------|---------------------------------|  
|Servicios de dominio de Active Directory|En este escenario, debe configurar dos bosques de Active Directory con confianza bidireccional. Tienes notificaciones en ambos bosques. También establecen directivas de acceso central en el bosque de confianza donde residen los recursos.|  
|Rol de servicios de archivos y almacenamiento|En este escenario, la clasificación de datos se aplica a los recursos en los servidores de archivos. Se aplica la directiva de acceso central en la carpeta donde quieres conceder acceso de usuario. Tras la transformación, la reclamación concede acceso de usuario a los recursos en función de la directiva de acceso central que se aplica a la carpeta en el servidor de archivos.|  
  


