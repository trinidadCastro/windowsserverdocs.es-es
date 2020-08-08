---
title: 'Caso práctico del SDK del centro de administración de Windows: Fujitsu'
description: 'Caso práctico del SDK del centro de administración de Windows: Fujitsu'
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.openlocfilehash: 15f0120fdf1792ad127a9f5fa0e045ef7df18e85
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958753"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensiones de estado y RAID de Fujitsu ServerView

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Incorporación de visibilidad de un extremo a otro, desde el sistema operativo al hardware, al centro de administración de Windows

Fujitsu es una empresa líder de tecnología de comunicación y información en japonés y un fabricante de productos de servidor [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) y [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . [Fujitsu ServerView Management Suite](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) proporciona un completo conjunto de herramientas para la administración del ciclo de vida del servidor, incluido un agente del lado servidor que proporciona una interfaz CIM y PowerShell para la administración de hardware.

Fujitsu vio una oportunidad para integrarse fácilmente con el centro de administración de Windows, ya que proporcionaba interfaces CIM y de PowerShell que podían comunicarse con los agentes del lado servidor. El equipo de desarrollo de Fujitsu era capaz de implementar con facilidad las llamadas CIM a las que les conocía el agente y visualizar la información en el centro de administración de Windows con los componentes de interfaz de usuario disponibles.

![Extensión Fujitsu: vista de árbol de estado](../../media/extend-case-study-fujitsu/health-tree.png)

Una vez que el equipo se ha familiarizado con el SDK del centro de administración de Windows, la adición de la interfaz de usuario para exponer información adicional de hardware suele ser solo unas pocas líneas de código HTML y se podía expandir rápidamente desde una sola herramienta para mostrar una vista de resumen del estado de los componentes de hardware, vistas detalladas de los registros de eventos del sistema, monitor de controladores, vistas , ventiladores, fuentes de alimentación, temperaturas y voltajes e incluso una herramienta adicional para la administración de RAID. El uso de controles de interfaz de usuario disponibles en el SDK, como los controles de panel de árbol, cuadrícula y detalle, permitió al equipo compilar rápidamente la interfaz de usuario y lograr un diseño visual y de interacción muy similar al resto del centro de administración de Windows.

![Extensión Fujitsu: vista de árbol RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extensión Fujitsu: vista de volúmenes RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

La asociación entre Fujitsu y el equipo del centro de administración de Windows muestra claramente el valor de integración dentro del centro de administración de Windows, lo que permite a los clientes tener una visión completa de los roles y servicios del servidor, el sistema operativo y la administración de hardware.