---
title: "Preparación para migrar al Proxy de servidor de AD FS federación 2.0"
description: "Proporciona información sobre la preparación migrar al proxy de servidor de AD FS a Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f207993580e6fd06c9ff185e58e5b7e81af60252
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Preparación para migrar al Proxy de servidor de AD FS federación 2.0

Para preparar la migración de un proxy de servidor de AD FS federación 2.0 para Windows Server 2012, debes exportar y hacer copias de seguridad de los datos de configuración de AD FS desde este proxy server.  Los pasos descritos en este tema se aplican a un escenario con un servidor proxy de federación o varios servidores proxy de federación.  
  
 Para exportar los datos de configuración de AD FS, realizar las siguientes tareas:  
  
-   [Paso 1: Exportar la configuración del servicio de proxy](#step-1-export-proxy-service-settings)  
  
-   [Paso 2: Realizar una copia de las personalizaciones de la página Web](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>Paso 1: Exportar la configuración del servicio de proxy  
 Para exportar la configuración de servicios de federación servidor proxy, realiza lo siguiente:  
  
### <a name="to-export-proxy-service-settings"></a>Para exportar la configuración del servicio de proxy  
  
1.  Exportar el certificado de capa de Sockets seguros (SSL) y su clave privada en un archivo pfx. Para obtener más información, consulta [exportar la parte de la clave privada de un certificado de autenticación de servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Este paso es opcional porque este certificado se conserva durante la actualización del sistema operativo.  
  
2.  Exportar propiedades del servidor proxy de federación 2.0 de AD FS a un archivo. Puedes hacerlo mediante Windows PowerShell.  
  
Abre Windows PowerShell y ejecuta el siguiente comando para agregar los cmdlets de AD FS a la sesión de Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. A continuación, ejecuta el siguiente comando para exportar propiedades del servidor proxy de federación a un archivo:`PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3.  Asegúrate de que conoces las credenciales de cuenta que es un administrador del servidor de federación de AD FS o la cuenta de servicio en la que se ejecuta el servicio de federación de AD FS.  Esta información es necesaria para la configuración de proxy de confianza.  
  
 Completar esta resultados paso en la recopilación de la siguiente información que se necesita para configurar al proxy de servidor de federación de AD FS:  
  
-   Nombre de servicio de federación de AD FS  
  
-   Nombre de la cuenta de dominio que se necesita para la configuración de proxy de confianza  
  
-   La dirección y el puerto del proxy HTTP (si hay un proxy HTTP entre el proxy de servidor de federación de AD FS y los servidores de federación de AD FS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Paso 2: Realizar una copia de las personalizaciones de la página Web  
 Para hacer una copia de las personalizaciones de la página Web, copiar las páginas Web de proxy de AD FS y **web.config** archivo desde el directorio que se asigna a la ruta de acceso virtual **"/ adfs/ls"** en IIS.  De manera predeterminada, está en la **%systemdrive%\inetpub\adfs\ls** directorio.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Preparación para migrar el servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparación para migrar al Proxy de servidor de AD FS federación 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar el servidor de AD FS federación 2.0](migrate-the-ad-fs-fed-server.md)   
 [Migrar al Proxy de servidor de AD FS federación 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar a los agentes de AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)