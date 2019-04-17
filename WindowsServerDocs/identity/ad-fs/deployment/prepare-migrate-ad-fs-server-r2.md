---
title: "Preparación para migrar el servidor de AD FS 2.0 federación a AD FS en Windows Server 2012 R2"
description: "Proporciona información sobre la preparación migrar el servidor de AD FS a Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparación para migrar el servidor de AD FS 2.0 federación a AD FS en Windows Server 2012 R2

Este documento describe cómo migrar una granja de servidores de federación de Windows Server 2012 o un AD FS 2.0a un conjunto de Windows Server 2012 R2 AD FS.  Los pasos pueden usarse con conjuntos de AD FS que usan WID o SQL Server, como la base de datos subyacente.  
  
-   [Descripción del proceso de migración](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Nueva funcionalidad de AD FS en Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisitos de AD FS en Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumenta los límites de Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Otras tareas de migración y las consideraciones de](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Descripción del proceso de migración  
 Para completar la migración de la granja de servidores de federación de AD FS a Windows Server 2012 R2, debes realizar las siguientes tareas:  
  
1.  Exportar, registro y la copia de seguridad de los siguientes datos de configuración en el conjunto de AD FS existente. Para obtener instrucciones detalladas sobre cómo completar estas tareas, consulta [migrar el servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md).  
  
La siguiente configuración se migra con los scripts ubicados en la carpeta \support\adfs en el CD de instalación de Windows Server 2012 R2:  
  
-   Las notificaciones de confianzas de proveedor, a excepción de las reglas de notificación personalizada en la confianza del proveedor de notificaciones de Active Directory. Para obtener más información, consulta [migrar el servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md).  
  
-   Confiar confianzas de terceros.  
  
-   AD FS generado internamente, la firma de tokens autofirmado y certificados de descifrado token.  
  
Cualquiera de los siguientes valores personalizados deben migrarse manualmente:  
  
 -   Configuración del servicio:  
  
     -   No predeterminado de firma de tokens y los certificados de descifrado tokens emitidos por una empresa o una entidad de certificación públicas.  
  
     -   El certificado de autenticación de servidor SSL usado por AD FS.  
  
     -   El certificado de comunicaciones de servicio usado por AD FS (de forma predeterminada, es el mismo certificado que el certificado SSL.  
  
      -   Valores predeterminados de no de las propiedades de servicio de federación, como la duración AutoCertificateRollover o SSO.  
  
      -   La configuración de extremo de AD FS no predeterminada y las descripciones de la reclamación.  
  
-   Personalizadas aseguran las reglas en la confianza de proveedor de reclamaciones de Active Directory.  
  
    -   Personalizaciones de página de inicio de sesión de AD FS  
  
Para obtener más información, consulta [migrar el servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md).  
  
2.  Crear una granja de servidores de federación de Windows Server 2012 R2.  
  
3.  Importar los datos de la configuración original a este nuevo conjunto de Windows Server 2012 R2 AD FS.  
  
4.  Configurar y personalizar las páginas de inicio de sesión de AD FS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Nueva funcionalidad de AD FS en Windows Server 2012 R2  
 Las siguientes funciones de AD FS los cambios en Windows Server 2012 R2 impacto una migración de AD FS 2.0 o AD FS en Windows Server 2012:  
  
**Dependencia IIS**  
   - AD FS en Windows Server 2012 R2 está hospedado en sí mismo y no requiere la instalación de IIS. Asegúrate de que se tenga en cuenta las siguientes acciones como resultado de este cambio:  
   -   Administración de certificados SSL para servidores de federación y equipos de proxy en el conjunto de AD FS ahora debe realizarse a través de Windows PowerShell.  
  
**Cambios en la configuración y las personalizaciones de AD FS inicio de sesión en las páginas**  
-   En AD FS en Windows Server 2012 R2, hay varios cambios destinados a mejorar la experiencia de inicio de sesión para los administradores y usuarios. Ahora se quitan las páginas web hospedada de IIS que existían en la versión anterior de AD FS. La apariencia de las páginas de web AD FS inicio de sesión automático se hospedan en AD FS y ahora se puede personalizar para adaptar la experiencia del usuario. Los cambios incluyen:  
    -   Personalizar la AD FS sesión experiencia, incluida la personalización del nombre de la compañía, logotipo, ilustraciones y descripción de inicio de sesión.  
    -   Personalización de los mensajes de error.  
    -   Personalizar la experiencia del detección Home territorio ADFS, que incluye lo siguiente:  
        -   Configurar el proveedor de identidad para usar determinados sufijos de correo electrónico.  
        -   Configurar una lista de proveedores de identidad por confiar terceros.  
        -   Se omitirá la detección del territorio Home de intranet.  
        -   Crear temas personalizados web.  
  
Para obtener instrucciones detalladas sobre cómo configurar la apariencia de las páginas de inicio de sesión de AD FS, consulta [personalizar las páginas AD FS Sign-in](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
Si tienes personalización de la página web en su conjunto de AD FS existente que quieres migrar a Windows Server 2012 R2, puedes volver a crear como parte del proceso de migración con las nuevas características de personalización en Windows Server 2012 R2.  
  
-   **Otros cambios**  
  
    -   AD FS en Windows Server 2012 R2 se basa en Windows Identity Foundation (WIF) 3.5, no WIF 4.5. Por lo tanto, no se admiten algunas características específicas de WIF 4.5 (por ejemplo, las reclamaciones de Kerberos y control de acceso dinámico) de AD FS en Windows Server 2012 R2.  
  
    -   Servicio de registro de dispositivo (DRS) en Windows Server 2012 R2 funciona en el puerto 443; Funciona ClientTLS para la autenticación de certificado de usuario en el puerto 49443  
  
        -   Para activos, distintos al navegador clientes mediante la autenticación del modo de transporte de certificado que están específicamente codificado de forma rígida para que apunte al puerto 443, un cambio de código es necesaria para seguir usando la autenticación de certificado de usuario en el puerto 49443.  
  
        -   Para las aplicaciones pasivas ningún cambio es necesario porque AD FS redirige al puerto correcto para la autenticación de certificado de usuario.  
  
        -   Puertos del firewall entre el cliente y el proxy deben habilitar el tráfico de puerto 49443 pasar para la autenticación de certificado de usuario.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisitos de AD FS en Windows Server 2012 R2  
 Para migrar correctamente la granja de servidores de AD FS a Windows Server 2012 R2, debe cumplir los siguientes requisitos:  
  
 Para AD FS funcione, todos los equipos que quieras que se encuentre una federación deben estar unido a un dominio.  
  
 De AD FS ejecutando en Windows Server 2012 R2a la función, el dominio de Active Directory debe ejecutar cualquiera de las siguientes acciones:  
  
-   Windows Server 2012 R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 Si tienes previsto usar un grupo de la cuenta de servicio administrada (gMSA) como la cuenta de servicio de AD FS, debes tener al menos un controlador de dominio en el entorno que se ejecuta en el sistema operativo Windows Server 2012 o Windows Server 2012 R2.  
  
 Si piensas implementar el servicio de registro de dispositivo (DRS) para unirse a un área de trabajo de AD como parte de la implementación de AD FS, el esquema de AD DS debe actualizarse en el nivel de Windows Server 2012 R2. Hay tres maneras de actualizar el esquema:  
  
1.  En un bosque de Active Directory existente, ejecute adprep /forestprep desde la carpeta \support\adprep del sistema operativo Windows Server 2012 R2 DVD en cualquier servidor de 64 bits que se ejecuta Windows Server 2008 o posterior. En este caso, no hay ningún controlador de dominio adicional debe instalarse y ningún controlador de dominio existente debe actualizarse.  
  
Para ejecutar adprep/forestprep, debe ser miembro del grupo Administradores de esquema, el grupo de administradores de empresa y el grupo de administradores de dominio del dominio que hospeda al maestro de esquema.  
  
2.  En un bosque de Active Directory existente, instala un controlador de dominio que ejecute Windows Server 2012 R2. En este caso, adprep /forestprep se ejecuta automáticamente como parte de la instalación del controlador de dominio.  
  
Durante la instalación del controlador de dominio, tienes que especificar credenciales adicionales para poder ejecutar adprep /forestprep.  
  
3.  Crear un nuevo bosque de Active Directory mediante la instalación de AD DS en un servidor que ejecuta Windows Server 2012 R2. En este caso, adprep /forestprep no deben ejecutarse porque el esquema se creará inicialmente con todos los objetos para admitir DRS y contenedores necesarios.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Soporte técnico de SQL Server de AD FS en Windows Server 2012 R2  
 Si quieres crear una granja de servidores de AD FS y usar SQL Server para almacenar los datos de configuración, puedes usar SQL Server 2008 y las versiones más recientes, incluido SQL Server 2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Aumenta los límites de Windows PowerShell  
 Si tienes más de 1.000 notificaciones confianzas de proveedor y confianzas de usuario de confianza en el conjunto de AD FS, o Si ves el siguiente error al intentar ejecutar la herramienta de exportación e importación de AD FS migración, deberás aumentar los límites de Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Este error se produce porque el límite de memoria de Windows PowerShell sesión predeterminado es demasiado bajo. En Windows PowerShell 2.0, la memoria de manera predeterminada de sesión es 150MB. En Windows PowerShell 3.0, la memoria de manera predeterminada de sesión es 1024MB. Puedes comprobar el límite de memoria de una sesión remota de Windows PowerShell usando el siguiente comando:`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. Puedes aumentar el límite ejecutando el siguiente comando:`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Otras tareas de migración y las consideraciones de  
 Para migrar correctamente la granja de servidores de AD FS a Windows Server 2012 R2, asegúrate de que el que estés informado de las siguientes acciones:  
  
-   Los scripts de migración que se encuentra en la carpeta \support\adfs en el CD de instalación de Windows Server 2012 R2 requieren que conservan el mismo nombre de granja de servidor de federación y el nombre de identidad de cuenta de servicio que usaste en el conjunto de AD FS heredado al migrar a Windows Server 2012 R2.  
  
-   Si vas a migrar un conjunto de SQL Server AD FS, ten en cuenta que el proceso de migración implica la creación de una nueva instancia de base de datos SQL en la que debe importar los datos de la configuración original.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar los servicios de rol de servicios de federación de Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrar al Proxy de servidor de federación de AD FS](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)