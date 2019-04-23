---
title: Configurar los exploradores para utilizar autenticación integrada de Windows (WIA) con AD FS
description: Este documento describe cómo configurar los exploradores para utilizar WIA con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f71680bb721635bd37197dca9d3ae4726099525f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845486"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurar los exploradores para utilizar autenticación integrada de Windows (WIA) con AD FS

De forma predeterminada, está habilitada la autenticación integrada de Windows (WIA) en Active Directory Federation Services (AD FS) en Windows Server 2012 R2 para las solicitudes de autenticación que se producen dentro de la red interna de la organización (intranet) para cualquier aplicación que usa un explorador para su autenticación.

AD FS 2016 ahora tiene una configuración predeterminada mejorada que permite que el explorador Microsoft Edge realizar WIA mientras no también se capturan (incorrectamente) de Windows Phone, así:

    =~Windows\s*NT.*Edge

Lo anterior significa que ya no tendrá que configurar cadenas de agente de usuario individual para admitir escenarios comunes de Edge, aunque se actualizan con bastante frecuencia.

Para otros exploradores, configure la propiedad de AD FS **WiaSupportedUserAgents** para agregar los valores necesarios en función de los exploradores que está usando.  Puede usar los procedimientos siguientes.



### <a name="view-wiasupporteduseragent-settings"></a>Ver la configuración de WIASupportedUserAgent
El **WIASupportedUserAgents** define los agentes de usuario que son compatibles con WIA. AD FS analiza la cadena de agente de usuario al realizar inicios de sesión en un explorador o el control del explorador.

Puede ver la configuración actual mediante el siguiente ejemplo de PowerShell:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Compatibilidad con WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Cambiar la configuración de WIASupportedUserAgent
De forma predeterminada, una nueva instalación de AD FS tiene un conjunto de coincidencias de la cadena de agente usuario creado. Sin embargo, estos pueden ser actualizados según los cambios realizados en los exploradores y dispositivos. Especialmente, los dispositivos de Windows tienen cadenas de agente de usuario similares con pequeñas variaciones en los tokens. El siguiente ejemplo de Windows PowerShell proporciona las instrucciones recomendadas para el conjunto actual de los dispositivos que están en el mercado hoy en día que son compatibles con WIA sin problemas:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

El comando anterior se asegurará de que AD FS solo cubre los siguientes casos de uso para WIA:

Agentes de usuario|Casos de uso|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0; Windows NT|IE 7, Internet Explorer en la zona de intranet. El fragmento "Windows NT" se envía al sistema operativo de escritorio.|
MSIE 8.0|Internet Explorer 8.0 (no hay ningún dispositivo enviar este, por lo que deberá hacer más específico)|
MSIE 9.0|9.0 de Internet Explorer (no hay ningún dispositivo enviar esto, no es necesario hacerlo más específico)|
MSIE 10.0; Windows NT 6|Internet Explorer 10.0 para Windows XP y versiones más recientes del sistema operativo de escritorio</br></br>Se excluyen los dispositivos Windows Phone 8.0 (con preferencia definida a dispositivos móviles) porque envían</br></br>User-Agent: Mozilla/5.0 (compatible con; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Entrada táctil; NOKIA; Lumia 920)|
Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Sistema de operativo de escritorio de Windows 8.1, distintas plataformas|
Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema de operativo de escritorio de Windows 8, distintas plataformas|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema de operativo de escritorio de Windows 7, distintas plataformas|
MSIPC| Cliente de Control y protección de la información de Microsoft|
Cliente de Windows Rights Management|Cliente de Windows Rights Management|
