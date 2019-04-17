---
title: Configurar la directiva de acceso condicional
description: Después de crear un certificado raíz, la conectividad VPN de' ' desencadena la creación de la aplicación en la nube de 'Servidor VPN' en el inquilino del cliente.
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
ms.openlocfilehash: 8c00855c50de79efa1b48c7b8762e1b679db4a87
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067472"
---
# Paso 7.3. Configurar la directiva de acceso condicional

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** paso 7.2. Crear certificados raíz para la autenticación de VPN con Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
& #187; [ **Siguiente:** paso 7.4. Implementar certificados raíz de acceso condicional en locales AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

En este paso, se configura la directiva de acceso condicional para la conectividad de VPN. Cuando se crea el primer certificado raíz en la hoja de 'Conectividad VPN', crea automáticamente una aplicación en la nube de 'Servidor VPN' en el inquilino. 

Crear una directiva de acceso condicional que está asignada al grupo de usuarios VPN y ámbito de la **aplicación en la nube** para el **Servidor VPN**: 

- **Los usuarios**: los usuarios VPN
- **Aplicación en la nube**: servidor VPN
- **Concesión (control de acceso)**: 'Requerir la autenticación multifactor'. Otros controles pueden usarse si lo deseas.

**Procedimiento:** Este paso comprende la creación de la directiva de acceso condicional más básica.Si lo deseas, pueden usarse condiciones y controles adicionales.


1. En la página de **Acceso condicional** , en la barra de herramientas en la parte superior, haz clic en **Agregar**.

    ![Seleccione Agregar en la página de acceso condicional](../../media/Always-On-Vpn/07.png)

2. En la página de **nuevo** , en el cuadro **nombre** , escribe un nombre para la directiva. Por ejemplo, escribe la **Directiva de VPN**.

    ![Agrega el nombre de directiva en la página de acceso condicional](../../media/Always-On-Vpn/08.png)

3. En la sección de **asignación** , haz clic en **los usuarios y grupos**.

    ![Seleccionar los usuarios y grupos](../../media/Always-On-Vpn/09.png)

4. En la página de **usuarios y grupos** , realice los pasos siguientes:

    ![Usuario de selección de prueba](../../media/Always-On-Vpn/10.png)

    a. Haz clic en **Seleccionar usuarios y grupos**.

    b. Haga clic en **Seleccionar**.

    c. En la página **Seleccionar** , selecciona el grupo de **usuarios de VPN** y, a continuación, haga clic en **Seleccionar**.

    d. En la página de **usuarios y grupos** , haz clic en **Listo**.

5. En la página de **nuevo** , realiza los siguientes pasos:

    ![Selecciona las aplicaciones en la nube](../../media/Always-On-Vpn/11.png)

    a. En la sección de **asignaciones** , haz clic en **las aplicaciones en la nube**.

    b. En la página de **aplicaciones en la nube** , haz clic en **seleccionar aplicaciones**.

    d. Selecciona el **servidor VPN**.

13. En la página de **nuevo** , para abrir la página de **concesión** , en la sección de **controles** , haga clic en **concesión**.

    ![Concesión de selección](../../media/Always-On-Vpn/13.png)

14. En la página de **concesión** , realiza los siguientes pasos:

    ![Seleccione Requerir la autenticación multifactor](../../media/Always-On-Vpn/14.png)

    a. Seleccione **requerir la autenticación multifactor**.

    b. Haga clic en **Seleccionar**.

15. En la página de **nuevo** , en **Habilitar la directiva**, haz clic **en**.

    ![Habilitar la directiva](../../media/Always-On-Vpn/15.png)

16. En la página de **nuevo** , haz clic en **crear**.


## Paso siguiente
Paso [7.4. Implementar certificados raíz de acceso condicional en locales AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): en este paso, se implementa el certificado raíz de acceso condicional como certificado raíz de confianza para la autenticación de VPN en tus locales AD.

---