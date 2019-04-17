---
title: "Migrar un conjunto SQL server de AD FS federación 2.0"
description: "Proporciona información sobre cómo migrar un conjunto SQL server 2.0 AD FS a Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrar un conjunto de AD FS 2.0 WID  
Este documento proporciona información detallada sobre cómo migrar una granja de servidores de AD FS 2.0 SQL a Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrar un conjunto de SQL Server  
 Para migrar un conjunto de SQL Server para Windows Server 2012, realiza lo siguiente:  
  
1.  Para cada servidor en el conjunto de SQL Server, revisa y realiza los procedimientos descritos en [migrar un conjunto de SQL Server](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Quite el equilibrado carga cualquier servidor en el conjunto de SQL Server.  
  
3.  Actualizar el sistema operativo en este servidor en el conjunto de SQL Server de Windows Server 2008 R2 o Windows Server 2008a Windows Server 2012. Para obtener más información, consulta [instalar Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se pierde la configuración de AD FS de este servidor y se quita el rol de servidor 2.0 de AD FS. El rol de servidor de AD FS de Windows Server 2012 se instala en su lugar, pero no está configurado. Debes crear la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración de servidor de federación manualmente.  
  
4.  Crear la configuración de AD FS original de este servidor en el conjunto de SQL Server mediante los cmdlets de AD FS Windows PowerShell para agregar un servidor a un conjunto existente.  
  
> [!IMPORTANT]
>  Debes usar Windows PowerShell para crear la configuración original de AD FS si estás usando SQL Server para almacenar la base de datos de configuración de AD FS.  

  - Abre Windows PowerShell y ejecuta el siguiente comando:`$fscredential = Get-Credential`.  
  - Escribe el nombre y la contraseña de la cuenta de servicio que registraron durante la preparación de la granja de SQL Server para la migración.  
  - Ejecuta el siguiente comando: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, donde `Data Source`es el valor de origen de datos en el valor de cadena de conexión de almacén de directivas en el siguiente archivo:`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5.  Agrega el servidor que acaba de actualizar a Windows Server 2012 para el equilibrado de carga.  
  
6.  Repite los pasos 2a 6 para los otros nodos de la granja de SQL Server.  
  
7.  Cuando todos los servidores en el conjunto de SQL Server se actualizan a Windows Server 2012, restaurar las personalizaciones de AD FS restantes, como el atributo personalizado tiendas.  

## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)



