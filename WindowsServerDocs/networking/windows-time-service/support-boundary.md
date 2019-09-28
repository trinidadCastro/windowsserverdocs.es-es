---
ms.assetid: ''
title: Límite de soporte para tiempo de alta precisión
description: En este artículo se describe el límite de compatibilidad para el servicio de hora de Windows (W32Time) en entornos que requieren una hora de sistema muy precisa y estable.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 212b9c79bc2e43e966180b928c865a9053332c3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405260"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Límite de soporte para tiempo de alta precisión

>Se aplica a: Windows Server 2016 y Windows 10 versión 1607 o posterior

En este artículo se describen los límites de compatibilidad del servicio de hora de Windows (W32Time) en entornos que requieren una hora de sistema muy precisa y estable.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Compatibilidad de alta precisión para Windows 8.1 y 2012 R2 (o anterior)

Las versiones anteriores de Windows (anteriores a Windows 10 1607 o Windows Server 2016 1607) no pueden garantizar un tiempo muy preciso. El servicio de hora de Windows en estos sistemas:

-   Proporcionó la precisión de tiempo necesaria para satisfacer los requisitos de autenticación de la versión 5 de Kerberos

-   Se proporcionó un tiempo muy preciso para los clientes y servidores de Windows Unidos a un bosque de Active Directory común

Los requisitos de precisión más estrictos estaban fuera de la especificación de diseño del servicio de hora de Windows en estos sistemas operativos y no se admiten.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 y Windows Server 2016

Se ha mejorado considerablemente la precisión de tiempo en Windows 10 y Windows Server 2016, a la vez que se mantiene la compatibilidad de NTP con versiones anteriores de Windows. En las condiciones de funcionamiento correctas, los sistemas que ejecutan Windows 10 o Windows Server 2016 y versiones más recientes pueden ofrecer una precisión de 1 segundo, 50 ms (milisegundos) o 1 ms.

>[!IMPORTANT]
>**Orígenes de hora muy precisos**<br>
>La precisión de tiempo resultante en la topología depende en gran medida del uso de un origen de tiempo preciso y estable (estrato 1). Hay hardware de origen de tiempo NTP de Windows, basado en Windows y no basado en Windows, que venden proveedores de terceros. Consulte con su proveedor la precisión de sus productos.

>[!IMPORTANT]
>**Precisión de tiempo**<br>
>La precisión temporal conlleva la distribución de un extremo a otro de la hora exacta desde un origen de tiempo autoritativo muy preciso hasta el dispositivo final. Todo lo que introduce la asimetría de red influirá negativamente en la precisión, por ejemplo, los dispositivos de red físicos o la carga de CPU alta en el sistema de destino.

## <a name="high-accuracy-requirements"></a>Requisitos de alta precisión

En el resto de este documento se describen los requisitos del entorno que deben cumplirse para admitir los objetivos de alta precisión correspondientes.

### <a name="target-accuracy-1-second-1s"></a>Precisión de destino: 1 segundo (1s)

Para lograr la precisión de 1% para un equipo de destino específico en comparación con un origen de hora muy preciso:

-   El sistema de destino debe ejecutar Windows 10, Windows Server 2016.

-   El sistema de destino debe sincronizar la hora desde una jerarquía NTP de servidores horarios, que culmina en un origen de hora NTP compatible con Windows y muy preciso.

-   Todos los sistemas operativos Windows de la jerarquía de NTP mencionados anteriormente deben configurarse como se documenta en la documentación de [configuración de sistemas para alta precisión](configuring-systems-for-high-accuracy.md) .

-   La latencia de red unidireccional acumulativa entre el destino y el origen no debe superar las 100 ms. El retraso acumulativo de la red se mide agregando los retrasos unidireccionales individuales entre pares de nodos cliente-servidor NTP de la jerarquía a partir del destino y finalizando en el origen. Para obtener más información, consulte el documento sincronización de hora de alta precisión.

### <a name="target-accuracy-50-milliseconds"></a>Precisión de destino: 50 milisegundos

Todos los requisitos descritos en la sección **Target precisión: 1 segundo @ no__t-0 se aplican, excepto en el caso de los controles más estrictos que se describen en esta sección.

Los requisitos adicionales para lograr la precisión de 50 ms para un sistema de destino específico son:

-   El equipo de destino debe tener un mejor rendimiento que 5 ms de la latencia de red entre su origen de hora.

-   El sistema de destino no debe ser más que el estrato 5 de un origen de hora muy preciso

    >[!Note]
    >Ejecute "W32tm/Query/status" desde la línea de comandos para ver la capa.

-   El sistema de destino debe estar dentro de 6 saltos de red o menos desde el origen de hora muy preciso

-   El uso promedio de CPU de un día en todas las estrato no debe superar el 90%

-   En el caso de los sistemas virtualizados, el uso promedio de CPU de un día del host no debe superar el 90%.

### <a name="target-accuracy-1-millisecond"></a>Precisión de destino: 1 milisegundo

Todos los requisitos descritos en las secciones @no__t precisión 0Target: 1 segundo @ no__t-0 y **Target precisión: 50 milisegundos @ no__t-0 se aplican, excepto cuando los controles más estrictos se describen en esta sección.

Los requisitos adicionales para conseguir una precisión de 1 ms para un sistema de destino específico son:

-   El equipo de destino debe tener más de 0,1 ms de latencia de red entre su origen de hora

-   El sistema de destino no debe ser más que el estrato 5 de un origen de hora muy preciso

    >[!Note]
    >Ejecute ' W32tm/Query/status ' desde la línea de comandos para ver el estrato

-   El sistema de destino debe estar en menos de cuatro saltos de red desde el origen de hora muy preciso

-   El uso promedio de CPU de un día en cada estrato no debe superar el 80%

-   En el caso de los sistemas virtualizados, el uso promedio de CPU de un día del host no debe superar el 80%.
