---
title: Información general acerca de las actualizaciones de Windows Server | Microsoft Docs
description: Obtenga información general sobre la actualización de Windows Server, junto con lo que debe pensar antes de realizar la actualización real.
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124795"
---
# <a name="overview-about-windows-server-upgrades"></a>Información general acerca de las actualizaciones de Windows Server

El proceso de actualización a una versión más reciente de Windows Server puede variar en gran medida según el sistema operativo desde el que se inicie y la ruta que se lleve a cabo. Usamos los siguientes términos para distinguir las diferentes acciones que podrían estar implicadas en una nueva implementación de Windows Server.

- **Actualización.** También se conoce como "actualización local". Puede pasar de una versión anterior del sistema operativo a una versión más reciente, a la vez que se mantiene en el mismo hardware físico. **Este es el método que se va a tratar en esta sección.**

    >[!Important]
    >Las actualizaciones en contexto también pueden ser compatibles con las compañías de la nube pública o privada; sin embargo, debe consultar a su proveedor de servicios en la nube para obtener los detalles. Además, no podrá realizar una actualización en contexto en ningún servidor de Windows que esté configurado para **arrancar desde VHD**.

- **Montaje.** También se conoce como "instalación limpia". Puede pasar de una versión anterior del sistema operativo a una versión más reciente y eliminar el sistema operativo anterior.

- **Migraciones.** Para pasar de una versión anterior del sistema operativo a una versión más reciente del sistema operativo, puede transferirlo a un conjunto diferente de hardware o máquina virtual.

- **Actualización gradual del sistema operativo del clúster.** Actualice el sistema operativo de los nodos del clúster sin detener las cargas de trabajo de Hyper-V o Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Para obtener más información, consulte [actualización gradual del sistema operativo del clúster](../failover-clustering/cluster-operating-system-rolling-upgrade.md) .

- **Conversión de licencia.** Convierta una edición determinada de la versión a otra edición de la misma versión en un solo paso con un comando simple y la clave de licencia adecuada. Llamamos a esta "conversión de licencia". Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter.

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>¿A qué versión de Windows Server debo actualizar?

Se recomienda actualizar a la versión más reciente de Windows Server: Windows Server 2019. La ejecución de la versión más reciente de Windows Server le permite usar las características más recientes, incluidas las características de seguridad más recientes, y ofrece el mejor rendimiento.

Sin embargo, sabemos que no siempre es posible. Puede usar el siguiente diagrama para averiguar a qué versión de Windows Server puede actualizar, en función de la versión en la que esté actualmente:

![Rutas de actualización en contexto disponibles](media/upgrade-paths.png)

Windows Server se puede actualizar normalmente a través de al menos una y, en ocasiones, incluso dos versiones. Por ejemplo, Windows Server 2012 R2 y Windows Server 2016 pueden actualizarse en contexto a Windows Server 2019.

También puede actualizar desde una versión de evaluación del sistema operativo a una versión comercial, de una versión comercial anterior a una versión más reciente o, en algunos casos, de una edición de licencia por volumen del sistema operativo a una edición comercial normal. Para obtener más información acerca de las opciones de actualización distintas de la actualización local, consulte [Opciones de actualización y conversión para Windows Server](../get-started/supported-upgrade-paths.md).
