---
title: Migrar el servidor de federación 2.0 de AD FS
description: Proporciona información sobre cómo migrar un servidor de AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46d0b643d786443093e0dfeafe4cddde3278ff76
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444624"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Migrar el servidor de AD FS 2.0 federation a AD FS en Windows Server 2012 R2

Para migrar un servidor de federación de AD FS que pertenezca a una granja de servidores de AD FS de nodo único, una granja WIF o una granja de servidores de SQL Server a Windows Server 2012 R2, debe realizar las siguientes tareas:  
  
1.  [Exportar y copia de seguridad de los datos de configuración de AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Crear una granja de servidores de federación de Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importar los datos de configuración original en la granja de servidores de Windows Server 2012 R2 AD FS](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Exportar y hacer una copia de seguridad de los datos de configuración de AD FS  
 Para exportar las opciones de configuración de AD FS, siga estos procedimientos:  
  
###  <a name="to-export-service-settings"></a>Exportar la configuración de servicios  
  
1.  Asegúrese de que tiene acceso a los siguientes certificados y a sus claves privadas en un archivo .pfx:  
  
    -   El certificado SSL utilizado por la granja de servidores de federación que desea migrar  
  
    -   El certificado de comunicación de servicios (si es distinto del certificado SSL) utilizado por la granja de servidores de federación que desea migrar  
  
    -   Todos los certificados de firma de tokens o cifrado y descifrado de tokens de terceros que utiliza la granja de servidores de federación que desea migrar  
  
Para buscar el certificado SSL, abra la consola administración de Internet Information Services (IIS), seleccione **Sitio web predeterminado** en el panel izquierdo, haga clic en **Enlaces…** en el panel **Acción** , busque y seleccione el enlace HTTPS, haga clic en **Editar**y, a continuación, haga clic en **Ver**.  
  
Debe exportar el certificado SSL que utilizará el Servicio de federación y su clave privada a un archivo .pfx. Para obtener más información, consulta [Exportar la parte de la clave privada de un certificado de autenticación de servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Si tiene previsto implementar el servicio de registro de dispositivos como parte de la ejecución de AD FS en Windows Server 2012 R2, debe obtener un nuevo certificado SSL. Para obtener más información, consulte [Enroll an SSL Certificate for AD FS](enroll-an-ssl-certificate-for-ad-fs.md) y [Configure a federation server with Device Registration Service](configure-a-federation-server-with-device-registration-service.md).  
  
Para ver los certificados de firma de tokens, descifrado de tokens y comunicación de servicios que se utilizan, ejecute el siguiente comando de Windows PowerShell para crear una lista de todos los certificados que se usan en un archivo:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2. Exporte las propiedades del Servicio de federación de AD FS, como su nombre, su nombre para mostrar y el identificador del servidor de federación, a un archivo.  
  
Para exportar las propiedades del servicio de federación, abra Windows PowerShell y ejecute el siguiente comando: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

El archivo de salida incluirá los siguientes valores de configuración importantes:  
 
|**Nombre de propiedad del servicio de federación devuelto por Get-ADFSProperties**|**Nombre de propiedad del servicio de federación en la consola de administración de AD FS**|
|-----|-----|  
|HostName|Nombre del Servicio de federación|  
|Identificador|Identificador del Servicio de federación|  
|DisplayName|Nombre para mostrar del Servicio de federación|  
  
3. Haga una copia de seguridad del archivo de configuración de la aplicación. Entre otras opciones de configuración, este archivo contiene la cadena de conexión a la base de datos de directivas.  
  
Para hacer una copia de seguridad del archivo de configuración de la aplicación, debe copiar manualmente el archivo `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` a una ubicación segura en un servidor de copia de seguridad.  
  
> [!NOTE]
>  Anota la cadena de conexión a la base de datos de este archivo, ubicada justo después de «policystore connectionstring=». Si la cadena de conexión especifica una base de datos de SQL Server, el valor es necesario al restaurar la configuración original de AD FS en el servidor de federación.  
>   
>  A continuación se muestra un ejemplo de una cadena de conexión de WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. A continuación se muestra un ejemplo de una cadena de conexión de SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
4. Registre la identidad de la cuenta del Servicio de federación de AD FS y su contraseña.  
  
Para buscar el valor de identidad, examine la columna **Iniciar sesión como** de **Servicio de Windows AD FS 2.0** en la consola **Servicios** y registre manualmente este valor.  
  
> [!NOTE]
>  Para un Servicio de federación independiente, se usa la cuenta integrada Servicio de red.  En este caso, no es necesario disponer de contraseña.  
  
5. Exporte la lista de extremos de AD FS habilitados a un archivo.  
  
Para ello, abre Windows PowerShell y ejecuta el comando siguiente: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6. Exporte las descripciones de notificaciones personalizadas a un archivo.  
  
Para ello, abre Windows PowerShell y ejecuta el comando siguiente: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7. Si tiene opciones personalizadas, como useRelayStateForIdpInitiatedSignOn, configuradas en el archivo web.config, asegúrese de hacer una copia de seguridad del archivo web.config para consultarlo. Puede copiar el archivo desde el directorio asignado a la ruta de acceso virtual **“/adfs/ls”** en IIS. De forma predeterminada, está en el directorio **%systemdrive%\inetpub\adfs\ls**.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Exportar las relaciones de confianza para proveedor de notificaciones y las relaciones de confianza para usuario autenticado  
  
1.  Para exportar a AD FS proveedor de notificaciones y autenticado, debe iniciar sesión como administrador (sin embargo, no como administrador de dominio) en la federación server y ejecute el siguiente comando de Windows PowerShell script se encuentran en la **media / adfs/server_support** carpeta del CD de instalación de Windows Server 2012 R2: `export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  El script de exportación admite los siguientes parámetros:  
> 
> - Export-FederationConfiguration.ps1 -Path <string\> [-ComputerName <string\>] [-Credential <pscredential\>] [-Force] [-CertificatePassword <securestring\>]  
>   -   Exportación FederationConfiguration.ps1-ruta de acceso < cadena\> [-ComputerName < cadena\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>] [- RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >]  
>   -   Exportación FederationConfiguration.ps1-ruta de acceso < cadena\> [-ComputerName < cadena\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>] [- RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** : el cmdlet solo exporta las relaciones de confianza para usuario autenticado cuyos identificadores se hayan especificado en la matriz de cadena. El valor predeterminado es no exportar ninguna de las relaciones de confianza para usuario autenticado. Si no se especifica ninguno de los parámetros RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName y ClaimsProviderTrustName, el script exportará todas las relaciones de confianza para usuario autenticado y todas las relaciones de confianza para proveedor de notificaciones.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** : el cmdlet solo exporta las relaciones de confianza para proveedor de notificaciones cuyos identificadores se hayan especificado en la matriz de cadena. El valor predeterminado es no exportar ninguna de las relaciones de confianza para proveedor de notificaciones.  
> 
>   **-RelyingPartyTrustName <string[]>** : el cmdlet solo exporta las relaciones de confianza para usuario autenticado cuyos nombres se hayan especificado en la matriz de cadena. El valor predeterminado es no exportar ninguna de las relaciones de confianza para usuario autenticado.  
> 
>   **-ClaimsProviderTrustName <string[]>** : el cmdlet solo exporta las relaciones de confianza para proveedor de notificaciones cuyos nombres se hayan especificado en la matriz de cadena. El valor predeterminado es no exportar ninguna de las relaciones de confianza para proveedor de notificaciones.  
> 
>   **-Path < string\>**  -la ruta de acceso a una carpeta que contendrá los archivos exportados.  
> 
>   **-ComputerName < string\>**  : especifica el nombre de host del servidor STS. El valor predeterminado es el equipo local. Si realiza la migración de AD FS 2.0 o AD FS en Windows Server 2012 a AD FS en Windows Server 2012 R2, es el nombre de host del servidor de AD FS heredado.  
> 
>   **-Credential < PSCredential\>**  : especifica una cuenta de usuario que tenga permiso para realizar esta acción. El valor predeterminado es el usuario actual.  
> 
>   **-Force** : especifica que no se solicitará confirmación por parte del usuario.  
> 
>   **-CertificatePassword < SecureString\>**  : especifica una contraseña para exportar las claves privadas de certificados de AD FS. Si no se especifica, el script solicitará una contraseña si se debe exportar un certificado de AD FS con una clave privada.  
> 
>   **Entradas**: Ninguno  
> 
>   **Salida**: un valor de cadena; este cmdlet devuelve la ruta de acceso a la carpeta de exportación. Puede canalizar el objeto devuelto a Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Hacer una copia de seguridad de los almacenes de atributos personalizados  
  
1.  Debe exportar manualmente todos los almacenes de atributos personalizados que desee mantener en la nueva granja de AD FS en Windows Server 2012 R2.  
  
> [!NOTE]
>  En Windows Server 2012 R2, AD FS requiere almacenes de atributos personalizados que se basan en .NET Framework 4.0 o posterior. Sigue las instrucciones de [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) para instalar y configurar .NET Framework 4.5.  
  
Encontrará información acerca de los almacenes de atributos personalizados que utiliza AD FS si ejecuta el siguiente comando de Windows PowerShell: 

``` powershell
Get-ADFSAttributeStore
```  

Los pasos para actualizar o migrar almacenes de atributos personalizados varían.  
  
2. Debe exportar manualmente todos los archivos .dll de los almacenes de atributos personalizados que desee mantener en la nueva granja de AD FS en Windows Server 2012 R2. Los pasos para actualizar o migrar los archivos .dll de los almacenes de atributos personalizados varían.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Crear una granja de servidores de federación de Windows Server 2012 R2  
  
1.  Instalar el sistema operativo Windows Server 2012 R2 en un equipo que desea que funcione como un servidor de federación y, a continuación, agregar el rol de servidor de AD FS. Para obtener más información, consulte [Install the AD FS Role Service](install-the-ad-fs-role-service.md). A continuación, configure el nuevo Servicio de federación mediante el Asistente de configuración de Servicios de federación de Active Directory o por medio de Windows PowerShell. Para obtener más información, vea "Configurar el primer servidor de federación en una nueva granja de servidores de federación" en [Configurar un servidor de federación](configure-a-federation-server.md).  

Mientras realice este paso, debe seguir estas instrucciones:  
  
-   Debe tener privilegios de administrador de dominio para configurar el Servicio de federación.  
  
-   Debe utilizar el mismo nombre de Servicio de federación (nombre de granja de servidores) que se utilizaba en AD FS 2.0 o AD FS en Windows Server 2012. Si no usa el mismo nombre de servicio de federación, los certificados que se hizo copia de seguridad no funcionarán en el servicio de federación de Windows Server 2012 R2 que está intentando configurar.  
  
-   Especifique si se trata de una granja de servidores de federación de WID o SQL Server. Si es una granja SQL, especifique la ubicación y el nombre de instancia de la base de datos de SQL Server.  
  
-   Debe proporcionar un archivo pfx que contenga el certificado de autenticación SSL del que hizo copia de seguridad como parte de la preparación del proceso de migración de AD FS.  
  
-   Debe especificar la misma identidad de cuenta de servicio que se usaba en la granja de AD FS 2.0 o AD FS en Windows Server 2012.  
  
2. En cuanto se haya configurado el nodo inicial, puede agregar nodos adicionales a la nueva granja. Para obtener más información, vea "Agregar un servidor de federación a una granja de servidores de federación existente" en [Configurar un servidor de federación](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importar los datos de configuración originales a la granja de AD FS de Windows Server 2012 R2  
 Ahora que tiene una granja de servidores de federación de AD FS que se ejecuta en Windows Server 2012 R2, puede importar los datos de configuración de AD FS originales en él.  
  
1.  Importe y configure otros certificados personalizados de AD FS, incluidos los certificados de firma de tokens y cifrado y descifrado de tokens inscritos externamente y el certificado de comunicación de servicios si es distinto del certificado SSL.  
  
En la consola de administración de AD FS, seleccione **Certificados**. Para comprobar los certificados de comunicaciones de servicios, cifrado y descifrado de tokens y firma de tokens, compárelos con los valores que exportó al archivo certificates.txt mientras preparaba la migración.  
  
Para cambiar los certificados de descifrado o de firma de tokens de los valores autofirmados predeterminados a certificados externos, primero debe deshabilitar la característica de sustitución automática de certificados habilitada de manera predeterminada. Para ello, puede utilizar el siguiente comando de Windows PowerShell:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2. Configure las opciones personalizadas del servicio AD FS, como AutoCertificateRollover o la duración de SSO con el cmdlet Set-AdfsProperties.  
  
3. Para importar AD FS autenticado y confianzas de proveedores de notificaciones, debe iniciar sesión como administrador (sin embargo, no como administrador de dominio) en la federación server y ejecute el siguiente comando de Windows PowerShell script se encuentran en la carpeta \support\adfs del CD de instalación de Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  El script de importación admite los siguientes parámetros:  
> 
> - Import-FederationConfiguration.ps1 -Path <string\> [-ComputerName <string\>] [-Credential <pscredential\>] [-Force] [-LogPath <string\>] [-CertificatePassword <securestring\>]  
>   -   Importación FederationConfiguration.ps1-ruta de acceso < cadena\> [-ComputerName < cadena\>] [-Credential < pscredential\>] [-Force] [-LogPath < cadena\>] [-CertificatePassword < securestring \>] [-RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >  
>   -   Importación FederationConfiguration.ps1-ruta de acceso < cadena\> [-ComputerName < cadena\>] [-Credential < pscredential\>] [-Force] [-LogPath < cadena\>] [-CertificatePassword < securestring \>] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** : el cmdlet solo importa las relaciones de confianza para usuario autenticado cuyos identificadores se hayan especificado en la matriz de cadena. El valor predeterminado es no importar ninguna de las relaciones de confianza para usuario autenticado. Si no se especifica ninguno de los parámetros RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName y ClaimsProviderTrustName, el script importará todas las relaciones de confianza para usuario autenticado y todas las relaciones de confianza para proveedor de notificaciones.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** : el cmdlet solo importa las relaciones de confianza para proveedor de notificaciones cuyos identificadores se hayan especificado en la matriz de cadena. El valor predeterminado es no importar ninguna de las relaciones de confianza para proveedor de notificaciones.  
> 
>   **-RelyingPartyTrustName <string[]>** : el cmdlet solo importa las relaciones de confianza para usuario autenticado cuyos nombres se hayan especificado en la matriz de cadena. El valor predeterminado es no importar ninguna de las relaciones de confianza para usuario autenticado.  
> 
>   **-ClaimsProviderTrustName <string[]>** : el cmdlet solo importa las relaciones de confianza para proveedor de notificaciones cuyos nombres se hayan especificado en la matriz de cadena. El valor predeterminado es no importar ninguna de las relaciones de confianza para proveedor de notificaciones.  
> 
>   **-Path < string\>**  -la ruta de acceso a una carpeta que contiene los archivos de configuración que desea importar.  
> 
>   **-LogPath < cadena\>**  -la ruta de acceso a una carpeta que contendrá el archivo de registro de importación. En esta carpeta se creará un archivo de registro denominado "import.log".  
> 
>   **-ComputerName < string\>**  : especifica el nombre de host del servidor STS. El valor predeterminado es el equipo local. Si realiza la migración de AD FS 2.0 o AD FS en Windows Server 2012 a AD FS en Windows Server 2012 R2, este parámetro debe establecerse en el nombre de host del servidor de AD FS heredado.  
> 
>   **-Credential < PSCredential\>** : especifica una cuenta de usuario que tenga permiso para realizar esta acción. El valor predeterminado es el usuario actual.  
> 
>   **-Force** : especifica que no se solicitará confirmación por parte del usuario.  
> 
>   **-CertificatePassword < SecureString\>**  : especifica una contraseña para importar las claves privadas de certificados de AD FS. Si no se especifica, el script solicitará una contraseña si se debe importar un certificado de AD FS con una clave privada.  
> 
>   **Entrada:** cadena; este comando toma la ruta de acceso de la carpeta de importación como entrada. Puede canalizar Export-FederationConfiguration a este comando.  
> 
>   **Salidas:** Ninguno.  
  
Los espacios finales en la propiedad WSFedEndpoint de una relación de confianza para usuario autenticado pueden provocar un error en el script de importación. En este caso, quite manualmente los espacios del archivo antes de realizar la importación. Por ejemplo, estas entradas pueden causar errores:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  Si tiene reglas de notificación personalizadas (distintas de las predeterminadas de AD FS) en la relación de confianza para proveedor de notificaciones de Active Directory del sistema de origen, no se migrarán mediante los scripts. Esto es porque Windows Server 2012 R2 tiene nuevos valores predeterminados. Las reglas personalizadas se deben combinar agregándolas manualmente a la confianza de proveedor de notificaciones de Active Directory en la nueva granja de servidores de Windows Server 2012 R2.  
  
4. Configure todas las opciones personalizadas de los extremos de AD FS. En la consola de administración de AD FS, seleccione **Extremos**. Compare los extremos de AD FS habilitados con la lista de extremos de AD FS habilitados que exportó a un archivo mientras preparaba la migración de AD FS.  
  
    \- y -  
  
    Configure las descripciones de notificaciones personalizadas. En la Consola de administración de AD FS, seleccione **Descripciones de notificaciones**. Compara la lista de descripciones de notificaciones de AD FS con la lista de descripciones de notificaciones que exportaste a un archivo mientras preparabas la migración de AD FS. Agregue las descripciones de notificaciones personalizadas incluidas en el archivo que no se estén incluidas en la lista predeterminada de AD FS. Tenga en cuenta que el identificador de la notificación en la consola de administración corresponde a ClaimType en el archivo.  
  
5. Instale y configure todos los almacenes de atributos personalizados de los que haya hecho copia de seguridad. Como administrador, asegúrese de que los archivos binarios de los almacenes de atributos personalizados estén actualizados a .NET Framework 4.0 o superior antes de actualizar la configuración de AD FS para que los incluya.  
  
6. Configure las propiedades del servicio correspondientes a los parámetros del archivo web.config heredados.  
  
   -   Si **useRelayStateForIdpInitiatedSignOn** se agregó a la **web.config** de archivos de AD FS 2.0 o AD FS en la granja de servidores de Windows Server 2012, debe configurar las siguientes propiedades del servicio de AD FS en Granja de servidores de Windows Server 2012 R2:  
  
       -   AD FS en Windows Server 2012 R2 incluye un **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** archivo. Cree un elemento con la misma sintaxis que el **web.config** elemento file: `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Incluya este elemento como parte de **< microsoft.identityserver.web >** sección de la **Microsoft.IdentityServer.Servicehost.exe.config** archivo.  
  
   -   Si **< persistIdentityProviderInformation habilitado = "true&#124;false" lifetimeInDays = enablewhrPersistence "90" = "true&#124;false" /\>**  se agregó a la **web.config** archivo de AD FS 2.0 o AD FS en la granja de servidores de Windows Server 2012, debe configurar las siguientes propiedades del servicio de AD FS en la granja de servidores de Windows Server 2012 R2:  
  
       1.  En AD FS en Windows Server 2012 R2, ejecute el siguiente comando de Windows PowerShell: `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
   -   Si **< inicio de sesión único habilitado = "true&#124;false" /\>**  se agregó a la **web.config** archivos de AD FS 2.0 o AD FS en la granja de servidores de Windows Server 2012, no es necesario establecer cualquier servicio adicional propiedades de AD FS en la granja de servidores de Windows Server 2012 R2. Inicio de sesión único está habilitado de forma predeterminada en AD FS en la granja de servidores de Windows Server 2012 R2.  
  
   -   Si la configuración de localAuthenticationTypes se agregaron a la **web.config** de archivos de AD FS 2.0 o AD FS en la granja de servidores de Windows Server 2012, debe configurar las siguientes propiedades del servicio de AD FS en la granja de servidores de Windows Server 2012 R2:  
  
       -   Integrado, Forms, TlsClient, lista Basic Transform en AD FS equivalente en Windows Server 2012 R2 tiene configuración de directiva de autenticación global para admitir ambos tipos de autenticación de proxy y el servicio de federación. Estas opciones se pueden configurar en el complemento de administración de AD FS, en **Directivas de autenticación**.  
  
   Tras importar los datos de configuración originales, puede personalizar las páginas de inicio de sesión de AD FS como resulte necesario. Para obtener más información, consulte [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de servicios de federación de Active Directory a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparar la migración del servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migración del servidor Proxy de federación de AD FS](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)