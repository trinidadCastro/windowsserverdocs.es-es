---
title: Preconfigurar un enrutador
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7dc66c8a439552c2087d0348b0115adba04027ee
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="preconfiguring-a-router"></a>Preconfigurar un enrutador

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Por lo general, una nueva instalación del sistema operativo requiere un enrutador capaz de Internet y del firewall para conectar la red interna del cliente a Internet. Si proporcionas un enrutador como valor adicional con un servidor preconfigurado, se pueden realizar pasos adicionales para preconfigurar el enrutador para proporcionar una mejor experiencia de usuario.  
  
 El enrutador debe tener DHCP activado. El servidor debe asignar una dirección IP estática. Puedes hacerlo mediante una reserva DHCP de una dirección IP o asignando una dirección IP que está fuera del intervalo de direcciones DHCP.  
  
 También debe preconfigura las opciones de configuración del enrutador para reenviar puertos específicos desde la interfaz externa del enrutador a la dirección del servidor en la red interna de enrutamiento de puertos. La siguiente tabla enumera la configuración recomendada.  
  
|Opción de configuración|Detalles|  
|---------------------------|-------------|  
|DHCP|En|  
|Enrutamiento de puertos|Reenviar los siguientes puertos a la dirección del servidor:<br /><br /> -80 (para la configuración hospedada, usa solo 443)<br />-   443|  
|Compatibilidad con UPnP|Debes habilitar la compatibilidad con ¢ de UPnP proporcionar la configuración de enrutador más sencilla para el cliente y la mejor experiencia posible durante la instalación.<br /><br /> **Advertencia:** la arquitectura de UPnP puede suponer un riesgo de seguridad si izquierda está habilitado.|  
  
 Además de la configuración de preconfiguración del enrutador básica, puede realizar las siguientes tareas para proporcionar una experiencia de usuario más integrada para administrar el enrutador:  
  
-   Amplía el panel proporcionando un complemento en el servidor que permite a los usuarios administrar el enrutador a través de una interfaz de usuario personalizada.  
  
-   Amplía las alertas de estado para que las alertas del enrutador pueden verse en el Visor de alertas.  
  
-   Si el enrutador admite varias subredes, la dirección IP del servidor debe distribuirse como un servidor DNS a través de DHCP.  
  
-   Si el enrutador tiene una función de control de acceso integrado para los servicios de dominio de Active DirectoryÂ (r), puede automatizar la integración de Active Directory durante la configuración inicial del servidor. También debes exponer esta característica mediante el complemento Administración de enrutador en el panel.  
  
> [!NOTE]
>  Para obtener más información acerca de cómo configurar conexiones inalámbricas, consulte [configurar la compatibilidad con una red inalámbrica](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Consulta también  
 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)