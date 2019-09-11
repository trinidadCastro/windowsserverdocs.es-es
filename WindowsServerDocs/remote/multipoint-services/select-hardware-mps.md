---
title: Selección del hardware de su sistema de MultiPoint Services
description: Consideraciones de hardware para Multipoint Services
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
ms.openlocfilehash: 697ab9a1f97eab399dafac4e0c5fa5b641ed841c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871398"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Selección del hardware de su sistema de MultiPoint Services
Al compilar un sistema Multipoint Services, debe seleccionar un equipo que cumpla los requisitos del sistema de Windows Server 2016. Si decide qué componentes desea seleccionar, tenga en cuenta lo siguiente:  
  
-   El intervalo de precios objetivo de la solución completa.  
  
-   Los tipos de escenarios de uso que podría esperar para el sistema Multipoint Services, por ejemplo, si los usuarios ejecutan programas multimedia, usan programas de procesamiento de texto o de productividad o exploran Internet.  
  
-   Si el escenario tiene grandes demandas de procesamiento o memoria.  
  
-   El número de usuarios que podrían estar usando el sistema al mismo tiempo. Si planea tener muchos usuarios en el sistema al mismo tiempo, o los usuarios que usan programas intensivos del sistema, debe planear la capacidad de computación para el sistema.  
  
-   Tipo de estaciones. ¿Cuántos puertos USB o puertos de vídeo necesita?  
  
-   Planes de expansión futuros. ¿Planea agregar estaciones al sistema Multipoint Services más adelante? ¿Dispone de suficientes ranuras de tarjeta de vídeo, puertos USB o grifos de red? ¿Cuántos usuarios adicionales necesitará el hardware para admitir?  
  
-   Diseño físico. Para obtener más información, consulte [planificación de sitios de Multipoint Services](MultiPoint-services-Site-Planning.md).  
  
Un sistema Multipoint Services normalmente incluye los siguientes componentes:  
  
-   Un equipo que ejecuta Multipoint Services, que incluye una CPU, RAM, unidades de disco duro y tarjetas de vídeo.  
  
-   Un monitor, un concentrador de estaciones, un teclado y un mouse para cada estación.  
  
-   Dispositivos periféricos opcionales para las estaciones de Multipoint Services, incluidos los altavoces, los auriculares, los micrófonos o los dispositivos de almacenamiento que solo están disponibles para el usuario de la estación.  
  
-   Dispositivos periféricos opcionales que están disponibles para todos los usuarios del sistema Multipoint Services, conectados directamente al equipo host, como impresoras, unidades de disco duro externas y dispositivos de almacenamiento USB.  
  
Use la siguiente información para tomar decisiones de hardware:  
  
-   [Selección de una CPU](#selecting-a-cpu)  
-   [Selección de componentes de hardware](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Selección de una CPU  
Un sistema Multipoint Services es un\-entorno de varios usuarios, donde todos los usuarios se conectan a un único equipo host. Esto aumenta el uso de CPU porque todos los usuarios comparten el mismo equipo. Algunas tareas, como los programas \(multimedia, por ejemplo, reproductores multimedia o software\)de edición de vídeo\-, tienen demandas de procesamiento más grandes. Por lo tanto, asegúrese de seleccionar una CPU que pueda controlar los requisitos de procesamiento para el número de usuarios y tipos de escenarios de usuario que necesitará admitir.  
  
Multipoint Services requiere una\-CPU basada en x64 y debe cumplir los requisitos del sistema para el equipo tal y como se describe en [requisitos de hardware y recomendaciones de rendimiento](Hardware-Requirements-and-Performance-Recommendations.md).  
  
Los siguientes tipos de procesadores se han probado para usarse en un sistema Multipoint Services con\-programas de procesamiento de alta demanda, como programas multimedia:  
  
-   **Procesador\-de doble núcleo:** Puede admitir hasta 8 estaciones.  
-   **Procesador\-de cuatro núcleos:** Puede admitir hasta 16 estaciones.
-   **Procesador\-de cuatro núcleos con multithreading:** Puede admitir hasta 20 estaciones.      
-   **Procesador\-de seis núcleos con multithreading:** Puede admitir hasta 24 estaciones.  
  
Con esta información, seleccione una CPU que cumpla los requisitos de procesamiento del sistema Multipoint Services. 
> [!NOTE] 
> Si está ejecutando aplicaciones con un uso intensivo de vídeo, la recomendación es al menos un núcleo por estación. 
  
## <a name="selecting-hardware-components"></a>Selección de componentes de hardware  
Cuando cree un sistema Multipoint Services, tenga en cuenta los siguientes componentes de hardware que puede necesitar:  
  
-   Hardware de vídeo  
  
-   Hardware de la estación Multipoint Services  
  
    -   *Concentradores USB*  
  
    -   Clientes USB cero  
  
    -   Teclados y dispositivos de mouse  
  
    -   Monitores  
  
-   Dispositivos periféricos  
  
    -   Dispositivos de audio, como altavoces y auriculares  
  
    -   Micrófonos  
  
    -   Dispositivos de almacenamiento masivo USB  
  
Una vez seleccionados los componentes de hardware del sistema Multipoint Services, asegúrese de obtener los controladores de 64\-bits actuales para los componentes.  
  
En los temas siguientes se proporciona información detallada para ayudarle a seleccionar los componentes del sistema Multipoint Services:  
  
[Selección de hardware de vídeo](#selecting-video-hardware)  
[Selección de dispositivos\-de estación de cliente USB conectados o sin conexión de\-vídeo](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Selección de otros dispositivos periféricos de la estación](#selecting-other-station-peripheral-devices)  
[Selección de\-hardware\-de\-estación conectada a RDP en LAN](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Selección de dispositivos de audio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Selección de hardware de vídeo
El hardware de vídeo que seleccione debe admitir el número de monitores que necesitará para el número de usuarios que desea que funcionen en las estaciones de Multipoint Services. Además, los diferentes tipos de hardware de vídeo pueden proporcionar una\-solución de mayor rendimiento\-para programas con un uso intensivo de gráficos, como el contenido multimedia.  
  
Seleccione el hardware de vídeo que puede admitir el número máximo de monitores para el tipo de rendimiento que requiere el sistema Multipoint Services. Asegúrese de validar el rendimiento del hardware de vídeo que elija para asegurarse de que cumple los requisitos de rendimiento.  
  
> [!NOTE]  
> Debe instalar un controlador de vídeo que admita la extensión del escritorio en varios monitores.  
  
Las opciones de hardware de vídeo incluyen:  
  
-   Tarjetas de vídeo internas que usan una interfaz de bus de PCI o PCIe  
  
-   Controladores de vídeo externos conectados mediante USB  
  
En las secciones siguientes se describen las capacidades de cada uno de estos tipos de hardware de vídeo. Puede combinar tarjetas de vídeo internas y controladores de vídeo externos para crear el sistema que desee.  
  
### <a name="internal-video-cards"></a>Tarjetas de vídeo internas  
Una tarjeta de vídeo interna está conectada\-a la placa base del equipo. La tarjeta de vídeo interna es una solución que puede ayudar a mejorar el\-rendimiento de los programas multimedia con uso intensivo de gráficos. Sin embargo, una tarjeta de vídeo interna requiere una ranura PCI o PCIe disponible\-para conectar con la placa base. Muchas tarjetas\-de vídeo de alto rendimiento requieren una ranura PCIe, pero hay un número limitado de ranuras PCIe en una placa base. Debe saber qué tipo de ranuras de tarjeta de vídeo están disponibles en el equipo para que pueda adquirir el tipo correcto de tarjetas de vídeo.  
  
El número de monitores que se pueden conectar a cada tarjeta de vídeo depende de la GPU que se usa en la tarjeta y el número de puertos que admite, que normalmente oscila entre 2 y 6.  
  
Al seleccionar tarjetas de vídeo internas, seleccione tarjetas de vídeo que admitan el número de monitores necesarios para crear el número deseado de estaciones conectadas de vídeo directo. El número máximo de monitores que se pueden admitir es igual al número de tarjetas de vídeo internas que están conectadas\-a la placa base multiplicada por el número de puertos de monitor en cada una de esas tarjetas de vídeo. Por ejemplo, si tuviera dos tarjetas de vídeo internas y cada tarjeta tuviera dos puertos de monitor, podría admitir hasta cuatro monitores.    
  
### <a name="external-video-controllers"></a>Controladores de vídeo externos  
Los clientes USB sin conexión contienen un controlador de vídeo externo para conectar un monitor al cliente. El cliente USB 0 también puede incluir conexiones para auriculares, altavoces, micrófono u otros dispositivos periféricos.  
  
Seleccione un cliente USB de cero si desea habilitar la compatibilidad con monitores adicionales sin abrir el equipo, o si desea admitir más estaciones que las salidas de vídeo disponibles. Por ejemplo, si anteriormente tenía cuatro monitores conectados\-a tarjetas de vídeo internas y desea agregar dos monitores más, puede conectar\-dos controladores de vídeo externos al equipo y tener espacio para dos monitores más. De esta manera, puede combinar un cliente USB de cero con el controlador de vídeo y no usar más ranuras PCI o PCIe en la placa base.  
  
## <a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>Selección de dispositivos\-de estación de cliente USB conectados o sin conexión de\-vídeo  
Una estación de Multipoint Services consta de un concentrador de estaciones o de un cliente USB con un\-teclado y un mouse, y un monitor que\-está conectado al equipo host o a un cliente USB sin conexión. Otros dispositivos periféricos pueden estar conectados\-al concentrador de estaciones o al cliente USB cero, pero no son necesarios para crear una estación multipoint. Estos otros dispositivos periféricos se describen en [seleccionar otros dispositivos periféricos](#selecting-other-station-peripheral-devices)de la estación.  
  
Los dispositivos que seleccione para crear una estación de Multipoint Services deben cumplir los requisitos mínimos para trabajar con Multipoint Services. En este tema se proporcionan detalles sobre los requisitos de los siguientes dispositivos de estación de Multipoint Services:  
  
-   [Selección de concentradores USB](#selecting-usb-hubs)  
-   [Selección de clientes USB cero](#selecting-usb-zero-clients)  
-   [Selección de teclados y dispositivos de mouse](#selecting-keyboards-and-mouse-devices)  
-   [Selección de monitores](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>Selección de concentradores USB  
Los concentradores USB que se usan en un sistema Multipoint Services pueden ser un concentrador USB genérico. Estos concentradores suelen tener cuatro o más puertos USB y permiten que varios dispositivos USB se conecten a un único puerto USB del equipo. Otros dispositivos, como los teclados y los monitores de vídeo, también pueden incorporar un concentrador USB en su diseño.  
  
Una consideración adicional es el uso de un concentrador *alimentado externamente* , en lugar de un concentrador *\-alimentado por bus* . Con un concentrador alimentado por bus\-, la cantidad de corriente que proporciona el equipo host debe ser suficiente para proporcionar energía a todos los dispositivos periféricos que están\-conectados al concentrador, sin degradar el rendimiento del sistema. Un concentrador alimentado externamente le permite conectar más dispositivos periféricos y proporcionar suficiente energía a todos ellos. El uso de concentradores con tecnología externa puede ayudar a evitar problemas de rendimiento, errores de puerto y otros problemas intermitentes.  
  
Al seleccionar un concentrador USB para el sistema Multipoint Services, considere su uso. El concentrador se puede usar como *concentrador de estaciones*, un *concentrador intermedio*o un *concentrador de nivel inferior*. Consulte la tabla siguiente para obtener descripciones de cada tipo de concentrador. Se recomienda que todos los dispositivos USB sean USB 2,0 o posterior.
  
||Motor|  
|-|-----------|  
|Concentrador de estaciones|Puede tratarse\-\-de buses\-a menos que se conecten dispositivos con alta potencia o que se conecte a él un centro de bajada.|  
|Concentrador intermedio |Se debe alimentar externamente|  
|Concentrador de bajada|Se puede alimentar externamente o de bus en función de los dispositivos conectados\-al centro.|  
|Cable de extensor USB activo|Los cables USB activos que incluyen un concentrador USB suelen alimentarse por bus; por lo tanto, no se recomiendan para conectar concentradores de estaciones al equipo.|  
  
### <a name="selecting-usb-zero-clients"></a>Selección de clientes USB cero  
Un cliente USB de cero es un concentrador USB que contiene una salida de vídeo. Por lo tanto, permite que un monitor se conecte al equipo a través de una conexión USB. Para obtener más información sobre el uso de clientes USB en el vídeo, consulte [selección de hardware de vídeo](#selecting-video-hardware) en este documento. Un cliente USB cero también puede habilitar la conexión de una variedad de dispositivos USB y\-no USB con el concentrador. Los fabricantes de hardware específicos generan los clientes USB y requieren la instalación de un controlador\-específico del dispositivo.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>Selección de teclados y dispositivos de mouse  
Los dispositivos de teclado y de mouse que\-se conectan a la estación normalmente serán dispositivos USB. Algunos clientes USB no proporcionan puertos\/PS 2, en cuyo caso, el teclado y el mouse deben usar\/PS 2 para conectarse al concentrador de estaciones. También puede usar un teclado y\/un mouse de PS 2 Si está configurando una\/estación conectada a\-un vídeo de PS 2 Direct\-.  
  
Se puede usar un teclado con un centro interno como concentrador de estaciones. Sin embargo, todos los demás dispositivos de estación deben conectarse al concentrador interno mediante puertos del teclado. Si este tipo de teclado está conectado al equipo a través de otro centro, ese centro se tratará como concentrador intermedio.  
  
Si usa estaciones de pantalla\-divididas, puede que desee considerar la posibilidad de usar un mini teclado que no tenga un panel numérico para que los dos teclados quepan delante del monitor.  
  
### <a name="selecting-monitors"></a>Selección de monitores  
Debe haber un monitor proporcionado para cada estación de Multipoint Services, a menos\-que se planee una pantalla dividida. Los monitores se conectan a la tarjeta de vídeo en el equipo, el cliente USB cero o\-el cliente basado en LAN. Se puede usar cualquier tipo de monitor que sea compatible con la tarjeta de vídeo, el cliente\-USB cero o el cliente basado en LAN, incluidos los monitores de CRT.  
  
Algunos monitores especiales incluyen un cliente basado\-en LAN interno o un cliente USB de cero. Estos monitores normalmente incluirán conectores de\/salida de entrada de audio y concentradores USB internos para conectar teclados y ratones. Se conectan al servidor a través de una conexión USB o LAN.  
  
#### <a name="display-resolution"></a>Resolución de pantalla  
La resolución mínima admitida para el área de visualización de una estación es 512 x 768 píxeles. Si el sistema Multipoint Services se inicia y detecta que el área de visualización de una estación es inferior a la resolución mínima, se mostrará una pantalla en blanco en esa estación y no se podrá usar la estación.  
  
Si un monitor de pantalla va a ser compartido por dos estaciones como estaciones\-de pantalla divididas, el requisito mínimo para la pantalla es 1024 x 768, de modo que las áreas de la pantalla de la estación individual resultante sean al menos 512 x 768. Para obtener la mejor\-experiencia de usuario de pantalla dividida, se recomienda una pantalla ancha con una resolución mínima de 1600 x 900.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Selección de otros dispositivos periféricos de la estación  
Multipoint Services es compatible con dispositivos periféricos que están conectados a un concentrador de estaciones, un cliente USB o directamente al equipo. Los dispositivos conectados a un concentrador de estaciones se asociarán a esa estación específica. Otros dispositivos están disponibles para todas las estaciones cuando se conectan directamente al equipo. Los clientes LAN también pueden admitir dispositivos periféricos.  
  
> [!IMPORTANT]  
> Un teclado no se puede conectar a un concentrador \(de bajada por ejemplo, un concentrador que está conectado a un concentrador\)de estaciones. Si conecta un teclado a un centro de bajada, los periféricos que estén conectados\-al concentrador de bajada dejarán de estar disponibles para esa estación. Este comportamiento permite la compatibilidad con los\-concentradores de estaciones conectados.  
  
**Disponible para todas las estaciones** Un dispositivo USB conectado al equipo \(, por ejemplo, no a través de un concentrador\) de estaciones, está disponible para todas las estaciones. En función del dispositivo, pueden usarse varios usuarios al mismo tiempo o solo un usuario puede acceder a él a la vez. En la tabla siguiente se explica cómo se puede obtener acceso a los dispositivos USB.  
  
> [!NOTE]  
> La columna "conectado a equipo host" de la tabla hace referencia al comportamiento cuando el equipo que ejecuta Multipoint Services se está ejecutando en modo de estación con estaciones. Si está ejecutando en el modo de consola, los periféricos que están conectados en cualquier parte se comportan de la misma manera que un servidor estándar en una sesión de consola.  
  
||Conectado al equipo host|Conectado al concentrador de estaciones o al centro de bajada|  
|-|------------------------------|----------------------------------------------|  
|Teclado|No funcional, a menos que forme parte de una estación PS/2. |Disponible para una estación individual<br /><br />No se puede conectar a un concentrador de nivel inferior|  
|Mouse|No funcional, a menos que forme parte de una estación PS/2. |Disponible para una estación individual|  
|Altavoces/auriculares|No funcional, a menos que forme parte de una estación PS/2.|Disponible para una estación individual|  
|Dispositivo de almacenamiento USB|Disponible para todas las estaciones|Disponible para una estación individual|  
|Control de consumidor HID|No funcional|Disponible para una estación individual|  
|Otros dispositivos USB, como cámaras, lectores de documentos y unidades de DVD|Disponible para todas las estaciones si es compatible con Windows Server 2012|Está disponible para todas las estaciones si es compatible con Windows Server 2008 R2 Servicios de Escritorio remoto|  
  
## <a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>Selección de\-hardware\-de\-estación conectada a RDP en LAN  
Cualquier cliente LAN que pueda conectarse a Servicios de Escritorio remoto, mediante Protocolo de escritorio remoto, puede convertirse en una estación de Multipoint Services.  
  
Si desea que el cliente de LAN solo se use como una estación Multipoint, es posible que desee "bloquear" el cliente de LAN. Por ejemplo, configure el cliente ligero para que solo pueda conectarse a una sesión de Multipoint Services o configurar los equipos de escritorio para que se quite el acceso a los iconos del escritorio y los elementos del menú Inicio, como un explorador Web, para evitar el acceso directo a Internet. Puede establecer estas configuraciones mediante las herramientas de configuración de cliente LAN o las directivas de grupo o local.  
  
## <a name="selecting-audio-devices"></a>Selección de dispositivos de audio  
Es importante asegurarse de que cuando se seleccionan dispositivos de audio, se pueden conectar en el concentrador de estaciones, en el cliente USB o en el cliente de LAN. Algunos concentradores USB, clientes USB y clientes LAN tienen un conector de audio analógico que se puede usar con dispositivos \(de audio analógicos tradicionales, como auriculares o Earbuds.\) Los concentradores de estaciones que no tienen conectores analógicos pueden usar dispositivos de audio USB.  
  
Si ha configurado una estación conectada\/a un\-vídeo\-de PS 2 mediante puertos\/PS 2 en la placa base del equipo para el teclado y el mouse, debe usar el audio analógico en la placa base del equipo en Pida que el dispositivo de audio esté disponible para esta estación cuando el sistema Multipoint Services se esté ejecutando en modo de estación.  
  
Si no tiene una estación conectada a\/un\-vídeo\-de PS 2, el dispositivo de audio de host de la placa base del sistema solo estará disponible cuando el sistema Multipoint Services se esté ejecutando en modo de consola.  