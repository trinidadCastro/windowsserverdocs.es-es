---
title: Migrar una granja de AD FS 2.0 federation server SQL
description: Proporciona información sobre cómo migrar una granja SQL server 2.0 de AD FS a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843576"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar una granja WID de AD FS 2.0  
Este documento proporciona información detallada sobre cómo migrar una granja de servidores de AD FS 2.0 SQL a Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrar una granja de SQL Server  
 Para migrar una granja de servidores de SQL Server a Windows Server 2012, siga el procedimiento siguiente:  
  
1.  Para cada servidor de la granja de SQL Server, revise y realice los procedimientos de [migrar una granja de servidores de SQL Server](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Elimina del equilibrador de carga los servidores de la granja de SQL Server.  
  
3.  Actualizar el sistema operativo del servidor de la granja de SQL Server de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. El rol de servidor de AD FS en Windows Server 2012 está instalado en su lugar, pero no está configurado. Es necesario crear de forma manual la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.  
  
4.  Crea la configuración de AD FS original en el servidor de la granja de SQL Server mediante los cmdlets de Windows PowerShell de AD FS para agregar un servidor a una granja existente.  
  
> [!IMPORTANT]
>  Si usas SQL Server para almacenar la base de datos de configuración de AD FS, deberás usar Windows PowerShell para crear la configuración de AD FS original.  

  - Abra Windows PowerShell y ejecute el siguiente comando: `$fscredential = Get-Credential`.  
  - Escribe el nombre y la contraseña de la cuenta de servicio que registraste al preparar la migración de la granja de SQL Server.  
  - Ejecute el siguiente comando: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, donde `Data Source` es el valor del origen de datos del valor de cadena de conexión de almacén de directivas en el siguiente archivo: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5.  Agregue el servidor que acaba de actualizar a Windows Server 2012 para el equilibrador de carga.  
  
6.  Repite los pasos del 2 al 6 en el resto de nodos de la granja de SQL Server.  
  
7.  Cuando todos los servidores de la granja de SQL Server se actualizan a Windows Server 2012, restaure las personalizaciones de AD FS restantes, como almacenes de atributos personalizados.  

## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS 1.1 de Web](migrate-the-ad-fs-web-agent.md)



