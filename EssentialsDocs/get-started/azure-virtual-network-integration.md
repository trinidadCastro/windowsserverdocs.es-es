---
title: "Integración de red Virtual Azure"
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 21057359967c73d8d9fc434694b8f203ad5c1f6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
#<a name="azure-virtual-network-integration"></a>Integración de red Virtual Azure

>Se aplica a: Windows Server 2016 Essentials

Como las organizaciones hacer su forma de la informática en nube, rara vez se les mover todos sus recursos de 100% a la vez, pero en su lugar adoptar un enfoque que algunos recursos están en la nube y algunos se encuentran aún en local. Este enfoque híbrido facilita el proceso para las organizaciones no solo mover algunos recursos informáticos a la nube, pero también pueden aumentar su infraestructura de TI sin tener que comprar de nuevo hardware.

Al implementar este enfoque híbrido informática, se requiere una forma sencilla para los recursos en ambas ubicaciones para comunicarse entre sí. Azure redes virtuales son un servicio de Azure que permite a las organizaciones crear un punto a punto (P2P) o un sitio a sitio (S2S) red privada virtual que hace que los recursos que se ejecutan en Azure (como máquinas virtuales y almacenamiento) aspecto como si fueran en la red local acceso a la aplicación y los recursos sin problemas.

Configuración de una red Virtual de Azure puede ser compleja. Con Windows Server Essentials 2016 puede configurar fácilmente su red Virtual de Azure a través de un sencillo asistente que te ayudará a elegir los valores predeterminados más adecuados para tu entorno de red. Como se muestra en la captura de pantalla siguiente, se agregó una nueva tarea de integración de red Virtual de Azure a la sección Servicios de nube de Microsoft del panel de Windows Essentials para introducir redes virtuales de Azure, además de proporcionar un vínculo rápido para iniciar la integración.

![Captura de pantalla que muestra la pestaña introducción en la página principal del panel de Windows Server Essentials. En la pestaña introducción, se ha seleccionado la sección Servicios y el panel se indica en la integración de servicios de nube de Microsoft que actualmente está deshabilitada red Virtual de Azure.](media/azure-virtual-network-1.PNG)

Al hacer clic en el **integrar ahora** vincular redes virtuales de Azure en la captura de pantalla anterior, aparecerá un cuadro de diálogo que le pedirá que inicie sesión en tu cuenta de Microsoft Azure. Si no tienes una cuenta de Microsoft Azure, tienes la opción de registro de uno en esta pantalla, que se redirigirá al portal de registro de cuenta de Azure:

![Captura de pantalla que muestra la página de inicio de sesión en a Microsoft Azure de la integración con Azure Virtual Asistente de red.](media/azure-virtual-network-2.PNG)

Después de iniciar sesión en Azure, se le presentará con la opción de elegir qué suscripción que desean asociar con el Virtual de Azure servicio de red:

![Captura de pantalla que muestra la página de mi suscripción a Microsoft Azure de la integración con el Asistente de red Virtual de Azure.](media/azure-virtual-network-3.PNG)

Después de elegir qué suscripción de Azure que quieras usar para redes virtuales de Azure, se muestra con la opción de crear una nueva red Virtual de Azure o si ya uno ha sido el programa de instalación en esta suscripción, se mostrará el cuadro de lista desplegable está disponible. También se elige un nombre para la red Local que usará la red Virtual de Azure para identificar los recursos de la red local. Por último, decide qué región de Azure en que quieres que su red Virtual de Azure que alojarse. Elegir una ubicación físicamente más cercana a la red local normalmente es mejor para optimizar la velocidad del ancho de banda para comunicarse con recursos que pueden hospedar en sus servicios de Azure:

![Captura de pantalla muestra la página de red establece una Azure Virtual del Asistente para integración con Azure Virtual de red.](media/azure-virtual-network-4.PNG)

El último paso del proceso de integración es el dispositivo VPN que se usará para la conexión VPN S2S del programa de instalación. Dado que las más pequeñas empresas tienen solo unos pocos servidores en su entorno y falta el personal de TI para configurar correctamente un enrutador VPN se conecte a Microsoft Azure, la selección predeterminada será configurar el servidor de Windows Server Essentials como el servidor VPN que va a conectarse a recursos de la red local para acceder a los recursos de la red Virtual de Azure. Sin embargo, si prefiere utilizar otro servidor en el entorno de que el servidor VPN, o que preferirías usar un enrutador VPN, puedes seleccionar estas opciones.

Debido a la variación de modelos y tipos del enrutador, Windows Server Essentials no intenta configurar automáticamente el enrutador de VPN. Seleccionar el enrutador de VPN en este asistente para la integración solo se notifica a redes virtuales de Azure del tipo de dispositivo para configuraciones de enrutamiento adecuadas en Azure es necesarios para la conectividad.

Después de completar al Asistente de integración, una nueva pestaña estarán visible en el panel de Windows Server Essentials para redes virtuales de Azure:

![Captura de pantalla que muestra la página de Azure VNet del panel de Windows Server Essentials. La pestaña red Virtual de Azure está seleccionada y muestra el estado de configuración.](media/azure-virtual-network-5.PNG)

>! Ten en cuenta la configuración de una red Virtual de Azure en la nube puede tardar mucho tiempo, hacia arriba y 30 minutos de finalización. Durante este tiempo, el estado de la configuración estarán presente en la página de estado de red Virtual de Azure del panel.

Una vez completada la configuración de la red Virtual de Azure, el estado cambiará conectado y mostrar los detalles de la red Virtual de Azure, como datos de entrada y salida, la dirección IP de puerta de enlace, detalles de dirección y la cuenta IP locales:

![Captura de pantalla que muestra la página de Azure VNet del panel de Windows Server Essentials. La pestaña red Virtual de Azure está seleccionada y muestra el estado como conectado y, en esta información de estado se muestran los detalles de la red virtual.](media/azure-virtual-network-6.PNG)

En el panel de tareas en el lado derecho del panel son las diferentes tareas que el que puedes tomar con tu red Virtual de Azure.

-   **Desconectar de Azure VNET** configurando una red Virtual de Azure es gratuito, pero hay un cargo por la puerta de enlace VPN que se conecta de forma local y otros VNETs en Azure. Desconectar desde el VNET Azure detiene todas facturación.

-   **Cambiar sobre el dispositivo VPN** en caso de que desea cambiar de un servidor VPN a un enrutador VPN, esta tarea te permitirá hacer el cambio y notificar a la VNET de Azure.

-   **Configurar Azure VNET** esta tarea te permite cambiar las opciones de configuración avanzada de la VNET Azure si se redirige a la página de configuración de portal de Azure para VNET de Azure.

-   **Actualizar estado** actualiza la página de estado, actualizar el estado de conexión de la VNET Azure, incluidos los datos de entrada/salida.

-   **Deshabilitar la integración de Azure VNET** se desconecta la VNET de Azure y quita la integración en el panel de Windows Server Essentials. Ten en cuenta esto no elimina el VNET de Azure, configuración aún se conserva en Azure si quieres más tarde vuelve a integrar VNET de Azure con el panel.

-   **Obtener más información sobre Azure VNET**[https://azure.microsoft.com/en-us/services/virtual-network/](https://azure.microsoft.com/en-us/services/virtual-network/).

<a name="see-also"></a>Consulta también
--------
[Introducción a Windows Server Essentials](get-started.md)
