---
title: Configurar el equipo físico y la estación principal
description: Aprenda a configurar su primer sistema, la estación principal, en Multipoint Services.
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
ms.openlocfilehash: 01b405b679afa3815652b91bec63c786bd661cf7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871548"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurar el equipo físico y la estación principal
Antes de instalar Multipoint Services, debe configurar la estación principal del sistema Multipoint Services. Si va a utilizar una red de área local (LAN), conecte el equipo a la LAN.  
  
Una *estación* es un punto de conexión por el que se tiene acceso a multipoint Services. La *estación principal* es la primera estación que se inicia cuando se inicia Multipoint Services. Los administradores pueden usarlo para tener acceso a los menús y a la configuración de inicio. La estación principal proporciona acceso a la configuración del sistema y a la información de solución de problemas que solo está disponible durante el inicio y antes de que se ejecute el sistema Multipoint Services. Después del inicio, puede usar la estación principal como lo haría con cualquier otra estación.  
  
La estación principal debe ser una estación conectada directamente a vídeo. En el procedimiento siguiente se describe cómo conectar el hardware necesario al equipo de Multipoint Services.  
  
Para obtener más información acerca de las estaciones, consulte [estaciones Multipoint](multipoint-services-stations.md). Para obtener ayuda con la selección de hardware, consulte [selección de hardware para el sistema Multipoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Para obtener información sobre cómo conectar otros tipos de estaciones a multipoint Services, consulte [conexión de estaciones adicionales al equipo de Multipoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Para crear una estación conectada a vídeo, debe usar un teclado latino (por ejemplo, un teclado en inglés o español).  
  
## <a name="to-set-up-your-primary-station"></a>Para configurar la estación principal  
  
1.  Asegúrese de que el equipo que ejecuta Multipoint Services esté desactivado y desconectado.  
  
2.  Conecte el cable de alimentación del monitor a una toma de corriente y conecte el cable del monitor al puerto de pantalla del vídeo en el equipo, como se muestra a continuación.  
  
    ![Image of Video connection to USB hub-based system](./media/WMSVideoConnection.gif)  
  
3.  Si la estación va a utilizar un teclado y un mouse USB, siga estos pasos:  
  
    1.  Conecte un concentrador USB externo a un puerto USB abierto en el equipo, como se muestra a continuación.  
  
        ![Imagen de conexión del concentrador USB de Multipoint Services](./media/WMSUSBHubConnection.gif)  
  
    2.  Conecte el teclado y el mouse USB al concentrador USB.  
  
        ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Si el equipo de Multipoint Services tiene puertos PS/2, puede, si es necesario, usar un teclado y un Mouse PS/2 conectados directamente al equipo. Sin embargo, esta configuración tiene limitaciones significativas. Los usuarios no pueden usar dispositivos de audio, cámaras Web y unidades Flash en estaciones PS/2.  
  
    3.  Si usa un concentrador alimentado externamente, conecte el cable de alimentación del centro a una toma de corriente.  
  
        > [!IMPORTANT]  
        > Se recomienda encarecidamente el uso de un concentrador controlado. El comportamiento errático del sistema puede deberse a condiciones de uso inferior.  
        >   
        > Los usuarios no deben adjuntar los ratones y los teclados directamente a los puertos USB del equipo. Si lo hace, es probable que se produzca una asociación incorrecta de varios teclados y ratones a la misma estación o que no haya ninguna estación.  
  
        > [!NOTE]  
        > El dispositivo de audio de host de la placa base del sistema solo está disponible mientras Multipoint Services está en modo de consola. Para garantizar un audio ininterrumpido para una estación que use un concentrador USB externo, debe usar un dispositivo de audio USB conectado al centro.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Para conectar el equipo a la LAN  
  
-   Si tiene una LAN, conecte el equipo a la red con un cable de red.