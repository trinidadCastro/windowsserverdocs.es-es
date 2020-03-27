---
title: Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)
description: Un cliente EAP-TLS no puede conectarse a menos que el servidor NPS complete una comprobación de revocación de la cadena de certificados (incluido el certificado raíz) del cliente y compruebe que los certificados se han revocado.
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: c58386467d632d77fdc96bf3bc18b5a85d1ecdaf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307707"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7. Opta Acceso condicional para la conectividad VPN con Azure AD](ad-ca-vpn-connectivity-windows10.md)
- [**Siguiente:** Paso 7,2. Crear certificados raíz para la autenticación VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Si no se implementa este cambio en el registro, se producirá un error en las conexiones IKEv2 que usan certificados de nube con PEAP, pero las conexiones de IKEv2 que usan certificados de autenticación de cliente emitidas desde la CA local seguirán funcionando.

En este paso, puede agregar **IgnoreNoRevocationCheck** y establecerlo para permitir la autenticación de clientes cuando el certificado no incluya puntos de distribución de CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

>[!NOTE]
>Si un servidor de enrutamiento y acceso remoto (RRAS) de Windows usa NPS para proxy llamadas RADIUS a un segundo NPS, debe establecer **IgnoreNoRevocationCheck = 1** en ambos servidores.

Un cliente EAP-TLS no puede conectarse a menos que el servidor NPS complete una comprobación de revocación de la cadena de certificados (incluido el certificado raíz). Los certificados de nube emitidos para el usuario por Azure AD no tienen una CRL porque son certificados de corta duración con una duración de una hora. EAP en NPS debe configurarse para omitir la ausencia de una CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado). Agregue IgnoreNoRevocationCheck y establézcalo en 1 para permitir la autenticación de clientes cuando el certificado no incluya puntos de distribución de CRL. 

Dado que el método de autenticación es EAP-TLS, este valor del registro solo es necesario en EAP\13. Si se usan otros métodos de autenticación de EAP, el valor del registro debe agregarse también en ellos. 

**Pasos**

1. Abra **regedit. exe** en el servidor NPS.

2. Vaya a **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\rasman\ppp\eap\13**.

3. Seleccione **editar > nuevo** y seleccione **el valor DWORD (32 bits)** y escriba **IgnoreNoRevocationCheck**.

4. Haga doble clic en **IgnoreNoRevocationCheck** y establezca los datos del valor en **1**.

5. Seleccione **Aceptar** y reinicie el servidor. El reinicio de los servicios RRAS y NPS no basta.

Para obtener más información, consulte [habilitar o deshabilitar la comprobación de revocación de certificados (CRL) en los clientes](https://technet.microsoft.com/library/bb680540.aspx).


|Ruta de acceso del Registro  |Extensión EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP V2         |

## <a name="next-steps"></a>Pasos siguientes

[Paso 7,2. Crear certificados raíz para la autenticación VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): en este paso, se configuran los certificados raíz de acceso condicional para la autenticación VPN con Azure ad, que crea automáticamente una aplicación de nube de servidor VPN en el inquilino.
