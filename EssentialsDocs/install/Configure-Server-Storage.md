---
title: Configurar el almacenamiento de servidor
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433747"
---
# <a name="configure-server-storage"></a>Configurar el almacenamiento de servidor

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Configuraciones de disco duro de ejemplo  
 La siguiente tabla sugiere algunas configuraciones de disco duro de ejemplo. Las estimaciones se basan en el uso y las funcionalidades típicos, pero no consideran problemas que afectan el rendimiento óptimo. Puede usar cualquier disco duro compatible que desee para estas configuraciones (por ejemplo, SATA o SCSI) en función de las preferencias y las necesidades del cliente.  
  
> [!IMPORTANT]
>   Windows Server Essentials debe estar instalado como volumen C:, y el tamaño del volumen debe ser al menos 60 GB. Se recomienda crear dos particiones en el disco del sistema operativo, en lugar de utilizar la C: (partición del sistema) para almacenar los datos de la empresa.  
  
|Nivel de servidor|Configuración del disco|  
|------------------|------------------------|  
|Entrada|-Dos discos físicos<br /><br /> -Configurados como conjunto reflejado RAID 1 que contiene lo siguiente:<br /><br /> ¿-Volumen C:? 60 GB<br /><br /> ¿-Volumen D:? 1000 GB|  
|Medio|-Tres discos físicos<br /><br /> -Configurados como conjunto RAID 5 que contiene lo siguiente:<br /><br /> ¿-Volumen C:? 60 GB<br /><br /> ¿-Volumen D:? 1500 GB|  
|Alto|-Cinco o más discos físicos en total<br /><br /> ¿-Dos discos en un conjunto reflejado RAID 1 que contiene el volumen C:? 100 GB<br /><br /> -Todos los demás discos en un conjunto RAID 5 que contiene lo siguiente:<br /><br /> ¿-Volumen D:? 1500 GB<br /><br /> ¿-Volumen de E:? 1500 GB|  
  
 Estas recomendaciones tienen en cuenta el tamaño del sistema operativo instalado, el tamaño promedio del almacenamiento de datos que usa el servidor y el crecimiento del almacenamiento de datos previsto durante toda la vida útil del servidor. Los volúmenes pueden ser particiones de un único disco físico o se pueden incluir en discos físicos separados. Dado que el servidor almacena datos importantes de cara al cliente, se recomienda que use varios discos físicos y ayudar a proteger los datos del cliente s mediante el uso de RAID de hardware o espacios de almacenamiento.  
  
## <a name="configuring-your-server-backup"></a>Configuración de la copia de seguridad del servidor  
 Además de los discos duros internos del servidor, los clientes deben plantearse el uso de discos duros USB externos para realizar copias de seguridad. En una situación ideal, el cliente dispondría de al menos dos discos duros externos con capacidad suficiente para realizar una copia de seguridad de todos los datos del servidor. Si se usan discos duros externos, el cliente puede retirar un disco de las instalaciones todas las noches para ofrecer mayor protección de los datos.  
  
## <a name="partition-configuration"></a>Configuración de partición  
 Durante la configuración inicial del servidor, se creará un conjunto de carpetas de servidor predeterminadas (incluidas carpetas compartidas y la carpeta de copia de seguridad del equipo cliente) en la partición de datos más grande del disco 0.  
  
## <a name="see-also"></a>Vea también  

 [Introducción al ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Introducción al ADK de Windows Server Essentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparar la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

