---
title: Directrices de optimización del rendimiento de Windows Server 2016
description: Directrices de optimización del rendimiento para Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1445ae40428bc8626f8e2a12cbfc2a1e6f41592f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891966"
---
# <a name="performance-tuning-guidelines-for-windows-server-2016"></a>Directrices de optimización del rendimiento para Windows Server 2016

Al ejecutar un sistema de servidor en su organización, podría tener necesidades empresariales que no se satisfagan con la configuración predeterminada del servidor. Por ejemplo, es posible que necesite el menor consumo de energía posible, o la menor latencia posible o el máximo rendimiento posible en el servidor. En esta guía, encontrará un conjunto de directrices que puede usar para optimizar la configuración del servidor en Windows Server 2016 y obtener ganancias de rendimiento incremental o eficiencia energética, especialmente cuando la naturaleza de la carga de trabajo varía poco con el tiempo.

Es importante los cambios de optimización tengan en cuenta el hardware, las cargas de trabajo, las asignaciones de energía y los objetivos de rendimiento del servidor. En esta guía se describe cada configuración y su posible efecto para ayudarle a tomar una decisión fundada sobre su relevancia para sus objetivos de uso del sistema, de las carga de trabajo, de rendimiento y de uso de energía.

> [!warning]
> La configuración del Registro y los parámetros de optimización cambiaron significativamente entre distintas versiones de Windows Server. Asegúrese de usar las directrices de optimización más reciente para evitar resultados inesperados.

## <a name="in-this-guide"></a>En esta guía
En esta guía, las directrices de rendimiento y optimización para Windows Server 2016 se organizan en tres categorías de optimización:

|Hardware del servidor | Rol de servidor | Subsistema del servidor |
|:---:|:---:|:---:|
|[Consideraciones de rendimiento de hardware](hardware/index.md) |[Servidores Active Directory](role/active-directory-server/index.md) |[Administración de caché y memoria](subsystem/cache-memory-management/index.md)|
|[Consideraciones de alimentación del hardware](hardware/power.md)|[Servidores de archivos](role/file-server/index.md)|[Subsistema de redes](../../networking/technologies/network-subsystem/net-sub-performance-top.md)|
||[Servidores de Hyper-V](role/hyper-v-server/index.md)|[Espacios de almacenamiento directo](subsystem/storage-spaces-direct/index.md)|
||[Servicios de Escritorio remoto](role/remote-desktop/session-hosts.md)|[Redes definidas por software (SDN)](subsystem/software-defined-networking/index.md)|
||[Servidores web](role/web-server/index.md)||
||[Contenedores de Windows Server](role/windows-server-container/index.md)||


## <a name="changes-in-this-version"></a>Cambios en esta versión

### <a name="sections-added"></a>Secciones agregadas
- [Consideraciones sobre la configuración del tipo de instalación de Nano Server](../../get-started/getting-started-with-nano-server.md)


- [Redes definidas por software](subsystem/software-defined-networking/index.md), incluidas [guías de configuración de puertas de enlace HNV](subsystem/software-defined-networking/hnv-gateway-performance.md) y [SLB](subsystem/software-defined-networking/slb-gateway-performance.md)

- [Espacios de almacenamiento directo](subsystem/storage-spaces-direct/index.md)

- [HTTP1.1 y HTTP2](role/web-server/http-performance.md)

- [Contenedores de Windows Server](role/windows-server-container/index.md)

### <a name="sections-changed"></a>Secciones cambiadas

- Actualizaciones en la sección [Active Directory guidance](role/active-directory-server/index.md) (Guía de Active Directory)

- Actualizaciones en la sección [File Server guidance](role/file-server/index.md) (Guía de servidores de archivos)

- Actualizaciones en la sección [Web Server guidance](role/web-server/index.md) (Guía de servidores web)

- Actualizaciones en la sección [Hardware Power guidance](hardware/power.md) (Guía de alimentación de hardware)

- Actualizaciones de la sección [Optimización del rendimiento para PowerShell](powershell/index.md)

- Actualizaciones importantes para la sección [Hyper-V guidance](role/hyper-v-server/index.md) (Guía de Hyper-V)

- *Se quitó Performance Tuning for Workloads* (Optimización de rendimiento para cargas de trabajo), se agregaron punteros a recursos pertinentes en el [artículo Recursos de optimización de rendimiento adicionales](additional-resources.md)

- *Se quitaron las secciones de almacenamiento dedicado*, a favor de la nueva sección [Espacios de almacenamiento directo](subsystem/storage-spaces-direct/index.md) y el contenido canónico de Technet

- *Se quitó la sección de redes dedicada*, a favor de contenido canónico de Technet  
