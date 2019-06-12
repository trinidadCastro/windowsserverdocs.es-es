---
title: Crear certificados raíz para la autenticación de VPN con Azure AD
description: Azure AD usa el certificado VPN para firmar los certificados emitidos para clientes de Windows 10 al autenticarse en Azure AD para la conectividad VPN. El certificado marcado como principal es el emisor que usa Azure AD.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: dc24f0275e8639ffd972ae24550d0ada38eff4f1
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749638"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>Paso 7.2. Crear certificados raíz para la autenticación de VPN de acceso condicional con Azure AD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**Siguiente:** Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, configurará los certificados raíz de acceso condicional para la autenticación de VPN con Azure AD, que crea automáticamente una aplicación en la nube llamada servidor VPN en el inquilino. Para configurar el acceso condicional para la conectividad VPN, deberá:

1. Crear un certificado VPN en el portal de Azure (también puede crear más de un certificado).
2. Descargue el certificado VPN.
3. Implementar el certificado en los servidores VPN y NPS.

Cuando un usuario intenta una conexión VPN, el cliente VPN realiza una llamada en el Administrador de cuenta de Web (WAM) en el cliente de Windows 10. WAM realiza una llamada a la aplicación de servidor VPN en la nube. Cuando se cumplen las condiciones y los controles de la directiva de acceso condicional, Azure AD emite un token en forma de un certificado de (1 hora) de corta duración en el Administrador de aplicaciones Web. La WAM coloca el certificado en el almacén de certificados del usuario y pasa el control al cliente VPN.  

El cliente VPN, a continuación, envía los problemas de certificados por Azure AD a la VPN para la validación de credencial.  Azure AD usa el certificado que está marcado como **principal** en la hoja de la conectividad VPN como el emisor. 

En el portal de Azure, cree dos certificados para administrar la transición cuando un certificado está a punto de expirar. Cuando se crea un certificado, elegir si es el certificado principal, que se usa durante la autenticación para firmar el certificado para la conexión.

**Procedimiento:**

1. Inicie sesión en su [portal Azure](https://portal.azure.com) como administrador global.

2. En el menú izquierdo, haga clic en **Azure Active Directory**. 

    ![Seleccione Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. En el **Azure Active Directory** página, en el **administrar** sección, haga clic en **acceso condicional**.

    ![Seleccionar acceso condicional](../../media/Always-On-Vpn/02.png)

4. En el **acceso condicional** página, en el **administrar** sección, haga clic en **conectividad VPN (versión preliminar)** .

    ![Seleccionar conectividad VPN](../../media/Always-On-Vpn/03.png)

5. En el **conectividad VPN** página, haga clic en **nuevo certificado**.

    ![Seleccione el nuevo certificado](../../media/Always-On-Vpn/04.png)

6. En el **New** , realice los pasos siguientes:

    ![Seleccione la duración y principal](../../media/Always-On-Vpn/05.png)

    a. Para **seleccionar duración**, seleccione 1 o 2 años. Puede agregar hasta dos certificados para administrar las transiciones cuando el certificado está a punto de expirar. Puede elegir cuál es el principal (aquella usada durante la autenticación para firmar el certificado para la conectividad).

    b. Para **principal**, seleccione **Sí**.

    c. Selecciona **Crear**.

## <a name="next-steps"></a>Pasos siguientes

[Paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md): En este paso, configure la directiva de acceso condicional para la conectividad VPN. 
