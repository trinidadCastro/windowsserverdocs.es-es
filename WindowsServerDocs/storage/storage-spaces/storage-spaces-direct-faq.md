---
title: 'Espacios de almacenamiento directo: preguntas más frecuentes'
description: Obtén información sobre cómo aproximadamente espacios de almacenamiento directo
keywords: Espacios de almacenamiento
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066889"
---
# Espacios de almacenamiento directo: preguntas más frecuentes (P+F)

En este artículo se enumera algunas preguntas frecuentes y comunes relacionados con [Espacios de almacenamiento directo](storage-spaces-direct-overview.md).

## ¿Cuando usas espacios de almacenamiento directo con 3 nodos, puede obtener rendimiento y los niveles de capacidad?

Sí, puedes obtener un rendimiento y el nivel de capacidad en una configuración de espacios de almacenamiento directo 2 o 3 nodos. Sin embargo, debe asegurarse de que tienes 2 dispositivos de capacidad. Esto significa que debes usar los tres tipos de dispositivos: NVME, SSD y HDD.
 
## Sistema de archivos de Refs proporciona tiaring en tiempo real con espacios de almacenamiento directo. ¿REFS proporciona la misma funcionalidad con espacios de almacenamiento compartido en 2016?

No, no obtendrá interconexión con espacios de almacenamiento compartido con 2016 en tiempo real. Esto es solo para espacios de almacenamiento directo. 
 
## ¿Puedo usar un sistema de archivos NTFS con espacios de almacenamiento directo?
  
Sí, puedes usar el sistema de archivos NTFS con espacios de almacenamiento directo. Sin embargo, se recomienda REFS. NTFS no proporciona niveles en tiempo real. 
 
## He configurado espacios de almacenamiento directo clústeres de 2 nodos, donde el disco virtual está configurado como la resistencia de reflejo de forma de 2. ¿Si se agrega un nuevo dominio de error, cambiará la resistencia del disco virtual existente?

Después de agregar el nuevo dominio de error, los nuevos discos virtuales que crees se desplazará al reflejo de 3 vías. Sin embargo, el disco virtual existente seguirá siendo un disco de forma de 2 reflejado. Puede copiar los datos a los nuevos discos virtuales de los volúmenes existentes para aprovechar las ventajas de la resistencia nueva.
 
## Espacios de almacenamiento directo se creó con el modificador de autoconfig:0 y el grupo que se crea manualmente. Al intentar consultar el grupo de espacios de almacenamiento directo para crear un nuevo volumen, aparece un mensaje que dice "Enable-ClusterS2D nuevo." ¿Qué debo hacer?

De manera predeterminada, al configurar espacios de almacenamiento directo mediante el cmdlet enable-S2D, el cmdlet hace todo por TI. Crea el grupo y los niveles. Al usar autoconfig:0, todo lo que debe hacerse manualmente. Si has creado solo el grupo, el nivel no es necesario crear. Si no crean capas de en absoluto o no se crean capas de una manera correspondiente a los dispositivos conectados, recibirás un mensaje de error de "Nuevo Enable-ClusterS2D". Te recomendamos que no usas el cambio de configuración automática en un entorno de producción. 
 
## ¿Es posible agregar un disco de giro (HDD) para el grupo de espacios de almacenamiento directo después de haber creado espacios de almacenamiento directo con dispositivos SSD?

No. De manera predeterminada, si usas el tipo de dispositivo único para crear el grupo, no configuran los discos de memoria caché y todos los discos se usarían para capacidad. Puedes agregar los discos de NVME para la configuración y los discos NVME podrían estar configurados para la memoria caché.
 
## He configurado un dominio de error de rack de 2: 1 RACK tiene 2 dominios de error, 2 RACK 1 dominio de error. Cada servidor tiene 4 dispositivos de 100 GB de capacidad. ¿Puedo usar los 1200 GB de espacio del grupo de?

No, puedes usar solo 800 GB. En un dominio de error rack, debe asegurarse de que tienes una configuración de reflejo de 2 vías para cada chuck y su tierra duplicado en un bastidor diferentes.
 
## ¿Cuál debe ser el tamaño de la memoria caché cuando voy a configurar espacios de almacenamiento directo?

La memoria caché debería tener un tamaño para dar cabida al espacio de trabajo (los datos que se va a activamente leen o escriben en cualquier momento determinado) de las aplicaciones y cargas de trabajo.

## ¿Cómo se puede determinar el tamaño de caché que se está usando por espacios de almacenamiento directo?

Usar la utilidad integrada Monitor de rendimiento para inspeccionar la caché. Revisa la memoria caché de lecturas/s miss que desde el contador Cluster Storage Hybrid Disk. Recuerda que si demasiadas lecturas faltan la memoria caché, la memoria caché es insuficiente y es posible que quieras expandirlo. 
 
## ¿Hay una calculadora que se muestra el tamaño exacto de los discos que se está reservado para caché, capacidad y resistencia que me planear mejor permitirían?

Puedes usar la calculadora de espacios de almacenamiento para ayudar con tu planificación. Está disponible en http://aka.ms/s2dcalc.
 
## ¿Qué es la mejor configuración que se recomiendan al configurar 6 servidores y 3 bastidores?

Usa 2 servidores en cada uno de los bastidores para obtener la resistencia de disco virtual de un espejo de 3 vías. Recuerda que la configuración de rack funcionaría correctamente solo si estás proporcionando la configuración en el sistema operativo de la manera en que se coloca en el rack. 
 
## ¿Puedo habilitar el modo de mantenimiento para un disco concreto en un servidor específico en el clúster de espacios de almacenamiento directo?

Sí, puedes habilitar el modo de mantenimiento de almacenamiento en un disco específico y un dominio de error específicos. El comando Enable-StorageMaintenanceMode se invoca automáticamente cuando se pausa un nodo. Puede habilitarla para un determinado disco ejecutando el siguiente comando:

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## ¿Se espacios de almacenamiento directo admite en el hardware?

Te recomendamos que ponerte en contacto con el proveedor del hardware para comprobar la compatibilidad con. Los proveedores de hardware probar la solución en su hardware y el comentario acerca de si se admite o no. Por ejemplo, en el momento de este documento, servidores como R730 / R730xd / R630 que tienen más de 8 ranuras de unidad puede admitir SES y son compatibles con espacios de almacenamiento directo. Dell admite solo la HBA330 con espacios de almacenamiento directo. R620 no admite SES y no es compatible con espacios de almacenamiento directo.

Para obtener más hardware información de soporte técnico, visita el sitio Web siguiente: catálogo de Windows Server
 
## ¿Cómo espacios de almacenamiento directo hace uso de SES?

Espacios de almacenamiento directo usa la asignación de servicios de revestimiento de SCSI (SES) para asegurarte de que losas de datos y los metadatos se reparte entre los dominios de error de forma resistente. Si el hardware no admite SES, no hay ninguna asignación de los contenedores y la colocación de datos no es resistente.
 
## ¿Qué comandos se pueden usar para comprobar la extensión física de un disco virtual?
  
Esta:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
