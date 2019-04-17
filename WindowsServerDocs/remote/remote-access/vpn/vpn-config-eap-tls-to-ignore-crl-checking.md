---
title: Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)
description: No se puede conectar a un cliente de EAP-TLS a menos que el servidor NPS realiza una comprobación de revocación de la cadena de certificados (incluido el certificado raíz) del cliente y comprueba que los certificados se han revocado.
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
ms.openlocfilehash: ac59c554c69a6138a106a648c3fab3ed4fe05b7b
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067511"
---
# Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 7. (Opcional) Acceso condicional para la conectividad VPN con Azure AD](ad-ca-vpn-connectivity-windows10.md)<br>
& #187; [ **Siguiente:** paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Error, para implementar este cambio del registro hará que el uso de certificados de la nube con PEAP un error de conexiones de IKEv2, pero conexiones IKEv2 con certificados de autenticación de cliente de la entidad de certificación local seguirá funcionando.

En este paso, puedes agregar **IgnoreNoRevocationCheck** y configurarla para permitir la autenticación de los clientes cuando el certificado no incluye los puntos de distribución CRL. De manera predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado).

>[!NOTE]
>Si un servidor de acceso remoto (RRAS) y de enrutamiento de Windows usa NPS para proxy RADIUS llamadas a un segundo NPS, a continuación, debes establecer **IgnoreNoRevocationCheck = 1** en ambos servidores.

A menos que el servidor NPS realiza una comprobación de revocación de la cadena de certificados (incluido el certificado raíz), no se puede conectar un cliente EAP-TLS. En la nube certificados al usuario por Azure AD no tienen una CRL porque son certificados de corta duración con un ciclo de vida de una hora. EAP en NPS debe estar configurada para omitir la ausencia de una CRL. De manera predeterminada, IgnoreNoRevocationCheck se establece en 0 (deshabilitado). Agregar IgnoreNoRevocationCheck y establécelo en 1 para permitir la autenticación de los clientes cuando el certificado no incluye los puntos de distribución CRL. 

Dado que el método de autenticación es EAP-TLS, este valor del registro solo es necesario con EAP\13. Si se usan otros métodos de autenticación de EAP, a continuación, el valor del registro debe agregarse en ellos también. 

**Procedimiento**

1. Abre **regedit.exe** en el servidor NPS.

2. Ve a **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Haz clic en **Editar > nuevo** y selecciona el **valor DWORD (32 bits)** y escribe **IgnoreNoRevocationCheck**.

4. Haz doble clic en **IgnoreNoRevocationCheck** y establece los datos del valor en **1**.

5. Haz clic en **Aceptar** y reinicie el servidor. Reiniciar los servicios RRAS y NPS no es suficiente.

Para obtener más información, consulta [cómo habilitar o deshabilitar la comprobación de revocación de certificados (CRL) en los clientes](https://technet.microsoft.com/library/bb680540.aspx).


|Ruta de acceso del Registro  |Extensión EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## Paso siguiente

Paso [7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): en este paso, configurar certificados raíz de acceso condicional para la autenticación de VPN con Azure AD, que crea automáticamente una aplicación en la nube de servidor VPN en el inquilino. 

---