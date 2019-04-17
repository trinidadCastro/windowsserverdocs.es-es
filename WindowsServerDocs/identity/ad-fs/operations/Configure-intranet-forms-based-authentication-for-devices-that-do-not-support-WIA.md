---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: "Configurar la autenticación basada en formularios de intranet para los dispositivos que no son compatibles con WIA"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c78dc702cbff8eab487c6c077dfe57b0f0663342
ms.sourcegitcommit: 5012c078b410f59261edf0cc917393467243a5c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2018
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configurar la autenticación basada en formularios de intranet para los dispositivos que no son compatibles con WIA

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

De manera predeterminada, la autenticación integrada de Windows (WIA) está habilitada en los servicios de federación de Active Directory (AD FS) en Windows Server 2012 R2 para solicitudes de autenticación que se produzcan en la red interna de la organización (intranet) para cualquier aplicación que usa un explorador para su autenticación. Por ejemplo, pueden ser aplicaciones basadas en el explorador que usan WS-federación o SAML protocolos y aplicaciones enriquecidas que usan el protocolo OAuth. WIA proporciona al usuario final con inicio de sesión perfecta para las aplicaciones sin tener que especificar manualmente sus credenciales. Sin embargo, algunos dispositivos y exploradores no son capaces de admitir WIA y como resultado no solicitudes de autenticación desde estos dispositivos. Además, la experiencia en determinados exploradores que negociar NTLM no es deseable. El enfoque recomendado es reserva a la autenticación basada en formularios para estos dispositivos y exploradores.

AD FS en Windows Server 2016 y Windows Server 2012 R2 proporciona los administradores la posibilidad de configurar la lista de los agentes de usuario que admiten la reserva a la autenticación basada en formularios. La reserva se realiza mediante dos configuraciones:


- La **WIASupportedUserAgentStrings** propiedad de la `Set-ADFSProperties` cmdlet
- La **WindowsIntegratedFallbackEnabled** propiedad de la `Set-AdfsGlobalAuthenticationPolicy` commmandlet

La **WIASupportedUserAgentStrings** define los agentes de usuario que admiten WIA. AD FS analiza la cadena de agente de usuario cuando se realiza inicios de sesión en un explorador o un control de explorador. Si el componente de la cadena de agente de usuario no coincide con ninguno de los componentes de las cadenas de agente de usuario que están configurados en **WIASupportedUserAgentStrings** propiedad, AD FS recurrirá a proporcionar autenticación basada en formularios, siempre que la **WindowsIntegratedFallbackEnabled** marca está establecida en True.

De manera predeterminada, una nueva instalación de AD FS tiene un conjunto de coincidencias de cadena de agente de usuario creado. Sin embargo, estos pueden estar desactualizados en función de los cambios en los exploradores y dispositivos. En particular, los dispositivos Windows tienen cadenas de agente de usuario similares con pequeñas variaciones en los tokens. El siguiente ejemplo de Windows PowerShell proporciona la mejor orientación para el conjunto actual de dispositivos que están en el mercado actual que admiten WIA transparente:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

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
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema de operativo de escritorio de Windows 7, distintas plataformas|
MSIPC| Protección de la información de Microsoft y el cliente de Control|
Cliente de Windows Rights Management|Cliente de Windows Rights Management|

Para habilitar la reserva para la autenticación de formulario para agentes de usuario que no sean las mencionadas en la cadena WIASupportedUserAgents, Establece la marca WindowsIntegratedFallbackEnabled en true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Asegúrate de que está habilitada la autenticación basada en formularios de intranet.

## <a name="configuring-wia-for-chrome"></a>Configurar WIA cromo
Puedes agregar cromo u otros agentes de usuario a la configuración de AD FS que admite WIA. Esto permite un inicio de sesión perfecta para aplicaciones sin tener que escribir manualmente las credenciales cuando obtienes acceso a recursos protegidos por AD FS. Sigue estos pasos para habilitar WIA en Chrome:

Agregar una cadena de agente de usuario cromo en configuración de AD FS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Comprueba que la cadena de agente de usuario cromo ahora está establecida en las propiedades de AD FS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![configurar la autenticación](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Cuando se publiquen nuevos exploradores y dispositivos, se recomienda que conciliar las capacidades de los agentes de usuario y actualizar la configuración de AD FS según corresponda para optimizar la experiencia del usuario autenticación al uso de dicho explorador y dispositivos. Más concretamente, se recomienda que vuelva a evaluar la **WIASupportedUserAgents** establecer en AD FS al agregar un nuevo tipo de dispositivo o el explorador a la matriz de soporte técnico para WIA.


