---
title: Comprobar la migración de AD FS 2,0 a Windows Server 2012 R2
description: Proporciona información sobre cómo migrar un servidor de AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fe03e1c5352c82d8654fb12413b5ed169190f874
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87863868"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Comprobar la migración de AD FS 2,0 a Windows Server 2012 R2

Una vez que complete la misma migración de servidor de la granja de Active Directory Servicio de federación (AD FS) a Windows Server 2012 R2, puede usar el procedimiento siguiente para comprobar que los servidores de Federación de la granja están operativos; es decir, que cualquier cliente de la misma red pueda acceder a los servidores de federtation.  
  
Para completar este procedimiento, se requiere como mínimo la pertenencia a **Usuarios**, **Operadores de copia de seguridad**, **Usuarios avanzados**, **Administradores** o equivalente.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Para comprobar que un servidor de federación sea operativo  
  
1.  Abra una ventana del explorador y, en la barra de direcciones, escriba el nombre de los servidores de Federación y, a continuación, agréguelo `federationmetadata/2007-06/federationmetadata.xml` a para buscar el extremo de metadatos del servicio de Federación. Por ejemplo, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Si en la ventana del explorador puede ver los metadatos del servidor de federación sin errores ni advertencias de SSL, el servidor de federación es operativo.  
  
2. También puede ir a la página de inicio de sesión de AD FS (nombre de su servicio de federación con `adfs/ls/idpinitiatedsignon.htm` anexado, por ejemplo, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Esto muestra la página de inicio de sesión de AD FS donde puede iniciar sesión con credenciales de administrador del dominio.  
  
> [!IMPORTANT]
>  Asegúrese de configurar las opciones del explorador para que confíe en el rol de servidor de Federación. para ello, agregue el nombre del servicio de Federación (por ejemplo, `https://fs.contoso.com` ) a la zona de Intranet local del explorador.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de Servicios de federación de Active Directory (AD FS) a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparación de la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migración del servidor de Federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migración del servidor proxy de federación de AD FS](migrate-fed-server-proxy-r2.md)   
