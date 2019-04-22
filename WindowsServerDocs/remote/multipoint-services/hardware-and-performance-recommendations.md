---
title: Requisitos de hardware y recomendaciones de rendimiento
description: Proporciona los requisitos de hardware y el rendimiento y recomendaciones de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b47f4c224d4a4f6f4aa104b6d5ee5d93590ac0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823396"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Requisitos de hardware y recomendaciones de rendimiento
Este tema describe el hardware necesario para ejecutar un sistema MultiPoint Services y admitir escenarios de aplicación de usuario. El escenario de usuario afecta directamente a la CPU, RAM y requisitos de ancho de banda de red.  

## <a name="optimize-multipoint-services-system-performance"></a>Optimizar el rendimiento del sistema MultiPoint Services  
El rendimiento del sistema MultiPoint Services se verá afectado directamente por la capacidad de la CPU, la GPU y la cantidad de RAM que está disponible en el equipo que está ejecutando MultiPoint Services.  
  
### <a name="applications-and-internet-content"></a>Las aplicaciones y el contenido de Internet  
Dado que MultiPoint Services es una solución informática de recurso compartido, el tipo y número de aplicaciones que se ejecutan en las estaciones pueden afectar al rendimiento del sistema MultiPoint Services. Es importante tener en cuenta los tipos de programas que se usan con regularidad si tiene previsto del sistema. Por ejemplo, una aplicación de gráficos requiere un equipo más eficaz que una aplicación como un procesador de textos. Sobrecargar el equipo con las aplicaciones de gráficos probablemente causará problemas de retraso en todo el sistema.  
  
El tipo de contenido que se tiene acceso a las aplicaciones también afecta al rendimiento del sistema. Si varias estaciones están utilizando los exploradores web para tener acceso a contenido multimedia, como vídeo y en movimiento, se pueden conectar estaciones menos antes afecta negativamente al rendimiento del sistema. Por el contrario, si el varias estaciones están utilizando los exploradores web para tener acceso a contenido web estático, se pueden conectar las estaciones más sin un efecto significativo en el rendimiento.  
  
### <a name="hardware-recommendations"></a>Recomendaciones de hardware  
Para lograr un buen rendimiento con el sistema MultiPoint Services con distintas cargas, utilice las instrucciones en la tabla siguiente cuando esté planeando y probar el sistema. Estos son los requisitos básicos de servicios de forMultiPoint. El tamaño de la configuración real depende de la configuración del sistema, la carga de trabajo que se está ejecutando y la capacidad de hardware. Siempre debe validar por probar sus aplicaciones y hardware.  
  
> [!NOTE]  
> C = 2 de 2 núcleos, 4, C = 4 núcleos, C = 6 de 6 núcleos, MT = multithreading. Velocidad del procesador debe ser al menos 2,0 gigahercios (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Mínimo recomendado de hardware para ejecutar las estaciones de MultiPoint Server de forma predeterminada  
  
|Escenario de aplicación|Hasta 5 estaciones|Estaciones de 6 a 8|9-12 estaciones|13-16 estaciones|17-20 estaciones|Estaciones de 21 a 24|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Productividad**<br /><br />Office, aplicaciones web de exploración, línea de negocio|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C<br /><br />RAM: 8 GB|CPU: C + MT 4 o 6C<br /><br />RAM: 10 GB| CPU: 6 DE C + MT<br /><br />RAM: 12 GB|
|**Mixto**<br /><br />Office, exploración web, aplicaciones de línea de negocio y uso ocasional de vídeo por algunos usuarios|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: C + MT 4 o 6C<br /><br />RAM: 8 GB|CPU: 6 DE C + MT<br /><br />RAM: 10 GB| CPU: 6 DE C + MT<br /><br />RAM: 12 GB| 
|**Vídeo intensiva**<br /><br />Office, exploración web, aplicaciones de línea de negocio y uso de vídeo frecuente por todos los usuarios **Nota:** Vídeo de prueba se realizó con vídeo H.264 de 360p en resolución nativa.|CPU: 4C + MT<br /><br />RAM: 2 GB|CPU: 6 DE C + MT<br /><br />RAM: 4 GB|CPU: 8C + MT<br /><br />RAM: 6 GB|CPU: 12C + MT<br /><br />RAM: 8 GB|CPU: 16C + MT<br /><br />RAM: 10 GB<br /><br />-Cliente ligero: RemoteFX<br />-Vídeo USB no recomendado| CPU: 20C + MT<br /><br />RAM: 12 GB<br /><br />-Cliente ligero: RemoteFX<br />-Vídeo USB no recomendado|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Mínimo recomendado de hardware para la ejecución completa de escritorios virtuales de Windows 10  
Ejecuta una instancia de sistema operativo virtual completa para cada estación es que más intensivo de recursos de proceso que la ejecución de las sesiones de escritorio multipunto de forma predeterminada, por lo que los requisitos de hardware de host por estación son mayores:  
  
1.  CPU: 1 núcleo o subproceso a cada estación  
  
2.  Unidades de estado sólido (SSD)  
  
    1.  Capacidad de > = 20GB cada estación + 40GB para el WMS del sistema operativo de hosts  
  
    2.  IOPS de lectura/escritura aleatoria > = 3K cada estación  
  
3.  RAM > = 2GB por estación + 2GB para el WMS del sistema operativo de hosts  
  
Configuración de la CPU de BIOS se ha configurado para habilitar la virtualización: traducción de direcciones de segundo nivel (SLAT)  
  
Para obtener más información acerca de cómo elegir el mejor hardware de MultiPoint Services para sus necesidades, póngase en contacto con el fabricante del hardware.  