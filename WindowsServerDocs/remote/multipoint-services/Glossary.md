---
title: Glosario
description: Define palabras, términos y conceptos en Multipoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9a5f76f0f41d9ff1726a1a468fde7f53b6a7634d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859228"
---
# <a name="glossary"></a>Glosario
**asociar una estación**  
Para especificar el monitor que se usa con qué dispositivos periféricos y de estación, como un teclado y un mouse. En el caso de las estaciones conectadas a vídeo directo, esto se hace presionando una tecla especificada en el teclado de la estación cuando se le solicite. En el caso de las estaciones conectadas al cliente USB, esto suele ocurrir automáticamente.  
  
**concentrador alimentado por bus**  
Un concentrador que dibuja toda su energía desde la interfaz USB del equipo. Los concentradores con tecnología de bus no necesitan conexiones de alimentación independientes. Sin embargo, muchos dispositivos no funcionan con este tipo de concentrador porque requieren más energía de la que proporciona este tipo de concentrador.  
  
**modo de consola**  
Se puede iniciar uno de los dos modos Multipoint Services. Cuando el sistema está en modo de consola, no hay ninguna estación disponible para su uso. En su lugar, todos los monitores se tratan como un solo escritorio extendido para la sesión de consola del sistema. El modo de consola se usa normalmente para instalar, actualizar o configurar el software, que no se puede hacer cuando el equipo está en modo de estación. Vea también: *modo de estación*.  
  
**estación conectada a vídeo directo**  
Una estación Multipoint formada por un monitor que está conectado directamente a una salida de vídeo en el servidor y, como mínimo, incluye un teclado y un mouse que están conectados al servidor a través de un concentrador USB.  
  
**cuenta de usuario de dominio**  
Una cuenta de usuario que está hospedada en un equipo de dominio. Se puede tener acceso a las cuentas de usuario de dominio desde cualquier equipo conectado al dominio y no están vinculados a ningún equipo determinado.  
  
**concentrador de bajada**  
Un concentrador que está conectado a un concentrador de estaciones para agregar más puertos disponibles para dispositivos de estación. Un concentrador de nivel inferior no debe tener un teclado conectado.  
  
**central alimentada externamente**  
También conocido como concentrador autoalimentado, este centro toma su potencia de una unidad de fuente de alimentación externa. por lo tanto, puede proporcionar una potencia completa (hasta 500 mA) en cada puerto. Muchos concentradores pueden funcionar como concentradores con tecnología de bus o externamente.  
  
**Dispositivo de control de consumidor HID**  
Un HID (HID) es un dispositivo de equipo que interactúa directamente con los usuarios. Puede tomar como entrada o enviar la salida a los usuarios. Algunos ejemplos son el teclado, el mouse, el lápiz, el panel táctil, el Stick apuntador, la tabla de gráficos, el joystick, el escáner de huellas digitales, el controlador de vídeo, la cámara web, el casco y los dispositivos Un dispositivo de control de consumidor HID es una clase concreta de dispositivos HID que incluye controles de volumen de audio y claves de control de multimedia y de explorador.  
  
**concentrador intermedio**  
Un concentrador que se encuentra entre un *concentrador raíz* en el servidor y un concentrador de estaciones. Los concentradores intermedios se suelen usar para aumentar el número de puertos disponibles para los concentradores de estaciones o para ampliar la distancia de las estaciones del equipo.  
  
**cuenta de usuario local**  
Una cuenta de usuario en un equipo específico. Una cuenta de usuario local solo está disponible en el equipo donde se ha definido la cuenta.  
  
**concentrador multifunción**  
Consulte *cliente USB Zero*.  
  
**Sistema Multipoint Services**  
Una colección de hardware y software que consta de un equipo que tiene instalado Windows Server 2016 con la función Multipoint Services habilitada y al menos una estación multipoint. Para obtener más información sobre las opciones de diseño del sistema, consulte [diseño de sitios de Multipoint Services](MultiPoint-services-Site-Planning.md)  
  
**divide**  
Sección de espacio en un disco físico que funciona como si se tratase de un disco independiente.  
  
**estación principal**  
La estación que es la primera en iniciarse cuando se inicia Multipoint Services. Un administrador puede usar la estación principal para tener acceso a los menús y a la configuración de inicio. Cuando el administrador no la usa, se puede usar como una estación normal (no tiene que reservarse exclusivamente para la administración). El monitor de la estación primaria siempre debe estar conectado directamente a una salida de vídeo en el equipo que ejecuta Multipoint Services. Vea también: estación.  
  
**Estación conectada a través de LAN con RDP**  
Una estación que es un cliente ligero, un escritorio tradicional o un equipo portátil que se conecta a multipoint Services mediante Protocolo de escritorio remoto (RDP) a través de la red de área local (LAN).  
  
**concentrador raíz**  
Un concentrador USB integrado en el controlador de host de la placa base de un equipo.  
  
**pantalla dividida**  
Una estación en la que se puede usar un solo monitor para mostrar dos escritorios de usuario independientes. Dos conjuntos de concentradores, teclados y ratones están asociados a un único monitor. Un conjunto está asociado con el lado izquierdo del monitor y el otro conjunto está asociado con el lado derecho del monitor.  
  
**estación estándar**  
A diferencia de la *estación principal*, que un administrador puede usar para tener acceso a los menús de inicio, las estaciones estándar no mostrarán los menús de inicio y solo se pueden usar una vez que Multipoint Services haya completado el proceso de inicio. Vea también: estación.  
  
*deja*  
Extremo de usuario para conectarse al equipo que ejecuta Multipoint Services. Se admiten tres tipos de estación: estaciones conectadas directamente a través de la LAN y conectadas por el cliente. Para obtener más información acerca de las estaciones, consulte [estaciones Multipoint](MultiPoint-services-Stations.md).  
  
**concentrador de estaciones**  
Un concentrador USB que se ha asociado con un monitor para crear una estación multipoint. Conecta dispositivos periféricos USB a multipoint Services. Vea también: *cliente USB cero* y *concentrador USB*.  
  
**modo de estación**  
Se puede iniciar uno de los dos modos Multipoint Services. Normalmente, el sistema Multipoint Services está en modo de estación. En el modo de estación, las estaciones de Multipoint Services se comportan como si cada estación es un equipo independiente que ejecuta el sistema operativo Windows y varios usuarios pueden usar el sistema al mismo tiempo. Vea también: *modo de consola*.  
  
**Concentrador USB**  
Un concentrador de expansión USB multipuerto genérico que cumple con las especificaciones de bus serie universal (USB) 2,0 o posterior. Estos concentradores suelen tener varios puertos USB, lo que permite que varios dispositivos USB se conecten a un único puerto USB del equipo. Los concentradores USB son normalmente dispositivos independientes que se pueden alimentar de forma *externa* o por *bus*. Otros dispositivos, como algunos teclados y monitores de vídeo, pueden incorporar un concentrador USB en su diseño. Vea también: *cliente USB cero*.  
  
**Cliente USB a través de Ethernet cero**  
Un cliente USB de cero que se conecta al equipo a través de una conexión LAN en lugar de un puerto USB. Este cliente aparece en el servidor como un dispositivo USB incluso a través de los datos que se envían a través de la conexión Ethernet.  
  
**Cliente USB cero**  
Un concentrador de expansión que se conecta al equipo a través de un puerto USB y permite la conexión de una variedad de dispositivos que no sean USB al concentrador. Los fabricantes de hardware específicos generan los clientes USB y requieren la instalación de un controlador específico del dispositivo. Los clientes USB no admiten la conexión de un monitor de vídeo (a través de VGA, DVI, etc.) y periféricos (a veces PS/2 y audio analógico). El cliente USB cero se puede *alimentar externamente* o *por bus*. Vea también *concentradores USB*.  
  
**Estación conectada de cliente USB cero**  
Una estación de Multipoint Services que consta de (como mínimo) un monitor, un teclado y un mouse, que están conectados al servidor a través de un cliente USB sin conexión.  
  
