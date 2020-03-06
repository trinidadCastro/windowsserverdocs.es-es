---
title: Requisitos de hardware y recomendaciones de rendimiento
description: Proporciona los requisitos de hardware y rendimiento, así como recomendaciones para Multipoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 284131028b308ee86389f25102d934390ba2f16d
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371881"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Requisitos de hardware y recomendaciones de rendimiento
En este tema se describe el hardware necesario para ejecutar un sistema Multipoint Services y escenarios de aplicaciones de usuario de soporte técnico. El escenario de usuario afecta directamente a los requisitos de CPU, RAM y ancho de banda de red.  

## <a name="optimize-multipoint-services-system-performance"></a>Optimizar el rendimiento del sistema Multipoint Services  
El rendimiento del sistema Multipoint Services se verá afectado directamente por la capacidad de la CPU, la GPU y la cantidad de RAM disponible en el equipo que ejecuta Multipoint Services.  
  
### <a name="applications-and-internet-content"></a>Aplicaciones y contenido de Internet  
Como Multipoint Services es una solución de computación de recursos compartidos, el tipo y el número de aplicaciones que se ejecutan en las estaciones pueden afectar al rendimiento del sistema Multipoint Services. Es importante tener en cuenta los tipos de programas que se usan con frecuencia cuando se planea el sistema. Por ejemplo, una aplicación que usa muchos gráficos requiere un equipo más eficaz que una aplicación como un procesador de textos. La sobrecarga del equipo con aplicaciones con uso intensivo de gráficos probablemente provocará problemas de retraso en todo el sistema.  
  
El tipo de contenido al que tienen acceso las aplicaciones también afecta al rendimiento del sistema. Si hay varias estaciones usando exploradores Web para tener acceso al contenido multimedia, como un vídeo de movimiento completo, se pueden conectar menos estaciones antes de afectar negativamente al rendimiento del sistema. Por el contrario, si las distintas estaciones usan exploradores Web para tener acceso al contenido Web estático, se pueden conectar más estaciones sin un efecto significativo en el rendimiento.  
  
### <a name="hardware-recommendations"></a>Recomendaciones de hardware  
Para lograr un buen rendimiento con el sistema Multipoint Services en varias cargas, siga las directrices de la tabla siguiente al planear y probar el sistema. Estos son los requisitos básicos forMultiPoint Services. El tamaño real de la configuración depende de la configuración del sistema, la carga de trabajo que se está ejecutando y la capacidad de hardware. Siempre debe comprobar las aplicaciones y el hardware.  
  
> [!NOTE]  
> 2C = 2 núcleos, 4C = 4 núcleos, 6C = 6 núcleos, MT = multithreading. La velocidad del procesador debe ser de al menos 2,0 gigahercio (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Hardware mínimo recomendado para ejecutar estaciones de MultiPoint Server predeterminadas  
  
|Escenario de aplicación|Hasta 5 estaciones|6-8 estaciones|9-12 estaciones|13-16 estaciones|17-20 estaciones|21-24 estaciones|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Aumenta**<br /><br />Office, exploración Web, aplicaciones de línea de negocio|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C<br /><br />RAM: 8 GB|CPU: 4C + MT o 6C<br /><br />RAM: 10 GB| CPU: 6C + MT<br /><br />RAM: 12 GB|
|**Combinados**<br /><br />Office, exploración Web, aplicaciones de línea de negocio y uso de vídeo ocasional por parte de algunos usuarios|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C + MT o 6C<br /><br />RAM: 8 GB|CPU: 6C + MT<br /><br />RAM: 10 GB| CPU: 6C + MT<br /><br />RAM: 12 GB| 
|**Uso intensivo de vídeo**<br /><br />Office, exploración Web, aplicaciones de línea de negocio y uso frecuente de vídeo por todos los usuarios **Nota:** las pruebas de vídeo se realizaron con vídeo 360p H. 264 en la resolución nativa.|CPU: 4C + MT<br /><br />RAM: 2 GB|CPU: 6C + MT<br /><br />RAM: 4 GB|CPU: 8C + MT<br /><br />RAM: 6 GB|CPU: 12C + MT<br /><br />RAM: 8 GB|CPU: 16C + MT<br /><br />RAM: 10 GB<br /><br />-Cliente ligero: RemoteFX<br />-Vídeo USB no recomendado| CPU: 20C + MT<br /><br />RAM: 12 GB<br /><br />-Cliente ligero: RemoteFX<br />-Vídeo USB no recomendado|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Hardware mínimo recomendado para ejecutar escritorios virtuales de Windows 10 completos  
Ejecutar una instancia completa del sistema operativo virtual para cada estación es más intensivo en los recursos de proceso que la ejecución de las sesiones de Multipoint Desktop predeterminadas, por lo que los requisitos de hardware del host por estación son mayores:  
  
1.  CPU: 1 núcleo o subproceso por estación  
  
2.  Unidad de estado sólido (SSD)  
  
    1.  Capacidad > = 20 GB por estación + 40 GB para el sistema operativo host de WMS  
  
    2.  IOPS de lectura/escritura aleatoria > = 3K por estación  
  
3.  RAM > = 2 GB por estación + 2 GB para el sistema operativo host de WMS  
  
La configuración de CPU de BIOS se ha configurado para habilitar la virtualización: traducción de direcciones de segundo nivel (SLAT)  
  
Para obtener más información acerca de cómo elegir el mejor hardware de Multipoint Services para sus necesidades, póngase en contacto con su proveedor de hardware.  