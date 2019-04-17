---
title: Crear certificados raíz para la autenticación de VPN con Azure AD
description: Azure AD, usa el certificado VPN para firmar los certificados emitidos para los clientes de Windows 10 al autenticar a Azure AD para la conectividad de VPN. El certificado que se marcan como principal es el emisor que usa Azure AD.
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
ms.openlocfilehash: 14ef17ab403cc4e7c9891f4ede48e41c25e8522d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067471"
---
# Paso 7.2. Crear certificados raíz para la autenticación de VPN de acceso condicional con Azure AD

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 7.1. Configurar EAP-TLS para omitir la comprobación de la lista de revocación de certificados (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
& #187; [ **Siguiente:** paso 7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md)

En este paso, los certificados raíz de acceso condicional para la autenticación de VPN se configura con Azure AD, que crea automáticamente una aplicación en la nube, llamada servidor de VPN en el inquilino. Para configurar el acceso condicional para la conectividad de VPN, debes:

1. Crear un certificado VPN en el portal de Azure (puedes crear más de un certificado).
2. Descargar el certificado VPN.
2. Implementar el certificado en los servidores NPS y VPN.

Cuando un usuario intenta una conexión VPN, el cliente VPN realiza una llamada en el Administrador de cuentas Web (WAM) en el cliente de Windows 10. Administrador de aplicaciones Web realiza una llamada a la aplicación en la nube de servidor VPN. Cuando se cumplen las condiciones y los controles de la directiva de acceso condicional, Azure AD emite un token en forma de un certificado de corta duración (1 hora) para el Administrador de aplicaciones Web. El Administrador de aplicaciones Web coloca el certificado en el almacén de certificados del usuario y se pasa desactivar el control al cliente VPN.  

El cliente VPN, a continuación, envía los problemas de certificado por Azure AD a la VPN para la validación de credenciales.Azure AD usa el certificado que se marca como**principal**en la hoja de conectividad VPN como el emisor. 

En el portal de Azure, se crean dos certificados para administrar la transición cuando un certificado está a punto de expirar. Al crear un certificado, elegir si se encuentra el certificado principal, que se usa durante la autenticación para firmar el certificado para la conexión.

**Procedimiento:**

1. Inicia sesión como administrador global en el [portal de Azure](https://portal.azure.com) .

2. En el menú izquierdo, haz clic en **Azure Active Directory**. 

    ![Selecciona Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. En la página de **Azure Active Directory** , en la sección **Administrar** , haz clic en **el acceso condicional**.

    ![Selecciona el acceso condicional](../../media/Always-On-Vpn/02.png)

4. En la página de **acceso condicional** , en la sección **Administrar** , haz clic en **la conectividad VPN (versión preliminar)**.

    ![Selecciona la conectividad VPN](../../media/Always-On-Vpn/03.png)

5. En la página de **conectividad VPN** , haz clic en **un nuevo certificado**.

    ![Selecciona el nuevo certificado](../../media/Always-On-Vpn/04.png)

6. En la página de **nuevo** , realiza los siguientes pasos:

    ![Selecciona principales y duración](../../media/Always-On-Vpn/05.png)

    a. Para **Seleccionar la duración**, seleccione 1 o 2 años. Puedes agregar hasta dos certificados para administrar las transiciones cuando el certificado está a punto de expirar. Puedes elegir cuál es el principal (el que uno se usa durante la autenticación para firmar el certificado para la conectividad).

    b. Para **principal**, selecciona **"Sí"**.

    c. Haz clic en **Crear**.

## Paso siguiente
Paso [7.3. Configurar la directiva de acceso condicional](vpn-config-conditional-access-policy.md): en este paso, se configura la directiva de acceso condicional para la conectividad de VPN. 

---