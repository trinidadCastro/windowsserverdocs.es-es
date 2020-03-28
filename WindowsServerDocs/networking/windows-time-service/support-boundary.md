---
ms.assetid: ''
title: Límite de compatibilidad de la hora de alta precisión
description: En este artículo se describe el límite de compatibilidad del servicio de hora de Windows (W32Time) en entornos que requieren una hora del sistema muy precisa y estable.
author: eross-msft
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 5748d598272c8f096876bab0d24142d38c2fd64b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314962"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Límite de compatibilidad de la hora de alta precisión

>Se aplica a: Windows Server 2016 y Windows 10, versión 1607 o superior

En este artículo se describen los límites de compatibilidad del servicio de hora de Windows (W32Time) en entornos que requieren una hora del sistema muy precisa y estable.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Compatibilidad de alta precisión para Windows 8.1 y 2012 R2 (o anterior)

Las versiones anteriores de Windows (anteriores a Windows 10, 1607, o Windows Server 2016, 1607) no pueden garantizar una hora muy precisa. El servicio de hora de Windows en estos sistemas:

-   Proporcionaba la precisión de hora necesaria para satisfacer los requisitos de autenticación de Kerberos, versión 5.

-   Proporcionaba una hora algo preciso para los clientes y servidores de Windows unidos a un bosque común de Active Directory.

Los requisitos de precisión más estrictos estaban fuera de la especificación de diseño del servicio de hora de Windows en estos sistemas operativos y no se admite.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 y Windows Server 2016

La precisión de la hora en Windows 10 y Windows Server 2016 ha mejorado considerablemente, a la vez que se mantiene totalmente la compatibilidad de NTP con versiones anteriores de Windows. En las condiciones de funcionamiento correctas, los sistemas que ejecutan Windows 10 o Windows Server 2016 y versiones más recientes pueden ofrecer una precisión de 1 segundo, 50 ms (milisegundos) o 1 ms.

>[!IMPORTANT]
>**Orígenes de la hora muy precisos**<br>
>La precisión en la hora que se obtiene en la topología depende en gran medida del uso de un origen de la hora preciso y estable (estrato 1). Hay terceros proveedores que venden hardware de origen de la hora NTP, compatible con Windows, sumamente preciso, basado en Windows y no basado en Windows. Ponte en contacto con tu proveedor para conocer la precisión de sus productos.

>[!IMPORTANT]
>**Precisión de la hora**<br>
>La precisión de la hora conlleva la distribución de un extremo a otro de la hora precisa desde un origen de la hora de autoridad sumamente preciso hasta el dispositivo final. Todo lo que introduce asimetría de red influirá negativamente en la precisión, por ejemplo, los dispositivos físicos de red o una alta carga de CPU en el sistema de destino.

## <a name="high-accuracy-requirements"></a>Requisitos para la alta precisión

En el resto de este documento se describen los requisitos del entorno que deben cumplirse para admitir los objetivos de alta precisión correspondientes.

### <a name="target-accuracy-1-second-1s"></a>Precisión del destino: 1 segundo (1 s)

Para lograr la precisión de 1 s para un equipo de destino específico en comparación con un origen de la hora muy preciso:

-   El sistema de destino debe ejecutar Windows 10 o Windows Server 2016.

-   El sistema de destino debe sincronizar la hora desde una jerarquía NTP de servidores de hora, que culmina en un origen de la hora NTP compatible con Windows y muy preciso.

-   Todos los sistemas operativos Windows de la jerarquía NTP mencionados anteriormente deben configurarse según se documentan en la documentación [Configuración de sistemas para alta precisión](configuring-systems-for-high-accuracy.md).

-   La latencia de red unidireccional acumulativa entre el destino y el origen no debe superar los 100 ms. El retraso acumulativo de la red se mide sumando los retrasos unidireccionales individuales entre pares de nodos cliente-servidor NTP de la jerarquía a partir del destino y finalizando en el origen. Para más información, consulta el documento de sincronización de la hora de alta precisión.

### <a name="target-accuracy-50-milliseconds"></a>Precisión del destino: 50 milisegundos

Se aplican todos los requisitos descritos en la sección **Precisión del destino: 1 segundo**, excepto en el caso de los controles más estrictos que se describen en esta sección.

Los requisitos adicionales para lograr la precisión de 50 ms para un sistema de destino específico son:

-   El equipo de destino debe tener una latencia de red inferior a 5 ms entre su origen de la hora.

-   El sistema de destino no debe estar más allá del estrato 5 de un origen de la hora muy preciso.

    >[!Note]
    >Ejecuta "w32tm /query /status" desde la línea de comandos para ver el estrato.

-   El sistema de destino debe estar dentro de 6 saltos de red o menos desde el origen de la hora muy preciso.

-   El uso promedio de CPU de un día en todos los estratos no debe superar el 90 %.

-   En el caso de los sistemas virtualizados, el uso promedio de CPU de un día del host no debe superar el 90 %.

### <a name="target-accuracy-1-millisecond"></a>Precisión del destino: 1 milisegundo

Se aplican todos los requisitos descritos en las secciones **Precisión del destino: 1 segundo** y **Precisión del destino: 50 milisegundos**, excepto en el caso de los controles más estrictos que se describen en esta sección.

Los requisitos adicionales para lograr la precisión de 1 ms para un sistema de destino específico son:

-   El equipo de destino debe tener una latencia de red inferior a 0,1 ms entre su origen de la hora.

-   El sistema de destino no debe estar más allá del estrato 5 de un origen de la hora muy preciso.

    >[!Note]
    >Ejecuta "w32tm /query /status" desde la línea de comandos para ver el estrato.

-   El sistema de destino debe estar dentro de 4 saltos de red o menos desde el origen de la hora muy preciso.

-   El uso promedio de CPU de un día en cada estrato no debe superar el 80 %.

-   En el caso de los sistemas virtualizados, el uso promedio de CPU de un día del host no debe superar el 80 %.
