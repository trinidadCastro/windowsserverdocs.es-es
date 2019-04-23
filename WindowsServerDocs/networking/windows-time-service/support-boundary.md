---
ms.assetid: ''
title: Límite de soporte para tiempo de alta precisión
description: En este artículo se describe el límite de soporte técnico para el servicio de hora de Windows (W32Time) en entornos que requieren la hora del sistema estable y de alta precisión.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 991bf4502546771dae9f092c6d5732f96b1278ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866276"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Límite de soporte para tiempo de alta precisión

>Se aplica a: Windows Server 2016 y Windows 10 versión 1607 o posterior

En este artículo se describe los límites de compatibilidad para el servicio de hora de Windows (W32Time) en entornos que requieren la hora del sistema estable y de alta precisión.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Compatibilidad alta precisión para Windows 8.1 y 2012 R2 (o anterior)

Las versiones anteriores de Windows (anterior a Windows 10 1607 o 1607 de Windows Server 2016) no pueden garantizar un tiempo muy preciso. El servicio de hora de Windows en estos sistemas:

-   Proporciona la precisión de tiempo necesario para satisfacer los requisitos de autenticación Kerberos versión 5

-   Proporciona la hora exacta de acoplamiento flexible para los clientes de Windows y servidores unidos a un bosque de Active Directory

Requisitos de una mayor precisión estaban fuera de la especificación de diseño de este servicio en estos sistemas operativos Windows y no se admite.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 y Windows Server 2016

Precisión de tiempo en Windows 10 y Windows Server 2016 se ha mejorado substancialmente, manteniendo la completa hacia atrás NTP compatibilidad con versiones anteriores de Windows. En la derecha de las condiciones de funcionamiento, los sistemas que ejecutan Windows 10 o Windows Server 2016 y versiones más recientes pueden entregar 50 ms segundo, 1 (en milisegundos), o 1 ms precisión.

>[!IMPORTANT]
>**Orígenes de hora muy precisos**<br>
>La precisión de hora resultante en su topología es sumamente dependiente de uso de una raíz precisa y estable (estrato 1) el origen de hora. Existe Windows base y que no son de Windows en función muy precisos, Windows compatibles, NTP tiempo origen hardware comercializado a través de proveedores 3rd. Compruebe con su proveedor de la exactitud de sus productos.

>[!IMPORTANT]
>**Precisión de tiempo**<br>
>Precisión de tiempo conlleva la distribución de-to-end de la hora exacta de un origen autorizado de la hora muy precisos al dispositivo final. Todo lo que introduce la asimetría de red influirán negativamente precisión, dispositivos de red físico como o carga elevada de CPU en el sistema de destino.

## <a name="high-accuracy-requirements"></a>Requisitos de alta precisión

El resto de este documento describe los requisitos del entorno que deben cumplirse para admitir los objetivos de alta precisión respectivos.

### <a name="target-accuracy-1-second-1s"></a>Precisión de destino: 1 segundo (1)

Para lograr 1s precisión para un destino concreto del equipo en comparación con un origen de hora muy precisos:

-   El sistema de destino debe ejecutar Windows 10, Windows Server 2016.

-   El sistema de destino debe sincronizar la hora de una jerarquía NTP de servidores de hora, culminante en un origen de hora NTP compatible de Windows muy preciso.

-   Todos los sistemas operativos de Windows en la jerarquía NTP mencionados anteriormente debe configurarse como se documenta en el [configurar sistemas de alta precisión](configuring-systems-for-high-accuracy.md) documentación.

-   La latencia acumulativa red unidireccional entre el origen y destino no debe superar los 100 ms. El retraso de red acumulativa se mide mediante la adición de la persona retrasos unidireccionales entre los pares de nodos de cliente / servidor NTP en la jerarquía empezando por el destino y finalizando en el origen. Para obtener más información, consulte el documento de sincronización de hora de alta precisión.

### <a name="target-accuracy-50-milliseconds"></a>Precisión de destino: 50 milisegundos

Todos los requisitos descritos en la sección **precisión de destino: 1 segundo** aplicar, excepto donde se describen los controles más estrictos en esta sección.

Los requisitos adicionales para lograr precisión 50 ms para un sistema de destino específicos son:

-   El equipo de destino debe tener un mejor rendimiento que 5 ms de latencia de red entre su origen de hora.

-   El sistema de destino debe ser no más de 5 desde un origen de hora muy precisos de los satélites

    >[!Note]
    >Ejecute "w32tm /query /status" desde la línea de comandos para ver el satélites.

-   El sistema de destino debe estar dentro de 6 o menor de saltos de red desde el origen de hora muy precisos

-   El uso medio de CPU de un día en todos los stratums no puede superar el 90%

-   Para los sistemas virtualizados, el uso de CPU promedio de un día del host no puede superar el 90%

### <a name="target-accuracy-1-millisecond"></a>Precisión de destino: 1 milisegundo

Todos los requisitos descritos en las secciones **precisión de destino: 1 segundo** y **precisión de destino: 50 milisegundos** aplicar, excepto donde se describen los controles más estrictos en esta sección.

Los requisitos adicionales para lograr precisión de 1 ms para un sistema de destino específicos son:

-   El equipo de destino debe tener un mejor rendimiento que 0,1 ms de latencia de red entre su origen de hora

-   El sistema de destino debe ser no más de 5 desde un origen de hora muy precisos de los satélites

    >[!Note]
    >Ejecute 'w32tm /query /status' desde la línea de comandos para ver el estrato

-   El sistema de destino debe estar dentro de 4 o menos saltos de red desde el origen de hora muy precisos

-   El uso medio de CPU de un día a través de cada estrato no debe superar los 80%

-   Para los sistemas virtualizados, el uso de CPU promedio de un día del host no debe superar los 80%
