---
title: Selección del hardware de su sistema de MultiPoint Services
description: Consideraciones de hardware de MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e74961a2-bd38-48ae-b1c0-4b3eff761b4a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 969ab0e97b5456c71a43cc14bd82204481bdcc42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835226"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Selección del hardware de su sistema de MultiPoint Services
Al compilar un sistema MultiPoint Services, debe seleccionar un equipo que cumpla los requisitos del sistema de Windows Server 2016. Si decide qué componentes para seleccionar, tenga en cuenta lo siguiente:  
  
-   El intervalo de precios de destino de la solución completa.  
  
-   Los tipos de escenarios de uso que necesita para el sistema MultiPoint Services, por ejemplo, si los usuarios se ejecuten programas multimedios, con procesamiento de textos o programas de productividad o explorar Internet.  
  
-   Si el escenario tenga las demandas de procesamiento o de memoria grandes.  
  
-   El número de usuarios que podrían estar usando el sistema al mismo tiempo. Si va a tener muchos usuarios en el sistema al mismo tiempo, o los usuarios que usan los programas que usan el sistema, debe planear la capacidad de proceso más para su sistema.  
  
-   El tipo de estaciones. ¿Cuántos puertos USB o puertos de vídeo es necesario?  
  
-   Planes de ampliación futura. ¿Tiene pensado agregar estaciones en el sistema MultiPoint Services en una fecha posterior? ¿Pulsa de red o tendrá suficientes ranuras de tarjeta de vídeo, puertos USB? ¿Cuántos usuarios adicionales necesitará el hardware admitir?  
  
-   Diseño físico. Para obtener más información, consulte [planeación del sitio de MultiPoint Services](MultiPoint-services-Site-Planning.md).  
  
Normalmente, un sistema MultiPoint Services incluye los siguientes componentes:  
  
-   Un equipo que está ejecutando MultiPoint Services, que incluye una CPU, RAM, unidades de disco duro y tarjetas de vídeo.  
  
-   Un monitor, concentrador de estaciones, teclado y mouse (ratón) para cada estación.  
  
-   Dispositivos periféricos opcionales para las estaciones de MultiPoint Services, incluidos los dispositivos de almacenamiento que solo están disponibles para el usuario de la estación, micrófonos, auriculares o altavoces.  
  
-   Los dispositivos periféricos opcionales que están disponibles para todos los usuarios del sistema MultiPoint Services, conectado directamente al equipo host, como impresoras, unidades de disco duro externas y dispositivos de almacenamiento USB.  
  
Use la siguiente información para tomar decisiones de hardware:  
  
-   [Seleccionar una CPU](#selecting-a-cpu)  
-   [Seleccionar los componentes de hardware](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Seleccionar una CPU  
Un sistema MultiPoint Services es un múltiplo\-entorno de usuario, con todos los usuarios conectados a un único equipo host. Esto aumenta el uso de CPU, porque todos los usuarios comparten el mismo equipo. Algunas tareas, como programas multimedia \(por ejemplo, reproductores multimedia o vídeo\-software de edición\), tienen mayor demanda de procesamiento. Por lo tanto, asegúrese de seleccionar una CPU que puede controlar los requisitos de procesamiento para el número de usuarios y los tipos de escenarios de usuario que lo necesitará para admitir.  
  
MultiPoint Services requiere una x64\-en función de CPU y debe cumplir los requisitos del sistema para el equipo, como se describe en [los requisitos de Hardware y las recomendaciones de rendimiento](Hardware-Requirements-and-Performance-Recommendations.md).  
  
Se han probado los siguientes tipos de procesadores para usarse en un sistema MultiPoint Services con gran\-exigen los programas de procesamiento, como programas multimedia:  
  
-   **Dual\-procesador core:** Puede admitir hasta 8 estaciones.  
-   **Cuádruple\-procesador core:** Puede admitir hasta 16 estaciones.
-   **Cuádruple\-núcleo de procesador con multithreading:** Puede admitir hasta 20 estaciones.      
-   **Seis\-núcleo de procesador con multithreading:** Puede admitir hasta 24 estaciones.  
  
Con esta información, seleccione una CPU que cumple los requisitos de procesamiento para el sistema MultiPoint Services. 
> [!NOTE] 
> Si ejecuta aplicaciones con uso intensivo de vídeo de la recomendación es al menos un núcleo cada estación. 
  
## <a name="selecting-hardware-components"></a>Seleccionar los componentes de hardware  
Cuando se está generando un sistema MultiPoint Services, tenga en cuenta los siguientes componentes de hardware que puede que necesite:  
  
-   Hardware de vídeo  
  
-   Hardware de la estación de multiPoint Services  
  
    -   *Concentradores USB*  
  
    -   Clientes USB cero  
  
    -   Teclados y mouse  
  
    -   Monitores  
  
-   Dispositivos periféricos  
  
    -   Dispositivos de audio, como altavoces y auriculares  
  
    -   Micrófonos  
  
    -   Dispositivos de almacenamiento masivo USB  
  
Cuando haya seleccionado los componentes de hardware para su sistema MultiPoint Services, asegúrese de que obtienen current, 64\-controladores para los componentes de bits.  
  
Los temas siguientes proporcionan información detallada para ayudarle a seleccionar los componentes de su sistema MultiPoint Services:  
  
[Seleccionar el hardware de vídeo](#selecting-video-hardware)  
[Seleccione direct\-vídeo\-conectado o USB cero dispositivos de la estación de cliente](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Selección de otros dispositivos periféricos de estación](#selecting-other-station-peripheral-devices)  
[Selección de RDP\-sobre\-LAN\-conectado el hardware de la estación](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Seleccionar dispositivos de audio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Seleccionar el hardware de vídeo
El hardware de vídeo que seleccione debe admitir el número de monitores que necesite para el número de usuarios que desee tener trabajar en estaciones de MultiPoint Services. Además, los diferentes tipos de hardware de vídeo pueden proporcionar una mayor\-soluciones de rendimiento para los gráficos\-programas intensivo, como contenido multimedia.  
  
Seleccione el hardware de vídeo que puede admitir el número máximo de monitores para el tipo de rendimiento que requiere el sistema MultiPoint Services. Asegúrese de que valide el rendimiento del hardware de vídeo que desee asegurarse de que cumple los requisitos de rendimiento.  
  
> [!NOTE]  
> Debe instalar a un controlador de vídeo que permite extender el escritorio a través de varios monitores.  
  
Las opciones de hardware de vídeo incluyen:  
  
-   Tarjetas de vídeo internas que usan un PCI o una interfaz de bus PCIe  
  
-   Controladores de vídeo externos conectados mediante USB  
  
Las secciones siguientes describen las capacidades de cada uno de estos tipos de hardware de vídeo. Puede combinar interno de tarjetas de vídeo y controladores de vídeo externos para crear el sistema que desee.  
  
### <a name="internal-video-cards"></a>Tarjetas de vídeo internas  
Está conectada a una tarjeta de vídeo interna\-en a la placa base en el equipo. La tarjeta de vídeo interna es una solución que puede ayudar al rendimiento de los gráficos\-intensivo programas multimedia. Sin embargo, una tarjeta de vídeo interna requiere una ranura PCI o PCIe disponible para conectar\-en a la placa base. Alto muchos\-tarjetas de vídeo de rendimiento requieren una ranura PCIe, pero hay un número limitado de PCIe ranuras de la placa base. Debe saber qué tipo de ranuras de tarjeta de vídeo están disponibles en el equipo para que se puede adquirir el tipo correcto de las tarjetas de vídeo.  
  
El número de monitores que se pueden conectar a cada tarjeta de vídeo depende de la GPU que se usa en la tarjeta y el número de puertos que admite, que normalmente comprendido entre 2 y 6.  
  
Cuando selecciona interno de tarjetas de vídeo, seleccione las tarjetas de vídeo que admiten el número de monitores requeridos para crear el número deseado de estaciones conectados de vídeo directos. El número máximo de monitores que se admiten es igual al número de tarjetas de vídeo internas que están enchufados\-en a la placa base multiplicada por el número de puertos de monitor en cada una de las tarjetas de vídeo. Por ejemplo, si tuviera dos tarjetas de vídeo internas y cada tarjeta tenía dos puertos de monitor, puede admitir a hasta cuatro monitores.    
  
### <a name="external-video-controllers"></a>Controladores de vídeo externos  
Los clientes USB cero contienen un controlador de vídeo externo para conectar a un monitor con el cliente. El zero client mediante USB puede incluir también las conexiones para auriculares, altavoces, un micrófono u otros dispositivos periféricos.  
  
Seleccione un puerto USB cero cliente si desea habilitar la compatibilidad con monitores adicionales sin tener que abrir el equipo, o si desea admitir las estaciones más que las salidas de vídeo disponibles. Por ejemplo, si anteriormente tenía cuatro monitores conectados\-en tarjetas de vídeo internas y desea agregar dos monitores más, puede conectar\-en dos controladores de vídeo externos al equipo y tener espacio para los dos monitores más. De esta manera, puede combinar a un zero client mediante USB con el controlador de vídeo y no use espacios adicionales de PCI o PCIe en la placa base.  
  
## <a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>Seleccione direct\-vídeo\-conectado o USB cero dispositivos de la estación de cliente  
Una estación de MultiPoint Services consta de un concentrador de estaciones o USB cero cliente con un teclado y mouse conectado\-en y un monitor que está conectado\-en el equipo host o en para un zero client mediante USB. Se pueden conectar otros dispositivos periféricos\-en a la estación de concentrador o USB cero cliente, pero no se necesitan para crear estaciones MultiPoint. En el que se describen estos otros dispositivos periféricos [seleccionar otros dispositivos periféricos estación](#selecting-other-station-peripheral-devices).  
  
Los dispositivos que se seleccione esta opción para crear una estación de MultiPoint Services deben cumplir los requisitos mínimos para que funcione con MultiPoint Services. En este tema se proporcionan detalles sobre los requisitos para los siguientes dispositivos de la estación de MultiPoint Services:  
  
-   [Selección de concentradores USB](#selecting-usb-hubs)  
-   [Seleccionar USB cero clientes](#selecting-usb-zero-clients)  
-   [Seleccionar dispositivos de mouse y teclados](#selecting-keyboards-and-mouse-devices)  
-   [Selección de monitores](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>Selección de concentradores USB  
Los concentradores USB que se usan en un sistema MultiPoint Services pueden ser un concentrador USB genérico. Estos concentradores normalmente tienen cuatro o más puertos USB, y permiten varios dispositivos USB se conecten a un único puerto USB en el equipo. Otros dispositivos, como teclados y monitores de vídeo, también pueden incorporar un concentrador USB en su diseño.  
  
Una consideración adicional es el uso de un *alimentar* concentrador, en lugar de un *bus\-con tecnología* concentrador. Con un bus\-concentrador, la cantidad de energía que se proporciona por el host del equipo debe ser suficiente para proporcionar potencia a todos los dispositivos periféricos que están conectados\-al concentrador, sin degradar el rendimiento del sistema. Un concentrador alimentado de forma externa permite conectar dispositivos periféricos más y proporcionar suficiente energía a todos ellos. El uso de concentradores alimentados de forma externa puede ayudar a evitar problemas de rendimiento, errores de puerto y otros problemas intermitentes.  
  
Al seleccionar un concentrador USB para el sistema MultiPoint Services, considere la posibilidad de su uso. El concentrador puede usarse como un *concentrador de estaciones*, un *concentrador intermedio*, o un *concentrador indirecto*. Consulte la tabla siguiente para obtener descripciones sobre cada tipo de concentrador. Se recomienda que todos los dispositivos USB como USB 2.0 o posterior.
  
||Con la tecnología|  
|-|-----------|  
|Concentrador de estaciones|Puede ser bus\-con la tecnología a menos que alta\-dispositivos se incluirse\-en un centro de nivel inferior o se conectará a él|  
|Centro de notificaciones intermedio |Debe alimentar|  
|Centro de nivel inferior|Bus apagados dependiendo de los dispositivos que están conectados o puede alimentar\-al concentrador|  
|Cable USB Active del extensor|Cables USB activos que incluyen un concentrador USB son normalmente bus con la tecnología; por lo tanto, no se recomiendan para conectar concentradores de estaciones en el equipo.|  
  
### <a name="selecting-usb-zero-clients"></a>Seleccionar USB cero clientes  
Un cliente USB cero es un concentrador USB que contiene una salida de vídeo. Por lo tanto, permite un monitor de estar conectado al equipo a través de una conexión USB. Para obtener más información sobre el uso de los clientes USB cero para el vídeo, consulte [seleccionar hardware de vídeo](#selecting-video-hardware) en este documento. Un zero client mediante USB también puede habilitar la conexión de una variedad de USB y que no sean\-dispositivos USB al concentrador. Los clientes USB cero generados por los fabricantes de hardware específico y requieren la instalación de un dispositivo\-controlador específico.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>Seleccionar dispositivos de mouse y teclados  
Los dispositivos de teclado y mouse que conecte\-en a la estación suele ser dispositivos USB. Algunos USB cero clientes proporcionan PS\/2 puertos, en cuyo caso, el teclado y mouse (ratón) debe usar PS\/2 para conectarse al concentrador de estaciones. También puede usar un PS\/2 de teclado y mouse si está configurando un PS\/2 directo\-vídeo\-estación conectada.  
  
Un teclado con un concentrador interno puede usarse como un concentrador de estaciones. Sin embargo, todos los demás dispositivos de la estación deben conectarse al concentrador interno mediante el uso de puertos en el teclado. Si este tipo de un teclado está conectado al equipo a través de otro centro, ese concentrador se tratará como concentrador intermedio.  
  
Si usas división\-estaciones de pantalla, puede desear considere la posibilidad de usar un teclado mini que no tiene un teclado numérico para que quepan los dos teclados delante el monitor.  
  
### <a name="selecting-monitors"></a>Selección de monitores  
Debe haber un monitor que se proporcionan para cada estación de MultiPoint Services, a menos que una división\-está prevista la pantalla. Monitores están conectados a la tarjeta de vídeo en el equipo, el zero client mediante USB o la LAN\-cliente basado en. Cualquier tipo de monitor que es compatible con la tarjeta de vídeo, zero client mediante USB o LAN\-se puede usar en función del cliente, incluidos los monitores CRT.  
  
Algunos monitores especiales incluyen una LAN interna\-en función de cliente o zero client mediante USB. Estos monitores incluirá, normalmente, la entrada de audio\/conectores e internos concentradores USB para conectar teclados y mouse de salida. Se conectan al servidor a través de un puerto USB o una conexión LAN.  
  
#### <a name="display-resolution"></a>Resolución de pantalla  
La resolución mínima compatible para el área de presentación de la estación es 512 x 768 píxeles. Si el sistema MultiPoint Services se inicia y encuentra que área de presentación de la estación es menor que la resolución mínima, se mostrará una pantalla en blanco en esa estación y no se podrá usar la estación.  
  
Si un monitor se va a compartirse entre dos estaciones como división\-pantalla estaciones, el requisito mínimo para la presentación es de 1024 x 768, para que las áreas de pantalla de estación individuales resultantes sean al menos 512 x 768. Para obtener la mejor dividir\-experiencia de usuario de la pantalla, se recomienda una pantalla panorámica con un mínimo de resolución de 1600 x 900.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Selección de otros dispositivos periféricos de estación  
MultiPoint Services es compatible con dispositivos periféricos que están conectados a un concentrador de estaciones, un zero client mediante USB, o directamente en el equipo. Los dispositivos conectados a un concentrador de estaciones se asociarán a esa estación específica. Otros dispositivos están disponibles para todas las estaciones cuando está conectado directamente en el equipo. Los clientes de LAN también pueden admitir dispositivos periféricos.  
  
> [!IMPORTANT]  
> No se puede conectar un teclado a un centro de nivel inferior \(por ejemplo, un concentrador que está conectado a un concentrador de estaciones\). Si conecta un teclado a un concentrador de nivel inferior, los periféricos que están conectados\-al centro de nivel inferior ya no estará disponible para esa estación. Este comportamiento permite la compatibilidad de margarita\-encadenadas concentradores de estaciones.  
  
**Disponible en todas las estaciones** una unidad USB que se conecta al equipo \(por ejemplo, no a través de un concentrador de estaciones\) está disponible en todas las estaciones. En función del dispositivo, se puede utilizar por varios usuarios al mismo tiempo o solo un usuario puede acceder a la vez. En la tabla siguiente se explica cómo se pueden acceder a los dispositivos USB.  
  
> [!NOTE]  
> La columna "Conectado al equipo de Host" en la tabla hace referencia al comportamiento cuando el equipo que ejecuta MultiPoint Services se está ejecutando en modo de estación con estaciones. Si está ejecutando en modo de consola, se comportan los periféricos que están conectados en cualquier lugar en la misma manera que un servidor estándar en una sesión de consola.  
  
||Conectado al equipo Host|Conectado al concentrador de estaciones o Hub de nivel inferior|  
|-|------------------------------|----------------------------------------------|  
|Teclado|No es funcional, a menos que sea parte de una estación de PS/2. |Disponible para estaciones individuales<br /><br />No se puede conectar a un concentrador de nivel inferior|  
|Mouse|No es funcional, a menos que sea parte de una estación de PS/2. |Disponible para estaciones individuales|  
|Auriculares o altavoces|No es funcional, a menos que sea parte de una estación de PS/2.|Disponible para estaciones individuales|  
|Dispositivo de almacenamiento USB|Disponible en todas las estaciones|Disponible para estaciones individuales|  
|Control consumidor HID|No es funcional|Disponible para estaciones individuales|  
|Otros dispositivos USB, como las cámaras, los lectores de documentos y unidades de DVD|Disponible en todas las estaciones si es compatible con Windows Server 2012|Disponible en todas las estaciones si es compatible con servicios de escritorio remoto de Windows Server 2008 R2|  
  
## <a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>Selección de RDP\-sobre\-LAN\-conectado el hardware de la estación  
Cualquier cliente de LAN que puede conectarse a servicios de escritorio remoto, utilizando el protocolo de escritorio remoto, puede convertirse en una estación de MultiPoint Services.  
  
Si desea que el cliente de LAN para usarse solo como una estación de MultiPoint, puede "bloquear" en el cliente de LAN. Por ejemplo, puede configurar a su cliente ligero para que solo puede conectarse a una sesión de MultiPoint Services, o configurar los equipos de escritorio para que los elementos que el acceso a los iconos del escritorio y del menú Inicio, como se quita un explorador web para evitar el acceso directo a Internet. Puede realizar estas configuraciones mediante sus herramientas de configuración de cliente de LAN, grupo o directivas locales.  
  
## <a name="selecting-audio-devices"></a>Seleccionar dispositivos de audio  
Es importante asegurarse de que al seleccionar dispositivos de audio, pueden incluirse en el concentrador de estaciones, zero client mediante USB o cliente de LAN. Algunos concentradores USB, los clientes USB cero y los clientes de LAN tienen un conector de audio analógico que puede usarse con dispositivos de audio analógicos tradicionales \(como auriculares o auriculares\). Concentradores de estaciones que no dispongan de conectores analógicas pueden usar dispositivos de audio USB.  
  
Si ha configurado un PS\/2 directo\-vídeo\-estación conectada mediante el uso de PS\/2 puertos en la placa base para el teclado y mouse, debe usar el audio analógico en la placa base en orden para el dispositivo de audio que estén disponibles para esta estación cuando el sistema MultiPoint Services se ejecuta en modo de estación.  
  
Si no tienes un PS\/2 directo\-vídeo\-estación conectada, el dispositivo de audio de host en la placa base del sistema estará disponible sólo cuando el sistema MultiPoint Services se está ejecutando en modo de consola.  