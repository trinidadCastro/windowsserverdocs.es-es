---
title: Configuración previa del enrutador
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bdfa3215b7a2426bcde807119971d99ccc229716
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311574"
---
# <a name="preconfiguring-a-router"></a>Configuración previa del enrutador

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Normalmente, una nueva instalación del sistema operativo requiere un enrutador con conexión a Internet para conectarse a la red interna del cliente. Si proporciona enrutadores como valor adicional con un servidor preconfigurado, puede tomar medidas adicionales para preconfigurar el enrutador con el fin de ofrecer una mejor experiencia de usuario.  
  
 El enrutador debería tener activada la opción DHCP. El servidor debería recibir una dirección IP estática. Para ello puede realizar una reserva DHCP de una dirección IP o asignar una dirección IP que se encuentre fuera del rango de direcciones DHCP.  
  
 También puede preconfigurar las opciones de reenvío de puertos en el enrutador para reenviar puertos específicos desde la conexión externa del enrutador a la dirección del servidor en la red interna. La siguiente tabla muestra la configuración recomendada.  
  
|Ajuste de configuración|Detalles|  
|---------------------------|-------------|  
|DHCP|Activado|  
|Reenvío de puerto|Debe reenviar los siguientes puertos a la dirección del servidor:<br /><br /> -80 (para la configuración hospedada, use solo 443)<br />-443|  
|Compatibilidad con UPnP|Debe habilitar la compatibilidad con UPnP para proporcionar la configuración de enrutador más sencilla para el cliente y la mejor experiencia de cliente durante la instalación.<br /><br /> **ADVERTENCIA:** La arquitectura UPnP puede suponer un riesgo para la seguridad si se deja habilitada.|  
  
 Además de la configuración previa básica del enrutador, complete las tareas que se indican a continuación para ofrecer una experiencia del usuario integrada para la administración del enrutador:  
  
-   Para ampliar el Panel, instale un complemento en el servidor que permita a los usuarios administrar el enrutador a través de una interfaz de usuario personalizada.  
  
-   Amplíe las alertas de estado de forma que las alertas del enrutador se puedan visualizar en el Visor de alertas.  
  
-   Si el enrutador es compatible con varias subredes, la dirección IP del servidor debe entregarse como un servidor DNS a través de DHCP.  
  
-   Si el enrutador tiene una característica de control de acceso integrada para Active DirectoryÂ® Domain Services, puede automatizar la integración de Active Directory durante la configuración inicial del servidor. También debería exponer esta función mediante el complemento de gestión del enrutador en el Panel.  
  
> [!NOTE]
>  Para obtener más información acerca de la configuración de conexiones inalámbricas, consulte [Configure Support for a Wireless Network](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Consulta también  
 [Introducción con el ADK de Windows Server essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)