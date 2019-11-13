---
title: Crear certificados raíz para la autenticación de VPN con Azure AD
description: Azure AD usa el certificado VPN para firmar los certificados emitidos para los clientes de Windows 10 al autenticarse en Azure AD para la conectividad VPN. El certificado marcado como principal es el emisor que usa Azure AD.
services: active-directory
ms.prod: windows-server
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 41e98648ab963347f8370233c320f5e38b5d4d96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388006"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Paso 7.2. Crear certificados raíz de acceso condicional para la autenticación de VPN con Azure AD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7,1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Siguiente:** Paso 7,3. Configuración de la Directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, configurará los certificados raíz de acceso condicional para la autenticación VPN con Azure AD, que crea automáticamente una aplicación en la nube llamada servidor VPN en el inquilino. Para configurar el acceso condicional para la conectividad VPN, debe:

1. Cree un certificado VPN en el Azure Portal.
2. Descargue el certificado VPN.
3. Implemente el certificado en los servidores VPN y NPS.

> [!IMPORTANT]
> Una vez creado un certificado VPN en el Azure Portal, Azure AD comenzará a usarlo inmediatamente para emitir certificados de corta duración al cliente VPN. Es fundamental que el certificado VPN se implemente inmediatamente en el servidor VPN para evitar problemas con la validación de credenciales del cliente VPN.

Cuando un usuario intenta una conexión VPN, el cliente VPN realiza una llamada al administrador de cuentas web (WAM) en el cliente de Windows 10. WAM realiza una llamada a la aplicación de nube de servidor VPN. Cuando se cumplen las condiciones y los controles de la Directiva de acceso condicional, Azure AD emite un token en forma de un certificado de corta duración (1 hora) al WAM. El WAM coloca el certificado en el almacén de certificados del usuario y pasa el control al cliente VPN.  

A continuación, el cliente VPN envía los problemas de certificado Azure AD a la VPN para la validación de credenciales.  

> [!NOTE]
> Azure AD usa el certificado creado más recientemente en la hoja de conectividad VPN como emisor.

**Pasos**

1. Inicie sesión en su [Azure portal](https://portal.azure.com) como administrador global.
2. En el menú de la izquierda, haga clic en **Azure Active Directory**.
3. En la página **Azure Active Directory** , en la sección **administrar** , haga clic en **acceso condicional**.
4. En la página **acceso condicional** , en la sección **administrar** , haga clic en **Conectividad VPN (vista previa)** .
5. En la página **Conectividad VPN** , haga clic en **nuevo certificado**.
6. En la página **nuevo** , realice los pasos siguientes: a. En **seleccionar duración**, seleccione 1, 2 o 3 años.
   b. Selecciona **Crear**.

## <a name="next-steps"></a>Pasos siguientes

[Paso 7,3. Configurar la Directiva de acceso condicional](vpn-config-conditional-access-policy.md): en este paso, se configura la Directiva de acceso condicional para la conectividad VPN.
