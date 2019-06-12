---
title: Integración de Azure Virtual network
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 673cb5a2292bab113aefb1de37f80bf4d880b467
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433904"
---
# <a name="azure-virtual-network-integration"></a>Integración de Azure Virtual network

>Se aplica a: Windows Server 2016 Essentials

Como las organizaciones lleguen a la informática en nube, rara vez se les mover todos sus recursos de 100% a la vez, pero asumir un enfoque donde algunos recursos se encuentran en la nube y algunos son todavía en el entorno local. Este enfoque híbrido, facilita que las organizaciones no solo para mover algunos recursos informáticos en la nube, pero también pueden aumentar su infraestructura de TI sin tener que adquirir nuevo hardware.

Al implementar este enfoque híbrido de informática, es necesaria una forma sencilla para los recursos en ambas ubicaciones para comunicarse entre sí. Las redes virtuales de Azure son un servicio de Azure que permite a las organizaciones crear un punto a punto (P2P) o el sitio a sitio (S2S) red privada virtual que hace que los recursos que se ejecutan en Azure (por ejemplo, máquinas virtuales y almacenamiento) Buscar como si fueran en la red local para el acceso sin problemas de aplicaciones y recursos.

Configuración de una red Virtual de Azure puede ser compleja. Con Windows Server Essentials 2016 se puede configurar fácilmente la red Virtual de Azure a través de un sencillo asistente que le ayudará a elegir los valores predeterminados más adecuados para su entorno de red. Como se muestra en la captura de pantalla siguiente, se ha agregado una nueva tarea de integración de red Virtual de Azure a la sección de servicios en la nube de Microsoft del panel Essentials de Windows para introducir una red Virtual de Azure, así como para proporcionar un vínculo rápido para iniciar la integración .

![Captura de pantalla que muestra la pestaña de empezar a trabajar en la página principal del panel de Windows Server Essentials. En la ficha empezar a trabajar, se ha seleccionado la sección de servicios y el panel se indica en la integración de servicios en la nube de Microsoft que la red Virtual de Azure está deshabilitada actualmente.](media/azure-virtual-network-1.PNG)

Al hacer clic en el **integrar ahora** vincular redes virtuales de Azure en la captura de pantalla anterior, aparecerá un cuadro de diálogo que le pide que inicie sesión en su cuenta de Microsoft Azure. Si no tiene una cuenta de Microsoft Azure, tendrá la opción de registro de uno en esta pantalla, que se redirigirá al portal de registro de cuenta de Azure:

![Captura de pantalla que muestra la página de inicio de sesión en Microsoft Azure del Asistente para la integración con Azure Virtual network.](media/azure-virtual-network-2.PNG)

Después de iniciar sesión en Azure, se le mostrará con la opción de elegir qué suscripción que desean asociar con Azure Virtual networking service:

![Captura de pantalla que muestra la página de mi suscripción a Microsoft Azure de la integración con el Asistente para la red Virtual de Azure.](media/azure-virtual-network-3.PNG)

Una vez que ha elegido qué suscripción de Azure que desea usar para las redes virtuales de Azure, aparecerá la opción para crear una nueva red Virtual de Azure, o si uno ya ha sido el programa de instalación en esta suscripción, se mostrará el cuadro de lista desplegable que está disponible. También debe elegir un nombre para la red Local que usará la red Virtual de Azure para identificar los recursos en la red local. Por último, decide qué región de Azure en el que desea que su red Virtual de Azure que se va a hospedar. Elegir una ubicación que esté físicamente más cercano a la red local es normalmente más adecuado para optimizar la velocidad de ancho de banda para la comunicación con los recursos que puede hospedar en sus servicios de Azure:

![Captura de pantalla que muestra la página establecer seguridad de Azure Virtual de red del Asistente para la integración con Azure Virtual network.](media/azure-virtual-network-4.PNG)

Es el último paso del proceso de integración configurar el dispositivo VPN que se usará para la conexión VPN de S2S. Dado que las empresas más pequeñas tienen sólo unos pocos servidores en su entorno y falta el personal de TI para configurar correctamente un enrutador VPN para conectarse a Microsoft Azure, será la selección predeterminada configurar el servidor de Windows Server Essentials como servidor VPN que los recursos en la red local se conectará a fin de obtener acceso a recursos de la red Virtual de Azure. Sin embargo, si prefiere utilizar otro servidor en su entorno que el servidor VPN o prefiere usar un enrutador VPN, puede seleccionar estas opciones.

Debido a la variación en los modelos y los tipos de enrutadores, Windows Server Essentials no intenta configurar automáticamente el enrutador VPN. Seleccionar el enrutador VPN en este asistente para la integración solo notifica a una red Virtual de Azure del tipo de dispositivo para configuraciones de enrutamiento adecuados en Azure es necesarios para la conectividad.

Al finalizar al Asistente de integración, una nueva pestaña será visible en el panel de Windows Server Essentials para las redes virtuales de Azure:

![Captura de pantalla que muestra la página de la red virtual de Azure del panel de Windows Server Essentials. La pestaña de la red Virtual de Azure está seleccionada y muestra el estado de configuración.](media/azure-virtual-network-5.PNG)

>! Tenga en cuenta la finalización de la configuración de una red Virtual de Azure en la nube puede tardar mucho tiempo, hacia arriba en 30 minutos. Durante este tiempo, el estado de configuración estará presente en la página de estado de red Virtual de Azure del panel.

Una vez completada la configuración de la red Virtual de Azure, el estado se cambia a conectado y mostrar los detalles de la red Virtual de Azure como datos de entrada/salida, dirección IP de puerta de enlace, detalles de dirección y la cuenta locales de IP:

![Captura de pantalla que muestra la página de la red virtual de Azure del panel de Windows Server Essentials. La pestaña de la red Virtual de Azure está seleccionada y muestra el estado conectado y en esta información de estado se muestran los detalles de la red virtual.](media/azure-virtual-network-6.PNG)

En el panel de tareas en el lado derecho del panel son las diversas tareas que la puede llevar a cabo con la red Virtual de Azure.

-   **Desconexión de red virtual de Azure** la configuración de una red Virtual de Azure es gratuita, pero hay un cargo por la puerta de enlace VPN que se conecta a un entorno local y otras redes virtuales en Azure. Desconectando la red virtual de Azure, detiene toda la facturación.

-   **Cambiar a través de dispositivo VPN** en caso de que desea cambiar de un servidor VPN a un enrutador VPN, esta tarea le permitirá realizar el cambio y notificar a la red virtual de Azure.

-   **Configurar la red virtual de Azure** esta tarea permite cambiar las opciones de configuración avanzada de la red virtual de Azure mediante el redireccionamiento a la página de configuración del portal de Azure para la red virtual de Azure.

-   **Actualizar estado** actualiza la página de estado, actualizar el estado de conexión de la red virtual de Azure incluidos los datos de entrada/salida.

-   **Deshabilitar la integración de red virtual de Azure** se desconecta de la red virtual de Azure y quita la integración desde el panel de Windows Server Essentials. Tenga en cuenta esto no elimina la red virtual de Azure, configuración todavía se conserva en Azure si desea más adelante vuelve a integrar red virtual de Azure con el panel.

-   **Más información sobre la red virtual de Azure** [ https://azure.microsoft.com/services/virtual-network/ ](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Vea también
--------
[Introducción a Windows Server Essentials](get-started.md)
