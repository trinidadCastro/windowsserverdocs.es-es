---
title: Migrar un servidor de federación de AD FS independiente o una granja de AD FS de nodo único
description: Proporciona información sobre cómo migrar un servidor de espera por sí solo o nodo único de AD FS 2.0 para Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851936"
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Migrar un servidor de federación de AD FS independiente o una granja de AD FS de nodo único  
Este documento proporciona información detallada sobre cómo migrar un servidor independiente de AD FS 2.0 independiente para Windows Server 2012.

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>Migrar una independiente AD FS 2.0 server

Utilice el procedimiento siguiente para migrar de AD FS 2.0 server a Windows Server 2012.
  
1.  Revise y complete los procedimientos de [preparación para migrar un servidor de federación de AD FS independiente o una granja de servidores de AD FS de nodo único](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md).  
  
2.  Realizar una actualización en contexto del sistema operativo del servidor de Windows Server 2008 R2 o Windows Server 2008 a Windows Server 2012. Para obtener más información, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado de la actualización del sistema operativo, se perderá la configuración de AD FS en el servidor y se eliminará el rol del servidor de AD FS 2.0. El rol de servidor de AD FS en Windows Server 2012 está instalado en su lugar, pero no está configurado. Es necesario crear de forma manual la configuración de AD FS original y restaurar la configuración de AD FS restante para completar la migración del servidor de federación.  
  
3.  Crea la configuración de AD FS original. La configuración de AD FS original se puede crear con uno de los métodos siguientes:  
  
-   Usa el **Asistente para configuración de servidor de federación de AD FS** para crear un nuevo servidor de federación. Para obtener más información, consulta [Crear el primer servidor de federación de una granja de servidores de federación](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
A medida que avances en el asistente, usa la información que has recopilado al preparar la migración del servidor de federación de AD FS, como se indica a continuación:  
  
 |**Opción de entrada del Asistente para configuración de servidor de federación**|**Utilice el siguiente valor**| 
|-----|-----| 
|**Certificado SSL** en la página **Especificar el nombre del Servicio de federación**|Selecciona el certificado SSL cuyo nombre de sujeto y huella digital hayas registrado al preparar la migración del servidor de federación de AD FS.|  
|**Cuenta de servicio** y **Contraseña** en la página **Especificar una cuenta de servicio**|Especifica la información de la cuenta de servicio que registraste al preparar la migración del servidor de federación de AD FS. **Nota:**  Si seleccionas la opción de servidor de federación independiente en la segunda página del asistente, se usará automáticamente SERVICIO DE RED como la cuenta de servicio.|  
  
> [!IMPORTANT] 
> Este método solo es válido si usas Windows Internal Database (WID) para almacenar la base de datos de configuración de AD FS para el servidor de federación independiente o para una granja de AD FS de nodo único.  
>
>  Si usas SQL Server para almacenar la base de datos de configuración de AD FS para la granja de AD FS de nodo único, tendrás que usar Windows PowerShell para crear la configuración de AD FS original en el servidor de federación.  
  
-   Utilice Windows PowerShell  
  
> [!IMPORTANT]
>  Deberás usar Windows PowerShell si usas SQL Server para almacenar la base de datos de configuración de AD FS para el servidor de federación independiente o para una granja de AD FS de nodo único.  
  
A continuación verás un ejemplo de cómo usar Windows PowerShell para crear la configuración de AD FS original en un servidor de federación de una granja de SQL Server de nodo único.  Abra el módulo de Windows PowerShell y ejecute el comando siguiente: `$fscredential = Get-Credential` Escribe el nombre y la contraseña de la cuenta de servicio que registraste al preparar la migración de la granja de SQL Server. A continuación, ejecute el siguiente comando: `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` , donde `Data Source` es el valor del origen de datos del valor de cadena de conexión de almacén de directivas en el siguiente archivo: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
4.  Restaura la configuración del servicio de AD FS y las relaciones de confianza restantes. Este es un paso manual en el que puedes usar los archivos que hayas exportado y los valores que hayas recopilado al preparar la migración de AD FS. Consulta las instrucciones detalladas en Restaurar la configuración restante de la granja de AD FS.  
  
> [!NOTE]
>  Este paso solo es obligatorio si vas a migrar un servidor de federación independiente o una granja WID de nodo único.  Si el servidor de federación usa una base de datos de SQL Server como el almacén de configuración, la configuración del servicio y las relaciones de confianza se preservarán en la base de datos.  
  
5.  Actualiza las páginas web de AD FS. Este paso es manual. Si la copia de sus páginas Web personalizadas de AD FS al preparar la migración, usar los datos de copia de seguridad para sobrescribir el valor predeterminado de páginas Web de AD FS que se crearon de forma predeterminada en el **%systemdrive%\inetpub\adfs\ls** directorio como resultado de la configuración de AD FS en Windows Server 2012.  
  
6.  Restaure el resto de las personalizaciones de AD FS, como los almacenes de atributos personalizados.  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>Restaurar la configuración restante de la granja de AD FS  
  
-   Sigue estos pasos para restaurar la siguiente configuración del servicio de AD FS a una granja WID de nodo único o a un servicio de federación independiente:  
  
    -   En la consola de administración de AD FS, selecciona **Servicios** y haz clic en **Editar servicio de federación…**. Para comprobar la configuración del servicio de federación, compara los valores con los que exportaste al archivo properties.txt al preparar la migración:  
  
    
|**Nombre de propiedad del servicio de federación devuelto por Get-ADFSProperties**|**Nombre de propiedad del servicio de federación en la consola de administración de AD FS**|  
|-----|-----|
|DisplayName|Nombre para mostrar del Servicio de federación|  
|HostName|Nombre del Servicio de federación|  
|Identificador|Identificador del Servicio de federación|  
  
-   En la consola de administración de AD FS, seleccione **Certificados**. Para comprobar los certificados de comunicaciones de servicios, descifrado de tokens y firma de tokens, compáralos con los valores que exportaste al archivo certificates.txt al preparar la migración.  
  
Para cambiar los certificados de descifrado o de firma de tokens de los valores autofirmados predeterminados a certificados externos, primero debe deshabilitar la característica de sustitución automática de certificados habilitada de manera predeterminada.  Para ello, puede utilizar el siguiente comando de Windows PowerShell: `PSH: Set-ADFSProperties –AutoCertificateRollover $false`  
  
-   En la consola de administración de AD FS, seleccione **Extremos**. Compare los extremos de AD FS habilitados con la lista de extremos de AD FS habilitados que exportó a un archivo mientras preparaba la migración de AD FS.  
  
-   En la Consola de administración de AD FS, seleccione **Descripciones de notificaciones**. Compara la lista de descripciones de notificaciones de AD FS con la lista de descripciones de notificaciones que exportaste a un archivo al preparar la migración de AD FS. Agregue las descripciones de notificaciones personalizadas incluidas en el archivo que no se estén incluidas en la lista predeterminada de AD FS.  Tenga en cuenta que el identificador de la notificación en la consola de administración corresponde a ClaimType en el archivo.  Para obtener más información sobre cómo agregar descripciones de notificaciones, consulta [Agregar una descripción de notificaciones](../operations/add-a-claim-description.md). Para obtener más información, consulta la sección “Paso 1: configuración del servicio de exportación” en Preparación para migrar el servidor de federación de AD FS 2.0.  
  
-   En la Consola de administración de AD FS, selecciona **Confianzas de proveedores de notificaciones**. Debes recrear de forma manual todas las confianzas del proveedor de notificaciones mediante el **Asistente para agregar confianza del proveedor de notificaciones**.  Usa la lista de confianzas de proveedores de notificaciones que exportaste y registraste al preparar la migración de AD FS. Puedes ignorar la confianza de proveedor de notificaciones con el identificador “AD AUTHORITY” en el archivo, ya que esta es la confianza del proveedor de notificaciones de “Active Directory” que forma parte de la configuración predeterminada de AD FS.  Pero debes comprobar las reglas de notificaciones personalizadas que puedas haber agregado a la confianza de Active Directory antes de la migración. Para obtener más información sobre cómo crear confianzas de proveedores de notificaciones, consulta [Crear una confianza de proveedor de notificaciones con metadatos de federación](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata) o [Crear una confianza de proveedor de notificaciones de forma manual](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually).  
  
-   En la consola de administración de AD FS, selecciona **Relaciones de confianza para usuarios autenticados**. Debes recrear de forma manual todas las relaciones de confianza para usuario autenticado mediante el **Asistente para agregar relación de confianza para usuario autenticado**. Usa la lista de relaciones de confianza para usuarios autenticados que exportaste y registraste al preparar la migración de AD FS. Para obtener más información sobre cómo crear relaciones de confianza para usuarios autenticados, consulta [Crear una relación de confianza para usuario autenticado con metadatos de federación](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata) o [Crear una relación de confianza para usuario autenticado de forma manual](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually). 

## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS 1.1 de Web](migrate-the-ad-fs-web-agent.md)




-   
-    