---
title: "Preparación para migrar un servidor de federación de AD FS independiente"
description: "Proporciona información sobre la preparación migrar un servidor de AD FS independiente para Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Preparación para migrar un servidor de federación de AD FS independiente o una granja de servidores de AD FS de nodo único  
 
Para preparar la migración (mismo migración de servidor) un servidor de federación 2.0 de AD FS independiente o una granja de servidores de AD FS nodo único en Windows Server 2012, debes exportar y copia los datos de configuración de AD FS de este servidor.  
  
Para exportar los datos de configuración de AD FS, realizar las siguientes tareas:  
  
-   [Paso 1: Exportar la configuración del servicio](#step-1-export-service-settings)  
  
-   [Paso 2: Exportar dice confianzas de proveedor](#step-2-export-claims-provider-trusts)  
  
-   [Paso 3: Exportar confianzas de terceros de confianza](#step-3-export-relying-party-trusts)  
  
-   [Paso 4: Crear una copia de seguridad de atributo personalizado almacena](#step-4-back-up-custom-attribute-stores)  
  
-   [Paso 5: Realizar una copia de las personalizaciones de la página Web](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Paso 1: Exportar la configuración del servicio  
 Para exportar la configuración del servicio, realiza lo siguiente:  
  
### <a name="to-export-service-settings"></a>Para exportar la configuración del servicio  
  
1.  El valor de nombre y la huella digital de asunto de certificado del certificado SSL usado por el servicio de federación de registro. Para encontrar el certificado SSL, abre la administración de Internet Information Services (IIS) de la consola, selecciona **sitio Web predeterminado** en el panel izquierdo, haz clic en **enlaces...** En la **acción** panel, busca y selecciona el enlace de https, haz clic en **editar**y, a continuación, haz clic en **vista**.  
  
> [!NOTE]
>  Opcionalmente, también puedes exportar el certificado SSL utilizado por el servicio de federación y su clave privada en un archivo pfx. Para obtener más información, consulta [exportar la parte de la clave privada de un certificado de autenticación de servidor ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Exportar el certificado SSL es opcional porque este certificado se almacena en el almacén de certificados personales del equipo local y se conserva en la actualización del sistema operativo.  
  
2.  Registra la configuración de las comunicaciones de servicio de AD FS, los certificados de descifrado de token y la firma de tokens.  Para ver todos los certificados que se usan, abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para crear una lista de todos los certificados en uso en un archivo `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Opcionalmente, también puedes exportar los certificados de firma de token, el cifrado de token o comunicaciones de servicio y las claves que no se internamente generan, además de todos los certificados autofirmados. Puedes ver todos los certificados que se usan en el servidor mediante Windows PowerShell. Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell`. A continuación, ejecuta el siguiente comando para ver todos los certificados que se usan en el servidor `PSH:>Get-ADFSCertificate`. El resultado de este comando incluye valores StoreLocation y StoreName que especifican la ubicación del almacén de cada certificado. Puedes usar la Guía de [exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado y su clave privada a un archivo .pfx.  
>   
>  Exportación de estos certificados es opcional porque todos los certificados externos se conservan durante la actualización del sistema operativo.  
  
3.  Propiedades de servicio de federación de exportación AD FS 2.0, como el nombre de servicio de federación, servicios de federación de nombre para mostrar y el identificador del servidor de federación en un archivo.  
  
Para exportar las propiedades del servicio de federación, abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para exportar las propiedades del servicio de federación:`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
El archivo de salida contendrá los siguientes valores de configuración importantes:  
  
    
|**Nombre de la propiedad de servicios de federación según lo informado por Get-ADFSProperties**|**Nombre de la propiedad de servicios de federación en la consola de administración de AD FS**|
|------|------|
|Nombre de host|Nombre de servicio de federación|  
|Identificador|Identificador de servicios de federación|  
|DisplayName|Nombre de servicio federación|  
  
4.  Copia el archivo de configuración de la aplicación. Entre otras opciones, este archivo contiene la cadena de conexión de la base de datos de directiva.  
  
Para hacer una copia el archivo de configuración de la aplicación, debes copiar manualmente los `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`archivo a una ubicación segura en un servidor de copia de seguridad.  
  
> [!NOTE]
>  Toma nota de la cadena de conexión de la base de datos en este archivo, ubicados inmediatamente después de "policystore connectionstring ="). Si la cadena de conexión especifica una base de datos de SQL Server, el valor es necesario cuando se restaura la configuración de AD FS original en el servidor de federación.  
>   
>  El siguiente es un ejemplo de una cadena de conexión WID:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. El siguiente es un ejemplo de una cadena de conexión de SQL Server:`"Data Source=databasehostname;Integrated Security=True"`.  
  
5.  Registra la identidad de la cuenta de servicio de AD FS federación 2.0 y la contraseña de esta cuenta.  
  
Para encontrar el valor de identidad, examinar la **iniciar sesión como** columna de **AD FS 2.0 Windows servicio** en la **servicios** consola y grabar manualmente este valor.  
  
> [!NOTE]
>  Para un servicio de federación independiente, se usa la cuenta predefinida de servicio de red.  En este caso, no es necesario tener una contraseña.  
  
6.  Exportar la lista de los extremos de AD FS habilitados a un archivo.  
  
Para ello, abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para exportar la lista de los extremos de AD FS habilitados a un archivo:`PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7.  Exportar las descripciones de notificación personalizada en un archivo.  
  
Para ello, abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para exportar las descripciones de notificación personalizada en un archivo:`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Paso 2: Exportar dice confianzas de proveedor  
 Para exportar las confianzas de proveedor de reclamaciones, realiza lo siguiente:  
  
### <a name="to-export-claims-provider-trusts"></a>Para exportar confianzas de proveedor de notificaciones  
  
1.  Puedes usar Windows PowerShell para exportar todas las confianzas de proveedor de notificaciones. Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para exportar todas las reclamaciones proveedor confianzas:`PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Paso 3: Exportar confianzas de terceros de confianza  
 Para exportar confianzas de terceros de confianza, realiza lo siguiente:  
  
### <a name="to-export-relying-party-trusts"></a>Para exportar confianzas de terceros de confianza  
  
1.  Para exportar todos los usuarios de confianza confianzas de terceros, abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para exportar todos los usuarios de confianza confianzas de terceros:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Paso 4: Crear una copia de seguridad de atributo personalizado almacena  
 Encontrarás información acerca de los almacenes del atributo personalizado en uso por AD FS mediante Windows PowerShell. Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para buscar información sobre el atributo personalizado las tiendas:`PSH:>Get-ADFSAttributeStore`. Los pasos para actualizar o migrar el atributo personalizado almacenes varían.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Paso 5: Realizar una copia de las personalizaciones de la página Web  
 Para hacer una copia de las personalizaciones de la página Web, copiar las páginas Web de AD FS y **web.config** archivo desde el directorio que se asigna a la ruta de acceso virtual **"/ adfs/ls"** en IIS. De manera predeterminada, está en la **%systemdrive%\inetpub\adfs\ls** directorio.  

## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)