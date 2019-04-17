---
title: "Migrar el servidor de AD FS federación 2.0"
description: "Proporciona información sobre cómo migrar un servidor de AD FS a Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2017
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Comprueba la AD FS 2.0 migración a Windows Server 2012 R2

Una vez que haya finalizado la migración de servidor mismo del conjunto de servicios de federación de Active Directory (AD FS) para Windows Server 2012 R2, puedes usar el siguiente procedimiento para comprobar que los servidores de federación de la granja operativos; es decir, que cualquier cliente en la misma red puede llegar a los servidores de federtation.  
  
Pertenencia a **usuarios**, **operadores de copia de seguridad**, **usuarios avanzados**, **administradores** o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Para comprobar que un servidor de federación está operativo  
  
1.  Abre una ventana del explorador y en la barra de direcciones, escribe el nombre de los servidores de federación y anexarlo con `federationmetadata/2007-06/federationmetadata.xml` para examinar el extremo de metadatos de servicio de federación. Por ejemplo, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Si puedes ver los metadatos de servidor de federación sin SSL errores ni advertencias en la ventana del explorador, el servidor de federación está operativo.  
  
2.  También puedes examinar a la página de inicio de sesión de AD FS (anexa tu nombre de servicio de federación con `adfs/ls/idpinitiatedsignon.htm`, por ejemplo, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Aparecerá la AD FS sesión página donde iniciar sesión con credenciales de administrador de dominio.  
  
> [!IMPORTANT]
>  Asegúrate de que configurar la configuración del explorador para que confíe en el rol de servidor de federación agregando tu nombre de servicio de federación (por ejemplo, `https://fs.contoso.com`) a la zona de intranet local del explorador.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparación para migrar el servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrar al Proxy de servidor de federación de AD FS](migrate-fed-server-proxy-r2.md)   
