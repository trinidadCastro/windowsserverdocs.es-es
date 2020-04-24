---
title: IIS en Nano Server
description: Detalles de configuración de IIS en Nano Server
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.date: 09/06/2017
ms.assetid: 16984724-2d77-4d7b-9738-3dff375ed68c
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: d2522dc94c0d3b68c75e14fec19466529256aad0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826868"
---
# <a name="iis-on-nano-server"></a>IIS en Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consulta [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información. 

Puede instalar el rol de servidor de Internet Information Services (IIS) en Nano Server mediante el parámetro -Package con Microsoft-NanoServer-IIS-Package. Para obtener información sobre cómo configurar Nano Server, incluida la instalación de paquetes, vea [Instalación de Nano Server](Getting-Started-with-Nano-Server.md).  

En esta versión de Nano Server, están disponibles las siguientes características de IIS:  

|Característica|Habilitado de forma predeterminada|  
|-----------|----------------------|  
|**Características HTTP comunes**||  
|Documento predeterminado|x|  
|Examen de directorios|x|  
|Errores HTTP|x|  
|Contenido estático|x|  
|Redirección HTTP||  
|**Estado y diagnóstico**||  
|Registro HTTP|x|  
|Registro personalizado||  
|Monitor de solicitudes||  
|Seguimiento||  
|**Rendimiento**||  
|Compresión de contenido estático|x|  
|Compresión de contenido dinámico||  
|**Seguridad**||  
|Filtro de solicitudes|x|  
|Autenticación básica||  
|Autenticación de asignaciones de certificado de cliente||  
|Autenticación implícita||  
|Autenticación de asignaciones de certificado de cliente de IIS||  
|Restricciones de dominio y dirección IP||  
|Autorización para URL||  
|Autenticación de Windows.||  
|**Desarrollo de aplicaciones**||  
|Inicialización de aplicaciones||  
|CGI||  
|Extensiones ISAPI||  
|Filtros ISAPI||  
|Inclusiones del lado servidor||  
|Protocolo WebSocket||  
|**Herramientas de administración**||  
|Módulo IISAdministration para Windows PowerShell|x|  

Una serie de artículos sobre otras configuraciones de IIS (por ejemplo, con ASP.NET, PHP y Java) y otros relacionados con el contenido están publicados en [http://iis.net/learn](https://iis.net/learn).  

## <a name="installing-iis-on-nano-server"></a>Instalación de IIS en Nano Server  
Puede instalar este rol de servidor sin conexión (con Nano Server desactivado) o en línea (con Nano Server activado); la instalación sin conexión es la opción recomendada.  

Para una instalación sin conexión, agregue el paquete con el parámetro -Packages de New-NanoServerImage, como en este ejemplo:  

`New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package`  

Si tiene un archivo VHD existente, puede instalar IIS sin conexión con DISM.exe; para ello, monte el VHD y luego use la opción **Add-Package**.   
En los pasos de ejemplo siguientes se supone que la ejecución se realiza desde el directorio especificado por la opción BasePath, creado después de ejecutar New-NanoServerImage.  

1.  mkdir mountdir
2.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir
3.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\Microsoft-NanoServer-IIS-Package.cab /Image:.\mountdir
4.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab /Image:.\mountdir
5.  .\Tools\dism.exe /Unmount-Image /MountDir:.\MountDir /Commit


> [!NOTE]  
> Tenga en cuenta que el paso 4 agrega el paquete de idioma; en este ejemplo instala EN-US.  

En este punto puede iniciar Nano Server con IIS.  

### <a name="installing-iis-on-nano-server-online"></a>Instalación de IIS en Nano Server en línea  
Aunque se recomienda la instalación sin conexión del rol de servidor, debe realizar la instalación en línea (con Nano Server activado) en escenarios de contenedor. Para ello, realice los pasos siguientes:  

1.  Copie la carpeta Packages desde el medio de instalación localmente al servidor Nano Server activado (por ejemplo, en C:\packages).  

2.  Cree un nuevo archivo Unattend.xml en otro equipo y luego cópielo en Nano Server. Puede copiar y pegar este contenido XML en el archivo XML creado:  



```  

    <unattend xmlns=urn:schemas-microsoft-com:unattend>  
    <servicing>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=neutral />  
            <source location=c:\packages\Microsoft-NanoServer-IIS-Package.cab />  
        </package>  
        <package action=install>  
            <assemblyIdentity name=Microsoft-NanoServer-IIS-Package version=10.0.14393.0 processorArchitecture=amd64 publicKeyToken=31bf3856ad364e35 language=en-US />  
            <source location=c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source= xmlns:cpi=urn:schemas-microsoft-com:cpi />  
</unattend>  
```  




3. En el archivo XML nuevo creado (o copiado), edite C:\packages en el directorio en que ha copiado el contenido de Packages.  

4. Cambie al directorio con el archivo XML recién creado y ejecute  

   **dism /online /apply-unattend:.\unattend.xml**  


5. Confirme que el paquete de IIS y su paquete de idioma asociado está instalado correctamente; para ello, ejecute:  

   **dism /online /get-packages**  

   Deberías ver que Package Identity: Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~~10.0.14393.1000 aparezca dos veces, una para Release Type: Language Pack y una para Release Type : Feature Pack.  

6. Inicie el servicio W3SVC con **net start w3svc** o reiniciando Nano Server.  

## <a name="starting-iis"></a>Inicio de IIS  
Una vez que IIS está instalado y en ejecución, está listo para atender las solicitudes web. Compruebe que IIS está en ejecución; para ello, examine la página web de IIS predeterminada en http://\<dirección IP de Nano Server>. En un equipo físico, puede determinar la dirección IP mediante la Consola de recuperación. En una máquina virtual, puede obtener la dirección IP mediante un símbolo del sistema de Windows PowerShell y ejecutando:  

`Get-VM -name <VM name> | Select -ExpandProperty networkadapters | select IPAddresses`  

Si no puede acceder a la página web IIS predeterminada, vuelva a comprobar la instalación de IIS; para ello, busque el directorio **c:\inetpub** en Nano Server.  

## <a name="enabling-and-disabling-iis-features"></a>Habilitar y deshabilitar características de IIS  
Una serie de características de IIS están habilitadas de forma predeterminada al instalar el rol de IIS (consulta la tabla de la sección Información general sobre IIS en Nano Server de este tema). Puede habilitar (o deshabilitar) características adicionales mediante DISM.exe.

Cada característica de IIS existe como un conjunto de elementos de configuración. Por ejemplo, la característica de autenticación de Windows consta de estos elementos:  

|Sección|Elementos de configuración|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name=WindowsAuthenticationModule image=%windir%\System32\inetsrv\authsspi.dll`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true \/>`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled=false authPersistNonNTLM\=true><providers><add value=Negotiate /><add value=NTLM /><br /></providers><br /></windowsAuthentication>`|  

El conjunto completo de subcaracterísticas de IIS se encuentra en el Apéndice 1 de este tema y sus elementos de configuración correspondientes se indican en el Apéndice 2 de este tema.  


### <a name="example-installing-windows-authentication"></a>Ejemplo: instalación de la autenticación de Windows  

1.  Abra una consola de la sesión remota de Windows PowerShell en Nano Server.  

2.  Use `DISM.exe` para instalar el módulo de autenticación de Windows:

    ```
    dism /Enable-Feature /online /featurename:IIS-WindowsAuthentication /all
    ```

    El conmutador `/all` instalará cualquier característica de la que dependa la característica seleccionada.

### <a name="example-uninstalling-windows-authentication"></a>Ejemplo: desinstalación de la autenticación de Windows  

1.  Abra una consola de la sesión remota de Windows PowerShell en Nano Server.  

2.  Use `DISM.exe` para desinstalar el módulo de autenticación de Windows:

    ```
    dism /Disable-Feature /online /featurename:IIS-WindowsAuthentication
    ```

## <a name="other-common-iis-configuration-tasks"></a>Otras tareas comunes de configuración de IIS  
**Creación de sitios web**  

Use este cmdlet:  

`PS D:\> New-IISSite -Name TestSite -BindingInformation *:80:TestSite -PhysicalPath c:\test`  

Después puede ejecutar `Get-IISSite` para comprobar el estado del sitio (devuelve el nombre del sitio web, el identificador, el estado, la ruta de acceso física y los enlaces).  

**Eliminación de sitios web**  

Ejecute `Remove-IISSite -Name TestSite -Confirm:$false`.  

**Creación de directorios virtuales**  

Puede crear directorios virtuales mediante el objeto IISServerManager devuelto por Get-IISServerManager, que expone la API de .NET Microsoft.Web.Administration.ServerManager. En este ejemplo, estos comandos acceden al elemento Sitio web predeterminado de la colección de sitios y al elemento de aplicación raíz (/) de la sección Aplicaciones. Luego llaman al método Add() de la colección VirtualDirectories para que ese elemento de la aplicación cree el nuevo directorio:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.Sites[Default Web Site].Applications[/].VirtualDirectories.Add(/DemoVirtualDir1, c:\test\virtualDirectory1)  
PS C:\> $sm.Sites[Default Web Site].Applications[/].VirtualDirectories.Add(/DemoVirtualDir2, c:\test\virtualDirectory2)  
PS C:\> $sm.CommitChanges()  
```  

**Creación de grupos de aplicaciones**  

De forma similar puede usar Get-IISServerManager para crear grupos de aplicaciones:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.ApplicationPools.Add(DemoAppPool)  
```  

**Configuración de HTTPS y certificados**  

Use la utilidad Certoc.exe para importar certificados, como en este ejemplo, que muestra la configuración de HTTPS para un sitio web en Nano Server:  

1.  En otro equipo que no se está ejecutando Nano Server, cree un certificado (con su propio nombre de certificado y una contraseña) y después expórtelo a c:\temp\test.pfx.  

    `$newCert = New-SelfSignedCertificate -DnsName www.foo.bar.com -CertStoreLocation cert:\LocalMachine\my`  

    `$mypwd = ConvertTo-SecureString -String YOUR_PFX_PASSWD -Force -AsPlainText`  

    `Export-PfxCertificate -FilePath c:\temp\test.pfx -Cert $newCert -Password $mypwd`  

2.  Copie el archivo test.pfx en el equipo de Nano Server.  

3.  En Nano Server, importa el certificado al almacén My con este comando:  

    **certoc.exe -ImportPFX -p YOUR_PFX_PASSWD My c:\temp\test.pfx**  

4.  Recuperar la huella digital de este certificado nuevo (en este ejemplo, 61E71251294B2A7BB8259C2AC5CF7BA622777E73) con `Get-ChildItem Cert:\LocalMachine\my`.  

5.  Agregue el enlace HTTPS al sitio web predeterminado (o cualquier sitio web al que desea agregar el enlace) mediante los siguientes comandos de Windows PowerShell:  

    ```  
    $certificate = get-item Cert:\LocalMachine\my\61E71251294B2A7BB8259C2AC5CF7BA622777E73  
    # Use your actual thumbprint instead of this example  
    $hash = $certificate.GetCertHash()  

    Import-Module IISAdministration  
    $sm = Get-IISServerManager  
    $sm.Sites[Default Web Site].Bindings.Add(*:443:, $hash, My, 0)    # My is the certificate store name  
    $sm.CommitChanges()  
    ```  

    También puedes usar la Indicación de nombre de servidor (SNI) mediante un nombre de host específico con esta sintaxis: `$sm.Sites[Default Web Site].Bindings.Add(*:443:www.foo.bar.com, $hash, My, Sni.`  

## <a name="appendix-1-list-of-iis-sub-features"></a>Apéndice 1: Lista de subcaracterísticas de IIS

- IIS-WebServer
- IIS-CommonHttpFeatures
- IIS-StaticContent
- IIS-DefaultDocument
- IIS-DirectoryBrowsing
- IIS-HttpErrors
- IIS-HttpRedirect
- IIS-ApplicationDevelopment
- IIS-CGI
- IIS-ISAPIExtensions
- IIS-ISAPIFilter
- IIS-ServerSideIncludes
- IIS-WebSockets
- IIS-ApplicationInit
- IIS-Security
- IIS-BasicAuthentication
- IIS-WindowsAuthentication
- IIS-WindowsAuthentication
- IIS-ClientCertificateMappingAuthentication
- IIS-IISCertificateMappingAuthentication
- IIS-URLAuthorization
- IIS-RequestFiltering
- IIS-IPSecurity
- IIS-CertProvider
- IIS-Performance
- IIS-HttpCompressionStatic
- IIS-HttpCompressionDynamic
- IIS-HealthAndDiagnostics
- IIS-HttpLogging
- IIS-LoggingLibraries
- IIS-RequestMonitor
- IIS-HttpTracing
- IIS-CustomLogging

## <a name="appendix-2-elements-of-http-features"></a>Apéndice 2: Elementos de características HTTP  
Cada característica de IIS existe como un conjunto de elementos de configuración. En este apéndice se enumeran los elementos de configuración de todas las características de esta versión de Nano Server  

### <a name="common-http-features"></a>Características HTTP comunes  
**Documento predeterminado**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=DefaultDocumentModule image=%windir%\System32\inetsrv\defdoc.dll />`|  
|`<modules>`|`<add name=DefaultDocumentModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=DefaultDocumentModule resourceType=EiSecther requireAccess=Read />`|  
|`<defaultDocument>`|`<defaultDocument enabled=true><br /><files><br /> <add value=Default.htm /><br />        <add value=Default.asp /><br />        <add value=index.htm /><br />        <add value=index.html /><br />        <add value=iisstart.htm /><br />    </files><br /></defaultDocument>`|  

La entrada `StaticFile <handlers>` podría estar ya presente; si es así, agrega DefaultDocumentModule al atributo \<modules>, separados por coma.  

**Examen de directorios**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=DirectoryListingModule image=%windir%\System32\inetsrv\dirlist.dll />`|  
|`<modules>`|`<add name=DirectoryListingModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=DirectoryListingModule resourceType=Either requireAccess=Read />`|  

La entrada `StaticFile <handlers>` podría estar ya presente; si es así, agrega DirectoryListingModule al atributo \<modules>, separados por coma.  

**Errores HTTP**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=CustomErrorModule image=%windir%\System32\inetsrv\custerr.dll />`|  
|`<modules>`|`<add name=CustomErrorModule lockItem=true />`|  
|`<httpErrors>`|`<httpErrors lockAttributes=allowAbsolutePathsWhenDelegated,defaultPath><br />    <error statusCode=401    prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=401.htm ><br />    <error statusCode=403 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=403.htm /><br />    <error statusCode=404 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=404.htm /><br />    <error statusCode=405 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=405.htm /><br />    <error statusCode=406 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=406.htm /><br />    <error statusCode=412 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=412.htm /><br />    <error statusCode=500 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=500.htm /><br />    <error statusCode=501 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=501.htm /><br />    <error statusCode=502 prefixLanguageFilePath=%SystemDrive%\inetpub\custerr path=502.htm /><br /></httpErrors>`|  

**Contenido estático**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=StaticFileModule image=%windir%\System32\inetsrv\static.dll />`|  
|`<modules>`|`<add name=StaticFileModule lockItem=true />`|  
|`<handlers>`|`<add name=StaticFile path=* verb=* modules=StaticFileModule resourceType=Either requireAccess=Read />`|  

La entrada `StaticFile \<handlers>` podría estar ya presente; si es así, agrega StaticFileModule al atributo \<modules>, separados por coma.  

**Redirección HTTP**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=HttpRedirectionModule image=%windir%\System32\inetsrv\redirect.dll />`|  
|`<modules>`|`<add name=HttpRedirectionModule lockItem=true />`|  
|`<httpRedirect>`|`<httpRedirect enabled=false />`|  

### <a name="health-and-diagnostics"></a>Estado y diagnóstico  
**Registro HTTP**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=HttpLoggingModule image=%windir%\System32\inetsrv\loghttp.dll />`|  
|`<modules>`|`<add name=HttpLoggingModule lockItem=true />`|  
|`<httpLogging>`|`<httpLogging dontLog=false />`|  

**Registro personalizado**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CustomLoggingModule image=%windir%\System32\inetsrv\logcust.dll />`|  
|`<modules>`|`<add name=CustomLoggingModule lockItem=true />`|  

**Monitor de solicitudes**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=RequestMonitorModule image=%windir%\System32\inetsrv\iisreqs.dll />`|  

**Seguimiento**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=TracingModule image=%windir%\System32\inetsrv\iisetw.dll \/><br /><add name=FailedRequestsTracingModule image=%windir%\System32\inetsrv\iisfreb.dll />`|  
|`<modules>`|`<add name=FailedRequestsTracingModule lockItem=true />`|  
|`<traceProviderDefinitions>`|`<traceProviderDefinitions><br />    <add name=WWW Server guid\={3a2a4e84-4c21-4981-ae10-3fda0d9b0f83}><br />        <areas><br />            <clear /><br />            <add name=Authentication value=2 /><br />            <add name=Security value=4 /><br />            <add name=Filter value=8 /><br />            <add name=StaticFile value=16 /><br />            <add name=CGI value=32 /><br />            <add name=Compression value=64 /><br />            <add name=Cache value=128 /><br />            <add name=RequestNotifications value=256 /><br />            <add name=Module value=512 /><br />            <add name=FastCGI value=4096 /><br />            <add name=WebSocket value=16384 /><br />        </areas><br />    </add><br />    <add name=ISAPI Extension guid={a1c2040e-8840-4c31-ba11-9871031a19ea}><br />        <areas><br />            <clear /><br />        </areas><br />    </add><br /></traceProviderDefinitions>`|  

### <a name="performance"></a>Rendimiento  
**Compresión de contenido estático**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=StaticCompressionModule image=%windir%\System32\inetsrv\compstat.dll />`|  
|`<modules>`|`<add name=StaticCompressionModule lockItem=true />`|  
|`<httpCompression>`|`<httpCompression directory=%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files><br />    <scheme name=gzip dll=%Windir%\system32\inetsrv\gzip.dll /><br />   <staticTypes><br />        <add mimeType=text/* enabled=true /><br />        <add mimeType=message/* enabled=true /><br />        <add mimeType=application/javascript enabled=true \/><br />        <add mimeType=application/atom+xml enabled=true /><br />        <add mimeType=application/xaml+xml enabled=true /><br />        <add mimeType=\*\* enabled=false /><br />    </staticTypes><br /></httpCompression>`|  

**Compresión de contenido dinámico**  

|Sección|Elementos de configuración|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name=DynamicCompressionModule image=%windir%\System32\inetsrv\compdyn.dll />`|  
|`<modules>`|`<add name=DynamicCompressionModule lockItem=true />`|  
|`<httpCompression>`|`<httpCompression directory\=%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files><br />    <scheme name=gzip dll=%Windir%\system32\inetsrv\gzip.dll \/><br />    \<dynamicTypes><br />        <add mimeType=text/* enabled=true \/><br />        <add mimeType=message/* enabled=true /><br />        <add mimeType=application/x-javascript enabled=true /><br />        <add mimeType=application/javascript enabled=true /><br />        <add mimeType=*/* enabled=false /><br />    <\/dynamicTypes><br /></httpCompression>`|  

### <a name="security"></a>Seguridad  
**Filtrado de solicitudes**  


|       Sección        |                                                                                                                                        Elementos de configuración                                                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `<globalModules>`   |                                                                                                        `<add name=RequestFilteringModule image=%windir%\System32\inetsrv\modrqflt.dll />`                                                                                                        |
|     `<modules>`      |                                                                                                                       `<add name=RequestFilteringModule lockItem=true />`                                                                                                                        |
| \`<requestFiltering> | `<requestFiltering><br />    <fileExtensions allowUnlisted=true applyToWebDAV=true /><br />    <verbs allowUnlisted=true applyToWebDAV=true /><br />    <hiddenSegments applyToWebDAV=true><br />        <add segment=web.config /><br />    </hiddenSegments><br /></requestFiltering>` |

**Autenticación básica**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name=BasicAuthenticationModule image=%windir%\System32\inetsrv\authbas.dll />`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true />`|  
|`<basicAuthentication>`|`<basicAuthentication enabled=false />`|  

**Autenticación de asignaciones de certificado de cliente**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CertificateMappingAuthentication image=%windir%\System32\inetsrv\authcert.dll />`|  
|`<modules>`|`<add name=CertificateMappingAuthenticationModule lockItem=true />`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled=false />`|  

**Autenticación implícita**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=DigestAuthenticationModule image=%windir%\System32\inetsrv\authmd5.dll />`|  
|`<modules>`|`<add name=DigestAuthenticationModule lockItem=true />`|  
|`<other>`|`<digestAuthentication enabled=false />`|  

**Autenticación de asignaciones de certificado de cliente de IIS**  


|                  Sección                   |                                         Elementos de configuración                                         |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------|
|             `<globalModules>`              | `<add name=CertificateMappingAuthenticationModule image=%windir%\System32\inetsrv\authcert.dll />` |
|                `<modules>`                 |               `<add name=CertificateMappingAuthenticationModule lockItem=true `/>\`                |
| `<clientCertificateMappingAuthentication>` |                      `<clientCertificateMappingAuthentication enabled=false />`                      |

**Restricciones de dominio y dirección IP**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|```<add name=IpRestrictionModule image=%windir%\System32\inetsrv\iprestr.dll /><br /><add name=DynamicIpRestrictionModule image=%windir%\System32\inetsrv\diprestr.dll />```|  
|`<modules>`|`<add name=IpRestrictionModule lockItem=true \/><br /><add name=DynamicIpRestrictionModule lockItem=true \/>`|  
|`<ipSecurity>`|`<ipSecurity allowUnlisted=true />`|  

**Autorización para URL**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=UrlAuthorizationModule image=%windir%\System32\inetsrv\urlauthz.dll />`|  
|`<modules>`|`<add name=UrlAuthorizationModule lockItem=true />`|  
|`<authorization>`|`<authorization><br />    <add accessType=Allow users=* /><br /></authorization>`|  

**Autenticación de Windows**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=WindowsAuthenticationModule image=%windir%\System32\inetsrv\authsspi.dll />`|  
|`<modules>`|`<add name=WindowsAuthenticationModule lockItem=true />`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled=false authPersistNonNTLM\=true><br />    <providers><br />        <add value=Negotiate /><br />        <add value=NTLM /><br />    <\providers><br /><\windowsAuthentication><windowsAuthentication enabled=false authPersistNonNTLM\=true><br />    <providers><br />        <add value=Negotiate /><br />        <add value=NTLM /><br />    <\/providers><br /><\/windowsAuthentication>`|  

### <a name="application-development"></a>Desarrollo de aplicaciones  
**Inicialización de aplicaciones**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=ApplicationInitializationModule image=%windir%\System32\inetsrv\warmup.dll />`|  
|`<modules>`|`<add name=ApplicationInitializationModule lockItem=true />`|  

**CGI**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name=CgiModule image=%windir%\System32\inetsrv\cgi.dll /><br /><add name=FastCgiModule image=%windir%\System32\inetsrv\iisfcgi.dll />`|  
|`<modules>`|`<add name=CgiModule lockItem=true /><br /><add name=FastCgiModule lockItem=true />`|  
|`<handlers>`|`<add name=CGI-exe path=*.exe verb=\* modules=CgiModule resourceType=File requireAccess=Execute allowPathInfo=true />`|  

**Extensiones ISAPI**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=IsapiModule image=%windir%\System32\inetsrv\isapi.dll />`|  
|`<modules>`|`<add name=IsapiModule lockItem=true />`|  
|`<handlers>`|`<add name=ISAPI-dll path=*.dll verb=* modules=IsapiModule resourceType=File requireAccess=Execute allowPathInfo=true />`|  

**Filtros ISAPI**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=IsapiFilterModule image=%windir%\System32\inetsrv\filter.dll />`|  
|`<modules>`|`<add name=IsapiFilterModule lockItem=true />`|  

**Inclusiones del lado servidor**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|  
|`<globalModules>`|<`add name=ServerSideIncludeModule image=%windir%\System32\inetsrv\iis_ssi.dll />`|  
|`<modules>`|`<add name=ServerSideIncludeModule lockItem=true />`|  
|`<handlers>`|`<add name=SSINC-stm path=*.stm verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File \/><br /><add name=SSINC-shtm path=*.shtm verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File /><br /><add name=SSINC-shtml path=*.shtml verb=GET,HEAD,POST modules=ServerSideIncludeModule resourceType=File />`|  
|`<serverSideInclude>`|`<serverSideInclude ssiExecDisable=false />`|  

**Protocolo WebSocket**  

|Sección|Elementos de configuración|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name=WebSocketModule image=%windir%\System32\inetsrv\iiswsock.dll />`|  
|`<modules>`|`<add name=WebSocketModule lockItem=true />`|  