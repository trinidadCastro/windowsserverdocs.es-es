---
title: Preparar la migración de una granja AD FS 2,0 WID
description: Proporciona información sobre cómo preparar la migración de una granja de servidores WID AD FS 2,0 Server a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: 2393e289e6a20d30db2d5523810437b6c5049fe5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940604"
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Preparar la migración de una granja AD FS 2,0 WID
 Para preparar la migración de servidores de Federación de AD FS 2,0 que pertenecen a una granja de Windows Internal Database (WID) a Windows Server 2012, debe exportar y realizar una copia de seguridad de los datos de configuración de AD FS desde estos servidores.

 Para exportar los datos de configuración de AD FS, realiza estas tareas:

-   [Paso 1: Exportar la configuración del servicio](#step-1-export-service-settings)

-   [Paso 2: Hacer una copia de seguridad de los almacenes de atributos personalizados](#step-2-back-up-custom-attribute-stores)

-   [Paso 3: Hacer una copia de seguridad de las personalizaciones de las páginas web](#step-3-back-up-webpage-customizations)

## <a name="step-1-export-service-settings"></a>Paso 1: Exportar la configuración del servicio
 Para exportar la configuración del servicio, realiza el siguiente procedimiento:

### <a name="to-export-service-settings"></a>Exportar la configuración de servicios

1.  Anota el nombre del sujeto del certificado y el valor de huella digital del certificado SSL usado por el servicio de federación. Para buscar el certificado SSL, abra la consola de administración de Internet Information Services (IIS), seleccione **sitio web predeterminado** en el panel izquierdo y haga clic en **enlaces...** en el panel **Acción** , busque y seleccione el enlace HTTPS, haga clic en **Editar**y, después, haz clic en **Ver**.

> [!NOTE]
>  También puedes exportar el certificado SSL y su clave privada a un archivo .pfx. Para obtener más información, consulte [Export the Private Key Portion of a Server Authentication Certificate](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).
>
>  Este paso es opcional porque este certificado se almacena en el almacén de certificados personales del equipo local y se conservará durante la actualización del sistema operativo.

2. Exporta las claves y los certificados de comunicaciones de servicio, cifrado de tokens o firma de tokens que no se hayan generado internamente, además de los certificados autofirmados.

Para ver todos los certificados que están en uso en el servidor, usa Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el siguiente comando para ver todos los certificados que están en uso en el servidor `PSH:>Get-ADFSCertificate`. El resultado de este comando incluye los valores de StoreLocation y StoreName que especifica la ubicación del almacén de cada certificado.  Después, usa las instrucciones de [Exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado y su clave privada a un archivo .pfx.

> [!NOTE]
>  Este paso es opcional porque todos los certificados externos se conservan durante la actualización del sistema operativo.

3. Anota la identidad de la cuenta del Servicio de federación de AD FS 2.0 y su contraseña.

Para buscar el valor de identidad, examina la columna **Iniciar sesión como** de **Servicio de Windows AD FS 2.0** en la consola **Servicios** y registra manualmente este valor.

## <a name="step-2-back-up-custom-attribute-stores"></a>Paso 2: Hacer una copia de seguridad de los almacenes de atributos personalizados
 Para obtener información acerca de los almacenes de atributos personalizados que utiliza AD FS, usa Windows PowerShell. Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Luego, ejecute el siguiente comando para encontrar información acerca de los almacenes de atributos personalizados: `PSH:>Get-ADFSAttributeStore`. Los pasos para actualizar o migrar almacenes de atributos personalizados varían.

## <a name="step-3-back-up-webpage-customizations"></a>Paso 3: Hacer una copia de seguridad de las personalizaciones de las páginas web
 Para hacer una copia de seguridad de las personalizaciones de las páginas web, copia las páginas web de AD FS y el archivo **web.config** del directorio que está asignado a la ruta virtual **“/adfs/ls”** en IIS. De forma predeterminada, está en el directorio **%systemdrive%\inetpub\adfs\ls**.

## <a name="next-steps"></a>Pasos a seguir
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparación para migrar el servidor proxy de federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar el servidor de Federación de AD FS 2,0](migrate-the-ad-fs-fed-server.md) [migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) [migrar los agentes Web de AD FS 1,1](migrate-the-ad-fs-web-agent.md)