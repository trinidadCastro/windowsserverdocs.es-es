---
title: Preguntas más frecuentes sobre Espacios de almacenamiento directo
description: Más información sobre Espacios de almacenamiento directo
keywords: Espacios de almacenamiento
ms.prod: windows-server
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 19dcc1c57fe7c7eea74b003553a0b0a6ab5508aa
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950235"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Espacios de almacenamiento directo preguntas más frecuentes (p + f)

En este artículo se enumeran algunas preguntas frecuentes y frecuentes relacionadas con [espacios de almacenamiento directo](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Al usar Espacios de almacenamiento directo con 3 nodos, ¿puede obtener niveles de rendimiento y capacidad?

Sí, puede obtener un nivel de rendimiento y capacidad en una configuración de Espacios de almacenamiento directo de dos o tres nodos. Sin embargo, debe asegurarse de que tiene 2 dispositivos de capacidad. Esto significa que debe usar los tres tipos de dispositivos: NVME, SSD y HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>El sistema de archivos Refs proporciona tiaring en tiempo real con Espacios de almacenamiento directo. ¿La función REFS proporciona la misma funcionalidad con espacios de almacenamiento compartido en 2016?

No, no obtendrá niveles en tiempo real con espacios de almacenamiento compartido con 2016. Esto solo es para Espacios de almacenamiento directo. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>¿Puedo usar un sistema de archivos NTFS con Espacios de almacenamiento directo?
  
Sí, puede usar el sistema de archivos NTFS con Espacios de almacenamiento directo. Sin embargo, se recomienda REFS. NTFS no proporciona niveles en tiempo real. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>He configurado dos clústeres de Espacios de almacenamiento directo de nodos, donde el disco virtual está configurado como resistencia de reflejo bidireccional. Si agrego un nuevo dominio de error, ¿cambiará la resistencia del disco virtual existente?

Después de agregar el nuevo dominio de error, los nuevos discos virtuales que cree pasarán a reflejo triple. Sin embargo, el disco virtual existente seguirá siendo un disco reflejado de dos vías. Puede copiar los datos en los nuevos discos virtuales desde los volúmenes existentes para obtener las ventajas de la nueva resistencia.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>El Espacios de almacenamiento directo se creó con la configuración automática: 0 modificador y el grupo creado manualmente. Cuando intento consultar el grupo de Espacios de almacenamiento directo para crear un nuevo volumen, aparece un mensaje que dice "Enable-ClusterS2D de nuevo". ¿Qué debo hacer?

De forma predeterminada, al configurar Espacios de almacenamiento directo mediante el cmdlet enable-S2D, el cmdlet hace todo lo posible. Crea el grupo y los niveles. Al usar AutoConfig: 0, todo debe realizarse manualmente. Si solo ha creado el grupo, no se crea necesariamente el nivel. Recibirá un mensaje de error "Enable-ClusterS2D de nuevo" si no ha creado niveles en todos los niveles o no creados de la manera correspondiente a los dispositivos conectados. Se recomienda no usar el modificador de configuración automática en un entorno de producción. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>¿Es posible agregar un disco girado (HDD) al grupo de Espacios de almacenamiento directo después de haber creado Espacios de almacenamiento directo con dispositivos SSD?

No. De forma predeterminada, si usa el tipo de dispositivo único para crear el grupo, no configurará los discos de caché y se usarán todos los discos para la capacidad. Puede agregar discos de NVME a la configuración y los discos de NVME se configurarán para la memoria caché.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>He configurado un dominio de error de 2 bastidores: el bastidor 1 tiene 2 dominios de error, el bastidor 2 tiene un dominio de error. Cada servidor tiene 4 dispositivos de capacidad 100 GB. ¿Puedo usar todos los 1.200 GB de espacio del grupo?

No, solo puede usar 800 GB. En un dominio de error de bastidor, debe asegurarse de que tiene una configuración de reflejo bidireccional para que cada Chuck y sus elementos duplicados se coloquen en un bastidor diferente.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>¿Cuál debe ser el tamaño de la memoria caché cuando se configura Espacios de almacenamiento directo?

La memoria caché debe ajustarse para adaptarse al espacio de trabajo (los datos que se leen o escriben activamente en un momento dado) de las aplicaciones y las cargas de trabajo.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>¿Cómo se puede determinar el tamaño de la memoria caché que usa Espacios de almacenamiento directo?

Use la utilidad integrada PerfMon para inspeccionar los errores de caché. Revise las lecturas de errores de caché por segundo del contador de discos híbridos de almacenamiento de clúster. Recuerde que si faltan demasiadas lecturas en la memoria caché, la memoria caché está insuficiente y es posible que desee expandirla. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>¿Hay una calculadora que muestre el tamaño exacto de los discos que se reservan para la caché, la capacidad y la resistencia que me permitan planear mejor?

Puede usar la calculadora de espacios de almacenamiento para ayudarle con la planeación. Está disponible en https://aka.ms/s2dcalc.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>¿Cuál es la mejor configuración que se recomienda al configurar 6 servidores y 3 bastidores?

Use 2 servidores en cada uno de los bastidores para obtener la resistencia de los discos virtuales de un reflejo tridimensional. Recuerde que la configuración del bastidor funcionaría correctamente solo si proporciona la configuración al sistema operativo de la forma en que se coloca en el bastidor. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>¿Puedo habilitar el modo de mantenimiento para un disco específico en un servidor específico en Espacios de almacenamiento directo clúster?

Sí, puede habilitar el modo de mantenimiento de almacenamiento en un disco específico y un dominio de error específico. El comando enable-StorageMaintenanceMode se invoca automáticamente al pausar un nodo. Puede habilitarlo para un disco específico ejecutando el comando siguiente:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>¿Es Espacios de almacenamiento directo compatible con el hardware?

Se recomienda que se ponga en contacto con el proveedor de hardware para comprobar la compatibilidad. Los proveedores de hardware prueban la solución en su hardware y comentan si es compatible o no. Por ejemplo, en el momento de redactar este documento, los servidores como R730/R730xd/R630 con más de 8 ranuras de unidad pueden admitir SES y son compatibles con Espacios de almacenamiento directo. Dell solo admite el HBA330 con Espacios de almacenamiento directo. R620 no admite SES y no es compatible con Espacios de almacenamiento directo.

Para obtener más información de compatibilidad de hardware, visite el siguiente sitio web: Catálogo de Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>¿Cómo Espacios de almacenamiento directo usar SES?

Espacios de almacenamiento directo usa la asignación de los servicios de alojamiento SCSI (SES) para asegurarse de que los bloques de datos y los metadatos se reparten entre los dominios de error de un modo resistente. Si el hardware no es compatible con SES, no hay ninguna asignación de los alojamientos y la ubicación de los datos no es resistente.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>¿Qué comando se puede usar para comprobar la extensión física de un disco virtual?
  
Este:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
