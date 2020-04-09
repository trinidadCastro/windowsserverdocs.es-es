---
title: Requisitos de hardware de Espacios de almacenamiento directo
ms.prod: windows-server
description: Requisitos de hardware mínimos para las pruebas de Espacios de almacenamiento directo.
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 08/05/2019
ms.localizationpriority: medium
ms.openlocfilehash: 42022b6e2e3564d1440e2ba1d45f9f98430242c0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861058"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Requisitos de hardware de Espacios de almacenamiento directo

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se describen los requisitos mínimos de hardware para Espacios de almacenamiento directo.

En el caso de producción, Microsoft recomienda adquirir una solución de hardware/software validada de nuestros asociados, que incluyen herramientas y procedimientos de implementación. Estas soluciones se diseñan, ensamblan y validan con nuestra arquitectura de referencia para garantizar la compatibilidad y la confiabilidad, de modo que pueda ponerse en marcha rápidamente. Para obtener soluciones de Windows Server 2019, visite el [sitio web de soluciones de hcl Azure Stack](https://azure.microsoft.com/overview/azure-stack/hci). En el caso de las soluciones de Windows Server 2016, obtenga más información en [Windows Server Software-Defined](https://microsoft.com/wssd).

   > [!TIP]
   > ¿Desea evaluar Espacios de almacenamiento directo pero no tiene hardware? Use Hyper-V o máquinas virtuales de Azure como se describe en [uso de espacios de almacenamiento directo en clústeres de máquinas virtuales invitadas](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Requisitos básicos

Los sistemas, los componentes, los dispositivos y los controladores deben tener **Windows server 2016 certificado** por el [Catálogo de Windows Server](https://www.windowsservercatalog.com). Además, se recomienda que los servidores, las unidades, los adaptadores de bus host y los adaptadores de red tengan el **estándar del centro de datos definido por software (SDDC)** o el **centro de datos definido por software (SDDC)** , como se muestra a continuación. Hay más de 1.000 componentes con la AQs de SDDC.

![captura de pantalla del catálogo de Windows Server que muestra la AQs de SDDC](media/hardware-requirements/sddc-aqs.png)

El clúster totalmente configurado (servidores, redes y almacenamiento) debe pasar todas las [pruebas de validación de clústeres](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) según el asistente en Administrador de clústeres de conmutación por error o con el [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) `Test-Cluster` en PowerShell.

Además, se aplican los siguientes requisitos:

## <a name="servers"></a>Servidores

- Mínimo de 2 servidores y máximo de 16 servidores
- Se recomienda que todos los servidores sean del mismo fabricante y modelo

## <a name="cpu"></a>CPU

- Procesador compatible con Intel Nehalem o posterior; de
- Procesador compatible con AMD EPYC o posterior

## <a name="memory"></a>Memoria

- Memoria para Windows Server, máquinas virtuales y otras aplicaciones o cargas de trabajo; signos
- 4 GB de RAM por terabyte (TB) de capacidad de la unidad de caché en cada servidor, para metadatos de Espacios de almacenamiento directo

## <a name="boot"></a>Arranque

- Cualquier dispositivo de arranque compatible con Windows Server, que [ahora incluye SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- El reflejo RAID 1 **no** es necesario, pero se admite para el arranque
- Recomendado: 200 GB de tamaño mínimo

## <a name="networking"></a>Funciones de red

Espacios de almacenamiento directo requiere una conexión de red de baja latencia y ancho de banda alto confiable entre cada nodo.  

Interconexión mínima para el nodo 2-3 de pequeña escala
- tarjeta de interfaz de red (NIC) de 10 Gbps o más rápido
- Se recomiendan dos o más conexiones de red de cada nodo para redundancia y rendimiento

Interconexión recomendada para un alto rendimiento, a escala o a implementaciones de 4 + 
- NIC que son compatibles con el acceso directo a memoria remota (RDMA), iWARP (recomendado) o RoCE
- Se recomiendan dos o más conexiones de red de cada nodo para redundancia y rendimiento
- NIC de 25 Gbps o más rápido

Interconexiones de nodo conmutado o sin conmutador
- Cambiado: los conmutadores de red deben estar configurados correctamente para administrar el ancho de banda y el tipo de red.  Si usa RDMA que implementa el protocolo RoCE, la configuración del dispositivo y del conmutador de red es aún más importante. 
- Sin conmutador: los nodos se pueden interconectar mediante conexiones directas, evitando el uso de un modificador.  Es necesario que todos los nodos tengan una conexión directa con todos los demás nodos del clúster.


## <a name="drives"></a>Unidades

Espacios de almacenamiento directo funciona con unidades SATA, SAS o NVMe conectadas directamente que están conectadas físicamente a un solo servidor. Para obtener más ayuda para elegir las unidades, consulte el tema [Elegir las unidades](choosing-drives.md).

- Se admiten todas las unidades SATA, SAS y NVMe (M. 2, U. 2 y de agregación).
- se admiten todas las unidades nativas de 512n, 512e y 4K
- Las unidades de estado sólido deben proporcionar [protección contra la pérdida de energía](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- El mismo número y tipos de unidades en cada servidor: consulte [consideraciones sobre la simetría de unidades](drive-symmetry-considerations.md)
- Los dispositivos de caché deben tener 32 GB o más.
- Al usar dispositivos de memoria persistentes como dispositivos de caché, debe usar dispositivos de capacidad de NVMe o SSD (no puede usar HDD).
- El controlador de NVMe es el proporcionado por Microsoft que se incluye en Windows. (stornvme. sys)
- Recomendado: el número de unidades de capacidad es un múltiplo entero del número de unidades de caché
- Recomendado: las unidades de caché deben tener una gran resistencia de escritura: al menos 3 unidades-escrituras por día (DWPD) o al menos 4 terabytes escritas (TBW) al día; vea [Descripción de las escrituras de unidad al día (DWPD), terabytes escritos (TBW) y el mínimo recomendado para espacios de almacenamiento directo](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

A continuación se muestra cómo se pueden conectar las unidades de Espacios de almacenamiento directo:

- Unidades SATA conectadas directamente
- Unidades de NVMe conectadas directamente
- Adaptador de bus host (HBA) SAS con unidades SAS
- Adaptador de bus host (HBA) SAS con unidades SATA
- **no compatible:** Tarjetas de controlador RAID o almacenamiento SAN (Canal de fibra, iSCSI, FCoE). Las tarjetas del adaptador de bus host (HBA) deben implementar el modo de paso a través simple.

![diagrama de interconexiones de unidad compatibles](media/hardware-requirements/drive-interconnect-support-1.png)

Las unidades pueden ser internas en el servidor o en un contenedor externo que esté conectado a un solo servidor. Se requiere SCSI Enclosure Services (SES) para la asignación e identificación de ranuras. Cada contenedor externo debe presentar un identificador único (ID. único).

- Unidades internas del servidor
- Unidades en un contenedor externo ("JBOD") conectadas a un servidor
- **no compatible:** Alojamientos compartidos de SAS conectados a varios servidores o a cualquier forma de e/s de múltiples rutas (MPIO) en las que se puede tener acceso a las unidades mediante múltiples rutas.

![diagrama de interconexiones de unidad compatibles](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Número mínimo de unidades (excluye la unidad de arranque)

- Si hay unidades que se usan como memoria caché, debe haber al menos 2 por servidor.
- Debe haber al menos 4 unidades de capacidad (no de caché) por servidor.

| Tipos de unidades presentes   | Número mínimo necesario |
|-----------------------|-------------------------|
| Toda la memoria persistente (mismo modelo) | 4 memoria persistente |
| Todas las NVMe (mismo modelo) | 4 NVMe                  |
| Todas las SSD (mismo modelo)  | 4 SSD                   |
| Memoria persistente + NVMe o SSD | 2 memoria persistente + 4 NVMe o SSD |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + otras 4       |

   >[!NOTE]
   > En esta tabla se proporciona el mínimo para las implementaciones de hardware. Si va a realizar la implementación con máquinas virtuales y almacenamiento virtualizado, como en Microsoft Azure, consulte [uso de espacios de almacenamiento directo en clústeres de máquinas virtuales invitadas](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacidad máxima

| Valores máximos                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Capacidad sin procesar por servidor | 400 TB               | 100 TB               |
| Capacidad de grupo           | 4 PB (4.000 TB)      | 1 PB                 |
