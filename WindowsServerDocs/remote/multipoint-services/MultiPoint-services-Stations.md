---
title: Estaciones de MultiPoint
description: Obtenga información sobre las estaciones en MultiPoint Services, incluidas las distintas opciones para los usuarios
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e747826a7cd84521bc62e48abedf3092bf6d844c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855656"
---
# <a name="multipoint--stations"></a>Las estaciones de multiPoint
En un entorno de sistema MultiPoint Services, *estaciones* son los puntos de conexión de usuario para conectarse al equipo que ejecuta MultiPoint Services. Cada estación proporciona al usuario una experiencia independiente de Windows 10. Se admiten los siguientes tipos de estación:  
  
-   Estaciones de vídeo directamente conectados  
  
-   Estaciones conectados por USB cero-client (incluidos a los clientes de USB a través de Ethernet cero)  
  
-   Estaciones de RDP-over-LAN-conectado (para cliente enriquecido o equipos de cliente ligero)  
  
Completos equipos que tengan instalado el conector de MultiPoint también se pueden supervisar y controlan mediante MultiPoint Dashboard. En Windows 10 se puede habilitar el conector de MultiPoint mediante el panel de control para las características de Windows. 

Multipoint Services es compatible con cualquier combinación de estos tipos de estación, pero se recomienda que una estación sea una estación de vídeo directamente conectados, que puede servir como la estación principal. El motivo de esta recomendación es poder anticiparse a los escenarios de soporte técnico. Por ejemplo interactuar con el sistema de BIOS antes de que se está ejecutando MultiPoint Services.  
  
## <a name="primary-stations-and-standard-stations"></a>Estaciones principales y las estaciones estándares  
Una estación de vídeo directamente conectados se define como el *estación principal*. Las estaciones restantes se conocen como *estaciones estándares*.  
  
La estación principal muestra las pantallas de inicio cuando el equipo está encendido. Proporciona acceso a la configuración del sistema y solución de problemas de información que solo está disponible durante el inicio. La estación principal debe ser una estación de vídeo directamente conectados. Después de iniciarse, puede usar la estación principal como cualquier otra estación MultiPoint.  
  
## <a name="direct-video-connected-stations"></a>Estaciones de vídeo directamente conectados  
El equipo que ejecuta MultiPoint Services puede contener varias tarjetas de vídeo, cada uno de los cuales puede tener uno o varios puertos de vídeo. Esto le permite conectar a varias estaciones supervisa directamente en el equipo. Teclados y mouse está conectados a través de los concentradores USB que están asociados con cada monitor. Estos centros se conocen como *concentradores de estación*. Otros dispositivos periféricos, como dispositivos de almacenamiento USB, auriculares o altavoces también se pueden conectar a un concentrador de estaciones, y están disponibles solo para el usuario de esa estación.  
  
> [!IMPORTANT]  
> Debe haber al menos un *vídeo directamente conectados estación* por servidor para que actúe como la estación principal para mostrar el proceso de inicio cuando el equipo está encendido.  
  
![Imagen del diseño del sistema basado en USB de MultiPoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figura 1** MultiPoint services del sistema con cuatro estaciones vídeo directamente conectados  
  
### <a name="BKMK_PS2stations"></a>Estaciones de PS/2  
Con MultiPoint Services, puede asignar el teclado PS/2 y el mouse en la placa base a un monitor conectado vídeo directo para crear una estación de PS/2. Audio analógico de alta definición en la placa base es el audio asociado a este tipo de estación. Esto no es aplicable a los equipos donde no hay ningún conectores PS/2 en la placa base.  
  
## <a name="usb-zero-client-connected-stations"></a>Estaciones conectados por USB cero-client  
Usan estaciones conectados por USB cero-client un *zero client mediante USB* como un concentrador de estaciones. Los clientes USB cero a veces se conocen como un concentrador multifunción con vídeo. Son un concentrador que se conecta al equipo mediante un cable USB, y estos concentradores normalmente admiten un vídeo monitor, un ratón y teclado (PS/2 o USB), audio y otros dispositivos USB. Esta guía hace referencia a estos centros especializados como USB cero los clientes.  
  
El siguiente diagrama muestra un sistema de MultiPoint server con una estación principal (estación conectada a vídeo directo) y dos adicionales zero client mediante USB estaciones conectadas.  
  
![Estaciones conectadas de USB cero-cliente](./media/WMS11_diagram7.gif)  
  
**Figura 2** sistema MultiPoint Services con una estación principal y dos estaciones USB cero cliente conectado  
  
### <a name="usb-over-ethernet-zero-clients"></a>Clientes de USB a través de Ethernet cero  
Los clientes de USB a través de Ethernet cero son una variación de los clientes USB cero que envían USB a través de LAN para el sistema MultiPoint Services. Estos tipos de clientes USB cero funcionan de forma similar a otro USB cero los clientes, pero no están limitados por los valores máximos de la longitud del cable USB. Los clientes de USB a través de Ethernet cero no son clientes ligeros tradicionales, y aparecen como virtuales dispositivos USB en el sistema MultiPoint Services. Al usar estos dispositivos, consulte al fabricante del dispositivo para el rendimiento específico y las recomendaciones de planeación de sitio. La mayoría de los dispositivos tiene un complemento de terceros para MultiPoint Manager que le permite asociar y conectar dispositivos al sistema MultiPoint Services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Estaciones conectadas de RDP a través de LAN  
Clientes ligeros y tradicionales de escritorio, portátil o tablet PC, puede conectarse al equipo que ejecuta MultiPoint Services a través de la red de área local (LAN) mediante el protocolo de escritorio remoto (RDP) o un protocolo propietario y el protocolo de escritorio remoto Proveedor. Las conexiones RDP proporcionan una experiencia de usuario final que es muy similar a cualquier estación de MultiPoint, pero hace uso de hardware del equipo cliente local. Más información sobre nuestras aplicaciones de escritorio remotos disponibles para Android, iOS, Mac y Windows en [clientes de escritorio remoto](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
Los clientes y dispositivos que ejecutan Microsoft RemoteFX pueden proporcionar una experiencia multimedia enriquecida aprovechando las ventajas de las capacidades de hardware de procesador y vídeo del cliente ligero local o del equipo para proporcionar vídeo de alta definición a través de la red.  
  
Si tiene clientes de LAN existentes MultiPoint Services puede proporcionar una manera rápida y rentable para todos los usuarios actualizar simultáneamente a una experiencia de Windows 10.  
  
Desde una perspectiva de implementación y administración, existen las siguientes diferencias al usar estaciones RDP-over-LAN-conectada:  
  
-   No se limita a distancias de conexión USB físicas  
  
-   Posibilidad de volver a usar hardware informático más antiguo, como las estaciones  
  
-   Más sencillo escalar a un número mayor de estaciones. Cualquier cliente de la red puede emplearse como estación remota  
  
-   No hay hardware de solución de problemas a través de la consola del Administrador de MultiPoint  
  
-   Ninguna funcionalidad de pantalla dividida.  
  
    Para obtener más información, consulte [estaciones de pantalla dividida](#a-namebkmksplitscreenstationsasplit-screen-stations) más adelante en este tema.  
  
-   Ningún cambio de nombre de estación o configurar inicio de sesión automático a través de la consola del Administrador de MultiPoint  
  
![Estación conectada a Zero Client mediante USB](./media/Diagram1.gif)  
  
**Figura 3** sistema MultiPoint Services con estaciones de RDP-over-LAN-conectado  
  
## <a name="additional-configuration-options"></a>Opciones de configuración adicionales  
  
### <a name="BKMK_SplitscreenStations"></a>Estaciones de pantalla dividida  
MultiPoint Services ofrece una opción de pantalla dividida en equipos con estaciones de vídeo directamente conectados o conectados por USB cero-client estaciones. Una pantalla dividida proporciona la capacidad para crear una estación adicional por cada monitor. En lugar de requerir dos monitores, puede usar a un monitor con dos configuraciones de concentrador de estaciones para crear dos estaciones con un monitor. Puede aumentar rápidamente el número de las emisoras disponibles sin necesidad de adquirir monitores adicionales, los clientes de USB de cero o tarjetas de vídeo.  
  
Las ventajas del uso de una estación de pantalla dividida pueden incluir:  
  
-   Reducir el costo y el espacio al albergar a más usuarios en un sistema MultiPoint Services.  
  
-   Permite que dos usuarios colaborar side-by-side en un proyecto.  
  
-   Permitir al profesor demostrar un procedimiento en una estación mientras un estudiante sigue la explicación en otra estación.  
  
Los servicios de MultiPoint estación a monitor con una resolución de 1024 x 768 o posterior pueden dividirse en dos pantallas de la estación. Para la mejor experiencia de usuario de pantalla dividida, se recomienda una pantalla panorámica con una resolución de 1600 x 900 mínima. También se recomienda un teclado mini sin un teclado numérico para permitir que los teclados para ajustarse a delante el monitor de dos.  
  
Para crear estaciones de pantalla dividida, configurar una estación de vídeo directamente conectados o conectados por USB cero-client. A continuación, agregar un concentrador de estaciones adicionales con conectar un teclado y mouse a un concentrador USB que está conectado al servidor. A continuación, puede convertir la estación en dos estaciones mediante MultiPoint Manager para dividir la pantalla y el nuevo concentrador se asignan a la mitad del monitor. La mitad izquierda de la pantalla se convierte en una estación y la mitad derecha se convierte en una segunda estación.  
  
Una vez haya dividido la estación, un usuario puede iniciar sesión en la estación izquierda mientras otro usuario inicia sesión en la estación derecha.  
  
![Estaciones con pantalla dividida](./media/WMS_diagram3.gif)  
  
**Figura 4** sistema MultiPoint Services con estaciones de pantalla dividida  
  
## <a name="BKMK_StationTypeComparison"></a>Comparación de tipo de estación  
  
||Vídeo directo conectado|Zero Client mediante USB conectado|RDP a través de LAN conectados|  
|-|--------------------------|-----------------------------|----------------------------|  
|Rendimiento de vídeo|Se recomienda para obtener el mejor rendimiento de vídeo||Usar a clientes ligeros que admiten RemoteFX de calidad de vídeo al ancho de banda de red inferior|  
|Limitaciones físicas|Limitado por la duración del cable de vídeo y concentrador USB y cable de longitud (15 recomienda medidor de la longitud máxima)|Limitado por el concentrador USB y la longitud de cable (longitud máxima de 15 recomienda medidor)|Limitado por la distribución de LAN|  
|Número de estaciones permitido |Limitado por el número de ranuras de PCIe disponibles en la placa base veces los puertos de vídeo por tarjeta de vídeo|Número total puede estar limitado por el fabricante del cliente USB cero (para obtener más información, consulte la nota que sigue a esta tabla).|Limitado por los puertos disponibles en el conmutador de red|  
|Pantalla dividida|Sí|Sí|No|  
|Estado periférico de estación de multiPoint Manager, configuración de inicio de sesión automático, cambiar el nombre de la estación|Sí|Sí|No|  
|Acceso a los menús de inicio del servidor|Sí|No|No|  
  
> [!NOTE]  
> El número total de clientes USB cero que están conectados al servidor puede estar limitado por el fabricante o la capacidad de hardware del equipo que ejecuta MultiPoint Services.