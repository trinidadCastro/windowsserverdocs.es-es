---
title: Presentación de MultiPoint Services
description: Proporciona información general de Multipoint Services, una manera de permitir que varios usuarios compartan un sistema
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: c1f317909b05c421058041bcd2546c4cc0e704bb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970642"
---
# <a name="introducing-multipoint-services"></a>Presentación de MultiPoint Services
El rol Multipoint Services en Windows Server 2016 permite que varios usuarios, cada uno con su propia experiencia de Windows independiente y familiar, compartan simultáneamente un solo equipo. Hay varias maneras en que los usuarios pueden tener acceso a sus sesiones. Una manera es por la comunicación remota en el servidor mediante las [aplicaciones de escritorio remoto](../remote-desktop-services/clients/remote-desktop-clients.md) con cualquier dispositivo. Otra manera es a través de las estaciones físicas que se conectan a MultiPoint Server:

-   Directamente a los puertos de vídeo del equipo

-   A través de clientes USB especializados (también conocidos como concentradores USB multifunción), así como a través de dispositivos USB a través de Ethernet similares.

-   A través de la red de área local (LAN)

Cada uno de estos métodos se describe con más detalle en las [estaciones de Multipoint Services](MultiPoint-services-Stations.md) más adelante en este documento.

En este documento se tratan los siguientes factores que se deben tener en cuenta al planear la implementación de Multipoint Services:

-   ¿Qué tipo de equipos de escritorio usar con el sistema Multipoint Services: ¿necesitará sesiones, máquinas virtuales o equipos Windows?

-   [Selección de hardware para el sistema Multipoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md): ¿Qué decisiones de hardware debe tomar?

-   [Requisitos de hardware y recomendaciones de rendimiento](Hardware-Requirements-and-Performance-Recommendations.md): ¿Qué hardware se requiere para Multipoint Services?

-   [Planeación de sitios de Multipoint Services](MultiPoint-services-Site-Planning.md): ¿Dónde se encuentran los equipos que ejecutan Multipoint Services y sus estaciones y cómo se configuran?

-   [Consideraciones de red y cuentas de usuario](Network-Considerations-and-User-Accounts.md): el entorno de red en el que se implementa el sistema Multipoint Services puede afectar al modo en que se administran las cuentas de usuario. ¿Cuál es su entorno de red? ¿Cómo se administrarán las cuentas de usuario?

-   [Almacenar archivos con Multipoint Services](Storing-Files-with-MultiPoint-services.md): ¿Dónde se almacenarán los archivos de usuario y cómo se tendrá acceso a ellos?

-   [Lista de comprobación de implementación previa](Predeployment-Checklist.md)