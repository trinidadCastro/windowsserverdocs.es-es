---
title: Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)
description: No se puede conectar un cliente EAP-TLS, a menos que el servidor NPS complete una comprobación de revocación de la cadena de certificados (incluido el certificado raíz) del cliente y comprueba que se han revocado los certificados.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 781239f45b9b260b7d374c2a6972cdb8faad2879
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749602"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7. (Opcional) Acceso condicional para la conectividad VPN con Azure AD](ad-ca-vpn-connectivity-windows10.md)
- [**Siguiente:** Paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Error al implementar este cambio del registro hará que las conexiones de IKEv2 mediante certificados en la nube con PEAP para producir un error, pero las conexiones IKEv2 mediante certificados de autenticación de cliente emitidos desde la CA local seguiría funcionando.

En este paso, puede agregar **IgnoreNoRevocationCheck** y configúrelo para permitir la autenticación de clientes cuando el certificado no incluye puntos de distribución CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

>[!NOTE]
>Si un servidor de acceso remoto (RRAS) y de enrutamiento de Windows usa NPS al proxy RADIUS, las llamadas a un segundo NPS, debe establecer **IgnoreNoRevocationCheck = 1** en ambos servidores.

No se puede conectar un cliente EAP-TLS, a menos que el servidor NPS complete una comprobación de revocación de la cadena de certificados (incluido el certificado raíz). Certificados en la nube Azure AD emitidos para el usuario no tiene una CRL porque son los certificados de corta duración con una duración de una hora. EAP en NPS debe configurarse para que omita la ausencia de una CRL. De forma predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado). Agregue IgnoreNoRevocationCheck y establézcalo en 1 para permitir la autenticación de clientes cuando el certificado no incluye puntos de distribución CRL. 

Puesto que el método de autenticación es EAP-TLS, solo se necesita este valor del registro bajo EAP\13. Si se utilizan otros métodos de autenticación EAP, a continuación, el valor del registro debe agregarse en los que también. 

**Procedimiento**

1. Abra **regedit.exe** en el servidor NPS.

2. Vaya a **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Seleccione **Editar > New** y seleccione **valor DWORD (32 bits)** y escriba **IgnoreNoRevocationCheck**.

4. Haga doble clic en **IgnoreNoRevocationCheck** y establezca el valor **1**.

5. Seleccione **Aceptar** y reinicie el servidor. Reiniciar los servicios de RRAS y NPS no es suficiente.

Para obtener más información, consulte [cómo habilitar o deshabilitar la comprobación de revocación de certificados (CRL) en los clientes](https://technet.microsoft.com/library/bb680540.aspx).


|Ruta de acceso del Registro  |Extensión EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>Pasos siguientes

[Paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): En este paso, configurará los certificados raíz de acceso condicional para la autenticación de VPN con Azure AD, que crea automáticamente una aplicación de servidor VPN en la nube en el inquilino.
