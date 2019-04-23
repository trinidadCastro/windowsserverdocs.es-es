---
title: Preparación para migrar una granja de AD FS SQL
description: Proporciona información sobre cómo prepararse para migrar una granja SQL server de AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 284e02174b4a8c06f114640223d289dc63ea3a26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890396"
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>Preparar la migración de una granja de servidores de SQL Server  
 Para preparar la migración de servidores de federación 2.0 de AD FS que pertenecen a una granja de servidores de SQL Server a Windows Server 2012, debe exportar y realizar una copia de seguridad de los datos de configuración de AD FS desde estos servidores.  
  
 Para exportar los datos de configuración de AD FS, realiza estas tareas:  
  
-   [Paso 1: Exportar la configuración de servicio](#step-1-export-service-settings)  
  
-   [Paso 2: Realizar una copia de seguridad de los almacenes de atributos personalizados](#step-2-back-up-custom-attribute-stores)  
  
-   [Paso 3: Realizar copias de seguridad de personalizaciones de páginas Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Paso 1: Exportar la configuración del servicio  
 Para exportar la configuración del servicio, realiza el siguiente procedimiento:  
  
### <a name="to-export-service-settings"></a>Exportar la configuración de servicios  
  
1.  Anota el nombre del sujeto del certificado y el valor de huella digital del certificado SSL usado por el servicio de federación. To find the SSL certificate, open the Internet Information Services (IIS) management console, select **Sitio web predeterminado** en el panel izquierdo, haga clic en **Enlaces…** en el panel **Acción** , busque y seleccione el enlace HTTPS, haga clic en **Editar**y, a continuación, haga clic en **Ver**.  
  
> [!NOTE]
>  También puedes exportar el certificado SSL y su clave privada a un archivo .pfx. Para obtener más información, consulta [Exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Este paso es opcional porque este certificado se almacena en el almacén de certificados personales del equipo local y se conservará durante la actualización del sistema operativo.  
  
2.  Exporta otras claves y certificados de comunicaciones de servicio, cifrado de tokens o firma de tokens que AD FS no haya generado internamente.  
  
Para ver todos los certificados que AD FS está usando en el servidor, usa Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para ver todos los certificados que están en uso en el servidor `PSH:>Get-ADFSCertificate`. El resultado de este comando incluye los valores de StoreLocation y StoreName que especifica la ubicación del almacén de cada certificado.  
  
> [!NOTE]
>  También puedes seguir las instrucciones de [Exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado y su clave privada a un archivo .pfx. Este paso es opcional porque todos los certificados externos se conservan durante la actualización del sistema operativo.  
  
3.  Haga una copia de seguridad del archivo de configuración de la aplicación. Entre otras opciones de configuración, este archivo contiene la cadena de conexión a la base de datos de directivas.  
  
Para hacer una copia de seguridad del archivo de configuración de la aplicación, debe copiar manualmente el archivo `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` a una ubicación segura en un servidor de copia de seguridad.  
  
> [!NOTE]
>  Registre la cadena de conexión de SQL Server después de “policystore connectionstring=” en el siguiente archivo:  `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`. Necesitarás esta cadena cuando restaures la configuración de AD FS original en el servidor de federación.  
  
4.  Anota la identidad de la cuenta del Servicio de federación de AD FS 2.0 y su contraseña.  
  
Para buscar el valor de identidad, examina la columna **Iniciar sesión como** de **Servicio de Windows AD FS 2.0** en la consola **Servicios** y registra manualmente este valor.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Paso 2: Hacer una copia de seguridad de los almacenes de atributos personalizados  
 Para obtener información acerca de los almacenes de atributos personalizados que utiliza AD FS, usa Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Luego, ejecute el siguiente comando para encontrar información acerca de los almacenes de atributos personalizados: `PSH:>Get-ADFSAttributeStore`. Los pasos para actualizar o migrar almacenes de atributos personalizados varían.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Paso 3: Hacer una copia de seguridad de las personalizaciones de las páginas web  
 Para hacer una copia de seguridad de las personalizaciones de páginas web, copie las páginas web de AD FS y el archivo **web.config** del directorio que está asignado a la ruta de acceso virtual **“/adfs/ls”** en IIS. De forma predeterminada, está en el directorio **%systemdrive%\inetpub\adfs\ls**.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparar la migración del servidor de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar la migración del servidor Proxy de AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar al servidor Proxy de AD FS 2.0 Federation](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS 1.1 de Web](migrate-the-ad-fs-web-agent.md)