---
title: Crear certificados raíz para la autenticación de VPN con Azure AD
description: Azure AD usa el certificado VPN para firmar los certificados emitidos para clientes de Windows 10 al autenticarse en Azure AD para la conectividad VPN. El certificado marcado como principal es el emisor que usa Azure AD.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 40403f6be65c84cc1e2506a222a009400fcdaacc
ms.sourcegitcommit: 34232723f15c7b4d6a32ca38b7348a417ba30ae0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464956"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Paso 7.2. Crear certificados raíz para la autenticación de VPN de acceso condicional con Azure AD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Siguiente:** Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, configurará los certificados raíz de acceso condicional para la autenticación de VPN con Azure AD, que crea automáticamente una aplicación en la nube llamada servidor VPN en el inquilino. Para configurar el acceso condicional para la conectividad VPN, deberá:

1. Crear un certificado VPN en Azure portal.
2. Descargue el certificado VPN.
3. Implementar el certificado en los servidores VPN y NPS.

> [!IMPORTANT]
> Una vez creado un certificado VPN en Azure portal, Azure AD comenzará inmediatamente a usarlo para emitir certificados de duración breves para el cliente de VPN. Es fundamental que el certificado VPN implementará inmediatamente en el servidor VPN para evitar problemas con la validación de la credencial del cliente VPN.

Cuando un usuario intenta una conexión VPN, el cliente VPN realiza una llamada en el Administrador de cuenta de Web (WAM) en el cliente de Windows 10. WAM realiza una llamada a la aplicación de servidor VPN en la nube. Cuando se cumplen las condiciones y los controles de la directiva de acceso condicional, Azure AD emite un token en forma de un certificado de (1 hora) de corta duración en el Administrador de aplicaciones Web. La WAM coloca el certificado en el almacén de certificados del usuario y pasa el control al cliente VPN.  

El cliente VPN, a continuación, envía los problemas de certificados por Azure AD a la VPN para la validación de credencial.  

> [!NOTE]
> Azure AD usa el certificado creado más recientemente en la hoja de la conectividad VPN como el emisor.

**Procedimiento:**

1. Inicie sesión en su [portal Azure](https://portal.azure.com) como administrador global.
2. En el menú izquierdo, haga clic en **Azure Active Directory**.
3. En el **Azure Active Directory** página, en el **administrar** sección, haga clic en **acceso condicional**.
4. En el **acceso condicional** página, en el **administrar** sección, haga clic en **conectividad VPN (versión preliminar)** .
5. En el **conectividad VPN** página, haga clic en **nuevo certificado**.
6. En el **New** , realice los pasos siguientes: una. Para **seleccionar duración**, seleccione 1, 2 o 3 años.
   b. Selecciona **Crear**.

## <a name="next-steps"></a>Pasos siguientes

[Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md): En este paso, configure la directiva de acceso condicional para la conectividad VPN.
