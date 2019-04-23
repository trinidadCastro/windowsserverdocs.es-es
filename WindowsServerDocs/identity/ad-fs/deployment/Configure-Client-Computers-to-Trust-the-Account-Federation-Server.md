---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurar los equipos cliente para confiar en el servidor de federación de cuenta
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886756"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurar los equipos cliente para confiar en el servidor de federación de cuenta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para que los equipos cliente pueden tener acceso correctamente a las aplicaciones federadas con Active Directory Federation Services \(AD FS\), debe configurar primero las opciones de Internet Explorer en cada equipo cliente para que confíe en el explorador el servidor de federación de cuenta. Puede hacerlo manualmente o mediante la directiva de grupo, dependiendo de sus preferencias administrativas, completando uno de los procedimientos siguientes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configuración de las opciones de Internet Explorer manualmente  
Puede usar el procedimiento siguiente para configurar manualmente la configuración de Internet Explorer de cada usuario para admitir la federación mediante AD FS. Si varios usuarios utilizan un único equipo, complete este procedimiento varias veces: una vez para cada perfil de usuario.  
  
Para llevar a cabo este procedimiento, inicie sesión como usuario que tendrá acceso a aplicaciones federadas. Se trata de un perfil\-configuración específica. Por lo tanto, requiere que agregar manualmente esta configuración para cada perfil que existe en un equipo específico.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Para configurar manualmente los equipos cliente para confiar en el servidor de federación de cuenta  
  
1.  En el equipo cliente, inicie Internet Explorer.  
  
2.  En el **herramientas** menú, haga clic en **opciones de Internet**.  
  
3.  En el **seguridad** pestaña, haga clic en el **intranet Local** icono y, a continuación, haga clic en **sitios**.  
  
4.  Haga clic en **avanzadas**y en **agregar este sitio Web a la zona**, escriba el sistema de nombres de dominio completo \(DNS\) nombre del servidor de federación de cuenta \(por ejemplo, https :\/\/fs1.fabrikam.com\)y, a continuación, haga clic en **agregar**.  
  
5.  Haz clic en **Aceptar** tres veces.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configurar opciones de Internet Explorer mediante la directiva de grupo  
Para la mayoría de las implementaciones, se recomienda que use Directiva de grupo para insertar la configuración adecuada de Internet Explorer en cada equipo cliente.  
  
Pertenencia a **Admins. del dominio** o **administradores de empresas**, o equivalente, en servicios de dominio de Active Directory \(AD DS\) es el requisito mínimo para completar este procedimiento.  ¿Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Para configurar los equipos cliente para confiar en el servidor de federación de cuenta mediante la directiva de grupo  
  
1.  En un controlador de dominio del bosque de la organización del asociado de cuenta, inicie la **Group Policy Management** ajustar\-en.  
  
2.  Busque el objeto de directiva de grupo adecuado \(GPO\), ¿no\-haga clic en él y, a continuación, haga clic en **editar**.  
  
3.  En el árbol de consola, abra **configuración de usuario\\preferencias\\Windows configuración\\mantenimiento de Internet Explorer**y, a continuación, haga clic en **seguridad**.  
  
4.  En el panel de detalles, haga doble\-haga clic en **zonas de seguridad y clasificación de contenido**.  
  
5.  En **zonas de seguridad y privacidad**, haga clic en **importar la configuración de privacidad y las zonas de seguridad actual**y, a continuación, haga clic en **modificar la configuración**.  
  
6.  Haga clic en **intranet Local**y, a continuación, haga clic en **sitios**.  
  
7.  En **agregar este sitio Web a la zona**, escriba el nombre DNS completo del servidor de federación de cuenta \(por ejemplo, https:\/\/fs1.fabrikam.com\), haga clic en **agregar**y, a continuación, haga clic en **cerrar**.  
  
8.  Haga clic en **Aceptar** dos veces para aplicar estos cambios a la directiva de grupo.  
  
