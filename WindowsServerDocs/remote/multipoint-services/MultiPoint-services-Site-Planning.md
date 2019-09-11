---
title: Planeación del sitio de MultiPoint Services
description: Información de planeación para implementaciones de Multipoint Services en Windows Server 2016
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
ms.openlocfilehash: 3d49b2861d81a938fb20544c3edeb0976ac6d327
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871645"
---
# <a name="multipoint-services-site-planning"></a>Planeación del sitio de MultiPoint Services
Debe considerar la ubicación en la que se implementarán uno o más equipos que ejecuten Multipoint Services y sus estaciones asociadas.  
  
El equipo que ejecuta el rol Multipoint Services debe tener un acceso cómodo a una fuente de alimentación y a los dispositivos periféricos que están conectados directamente a él, como una impresora. Además, el equipo que ejecuta Multipoint Services debe tener un acceso cómodo a una conexión de red. Se necesita una conexión de red para tener acceso a Internet y, si está disponible, una LAN.  
  
Entre los factores adicionales que se deben tener en cuenta se incluyen los siguientes:  
  
-   ¿Se configurará el sistema Multipoint Services en un salón específico o se configurará en una tabla o carro rodante, de modo que se pueda pasar de un lugar a otro?  
  
    > [!NOTE]  
    > Si tiene previsto usar una configuración de móvil, puede *asociar* las estaciones con Multipoint Services cada vez que vuelva a conectarlas para asegurarse de que todos los teclados y el mouse estén asociados con el monitor adecuado.  
  
-   ¿Se ubicará la estación principal junto a las otras estaciones o será independiente? Por ejemplo, si el sistema Multipoint Services está configurado en un aula, ¿la estación principal estará en el escritorio del profesor y en las estaciones estándar colocadas en otro lugar de la habitación? Cuando se reinicie el equipo que ejecuta Multipoint Services, la estación principal tendrá acceso a las pantallas de inicio. Si le preocupa este nivel de acceso en una configuración de aula, puede que prefiera colocar la estación principal en el escritorio del profesor.  
  
-   ¿Cuántas estaciones caben en el salón?  
  
-   ¿Necesita una red? Una solución de un solo servidor que usa el vídeo directo conectado o una estación conectada de cliente USB sin necesidad de una red.  
  
-   Hay suficientes conexiones de red en el salón para admitir el número necesario de equipos que ejecutan Multipoint Services  
  
-   ¿Dónde se encuentran las tomas de corriente?  
  
-   ¿Necesita un dispositivo de pantalla adicional, como un proyector? Si piensa usar un proyector, ¿se bloqueará desde el límite superior o se colocará en una tabla?  
  
-   ¿Qué tipo de cables serán necesarios y cuántos serán necesarios?  
  
-   Tenga en cuenta cómo podría querer expandir en el futuro. ¿Va a agregar más estaciones?  
  
## <a name="station-layout-and-configuration"></a>Diseño y configuración de la estación  
El diseño físico del sitio puede afectar a la elección del tipo de estación. Para obtener más información sobre los distintos tipos de estación, consulte en esta guía las [estaciones Multipoint](MultiPoint-services-Stations.md) . Se permiten varios tipos de estación en un solo Multipoint Services. Esto le proporciona flexibilidad adicional para satisfacer sus necesidades de instalación.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Diseño para estaciones conectadas directamente a vídeo  
  
-   En el caso de una estación conectada directamente a vídeo, la distancia entre los monitores y el equipo está limitada por la longitud del cable de vídeo.  
  
-   El uso de concentradores intermedios o concentradores de estaciones encadenados en Margarita se admite para facilitar la implementación, pero el número máximo recomendado de concentradores consecutivos es tres. Esto significa que la distancia máxima entre el equipo y el concentrador de estaciones es de 15 metros, ya que cada uno de los cables 2,0 USB tiene la longitud máxima de cinco metros.  
  
> [!IMPORTANT]  
> Siempre debe haber al menos una estación conectada de vídeo directo por equipo para que actúe como la estación principal.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Diseño de estaciones conectadas al cliente USB sin conexión  
  
-   El uso de concentradores intermedios o concentradores de estaciones encadenados en Margarita se admite para facilitar la implementación, pero el número máximo recomendado de concentradores consecutivos es tres. Esto significa que la distancia máxima entre el equipo y el concentrador de estaciones es de 15 metros, ya que cada uno de los cables 2,0 USB tiene la longitud máxima de cinco metros.  
  
-   El número máximo recomendado de clientes USB sin conexión a un solo concentrador intermedio es tres.  
  
    > [!NOTE]  
    > Algunos equipos incorporan un concentrador genérico en la placa base, lo que tiene el efecto de agregar un concentrador adicional entre el *concentrador raíz* del equipo y los concentradores de estaciones.  
  
-   Si el vídeo se va a usar con mucha frecuencia, se recomienda que no conecte más de dos clientes USB a un puerto USB en el servidor. Por ejemplo, si se usa un concentrador intermedio, solo se deben conectar a él dos clientes USB. O bien, si está encadenando a través de un puerto USB sin clientes, solo se deben encadenar dos clientes USB. La adición de cada cliente USB Zero al puerto USB en el servidor reduce el ancho de banda de vídeo disponible.  
  
-   Si planea conectar más de tres clientes USB a un solo puerto USB en el servidor, se recomienda usar USB 3,0 entre el servidor y el concentrador intermedio.  
  
> [!NOTE]  
> Se recomienda que compruebe el rendimiento mediante el uso de las aplicaciones y el hardware para decidir el número de clientes USB que puede conectar a un puerto USB en el servidor.  
  
![Downstream hubs](./media/WMS_diagram6.gif)  
  
**Figura 5** Sistema Multipoint Services con tres clientes USB sin conexión conectada a un solo concentrador intermedio  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Diseño de estaciones conectadas de RDP a través de LAN  
No hay limitaciones de distancia física para los clientes de LAN. Siempre que se encuentren en la LAN, podrán conectarse al sistema Multipoint Services.  
  
## <a name="using-additional-hubs"></a>Uso de centros adicionales  
Se pueden usar concentradores adicionales para facilitar la instalación. Hay tres tipos de concentradores que se usan en un sistema Multipoint Services:  
  
-   [Concentradores de estaciones](#station-hubs)  
  
-   [Concentradores intermedios](#intermediate-hubs)  
  
-   [Concentradores de bajada](#downstream-hubs)  
  
### <a name="station-hubs"></a>Concentradores de estaciones  
Un concentrador de estaciones es un centro externo que se ha asociado a una estación de Multipoint Services. Como mínimo, el concentrador de estaciones tendrá un teclado conectado. También puede tener conectados periféricos adicionales. Un concentrador de estaciones puede ser un concentrador USB genérico que se ajusta a la especificación USB 2,0 o posterior. Los concentradores de estaciones deben alimentarse externamente si los dispositivos de alta potencia se encargarán de su complemento.  
  
**Concentrador raíz** Un concentrador USB integrado en el controlador de host de la placa base de un equipo se conoce como *concentrador raíz*. Normalmente, los concentradores de estaciones están conectados al concentrador raíz en el equipo que ejecuta Multipoint Services.  
  
> [!NOTE]  
> Los concentradores raíz no deben usarse como concentradores de estaciones. Cuando los puertos USB están integrados en un equipo, a menudo no es posible determinar a qué concentrador de raíz USB están conectados internamente. Por lo tanto, si conectó un teclado y un mouse de la estación directamente a los puertos USB del equipo, es posible que esté conectando el teclado y el mouse a diferentes concentradores raíz USB. Para asegurarse de que el teclado y el mouse están en el mismo centro, conecte un concentrador de estaciones al puerto USB del equipo y, a continuación, conecte el teclado y el mouse a ese concentrador de estaciones.  
  
**Estaciones de encadenamiento en Margarita** Puede ser más fácil conectar concentradores de estaciones a otro concentrador de estaciones, en lugar de hacerlo directamente al equipo. Esto le permite conectar un concentrador USB a un concentrador de estaciones que ya está conectado al equipo, de modo que tenga un concentrador de estaciones conectado a otro concentrador de estaciones.  
  
No debe haber más de tres clientes USB o concentradores de estación conectados consecutivamente. Se debe tener cuidado de que no se supere el ancho de banda USB al encadenar concentradores de estaciones.  
  
![Daisy chaining stations](./media/WMS_diagram5.gif)  
  
**Figura 6** Sistema Multipoint Services con estaciones encadenadas en Margarita  
  
### <a name="intermediate-hubs"></a>Concentradores intermedios  
Un concentrador intermedio es un concentrador que se encuentra entre el servidor y un concentrador de estaciones. Normalmente se utiliza para aumentar el número de puertos disponibles para los concentradores de estaciones o para ampliar la distancia de las estaciones del equipo. Se recomienda que no se usen más de dos concentradores intermedios entre un concentrador de estaciones y el servidor.  
  
Los concentradores intermedios deben ser USB 2,0 o posterior, y deben alimentarse externamente. Se recomienda el USB 3,0 entre el servidor y el concentrador intermedio si está conectando más de tres clientes USB sin conexión a un concentrador intermedio.  
  
### <a name="downstream-hubs"></a>Downstream hubs  
Un concentrador de bajada se conecta a un concentrador de estaciones para agregar más puertos disponibles para los dispositivos de estación. Un concentrador de bajada puede alimentarse externamente o de bus, dependiendo de los dispositivos que estén conectados al concentrador.  
  
![Varias conexiones de cliente USB sin conexión](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figura 7** Sistema Multipoint Services con un concentrador intermedio, un concentrador de estaciones y un concentrador de nivel inferior  
  
## <a name="users-stations-and-computers"></a>Usuarios, estaciones y equipos  
El número de estaciones que necesitará depende del número de personas que tendrán que acceder a los equipos que ejecutan Multipoint Services al mismo tiempo. Del mismo modo, el número de equipos que ejecutan Multipoint Services será necesario depende del número total de estaciones necesarias. Las estaciones conectadas directamente a vídeo, las estaciones conectadas en USB a cero y en el cliente, y las estaciones conectadas a RDP a través de LAN se consideran estaciones. Además, si se usa la funcionalidad de pantalla dividida, cada mitad se considera una estación.  
  
## <a name="power-considerations"></a>Consideraciones de energía  
Los componentes siguientes requieren acceso a una franja de alimentación o una toma:  
  
-   Server  
-   Monitores
-   Concentradores \(intermedios si se usan\) 
-   Algunos clientes USB sin  
-   Dispositivos USB alimentados, como algunos dispositivos de almacenamiento externo y unidades de DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Diseños del sistema Multipoint Services de ejemplo  
En función del mobiliario disponible, del tamaño de la habitación, del número de equipos que ejecutan Multipoint Services y de las estaciones de la habitación, hay varias formas de organizar las estaciones físicas. En los diagramas siguientes se muestran cinco alternativas posibles.  
  
> [!NOTE]  
> Algunos de estos diagramas muestran un proyector conectado al sistema Multipoint Services. Este es solo un ejemplo; incluir un proyector en un sistema Multipoint Services es opcional.  
  
**Laboratorio de equipos** En esta configuración, las estaciones se organizan alrededor de las paredes de la habitación, con los estudiantes orientados a las paredes.  
  
![Organización de aula en laboratorio de equipos](./media/WMS_ComputerLabLayout.gif)  
  
**Grupos** de En esta configuración, hay tres equipos que ejecutan Multipoint Services, con estaciones agrupadas en torno a cada equipo.  
  
![Aula configurada con compartimentos para servidores](./media/WMS_ClassroomPods.gif)  
  
**Salón de charla** En esta configuración, las estaciones se configuran en filas. Una ventaja de esta configuración es que todos los alumnos se encuentran en el instructor.  
  
![Classroom configured as Lecture Room](./media/WMS_LectureRoom.gif)  
  
**Centro de actividades** Esta configuración consta de un diseño tradicional de salón de charlas para los escritorios y tiene un área independiente con un solo equipo que ejecuta Multipoint Services con sus estaciones asociadas.  
  
![Centro de actividades de Multipoint Services](./media/WMSActivityCenter.gif)  
  
**Oficina de pequeña empresa** En esta configuración, el equipo que ejecuta Multipoint Services se coloca en una ubicación central y los usuarios de la oficina se conectan a él mediante una LAN \(\)de red de área local.  
  
![Estación conectada a Zero Client mediante USB](./media/Diagram1.gif)