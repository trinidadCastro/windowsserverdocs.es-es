---
title: Configurar una estación
description: Obtenga información sobre cómo configurar una estación de Multipoint Services
ms.topic: article
ms.assetid: dce05b6c-795e-43b2-9920-026550b873c5
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: b50022a19ea472f68dad1032a2fe0abeb46ae910
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946918"
---
# <a name="set-up-a-station"></a>Configurar una estación
Una *estación* de MultiPoint Services consta normalmente de un *concentrador de estaciones*, mouse, teclado y monitor de vídeo. En este tema, se describe cómo conectar los dispositivos de hardware al concentrador de estaciones para crear una estación de MultiPoint Services.

El concentrador de estaciones es un dispositivo de hardware que conecta dispositivos periféricos a un equipo en un sistema MultiPoint Services. MultiPoint Services admite dos tipos de concentradores de estación:

-   **Concentrador USB:** un concentrador de expansión USB multipuerto genérico que cumpla con las especificaciones de bus serie universal 2.0 o posteriores. Estos concentradores normalmente tienen dos, cuatro o más puertos USB que permiten que estén conectados varios dispositivos USB a un único puerto USB en el equipo. Los concentradores USB son normalmente dispositivos independientes que pueden estar alimentados externamente o en buses. Cuando se use como un concentrador de estaciones con MultiPoint Services, le recomendamos que use un concentrador con cuatro puertos o más.

    > [!IMPORTANT]
    > Si tiene previsto conectar al concentrador dispositivos USB que no son un teclado y un mouse, le recomendamos que use un concentrador alimentado de forma externa para mejorar el rendimiento.

-   **Concentrador multifunción:** Un concentrador de expansión que se conecta al equipo a través de un puerto USB y permite la conexión de una variedad de dispositivos que no sean USB al concentrador, incluido un monitor de vídeo. Los concentradores multifunción los generan los fabricantes de hardware específicos y pueden requerir la instalación de un controlador específico del dispositivo.

Si quiere agregar una estación a su sistema MultiPoint Services, primero debe asegurarse de que tiene suficientes puertos de conexión disponibles para el hardware de la estación que quiera usar. Además, debe proteger el número adecuado de licencias de *acceso de cliente (cal)* para el sistema Multipoint Services.

## <a name="setting-up-station-hardware"></a>Configuración del hardware de la estación
Los procedimientos de esta sección describen cómo conectar el hardware de la estación de MultiPoint Services a los distintos tipos de concentradores de estaciones.

### <a name="to-set-up-a-station-with-a-usb-hub"></a>Para configurar una estación con un concentrador USB

1.  Para poder conectar una nueva estación *finalice* todas las *sesiones* de usuarios y después apague el equipo y otros dispositivos en el sistema MultiPoint Services.

2.  Conecte el cable del nuevo monitor de vídeo al puerto de la pantalla de vídeo en el equipo, como se muestra en esta imagen:

    ![Imagen de conexión de vídeo en sistema basado en concentrador USB](./media/WMSVideoConnection.gif)

3.  Conecte el nuevo concentrador USB a un puerto USB disponible en el equipo:

    ![Imagen de conexión de concentrador USB de MultiPoint Server](./media/WMSUSBHubConnection.gif)

4.  Conecte un teclado y un mouse al concentrador USB:

    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)

5.  Conecte el cable de alimentación del monitor de vídeo a una toma de corriente.

6.  Encienda el equipo.

7.  Se inicia MultiPoint Services. Siga las instrucciones que aparecen en el monitor de vídeo de la nueva estación para asociar los dispositivos a la nueva estación.

### <a name="to-set-up-a-station-with-a-multifunction-hub"></a>Para configurar una estación con un concentrador multifunción

1.  Para poder conectar una nueva estación finalice todas las sesiones de usuarios y después apague el equipo y otros dispositivos en el sistema MultiPoint Services.

2.  Conecte el cable del nuevo monitor de vídeo al puerto de la pantalla de vídeo DVI o VGA en el concentrador multifunción, como se muestra en esta imagen:

    ![Imagen de conexión de vídeo y concentrador multifunción](./media/WMSMultifunctionHubVideoConnection.gif)

3.  Conecte el nuevo concentrador multifunción a un puerto USB libre en el equipo:

    ![Imagen de una conexión de concentrador multifunción](./media/WMSMultifunctionHubConnection.gif)

4.  Conecte un teclado y un mouse a los conectores PS2 o USB en el concentrador multifunción:

    ![Imagen de conectores PS2 y concentrador multifunción](./media/WMSMultifunctionHubPS2Connection.gif)

5.  Conecte el cable de alimentación del monitor de vídeo a una toma de corriente.

6.  Encienda el equipo.

7.  Se inicia MultiPoint Services. Si se le solicita, siga las instrucciones que aparecen en el monitor de vídeo de la nueva estación para *asociar* los dispositivos a la nueva estación.

## <a name="see-also"></a>Consulte también
[Finalizar una sesión](End-a-User-Session.md) 
 de usuario [Reiniciar o apagar](Restart-or-Shut-Down.md) 
 [Administrar el hardware](Manage-Station-Hardware.md) 
 de la estación [Trabajar con dispositivos USB](Work-with-USB-Devices.md)