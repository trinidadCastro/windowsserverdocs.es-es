---
title: Migrar el servidor proxy de Federación AD FS 2,0
description: Proporciona información sobre cómo migrar el servidor proxy de Federación de AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e97a1b3b66d7c12c96570022b7e69caaf0e92f65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408221"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrar el servidor proxy de Federación AD FS 2,0
En este documento se proporciona información detallada sobre cómo migrar un servidor proxy de Federación AD FS 2,0 a Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migración del proxy

Para migrar un servidor proxy de Federación AD FS 2,0 a Windows Server 2012, realice el procedimiento siguiente:  
  
1.  Para cada servidor proxy de Federación que piense migrar a Windows Server 2012, revise y realice los procedimientos de [preparación para migrar el servidor proxy de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Quita un servidor proxy de federación del equilibrador de carga.  
  
3.  Realiza una actualización local del sistema operativo en este servidor de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en este servidor y se eliminará el rol del servidor de AD FS 2.0. En su lugar, se instala el rol de servidor de AD FS de Windows Server 2012, pero no está configurado. Tienes que crear la configuración de AD FS original de forma manual y restaurar la configuración de AD FS restante para completar la migración del servidor proxy de federación.  
  
4. Crea la configuración proxy de AD FS original usando el **Asistente para la configuración del servidor proxy de federación de AD FS**. Para obtener más información, consulte [configurar un equipo para el rol de servidor Proxy de federación](configure-a-computer-for-the-federation-server-proxy-role.md). Cuando ejecutes el asistente, usa la información recopilada en la Preparación para migrar el servidor de federación AD FS 2.0 como se muestra a continuación:  
  
 
|**Opción de entrada del asistente del servidor proxy de Federación**|**Usar el siguiente valor**|
|-----|-----|  
|**Nombre del Servicio de federación**|Escribe el valor de BaseHostName del archivo proxyproperties.txt|  
|casilla de verificación **Usa un servidor proxy HTTP al enviar solicitudes a este servicio de federación**|Activa esta casilla si tu archivo proxyproperties.txt contiene un valor para la propiedad ForwardProxyUrl|  
|**Dirección del servidor proxy HTTP**|Escribe el valor de ForwardProxyUrl del archivo proxyproperties.txt|  
|Petición de credenciales|Escribe las credenciales de una cuenta que sea un administrador del servidor de federación de AD FS o la cuenta de servicio en la que se ejecuta el servicio de federación de AD FS.|  
  
5. Actualiza las páginas web de AD FS del servidor. Si realizó una copia de seguridad de las páginas web personalizadas del proxy de AD FS mientras prepara el servidor proxy de Federación para la migración, use los datos de copia de seguridad para sobrescribir las páginas Web predeterminadas AD FS que se crearon de forma predeterminada en el directorio **%SystemDrive%\inetpub\adfs\ls** como resultado de la configuración del proxy de AD FS en Windows Server 2012.  
  
6. Vuelve a agregar este servidor al equilibrador de carga.  
  
7. Si tienes que migrar más servidores proxy de federación AD FS 2.0, repite los pasos del 2 al 6 para los equipos de servidores proxy de federación restantes.  
  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor proxy de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de federación AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)