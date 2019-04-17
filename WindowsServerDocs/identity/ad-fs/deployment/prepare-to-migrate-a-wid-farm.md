---
title: "Preparación para migrar un conjunto de AD FS 2.0 WID"
description: "Proporciona información sobre la preparación migrar una granja de servidores de AD FS 2.0 servidor WID a Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4985a8d16614bd12bce991e196d105464d37634d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-an-ad-fs-20-wid-farm"></a>Preparación para migrar un conjunto de AD FS 2.0 WID  
 Para preparar la migración de los servidores de AD FS federación 2.0 que pertenecen a un conjunto de Windows Internal Database (WID) para Windows Server 2012, debes exportar y copia los datos de configuración de AD FS de estos servidores.  
  
 Para exportar los datos de configuración de AD FS, realizar las siguientes tareas:  
  
-   [Paso 1:: exportar la configuración del servicio](#step-1-export-service-settings)  
  
-   [Paso 2: Crear una copia de seguridad de atributo personalizado almacena](#step-2-back-up-custom-attribute-stores)  
  
-   [Paso 3: Realizar una copia de las personalizaciones de la página Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Paso 1: Exportar la configuración del servicio  
 Para exportar la configuración del servicio, realiza lo siguiente:  
  
### <a name="to-export-service-settings"></a>Para exportar la configuración del servicio  
  
1.  El valor de nombre y la huella digital de asunto de certificado del certificado SSL usado por el servicio de federación de registro. Para buscar el certificado SSL, abre la consola de administración de Internet Information Services (IIS), selecciona **sitio Web predeterminado** en el panel izquierdo, haz clic en **enlaces...** En la **acción** panel, busca y selecciona el enlace de https, haz clic en **editar**, a continuación, haz clic en **vista **.  
  
> [!NOTE]
>  Opcionalmente, también puedes exportar el certificado SSL y su clave privada en un archivo pfx. Para obtener más información, consulta [exportar la parte de la clave privada de un certificado de autenticación de servidor ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Este paso es opcional porque este certificado se almacena en el almacén de certificados personales del equipo local y se conservará en la actualización del sistema operativo.  
  
2.  Exportar los certificados de firma de token, el cifrado de token o comunicaciones de servicio y las claves que no se internamente generan, además de certificados autofirmados.  
  
Puedes ver todos los certificados que se usan en el servidor mediante Windows PowerShell. Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para ver todos los certificados que se usan en el servidor `PSH:>Get-ADFSCertificate`. El resultado de este comando incluye valores StoreLocation y StoreName que especifican la ubicación del almacén de cada certificado.  Puedes usar la Guía de [exportar la parte de la clave privada de un certificado de autenticación de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado y su clave privada a un archivo .pfx.  
  
> [!NOTE]
>  Este paso es opcional, porque todos los certificados externos se conservan durante la actualización del sistema operativo.  
  
3.  Registra la identidad de la cuenta de servicio de AD FS federación 2.0 y la contraseña de esta cuenta.  
  
Para encontrar el valor de identidad, examinar la **iniciar sesión como** columna de **AD FS 2.0 Windows servicio** en la **servicios** consola y grabar manualmente el valor.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Paso 2: Crear una copia de seguridad de atributo personalizado almacena  
 Encontrarás información acerca de los almacenes del atributo personalizado en uso por AD FS mediante Windows PowerShell. Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para buscar información sobre el atributo personalizado las tiendas:`PSH:>Get-ADFSAttributeStore`. Los pasos para actualizar o migrar el atributo personalizado almacenes varían.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Paso 3: Realizar una copia de las personalizaciones de la página Web  
 Para hacer una copia de las personalizaciones de la página Web, copiar las páginas Web de AD FS y **web.config** archivo desde el directorio que se asigna a la ruta de acceso virtual **"/ adfs/ls"** en IIS. De manera predeterminada, está en la **%systemdrive%\inetpub\adfs\ls** directorio.  

## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)