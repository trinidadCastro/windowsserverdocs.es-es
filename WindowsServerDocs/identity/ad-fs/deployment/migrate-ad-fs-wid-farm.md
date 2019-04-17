---
title: "Migrar un conjunto de WID servidores de AD FS federación 2.0"
description: "Proporciona información sobre cómo migrar una granja de servidores de AD FS 2.0 servidor WID a Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar un conjunto de AD FS 2.0 WID  
Este documento proporciona información detallada sobre cómo migrar un anuncio granja FS 2.0 Windows Internal Database (WID) a Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrar un conjunto de AD FS WID
Para migrar una granja de servidores WID a Windows Server 2012, realiza lo siguiente:  
  
1.  Para cada nodo (servidor) de la granja de servidores WID, revisar y realizar los procedimientos descritos en [preparación para migrar una granja de servidores WID](prepare-to-migrate-a-wid-farm.md).  
  
2.  Quite los nodos no principal de la carga de equilibrado.  
  
3.  Actualización del sistema operativo de este servidor de Windows Server 2008 R2 o Windows Server 2008a Windows Server 2012. Para obtener más información, consulta [instalar Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se pierde la configuración de AD FS de este servidor y se quita el rol de servidor 2.0 de AD FS. El rol de servidor de AD FS de Windows Server 2012 se instala en su lugar, pero no está configurado. Debes crear la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración de servidor de federación.  
  
4.  Crear la configuración de AD FS original de este servidor.  
  
Puede crear la configuración de AD FS original mediante el **Asistente de configuración del servidor de AD FS federación** para agregar un servidor de federación a una granja de servidores WID. Para obtener más información, consulta [agregar un servidor de federación a una granja de servidores de federación](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Cuando llegues a la **especificar el servidor de federación principal y una cuenta de servicio** página en la **Asistente de configuración del servidor de AD FS federación**, escribe el nombre del servidor de federación principal de la granja de servidores WID y asegúrate de escribir la información de cuenta de servicio que registraron durante la preparación para la migración de AD FS. Para obtener más información, consulta [preparación para migrar AD FS 2.0 servidor de federación](prepare-to-migrate-a-wid-farm.md). 
>  
> Cuando llegues a la **especifique el nombre de servicio de federación** página, asegúrate de seleccionar el mismo certificado SSL que anotó en el "preparación para migrar una granja de servidores WID" en [preparación para migrar AD FS 2.0 servidor de federación](prepare-to-migrate-a-wid-farm.md).  
  
5.  Actualiza las páginas Web de AD FS de este servidor. Si la copia de las páginas Web de AD FS personalizadas durante la preparación para la migración, deberás usar los datos de copia de seguridad para sobrescribir el valor predeterminado de las páginas Web de AD FS que se crearon de manera predeterminada en la **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de AD FS en Windows Server 2012.  
  
6.  Agrega el servidor que acaba de actualizar a Windows Server 2012 para el equilibrado de carga.  
  
7.  Repite los pasos del 1 al 6 para los servidores secundarios restantes en su conjunto de servidores WID.  
  
8.  Promover uno de los servidores secundarios actualizados para el servidor en el conjunto de WID principal. Para ello, abre Windows PowerShell y ejecuta el siguiente comando:`PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Quite el servidor principal original de la granja de servidores WID el equilibrado de carga.  
  
10. Disminuir al servidor principal original en el conjunto de WID ser un servidor secundario mediante Windows PowerShell. Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para disminuir el servidor principal original para que sea un servidor secundario:`PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. Actualización del sistema operativo en este último nodo (servidor) en el conjunto de WID de Windows Server 2008 R2 o Windows Server 2008a Windows Server 2012. Para obtener más información, consulta [instalar Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se pierde la configuración de AD FS de este servidor y se quita el rol de servidor 2.0 de AD FS. El rol de servidor de AD FS de Windows Server 2012 se instala en su lugar, pero no está configurado. Debes crear la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración de servidor de federación manualmente.  
  
12. Crear la configuración de AD FS original en este último nodo (servidor) de la granja de servidores WID.  
  
Puede crear la configuración de AD FS original mediante el **Asistente de configuración del servidor de AD FS federación** para agregar un servidor de federación a una granja de servidores WID. Para obtener más información, consulta [agregar un servidor de federación a una granja de servidores de federación](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Cuando llegues a la **especificar el servidor de federación principal y una cuenta de servicio** página en la **Asistente de configuración del servidor de AD FS federación**, escribe la información de cuenta de servicio que registraron durante la preparación para la migración de AD FS. Para obtener más información, consulta [preparación para migrar AD FS 2.0 servidor de federación](prepare-to-migrate-a-wid-farm.md). 
>  
> Cuando llegues a la **especifique el nombre de servicio de federación** página, asegúrate de seleccionar el mismo certificado SSL registraste en [preparación para migrar AD FS 2.0 servidor de federación](prepare-to-migrate-a-wid-farm.md).  
  
13. Actualiza las páginas Web de AD FS en este último servidor de la granja de servidores WID. Si copias de tus páginas Web de AD FS personalizada durante la preparación para la migración, usa los datos de copia de seguridad para sobrescribir el valor predeterminado de las páginas Web de AD FS que se crearon de manera predeterminada en la **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de AD FS en Windows Server 2012.  
  
14. Agrega este último servidor de la granja de servidores WID que acaba de actualizar a Windows Server 2012 para el equilibrado de carga.  
  
15. Restaurar las personalizaciones de AD FS restantes, como el atributo personalizado tiendas.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)