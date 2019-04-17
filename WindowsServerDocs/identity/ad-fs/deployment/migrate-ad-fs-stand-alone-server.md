---
title: "Migrar un servidor de federación de AD FS independiente o una granja de servidores de AD FS de nodo único"
description: "Proporciona información sobre cómo migrar un independiente o un nodo único AD FS 2.0 server para Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Migrar un servidor de federación de AD FS independiente o una granja de servidores de AD FS de nodo único  
Este documento proporciona información detallada sobre cómo migrar un servidor de AD FS 2.0 independiente para Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Migrar un anuncio independiente servidor FS 2.0

Usa el siguiente procedimiento para migrar a AD FS 2.0 servidor para Windows Server 2012.
  
1.  Revisar y realizar los procedimientos descritos en [preparación para migrar un servidor de federación de AD FS independiente o una granja de servidores de AD FS nodo único](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Realizar una actualización en contexto del sistema operativo en el servidor de Windows Server 2008 R2 o Windows Server 2008a Windows Server 2012. Para obtener más información, consulta [instalar Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se pierde la configuración de AD FS de este servidor y se quita el rol de servidor 2.0 de AD FS. El rol de servidor de AD FS de Windows Server 2012 se instala en su lugar, pero no está configurado. Debes crear la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración de servidor de federación manualmente.  
  
3.  Crear la configuración de AD FS original. Puedes crear la configuración original de AD FS mediante uno de los siguientes métodos:  
  
-   Usa el **Asistente de configuración del servidor de AD FS federación** para crear un servidor de federación de nuevo. Para obtener más información, consulta [crea el primer servidor de federación en una granja de servidores de federación](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
Medida que avances por el asistente, usa la información recopilada durante la preparación para migrar el servidor de federación de AD FS como sigue:  
  
 |**Asistente para configuración del servidor de federación de opción de entrada**|**Usa el siguiente valor**| 
|-----|-----| 
|**Certificado SSL** en la **especifique el nombre de servicio de federación** página|Selecciona el certificado SSL cuyo nombre de asunto y la huella digital registra durante la preparación para la migración de servidor de federación de AD FS.|  
|**Cuenta de servicio** y **contraseña** en la **especificar una cuenta de servicio** página|Escribe la información de cuenta de servicio que registraron durante la preparación para la migración de servidor de federación de AD FS. **Nota:** si seleccionas el servidor de federación independiente en la segunda página del asistente, el servicio de red se usa automáticamente como la cuenta de servicio.|  
  
> [!IMPORTANT] 
> Puede usar este método solo si estás usando Windows Internal Database (WID) para almacenar la base de datos de configuración de AD FS para el servidor de federación independiente o una granja de servidores de AD FS nodo único.  
>
>  Si estás usando SQL Server para almacenar la base de datos de configuración de AD FS de la granja de servidores de AD FS nodo único, debes usar Windows PowerShell para crear la configuración de AD FS original en el servidor de federación.  
  
-   Usar Windows PowerShell  
  
> [!IMPORTANT]
>  Debes usar Windows PowerShell si estás usando SQL Server para almacenar la base de datos de configuración de AD FS para el servidor de federación independiente o una granja de servidores de AD FS nodo único.  
  
El siguiente es un ejemplo de cómo usar Windows PowerShell para crear la configuración de AD FS original en un servidor de federación de un conjunto de SQL Server nodo único.  Abra el módulo de Windows PowerShell y ejecuta el siguiente comando:`$fscredential = Get-Credential`. Escribe el nombre y la contraseña de la cuenta de servicio que registraron durante la preparación de la granja de servidores SQL para la migración. A continuación, ejecuta el siguiente comando: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`donde `Data Source`es el valor de origen de datos en el valor de cadena de conexión de almacén de directivas en el siguiente archivo:`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4.  Restaurar la configuración de servicio de AD FS restante y relaciones de confianza. Este es un paso manual durante el que puedes usar los archivos que se exportó y los valores que recopilan durante la preparación para la migración de AD FS. Para obtener instrucciones detalladas, consulta restaurar la configuración de granja restante AD FS.  
  
> [!NOTE]
>  Este paso solo es necesario si vas a migrar un servidor de federación independiente o una granja WID nodo único.  Si el servidor de federación usa una base de datos de SQL Server que el almacén de configuración, la configuración del servicio y relaciones de confianza se conservan en la base de datos.  
  
5.  Actualiza las páginas Web de AD FS. Este es un paso manual. Si copias de tus páginas Web de AD FS personalizada durante la preparación para la migración, usa los datos de copia de seguridad para sobrescribir el valor predeterminado de las páginas Web de AD FS que se crearon de manera predeterminada en la **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de AD FS en Windows Server 2012.  
  
6.  Restaurar las personalizaciones de AD FS restantes, como el atributo personalizado tiendas.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Restaurar la configuración de granja FS AD restante  
  
-   Restaurar la siguiente configuración de servicio de AD FS a un conjunto de nodo único WID o servicios de federación independiente como sigue:  
  
    -   En la consola de administración de AD FS, selecciona **servicio** y haz clic en **servicios de federación de editar...**. Comprueba la configuración de servicios de federación comprobando cada uno de los valores en los valores exportado en el archivo properties.txt durante la preparación para la migración:  
  
    
|**Nombre de la propiedad de servicios de federación según lo informado por Get-ADFSProperties**|**Nombre de la propiedad de servicios de federación en la consola de administración de AD FS**|  
|-----|-----|
|DisplayName|Nombre de servicio federación|  
|Nombre de host|Nombre de servicio de federación|  
|Identificador|Identificador de servicios de federación|  
  
-   En la consola de administración de AD FS, selecciona **certificados**. Compruebe las comunicaciones de servicio, los certificados de descifrado de token y la firma de token comprobando cada uno con los valores exportado en el archivo certificates.txt durante la preparación para la migración.  
  
Para cambiar los certificados de descifrado de token o la firma de tokens de los certificados autofirmados predeterminado a certificados externos, primero debes deshabilitar la característica de sustitución de certificados automática está habilitada de manera predeterminada.  Para ello, puedes usar el siguiente comando de Windows PowerShell:`PSH: Set-ADFSProperties –AutoCertificateRollover $false`.  
  
-   En la consola de administración de AD FS, seleccione **extremos**. Comprueba los extremos de AD FS habilitados con la lista de los extremos de AD FS habilitados que exportar a un archivo durante la preparación para la migración de AD FS.  
  
-   En la consola de administración de AD FS, seleccione **descripciones de Reclamación**. Compruebe la lista de descripciones de notificación de AD FS con la lista de descripciones de notificación que exportación a un archivo durante la preparación para la migración de AD FS. Agrega las descripciones de notificación personalizada incluir en el archivo pero no incluido en la lista predeterminada de AD FS.  Ten en cuenta que el identificador de la notificación en la consola de administración se asigna a ClaimType en el archivo.  Para obtener más información sobre cómo agregar descripciones de notificación, consulta [agregar una descripción reclamar](../operations/add-a-claim-description.md). Para obtener más información, consulta la sección de "Paso 1 – Exportar configuración del servicio" en preparación para migrar AD FS servidor de federación de 2.0.  
  
-   En la consola de administración de AD FS, seleccione **reclamaciones proveedor confía**. Debes volver a crear cada confianza de proveedor de notificaciones manualmente a través de la **Asistente para agregar notificaciones de proveedor de confianza para**.  Usa la lista de confianzas de proveedor de notificaciones que exportado y se registran durante la preparación para la migración de AD FS. Puede pasar por alto la confianza del proveedor de notificaciones con el identificador "AD autoridad" en el archivo porque es la confianza del proveedor de notificaciones "Active Directory" que forma parte de la configuración de AD FS predeterminada.  Sin embargo, comprueba las reglas de notificación personalizada que agregaste a la confianza de Active Directory antes de la migración. Para obtener más información sobre la creación de notificaciones de confianzas de proveedor, consulta [crear un reclamaciones proveedor confianza usando federación metadatos](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) o [crear un reclamaciones proveedor confianza manualmente](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   En la consola de administración de AD FS, **selecciona confiar confía en las partes**. Debes volver a crear cada confianza del usuario de confianza manualmente a través de la **confiar terceros confianza Asistente para agregar**. Usa la lista de usuarios de confianza confianzas de terceros que exportado y se registran durante la preparación para la migración de AD FS. Para obtener más información sobre la creación de confianzas de terceros de confianza, consulta [crear un confiar terceros de confianza usando federación metadatos](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) o [crear un confiar terceros de confianza manualmente](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)




-   
-    