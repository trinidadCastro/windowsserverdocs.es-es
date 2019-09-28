---
title: Preparar la migración de un servidor de Federación de AD FS independiente
description: Proporciona información sobre cómo preparar la migración de un servidor de AD FS independiente a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09b8cbd9097a95cd00b1413ce9e32ff9bf2f44c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359319"
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Preparar la migración de un servidor de federación de AD FS independiente o una granja de servidores de AD FS de un solo nodo  
 
Para preparar la migración (migración del mismo servidor) de un servidor de Federación AD FS 2,0 independiente o una granja de AD FS de un solo nodo a Windows Server 2012, debe exportar y realizar una copia de seguridad de los datos de configuración de AD FS desde este servidor.  
  
Para exportar los datos de configuración de AD FS, realiza estas tareas:  
  
-   [Paso 1:  Exportar la configuración del servicio @ no__t-0  
  
-   [Paso 2:  Exportar confianzas de proveedor de notificaciones @ no__t-0  
  
-   [Paso 3:  Exportar las relaciones de confianza para usuario autenticado @ no__t-0  
  
-   [Paso 4:  Copia de seguridad de almacenes de atributos personalizados @ no__t-0  
  
-   [Paso 5:  Realizar copias de seguridad de las personalizaciones de páginas web @ no__t-0  
  
## <a name="step-1-export-service-settings"></a>Paso 1: Exportar la configuración del servicio  
 Para exportar la configuración del servicio, realiza el siguiente procedimiento:  
  
### <a name="to-export-service-settings"></a>Exportar la configuración de servicios  
  
1.  Anota el nombre del sujeto del certificado y el valor de huella digital del certificado SSL usado por el servicio de federación. Para buscar el certificado SSL, abra la consola administración de Internet Information Services (IIS), seleccione **Sitio web predeterminado** en el panel izquierdo, haga clic en **Enlaces…** en el panel **Acción** , busque y seleccione el enlace HTTPS, haga clic en **Editar**y, a continuación, haga clic en **Ver**.  
  
> [!NOTE]
>  También puedes exportar el certificado SSL que usará el servicio de federación y su clave privada a un archivo .pfx. Para obtener más información, consulta [Exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  La exportación del certificado SSL es opcional porque este certificado se almacena en el almacén de certificados personales del equipo local y se conserva durante la actualización del sistema operativo.  
  
2. Anota la configuración de los certificados de comunicaciones de servicio, descifrado de tokens y firma de tokens de AD FS.  Para ver todos los documentos que se usan, abra Windows PowerShell y ejecute el siguiente comando para agregar los cmdlets de AD FS a su sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para crear una lista de todos los certificados en uso en un archivo `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`.  
  
> [!NOTE]
>  También puedes exportar las claves y los certificados de comunicaciones de servicio, cifrado de tokens o firma de tokens que no se hayan generado internamente, además de todos los certificados autofirmados. Para ver todos los certificados que están en uso en el servidor, usa Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell`. Después, ejecute el siguiente comando para ver todos los certificados que están en uso en el servidor `PSH:>Get-ADFSCertificate`. El resultado de este comando incluye los valores de StoreLocation y StoreName que especifica la ubicación del almacén de cada certificado. Después, usa las instrucciones de [Exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado y su clave privada a un archivo .pfx.  
>   
>  Exportar estos certificados es opcional porque todos los certificados externos se conservan durante la actualización del sistema operativo.  
  
3. Exporta las propiedades del Servicio de federación de AD FS 2.0, como el nombre, el nombre para mostrar y el identificador del servidor de federación, a un archivo.  
  
Para exportar las propiedades del Servicio de federación, abra Windows PowerShell y ejecute el siguiente comando para agregar los cmdlets de AD FS a su sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para exportar las propiedades del Servicio de federación: `PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
El archivo de salida incluirá los siguientes valores de configuración importantes:  
  
    
|**Servicio de federación nombre de la propiedad tal y como lo declara Get-ADFSProperties**|**Servicio de federación nombre de propiedad en la consola de administración de AD FS**|
|------|------|
|HostName|Nombre del Servicio de federación|  
|Identificador|Identificador del Servicio de federación|  
|DisplayName|Nombre para mostrar del Servicio de federación|  
  
4. Haga una copia de seguridad del archivo de configuración de la aplicación. Entre otras opciones de configuración, este archivo contiene la cadena de conexión a la base de datos de directivas.  
  
Para hacer una copia de seguridad del archivo de configuración de la aplicación, debe copiar manualmente el archivo `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` a una ubicación segura en un servidor de copia de seguridad.  
  
> [!NOTE]
>  Anote la cadena de conexión a la base de datos de este archivo, ubicada justo después de "policystore connectionstring=". Si la cadena de conexión especifica una base de datos de SQL Server, el valor es necesario al restaurar la configuración original de AD FS en el servidor de federación.  
>   
>  A continuación se muestra un ejemplo de una cadena de conexión de WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. A continuación se muestra un ejemplo de una cadena de conexión de SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
5. Anota la identidad de la cuenta del Servicio de federación de AD FS 2.0 y su contraseña.  
  
Para buscar el valor de identidad, examine la columna **Iniciar sesión como** de **Servicio de Windows AD FS 2.0** en la consola **Servicios** y registre manualmente este valor.  
  
> [!NOTE]
>  Para un Servicio de federación independiente, se usa la cuenta integrada Servicio de red.  En este caso, no es necesario disponer de contraseña.  
  
6. Exporte la lista de extremos de AD FS habilitados a un archivo.  
  
Para ello, abra Windows PowerShell y ejecute el siguiente comando para agregar los cmdlets de AD FS a su sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para exportar la lista de los extremos de AD FS habilitados a un archivo: `PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7. Exporte las descripciones de notificaciones personalizadas a un archivo.  
  
Para ello, abra Windows PowerShell y ejecute el siguiente comando para agregar los cmdlets de AD FS a su sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para exportar las descripciones de notificaciones personalizadas a un archivo: `Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Paso 2: Exportar las relaciones de confianza para proveedor de notificaciones  
 Para exportar las relaciones de confianza para proveedor de notificaciones, realiza el siguiente procedimiento:  
  
### <a name="to-export-claims-provider-trusts"></a>Exportar las relaciones de confianza para proveedor de notificaciones  
  
1.  Puedes usar Windows PowerShell para exportar todas las relaciones de confianza para proveedor de notificaciones. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para exportar todas las relaciones de confianza para proveedor de notificaciones: `PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Paso 3: Exportar las relaciones de confianza para usuario autenticado  
 Para exportar las relaciones de confianza para usuario autenticado, realiza el siguiente procedimiento:  
  
### <a name="to-export-relying-party-trusts"></a>Exportar las relaciones de confianza para usuario autenticado  
  
1.  Para exportar todas las relaciones de confianza para usuario autenticado, abra Windows PowerShell y ejecute el siguiente comando para agregar los cmdlets de AD FS a su sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Luego, ejecute el siguiente comando para exportar todas las relaciones de confianza para usuario autenticado:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Paso 4: Hacer una copia de seguridad de los almacenes de atributos personalizados  
 Para obtener información acerca de los almacenes de atributos personalizados que utiliza AD FS, usa Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Luego, ejecute el siguiente comando para encontrar información acerca de los almacenes de atributos personalizados: `PSH:>Get-ADFSAttributeStore`. Los pasos para actualizar o migrar almacenes de atributos personalizados varían.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Paso 5: Hacer una copia de seguridad de las personalizaciones de las páginas web  
 Para hacer una copia de seguridad de las personalizaciones de páginas web, copie las páginas web de AD FS y el archivo **web.config** del directorio que está asignado a la ruta de acceso virtual **“/adfs/ls”** en IIS. De forma predeterminada, está en el directorio **%systemdrive%\inetpub\adfs\ls**.  

## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor proxy de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de federación AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar los agentes web de AD FS 1.1](migrate-the-ad-fs-web-agent.md)