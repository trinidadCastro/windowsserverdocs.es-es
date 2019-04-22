---
title: Configurar el equipo físico y la estación principal
description: Obtenga información sobre cómo configurar el primer sistema, la estación principal en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6569c4963d31b72216943bf29b71411e702caf84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812016"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurar el equipo físico y la estación principal
Antes de instalar MultiPoint Services, deberá configurar la estación principal para su sistema MultiPoint Services. Si va a usar una red de área local (LAN) conecte el equipo a la LAN.  
  
Un *estación* es un punto de conexión por la que se tiene acceso a MultiPoint Services. El *estación principal* es la primera estación para iniciarse cuando se inicia MultiPoint Services. Los administradores pueden usar, configuración y los menús de inicio de access. La estación principal proporciona acceso a la configuración del sistema y solución de problemas de información que solo está disponible durante el inicio y antes de los servicios de MultiPoint sistema se está ejecutando. Después de iniciarse, puede usar la estación principal como lo haría con cualquier otro lugar.  
  
La estación principal debe ser una estación de vídeo directamente conectados. El siguiente procedimiento describe cómo conectar el hardware necesario para el equipo de MultiPoint Services.  
  
Para obtener más información acerca de las estaciones, vea [estaciones de MultiPoint](multipoint-services-stations.md). Para obtener ayuda con la realización de selecciones de hardware, consulte [seleccionar Hardware para su sistema de MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Para obtener información acerca de cómo conectar otra estaciones tipos a MultiPoint Services, consulte [adjuntar emisoras adicionales para el equipo de MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Para crear una estación de vídeo conectado, debe usar un teclado latino (por ejemplo, un teclado inglés o español).  
  
## <a name="to-set-up-your-primary-station"></a>Para configurar la estación principal  
  
1.  Asegúrese de que el equipo que ejecuta MultiPoint Services se ha desactivado y desconectado.  
  
2.  Conecte el cable de alimentación del monitor a una toma de corriente y conecte el cable del monitor al puerto de vídeo en el equipo, tal como se muestra a continuación.  
  
    ![Image of Video connection to USB hub-based system](./media/WMSVideoConnection.gif)  
  
3.  Si va a usar la estación de un teclado USB y un mouse, complete los pasos siguientes:  
  
    1.  Conectar un concentrador USB externo a un puerto USB abierto en el equipo, como se muestra a continuación.  
  
        ![Imagen de conexión de concentrador USB de MultiPoint Services](./media/WMSUSBHubConnection.gif)  
  
    2.  Conecte el teclado USB y un mouse al concentrador USB.  
  
        ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Si el equipo de MultiPoint Services tiene puertos PS/2, puede, si es necesario, usar un teclado PS/2 y un mouse conectados directamente al equipo. Sin embargo, esta configuración tiene limitaciones importantes. Los usuarios no pueden usar dispositivos de audio, cámaras web y las unidades en las estaciones de PS/2 flash.  
  
    3.  Si usas un concentrador alimentado de forma externa, conecte el cable de alimentación del concentrador a una toma de corriente.  
  
        > [!IMPORTANT]  
        > Se recomienda encarecidamente el uso de un concentrador alimentado. Comportamiento del sistema errático puede deberse a condiciones debajo del actual.  
        >   
        > Los usuarios no deben adjuntar teclados y ratones directamente a los puertos USB del equipo. Si lo hace, es probable que provoque la asociación de varios teclados y mouse a la misma estación o a ninguna estación incorrecta en absoluto.  
  
        > [!NOTE]  
        > El dispositivo de audio de host en la placa base del sistema sólo está disponible mientras MultiPoint Services está en modo de consola. Para asegurarse de audio sin interrupciones para una estación que utiliza un concentrador USB externo, debe usar un dispositivo de audio USB conectado al concentrador.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Para conectar el equipo a la LAN  
  
-   Si tiene una LAN, conectar el equipo a la red con un cable de red.