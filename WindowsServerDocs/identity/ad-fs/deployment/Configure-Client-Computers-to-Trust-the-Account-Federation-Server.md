---
description: Más información acerca de cómo configurar los equipos cliente para que confíen en el servidor de Federación de cuenta
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurar los equipos cliente para que confíen en el servidor de Federación de cuentas
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 9b76fd4bd7ae09e9940dc7501136c5b0bb3449f6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050303"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar los equipos cliente para que confíen en el servidor de Federación de cuentas

Para que los equipos cliente puedan tener acceso correctamente a las aplicaciones federadas mediante Servicios de federación de Active Directory (AD FS) \( AD FS \) , primero debe configurar las opciones de Internet Explorer en cada equipo cliente para que el explorador confíe en el servidor de Federación de la cuenta. Puede hacerlo de forma manual o a través de directiva de grupo, en función de sus preferencias administrativas, completando uno de los procedimientos siguientes.

## <a name="configuring-internet-explorer-settings-manually"></a>Configuración manual de Internet Explorer
Puede usar el siguiente procedimiento para configurar manualmente las opciones de Internet Explorer de cada usuario para que admitan la Federación a través de AD FS. Si varios usuarios usan un solo equipo, realice este procedimiento varias veces, una vez para cada perfil de usuario.

Para llevar a cabo este procedimiento, inicie sesión como el usuario que va a obtener acceso a las aplicaciones federadas. Se trata de una \- configuración específica del perfil. Por lo tanto, es necesario que agregue manualmente esta configuración para cada perfil que exista en un equipo específico.

#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Para configurar manualmente los equipos cliente para que confíen en el servidor de Federación de cuentas

1.  En el equipo cliente, inicie Internet Explorer.

2.  En el menú **herramientas** , haga clic en **Opciones de Internet**.

3.  En la pestaña **seguridad** , haga clic en el icono **Intranet local** y, a continuación, haga clic en **sitios**.

4.  Haga clic en **Opciones avanzadas** y, en **Agregar este sitio web a la zona**, escriba el nombre completo del sistema \( de nombres de dominio DNS \) del servidor de Federación de la cuenta, \( por ejemplo, https: \/ \/ Fs1.fabrikam.com \) y, a continuación, haga clic en **Agregar**.

5.  Haz clic en **Aceptar** tres veces.

## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>Configuración de Internet Explorer mediante directiva de grupo
Para la mayoría de las implementaciones, se recomienda usar directiva de grupo para enviar la configuración de Internet Explorer adecuada a cada equipo cliente.

  \( \) Para completar este procedimiento, lo mínimo que se necesita es pertenecer al grupo Admins. del dominio o administradores de organización, o equivalente, en Active Directory Domain Services AD DS.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .

#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>Para configurar los equipos cliente para que confíen en el servidor de Federación de cuenta mediante directiva de grupo

1.  En un controlador de dominio del bosque de la organización del asociado de cuenta,  inicie el complemento \- de administración de directiva de grupo en.

2.  Busque el objeto de directiva de grupo \( GPO correspondiente \) , haga clic con el botón secundario \- en él y, a continuación, haga clic en **Editar**.

3.  En el árbol de consola, Abra **preferencias de configuración de usuario configuración de \\ \\ Windows mantenimiento de \\ Internet Explorer** y, a continuación, haga clic en **seguridad**.

4.  En el panel de detalles, \- haga doble clic en **zonas de seguridad y clasificación de contenido**.

5.  En **zonas de seguridad y privacidad**, haga clic en **importar la configuración actual de las zonas de seguridad y privacidad** y, a continuación, haga clic en **modificar configuración**.

6.  Haga clic en **Intranet local** y, a continuación, en **sitios**.

7.  En **Agregar este sitio web a la zona**, escriba el nombre DNS completo del servidor de Federación de la cuenta, \( por ejemplo, https: \/ \/ Fs1.fabrikam.com \) , haga clic en **Agregar** y, a continuación, haga clic en **cerrar**.

8.  Haga clic en **Aceptar** dos veces para aplicar estos cambios a Directiva de grupo.

