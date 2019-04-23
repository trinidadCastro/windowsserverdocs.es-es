---
title: Glosario
description: Define las palabras, los términos y conceptos en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 4449d2d6fb87f74496b7d482a7a7263f703c7822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831116"
---
# <a name="glossary"></a>Glosario
**asociar una estación**  
Para especificar qué monitor se utiliza con qué dispositivos periféricos, como un teclado y mouse (ratón) y la estación. Para las estaciones conectadas vídeo directas, esto se hace presionando una clave especificada teclado de la estación cuando se le solicite hacerlo. Para USB cero cliente estaciones conectadas, esto normalmente sucede automáticamente.  
  
**alimentado por bus**  
Un concentrador que dibuja todo su potencial de la interfaz USB del equipo. Concentradores alimentado por bus no necesitan las conexiones de alimentación independientes. Sin embargo, muchos dispositivos no funcionan con este tipo de concentrador porque requieren más energía que proporciona este tipo de concentrador.  
  
**modo de consola**  
Puede iniciar uno de los dos modos de MultiPoint services. Cuando el sistema está en modo de consola, no las estaciones están disponibles para su uso. En su lugar, todos los monitores se tratan como un único escritorio extendido para la sesión de consola del sistema del equipo. Modo de consola se suele usar para instalar, actualizar o configurar el software, que no se puede realizar cuando el equipo está en modo de estación. Vea también: *modo de estación*.  
  
**estación de vídeo directamente conectados**  
Una estación de MultiPoint que consta de un monitor que está conectado directamente a una salida de vídeo en el servidor y, como mínimo, incluye un teclado y mouse (ratón) que están conectados al servidor a través de un concentrador USB.  
  
**Cuenta de usuario de dominio**  
Una cuenta de usuario que se hospeda en un equipo de dominio. Las cuentas de usuario de dominio se pueden acceder desde cualquier equipo que está conectado al dominio y no están asociados a cualquier equipo en particular.  
  
**Centro de nivel inferior**  
Un concentrador que está conectado a un concentrador de estaciones para agregar puertos más disponibles para los dispositivos de la estación. Un centro de nivel inferior no debe tener un teclado conectado a ella.  
  
**concentrador alimentado de forma externa**  
También conocido como un concentrador con autoalimentación, este concentrador toma su potencial de una unidad de alimentación externas; por lo tanto, puede proporcionar toda la potencia (hasta 500 mA) para cada puerto. Muchos centros pueden funcionar como concentradores alimentado por bus o con tecnología externamente.  
  
**Dispositivo de control consumidor HID**  
Un dispositivo de interfaz de usuario (HID) es un dispositivo de equipo que interactúa directamente con los seres humanos. Puede obtener una entrada de o entregar los resultados para los humanos. Algunos ejemplos son el teclado, mouse, bola de seguimiento, panel táctil, señalador stick, tabla de gráficos, joystick, escáner de huella digital, gamepad, cámara Web, auriculares y dispositivos de conducción simulador. Un dispositivo de control de consumidor HID es una clase determinada de dispositivos HID que incluye controles de volumen de audio y teclas de control de explorador y multimedia.  
  
**Centro de notificaciones intermedio**  
Un concentrador que se encuentra entre una *concentrador raíz* en el servidor y un concentrador de estaciones. Concentradores intermedios se usan normalmente para aumentar el número de puertos disponibles para los concentradores de estaciones o para ampliar la distancia de las estaciones desde el equipo.  
  
**cuenta de usuario local**  
Una cuenta de usuario en un equipo específico. Una cuenta de usuario local solo está disponible en el equipo donde se define la cuenta.  
  
**concentrador multifunción**  
Consulte *zero client mediante USB*.  
  
**Sistema multiPoint Services**  
Una colección de hardware y software que consta de un equipo con Windows Server 2016 instalado con el rol Servicios MultiPoint habilitado y al menos una estación MultiPoint. Para obtener más información acerca de las opciones de diseño del sistema, consulte [planeación del sitio de MultiPoint Services](MultiPoint-services-Site-Planning.md)  
  
**partition**  
Una sección de espacio en un disco físico que funciona como si fuera un disco independiente.  
  
**estación principal**  
La estación que es la primera en iniciarse cuando se inicia MultiPoint Services. La estación principal puede utilizarse por un administrador para tener acceso a la configuración y los menús de inicio. Cuando no se emplean el administrador, se puede usar como estación normal (no tiene reservado exclusivamente para administración). Monitor de la estación principal siempre debe estar conectado directamente a una salida de vídeo en el equipo que está ejecutando MultiPoint Services. Vea también: estación.  
  
**Estación de RDP-over-LAN-conectado**  
Una estación que es un cliente ligero, de escritorio tradicional o equipo portátil que se conecta a MultiPoint services mediante el protocolo de escritorio remoto (RDP) a través de la red de área local (LAN).  
  
**concentrador raíz**  
Un concentrador USB que está integrado en la controladora de host en la placa base del equipo.  
  
**pantalla dividida**  
Una estación de donde se puede usar un solo monitor para mostrar dos equipos de escritorio de usuario independientes. Dos conjuntos de concentradores, teclados y ratones asociadas con un solo monitor. Un conjunto está asociado con el lado izquierdo del monitor y el otro conjunto está asociado con el lado derecho del monitor.  
  
**estación estándar**  
Por el contrario el *estación principal*, que puede utilizarse por un administrador para obtener acceso a menús de inicio, las estaciones estándares no mostrará los menús de inicio y solo se puede usar después de MultiPoint Services ha completado el proceso de inicio . Vea también: estación.  
  
*station*  
Punto de conexión de usuario para conectarse al equipo que ejecuta MultiPoint Services. Se admiten tres tipos de estación: estaciones vídeo directamente conectados, conectados por USB cero-client y RDP-over-LAN-conectada. Para obtener más información acerca de las estaciones, vea [estaciones de MultiPoint](MultiPoint-services-Stations.md).  
  
**concentrador de estaciones**  
Un concentrador USB que se ha asociado con un monitor de creación de una estación MultiPoint. Se conecta dispositivos periféricos de USB a MultiPoint Services. Consulte también: *Zero client mediante USB* y *concentrador USB*.  
  
**modo de estación**  
Puede iniciar uno de los dos modos de MultiPoint services. Normalmente, el sistema MultiPoint Services está en modo de estación. Cuando está en modo de estación, las estaciones de MultiPoint Services se comportan como si cada estación es un equipo independiente que se está ejecutando el sistema operativo de Windows y varios usuarios pueden usar el sistema al mismo tiempo. Vea también: *modo de consola*.  
  
**USB hub**  
Un concentrador USB multipuerto genérico expansión que cumpla con las especificaciones de bus serie universal (USB) 2.0 o posterior. Estos concentradores normalmente tienen varios puertos USB, que permite que varios dispositivos USB se conecten a un único puerto USB en el equipo. Concentradores USB son normalmente dispositivos independientes que pueden ser *alimentar* o *alimentado por bus*. Otros dispositivos, como algunos teclados y monitores de vídeo, pueden incorporar un concentrador USB en su diseño. Consulte también: *Zero client mediante USB*.  
  
**USB a través del cliente Ethernet cero**  
Un zero client mediante USB que se conecta al equipo a través de una conexión LAN en lugar de un puerto USB. Este cliente aparece en el servidor como un dispositivo USB, incluso a través de los datos se envía a través de la conexión Ethernet.  
  
**Zero client mediante USB**  
Un concentrador de expansión que se conecta al equipo a través de un puerto USB y permite la conexión de una variedad de dispositivos que no sean USB al concentrador. Clientes USB cero producidos por fabricantes de hardware específicos, y requieren la instalación de un controlador específico del dispositivo. Los clientes USB cero admiten la conexión de un monitor de vídeo (a través de VGA, DVI y así sucesivamente) y periféricos de (a través de USB, en ocasiones, PS/2 y audio analógico). La conexión USB puede ser cero cliente *alimentar* o *alimentado por bus*. Vea también *concentradores USB*.  
  
**Estación conectada a zero client mediante USB**  
Una estación de MultiPoint Services que consta de (como mínimo) de un monitor, teclado y un mouse, que están conectados al servidor mediante un zero client mediante USB.  
  
