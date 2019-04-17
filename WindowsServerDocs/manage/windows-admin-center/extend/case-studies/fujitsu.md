---
title: Centro de administración de Windows SDK caso práctico - Fujitsu
description: Centro de administración de Windows SDK caso práctico - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052379"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensiones de RAID y el mantenimiento de ServerView de Fujitsu

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Incorporación de visibilidad end-to-end, desde el hardware, sistema operativo en el centro de administración de Windows

Fujitsu es una información líderes en japonés y empresa de tecnología de comunicación y un fabricante de productos de servidor [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) y [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . El [conjunto de aplicaciones de administración de Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) proporciona un completo conjunto de herramientas para el servidor de administración de ciclo de vida incluye a un agente del lado del servidor que proporciona una interfaz de CIM y PowerShell para la administración de hardware.

Fujitsu vio una oportunidad para integrar fácilmente con el centro de administración de Windows, tal y como proporcionan interfaces CIM y PowerShell que podrían comunicarse con los agentes del servidor. El equipo de desarrollo de Fujitsu pudo fácilmente las llamadas CIM fueran familiarizados con al agente de implementar y ver la información en el centro de administración de Windows con los componentes de la interfaz de usuario disponibles.

![Extensión de Fujitsu - vista de árbol de mantenimiento](../../media/extend-case-study-fujitsu/health-tree.png)

Una vez que el equipo se convirtió en familiarizado con el SDK del centro de administración de Windows, adición de la interfaz de usuario para exponer información de hardware adicional a menudo era simplemente unas pocas líneas más de código HTML y podían rápidamente expandirse desde una sola herramienta para mostrar una vista de resumen de componente de hardware mantenimiento, vistas detalladas para registros de eventos del sistema, el monitor de controlador, separe las vistas para procesador, memoria, abanicos, fuentes de alimentación, temperaturas y voltajes e incluso una herramienta adicional para la administración de RAID. Uso de controles de la interfaz de usuario disponibles en el SDK como el árbol, controles del panel de cuadrícula y detalle habilitado para el equipo para crear rápidamente la interfaz de usuario y también lograr un diseño visual y la interacción muy similar al resto del centro de administración de Windows.

![Extensión de Fujitsu - vista de árbol RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extensión de Fujitsu - volúmenes RAID ver](../../media/extend-case-study-fujitsu/raid-volumes.png)

La asociación entre el equipo del centro de administración de Windows y de Fujitsu claramente muestra el valor de la integración de dentro del centro de administración de Windows, permitiendo a los clientes tienen una visión de end-to-end de roles de servidor y servicios, en el sistema operativo y a la administración de hardware .