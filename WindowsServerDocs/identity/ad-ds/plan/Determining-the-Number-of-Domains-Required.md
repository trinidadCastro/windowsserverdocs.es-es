---
ms.assetid: 87bca912-b912-4bbe-9533-2c34a7abc52d
title: Determinar el número de dominios necesarios
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 32093409150e54f30eec5385ea80fc1c30851142
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822568"
---
# <a name="determining-the-number-of-domains-required"></a>Determinar el número de dominios necesarios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada bosque se inicia con un solo dominio. El número máximo de usuarios que puede contener un solo bosque de dominio se basa en el vínculo más lento que debe dar cabida a la replicación entre los controladores de dominio y el ancho de banda disponible que desea asignar a Active Directory Domain Services (AD DS). En la tabla siguiente se muestra el número máximo recomendado de usuarios que un dominio puede contener en función de un bosque de dominio único, la velocidad del vínculo más lento y el porcentaje de ancho de banda que desea reservar para la replicación. Esta información se aplica a los bosques que contienen un máximo de 100.000 usuarios y que tienen una conectividad de 28,8 kilobits por segundo (kbps) o superior. Para obtener recomendaciones que se aplican a bosques que contienen más de 100.000 usuarios o conectividad de menos de 28,8 Kbps, consulte un diseñador de Active Directory con experiencia. Los valores de la tabla siguiente se basan en el tráfico de replicación generado en un entorno que tiene las siguientes características:  
  
- Los nuevos usuarios se unen al bosque a una velocidad del 20 por ciento al año.  
- Los usuarios dejan el bosque a una velocidad del 15 por ciento al año.  
- Cada usuario es miembro de cinco grupos globales y cinco grupos universales.  
- La relación entre los usuarios y los equipos es 1:1.  
- Active Directory se usa el sistema de nombres de dominio (DNS) integrado.  
- Se usa la eliminación de registros obsoletos de DNS.  

> [!NOTE]  
> Las cifras enumeradas en la tabla siguiente son aproximaciones. La cantidad de tráfico de replicación depende en gran medida del número de cambios realizados en el directorio en un período de tiempo determinado. Confirme que la red puede acomodar el tráfico de replicación probando la cantidad estimada y la tasa de cambios en el diseño de un laboratorio antes de implementar los dominios.  
  
|Vínculo más lento que conecta un controlador de dominio (kbps)|Número máximo de usuarios si hay disponible un ancho de banda del 1%|Número máximo de usuarios si el ancho de banda del 5 por ciento está disponible|Número máximo de usuarios si el ancho de banda de 10 por ciento está disponible|  
| --- | --- | --- | --- |  
|28.8|10.000|25.000|40.000|  
|32|10.000|25.000|50.000|  
|56|10.000|50.000|100.000|  
|64|10.000|50.000|100.000|  
|128|25.000|100.000|100.000|  
|256|50.000|100.000|100.000|  
|512|80,000|100.000|100.000|  
|1\.500|100.000|100.000|100.000|  

Para usar esta tabla:  

1. En el **vínculo más lento que conecta una** columna de controlador de dominio, busque el valor que coincida con la velocidad del vínculo más lento en el que AD DS se replicará en el dominio.  

2. En la fila que corresponde a la velocidad de vínculo más lenta, busque la columna que representa el porcentaje de ancho de banda que desea asignar a AD DS. El valor en esa ubicación es el número máximo de usuarios que puede contener el dominio de un bosque de dominio único.  

Si determina que el número total de usuarios del bosque es menor que el número máximo de usuarios que puede contener el dominio, puede usar un solo dominio. Asegúrese de acomodar el crecimiento futuro planeado cuando realice esta determinación. Si determina que el número total de usuarios del bosque es mayor que el número máximo de usuarios que puede contener el dominio, debe reservar un porcentaje mayor de ancho de banda para la replicación, aumentar la velocidad del vínculo o dividir la organización en dominios regionales.  
  
## <a name="dividing-the-organization-into-regional-domains"></a>División de la organización en dominios regionales

Si no puede dar cabida a todos los usuarios en un solo dominio, debe seleccionar el modelo de dominio regional. Divida su organización en regiones de una forma que tenga sentido para su organización y la red existente. Por ejemplo, puede crear regiones basadas en límites continental.  
  
Tenga en cuenta que, como debe crear un dominio de Active Directory para cada región que establezca, se recomienda minimizar el número de regiones que defina para AD DS. Aunque es posible incluir un número ilimitado de dominios en un bosque, para facilitar la administración, se recomienda que un bosque no incluya más de 10 dominios. Debe establecer el equilibrio adecuado entre optimizar el ancho de banda de replicación y minimizar la complejidad administrativa al dividir la organización en dominios regionales.  
  
En primer lugar, determine el número máximo de usuarios que el bosque puede hospedar. Base esto en el vínculo más lento del bosque en el que se replicarán los controladores de dominio y la cantidad media de ancho de banda que desea asignar a Active Directory replicación. En la tabla siguiente se muestra el número máximo recomendado de usuarios que puede contener un bosque. Esto se basa en la velocidad del vínculo más lento y el ancho de banda de porcentaje que desea reservar para la replicación. Esta información se aplica a los bosques que contienen un máximo de 100.000 usuarios y que tienen una conectividad de 28,8 kbps o superior. Los valores de la tabla siguiente se basan en los siguientes supuestos:  

- Todos los controladores de dominio son servidores de catálogo global.  
- Los nuevos usuarios se unen al bosque a una velocidad del 20 por ciento al año.  
- Los usuarios dejan el bosque a una velocidad del 15 por ciento al año.  
- Los usuarios son miembros de cinco grupos globales y cinco grupos universales.  
- La relación entre los usuarios y los equipos es 1:1.  
- Se usa el DNS integrado Active Directory.  
- Se usa la eliminación de registros obsoletos de DNS.  

> [!NOTE]  
> Las cifras enumeradas en la tabla siguiente son aproximaciones. La cantidad de tráfico de replicación depende en gran medida del número de cambios realizados en el directorio en un período de tiempo determinado. Confirme que la red puede acomodar el tráfico de replicación probando la cantidad estimada y la tasa de cambios en el diseño de un laboratorio antes de implementar los dominios.  
  
|Vínculo más lento que conecta un controlador de dominio (kbps)|Número máximo de usuarios si hay disponible un ancho de banda del 1%|Número máximo de usuarios si el ancho de banda del 5 por ciento está disponible|Número máximo de usuarios si el ancho de banda de 10 por ciento está disponible|  
| --- | --- | --- | --- |  
|28.8|10.000|50.000|75.000|  
|32|10.000|50.000|75.000|  
|56|10.000|75.000|100.000|  
|64|25.000|75.000|100.000|  
|128|50.000|100.000|100.000|  
|256|75.000|100.000|100.000|  
|512|100.000|100.000|100.000|  
|1\.500|100.000|100.000|100.000|  

Para usar esta tabla:  

1. En el **vínculo más lento que conecta una** columna de controlador de dominio, busque el valor que coincida con la velocidad del vínculo más lento en el que se replicará AD DS en el bosque.  

2. En la fila que corresponde a la velocidad de vínculo más lenta, busque la columna que representa el porcentaje de ancho de banda que desea asignar a AD DS. El valor en esa ubicación es el número máximo de usuarios que el bosque puede hospedar.  

Si el número máximo de usuarios que el bosque puede hospedar es mayor que el número de usuarios que necesita hospedar, un solo bosque funcionará para el diseño. Si necesita hospedar más usuarios que el número máximo que identificó, debe aumentar la velocidad de vínculo mínima, asignar un porcentaje mayor de ancho de banda para AD DS o implementar bosques adicionales.  

Si determina que un solo bosque va a contener el número de usuarios que necesita hospedar, el siguiente paso es determinar el número máximo de usuarios que cada región puede admitir según el vínculo más lento ubicado en esa región. Divida el bosque en regiones que le resulten adecuadas. Asegúrese de basar la decisión en algo que no es probable que cambie. Por ejemplo, use continentes en lugar de regiones de ventas. Estas regiones serán la base de la estructura de dominios cuando haya identificado el número máximo de usuarios.  

Determine el número de usuarios que deben estar hospedados en cada región y, a continuación, compruebe que no superan el máximo permitido según la velocidad de vínculo más lenta y el ancho de banda asignado a AD DS de esa región. En la tabla siguiente se muestra el número máximo recomendado de usuarios que puede contener un dominio regional. Se basa en la velocidad del vínculo más lento y en el porcentaje de ancho de banda que desea reservar para la replicación. Esta información se aplica a los bosques que contienen un máximo de 100.000 usuarios y que tienen una conectividad de 28,8 kbps o superior. Los valores de la tabla siguiente se basan en los siguientes supuestos:  

- Todos los controladores de dominio son servidores de catálogo global.  
- Los nuevos usuarios se unen al bosque a una velocidad del 20 por ciento al año.  
- Los usuarios dejan el bosque a una velocidad del 15 por ciento al año.  
- Los usuarios son miembros de cinco grupos globales y cinco grupos universales.  
- La relación entre los usuarios y los equipos es 1:1.  
- Se usa el DNS integrado Active Directory.  
- Se usa la eliminación de registros obsoletos de DNS.  
  
> [!NOTE]  
> Las cifras enumeradas en la tabla siguiente son aproximaciones. La cantidad de tráfico de replicación depende en gran medida del número de cambios realizados en el directorio en un período de tiempo determinado. Confirme que la red puede acomodar el tráfico de replicación probando la cantidad estimada y la tasa de cambios en el diseño de un laboratorio antes de implementar los dominios.  
  
|Vínculo más lento que conecta un controlador de dominio (kbps)|Número máximo de usuarios si hay disponible un ancho de banda del 1%|Número máximo de usuarios si el ancho de banda del 5 por ciento está disponible|Número máximo de usuarios si el ancho de banda de 10 por ciento está disponible|  
| --- | --- | --- | --- |  
|28.8|10.000|18.000|40.000|  
|32|10.000|20.000|50.000|  
|56|10.000|40.000|100.000|  
|64|10.000|50.000|100.000|  
|128|15.000|100.000|100.000|  
|256|30.000|100.000|100.000|  
|512|80,000|100.000|100.000|  
|1\.500|100.000|100.000|100.000|  

Para usar esta tabla:  

1. En el **vínculo más lento que conecta una** columna de controlador de dominio, busque el valor que coincida con la velocidad del vínculo más lento en el que se replicará AD DS en su región.  

2. En la fila que corresponde a la velocidad de vínculo más lenta, busque la columna que representa el porcentaje de ancho de banda que desea asignar a AD DS. Ese valor representa el número máximo de usuarios que la región puede hospedar.  

Evalúe cada región propuesta y determine si el número máximo de usuarios de cada región es menor que el número máximo recomendado de usuarios que puede contener un dominio. Si determina que la región puede hospedar el número de usuarios que necesita, puede crear un dominio para esa región. Si determina que no puede hospedar a muchos usuarios, considere la posibilidad de dividir el diseño en regiones más pequeñas y recalcular el número máximo de usuarios que se pueden hospedar en cada región. Las demás alternativas son asignar más ancho de banda o aumentar la velocidad del vínculo.  

Aunque el número total de usuarios que puede colocar en un dominio en un bosque de varios dominios es menor que el número de usuarios del dominio en un bosque de un solo dominio, el número total de usuarios del bosque de varios dominios puede ser mayor. El menor número de usuarios por dominio en un bosque de varios dominios admite la sobrecarga de replicación adicional que se crea al mantener el catálogo global en ese entorno. Para obtener recomendaciones que se aplican a bosques que contienen más de 100.000 usuarios o conectividad de menos de 28,8 Kbps, consulte un diseñador de Active Directory con experiencia.  
  
## <a name="documenting-the-regions-identified"></a>Documentar las regiones identificadas

Después de dividir la organización en dominios regionales, documente las regiones que desee representar y el número de usuarios que existirán en cada región. Además, tenga en cuenta la velocidad de los vínculos más lentos en cada región que utilizará para la replicación de Active Directory. Esta información se usa para determinar si se requieren dominios o bosques adicionales.  

Para obtener una hoja de cálculo que le ayude a documentar las regiones que identificó, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de la [ayuda del trabajo para el kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558) y abra "identificar regiones" (DSSLOGI_4. doc).  
