---
title: Migración del servidor proxy de AD FS 2.0 federation
description: Proporciona información sobre cómo migrar al servidor proxy de federación de AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eddb0d3432c69ecff4542ff1b8f2204b96ce0820
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445665"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migración del servidor proxy de AD FS 2.0 federation
Este documento proporciona información detallada sobre cómo migrar un servidor proxy de federación 2.0 de AD FS a Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrar el servidor proxy

Para migrar a un servidor proxy de federación 2.0 de AD FS a Windows Server 2012, siga el procedimiento siguiente:  
  
1.  Para cada servidor proxy de federación que se va a migrar a Windows Server 2012, revisar y realizar los procedimientos de [Prepare to Migrate AD FS 2.0 servidor Proxy de federación](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Quita un servidor proxy de federación del equilibrador de carga.  
  
3.  Realizar una actualización en contexto del sistema operativo del servidor de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en este servidor y se eliminará el rol del servidor de AD FS 2.0. El rol de servidor de AD FS en Windows Server 2012 está instalado en su lugar, pero no está configurado. Tienes que crear la configuración de AD FS original de forma manual y restaurar la configuración de AD FS restante para completar la migración del servidor proxy de federación.  
  
4. Crea la configuración proxy de AD FS original usando el **Asistente para la configuración del servidor proxy de federación de AD FS**. Para obtener más información, consulte [configurar un equipo para el rol de servidor Proxy de federación](configure-a-computer-for-the-federation-server-proxy-role.md). Cuando ejecutes el asistente, usa la información recopilada en la Preparación para migrar el servidor de federación AD FS 2.0 como se muestra a continuación:  
  
 
|**Opción de entrada del Asistente del Proxy de servidor de federación**|**Utilice el siguiente valor**|
|-----|-----|  
|**Nombre del servicio de federación**|Escribe el valor de BaseHostName del archivo proxyproperties.txt|  
|casilla de verificación **Usa un servidor proxy HTTP al enviar solicitudes a este servicio de federación**|Activa esta casilla si tu archivo proxyproperties.txt contiene un valor para la propiedad ForwardProxyUrl|  
|**Dirección del servidor proxy HTTP**|Escribe el valor de ForwardProxyUrl del archivo proxyproperties.txt|  
|Petición de credenciales|Escribe las credenciales de una cuenta que sea un administrador del servidor de federación de AD FS o la cuenta de servicio en la que se ejecuta el servicio de federación de AD FS.|  
  
5. Actualiza las páginas web de AD FS del servidor. Si copia las páginas Web personalizadas proxy de AD FS al preparar el servidor proxy de federación para la migración, usar los datos de copia de seguridad para sobrescribir el valor predeterminado de páginas Web de AD FS que se crearon de forma predeterminada en el **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de proxy de AD FS en Windows Server 2012.  
  
6. Vuelve a agregar este servidor al equilibrador de carga.  
  
7. Si tienes que migrar más servidores proxy de federación AD FS 2.0, repite los pasos del 2 al 6 para los equipos de servidores proxy de federación restantes.  
  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)