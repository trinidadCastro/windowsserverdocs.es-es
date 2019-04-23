---
title: Migrar una granja WID de AD FS 2.0 federation server
description: Proporciona información sobre cómo migrar una granja de AD FS 2.0 servidor WID a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868326"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar una granja WID de AD FS 2.0  
Este documento proporciona información detallada sobre cómo migrar un anuncio granja FS 2.0 Windows Internal Database (WID) a Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrar una granja WID de AD FS
Para migrar una granja WID a Windows Server 2012, siga el procedimiento siguiente:  
  
1.  Para cada nodo (servidor) de la granja WID, revise y realice los procedimientos de [preparación para migrar una granja WID](prepare-to-migrate-a-wid-farm.md).  
  
2.  Elimina los nodos no principales del equilibrador de carga.  
  
3.  Actualización del sistema operativo del servidor de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. El rol de servidor de AD FS en Windows Server 2012 está instalado en su lugar, pero no está configurado. Es necesario crear la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.  
  
4.  Crea la configuración de AD FS original en el servidor.  
  
Para crear la configuración de AD FS original se puede usar el **Asistente para configuración de servidor de federación de AD FS** para agregar un servidor de federación a una granja WID. Para obtener más información, consulta [Agregar un servidor de federación a una granja de servidores de federación](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Cuando llegues a la página **Especificar el servidor de federación principal y una cuenta de servicio** del **Asistente para configuración de servidor de federación de AD FS**, escribe el nombre del servidor de federación principal de la granja WID y asegúrate de especificar la información de la cuenta de servicio que registraste al preparar la migración de AD FS. Para obtener más información, consulte [Prepare to Migrate AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md). 
>  
> Cuando llegue a la **especificar el nombre del servicio de federación** página, asegúrese de seleccionar el mismo certificado SSL que registró en la "preparación para migrar una granja WID" en [Prepare to Migrate AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md).  
  
5.  Actualiza las páginas web de AD FS del servidor. Si la copia de sus páginas Web personalizadas de AD FS al preparar la migración, deberá usar los datos de copia de seguridad para sobrescribir el valor predeterminado de páginas Web de AD FS que se crearon de forma predeterminada en el **%systemdrive%\inetpub\adfs\ls** directorio como un resultado de la configuración de AD FS en Windows Server 2012.  
  
6.  Agregue el servidor que acaba de actualizar a Windows Server 2012 para el equilibrador de carga.  
  
7.  Repite los pasos del 1 al 6 con el resto de servidores secundarios de la granja WID.  
  
8.  Promociona uno de los servidores secundarios actualizados para que sea el servidor principal de la granja WID. Para ello, abra Windows PowerShell y ejecute el siguiente comando: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Elimina del equilibrador de carga el servidor principal original de la granja WID.  
  
10. Degrada el servidor principal de la granja WID para que sea un servidor secundario mediante Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecute el comando siguiente para degradar el servidor principal original a un servidor secundario: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. La actualización del sistema operativo en el último nodo (servidor) en la granja WID de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. El rol de servidor de AD FS en Windows Server 2012 está instalado en su lugar, pero no está configurado. Es necesario crear de forma manual la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.  
  
12. Crea la configuración de AD FS original en el último nodo (servidor) de la granja WID.  
  
Para crear la configuración de AD FS original se puede usar el **Asistente para configuración de servidor de federación de AD FS** para agregar un servidor de federación a una granja WID. Para obtener más información, consulta [Agregar un servidor de federación a una granja de servidores de federación](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Cuando llegues a la página **Especificar el servidor de federación principal y una cuenta de servicio** del **Asistente para configuración de servidor de federación de AD FS**, especifica la información de la cuenta de servicio que registraste al preparar la migración de AD FS. Para obtener más información, consulte [Prepare to Migrate AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md). 
>  
> Cuando llegue a la **especificar el nombre del servicio de federación** página, asegúrese de seleccionar el mismo certificado SSL que registró en [Prepare to Migrate AD FS 2.0 Federation Server](prepare-to-migrate-a-wid-farm.md).  
  
13. Actualiza las páginas web de AD FS del último servidor de la granja WID. Si la copia de sus páginas Web personalizadas de AD FS al preparar la migración, usar los datos de copia de seguridad para sobrescribir el valor predeterminado de páginas Web de AD FS que se crearon de forma predeterminada en el **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de AD FS en Windows Server 2012.  
  
14. Agregue el último servidor de la granja WID que acabas de actualizar a Windows Server 2012 para el equilibrador de carga.  
  
15. Restaure el resto de las personalizaciones de AD FS, como los almacenes de atributos personalizados.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS 1.1 de Web](migrate-the-ad-fs-web-agent.md)