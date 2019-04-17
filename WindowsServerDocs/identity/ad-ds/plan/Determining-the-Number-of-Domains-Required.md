---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: "Determinar el número de dominios necesarios"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b65c792929a29c959244d1da83b4882f15fa2935
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="determining-the-number-of-domains-required"></a>Determinar el número de dominios necesarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada bosque comienza con un solo dominio. El número máximo de usuarios que puede contener un bosque único dominio se basa en el vínculo más lento que debe adaptarse la replicación entre controladores de dominio y el ancho de banda disponible que quieras asignar a los servicios de dominio de Active Directory (AD DS). La siguiente tabla enumera la máxima recomendada número de usuarios que puede contener un dominio en función de un bosque de dominio único, la velocidad del vínculo más lento y el porcentaje del ancho de banda que quieras reservar de replicación. Esta información se aplica a los bosques que contienen un máximo de 100.000 usuarios y que tengan una conectividad de 28,8 kilobits por segundo (Kbps) o superior. Para obtener recomendaciones que se aplican a bosques que contienen más de 100.000a los usuarios o la conectividad de menos de 28,8 Kbps, consultar un diseñador experimentado de Active Directory. Los valores en la siguiente tabla se basan en el tráfico de replicación generado en un entorno que tiene las siguientes características:  
  
-   Los nuevos usuarios unir el bosque a una velocidad de 20 por ciento al año.  
  
-   Los usuarios salir del bosque a una velocidad de 15 por ciento por año.  
  
-   Cada usuario es un miembro de cinco grupos globales y cinco universal.  
  
-   La proporción de equipos a los usuarios es 1:1.  
  
-   Se usa Active Directory integrado nombre sistema dominio (DNS).  
  
-   Se usa el borrado de DNS.  
  
> [!NOTE]  
> En las figuras enumeradas en la siguiente tabla son aproximaciones. La cantidad de tráfico de replicación depende en gran medida el número de cambios realizados en el directorio en un determinado período de tiempo. Confirma que la red puede acomodar el tráfico de replicación comprobando la cantidad estimada y la velocidad de los cambios en el diseño en un laboratorio antes de implementar los dominios.  
  
|Vínculo más lenta que se conecta un controlador de dominio (Kbps)|Número máximo de usuarios si está disponible el ancho de banda de % 1|Número máximo de usuarios si está disponible el ancho de banda de % 5|Número máximo de usuarios si está disponible el ancho de banda de 10 por ciento|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Para usar esta tabla:  
  
1.  En la **más lenta vínculo que se conecta un controlador de dominio** columna, busque el valor que coincida con la velocidad del vínculo más lento en la cual se replicarán AD DS en el dominio.  
  
2.  En la fila que corresponde a la velocidad del vínculo, busque la columna que representa el ancho de banda de porcentaje que desea asignar en AD DS. El valor en esa ubicación es el número máximo de usuarios que puede contener el dominio en el bosque de un solo dominio.  
  
Si determinas que el número total de usuarios en el bosque es menor que el número máximo de usuarios que puede contener tu dominio, puedes usar un solo dominio. Asegúrate de acomodar planeada crecimiento futuro cuando averiguarlo. Si determinas que el número total de usuarios en el bosque es mayor que el número máximo de usuarios que puede contener tu dominio, necesaria para reservar un mayor porcentaje del ancho de banda para la replicación, aumentar la velocidad del vínculo, o dividir la organización en dominios regionales.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>Dividir la organización en dominios regionales  
Si no se puede dar cabida a todos los usuarios en un solo dominio, debes seleccionar el modelo de dominio regional. Dividir la organización en regiones de manera que tenga sentido para la organización y la red existente. Por ejemplo, podrías crear regiones en función de los límites continentales.  
  
Ten en cuenta que dado que necesitas crear un dominio de Active Directory para cada región que usted establezca, te recomendamos minimizar el número de regiones que defines de AD DS. Aunque es posible incluir un número ilimitado de dominios en un bosque, para facilitar la administración te recomendamos que un bosque de incluir los dominios no más de 10. Se debe establecer el equilibrio adecuado entre optimizar el ancho de banda de replicación y minimizar la complejidad administrativa al dividir la organización en dominios regionales.  
  
En primer lugar, determina el número máximo de usuarios que puede hospedar del bosque. Base esto en el vínculo más lento en el bosque a través de la cantidad media de ancho de banda que quieras asignar a la réplica de Active Directory y replicarán los controladores de dominio. La siguiente tabla enumera la máxima recomendada número de usuarios que puede contener un bosque. Esto se basa en la velocidad del vínculo más lento y el ancho de banda de porcentaje que quieras reservar de replicación. Esta información se aplica a los bosques que contienen un máximo de 100.000 usuarios y que tengan una conectividad de 28,8 Kbps o superior. Los valores en la siguiente tabla se basan en los siguientes supuestos:  
  
-   Todos los controladores de dominio son servidores de catálogo global.  
  
-   Los nuevos usuarios unir el bosque a una velocidad de 20 por ciento al año.  
  
-   Los usuarios salir del bosque a una velocidad de 15 por ciento por año.  
  
-   Los usuarios son miembros de grupos globales cinco y cinco grupos universales.  
  
-   La proporción de equipos a los usuarios es 1:1.  
  
-   Se usa DNS de Active Directory integrado.  
  
-   Se usa el borrado de DNS.  
  
> [!NOTE]  
> En las figuras enumeradas en la siguiente tabla son aproximaciones. La cantidad de tráfico de replicación depende en gran medida el número de cambios realizados en el directorio en un determinado período de tiempo. Confirma que la red puede acomodar el tráfico de replicación comprobando la cantidad estimada y la velocidad de los cambios en el diseño en un laboratorio antes de implementar los dominios.  
  
|Vínculo más lenta que se conecta un controlador de dominio (Kbps)|Número máximo de usuarios si está disponible el ancho de banda de % 1|Número máximo de usuarios si está disponible el ancho de banda de % 5|Número máximo de usuarios si está disponible el ancho de banda de 10 por ciento|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Para usar esta tabla:  
  
1.  En la **más lenta vínculo que se conecta un controlador de dominio** columna, busque el valor que coincida con la velocidad del vínculo más lento en la cual se replicarán AD DS en el bosque.  
  
2.  En la fila que corresponde a la velocidad del vínculo, busque la columna que representa el ancho de banda de porcentaje que quieras asignar a AD DS. El valor en esa ubicación es el número máximo de usuarios que puede hospedar del bosque.  
  
Si es mayor que el número de usuarios que necesitas para hospedar el número máximo de usuarios que puede hospedar del bosque, un bosque único funcionarán para el diseño. Si necesitas hospedar más usuarios que el número máximo que identificaste, necesaria para aumentar la velocidad del vínculo mínimo, asignar un mayor porcentaje del ancho de banda de AD DS, o implementar bosques adicionales.  
  
Si determinas que un bosque único aceptará al número de usuarios que necesitas para hospedar, el siguiente paso es determinar el número máximo de usuarios que cada región puede admitir basándose en el vínculo más lento de esa región. Dividir el bosque en regiones que tengan sentido para TI. Asegúrate de que basar tu decisión en algo que no es probable que cambie. Por ejemplo, usa continentes en lugar de regiones de ventas. Estas regiones serán la base de la estructura del dominio cuando se ha identificado el número máximo de usuarios.  
  
Determinar el número de usuarios que necesiten pueden hospedarse en cada región y, a continuación, comprueba que no superan el máximo permitido en función de la velocidad del vínculo y el ancho de banda asignada a AD DS en esa región. La siguiente tabla enumera la máxima recomendada número de usuarios que puede contener un dominio regional. Se basa en la velocidad del vínculo más lento y el porcentaje del ancho de banda que quieras reservar de replicación. Esta información se aplica a los bosques que contienen un máximo de 100.000 usuarios y que tengan una conectividad de 28,8 Kbps o superior. Los valores en la siguiente tabla se basan en los siguientes supuestos:  
  
-   Todos los controladores de dominio son servidores de catálogo global.  
  
-   Los nuevos usuarios unir el bosque a una velocidad de 20 por ciento al año.  
  
-   Los usuarios salir del bosque a una velocidad de 15 por ciento por año.  
  
-   Los usuarios son miembros de grupos globales cinco y cinco grupos universales.  
  
-   La proporción de equipos a los usuarios es 1:1.  
  
-   Se usa DNS de Active Directory integrado.  
  
-   Se usa el borrado de DNS.  
  
> [!NOTE]  
> En las figuras enumeradas en la siguiente tabla son aproximaciones. La cantidad de tráfico de replicación depende en gran medida el número de cambios realizados en el directorio en un determinado período de tiempo. Confirma que la red puede acomodar el tráfico de replicación comprobando la cantidad estimada y la velocidad de los cambios en el diseño en un laboratorio antes de implementar los dominios.  
  
|Vínculo más lenta que se conecta un controlador de dominio (Kbps)|Número máximo de usuarios si está disponible el ancho de banda de % 1|Número máximo de usuarios si está disponible el ancho de banda de % 5|Número máximo de usuarios si está disponible el ancho de banda de 10 por ciento|  
|--------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------------------|  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  
  
Para usar esta tabla:  
  
1.  En la **más lenta vínculo que se conecta un controlador de dominio** columna, busque el valor que coincida con la velocidad del vínculo más lento en la cual se replicarán AD DS en tu región.  
  
2.  En la fila que corresponde a la velocidad del vínculo, busque la columna que representa el ancho de banda de porcentaje que quieras asignar a AD DS. Ese valor representa el número máximo de usuarios que puede hospedar la región.  
  
Evalúa cada región propuesta y determinar si el número máximo de usuarios en cada región es menor que el recomendado número máximo de usuarios que puede contener un dominio. Si determinas que la región puede hospedar el número de usuarios que necesitas, puedes crear un dominio para esa región. Si determinas que no puede hospedar muchos usuarios, considera la posibilidad de dividir el diseño en regiones más pequeñas y volver a calcular el número máximo de usuarios que se pueden hospedar en cada región. Las otras alternativas son asignar más ancho de banda o aumentar la velocidad del vínculo.  
  
Aunque el número total de usuarios que se pueden colocar en un dominio en un bosque de varios dominios es menor que el número de usuarios del dominio en un bosque de dominio único, el número total de usuarios en el bosque de varios dominios puede ser mayor. El menor número de usuarios por dominio en un bosque de varios dominios adapta a la sobrecarga de replicación adicional creada por mantener el catálogo global en el entorno. Para obtener recomendaciones que se aplican a bosques que contienen más de 100.000a los usuarios o la conectividad de menos de 28,8 Kbps, consultar un diseñador experimentado de Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documentar las regiones identificadas  
Después de dividir la organización en dominios regionales, documento representadas las regiones que quieras y el número de usuarios que existirán en cada región. Además, ten en cuenta la velocidad de los vínculos más lentas de cada región que se usará para la replicación de Active Directory. Esta información se usa para determinar si se requieren dominios adicionales o bosques.  
  
Para que una hoja de cálculo que le ayudarán a documentar las regiones que has identificado, descarga Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  


