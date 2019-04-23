---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configurar la autenticación basada en formularios de intranet para los dispositivos que no son compatibles con WIA
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cddc5d890114dec7e0053b16701db6f03c3cbbdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889856"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configurar la autenticación basada en formularios de intranet para los dispositivos que no son compatibles con WIA

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

De forma predeterminada, está habilitada la autenticación integrada de Windows (WIA) en Active Directory Federation Services (AD FS) en Windows Server 2012 R2 para las solicitudes de autenticación que se producen dentro de la red interna de la organización (intranet) para cualquier aplicación que usa un explorador para su autenticación. Por ejemplo, pueden ser aplicaciones basadas en explorador que use WS-Federation o SAML protocolos y aplicaciones enriquecidas que utilizan el protocolo OAuth. WIA proporciona a los usuarios finales con inicio de sesión sin problemas a las aplicaciones sin tener que escribir sus credenciales de forma manual. Sin embargo, algunos exploradores y dispositivos no son capaces de admitir WIA y como resultado errores de solicitudes de autenticación desde estos dispositivos. Además, la experiencia en algunos exploradores que negociar a NTLM no es deseable. El enfoque recomendado es la reserva para la autenticación basada en formularios para estos dispositivos y exploradores.

AD FS en Windows Server 2016 y Windows Server 2012 R2 proporciona los administradores la posibilidad de configurar la lista de agentes de usuario que admiten la reserva para la autenticación basada en formularios. La reserva se realiza mediante dos configuraciones:


- El **WIASupportedUserAgentStrings** propiedad de la `Set-ADFSProperties` commandlet
- El **WindowsIntegratedFallbackEnabled** propiedad de la `Set-AdfsGlobalAuthenticationPolicy` commandlet

El **WIASupportedUserAgentStrings** define los agentes de usuario que son compatibles con WIA. AD FS analiza la cadena de agente de usuario al realizar inicios de sesión en un explorador o el control del explorador. Si el componente de la cadena de agente de usuario no coincide con cualquiera de los componentes de las cadenas de agente de usuario que se configuran en **WIASupportedUserAgentStrings** propiedad, AD FS recurrirá a lo que proporciona autenticación basada en formularios, siempre que el **WindowsIntegratedFallbackEnabled** marca está establecida en True.

De forma predeterminada, una nueva instalación de AD FS tiene un conjunto de coincidencias de la cadena de agente usuario creado. Sin embargo, estos pueden ser actualizados según los cambios realizados en los exploradores y dispositivos. Especialmente, los dispositivos de Windows tienen cadenas de agente de usuario similares con pequeñas variaciones en los tokens. El siguiente ejemplo de Windows PowerShell proporciona las instrucciones recomendadas para el conjunto actual de los dispositivos que están en el mercado hoy en día que son compatibles con WIA sin problemas:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

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

Para permitir que la reserva para la autenticación basada en el formulario para los agentes de usuario distintas de las mencionadas en la cadena WIASupportedUserAgents, establezca la marca de WindowsIntegratedFallbackEnabled en true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Asegúrese también de que la autenticación basada en formularios está habilitada para la intranet.

## <a name="configuring-wia-for-chrome"></a>Configuración de WIA para Chrome
Puede agregar a la configuración de AD FS compatibles con WIA Chrome u otros agentes de usuario. Esto permite el inicio de sesión sin problemas a las aplicaciones sin tener que escribir manualmente las credenciales al acceder a recursos protegidos por AD FS. Siga estos pasos para habilitar WIA en Chrome:

Agregar una cadena de agente de usuario para Chrome en configuración de AD FS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Confirme que la cadena de agente de usuario para Chrome ahora está configurada en las propiedades de AD FS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![configuración de la autenticación](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Medida que se publican nuevos exploradores y dispositivos, se recomienda que conciliar las capacidades de los agentes de usuario y actualizar la configuración de AD FS según corresponda para optimizar la experiencia de autenticación del usuario al usar dicho explorador y dispositivos. Más concretamente, se recomienda que vuelva a evaluar el **WIASupportedUserAgents** configuración de AD FS al agregar un nuevo tipo de dispositivo o explorador a la matriz de compatibilidad de WIA.


