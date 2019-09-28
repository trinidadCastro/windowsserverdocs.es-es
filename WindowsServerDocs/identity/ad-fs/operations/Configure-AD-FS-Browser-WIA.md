---
title: Configurar exploradores para usar la autenticación integrada de Windows (WIA) con AD FS
description: En este documento se describe cómo configurar los exploradores para que usen WIA con AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 576ec407e244485441e99ed831b4ed9a0dac198c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358253"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurar exploradores para usar la autenticación integrada de Windows (WIA) con AD FS

De forma predeterminada, la autenticación integrada de Windows (WIA) está habilitada en Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2 para las solicitudes de autenticación que se producen dentro de la red interna de la organización (Intranet) para cualquier aplicación que use un explorador para su autenticación.

AD FS 2016 ahora tiene una configuración predeterminada mejorada que permite que el explorador de Edge realice WIA y no también (incorrectamente) la detección de Windows Phone:

    =~Windows\s*NT.*Edge

Lo anterior significa que ya no tiene que configurar cadenas de agente de usuario individuales para admitir escenarios perimetrales comunes, aunque se actualicen con bastante frecuencia.

Para otros exploradores, configure la propiedad AD FS **WiaSupportedUserAgents** para agregar los valores necesarios en función de los exploradores que esté usando.  Puede usar los procedimientos siguientes.



### <a name="view-wiasupporteduseragent-settings"></a>Ver la configuración de WIASupportedUserAgent
**WIASupportedUserAgents** define los agentes de usuario que admiten WIA. AD FS analiza la cadena del agente de usuario al realizar inicios de sesión en un explorador o control del explorador.

Puede ver la configuración actual mediante el siguiente ejemplo de PowerShell:

```powershell
    Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Compatibilidad con WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Cambiar la configuración de WIASupportedUserAgent
De forma predeterminada, una nueva instalación de AD FS tiene un conjunto de coincidencias de cadena de agente de usuario creadas. Sin embargo, es posible que no estén actualizados según los cambios en los exploradores y dispositivos. En particular, los dispositivos de Windows tienen cadenas de agente de usuario similares con pequeñas variaciones en los tokens. El siguiente ejemplo de Windows PowerShell proporciona la mejor orientación para el conjunto actual de dispositivos que se encuentran en el mercado hoy en día que admiten WIA sin problemas:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

El comando anterior garantizará que AD FS solo cubre los siguientes casos de uso para WIA:

Agentes de usuario|Casos de uso|
-----|-----|
MSIE 6,0|IE 6,0|
MSIE 7,0; Windows NT|IE 7, IE en la zona de intranet. El sistema operativo de escritorio envía el fragmento "Windows NT".|
MSIE 8,0|IE 8,0 (ningún dispositivo envía esto, por lo que debe hacer más específico)|
MSIE 9,0|IE 9,0 (ningún dispositivo envía esto, por lo que no es necesario hacer esto más específico)|
MSIE 10,0; Windows NT 6|IE 10,0 para Windows XP y versiones más recientes del sistema operativo de escritorio</br></br>Los dispositivos Windows Phone 8,0 (con la preferencia establecida en móvil) se excluyen porque envían</br></br>Agente de usuario: Mozilla/5.0 (compatible; MSIE 10,0; Windows Phone 8,0; Trident/6.0; IEMobile/10.0; TOPOLOGÍA Entradas ADMITI Lumia 920)|
Windows NT 6,3; Trident/7.0</br></br>Windows NT 6,3; Win64 bits Trident/7.0</br></br>Windows NT 6,3; WOW64 Trident/7.0| Sistema operativo de escritorio Windows 8.1, distintas plataformas|
Windows NT 6,2; Trident/7.0</br></br>Windows NT 6,2; Win64 bits Trident/7.0</br></br>Windows NT 6,2; WOW64 Trident/7.0|Sistema operativo de escritorio de Windows 8, distintas plataformas|
Windows NT 6,1; Trident/7.0</br></br>Windows NT 6,1; Win64 bits Trident/7.0</br></br>Windows NT 6,1; WOW64 Trident/7.0|Sistema operativo de escritorio de Windows 7, distintas plataformas|
MSIPC| Cliente de Microsoft Information Protection and Control|
Cliente de Windows Rights Management|Cliente de Windows Rights Management|
