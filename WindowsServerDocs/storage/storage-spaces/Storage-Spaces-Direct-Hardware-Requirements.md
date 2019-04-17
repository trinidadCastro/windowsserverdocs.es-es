---
title: Requisitos de hardware de Espacios de almacenamiento directo
ms.prod: windows-server-threshold
description: Requisitos de hardware mínimos para las pruebas de Espacios de almacenamiento directo.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: 84d10ab3e25500720dd13e2ba057dc3c5bf05a6f
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232534"
---
# Requisitos de hardware de Espacios de almacenamiento directo

> Se aplica a: Windows Server 2016, Windows Server Insider Preview

En este tema se describe los requisitos mínimos de hardware para espacios de almacenamiento directo.

Para producción, Microsoft recomienda estas ofertas de hardware y software [Definido por Software de Windows Server](https://microsoft.com/wssd) de nuestros socios, que incluye procedimientos y herramientas de implementación. Están diseñadas, ensamblan y validan con nuestra arquitectura de referencia para garantizar la compatibilidad y confiabilidad, por lo que obtienes y ejecutar rápidamente. Más información en [https://microsoft.com/wssd](https://microsoft.com/wssd).

![logotipos de nuestros socios de Windows Server definido por Software](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > Va a evaluar espacios de almacenamiento directo, pero no tienes el hardware? Usar Hyper-V o las máquinas virtuales de Azure, tal como se describe en el [Uso de espacios de almacenamiento directo en clústeres de máquina virtual de invitado](storage-spaces-direct-in-vm.md).

## Requisitos de base

Los sistemas, componentes, dispositivos y controladores deben ser la **Certificación de Windows Server 2016** por el [Catálogo de Windows Server](https://www.windowsservercatalog.com). Además, te recomendamos que los servidores, las unidades, adaptadores de bus host y adaptadores de red tienen las calificaciones adicionales **estándar del centro de datos de Software-Defined (SDDC)** o **el centro de datos de Software-Defined (SDDC) Premium** (AQs), como se representa a continuación. Hay más de 1.000 componentes con el AQs SDDC.

![captura de pantalla del catálogo de Windows Server que muestra la AQs SDDC](media/hardware-requirements/sddc-aqs.png)

El clúster completamente configurado (servidores, redes y almacenamiento) debe pasar todas las [pruebas de validación de clúster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) por el Asistente en el Administrador de clústeres de conmutación por error o con el `Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) de PowerShell.

Además, se aplican los siguientes requisitos:

## Servidores

- Mínimo de 2 servidores y máximo de 16 servidores
- Recomienda que todos los servidores sea el mismo fabricante y modelo

## CPU

- Procesador Intel Nehalem o posterior compatible; o
- AMD EPYC o procesador compatible con versiones posteriores

## Memoria

- Memoria para Windows Server, las máquinas virtuales y otras aplicaciones o cargas de trabajo; Plus
- 4 GB de RAM por terabyte (TB) de capacidad de unidad de caché en cada servidor, para los metadatos de espacios de almacenamiento directo

## Arranque

- Cualquier dispositivo de arranque compatible con Windows Server, que [ahora incluye SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 reflejo es **no** necesarios, pero es compatible con el arranque
- Recomendación: el tamaño mínimo de 200 GB

## Redes

Mínimo (para el nodo de pequeña escala 2-3)
- Interfaz de red de 10 Gbps como mínimo
- Conexión directa (cluster sin Switch) es compatible con 2 nodos

Se recomienda (para un alto rendimiento, al escala o las implementaciones de 4 + nodos)
- NICs compatibles remoto-acceso directo a memoria (RDMA) capaz, (recomendado) de iWARP o RoCE
- Dos o más NIC para redundancia y rendimiento
- Interfaz de red 25 Gbps o superior

## Unidades

Espacios de almacenamiento directo funciona con unidades conectadas directamente SATA, SAS o NVMe conectadas físicamente a un solo servidor. Para obtener más ayuda para elegir las unidades, consulte el tema [Elegir las unidades](choosing-drives.md).

- Unidades SATA, SAS y NVMe (M.2, U.2 y tarjeta Add) son compatibles
- 512n, 512e y unidades de 4K nativas son compatibles
- Unidades de estado sólido deben proporcionar [protección contra pérdida de energía](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- [Consideraciones de simetría de unidad](drive-symmetry-considerations.md) de consulta de mismo número y los tipos de unidades en cada servidor:
- Controlador NVMe es en el equipo de Microsoft o actualiza el controlador NVMe.
- Recomendación: Número de unidades de capacidad es un múltiplo entero del número de unidades de caché
- Recomendación: Las unidades de caché deben tener una resistencia alta de escritura: al menos 3 escrituras de unidad-por-día (DWPD) o al menos 4 terabytes (tbw) por día: consulta [Understanding drive escrituras por día (DWPD), escrito de terabytes (TBW) y el mínimo recomiendan para almacenamiento Espacios de Direct](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Aquí es cómo se pueden conectar unidades para espacios de almacenamiento directo:

1. Unidades SATA conectado directo
2. Unidades de NVMe de conexión directa
3. Adaptador de bus host (HBA) de SAS con unidades SAS
4. Adaptador de bus host (HBA) de SAS con unidades SATA
5. **No admite:** RAID SAN (canal de fibra, iSCSI, FCoE) o tarjetas controladoras de almacenamiento. Tarjetas de bus host (HBA) del adaptador deben implementar el modo de paso a través de simple.

![diagrama de unidad admitidos interconexiones](media/hardware-requirements/drive-interconnect-support-1.png)

Las unidades pueden ser internas para el servidor o en un alojamiento externo está conectado a un solo servidor. Servicios de revestimiento de SCSI (SES) es necesario para la identificación y asignación de ranura. Cada contenedor externo debe presentar un identificador único (Id. único).

1. Unidades internas para el servidor
2. Unidades de disco en un alojamiento externo ("JBOD") conectado a un servidor
3. **No admite:** Contenedores unidad SAS compartidos conectado a varios servidores o ninguna forma de múltiples rutas E/S (MPIO), donde las unidades son accesibles mediante varias rutas de acceso.

![diagrama de unidad admitidos interconexiones](media/hardware-requirements/drive-interconnect-support-2.png)

### Número mínimo de unidades (excluye la unidad de arranque)

- Si hay unidades que se usan como memoria caché, debe haber al menos 2 por servidor.
- Debe haber al menos 4 unidades de capacidad (no de caché) por servidor.

| Tipos de unidades presentes   | Número mínimo necesario |
|-----------------------|-------------------------|
| Todas las NVMe (mismo modelo) | 4 NVMe                  |
| Todas las SSD (mismo modelo)  | 4 SSD                   |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + otras 4       |

   >[!NOTE]
   > Esta tabla proporciona el valor mínimo para las implementaciones de hardware. Si estás implementando con máquinas virtuales y virtualizadas de almacenamiento, como en Microsoft Azure, consulta [Usando espacios de almacenamiento directo en clústeres de máquina virtual de invitado](storage-spaces-direct-in-vm.md).

### Capacidad máxima

- Recomendación: Capacidad máxima de almacenamiento sin procesar 100 terabytes (TB) por servidor.
- 1 petabyte (1000 TB) sin procesar capacidad máxima del grupo de almacenamiento
