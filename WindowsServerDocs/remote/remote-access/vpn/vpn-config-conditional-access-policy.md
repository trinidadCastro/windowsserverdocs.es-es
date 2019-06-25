---
title: Configurar la directiva de acceso condicional
description: Una vez creado un certificado raíz, la "conectividad VPN" desencadena la creación de la aplicación de 'Servidor VPN' en la nube en el inquilino del cliente.
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
ms.openlocfilehash: 466e76d01ca99a1e1ed72fa955ccd287ae63c5df
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749499"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>Paso 7.3. Configurar la directiva de acceso condicional

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**Siguiente:** Paso 7.4. Implementar certificados de raíz de acceso condicional en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

En este paso, configure la directiva de acceso condicional para la conectividad VPN. Cuando se crea el primer certificado de raíz en la hoja "Conectividad de VPN", crea automáticamente una aplicación de nube 'Servidor VPN' en el inquilino.

Crear una directiva de acceso condicional que se asigna al grupo de usuarios VPN y el ámbito del **aplicación en la nube** a **servidor VPN**:

- **Usuarios**: Usuarios VPN
- **Aplicación en la nube**: VPN Server
- **GRANT (control de acceso)** : 'Requerir autenticación multifactor'. Si lo desea, pueden usarse otros controles.

**Procedimiento:** Este paso describe la creación de la directiva de acceso condicional más básica.  Si lo desea, pueden usar los controles y condiciones adicionales.


1. En el **acceso condicional** página, en la barra de herramientas en la parte superior, seleccione **agregar**.

    ![Seleccione Agregar en la página de acceso condicional](../../media/Always-On-Vpn/07.png)

2. En el **New** página, en el **nombre** , escriba un nombre para la directiva. Por ejemplo, escriba **Directiva VPN**.

    ![Agregar nombre de directiva en la página de acceso condicional](../../media/Always-On-Vpn/08.png)

3. En el **asignación** sección, seleccione **usuarios y grupos**.

    ![Seleccionar usuarios y grupos](../../media/Always-On-Vpn/09.png)

4. En el **usuarios y grupos** , realice los pasos siguientes:

    ![Usuario de prueba de SELECT](../../media/Always-On-Vpn/10.png)

    a. Seleccione **Seleccionar usuarios y grupos**.

    b. Seleccione **seleccione**.

    c. En el **seleccione** , seleccione el **usuarios de VPN** de grupo y, a continuación, seleccione **seleccione**.

    d. En el **usuarios y grupos** página, seleccione **realiza**.

5. En el **New** , realice los pasos siguientes:

    ![Seleccione las aplicaciones en la nube](../../media/Always-On-Vpn/11.png)

    a. En el **asignaciones** sección, seleccione **aplicaciones en la nube**.

    b. En el **aplicaciones en la nube** página, seleccione **seleccionar aplicaciones**.

    d. Seleccione **servidor VPN**.

6.  En el **New** página para abrir el **Grant** página, en el **controles** sección, seleccione **Grant**.

    ![Seleccionar conceder](../../media/Always-On-Vpn/13.png)

7.  En el **Grant** , realice los pasos siguientes:

    ![Seleccionar requerir autenticación multifactor](../../media/Always-On-Vpn/14.png)

    a. Seleccione **requerir autenticación multifactor**.

    b. Seleccione **seleccione**.

8.  En el **New** página, en **Habilitar directiva**, seleccione **en**.

    ![Habilitar directiva](../../media/Always-On-Vpn/15.png)

9.  En el **New** página, seleccione **crear**.


## <a name="next-steps"></a>Pasos siguientes
[Paso 7.4. Implementar certificados de raíz de acceso condicional en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): En este paso, implementará el certificado raíz de acceso condicional como certificado raíz de confianza para la autenticación de VPN a sus instalaciones en AD.