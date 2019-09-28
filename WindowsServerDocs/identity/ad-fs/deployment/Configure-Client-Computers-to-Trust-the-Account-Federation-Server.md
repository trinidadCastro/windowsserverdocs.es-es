---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurar los equipos cliente para que confíen en el servidor de Federación de cuentas
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8c047f69cb0cb2db57ea33697c49e53a212fe823
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359860"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar los equipos cliente para que confíen en el servidor de Federación de cuentas

Para que los equipos cliente puedan acceder correctamente a las aplicaciones federadas mediante Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1, primero debe configurar las opciones de Internet Explorer en cada equipo cliente para que el explorador confíe en la cuenta. servidor de Federación. Puede hacerlo de forma manual o a través de directiva de grupo, en función de sus preferencias administrativas, completando uno de los procedimientos siguientes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configuración manual de Internet Explorer  
Puede usar el siguiente procedimiento para configurar manualmente las opciones de Internet Explorer de cada usuario para que admitan la Federación a través de AD FS. Si varios usuarios usan un solo equipo, realice este procedimiento varias veces, una vez para cada perfil de usuario.  
  
Para llevar a cabo este procedimiento, inicie sesión como el usuario que va a obtener acceso a las aplicaciones federadas. Se trata de una configuración de perfil @ no__t-0specific. Por lo tanto, es necesario que agregue manualmente esta configuración para cada perfil que exista en un equipo específico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Para configurar manualmente los equipos cliente para que confíen en el servidor de Federación de cuentas  
  
1.  En el equipo cliente, inicie Internet Explorer.  
  
2.  En el menú **herramientas** , haga clic en **Opciones de Internet**.  
  
3.  En la pestaña **seguridad** , haga clic en el icono **Intranet local** y, a continuación, haga clic en **sitios**.  
  
4.  Haga clic en **Opciones avanzadas**y, en **Agregar este sitio web a la zona**, escriba el nombre completo del sistema de nombres de dominio \(DNS @ no__t-3 del servidor de Federación de cuenta \(for ejemplo, https: @no__t -5\/fs1.fabrikam.com @ no__t-7 y, a continuación, haga clic en **Agregar.** .  
  
5.  Haz clic en **Aceptar** tres veces.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configuración de Internet Explorer mediante directiva de grupo  
Para la mayoría de las implementaciones, se recomienda usar directiva de grupo para enviar la configuración de Internet Explorer adecuada a cada equipo cliente.  
  
Para completar este procedimiento, lo mínimo que se necesita es pertenecer a **Admins** . del dominio o **administradores de organización**, o un grupo equivalente, en Active Directory Domain Services \(AD DS @ no__t-3.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en \( [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Para configurar los equipos cliente para que confíen en el servidor de Federación de cuenta mediante directiva de grupo  
  
1.  En un controlador de dominio del bosque de la organización del asociado de cuenta, inicie el **Directiva de grupo Management** Snap @ no__t-1in.  
  
2.  Busque el objeto de directiva de grupo adecuado \(GPO @ no__t-1, a la derecha @ no__t-2click y, a continuación, haga clic en **Editar**.  
  
3.  En el árbol de consola, Abra **configuración de usuario @ no__t-1Preferences @ no__t-2Windows Settings @ no__t-3Internet Explorer Maintenance**y, a continuación, haga clic en **seguridad**.  
  
4.  En el panel de detalles, haga doble @ no__t-0click **zonas de seguridad y clasificación de contenido**.  
  
5.  En **zonas de seguridad y privacidad**, haga clic en **importar la configuración actual de las zonas de seguridad y privacidad**y, a continuación, haga clic en **modificar configuración**.  
  
6.  Haga clic en **Intranet local**y, a continuación, en **sitios**.  
  
7.  En **Agregar este sitio web a la zona**, escriba el nombre DNS completo del servidor de Federación de cuenta \(Para ejemplo, https: @no__t -2\/fs1.fabrikam.com @ no__t-4, haga clic en **Agregar**y, a continuación, haga clic en **cerrar**.  
  
8.  Haga clic en **Aceptar** dos veces para aplicar estos cambios a Directiva de grupo.  
  
