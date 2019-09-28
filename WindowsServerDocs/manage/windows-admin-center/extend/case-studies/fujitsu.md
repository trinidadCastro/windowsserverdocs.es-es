---
title: 'Caso práctico del SDK del centro de administración de Windows: Fujitsu'
description: 'Caso práctico del SDK del centro de administración de Windows: Fujitsu'
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9acfa873e4ce7d3e91a23abff726836f0e11ce59
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357220"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensiones de estado y RAID de Fujitsu ServerView

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Incorporación de visibilidad de un extremo a otro, desde el sistema operativo al hardware, al centro de administración de Windows

Fujitsu es una empresa líder de tecnología de comunicación y información en japonés y un fabricante de productos de servidor [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) y [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . [Fujitsu ServerView Management Suite](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) proporciona un completo conjunto de herramientas para la administración del ciclo de vida del servidor, incluido un agente del lado servidor que proporciona una interfaz CIM y PowerShell para la administración de hardware.

Fujitsu vio una oportunidad para integrarse fácilmente con el centro de administración de Windows, ya que proporcionaba interfaces CIM y de PowerShell que podían comunicarse con los agentes del lado servidor. El equipo de desarrollo de Fujitsu era capaz de implementar con facilidad las llamadas CIM a las que les conocía el agente y visualizar la información en el centro de administración de Windows con los componentes de interfaz de usuario disponibles.

![Extensión Fujitsu: vista de árbol de estado](../../media/extend-case-study-fujitsu/health-tree.png)

Una vez que el equipo se ha familiarizado con el SDK del centro de administración de Windows, la adición de la interfaz de usuario para exponer información adicional de hardware suele ser solo unas pocas líneas de código HTML y se podía expandir rápidamente desde una sola herramienta para mostrar una vista de resumen del componente de hardware mantenimiento, vistas detalladas para los registros de eventos del sistema, monitor de controladores, vistas independientes del procesador, memoria, ventiladores, fuentes de alimentación, temperaturas y voltajes e incluso una herramienta adicional para la administración de RAID. El uso de controles de interfaz de usuario disponibles en el SDK, como los controles de panel de árbol, cuadrícula y detalle, permitió al equipo compilar rápidamente la interfaz de usuario y lograr un diseño visual y de interacción muy similar al resto del centro de administración de Windows.

![Extensión Fujitsu: vista de árbol RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extensión Fujitsu: vista de volúmenes RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

La asociación entre Fujitsu y el equipo del centro de administración de Windows muestra claramente el valor de integración dentro del centro de administración de Windows, lo que permite a los clientes tener una visión completa de los roles y servicios del servidor, el sistema operativo y la administración de hardware. .