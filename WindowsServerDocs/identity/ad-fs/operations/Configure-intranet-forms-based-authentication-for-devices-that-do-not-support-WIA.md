---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configuración de la autenticación basada en formularios de la intranet para dispositivos que no admiten WIA
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09172b3fcfcedf0888205d099409647a6e077577
ms.sourcegitcommit: b5c12007b4c8fdad56076d4827790a79686596af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78856359"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configuración de la autenticación basada en formularios de la intranet para dispositivos que no admiten WIA


De forma predeterminada, la autenticación integrada de Windows (WIA) está habilitada en Servicios de federación de Active Directory (AD FS) (AD FS) en Windows Server 2012 R2 para las solicitudes de autenticación que se producen dentro de la red interna de la organización (Intranet) para cualquier aplicación que use un explorador para su autenticación. Por ejemplo, puede tratarse de aplicaciones basadas en explorador que usan protocolos de WS-Federation o SAML y aplicaciones enriquecidas que usan el protocolo OAuth. WIA proporciona a los usuarios finales un inicio de sesión sin problemas en las aplicaciones sin tener que escribir sus credenciales manualmente. Sin embargo, algunos dispositivos y exploradores no son capaces de admitir WIA y, como consecuencia, se produce un error en las solicitudes de autenticación de estos dispositivos. Además, no se recomienda la experiencia en determinados exploradores que negocien con NTLM. El enfoque recomendado es la reserva a la autenticación basada en formularios para estos dispositivos y exploradores.

AD FS en Windows Server 2016 y Windows Server 2012 R2 proporcionan a los administradores la capacidad de configurar la lista de agentes de usuario que admiten la reserva para la autenticación basada en formularios. La reserva se hace posible mediante dos configuraciones:


- La propiedad **WIASupportedUserAgentStrings** de la `Set-ADFSProperties` commandlet
- La propiedad **WindowsIntegratedFallbackEnabled** de la `Set-AdfsGlobalAuthenticationPolicy` commandlet

**WIASupportedUserAgentStrings** define los agentes de usuario que admiten WIA. AD FS analiza la cadena del agente de usuario al realizar inicios de sesión en un explorador o control del explorador. Si el componente de la cadena de agente de usuario no coincide con ninguno de los componentes de las cadenas de agente de usuario configuradas en la propiedad **WIASupportedUserAgentStrings** , AD FS revertirá para proporcionar la autenticación basada en formularios, siempre que la marca **WindowsIntegratedFallbackEnabled** esté establecida en true.

De forma predeterminada, una nueva instalación de AD FS tiene un conjunto de coincidencias de cadena de agente de usuario creadas. Sin embargo, es posible que no estén actualizados según los cambios en los exploradores y dispositivos. En particular, los dispositivos de Windows tienen cadenas de agente de usuario similares con pequeñas variaciones en los tokens. El siguiente ejemplo de Windows PowerShell proporciona la mejor orientación para el conjunto actual de dispositivos que se encuentran en el mercado hoy en día que admiten WIA sin problemas:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

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

Para habilitar la reserva a la autenticación basada en formularios para los agentes de usuario distintos de los mencionados en la cadena WIASupportedUserAgents, establezca la marca WindowsIntegratedFallbackEnabled en true.

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Asegúrese también de que la autenticación basada en formularios está habilitada para la intranet.

## <a name="configuring-wia-for-chrome"></a>Configuración de WIA para Chrome
Puede Agregar cromo u otros agentes de usuario a la configuración de AD FS que admita WIA. Esto permite el inicio de sesión sin problemas en las aplicaciones sin tener que escribir manualmente las credenciales al acceder a los recursos protegidos por AD FS. Siga los pasos que se indican a continuación para habilitar WIA en Chrome:

En AD FS configuración, agregue una cadena de agente de usuario para Chrome en plataformas basadas en Windows:

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Windows NT)")

Y, de forma similar, para Chrome en Apple macOS, agregue la siguiente cadena de agente de usuario a la configuración de AD FS:

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Macintosh; Intel Mac OS X)")

Confirme que la cadena de agente de usuario para Chrome ahora está establecida en el AD FS propiedades:

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

(Aquí necesitará una nueva captura de pantalla) ![configurar la autenticación](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> A medida que se lancen nuevos exploradores y dispositivos, se recomienda reconciliar las capacidades de esos agentes de usuario y actualizar la configuración de AD FS en consecuencia para optimizar la experiencia de autenticación del usuario cuando se usa el explorador y los dispositivos. Más concretamente, se recomienda volver a evaluar la configuración **WIASupportedUserAgents** en AD FS al agregar un nuevo tipo de dispositivo o explorador a la matriz de compatibilidad para WIA.

