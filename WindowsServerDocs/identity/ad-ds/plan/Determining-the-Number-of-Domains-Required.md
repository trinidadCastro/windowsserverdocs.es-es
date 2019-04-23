---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinar el número de dominios necesarios
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e5d6369ca91c1d48375c22238d1e706576df7fbf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843526"
---
# <a name="determining-the-number-of-domains-required"></a>Determinar el número de dominios necesarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada bosque se inicia con un único dominio. El número máximo de usuarios que puede contener un único bosque de dominios se basa en el vínculo más lento que debe dar cabida a la replicación entre controladores de dominio y el ancho de banda disponible en la que desea asignar a los servicios de dominio de Active Directory (AD DS). La tabla siguiente muestra el número máximo recomendado de usuarios que puede contener un dominio basado en un bosque de dominio único, la velocidad del vínculo más lenta y el porcentaje de ancho de banda que desee reservar para la replicación. Esta información se aplica a los bosques que contienen un máximo de 100 000 usuarios y que tiene una conectividad de 28,8 kilobits por segundo (Kbps) o superior. Para obtener recomendaciones que se aplican a los bosques que contienen más de 100.000 usuarios o conectividad de menos de 28,8 Kbps, consulte un diseñador experimentado de Active Directory. Los valores en la siguiente tabla se basan en el tráfico de replicación generado en un entorno que tiene las siguientes características:  
  
- Los usuarios nuevos unirán del bosque a una velocidad de 20 por ciento al año.  
- Los usuarios abandonan el bosque con una frecuencia de 15 por ciento por año.  
- Cada usuario es miembro de los cinco grupos globales y cinco grupos universales.  
- La proporción de usuarios a los equipos es 1:1.  
- Se usa Active Directory integrado Domain Name System (DNS).  
- Se utiliza la limpieza de DNS.  

> [!NOTE]  
> Las cifras que se muestran en la tabla siguiente son aproximadas. La cantidad de tráfico de replicación depende en gran medida el número de los cambios realizados en el directorio en un período de tiempo determinado. Confirme que la red puede acomodar el tráfico de replicación mediante la comprobación de la cantidad estimada y la tasa de cambios en el diseño en un laboratorio antes de implementar los dominios.  
  
|Vínculo más lenta que se conecta un controlador de dominio (Kbps)|Número máximo de usuarios si está disponible el ancho de banda de 1 por ciento|Número máximo de usuarios si está disponible el ancho de banda de 5 por ciento|Número máximo de usuarios si está disponible el ancho de banda de 10 por ciento|  
| --- | --- | --- | --- |  
|28.8|10,000|25,000|40,000|  
|32|10,000|25,000|50,000|  
|56|10,000|50,000|100,000|  
|64|10,000|50,000|100,000|  
|128|25,000|100,000|100,000|  
|256|50,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Para usar esta tabla:  

1. En el **vínculo más lenta que se conecta un controlador de dominio** columna, busque el valor que coincida con la velocidad del vínculo más lento en los que AD DS se replicarán en el dominio.  

2. En la fila que corresponde a la velocidad de vínculo, busque la columna que representa el ancho de banda de porcentaje que desea asignar a AD DS. El valor en esa ubicación es el número máximo de usuarios que puede contener el dominio en un bosque de dominio único.  

Si determina que el número total de usuarios del bosque es menor que el número máximo de usuarios que puede contener su dominio, puede usar un único dominio. Asegúrese de dar cabida a para el crecimiento futuro planeado cuando realice esta determinación. Si determina que el número total de usuarios del bosque es mayor que el número máximo de usuarios que puede contener su dominio, necesita reservar un porcentaje mayor de ancho de banda para la replicación, aumentar la velocidad de vínculo, o dividir su organización en dominios regionales.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>División de la organización a dominios regionales

Si no se puede dar cabida a todos los usuarios en un solo dominio, debe seleccionar el modelo de dominio regional. Dividir su organización en las regiones de forma que tenga sentido para su organización y la red existente. Por ejemplo, puede crear regiones en función de los límites del continentales.  
  
Tenga en cuenta que dado que necesita crear un dominio de Active Directory para cada región en la que establece, se recomienda que minimice el número de regiones que defina para AD DS. Aunque es posible incluir un número ilimitado de dominios en un bosque, facilidad de administración se recomienda que un bosque incluya dominios no más de 10. Debe establecer el equilibrio adecuado entre optimizar el ancho de banda de replicación y minimizar la complejidad administrativa al dividir su organización en dominios regionales.  
  
En primer lugar, determine el número máximo de usuarios que puede hospedar su bosque. Basar este en el vínculo más lento en el bosque a través de la cantidad media de ancho de banda que desea asignar a la replicación de Active Directory y qué controladores de dominio se replicarán. La tabla siguiente muestra el número máximo recomendado de usuarios que puede contener un bosque. Esto se basa en la velocidad del vínculo más lento y el ancho de banda de porcentaje que desee reservar para la replicación. Esta información se aplica a los bosques que contienen un máximo de 100 000 usuarios y que tiene una conectividad de 28,8 Kbps o superior. Los valores en la siguiente tabla se basan en los siguientes supuestos:  

- Todos los controladores de dominio son servidores de catálogo global.  
- Los usuarios nuevos unirán del bosque a una velocidad de 20 por ciento al año.  
- Los usuarios abandonan el bosque con una frecuencia de 15 por ciento por año.  
- Los usuarios son miembros de los cinco grupos globales y cinco grupos universales.  
- La proporción de usuarios a los equipos es 1:1.  
- Se usa DNS integrado en Active Directory.  
- Se utiliza la limpieza de DNS.  

> [!NOTE]  
> Las cifras que se muestran en la tabla siguiente son aproximadas. La cantidad de tráfico de replicación depende en gran medida el número de los cambios realizados en el directorio en un período de tiempo determinado. Confirme que la red puede acomodar el tráfico de replicación mediante la comprobación de la cantidad estimada y la tasa de cambios en el diseño en un laboratorio antes de implementar los dominios.  
  
|Vínculo más lenta que se conecta un controlador de dominio (Kbps)|Número máximo de usuarios si está disponible el ancho de banda de 1 por ciento|Número máximo de usuarios si está disponible el ancho de banda de 5 por ciento|Número máximo de usuarios si está disponible el ancho de banda de 10 por ciento|  
| --- | --- | --- | --- |  
|28.8|10,000|50,000|75,000|  
|32|10,000|50,000|75,000|  
|56|10,000|75,000|100,000|  
|64|25,000|75,000|100,000|  
|128|50,000|100,000|100,000|  
|256|75,000|100,000|100,000|  
|512|100,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Para usar esta tabla:  

1. En el **vínculo más lenta que se conecta un controlador de dominio** columna, busque el valor que coincida con la velocidad del vínculo más lento en los que AD DS se replicarán en el bosque.  

2. En la fila que corresponde a la velocidad de vínculo, busque la columna que representa el ancho de banda de porcentaje que desea asignar a AD DS. El valor en esa ubicación es el número máximo de usuarios que puede hospedar su bosque.  

Si es mayor que el número de usuarios que necesitan para alojar el número máximo de usuarios que puede alojar el bosque, un único bosque funcionará para el diseño. Si necesita hospedar más usuarios que el número máximo que ha identificado, necesita aumentar la velocidad mínima de vínculo, asigne un mayor porcentaje de ancho de banda para AD DS o implementar bosques adicionales.  

Si determina que un único bosque alojará al número de usuarios que necesita para hospedar, el siguiente paso es determinar el número máximo de usuarios que puede admitir cada región se basa en el vínculo más lento que se encuentra en esa región. Dividir el bosque en regiones que tengan sentido para usted. Asegúrese de que basar la decisión sobre algo que no es probable que cambie. Por ejemplo, use continentes en lugar de las regiones de ventas. Estas regiones será la base de la estructura de dominio cuando haya identificado el número máximo de usuarios.  

Determinar el número de usuarios que deben hospedarse en cada región y, a continuación, compruebe que no superen el máximo permitido en función de la velocidad de vínculo y el ancho de banda asignado a AD DS en esa región. La tabla siguiente muestra el número máximo recomendado de usuarios que puede contener un dominio regional. Se basa en la velocidad del vínculo más lenta y el porcentaje de ancho de banda que desea reservar para la replicación. Esta información se aplica a los bosques que contienen un máximo de 100 000 usuarios y que tiene una conectividad de 28,8 Kbps o superior. Los valores en la siguiente tabla se basan en los siguientes supuestos:  

- Todos los controladores de dominio son servidores de catálogo global.  
- Los usuarios nuevos unirán del bosque a una velocidad de 20 por ciento al año.  
- Los usuarios abandonan el bosque con una frecuencia de 15 por ciento por año.  
- Los usuarios son miembros de los cinco grupos globales y cinco grupos universales.  
- La proporción de usuarios a los equipos es 1:1.  
- Se usa DNS integrado en Active Directory.  
- Se utiliza la limpieza de DNS.  
  
> [!NOTE]  
> Las cifras que se muestran en la tabla siguiente son aproximadas. La cantidad de tráfico de replicación depende en gran medida el número de los cambios realizados en el directorio en un período de tiempo determinado. Confirme que la red puede acomodar el tráfico de replicación mediante la comprobación de la cantidad estimada y la tasa de cambios en el diseño en un laboratorio antes de implementar los dominios.  
  
|Vínculo más lenta que se conecta un controlador de dominio (Kbps)|Número máximo de usuarios si está disponible el ancho de banda de 1 por ciento|Número máximo de usuarios si está disponible el ancho de banda de 5 por ciento|Número máximo de usuarios si está disponible el ancho de banda de 10 por ciento|  
| --- | --- | --- | --- |  
|28.8|10,000|18,000|40,000|  
|32|10,000|20,000|50,000|  
|56|10,000|40,000|100,000|  
|64|10,000|50,000|100,000|  
|128|15,000|100,000|100,000|  
|256|30,000|100,000|100,000|  
|512|80,000|100,000|100,000|  
|1,500|100,000|100,000|100,000|  

Para usar esta tabla:  

1. En el **vínculo más lenta que se conecta un controlador de dominio** columna, busque el valor que coincida con la velocidad del vínculo más lento en los que se replicarán AD DS en su región.  

2. En la fila que corresponde a la velocidad de vínculo, busque la columna que representa el ancho de banda de porcentaje que desea asignar a AD DS. Ese valor representa el número máximo de usuarios que puede hospedar la región.  

Evalúe cada región propuesto y determinar si el número máximo de usuarios en cada región es menor que el número máximo recomendado de los usuarios que puede contener un dominio. Si determina que la región puede alojar el número de usuarios que necesite, puede crear un dominio de esa región. Si decide que no se puede hospedar muchos usuarios, considere la posibilidad de dividir el diseño en regiones más pequeñas y volver a calcular el número máximo de usuarios que pueden hospedarse en cada región. Las alternativas son asigne más ancho de banda o aumentar la velocidad de vínculo.  

Aunque el número total de usuarios que puede colocar en un dominio en un bosque con varios dominios es menor que el número de usuarios en el dominio en un bosque de dominio único, el número total de los usuarios del bosque con varios dominios puede ser mayor. El número más pequeño de usuarios por dominio en un bosque con varios dominios se adapta a la sobrecarga de replicación adicional que se crea al mantener el catálogo global en ese entorno. Para obtener recomendaciones que se aplican a los bosques que contienen más de 100.000 usuarios o conectividad de menos de 28,8 Kbps, consulte un diseñador experimentado de Active Directory.  
  
## <a name="documenting-the-regions-identified"></a>Documentar las regiones identificadas

Después de dividir su organización en dominios regionales, documento representadas de las regiones que desee y el número de usuarios que existirá en cada región. Además, tenga en cuenta la velocidad de los vínculos más lentos en cada región que va a utilizar para la replicación de Active Directory. Esta información se utiliza para determinar si otros dominios o bosques están necesarios.  

Para que una hoja de cálculo que le ayudarán a documentar las regiones que identificó, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip desde [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) y abra " Identificación de las regiones"(DSSLOGI_4.doc).  
