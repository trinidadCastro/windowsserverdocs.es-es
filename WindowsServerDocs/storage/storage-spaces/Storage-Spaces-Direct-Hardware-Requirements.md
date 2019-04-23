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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849326"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Requisitos de hardware de Espacios de almacenamiento directo

> Se aplica a: Windows Server 2016, Windows Server Insider Preview

En este tema se describe los requisitos mínimos de hardware para espacios de almacenamiento directo.

Para entornos de producción, Microsoft recomienda estos [definido por el Software de Windows Server](https://microsoft.com/wssd) ofrece hardware y software de nuestros asociados, que incluyen herramientas de implementación y procedimientos. Están diseñadas, ensambla y validadas con respecto a nuestra arquitectura de referencia para garantizar la compatibilidad y confiabilidad, por lo que ponerse en marcha rápidamente. Obtenga más información en [ https://microsoft.com/wssd ](https://microsoft.com/wssd).

![logotipos de nuestros socios definida de Software de Windows Server](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > Desea evaluar espacios de almacenamiento directo, pero no tienen hardware? Usar máquinas virtuales de Hyper-V o Azure, como se describe en [usando espacios de almacenamiento directo en clústeres invitados de máquina virtual](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Requisitos básicos

Los sistemas, componentes, dispositivos y controladores deben ser **certificado para Windows Server 2016** por la [Windows Server Catalog](https://www.windowsservercatalog.com). Además, se recomienda que los adaptadores de red, unidades, adaptadores de bus host y servidores tengan la **estándar del centro de datos definidas mediante software (SDDC)** o **Premium de centro de datos definidas mediante software (SDDC)** calificaciones adicionales (AQs), como se muestra a continuación. Hay más de 1.000 componentes con el AQs SDDC.

![captura de pantalla del catálogo de Windows Server que se muestra el AQs SDDC](media/hardware-requirements/sddc-aqs.png)

El clúster configurado completamente (servidores, redes y almacenamiento) debe superar todas [las pruebas de validación de clúster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) por el Asistente en el Administrador de clústeres de conmutación por error o con el `Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) en PowerShell.

Además, se aplican los siguientes requisitos:

## <a name="servers"></a>Servidores

- Mínimo de 2 servidores y máximo de 16 servidores
- Recomienda que todos los servidores del mismo fabricante y modelo

## <a name="cpu"></a>CPU

- Intel Nehalem o procesador compatible más adelante; o
- AMD EPYC o procesador compatible más tarde

## <a name="memory"></a>Memoria

- Memoria de Windows Server, las máquinas virtuales y otras aplicaciones o cargas de trabajo; signo más
- 4 GB de RAM por cada terabyte (TB) de la capacidad de unidad de caché en cada servidor, para los metadatos de espacios de almacenamiento directo

## <a name="boot"></a>Arranque

- Cualquier dispositivo de arranque compatible con Windows Server, que [ahora incluye SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 es reflejado **no** necesarios, pero se admite para el arranque
- Recomendado: Tamaño mínimo de 200 GB

## <a name="networking"></a>Funciones de red

Valor mínimo (para el nodo de pequeña escala de 2-3)
- Interfaz de red de 10 Gbps
- Conexión directa (cluster sin Switch) es compatible con 2 nodos

Se recomienda (para alto rendimiento, en la escala o las implementaciones de nodos 4 +)
- NIC que son remotos acceso directo a memoria (RDMA) capaz, iWARP (recomendado) o RoCE
- Dos o varias NIC para obtener redundancia y rendimiento
- Interfaz de red de 25 GB/s o posterior

## <a name="drives"></a>Unidades

Espacios de almacenamiento directo funciona con conexión directa SATA, SAS o NVMe unidades que se conectan físicamente a un solo servidor. Para obtener más ayuda para elegir las unidades, consulte el tema [Elegir las unidades](choosing-drives.md).

- Las unidades SATA, SAS y NVMe (M.2, U.2 y agregar tarjeta) son compatibles todos
- se admiten 512n, 512e y las unidades nativas de 4K
- Deben proporcionar las unidades de estado sólidas [protección de pérdida de energía](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Consulte el mismo número y tipos de unidades en cada servidor: [unidad consideraciones de simetría](drive-symmetry-considerations.md)
- Controlador de NVMe es en el cuadro de Microsoft o NVMe controlador actualizado.
- Recomendado: Número de unidades de capacidad es un múltiplo entero del número de unidades de caché
- Recomendado: Unidades de caché deben tener la escritura alta resistencia: al menos 3 unidad escrituras por día (DWPD) o al menos 4 terabytes escritos (TBW) por día: consulte [unidad descripción se escribe al día (DWPD), terabytes escrito (TBW) y el mínimo recomiendan para el almacenamiento Espacios directo](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Le mostramos cómo se pueden conectar unidades de espacios de almacenamiento directo:

1. Unidades SATA de conexión directa
2. Unidades de NVMe de conexión directa
3. Adaptador de bus host (HBA) de SAS con unidades SAS
4. Adaptador de bus host (HBA) de SAS con unidades SATA
5. **NO SE ADMITE:** RAID de SAN (canal de fibra, iSCSI, FCoE) o tarjetas controladoras de almacenamiento. Las tarjetas de bus host (HBA) del adaptador deben implementar el modo de acceso directo sencillo.

![diagrama de unidad compatible interconexiones](media/hardware-requirements/drive-interconnect-support-1.png)

Unidades pueden ser internas del servidor o en un revestimiento externo está conectado a un solo servidor. Es necesario para la identificación y asignación de espacio de SCSI Enclosure Services (SES). Cada contenedor externo debe presentar un identificador único (Id. único).

1. Unidades internas al servidor
2. Las unidades en un revestimiento externo ("JBOD") conectado a un servidor
3. **NO SE ADMITE:** Contenedores SAS compartidos conectan a varios servidores o cualquier forma de múltiples rutas de acceso E/S (MPIO), donde las unidades son accesibles por varias rutas de acceso.

![diagrama de unidad compatible interconexiones](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Número mínimo de unidades de disco (excluye la unidad de arranque)

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
   > Esta tabla proporciona el mínimo para las implementaciones de hardware. Si tiene que implementar con máquinas virtuales y virtualizados almacenamiento, como se muestra en Microsoft Azure, vea [usando espacios de almacenamiento directo en clústeres invitados de máquina virtual](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacidad máxima

- Recomendado: Capacidad máxima de almacenamiento sin formato 100 terabytes (TB) por servidor
- 1 petabyte (1.000 TB) sin procesar capacidad máxima del grupo de almacenamiento
