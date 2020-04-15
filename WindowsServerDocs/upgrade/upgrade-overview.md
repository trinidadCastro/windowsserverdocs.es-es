---
title: Información general acerca de las actualizaciones de Windows Server | Microsoft Docs
description: Obtén información general sobre la actualización de Windows Server, junto con los aspectos a tener en cuenta antes de realizar la actualización real.
ms.prod: windows-server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 1ac4cbe8b9bda4ac2de2c7ad7ec27b1534c0de72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854238"
---
# <a name="overview-about-windows-server-upgrades"></a>Información general acerca de las actualizaciones de Windows Server

El proceso de actualización a una versión más reciente de Windows Server puede variar enormemente en función del sistema operativo desde el que se parte y el método elegido para hacerlo. Se usan los siguientes términos para distinguir las diferentes acciones que podrían tener lugar en una nueva implementación de Windows Server.

- **Actualización.** También se conoce como "actualización local". Puedes pasar de una versión anterior del sistema operativo a una versión más reciente en el mismo hardware físico. **Este es el método que se tratará en esta sección.**

    >[!Important]
    >Es posible que las empresas de nube pública o privada también admitan actualizaciones locales; sin embargo, debe consultar a su proveedor de nube para obtener los detalles. Además, no podrá realizar una actualización local en ninguna instancia de Windows Server configurada en **Arranque desde VHD**.

- **Instalación.** También se conoce como "instalación limpia". Puedes pasar de una versión anterior del sistema operativo a una versión más reciente y eliminar el sistema operativo anterior.

- **Migración.** Para pasar de una versión anterior del sistema operativo a una más reciente, cambia a un conjunto diferente de hardware o máquina virtual.

- **Actualización gradual del sistema operativo del clúster.** El sistema operativo de los nodos de clúster se actualiza sin detener las cargas de trabajo de Hyper-V o del Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Para más información, consulta [Actualización gradual del sistema operativo del clúster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).

- **Conversión de licencia.** Convierte una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Este proceso se denomina "conversión de licencia". Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>¿A qué versión de Windows Server debo actualizar?

Se recomienda actualizar a la versión más reciente de Windows Server: Windows Server 2019. La ejecución de la versión más reciente de Windows Server te permite usar las características más recientes, incluidas las de seguridad, y ofrece el máximo rendimiento.

Sin embargo, sabemos que no siempre es posible. Puedes usar el siguiente diagrama para averiguar a qué versión de Windows Server puedes actualizar, en función de la versión que tienes actualmente:

![Rutas de acceso de actualización local disponibles](media/upgrade-paths.png)

Windows Server se puede actualizar normalmente a través de una versión (como mínimo) y, en ocasiones, incluso dos. Por ejemplo, se puede realizar la actualización local de Windows Server 2012 R2 y Windows Server 2016 a Windows Server 2019.

También es posible actualizar de una versión de evaluación del sistema operativo a una versión comercial, de una versión comercial antigua a una nueva o, en algunos casos, de una edición de licencias por volumen del sistema operativo a una edición comercial normal. Para obtener más información sobre las opciones de actualización distintas de la actualización local, consulta [Opciones de actualización y conversión de Windows Server](../get-started/supported-upgrade-paths.md).
""'