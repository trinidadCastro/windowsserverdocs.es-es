---
title: Configuración de una estación conectada al cliente USB Zero en Multipoint Services
description: Obtenga información acerca de cómo crear una estación de cliente USB Zero en Multipoint Services
ms.date: 07/22/2016
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: e0db1c5013529e77aa757f086f842a1ad9007868
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953771"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configuración de una estación conectada al cliente USB Zero en Multipoint Services
Cuando se usan clientes USB sin conexión para crear estaciones de Multipoint Services, el monitor de cada estación se conecta al puerto de vídeo del cliente USB cero, tal como se muestra en la siguiente ilustración. Para obtener más información sobre este y otros tipos de estación, consulte [estaciones Multipoint](MultiPoint-services-Stations.md).

**Sistema Multipoint Services con una estación conectada directamente a vídeo y dos estaciones conectadas por el cliente USB**

![Estaciones conectadas a Zero Client mediante USB](./media/WMS11_diagram7.gif)

> [!IMPORTANT]
> Antes de configurar estaciones conectadas en el cliente USB cero, asegúrese de instalar los controladores más recientes para las tarjetas de vídeo y el cliente USB sin conexión. Los controladores obsoletos pueden impedir que la configuración de Multipoint Services se complete correctamente. Para obtener instrucciones, consulte [actualización e instalación de controladores de dispositivos, si es necesario](Update-and-install-device-drivers-if-needed.md).

> [!IMPORTANT]
> Si usa un cliente de cero USB a través de Ethernet, siga las instrucciones de su proveedor, en lugar de hacerlo, para usar la conexión Ethernet con el fin de configurar el dispositivo en la red.

### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Para configurar una estación conectada al cliente USB Zero

1.  Conecte el cable del monitor de vídeo al puerto de pantalla de vídeo DVI o VGA en el cliente USB Zero, tal como se muestra en la siguiente ilustración.

    ![Imagen de conexión de vídeo en sistema basado en concentrador USB](./media/WMSVideoConnection.gif)

2.  Conecte el cliente USB sin conexión a un puerto USB abierto en el equipo.

    ![Imagen de conexión del concentrador USB de Multipoint Services](./media/WMSUSBHubConnection.gif)

3.  Conecte un teclado y un mouse al cliente USB cero.

    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)

4.  Si usa un cliente USB sin tecnología externa, conecte el cable de alimentación del cliente USB sin conexión a una toma de corriente.

5.  Conecte el cable de alimentación del monitor de vídeo a una toma de corriente.

6.  Si se le pide que asocie dispositivos con la estación, siga las instrucciones del monitor para completar la instalación. (Por lo general, las estaciones conectadas al cliente USB no se asocian a las estaciones automáticamente a medida que se agregan al servidor).