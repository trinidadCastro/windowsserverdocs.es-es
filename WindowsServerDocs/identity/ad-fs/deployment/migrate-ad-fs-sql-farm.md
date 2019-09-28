---
title: Migrar una granja de SQL de servidor de Federación AD FS 2,0
description: Proporciona información sobre cómo migrar una granja de SQL Server de AD FS 2,0 a Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3c43d26868f39896ec8632397dc0fce1dfe2dd2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408283"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar una granja AD FS 2,0 WID  
En este documento se proporciona información detallada sobre cómo migrar una granja de servidores SQL AD FS 2,0 a Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrar una granja de SQL Server  
 Para migrar una granja de SQL Server a Windows Server 2012, realice el procedimiento siguiente:  
  
1.  Para cada servidor de la granja de SQL Server, revise y realice los procedimientos que se describen en [migración de una granja de SQL Server](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Elimina del equilibrador de carga los servidores de la granja de SQL Server.  
  
3.  Actualice el sistema operativo de este servidor en la granja de SQL Server de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. En su lugar, se instala el rol de servidor de AD FS de Windows Server 2012, pero no está configurado. Es necesario crear de forma manual la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.  
  
4. Crea la configuración de AD FS original en el servidor de la granja de SQL Server mediante los cmdlets de Windows PowerShell de AD FS para agregar un servidor a una granja existente.  
  
> [!IMPORTANT]
>  Si usas SQL Server para almacenar la base de datos de configuración de AD FS, deberás usar Windows PowerShell para crear la configuración de AD FS original.  

  - Abra Windows PowerShell y ejecute el siguiente comando: `$fscredential = Get-Credential`.  
  - Escribe el nombre y la contraseña de la cuenta de servicio que registraste al preparar la migración de la granja de SQL Server.  
  - Ejecute el siguiente comando: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, donde `Data Source` es el valor del origen de datos del valor de cadena de conexión de almacén de directivas en el siguiente archivo: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5. Agregue el servidor que acaba de actualizar a Windows Server 2012 al equilibrador de carga.  
  
6. Repite los pasos del 2 al 6 en el resto de nodos de la granja de SQL Server.  
  
7. Cuando todos los servidores de la granja de SQL Server se actualizan a Windows Server 2012, restaure las personalizaciones de AD FS restantes, como los almacenes de atributos personalizados.  

## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor proxy de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de federación AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)



