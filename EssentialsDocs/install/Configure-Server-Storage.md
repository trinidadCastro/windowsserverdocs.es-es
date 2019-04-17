---
title: Configurar el almacenamiento del servidor
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6de485f6fd46464ba707bc0871f60ac2fec5a1db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-server-storage"></a>Configurar el almacenamiento del servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Configuraciones de disco duro de muestra  
 La siguiente tabla sugiere configuraciones de disco duro de muestra. Las estimaciones basadas en uso típico y funcionalidad, pero no tienen en consideración problemas que afectan al rendimiento óptimo. Puedes usar cualquier tipo de disco duro compatible para estas configuraciones (como SATA o SCSI), en función de las preferencias y necesidades de tu cliente.  
  
> [!IMPORTANT]
>   Windows Server Essentials debe estar instalado como el volumen C: y el tamaño del volumen debe tener al menos 60 GB. Se recomienda que crear dos particiones del disco del sistema operativo y no uses la unidad C: (partición del sistema) para almacenar datos empresariales.  
  
|Nivel de servidor|Configuración de disco|  
|------------------|------------------------|  
|Entrada|-Dos discos físicos<br /><br /> -Configurado como un conjunto reflejado RAID 1 que contiene lo siguiente:<br /><br /> ¿-Volumen C:? 60 GB<br /><br /> ¿-Volumen D:? 1000 GB|  
|Mediano|-Tres discos físicos<br /><br /> -Configurado como un conjunto RAID 5 que contiene lo siguiente:<br /><br /> ¿-Volumen C:? 60 GB<br /><br /> ¿-Volumen D:? 1500 GB|  
|Alto|-Cinco o más discos físicos en total<br /><br /> ¿-Dos discos en un conjunto reflejado RAID 1 que contiene el volumen C:? 100 GB<br /><br /> -Todos los demás discos en un conjunto RAID 5 que contiene lo siguiente:<br /><br /> ¿-Volumen D:? 1500 GB<br /><br /> ¿-Volumen E:? 1500 GB|  
  
 Estas recomendaciones tienen en cuenta el tamaño del sistema operativo instalado, el tamaño medio del almacenamiento de datos que usa el servidor y el crecimiento de almacenamiento de datos que esperan durante el ciclo de vida del servidor. Los volúmenes pueden ser particiones en un único disco físico o pueden encontrarse en discos físicos independientes. Dado que el servidor almacena datos importantes para tu cliente, se recomienda usar varios discos físicos y proteger los datos de s de cliente mediante el uso de RAID de hardware espacios de almacenamiento.  
  
## <a name="configuring-your-server-backup"></a>Configurar la copia de seguridad del servidor  
 Además de los discos duros internos en el servidor, los clientes consideren el uso de discos duros externos USB para las copias de seguridad. En teoría, el cliente tuviera al menos dos discos duros externos con suficiente capacidad para hacer copia de seguridad de todos los datos en el servidor. Si se usan discos duros externos, el cliente puede sacar un disco de la oficina cada día para proteger aún más los datos.  
  
## <a name="partition-configuration"></a>Configuración de la partición  
 Durante la configuración inicial del servidor, se crea un conjunto de carpetas de servidor predeterminadas que se incluyen las carpetas compartidas y la carpeta de copia de seguridad del equipo cliente en la partición de datos más grande en el disco 0.  
  
## <a name="see-also"></a>Consulta también  

 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Tareas iniciales con el ADK de Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

