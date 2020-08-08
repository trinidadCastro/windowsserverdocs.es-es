---
title: Preparar la migración del servidor proxy de federación de AD FS 2.0
description: Proporciona información sobre cómo prepararse para migrar el proxy de AD FS Server a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: e0a040dd3ec717e1315c842d6e6b407025ceabfd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940666"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Preparar la migración del servidor proxy de federación de AD FS 2.0

Para preparar la migración de un servidor proxy de Federación AD FS 2,0 a Windows Server 2012, debe exportar y realizar una copia de seguridad de los datos de configuración de AD FS desde este proxy del servidor.  Los pasos que se describen en este tema se aplican a un escenario con uno o varios servidores de federación proxy.

 Para exportar los datos de configuración de AD FS, realiza estas tareas:

-   [Paso 1: Exportar la configuración del servicio de proxy](#step-1-export-proxy-service-settings)

-   [Paso 2: Hacer copia de seguridad de las personalizaciones de las páginas web](#step-2-back-up-webpage-customizations)

##  <a name="step-1-export-proxy-service-settings"></a>Paso 1: Exportar la configuración del servicio de proxy
 Para exportar la configuración del servicio de proxy de servidores de federación, realice el siguiente procedimiento:

### <a name="to-export-proxy-service-settings"></a>Para exportar la configuración del servicio de proxy

1.  Exporte el certificado de Capa de sockets seguros (SSL) y su clave privada a un archivo .pfx. Para obtener más información, consulte [Export the Private Key Portion of a Server Authentication Certificate](export-the-private-key-portion-of-a-server-authentication-certificate.md).

> [!NOTE]
>  Este paso es opcional porque este certificado se conserva durante la actualización del sistema operativo.

2. Exporte las propiedades del proxy de federación de AD FS 2.0 en un archivo. Puede hacerlo mediante Windows PowerShell.

Abra Windows PowerShell y ejecute el comando siguiente para agregar los cmdlets de AD FS a la sesión de Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Después, ejecute el comando siguiente para exportar las propiedades del proxy de federación a un archivo: `PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.

3. Asegúrese de que conoce las credenciales de una cuenta que sea un administrador del servidor de federación de AD FS o la cuenta de servicio en la que se ejecuta el servicio de federación de AD FS.  Esta información es necesaria para la configuración de la confianza del proxy.

   Al completar este paso, se recopila la siguiente información que es necesaria para configurar el proxy del servidor de federación de AD FS:

-   Nombre del servicio de federación de AD FS

-   Nombre de la cuenta de dominio que se requiere para la configuración de la confianza del proxy

-   La dirección y el puerto del proxy HTTP (si hay un servidor proxy HTTP entre el proxy del servidor de federación de AD FS y los servidores de federación de AD FS)

##  <a name="step-2-back-up-webpage-customizations"></a>Paso 2: Hacer copia de seguridad de las personalizaciones de las páginas web
 Para hacer una copia de seguridad de las personalizaciones de las páginas web, copie las páginas web del proxy de AD FS y el archivo **web.config** del directorio que está asignado a la ruta de acceso virtual **“/adfs/ls”** en IIS.  De forma predeterminada, está en el directorio **%systemdrive%\inetpub\adfs\ls**.

## <a name="next-steps"></a>Pasos a seguir
 [Preparar la migración del servidor de Federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparación para migrar el servidor proxy de federación de AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar el servidor de Federación de AD FS 2,0](migrate-the-ad-fs-fed-server.md) [migrar el servidor proxy de Federación de AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) [migrar los agentes Web de AD FS 1,1](migrate-the-ad-fs-web-agent.md)