---
title: Estaciones de MultiPoint
description: Más información sobre las estaciones en Multipoint Services, incluidas las diferentes opciones para los usuarios
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
ms.openlocfilehash: 4aa08f58f8fdf6d6fce816ee090275b0bf46a844
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871629"
---
# <a name="multipoint--stations"></a>Estaciones Multipoint
En un entorno del sistema Multipoint Services, las *estaciones* son los puntos de conexión de usuario para conectarse al equipo que ejecuta Multipoint Services. Cada estación proporciona al usuario una experiencia independiente de Windows 10. Se admiten los siguientes tipos de estación:  
  
-   Estaciones conectadas a vídeo directo  
  
-   Estaciones conectadas por el cliente USB a cero (incluidos los clientes de USB a través de Ethernet)  
  
-   Estaciones conectadas mediante RDP a través de LAN (para equipos cliente enriquecidos o clientes ligeros)  
  
Los equipos completos que tienen instalado Multipoint Connector también se pueden supervisar y controlar mediante Multipoint Dashboard. En Windows 10, el conector Multipoint se puede habilitar a través del panel de control para las características de Windows. 

Multipoint Services admite cualquier combinación de estos tipos de estaciones, pero se recomienda que una estación sea una estación conectada directamente a vídeo, que puede servir como estación principal. La razón de esta recomendación es poder anticiparse a los escenarios de soporte técnico. Por ejemplo, para interactuar con el BIOS del sistema antes de que se ejecute Multipoint Services.  
  
## <a name="primary-stations-and-standard-stations"></a>Estaciones principales y estaciones estándar  
Una estación conectada de vídeo directo se define como la *estación principal*. Las estaciones restantes se conocen como *estaciones estándar*.  
  
La estación primaria muestra las pantallas de inicio cuando el equipo está encendido. Proporciona acceso a la configuración del sistema y a la información de solución de problemas que solo está disponible durante el inicio. La estación principal debe ser una estación conectada directamente a vídeo. Después del inicio, puede usar la estación principal como cualquier otra estación multipoint.  
  
## <a name="direct-video-connected-stations"></a>Estaciones conectadas a vídeo directo  
El equipo que ejecuta Multipoint Services puede contener varias tarjetas de vídeo, cada una de las cuales puede tener uno o más puertos de vídeo. Esto le permite conectar monitores para varias estaciones directamente en el equipo. Los teclados y los ratones están conectados a través de concentradores USB que están asociados a cada monitor. Estos concentradores se conocen como *concentradores de estaciones*. Otros dispositivos periféricos, como altavoces, auriculares o dispositivos de almacenamiento USB, también se pueden conectar a un concentrador de estaciones y solo están disponibles para el usuario de esa estación.  
  
> [!IMPORTANT]  
> Debe haber al menos una *estación conectada de vídeo directa* por cada servidor para que actúe como la estación principal para mostrar el proceso de inicio cuando el equipo esté encendido.  
  
![Imagen del diseño del sistema basado en USB de Multipoint Services](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figura 1** Sistema Multipoint Services con cuatro estaciones conectadas a vídeo directo  
  
### <a name="BKMK_PS2stations"></a>Estaciones PS/2  
Con Multipoint Services, puede asignar el teclado y el mouse de PS/2 en la placa base a un monitor conectado a vídeo directo para crear una estación PS/2. El audio analógico de alta definición en la placa base es el audio asociado a este tipo de estación. Esto no se aplica a los equipos en los que no hay conectores PS/2 en la placa base.  
  
## <a name="usb-zero-client-connected-stations"></a>Estaciones conectadas por el cliente USB a cero  
USB: las estaciones conectadas por el cliente usan un *cliente USB sin* conexión como concentrador de estaciones. Los clientes USB a veces se denominan concentrador multifunción con vídeo. Son un centro que se conecta al equipo mediante un cable USB, y estos concentradores suelen ser compatibles con un monitor de vídeo, un mouse y un teclado (PS/2 o USB), audio y otros dispositivos USB. En esta guía se hace referencia a estos centros especializados como clientes USB de cero.  
  
En el diagrama siguiente se muestra un sistema MultiPoint Server con una estación principal (estación conectada de vídeo directo) y dos estaciones conectadas de cliente USB adicionales.  
  
![Estaciones conectadas de cliente USB sin conexión](./media/WMS11_diagram7.gif)  
  
**Ilustración 2** Sistema Multipoint Services con una estación principal y dos estaciones USB conectadas a un solo cliente  
  
### <a name="usb-over-ethernet-zero-clients"></a>Clientes de USB a través de Ethernet  
Los clientes USB a través de Ethernet son una variación de los clientes USB que envían USB a través de LAN al sistema Multipoint Services. Estos tipos de clientes USB no funcionan de forma similar a otros clientes USB, pero no están limitados por la longitud máxima del cable USB. Los clientes USB a través de Ethernet no son clientes ligeros tradicionales y aparecen como dispositivos USB virtuales en el sistema Multipoint Services. Al usar estos dispositivos, consulte al fabricante del dispositivo para obtener recomendaciones específicas sobre el rendimiento y el planeamiento del sitio. La mayoría de los dispositivos tienen un complemento de terceros para Multipoint Manager que le permite asociar y conectar dispositivos al sistema Multipoint Services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Estaciones conectadas de RDP a través de LAN  
Los clientes ligeros y equipos de escritorio, portátiles o Tablet PC tradicionales pueden conectarse al equipo que ejecuta Multipoint Services a través de la red de área local (LAN) mediante Protocolo de escritorio remoto (RDP) o un protocolo propietario y el Protocolo de escritorio remoto Presta. Las conexiones RDP proporcionan una experiencia de usuario final muy similar a cualquier otra estación Multipoint, pero hace uso del hardware del equipo cliente local. Obtenga más información sobre las aplicaciones de escritorio remoto disponibles para Android, iOS, Mac y Windows en [clientes de escritorio remoto](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
Los clientes y dispositivos que ejecutan Microsoft RemoteFX pueden proporcionar una experiencia multimedia enriquecida aprovechando las capacidades de hardware de procesador y vídeo del equipo o cliente ligero local para proporcionar vídeo de alta definición a través de la red.  
  
Si tiene clientes LAN existentes, Multipoint Services puede proporcionar una manera rápida y rentable de actualizar simultáneamente a todos los usuarios a una experiencia con Windows 10.  
  
Desde la perspectiva de la implementación y la administración, existen las siguientes diferencias cuando se usan estaciones conectadas a través de LAN con RDP:  
  
-   No se limita a las distancias de conexión USB física  
  
-   Posibilidad de reutilizar el hardware del equipo anterior como estaciones  
  
-   Es más fácil escalar a un número mayor de estaciones. Cualquier cliente de la red puede usarse potencialmente como una estación remota  
  
-   No hay solución de problemas de hardware a través de la consola de Multipoint Manager  
  
-   Sin funcionalidad de pantalla dividida.  
  
    Para obtener más información, vea [estaciones de pantalla dividida](#split-screen-stations) más adelante en este tema.  
  
-   No cambiar el nombre de la estación ni configurar el inicio de sesión automático a través de la consola de Multipoint Manager  
  
![Estación conectada a Zero Client mediante USB](./media/Diagram1.gif)  
  
**Figura 3** Sistema Multipoint Services con estaciones conectadas a través de LAN con RDP  
  
## <a name="additional-configuration-options"></a>Opciones de configuración adicionales  
  
### <a name="split-screen-stations"></a>Estaciones con pantalla dividida  
Multipoint Services ofrece una opción de pantalla dividida en los equipos con estaciones conectadas a vídeo directo o estaciones conectadas por el cliente USB a cero. Una pantalla dividida proporciona la capacidad de crear una estación adicional por monitor. En lugar de requerir dos monitores, puede usar un monitor con dos configuraciones de concentrador de estaciones para crear dos estaciones con un monitor. Puede aumentar rápidamente el número de estaciones disponibles sin necesidad de adquirir monitores adicionales, clientes USB-cero ni tarjetas de vídeo.  
  
Las ventajas de usar una estación de pantalla dividida pueden incluir:  
  
-   Reducir el costo y el espacio al acomodar a más usuarios en un sistema Multipoint Services.  
  
-   Permitir que dos usuarios colaboren en paralelo en un proyecto.  
  
-   Permitir a un profesor demostrar un procedimiento en una estación mientras un estudiante sigue el resto de la estación.  
  
Cualquier monitor de estación de Multipoint Services que tenga una resolución de 1024x768 o superior se puede dividir en dos pantallas de estación. Para obtener la mejor experiencia de usuario de pantalla dividida, se recomienda una pantalla ancha con una resolución de 1600 mínima. También se recomienda usar un mini teclado sin un teclado numérico para permitir que los dos teclados quepan delante del monitor.  
  
Para crear estaciones de pantalla dividida, configure una estación conectada directamente a un cliente o USB conectado. A continuación, agregue un concentrador de estaciones adicional conectando un teclado y un mouse a un concentrador USB que esté conectado al servidor. Después, puede convertir la estación en dos estaciones mediante Multipoint Manager para dividir la pantalla y asignar el nuevo centro a la mitad del monitor. La mitad izquierda de la pantalla se convierte en una estación y la mitad derecha se convierte en una segunda estación.  
  
Una vez que se divide una estación, un usuario puede iniciar sesión en la estación izquierda mientras otro usuario inicia sesión en la estación adecuada.  
  
![Estaciones con pantalla dividida](./media/WMS_diagram3.gif)  
  
**Figura 4** Sistema Multipoint Services con estaciones de pantalla divididas  
  
## <a name="BKMK_StationTypeComparison"></a>Comparación de tipo de estación  
  
||Vídeo directo conectado|Cliente USB sin conexión|RDP a través de LAN conectada|  
|-|--------------------------|-----------------------------|----------------------------|  
|Rendimiento de vídeo|Recomendado para el mejor rendimiento de vídeo||Use clientes ligeros que admitan RemoteFX para mejorar la calidad del vídeo con un ancho de banda de red inferior.|  
|Limitaciones físicas|Limitado por la longitud del cable de vídeo y el concentrador USB y la longitud del cable (se recomienda una longitud máxima de 15 medidor)|Limitado por el concentrador USB y la longitud del cable (se recomienda una longitud máxima de 15 medidor)|Limitado por distribución de LAN|  
|Número de estaciones permitidas |Limitado por el número de ranuras PCIe disponibles en la placa base, los puertos de vídeo por tarjeta de vídeo|El número total puede estar limitado por el fabricante del cliente USB cero (para obtener más información, consulte la nota que sigue a esta tabla).|Limitado por los puertos disponibles en el conmutador de red|  
|Pantalla dividida|Sí|Sí|Sin|  
|Estado de periféricos de la estación de Multipoint Manager, configuración de inicio de sesión automático, cambio de nombre de estación|Sí|Sí|Sin|  
|Acceso a los menús de inicio del servidor|Sí|No|Sin|  
  
> [!NOTE]  
> El número total de clientes USB sin conexión que están conectados al servidor puede estar limitado por el fabricante o la capacidad de hardware del equipo que ejecuta Multipoint Services.
