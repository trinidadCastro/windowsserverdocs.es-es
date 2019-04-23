---
title: Migrar el servidor de federación 2.0 de AD FS
description: Proporciona información sobre cómo migrar un servidor de AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877776"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Compruebe el AD FS 2.0 migración a Windows Server 2012 R2

Cuando haya completado la migración del mismo servidor de la granja de servidores de servicio de federación de Active Directory (AD FS) a Windows Server 2012 R2, puede usar el procedimiento siguiente para comprobar que los servidores de federación en la granja de servidores están operativos; es decir, que cualquier cliente en la misma red puede llegar a los servidores federtation.  
  
Para completar este procedimiento, se requiere como mínimo la pertenencia a **Usuarios**, **Operadores de copia de seguridad**, **Usuarios avanzados**, **Administradores** o equivalente.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Para comprobar que un servidor de federación sea operativo  
  
1.  Abra una ventana del explorador y en la barra de direcciones, escriba el nombre de los servidores de federación y, a continuación, anexe `federationmetadata/2007-06/federationmetadata.xml` para ir al extremo de metadatos del servicio de federación. Por ejemplo, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Si en la ventana del explorador puede ver los metadatos del servidor de federación sin errores ni advertencias de SSL, el servidor de federación es operativo.  
  
2.  También puede ir a la página de inicio de sesión de AD FS (nombre de su servicio de federación con `adfs/ls/idpinitiatedsignon.htm` anexado, por ejemplo, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Esto muestra la página de inicio de sesión de AD FS donde puede iniciar sesión con credenciales de administrador del dominio.  
  
> [!IMPORTANT]
>  Asegúrese de configurar las opciones del explorador para que confíe en el rol del servidor de federación. Para ello, agregue el nombre del servicio de federación (por ejemplo, `https://fs.contoso.com`) a la zona de intranet local del explorador.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de servicios de federación de Active Directory a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparar la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migración del servidor Proxy de federación de AD FS](migrate-fed-server-proxy-r2.md)   
