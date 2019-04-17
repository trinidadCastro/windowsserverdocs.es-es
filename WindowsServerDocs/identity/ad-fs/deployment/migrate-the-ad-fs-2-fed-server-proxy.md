---
title: "Migrar al proxy de servidor de AD FS federación 2.0"
description: "Proporciona información sobre cómo migrar al proxy de servidor de federación de AD FS a Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrar al proxy de servidor de AD FS federación 2.0
Este documento proporciona información detallada sobre cómo migrar un servidor proxy de federación 2.0 de AD FS a Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrar el proxy

Para migrar a un proxy de servidor de AD FS federación 2.0 en Windows Server 2012, realiza lo siguiente:  
  
1.  Para cada proxy de servidor de federación que vas a migrar a Windows Server 2012, revisar y realizar los procedimientos descritos en [preparación para migrar AD FS 2.0 Proxy del servidor de federación](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Quitar a un proxy de servidor de federación de la carga de equilibrado.  
  
3.  Realizar una actualización en contexto del sistema operativo de este servidor de Windows Server 2008 R2 o Windows Server 2008a Windows Server 2012. Para obtener más información, consulta [instalar Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se pierde la configuración de proxy de AD FS de este servidor y se quita el rol de servidor 2.0 de AD FS. El rol de servidor de AD FS de Windows Server 2012 se instala en su lugar, pero no está configurado. Debes crear la configuración de proxy de AD FS original y restaurar la configuración de proxy de AD FS restante para completar la migración de proxy del servidor de federación manualmente.  
  
4.  Crear la configuración de proxy de AD FS original mediante el **Asistente de configuración de Proxy de servidor de AD FS federación**. Para obtener más información, consulta [configurar un equipo para el rol de Proxy del servidor de federación](configure-a-computer-for-the-federation-server-proxy-role.md). Como se ejecuta al asistente, usa la información recopilada en preparación para migrar AD FS 2.0 Proxy del servidor de federación como sigue:  
  
 
|**Asistente de Proxy del servidor de federación de opción de entrada**|**Usa el siguiente valor**|
|-----|-----|  
|**Nombre de servicio de federación**|Escribe el valor de BaseHostName de archivo proxyproperties.txt|  
|**Usar un servidor proxy HTTP al enviar solicitudes a esta federación** casilla de verificación de servicio|Marca esta casilla si el archivo proxyproperties.txt contiene un valor para la propiedad ForwardProxyUrl|  
|**Dirección del servidor proxy HTTP**|Escribe el valor de ForwardProxyUrl de archivo proxyproperties.txt|  
|Petición de credenciales|Escribe las credenciales de cuenta que es un administrador del servidor de federación de AD FS o la cuenta de servicio en la que se ejecuta el servicio de federación de AD FS.|  
  
5.  Actualiza las páginas Web de AD FS de este servidor. Si copias de tus páginas Web personalizada proxy de AD FS durante la preparación para la migración de tu proxy de servidor de federación, usa los datos de copia de seguridad para sobrescribir el valor predeterminado de las páginas Web de AD FS que se crearon de manera predeterminada en la **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de proxy de AD FS en Windows Server 2012.  
  
6.  Este servidor volver a agregar el equilibrado de carga.  
  
7.  Si tienes otros servidores proxy de servidor de AD FS federación 2.0 para migrar, repite los pasos 2a 6 para los demás equipos de proxy del servidor de federación.  
  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)