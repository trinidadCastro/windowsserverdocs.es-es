---
title: Integración de Azure Virtual Network
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5ff685960c5690e1bdda47742d81ec44a38aeb8b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181681"
---
# <a name="azure-virtual-network-integration"></a>Integración de Azure Virtual Network

>Se aplica a: Windows Server 2016 Essentials

A medida que las organizaciones hacen su forma de computación en la nube, rara vez moverán todos los recursos del 100% al mismo tiempo, sino que adoptarán un enfoque en el que algunos recursos estén en la nube y otros aún en el entorno local. Este enfoque híbrido facilita a las organizaciones no solo a mover algunos recursos informáticos a la nube, sino que también les permite aumentar su infraestructura de TI sin tener que adquirir hardware nuevo.

Cuando se implementa este enfoque híbrido en informática, se requiere una manera sin problemas de que los recursos en ambas ubicaciones se comuniquen entre sí. La red virtual de Azure es un servicio de Azure que permite a las organizaciones crear una red privada virtual de punto a punto (P2P) o de sitio a sitio (S2S) que hace que los recursos que se ejecutan en Azure (como las máquinas virtuales y el almacenamiento) parezcan estar en la red local para obtener acceso a recursos y aplicaciones sin problemas.

La configuración de una red virtual de Azure puede ser compleja. Con Windows Server Essentials 2016 puede configurar fácilmente la red virtual de Azure mediante un sencillo asistente que le ayuda a elegir los valores predeterminados más adecuados para su entorno en red. Como se muestra en la captura de pantalla siguiente, se ha agregado una nueva tarea de integración de red virtual de Azure a la sección Microsoft Cloud Services del panel de Windows Essentials para introducir la red virtual de Azure y proporcionar un vínculo rápido para iniciar la integración.

![Captura de pantalla que muestra la pestaña introducción en la Página principal del panel de Windows Server Essentials. En la pestaña introducción, se ha seleccionado la sección servicios y el panel indica en Microsoft Cloud integración de servicios que Azure Virtual Network está actualmente deshabilitada.](media/azure-virtual-network-1.PNG)

Al hacer clic en el vínculo **integrar ahora** para la red virtual de Azure en la captura de pantalla anterior, aparecerá un cuadro de diálogo en el que se le pedirá que inicie sesión en su cuenta de Microsoft Azure. Si no tiene una cuenta de Microsoft Azure, tendrá la opción de suscribirse a una en esta pantalla, lo que le redirigirá al portal de registro de la cuenta de Azure:

![Una captura de pantalla que muestra la página iniciar sesión en Microsoft Azure del Asistente para la integración con Azure Virtual Network.](media/azure-virtual-network-2.PNG)

Una vez que inicie sesión en Azure, se le presentará la opción de elegir qué suscripción desea asociar con el servicio de red virtual de Azure:

![Una captura de pantalla que muestra la página mi Microsoft Azure suscripción del Asistente para la integración con Azure Virtual Network.](media/azure-virtual-network-3.PNG)

Una vez que haya elegido la suscripción de Azure que quiere usar para la red virtual de Azure, se le presentará la opción de crear una nueva red virtual de Azure, o bien, si ya se ha configurado una en esta suscripción, el cuadro desplegable mostrará que está disponible. También debe elegir un nombre para la red local que la red virtual de Azure usará para identificar los recursos en la red local. Por último, deberá elegir la región de Azure en la que desea hospedar la red virtual de Azure. La elección de una ubicación físicamente más cercana a la red local suele ser mejor para optimizar la velocidad de ancho de banda para la comunicación con los recursos que puede hospedar en sus servicios de Azure:

![Una captura de pantalla que muestra la página configurar red virtual de Azure del Asistente para la integración con Azure Virtual Network.](media/azure-virtual-network-4.PNG)

El último paso del proceso de integración es configurar el dispositivo VPN que se usará para la conexión VPN S2S. Dado que la mayoría de las pequeñas empresas tienen solo unos pocos servidores en su entorno y carecen del personal de TI para configurar correctamente un enrutador VPN para conectarse a Microsoft Azure, la selección predeterminada será configurar el servidor de Windows Server Essentials como el servidor VPN al que se conectarán los recursos de la red local para tener acceso a los recursos de la red virtual de Azure. Sin embargo, si prefiere usar otro servidor en su entorno como el servidor VPN o prefiere usar un enrutador VPN, puede seleccionar esas opciones.

Debido a la variación en los modelos y tipos de enrutadores, Windows Server Essentials no intenta configurar automáticamente el enrutador VPN. La selección del enrutador VPN en el Asistente para la integración solo notifica a la red virtual de Azure del tipo de dispositivo las configuraciones de enrutamiento adecuadas necesarias en Azure para la conectividad.

Al completar el Asistente para la integración, se mostrará una nueva pestaña en el panel de Windows Server Essentials para la red virtual de Azure:

![Una captura de pantalla que muestra la página red virtual de Azure del panel de Windows Server Essentials. La pestaña red virtual de Azure está seleccionada y muestra el estado en configuración.](media/azure-virtual-network-5.PNG)

>! Nota: completar la configuración de una red virtual de Azure en la nube puede tardar mucho tiempo, hasta 30 minutos. Durante este tiempo, el estado de la configuración estará presente en la página estado de Azure Virtual Network del panel.

Una vez completada la configuración de la red virtual de Azure, el estado cambiará a conectado y mostrará los detalles de la red virtual de Azure, como los datos de salida y de salida, la dirección IP de puerta de enlace, la dirección IP local y los detalles de la cuenta:

![Una captura de pantalla que muestra la página red virtual de Azure del panel de Windows Server Essentials. La pestaña red virtual de Azure está seleccionada y muestra el estado conectado y, en esta información de estado, se muestran los detalles de la red virtual.](media/azure-virtual-network-6.PNG)

En el panel de tareas del lado derecho del panel se encuentran las diversas tareas que puede realizar con la red virtual de Azure.

-   **Desconexión de la red virtual de Azure** La configuración de una red virtual de Azure es gratuita, pero hay un cargo por la puerta de enlace de VPN que se conecta a local y a otras redes virtuales en Azure. La desconexión de la red virtual de Azure detiene toda la facturación.

-   **Cambiar a través de un dispositivo VPN** En el caso de que desee cambiar de un servidor VPN a un enrutador VPN, esta tarea le permitirá hacer el cambio y notificar a la red virtual de Azure.

-   **Configuración de la red virtual de Azure** Esta tarea permite cambiar las opciones de configuración avanzada de la red virtual de Azure redirigiendo a la página de configuración de Azure Portal de la red virtual de Azure.

-   **Actualizar estado** Actualiza la página de estado, actualizando el estado de conexión de la red virtual de Azure, incluidos los datos de salida.

-   **Deshabilitación de Azure integración con red virtual** Desconecta la red virtual de Azure y quita la integración del panel de Windows Server Essentials. Tenga en cuenta que esto no elimina la red virtual de Azure, por lo que la configuración se conserva en Azure si posteriormente quiere volver a integrar la red virtual de Azure con el panel.

-   **Más información acerca de la red virtual de Azure** [https://azure.microsoft.com/services/virtual-network/](https://azure.microsoft.com/services/virtual-network/) .

<a name="see-also"></a>Consulta también
--------
[Introducción a Windows Server Essentials](get-started.md)
