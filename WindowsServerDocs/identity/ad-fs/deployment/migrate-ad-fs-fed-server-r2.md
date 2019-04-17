---
title: "Migrar el servidor de AD FS federación 2.0"
description: "Proporciona información sobre cómo migrar un servidor de AD FS a Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bef24f79cfa92dfeca1846501f14ebf6d8231f0d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Migrar el servidor de federación 2.0 AD FS a AD FS en Windows Server 2012 R2

Para migrar un servidor de federación de AD FS pertenece a un conjunto de AD FS nodo único, una granja WIF o un conjunto de SQL Server para Windows Server 2012 R2, debes realizar las siguientes tareas:  
  
1.  [Exportar y copia de seguridad de los datos de configuración de AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Crear una granja de servidores de federación de Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importar los datos de la configuración original de la granja de servidores de AD FS de Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Exportar y copia de seguridad de los datos de configuración de AD FS  
 Para exportar la configuración de AD FS, realiza los siguientes procedimientos:  
  
###  <a name="to-export-service-settings"></a>Para exportar la configuración del servicio  
  
1.  Asegúrate de que tienes acceso a los siguientes certificados y sus claves privadas en un archivo .pfx:  
  
    -   El certificado SSL que se usa en la granja de servidores de federación que vas a migrar  
  
    -   El certificado de comunicación de servicio (si es diferente el certificado SSL) que se usa el conjunto de servidores de servidor de federación que vas a migrar  
  
    -   Todos los certificados de firma de tokens o el token de cifrado y descifrado del terceros usadas por la granja de servidores de federación que vas a migrar  
  
Para encontrar el certificado SSL, abre la administración de Internet Information Services (IIS) de la consola, selecciona **sitio Web predeterminado** en el panel izquierdo, haz clic en **enlaces...** En la **acción** panel, busca y selecciona el enlace de https, haz clic en **editar**y, a continuación, haz clic en **vista**.  
  
Debe exportar el certificado SSL utilizado por el servicio de federación y su clave privada en un archivo pfx. Para obtener más información, consulta [exportar la parte de la clave privada de un certificado de autenticación de servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Si piensas implementar el servicio de registro de dispositivo como parte de la ejecución de tu AD FS en Windows Server 2012 R2, debes obtener un nuevo certificado SSL. Para obtener más información, consulta [inscribir un certificado SSL de AD FS](enroll-an-ssl-certificate-for-ad-fs.md) y [configurar un servidor de federación con el servicio de registro de dispositivo](configure-a-federation-server-with-device-registration-service.md).  
  
Para ver la firma de tokens, descifrado token y certificados de comunicación de servicio que se usan, ejecuta el siguiente comando de Windows PowerShell para crear una lista de todos los certificados en uso en un archivo:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  Exportar propiedades del servicio de federación de AD FS, como el nombre de servicio de federación, el nombre de servicio federación y el identificador de servidor de federación a un archivo.  
  
Para exportar las propiedades del servicio de federación, abre Windows PowerShell y ejecuta el siguiente comando: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

El archivo de salida contendrá los siguientes valores de configuración importantes:  
 
|**Nombre de la propiedad de servicios de federación según lo informado por Get-ADFSProperties**|**Nombre de la propiedad de servicios de federación en la consola de administración de AD FS**|
|-----|-----|  
|Nombre de host|Nombre de servicio de federación|  
|Identificador|Identificador de servicios de federación|  
|DisplayName|Nombre de servicio federación|  
  
3.  Copia el archivo de configuración de la aplicación. Entre otras opciones, este archivo contiene la cadena de conexión de la base de datos de directiva.  
  
Para hacer una copia el archivo de configuración de la aplicación, debes copiar manualmente los `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`archivo a una ubicación segura en un servidor de copia de seguridad.  
  
> [!NOTE]
>  Toma nota de la cadena de conexión de la base de datos en este archivo, ubicados inmediatamente después de "policystore connectionstring =". Si la cadena de conexión especifica una base de datos de SQL Server, el valor es necesario cuando se restaura la configuración de AD FS original en el servidor de federación.  
>   
>  El siguiente es un ejemplo de una cadena de conexión WID:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. El siguiente es un ejemplo de una cadena de conexión de SQL Server:`"Data Source=databasehostname;Integrated Security=True"`.  
  
4.  Registra la identidad de la cuenta de servicios de federación de AD FS y la contraseña de esta cuenta.  
  
Para encontrar el valor de identidad, examinar la **iniciar sesión como** columna de **AD FS 2.0 Windows servicio** en la **servicios** consola y grabar manualmente este valor.  
  
> [!NOTE]
>  Para un servicio de federación independiente, se usa la cuenta predefinida de servicio de red.  En este caso, no es necesario tener una contraseña.  
  
5.  Exportar la lista de los extremos de AD FS habilitados a un archivo.  
  
Para ello, abre Windows PowerShell y ejecuta el siguiente comando: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  Exportar las descripciones de notificación personalizada en un archivo.  
  
Para ello, abre Windows PowerShell y ejecuta el siguiente comando: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  Si tienes una configuración personalizada, como useRelayStateForIdpInitiatedSignOn configurado en el archivo web.config, asegúrate de que copia el archivo web.config de referencia. Puedes copiar el archivo desde el directorio que se asigna a la ruta de acceso virtual **"/ adfs/ls"** en IIS. De manera predeterminada, está en la **%systemdrive%\inetpub\adfs\ls** directorio.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Para exportar el proveedor de notificaciones confía y confía en usuario de confianza  
  
1.  Para exportar AD FS reclamaciones confianzas de proveedor y confiar confianzas de terceros, debe iniciar sesión como administrador (sin embargo, no como el administrador del dominio) en tu federación servidor y ejecuta el siguiente de Windows PowerShell script es se encuentran en la **server_support/media/adfs** carpeta del CD de instalación de Windows Server 2012 R2:`export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  El script de exportación toma los siguientes parámetros:  
>   
>  -   . Ps1 Export-FederationConfiguration-ruta de acceso < string\ > [-ComputerName < string\ >] [-credenciales < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >]  
> -   . Ps1 Export-FederationConfiguration-ruta de acceso < string\ > [-ComputerName < string\ >] [-credenciales < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >]  
> -   . Ps1 Export-FederationConfiguration-ruta de acceso < string\ > [-ComputerName < string\ >] [-credenciales < pscredential\ >] [-Force] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
>   
>  **-RelyingPartyTrustIdentifier < string [] >** - las exportaciones solo cmdlet confiar confianzas de terceros cuyos identificadores se especifican en la matriz de cadenas. El valor predeterminado es exportar ninguna de las confianzas de terceros de confianza. Si no se especifica ninguno de RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName y ClaimsProviderTrustName, el script exportará todos confianzas de usuario de confianza y dice confianzas de proveedor.  
>   
>  **-ClaimsProviderTrustIdentifier < string [] >** -el cmdlet solo exporta confianzas de proveedor de reclamaciones cuyos identificadores se especifican en la matriz de cadenas. El valor predeterminado es exportar ninguna de las relaciones de confianza del proveedor de notificaciones.  
>   
>  **-RelyingPartyTrustName < string [] >** - las exportaciones solo cmdlet confiar confianzas de terceros cuyos nombres se especifican en la matriz de cadenas. El valor predeterminado es exportar ninguna de las confianzas de terceros de confianza.  
>   
>  **-ClaimsProviderTrustName < string [] >** -el cmdlet solo exporta confianzas de proveedor de reclamaciones cuyos nombres se especifican en la matriz de cadenas. El valor predeterminado es exportar ninguna de las relaciones de confianza del proveedor de notificaciones.  
>   
>  **-Path < string\ >** -la ruta de acceso a una carpeta que contenga los archivos exportados.  
>   
>  **-ComputerName < string\ >** -especifica el nombre de host del servidor STS. El valor predeterminado es el equipo local. Si vas a migrar AD FS 2.0 o AD FS en Windows Server 2012 en AD FS en Windows Server 2012 R2, este es el nombre de host del servidor de AD FS heredado.  
>   
>  **-Credenciales < PSCredential\ >** -especifica una cuenta de usuario que tenga permiso para realizar esta acción. El valor predeterminado es el usuario actual.  
>   
>  **-Forzar** : Especifica que no pedir confirmación del usuario.  
>   
>  **-CertificatePassword < SecureString\ >** -especifica una contraseña para exportar las claves privadas de los certificados de AD FS. Si no se especifica, el script te pedirá una contraseña si necesita un certificado de AD FS con la clave privada se pueda exportar.  
>   
>  **Entradas**: None  
>   
>  **Salidas**: cadena - este cmdlet devuelve la ruta de carpeta de exportación. Puede canalizar el objeto devuelto a Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Copia el atributo personalizado tiendas  
  
1.  Todos los almacenes del atributo personalizado que desea conservar en el conjunto de AD FS nuevo en Windows Server 2012 R2 debe exportar manualmente.  
  
> [!NOTE]
>  En Windows Server 2012 R2, AD FS requiere almacenes de atributo personalizado que se basan en .NET Framework 4.0 o posterior. Sigue las instrucciones de [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) instalar y la instalación de .net Framework 4.5.  
  
Encontrarás información acerca de los almacenes del atributo personalizado en uso por AD FS ejecutando el siguiente comando de Windows PowerShell: 

``` powershell
Get-ADFSAttributeStore
```  

Los pasos para actualizar o migrar el atributo personalizado almacenes varían.  
  
2.  Todos los archivos .dll de las tiendas de atributo personalizado que desea conservar en el conjunto de AD FS nuevo en Windows Server 2012 R2 debe exportar manualmente. Los pasos para actualizar o migrar los archivos .dll de tiendas de atributo personalizado varían.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Crear una granja de servidores de federación de Windows Server 2012 R2  
  
1.  Instalar el sistema operativo de Windows Server 2012 R2 en un equipo que desea que funcionan como un servidor de federación y, a continuación, agrega el rol de servidor de AD FS. Para obtener más información, consulta [instalar el servicio de rol de AD FS](install-the-ad-fs-role-service.md). A continuación, configurar los servicios de federación de nuevo mediante el Asistente para la configuración de los servicios de federación de Active Directory o a través de Windows PowerShell. Para obtener más información, consulta "Configurar el primer servidor de federación en un nuevo conjunto de servidor de federación" en [configurar un servidor de federación](configure-a-federation-server.md).  

Al completar este paso, debes seguir estas instrucciones:  
  
-   Para configurar los servicios de federación debe tener privilegios de administrador de dominio.  
  
-   Debes usar el mismo nombre de servicio de federación (nombre de la granja de servidores) que se utilizó en AD FS 2.0 o AD FS en Windows Server 2012. Si no usas el mismo nombre de servicio de federación, los certificados que una copia de seguridad no funcionarán en el servicio de federación de Windows Server 2012 R2 que está intentando configurar.  
  
-   Especifica si se trata de una granja de servidores de federación WID o SQL Server. Si es un conjunto de SQL, especificar la ubicación de la base de datos de SQL Server y el nombre de la instancia.  
  
-   Debes proporcionar un archivo pfx que contenga el certificado de autenticación de servidor SSL que se hizo copia de seguridad como parte de la preparación para el proceso de migración de AD FS.  
  
-   Debes especificar la misma identidad de cuenta de servicio que se usó en AD FS 2.0 o AD FS en granja de servidores de Windows Server 2012.  
  
2.  Una vez configurado el nodo inicial, puedes agregar nodos adicionales al conjunto de nuevo. Para obtener más información, consulta "Agregar un servidor de federación a un conjunto de servidor de federación existente" en [configurar un servidor de federación](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importar los datos de la configuración original de la granja de servidores de AD FS de Windows Server 2012 R2  
 Ahora que ya tienes una granja de servidores de federación de AD FS ejecutándose en Windows Server 2012 R2, puede importar los datos de configuración de AD FS originales en él.  
  
1.  Importar y configurar otros certificados de AD FS personalizados, incluidas inscritos externamente certificados de firma de token y el cifrado de descifrado de token y el certificado de comunicación de servicio si es diferente en el certificado SSL.  
  
En la consola de administración de AD FS, selecciona **certificados**. Comprobar los certificados de token de cifrado y descifrado y la firma de tokens de comunicaciones, servicio comprobando cada uno con los valores exportado en el archivo certificates.txt durante la preparación para la migración.  
  
Para cambiar los certificados de descifrado de token o la firma de tokens de los certificados autofirmados predeterminado a certificados externos, primero debes deshabilitar la característica de sustitución de certificados automática está habilitada de manera predeterminada. Para ello, puedes usar el siguiente comando de Windows PowerShell:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  Configurar cualquier configuración personalizada de servicio de AD FS como AutoCertificateRollover o SSO duración mediante el cmdlet Set-AdfsProperties.  
  
3.  Para importar confianzas de terceros de confianza de AD FS y proveedor de reclamaciones, debe iniciar sesión como administrador (sin embargo, no como el administrador del dominio) en tu federación servidor y ejecuta el siguiente de Windows PowerShell script es se encuentran en la carpeta de \support\adfs del CD de instalación de Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  El script de importación toma los siguientes parámetros:  
>   
>  -  Importar FederationConfiguration.ps1-ruta de acceso < string\ > [-ComputerName < string\ >] [-credenciales < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >]  
> -   Importar FederationConfiguration.ps1-ruta de acceso < string\ > [-ComputerName < string\ >] [-credenciales < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >  
> -   Importar FederationConfiguration.ps1-ruta de acceso < string\ > [-ComputerName < string\ >] [-credenciales < pscredential\ >] [-Force] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
>   
>  **-RelyingPartyTrustIdentifier < string [] >** - las importaciones solo cmdlet confiar confianzas de terceros cuyos identificadores se especifican en la matriz de cadenas. El valor predeterminado es importar ninguna de las confianzas de terceros de confianza. Si no se especifica ninguno de RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName y ClaimsProviderTrustName, el script se encargará de importar todo confianzas de usuario de confianza y dice confianzas de proveedor.  
>   
>  **-ClaimsProviderTrustIdentifier < string [] >** -el cmdlet solo importa confianzas de proveedor de reclamaciones cuyos identificadores se especifican en la matriz de cadenas. El valor predeterminado es importar ninguna de las relaciones de confianza del proveedor de notificaciones.  
>   
>  **-RelyingPartyTrustName < string [] >** - las importaciones solo cmdlet confiar confianzas de terceros cuyos nombres se especifican en la matriz de cadenas. El valor predeterminado es importar ninguna de las confianzas de terceros de confianza.  
>   
>  **-ClaimsProviderTrustName < string [] >** -el cmdlet solo importa confianzas de proveedor de reclamaciones cuyos nombres se especifican en la matriz de cadenas. El valor predeterminado es importar ninguna de las relaciones de confianza del proveedor de notificaciones.  
>   
>  **-Path < string\ >** -la ruta de acceso a una carpeta que contiene los archivos de configuración que desea importar.  
>   
>  **-LogPath < string\ >** -la ruta de acceso a una carpeta que contenga el archivo de registro de importación. En esta carpeta se creará un archivo de registro denominado "import.log".  
>   
>  **-ComputerName < string\ >** -especifica el nombre de host del servidor STS. El valor predeterminado es el equipo local. Si vas a migrar AD FS 2.0 o AD FS en Windows Server 2012 en AD FS en Windows Server 2012 R2, este parámetro debe establecerse en el nombre de host del servidor de AD FS heredado.  
>   
>  **-Credenciales < PSCredential\ >**-especifica una cuenta de usuario que tenga permiso para realizar esta acción. El valor predeterminado es el usuario actual.  
>   
>  **-Forzar** : Especifica que no pedir confirmación del usuario.  
>   
>  **-CertificatePassword < SecureString\ >** -especifica una contraseña para importar claves privadas de los certificados de AD FS. Si no se especifica, el script te pedirá una contraseña si es necesario importar un certificado de AD FS con la clave privada.  
>   
>  **Entradas:** cadena - este comando adopta la ruta de acceso de carpeta de importación como entrada. Puede canalizar exportación FederationConfiguration a este comando.  
>   
>  **Salidas:** ninguno.  
  
Espacios en la propiedad WSFedEndpoint de una usuario de confianza confianza de terceros pueden provocar que el script de importación al error. En este caso, quitar manualmente los espacios de importar el antes de archivo. Por ejemplo, estas entradas provocar errores:  
  
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
>  Si tienes cualquier notificación personalizada reglas (que no sean las reglas predeterminadas de AD FS) en la confianza de proveedor de Active Directory notificaciones en el sistema de origen, estos no se migrarán los scripts. Esto es porque Windows Server 2012 R2 tiene nuevos valores predeterminados. Las reglas personalizadas deben combinarse agregándolos manualmente a la confianza de proveedor de reclamaciones de Active Directory en el nuevo conjunto de Windows Server 2012 R2.  
  
4.  Configurar todas las opciones de extremo de AD FS personalizadas. En la consola de administración de AD FS, seleccione **extremos**. Comprueba los extremos de AD FS habilitados con la lista de los extremos de AD FS habilitados que exportar a un archivo durante la preparación para la migración de AD FS.  
  
     \ (Y)  
  
     Configurar las descripciones de notificación personalizada. En la consola de administración de AD FS, seleccione **descripciones de Reclamación**. Compruebe la lista de descripciones de notificación de AD FS con la lista de descripciones de notificación que exportación a un archivo durante la preparación para la migración de AD FS. Agrega las descripciones de notificación personalizada incluir en el archivo pero no incluido en la lista predeterminada de AD FS. Ten en cuenta que el identificador de la notificación en la consola de administración se asigna a ClaimType en el archivo.  
  
5.  Instalar y configurar personalizado copia todos los almacenes de atributo. Como administrador, asegúrate de que todos los archivos binarios de atributo personalizado tienda sean actualización a .NET Framework 4.0 o posterior antes de actualizar la configuración de AD FS que apunta a ellos.  
  
6.  Configura las propiedades de servicio que se asignan a los parámetros de archivo de web.config heredado.  
  
    -   Si **useRelayStateForIdpInitiatedSignOn** se agregó a la **web.config** del archivo en tu AD FS 2.0 o AD FS conjunto de Windows Server 2012, debe configurar las siguientes propiedades del servicio de tu AD FS en granja de servidores de Windows Server 2012 R2:  
  
        -   AD FS en Windows Server 2012 R2 incluye un **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** archivo. Crea un elemento con la misma sintaxis como el **web.config** elemento del archivo: `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Este elemento se incluyen como parte de **< microsoft.identityserver.web >** sección de la **Microsoft.IdentityServer.Servicehost.exe.config** archivo.  
  
    -   Si **< persistIdentityProviderInformation habilitado = "" true "& #124; false" lifetimeInDays = "90" enablewhrPersistence = "" true "& #124;" false"" / \ >** se agregó a la **web.config** del archivo en tu AD FS 2.0 o AD FS conjunto de Windows Server 2012, debe configurar las siguientes propiedades del servicio de tu AD FS en granja de servidores de Windows Server 2012 R2:  
  
        1.  En AD FS en Windows Server 2012 R2, ejecuta el siguiente comando de Windows PowerShell: `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
    -   Si **< ensamblados singleSignOn habilitado = "" true "& #124;" false"" / \ >** se agregó a la **web.config** archivo en tu AD FS 2.0 o AD FS en granja de servidores de Windows Server 2012, no debes establecer las propiedades de servicio adicional en tu AD FS en granja de servidores de Windows Server 2012 R2. Inicio de sesión único está habilitada de manera predeterminada en AD FS en granja de servidores de Windows Server 2012 R2.  
  
    -   Si se agregaron localAuthenticationTypes configuración a la **web.config** del archivo en tu AD FS 2.0 o AD FS conjunto de Windows Server 2012, debe configurar las siguientes propiedades del servicio de tu AD FS en granja de servidores de Windows Server 2012 R2:  
  
        -   Integrado, formularios, TlsClient, lista de transformación básica en ADFS equivalente en Windows Server 2012 R2 tiene la configuración de directiva global de autenticación para admitir ambos tipos de autenticación de proxy y el servicio de federación. Estas opciones de configuración se pueden configurar en la AD FS en el complemento de administración en la **directivas de autenticación**.  
  
 Después de importar los datos de la configuración original, puedes personalizar el inicio de sesión AD FS páginas según sea necesario. Para obtener más información, consulta [personalización de las páginas de signo en AD FS](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparación para migrar el servidor de federación de AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrar al Proxy de servidor de federación de AD FS](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)