---
title: Tolerancia a errores y eficiencia del almacenamiento en Espacios de almacenamiento directo
ms.prod: windows-server
ms.author: cosmosdarwin
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/11/2017
ms.assetid: 5e1d7ecc-e22e-467f-8142-bad6d82fc5d0
description: Una explicación de las opciones de resistencia en Espacios de almacenamiento directo, incluida creación de reflejos y paridad.
ms.localizationpriority: medium
ms.openlocfilehash: b64592bf3cf5659410dcbbeb4c190d2d6a85485a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859018"
---
# <a name="fault-tolerance-and-storage-efficiency-in-storage-spaces-direct"></a>Tolerancia a errores y eficiencia del almacenamiento en Espacios de almacenamiento directo

>Se aplica a: Windows Server 2016

Este tema presenta las opciones de resistencia disponibles en [Espacios de almacenamiento directo](storage-spaces-direct-overview.md) y describe los requisitos de escala, la eficiencia del almacenamiento y las ventajas y desventajas generales de cada uno. También contiene algunas instrucciones de uso para empezar a trabajar y hace referencia a algunos excelentes artículos, blogs y contenido adicional donde puedes obtener más información.

Si ya estás familiarizado con Espacios de almacenamiento, puedes ir a la sección [Resumen](#summary).

## <a name="overview"></a>Información general

En esencia, Espacios de almacenamiento tiene que ver con proporcionar a los datos tolerancia a errores, lo que a menudo se denomina "resistencia". Su implementación es similar a RAID, excepto que se distribuye entre varios servidores y se implementa en el software.

Al igual que sucede con RAID, existen varias formas distintas en las que Espacios de almacenamiento puede hacer esto, que equilibran de forma diferente la tolerancia a errores, la eficiencia del almacenamiento y la complejidad del cálculo. De manera general, se dividen en dos categorías: "creación de reflejos" y "paridad"; esta última a veces se conoce como "codificación del borrado".

## <a name="mirroring"></a>Creación de reflejos

Los reflejos proporcionan tolerancia a errores mediante el mantenimiento de varias copias de todos los datos. Esto se parece mucho a RAID-1. El modo en que se seccionan y colocan esos datos no es sencillo (para aprender más, consulta [este blog](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/)), pero es completamente cierto que todos los datos almacenados mediante reflejos se escriben varias veces en su totalidad. Cada copia se escribe en hardware físico distinto (distintas unidades en distintos servidores), que se supone que no sufrirán errores a la vez.

En Windows Server 2016, Espacios de almacenamiento ofrece dos tipos de reflejos: "dobles" y "triples".

### <a name="two-way-mirror"></a>Reflejo doble

Los reflejos dobles escriben dos copias de todo. Su eficiencia del almacenamiento es del 50 %: para escribir 1 TB de datos, se necesitan al menos 2 TB de capacidad de almacenamiento físico. De manera similar, se necesitan al menos dos ["dominios de error" de hardware](../../failover-clustering/fault-domains.md); en el caso de Espacios de almacenamiento directo, eso significa dos servidores.

![two-way-mirror](media/Storage-Spaces-Fault-Tolerance/two-way-mirror-180px.png)

   >[!WARNING]
   > Si tienes más de dos servidores, se recomienda usar reflejos triples en su lugar.

### <a name="three-way-mirror"></a>Reflejo triple

Los reflejos triples escriben tres copias de todo. Su eficiencia del almacenamiento es del 33,3 %: para escribir 1 TB de datos, se necesitan al menos 3 TB de capacidad de almacenamiento físico. De manera similar, se necesitan al menos tres "dominios de error" de hardware; en el caso de Espacios de almacenamiento directos, eso significa tres servidores.

La creación de reflejos triple puede tolerar de forma segura [al menos dos problemas de hardware (en la unidad o el servidor) a la vez](#examples). Por ejemplo, si estás reiniciando un servidor cuando repentinamente se produce un error en otra unidad o servidor, todos los datos permanecen seguros y accesibles de forma continua.

![reflejo triple](media/Storage-Spaces-Fault-Tolerance/three-way-mirror-180px.png)

## <a name="parity"></a>Paridad

La codificación de la paridad, que a menudo se denomina "codificación de borrado", proporciona tolerancia a errores mediante aritmética bit a bit, lo que llegar a ser [muy complicado](https://www.microsoft.com/research/wp-content/uploads/2016/02/LRC12-cheng20webpage.pdf). Funciona de una forma menos obvia que los reflejos, y existen muchos recursos en línea excelentes (por ejemplo, esta [Dummies Guide to Erasure Coding](http://smahesh.com/blog/2012/07/01/dummies-guide-to-erasure-coding/) [Guía para principiantes para la codificación del borrado, en inglés] de terceros), que pueden ayudarte a entender la idea. Basta decir que proporciona una mayor eficiencia del almacenamiento sin comprometer la tolerancia a errores.

En Windows Server 2016, Espacios de almacenamiento ofrece dos tipos de paridad: la paridad "individual" y la paridad "doble". Esta última emplea una técnica avanzada denominada "códigos de reconstrucción local" a escalas mayores.

> [!IMPORTANT]
> Te recomendamos usar el reflejo de la mayoría de las cargas de trabajo dependientes del rendimiento. Para obtener más información sobre cómo equilibrar entre rendimiento y capacidad en función de la carga de trabajo, consulta [Planificar los volúmenes](plan-volumes.md#choosing-the-resiliency-type).

### <a name="single-parity"></a>Paridad individual
La paridad individual mantiene un solo símbolo de paridad bit a bit, que proporciona tolerancia a errores para un único error en cada momento. Se parece mucho a RAID-5. Para utilizar la paridad individual, se necesitan al menos tres "dominios de error" de hardware; en el caso de Espacios de almacenamiento directos, eso significa tres servidores. Debido a que los reflejos triples proporcionan más tolerancia a errores en la misma escala, se desaconseja el uso de la paridad individual. Pero está disponible si insistes en usarla y es completamente compatible.

   >[!WARNING]
   > Se desaconseja usar la paridad única porque solo tolera de forma segura un error de hardware a la vez: si estás reiniciando un servidor cuando repentinamente se produce un error en otra unidad o servidor, se producirá un tiempo de inactividad. Si solo dispones de tres servidores, recomendamos usar los reflejos triples. Si cuentas con cuatro o más, consulta la sección siguiente.

### <a name="dual-parity"></a>Paridad doble

La paridad doble implementa códigos de corrección de errores de Reed-Solomon para mantener dos símbolos de paridad bit a bit, lo que proporciona la misma tolerancia a errores que los reflejos triples (es decir, hasta dos errores a la vez), pero con una eficacia del almacenamiento mejor. Se parece mucho a RAID-6. Para utilizar la paridad doble, se necesitan al menos cuatro "dominios de error" de hardware; en el caso de Espacios de almacenamiento directos, eso significa cuatro servidores. En esa escala, la eficiencia del almacenamiento es del 50 %: para almacenar 2 TB de datos, se necesitan 4 TB de capacidad de almacenamiento físico.

![paridad doble](media/Storage-Spaces-Fault-Tolerance/dual-parity-180px.png)

La eficiencia del almacenamiento de la paridad doble aumenta con cuantos más dominios de error de hardware se tienen, de un 50 % hasta un 80 %. Por ejemplo, con 7 (en el caso de Espacios de almacenamiento directos esto significa 7 servidores), la eficiencia pasa al 66,7 %: para almacenar 4 TB de datos, se necesitan tan solo 6 TB de capacidad de almacenamiento físico.

![paridad doble a gran escala](media/Storage-Spaces-Fault-Tolerance/dual-parity-wide-180px.png)

Consulta la sección [Resumen](#summary) para conocer la eficacia de la paridad doble y los códigos de reconstrucción local a cada escala.

### <a name="local-reconstruction-codes"></a>Códigos de reconstrucción local

Espacios de almacenamiento en Windows Server 2016 presenta una técnica avanzada desarrollada por Microsoft Research denominada "códigos de reconstrucción local" o LRC. A gran escala, la paridad doble usa los LRC para dividir su codificación y descodificación en grupos más pequeños, con el fin de reducir la sobrecarga necesaria para realizar escrituras o recuperarse de errores.

En el caso de las unidades de disco duro (HDD), el tamaño del grupo es de 4 símbolos; en el caso de las unidades de estado sólido (SSD), el tamaño del grupo es de 6 símbolos. Por ejemplo, esta es la apariencia del diseño en el caso de unidades de disco duro y 12 dominios de error de hardware (es decir, 12 servidores): existen 2 grupos de 4 símbolos de datos. Consigue una eficiencia del almacenamiento del 72,7 %.

![códigos de reconstrucción local](media/Storage-Spaces-Fault-Tolerance/local-reconstruction-codes-180px.png)

Recomendamos este tutorial detallado aunque perfectamente legible, acerca de [cómo los códigos reconstrucción local controlan diversos escenarios de error y por qué resultan atractivos](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/) (en inglés), escrito por nuestro compañero [Claus Joergensen](https://twitter.com/clausjor).

## <a name="mirror-accelerated-parity"></a>Paridad acelerada por reflejos

A partir de Windows Server 2016, un volumen de Espacios de almacenamiento directo puede ser parte reflejo y parte paridad. Las operaciones de escritura se dirigen primero a la porción reflejada y, más adelante, se mueven gradualmente a la porción de paridad. De hecho, con esto [se usa la creación de reflejos para acelerar la codificación de borrado](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/).

Para combinar el reflejo triple y la paridad doble, se necesitan al menos 4 dominios de error, lo que significa 4 servidores.

La eficiencia del almacenamiento de paridad acelerada por reflejos se sitúa entre lo que se obtiene al usar todo reflejo o todo paridad y depende de las proporciones que se elijan. Por ejemplo, la demostración que aparece en la marca temporal de los 37 minutos de esta presentación muestra [varias combinaciones que logran una eficiencia del 46 %, el 54 % y el 65 %](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) (en inglés) con 12 servidores.

> [!IMPORTANT]
> Te recomendamos usar el reflejo de la mayoría de las cargas de trabajo dependientes del rendimiento. Para obtener más información sobre cómo equilibrar entre rendimiento y capacidad en función de la carga de trabajo, consulta [Planificar los volúmenes](plan-volumes.md#choosing-the-resiliency-type).

## <a name="summary"></a><a name="summary"></a>Sumido

En esta sección se resumen los tipos de resistencia disponibles en Espacios de almacenamiento directo, los requisitos mínimos de escala para usar cada tipo, la cantidad de errores que puede tolerar cada tipo y la eficiencia de almacenamiento correspondiente.

### <a name="resiliency-types"></a>Tipos de resistencia

|    Resistencia          |    Tolerancia a errores       |    Eficiencia del almacenamiento      |
|------------------------|----------------------------|----------------------------|
|    Reflejo doble      |    1                       |    50.0%                   |
|    Reflejo triple    |    2                       |    33,3 %                   |
|    Paridad doble         |    2                       |    50,0 %-80,0 %           |
|    Mixto               |    2                       |    33,3 %-80,0 %           |

### <a name="minimum-scale-requirements"></a>Requisitos mínimos de escala

|    Resistencia          |    Mínimo obligatorio de dominios de error   |
|------------------------|-------------------------------------|
|    Reflejo doble      |    2                                |
|    Reflejo triple    |    3                                |
|    Paridad doble         |    4                                |
|    Mixto               |    4                                |

   >[!TIP]
   > A menos que uses [tolerancia a errores de chasis o bastidor](../../failover-clustering/fault-domains.md), el número de dominios de error se refiere al número de servidores. El número de unidades de cada servidor no afecta a los tipos de resistencia que puedes usar, siempre que cumplas los requisitos mínimos de Espacios de almacenamiento directo. 

### <a name="dual-parity-efficiency-for-hybrid-deployments"></a>Eficiencia de la paridad doble para implementaciones híbridas

En la siguiente tabla se muestran la eficiencia del almacenamiento de paridad doble y los códigos de reconstrucción local en cada escala para las implementaciones híbridas que contienen unidades de disco duro (HDD) y unidades de estado sólido (SSD).

|    Dominios de error      |    Diseño           |    Eficacia   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50.0%        |
|    5                  |    RS 2+2           |    50.0%        |
|    6                  |    RS 2+2           |    50.0%        |
|    7                  |    RS 4+2           |    66,7 %        |
|    8                  |    RS 4+2           |    66,7 %        |
|    9                  |    RS 4+2           |    66,7 %        |
|    10                 |    RS 4+2           |    66,7 %        |
|    11                 |    RS 4+2           |    66,7 %        |
|    12                 |    LRC (8, 2, 1)    |    72,7 %        |
|    13                 |    LRC (8, 2, 1)    |    72,7 %        |
|    14                 |    LRC (8, 2, 1)    |    72,7 %        |
|    15                 |    LRC (8, 2, 1)    |    72,7 %        |
|    16                 |    LRC (8, 2, 1)    |    72,7 %        |

### <a name="dual-parity-efficiency-for-all-flash-deployments"></a>Eficiencia de la paridad doble para implementaciones en memoria flash

En la siguiente tabla se muestran la eficiencia del almacenamiento de paridad doble y los códigos de reconstrucción local en cada escala para las implementaciones en memoria flash que contienen unidades de estado sólido (SSD) únicamente. El diseño de la paridad puede usar tamaños de grupo más grandes y lograr una mayor eficiencia de almacenamiento en una configuración de memoria flash.

|    Dominios de error      |    Diseño           |    Eficacia   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50.0%        |
|    5                  |    RS 2+2           |    50.0%        |
|    6                  |    RS 2+2           |    50.0%        |
|    7                  |    RS 4+2           |    66,7 %        |
|    8                  |    RS 4+2           |    66,7 %        |
|    9                  |    RS 6+2           |    75.0%        |
|    10                 |    RS 6+2           |    75.0%        |
|    11                 |    RS 6+2           |    75.0%        |
|    12                 |    RS 6+2           |    75.0%        |
|    13                 |    RS 6+2           |    75.0%        |
|    14                 |    RS 6+2           |    75.0%        |
|    15                 |    RS 6+2           |    75.0%        |
|    16                 |    LRC (12, 2, 1)   |    80,0 %        |

## <a name="examples"></a><a name="examples"></a>Example

A menos que tengas solo dos servidores, te recomendamos usar el reflejo triple o la paridad doble, porque ofrecen mejor tolerancia a errores. En concreto, asegúrate de que todos los datos permanezcan seguros y accesibles continuamente incluso cuando dos dominios de error (con Espacios de almacenamiento directo, esto significa dos servidores) se vean afectados por errores simultáneos.

### <a name="examples-where-everything-stays-online"></a>Ejemplos en los que todo permanece en línea

En estos seis ejemplos, se muestra lo que el reflejo triple o la paridad doble **pueden** tolerar.

- **1.**    Una unidad perdida (incluye unidades de caché)
- **2.**    Un servidor perdido

![fault-tolerance-examples-1-and-2](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-12.png)

- **3.**    Un servidor y una unidad perdidos
- **4.**    Dos unidades perdidas en diferentes servidores

![fault-tolerance-examples-3-and-4](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-34.png)

- **5.**    Más de dos unidades perdidas, siempre y cuando se ven afectados a lo sumo dos servidores
- **6.**    Dos servidores perdidos

![fault-tolerance-examples-5-and-6](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-56.png)

...en cada caso, todos los volúmenes permanecerán en línea. (Asegúrate de que el clúster mantenga cuórum).

### <a name="examples-where-everything-goes-offline"></a>Ejemplos en los que todo es sin conexión

A lo largo de su vida, Espacios de almacenamiento puede tolerar cualquier número de errores, ya que se restaura a resistencia completa después de cada uno de ellos, con el tiempo suficiente. Sin embargo, al menos dos dominios de error pueden verse afectados de forma segura por errores en un momento determinado. En los siguientes ejemplos se muestran lo que el reflejo triple o la paridad doble **no pueden** tolerar.

- **7.** Unidades perdidas en tres o más servidores a la vez
- **8.** Tres o más servidores perdidos a la vez

![fault-tolerance-examples-7-and-8](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-78.png)

## <a name="usage"></a>Uso

Echa un vistazo a [Crear volúmenes en Espacios de almacenamiento directo](create-volumes.md).

## <a name="see-also"></a>Vea también

Cada uno de los siguientes vínculos se encuentra en línea en algún lugar del texto de este tema.

- [Espacios de almacenamiento directo en Windows Server 2016](storage-spaces-direct-overview.md)
- [Reconocimiento de dominios de error en Windows Server 2016](../../failover-clustering/fault-domains.md)
- [Codificación de borrado en Azure por Microsoft Research](https://www.microsoft.com/research/publication/erasure-coding-in-windows-azure-storage/)
- [Códigos de reconstrucción locales y aceleración de volúmenes de paridad](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)
- [Volúmenes en la API de administración de almacenamiento](https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/)
- [Demostración de la eficiencia del almacenamiento en Microsoft encendido 2016](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)
- [Versión preliminar de la calculadora de capacidad para Espacios de almacenamiento directo](https://aka.ms/s2dcalc)
