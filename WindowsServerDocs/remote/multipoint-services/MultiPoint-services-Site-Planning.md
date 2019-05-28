---
title: Planeación del sitio de MultiPoint Services
description: Información de planeación para las implementaciones de MultiPoint Services en Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: da27467efb842368167b7a315056506e99331e8d
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034592"
---
# <a name="multipoint-services-site-planning"></a>Planeación del sitio de MultiPoint Services
Debe considerar la ubicación donde se va a implementar uno o más equipos que ejecuta MultiPoint Services y sus estaciones asociados.  
  
El equipo que ejecuta el rol de MultiPoint Services debe tener un acceso cómodo a una fuente de alimentación y a los dispositivos periféricos que están conectados directamente a él, como una impresora. Además, el equipo que ejecuta MultiPoint Services debe tener un acceso cómodo a una conexión de red. Una conexión de red es necesaria para tener acceso a Internet y si está disponible, una LAN.  
  
Factores adicionales que considere la posibilidad de incluyen lo siguiente:  
  
-   ¿El sistema MultiPoint Services se configurará en una sala específica o se configurará en un carro de compra gradual o una tabla, por lo que se puede mover desde un lugar a otro?  
  
    > [!NOTE]  
    > Si va a usar un programa de instalación de dispositivos móvil, puede *asociar* las estaciones con MultiPoint Services cada vez que se vuelve a conectar, para asegurarse de que cada teclado y mouse (ratón) está asociado con el monitor apropiado.  
  
-   ¿Se encuentra junto a las otras estaciones la estación principal, o será independiente? ¿Por ejemplo, si el sistema MultiPoint Services se ha configurado en un aula, la estación principal en el departamento de soporte técnico del profesor y las estaciones estándares colocará en otro lugar en el salón? Cuando se reinicia el equipo que ejecuta MultiPoint Services, la estación principal tendrán acceso a las pantallas de inicio. Si le preocupa este nivel de acceso en el aula, prefiere poner la estación principal en el departamento de soporte técnico del profesor.  
  
-   ¿Cuántos estaciones se ajustará en el salón?  
  
-   ¿Necesita una red? Una solución de servidor único que usa direct vídeo conectado o zero client mediante USB estaciones conectadas no necesita una red.  
  
-   ¿Hay suficientes conexiones de red en la sala para admitir el número necesario de equipos que ejecuten MultiPoint Services  
  
-   ¿Dónde se encuentran las tomas de corriente?  
  
-   ¿Necesita un dispositivo de pantalla adicional, como un proyector? ¿Si tiene previsto utilizar un proyector, se bloqueará desde el techo o se colocarán en una tabla?  
  
-   ¿Qué tipo de cables será necesario y cuántas se necesitará?  
  
-   Tenga en cuenta cómo desea expandir en el futuro. ¿Se van a agregar las estaciones más?  
  
## <a name="station-layout-and-configuration"></a>Configuración y el diseño de la estación  
El diseño físico de su sitio puede afectar a la elección del tipo de estación. Para obtener más información acerca de los tipos diferentes de estación, consulte [estaciones de MultiPoint](MultiPoint-services-Stations.md) en esta guía. Se permiten varios tipos de estación en un único MultiPoint Services. Esto le brinda una flexibilidad adicional para satisfacer sus necesidades de la instalación.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Diseño de las estaciones de vídeo directamente conectados  
  
-   Para una estación de vídeo directamente conectados, la distancia entre los monitores y el equipo está limitada por la longitud del cable de vídeo.  
  
-   Se admite el uso de concentradores intermedios o concentradores de estaciones encadenados de facilidad de implementación, pero el número máximo recomendado de concentradores consecutivos es tres. Esto significa que la distancia máxima entre el equipo al concentrador de estaciones es de 15 metros, porque cada cable USB 2.0 tiene la longitud máxima de cinco medidores.  
  
> [!IMPORTANT]  
> Siempre debe haber al menos una estación conectada vídeo directa por equipo para que actúe como la estación principal.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Estaciones conectadas de diseño para zero client mediante USB  
  
-   Se admite el uso de concentradores intermedios o concentradores de estaciones encadenados de facilidad de implementación, pero el número máximo recomendado de concentradores consecutivos es tres. Esto significa que la distancia máxima entre el equipo al concentrador de estaciones es de 15 metros, porque cada cable USB 2.0 tiene la longitud máxima de cinco medidores.  
  
-   El número máximo recomendado de USB cero clientes conectados a un único concentrador intermedio es tres.  
  
    > [!NOTE]  
    > Algunos equipos vienen con un concentrador genérico en la placa base, que tiene el efecto de agregar un concentrador adicional entre el *concentrador raíz* del equipo y los concentradores de estaciones.  
  
-   Si se usará principalmente el vídeo, se recomienda que conecte a no más de dos clientes USB cero a un puerto USB en el servidor. Por ejemplo, si se usa un centro de notificaciones intermedio, sólo dos de los clientes de cero USB deben estar conectados a ella. O bien, si son daisy encadenamiento a clientes USB cero, solo dos puertos USB cero clientes deben estar encadenados. La adición de cada zero client mediante USB al puerto USB en el servidor reduce el ancho de banda vídeo disponible.  
  
-   Si va a conectar a más de tres clientes USB cero a un único puerto USB en el servidor, se recomienda usar USB 3.0 entre el servidor y el centro de notificaciones intermedio.  
  
> [!NOTE]  
> Se recomienda que compruebe el rendimiento mediante el uso de las aplicaciones y hardware para decidir cuántos USB cero a los clientes pueden conectarse a un puerto USB en el servidor.  
  
![Downstream hubs](./media/WMS_diagram6.gif)  
  
**Figura 5** sistema MultiPoint Services con tres USB cero clientes conectados a un único concentrador intermedio  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Estaciones conectadas de diseño para RDP a través de LAN  
No hay ninguna limitación de distancia física para los clientes de LAN. Siempre que se encuentran en la LAN, pueden conectarse al sistema MultiPoint Services.  
  
## <a name="using-additional-hubs"></a>Uso de concentradores adicionales  
Hubs adicionales se puede usar para facilitar la instalación. Hay tres tipos de concentradores que se usan en un sistema MultiPoint Services:  
  
-   [Concentradores de estaciones](#station-hubs)  
  
-   [Concentradores intermedios](#intermediate-hubs)  
  
-   [Hubs de nivel inferiores](#downstream-hubs)  
  
### <a name="station-hubs"></a>Concentradores de estaciones  
Un concentrador de estaciones es un concentrador externo que se ha asociado con una estación de MultiPoint Services. Como mínimo, el concentrador de estaciones tendrá un teclado conectado en ella. También puede tener más periféricos conectados. Un concentrador de estaciones puede ser un concentrador USB genérico que se ajusta a la USB 2.0 o la especificación de una versión posterior. Concentradores de estaciones deben ser alimentar si los dispositivos de alta potencia harán complemento a ellos.  
  
**Concentrador raíz** concentrador USB A que está integrado en la controladora de host en la placa base de un equipo se conoce como un *concentrador raíz*. Concentradores de estaciones están generalmente conectado en el concentrador raíz en el equipo que ejecuta MultiPoint Services.  
  
> [!NOTE]  
> Concentradores de raíz no deben usarse como concentradores de estaciones. Cuando los puertos USB están integrados en un equipo, a menudo no resulta posible determinar qué concentrador de raíz USB que están conectados internamente. Por lo tanto, si se conectan de un teclado de estación y un mouse directamente a los puertos USB del equipo, se pueden realmente se conecta en el teclado y mouse a diferentes centros de raíz USB. Para garantizar que el teclado y mouse estén en el mismo centro, complemento un concentrador de estaciones para el puerto USB del equipo y, a continuación, complemento el teclado y mouse a la estación de concentrador.  
  
**DAISY encadenamiento estaciones** , es posible que sea más fácil concentradores de estaciones se conectan al concentrador de estaciones de otra, en lugar de directamente al equipo. Esto le permite conectar un concentrador USB a un concentrador de estaciones que está ya conectado en el equipo, por lo que tiene un concentrador de estaciones conectado al concentrador de estaciones de otro.  
  
Debe haber no más de tres clientes USB cero o estación hubs margarita consecutivamente. Debe tener cuidado que no se supera el ancho de banda USB cuando margarita concentradores de estación.  
  
![Daisy chaining stations](./media/WMS_diagram5.gif)  
  
**Figura 6** sistema MultiPoint Services con la cadena de margarita estaciones  
  
### <a name="intermediate-hubs"></a>Concentradores intermedios  
Concentrador intermedio es un concentrador que está entre el servidor y un concentrador de estaciones. Se utiliza normalmente para aumentar el número de puertos que están disponibles para los concentradores de estaciones o para ampliar la distancia de las estaciones desde el equipo. Se recomienda que no más de dos centros intermedios se usan entre un concentrador de estaciones y el servidor.  
  
Concentradores intermedios deben USB 2.0 o posterior, y deben alimentar. Si va a conectar a más de tres clientes de cero USB a un concentrador intermedio, se recomienda USB 3.0 entre el servidor y el centro de notificaciones intermedio.  
  
### <a name="downstream-hubs"></a>Downstream hubs  
Un centro de nivel inferior está conectado a un concentrador de estaciones para agregar puertos más disponibles para los dispositivos de la estación. Un centro de nivel inferior puede ser externamente con tecnología o alimentado por bus, dependiendo de los dispositivos que están conectados en el concentrador.  
  
![Varias conexiones de cliente USB cero](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figura 7** sistema MultiPoint Services con un centro de notificaciones intermedio, un concentrador de estaciones y un centro de nivel inferior  
  
## <a name="users-stations-and-computers"></a>Los usuarios, las estaciones y equipos  
El número de estaciones que necesitará depende del número de personas que tendrán acceso a los equipos que ejecuta MultiPoint Services al mismo tiempo. De forma similar, el número de equipos que ejecutan servicios MultiPoint que tendrá depende el número total de las estaciones necesarios. Estaciones vídeo directamente conectados, conectados por USB cero-client estaciones y estaciones de RDP-over-LAN-conectada son todas las estaciones meditadas. Además, si se usa la funcionalidad de pantalla dividida, cada mitad se considera una estación.  
  
## <a name="power-considerations"></a>Consideraciones sobre la alimentación  
Los siguientes componentes requieren acceso a una regleta o toma de corriente:  
  
-   Servidor  
-   Monitores
-   Intermedio hubs \(si usa\) 
-   Algunos clientes USB cero  
-   Dispositivos USB, como algunos dispositivos de almacenamiento externo y las unidades de DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Diseños de sistema de MultiPoint Services de ejemplo  
Dependiendo de los muebles disponibles, el tamaño de la sala, el número de equipos que ejecuten MultiPoint Services y las estaciones en la sala, hay una variedad de formas que se pueden organizar las estaciones físicas. Los siguientes diagramas ilustran cinco de las alternativas posibles.  
  
> [!NOTE]  
> Algunos de estos diagramas muestran un proyector conectado al sistema MultiPoint Services. Esto es sólo un ejemplo; la inclusión de un proyector en un sistema MultiPoint Services es opcional.  
  
**Laboratorio de informática** en esta configuración, las estaciones están organizadas en torno a las paredes de la sala, con los estudiantes que enfrentan las paredes.  
  
![Organización de aula en laboratorio de equipos](./media/WMS_ComputerLabLayout.gif)  
  
**Grupos** en esta configuración, hay tres equipos que ejecuten MultiPoint Services, con las estaciones en clúster en torno a cada equipo.  
  
![Aula configurada con compartimentos para servidores](./media/WMS_ClassroomPods.gif)  
  
**Lecture sala** en esta configuración, se configuran las estaciones en filas. Una ventaja de esta configuración es que todos los alumnos enfrentan el instructor.  
  
![Classroom configured as Lecture Room](./media/WMS_LectureRoom.gif)  
  
**Centro de actividades de** esta configuración se compone de un diseño de la sala de conferencias tradicional para los escritorios, y tiene un área independiente con un único equipo que está ejecutando MultiPoint Services con sus asociados estaciones.  
  
![Centro de actividades de multiPoint Services](./media/WMSActivityCenter.gif)  
  
**Office Small business** en esta configuración, el equipo que está ejecutando MultiPoint Services se coloca en una ubicación central y los usuarios a lo largo de la oficina conectarse a él mediante una red de área local \(LAN\).  
  
![Estación conectada a Zero Client mediante USB](./media/Diagram1.gif)