---
title: Caso práctico de Windows Admin Center SDK - Fujitsu
description: Caso práctico de Windows Admin Center SDK - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814996"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensiones de Fujitsu ServerView salud y RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Permite incorporar visibilidad de extremo a otro, de hardware, sistema operativo a Windows Admin Center

Fujitsu es una empresa líder en japonés información y la comunicación tecnología y un fabricante de [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) y [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) productos de servidor. El [suite de administración de Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) proporciona un conjunto completo de herramientas para el servidor de administración de ciclo de vida, incluidos un agente de servidor que proporciona una interfaz CIM y PowerShell para administración de hardware.

Fujitsu vio la oportunidad para integrar fácilmente con Windows Admin Center como proporcionan las interfaces CIM y de PowerShell que se podrían comunicar con los agentes de servidor. El equipo de desarrollo de Fujitsu fue capaz de implementar las llamadas CIM que estaban familiarizados con el agente y visualizar la información dentro de Windows Admin Center mediante los componentes de interfaz de usuario disponibles fácilmente.

![Extensión de Fujitsu: vista de árbol de estado](../../media/extend-case-study-fujitsu/health-tree.png)

Una vez que se convirtió en el equipo está familiarizado con el SDK de Windows Admin Center, agregar la interfaz de usuario para exponer información de hardware adicionales a menudo era simplemente unas pocas líneas más de código HTML y ellos pudieron rápidamente expandir desde una única herramienta para mostrar una vista de resumen de componente de hardware mantenimiento, vistas detalladas para registros de eventos del sistema, el monitor de controlador, separe las vistas de procesador, memoria, ventiladores, fuentes de alimentación, las temperaturas y voltajes e incluso una herramienta adicional para la administración de RAID. Usar controles de interfaz de usuario disponibles en el SDK como el árbol, controles del panel de cuadrícula y los detalles permitieron al equipo crear rápidamente la interfaz de usuario y para conseguir un diseño visual e interacción muy similar al resto de Windows Admin Center.

![Extensión de Fujitsu: vista de árbol RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Ver volúmenes RAID de extensión de Fujitsu:](../../media/extend-case-study-fujitsu/raid-volumes.png)

La asociación entre Fujitsu y el equipo de Windows Admin Center muestra claramente el valor de la integración en Windows Admin Center, permite a los clientes tengan una visión general de extremo a otro en roles de servidor y servicios, para el sistema operativo y administración de hardware .