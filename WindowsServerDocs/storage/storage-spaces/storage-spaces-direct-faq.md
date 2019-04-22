---
title: Espacios de almacenamiento directo - preguntas más frecuentes
description: Obtenga información sobre cómo aproximadamente espacios de almacenamiento directo
keywords: Espacios de almacenamiento
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818916"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Espacios de almacenamiento directo - preguntas más frecuentes (P+F)

Este artículo enumeran algunos comunes y preguntas más frecuentes relacionadas con [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>¿Al utilizar espacios de almacenamiento directo con 3 nodos, se pueden obtener rendimiento y los niveles de capacidad?

Sí, puede obtener un nivel de capacidad y rendimiento en una configuración de espacios de almacenamiento directo de 2 o 3 nodos. Sin embargo, debe asegurarse de que tiene 2 dispositivos de capacidad. Esto significa que debe usar los tres tipos de dispositivos: NVME, SSD y HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Sistema de archivos Refs proporciona tiaring en tiempo real con espacios de almacenamiento directo. ¿REFS proporciona la misma funcionalidad con espacios de almacenamiento compartido en 2016?

No, no obtendrá los niveles con espacios de almacenamiento compartido con 2016 en tiempo real. Se trata sólo de espacios de almacenamiento directo. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>¿Puedo usar un sistema de archivos NTFS con espacios de almacenamiento directo?
  
Sí, puede usar el sistema de archivos NTFS con espacios de almacenamiento directo. Sin embargo, se recomienda REFS. NTFS no proporciona los niveles en tiempo real. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>He configurado 2 clústeres de espacios de almacenamiento directo de nodos, donde se configura el disco virtual como resistencia reflejada 2 vías. ¿Si agrego un nuevo dominio de error, cambiará la resistencia del disco virtual existente?

Después de haber agregado el nuevo dominio de error, los nuevos discos virtuales que cree se ubicará en 3-way mirror. Sin embargo, el disco virtual existente seguirá siendo un disco reflejado 2 vías. Puede copiar los datos a los nuevos discos virtuales desde los volúmenes existentes para aprovechar las ventajas de la resistencia nuevo.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>Espacios de almacenamiento directo se creó mediante el autoconfig:0 conmutador y el grupo creado manualmente. Cuando se intenta consultar el grupo de espacios de almacenamiento directo para crear un nuevo volumen, obtengo un mensaje que dice: "Enable-ClusterS2D nuevo." ¿Qué debo hacer?

De forma predeterminada, al configurar espacios de almacenamiento directo mediante el cmdlet enable-S2D, el cmdlet hace todo para usted. Crea el grupo y los niveles. Cuando se usa autoconfig:0, todo lo que debe realizarse manualmente. Si ha creado solo el grupo, el nivel no es necesario crear. Si no creó los niveles de en absoluto o no crear niveles de forma correspondiente a los dispositivos conectados, recibirá un mensaje de error "Enable-ClusterS2D nuevo". Se recomienda que no utilice el modificador de configuración automática en un entorno de producción. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>¿Es posible agregar un disco de giro (HDD) al bloque de espacios de almacenamiento directo después de haber creado espacios de almacenamiento directo con dispositivos SSD?

No. De forma predeterminada, si usa el tipo de dispositivo único para crear el grupo, no necesitará configurar discos de la memoria caché y todos los discos se usaría para la capacidad. Puede agregar discos NVME a la configuración y discos NVME se configuraría para caché.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>He configurado un dominio de error 2 rack: BASTIDOR 1 tiene 2 dominios de error, bastidor 2 tiene 1 dominio de error. Cada servidor tiene 4 dispositivos de 100 GB de capacidad. ¿Usar todos los 1200 GB de espacio del grupo?

No, puede usar solo 800 GB. En un dominio de error bastidor, debe asegurarse de que tiene una configuración de 2-way mirror, por lo que cada chuck y su land duplicado en un bastidor diferentes.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>¿Cuál debe ser el tamaño de caché al voy a configurar espacios de almacenamiento directo?

Se debe ajustar el tamaño de la memoria caché para alojar el espacio de trabajo (los datos que se está usando activamente leídos o escritos en un momento dado) de las aplicaciones y cargas de trabajo.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>¿Cómo puedo determinar el tamaño de caché que se está usando espacios de almacenamiento directo?

Use la utilidad integrada PerfMon para inspeccionar los errores de caché. Revise la memoria caché se pierda lecturas por segundo del contador de disco de clúster almacenamiento híbrido. Recuerde que si hay demasiados lecturas falta la memoria caché, la memoria caché es demasiado pequeña y desea expandirlo. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>¿Hay una calculadora que muestra el tamaño exacto de los discos que se se reservan para la memoria caché, la capacidad y resistencia que me permitirían a planear mejor?

Puede usar la calculadora de espacios de almacenamiento para ayudar a planear la. Está disponible en http://aka.ms/s2dcalc.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>¿Qué es la mejor configuración recomendaría al configurar 6 servidores y 3 bastidores?

Usar 2 servidores en cada uno de los bastidores para obtener la resistencia de disco virtual de un reflejo de la manera de 3. Recuerde que la configuración de bastidor funcionaría correctamente solo si va a proporcionar la configuración del sistema operativo de la manera en que se coloca en el bastidor. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>¿Puedo habilitar el modo de mantenimiento de un disco concreto en un servidor específico en el clúster de espacios de almacenamiento directo?

Sí, puede habilitar el modo de mantenimiento de almacenamiento en un disco específico y un dominio de error específico. El comando Enable-StorageMaintenanceMode se invoca automáticamente cuando se pausa un nodo. Puede habilitarlo para un disco concreto, ejecute el comando siguiente:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>¿Espacios de almacenamiento directo estará en mi hardware?

Se recomienda que póngase en contacto con su proveedor de hardware para comprobar la compatibilidad. Los proveedores de hardware probar la solución en su hardware y un comentario sobre si se admite o no. Por ejemplo, en el momento de redactar este artículo, los servidores como R730 / R730xd / R630 que tienen más de 8 ranuras de la unidad puede admitir SES y son compatibles con espacios de almacenamiento directo. Dell admite solo la HBA330 con espacios de almacenamiento directo. R620 no admite SES y no es compatible con espacios de almacenamiento directo.

Para el hardware más información de soporte técnico, visite el sitio Web siguiente: Catálogo de Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>¿Cómo hace espacios de almacenamiento directo que el uso de SES?

Espacios de almacenamiento directo usa la asignación de SCSI Enclosure Services (SES) para asegurarse de que los bloques de datos y los metadatos se reparten entre los dominios de error de forma resistente. Si el hardware no admite SES, no hay ninguna asignación de los contenedores y la colocación de los datos no es resistente.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>¿Qué comando se puede utilizar para comprobar la extensión física de un disco virtual?
  
Esta:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
