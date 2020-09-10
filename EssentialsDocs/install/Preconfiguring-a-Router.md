---
title: Configuración previa del enrutador
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 79ffa14cfabc26afd87c0771f7412c98e661421d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626150"
---
# <a name="preconfiguring-a-router"></a>Configuración previa del enrutador

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Normalmente, una nueva instalación del sistema operativo requiere un enrutador con conexión a Internet para conectarse a la red interna del cliente. Si proporciona enrutadores como valor adicional con un servidor preconfigurado, puede tomar medidas adicionales para preconfigurar el enrutador con el fin de ofrecer una mejor experiencia de usuario.

 El enrutador debería tener activada la opción DHCP. El servidor debería recibir una dirección IP estática. Para ello puede realizar una reserva DHCP de una dirección IP o asignar una dirección IP que se encuentre fuera del rango de direcciones DHCP.

 También puede preconfigurar las opciones de reenvío de puertos en el enrutador para reenviar puertos específicos desde la conexión externa del enrutador a la dirección del servidor en la red interna. La siguiente tabla muestra la configuración recomendada.

|Opción de configuración|Detalles|
|---------------------------|-------------|
|DHCP|Activado|
|Reenvío de puertos|Debe reenviar los siguientes puertos a la dirección del servidor:<br /><br /> -80 (para la configuración hospedada, use solo 443)<br />-443|
|Compatibilidad con UPnP|Debe habilitar la compatibilidad con UPnP para proporcionar la configuración de enrutador más sencilla para el cliente y la mejor experiencia de cliente durante la instalación.<br /><br /> **ADVERTENCIA:** La arquitectura UPnP puede suponer un riesgo para la seguridad si se deja habilitada.|

 Además de la configuración previa básica del enrutador, complete las tareas que se indican a continuación para ofrecer una experiencia del usuario integrada para la administración del enrutador:

-   Para ampliar el Panel, instale un complemento en el servidor que permita a los usuarios administrar el enrutador a través de una interfaz de usuario personalizada.

-   Amplíe las alertas de estado de forma que las alertas del enrutador se puedan visualizar en el Visor de alertas.

-   Si el enrutador es compatible con varias subredes, la dirección IP del servidor debe entregarse como un servidor DNS a través de DHCP.

-   Si el enrutador tiene una característica de control de acceso integrada para servicios de dominio de Active DirectoryÂ &reg; , puede automatizar la integración de Active Directory durante la configuración inicial del servidor. También debería exponer esta función mediante el complemento de gestión del enrutador en el Panel.

> [!NOTE]
>  Para obtener más información acerca de la configuración de conexiones inalámbricas, consulte [Configure Support for a Wireless Network](Configure-Support-for-a-Wireless-Network.md).

## <a name="see-also"></a>Consulte también
 [Introducción con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md) [crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para](Preparing-the-Image-for-Deployment.md) [probar la implementación de la experiencia del cliente](Testing-the-Customer-Experience.md)