---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: "Configurar los equipos cliente para que confíe en el servidor de federación de cuenta"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar los equipos cliente para que confíe en el servidor de federación de cuenta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que los equipos cliente puedan acceder correctamente federadas aplicaciones con los servicios de federación de Active Directory \(AD FS\), primero debes configurar la configuración de Internet Explorer en cada equipo cliente para que confíe en el explorador del servidor de federación de cuenta. Hacerlo manualmente o mediante la directiva de grupo, según tus preferencias administrativas siguiendo uno de los procedimientos siguientes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configuración de Internet Explorer manualmente  
Puedes usar el siguiente procedimiento para configurar manualmente la configuración de Internet Explorer de cada usuario para admitir federación a través de AD FS. Si varios usuarios usan un solo equipo, completar este procedimiento varias veces: una vez para cada perfil de usuario.  
  
Para realizar este procedimiento, inicia sesión como usuario que tendrán acceso a aplicaciones federadas. Este es un valor específico del profile\. Por lo tanto, requiere que agregar manualmente esta configuración para cada perfil de que existe en un equipo específico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Para configurar manualmente los equipos cliente para que confíe en el servidor de federación de cuenta  
  
1.  En el equipo cliente, inicia Internet Explorer.  
  
2.  En la **herramientas** menú, haz clic en **opciones de Internet**.  
  
3.  En la **seguridad** pestaña, haz clic en el **intranet Local** icono y, a continuación, haz clic en **sitios**.  
  
4.  Haz clic en **avanzadas**y en **agregar este sitio Web a la zona**, escriba el nombre completo del sistema de nombres de dominio \(DNS\) del servidor de federación de cuenta \ (por ejemplo, https:\/\/fs1.fabrikam.com\) y, a continuación, haz clic en **agregar**.  
  
5.  Haz clic en **Aceptar** tres veces.  
  
## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>Configuración de Internet Explorer mediante la directiva de grupo  
Para la mayoría de las implementaciones, te recomendamos que uses la directiva de grupo para insertar la configuración de Internet Explorer adecuada en cada equipo cliente.  
  
Pertenencia a **administradores de dominio** o **administradores**, o equivalente, en los servicios de dominio de Active Directory \(AD DS\) es lo mínimo necesario para completar este procedimiento.  ¿Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>Para configurar equipos cliente para que confíe en el servidor de federación de cuenta mediante la directiva de grupo  
  
1.  En un controlador de dominio en el bosque de la organización de partner de cuenta, inicia el **Group Policy Management** en snap\.  
  
2.  Buscar la \(GPO\) objeto de directiva de grupo apropiado, right\ y haga clic en la base de datos y, a continuación, haz clic en **editar**.  
  
3.  En el árbol de consola, abre **usuario Configuration\\Preferences\\Windows Settings\\Internet Explorer mantenimiento**y, a continuación, haz clic en **seguridad**.  
  
4.  En el panel de detalles, double\ clic **zonas de seguridad y clasificación de contenido**.  
  
5.  En **zonas de seguridad y privacidad**, haz clic en **importar las zonas de seguridad actual y la configuración de privacidad**y, a continuación, haz clic en **modificar la configuración**.  
  
6.  Haz clic en **intranet Local**y, a continuación, haz clic en **sitios**.  
  
7.  En **agregar este sitio Web a la zona**, escribe el nombre DNS completo del servidor de federación de cuenta \ (por ejemplo, https:\/\/fs1.fabrikam.com\), haz clic en **agregar**y, a continuación, haz clic en **cerrar**.  
  
8.  Haz clic en **Aceptar** dos veces para aplicar estos cambios a la directiva de grupo.  
  
