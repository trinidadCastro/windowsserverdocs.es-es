---
title: "Configurar exploradores para usar la autenticación integrada de Windows (WIA) con AD FS"
description: "Este documento describe cómo configurar exploradores para usar WIA con AD FS"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7d43931d1fe4958a539ff1b728e4cc154d06248
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurar exploradores para usar la autenticación integrada de Windows (WIA) con AD FS

De manera predeterminada, la autenticación integrada de Windows (WIA) está habilitada en los servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2 para solicitudes de autenticación que se produzcan en la red interna de la organización (intranet) para cualquier aplicación que usa un explorador para su autenticación.

AD FS 2016 ahora tiene una configuración de mejorada predeterminada que permite que el Explorador de Edge realizar WIA mientras no también se capturar (incorrectamente) de Windows Phone, así:

    =~Windows\s*NT.*Edge

Los medios anteriores que ya no tienes que configurar cadenas de agente de usuario individual para admitir escenarios comunes de borde, incluso aunque se actualizan con frecuencia.

Para otros exploradores, configurar la propiedad de AD FS **WiaSupportedUserAgents** para agregar los valores necesarios en función de los exploradores que estás usando.  Puedes usar los procedimientos siguientes.



### <a name="view-wiasupporteduseragent-settings"></a>Configuración de vista WIASupportedUserAgent
La **WIASupportedUserAgents** define los agentes de usuario que admiten WIA. AD FS analiza la cadena de agente de usuario cuando se realiza inicios de sesión en un explorador o un control de explorador.

Puedes ver la configuración actual mediante el siguiente ejemplo de PowerShell:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Soporte WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Cambiar la configuración de WIASupportedUserAgent
De manera predeterminada, una nueva instalación de AD FS tiene un conjunto de coincidencias de cadena de agente de usuario creado. Sin embargo, estos pueden estar desactualizados en función de los cambios en los exploradores y dispositivos. En particular, los dispositivos Windows tienen cadenas de agente de usuario similares con pequeñas variaciones en los tokens. El siguiente ejemplo de Windows PowerShell proporciona la mejor orientación para el conjunto actual de dispositivos que están en el mercado actual que admiten WIA transparente:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

El comando anterior se asegurará de que AD FS solo centra en los siguientes casos de uso de WIA:

Agentes de usuario|Casos de uso|
-----|-----|
MSIE 6.0|INTERNET EXPLORER 6.0|
MSIE 7.0; Windows NT|Internet Explorer 7, IE en la zona de intranet. El fragmento de "Windows NT" se envía al sistema operativo de escritorio.|
MSIE 8.0|IE 8.0 (ningún dispositivo enviar esta información, por lo que necesitas para que sea más específica)|
MSIE 9.0|9.0 De IE (ningún dispositivo enviar esta información, por lo que no necesite que esto sea más específica)|
MSIE 10.0; Windows NT 6|Internet Explorer 10.0 para Windows XP y las versiones más recientes del sistema operativo de escritorio</br></br>Dispositivos de Windows Phone 8.0 (con las preferencias que establece al móvil) se excluyen porque envían</br></br>Agente de usuario: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Entrada táctil NOKIA; Lumia 920)|
Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Sistema de operativo de escritorio de Windows 8.1, distintas plataformas|
Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema de operativo de escritorio de Windows 8, distintas plataformas|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema de operativo de escritorio de Windows 7, platoforms diferentes|
MSIPC| Protección de la información de Microsoft y el cliente de Control|
Cliente de Windows Rights Management|Cliente de Windows Rights Management|
