---
title: Preparar la migración del servidor de AD FS 2.0 Federation a AD FS en Windows Server 2012 R2
description: Proporciona información sobre cómo prepararse para migrar el servidor de AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889026"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Preparar la migración del servidor de AD FS 2.0 Federation a AD FS en Windows Server 2012 R2

Este documento describe cómo migrar un AD FS 2.0 o una granja de servidores de federación de Windows Server 2012 a una granja de servidores de Windows Server 2012 R2 AD FS.  Los pasos pueden usarse con granjas de servidores de AD FS utiliza WID o SQL Server como base de datos subyacente.  
  
-   [Esquema del proceso de migración](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Nueva funcionalidad de AD FS en Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Requisitos de AD FS en Windows Server 2012 R2](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [Aumentar los límites de Windows PowerShell](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [Otras consideraciones y tareas de migración](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>Esquema del proceso de migración  
 Para realizar la migración de la granja de servidores de AD FS a Windows Server 2012 R2, debe completar las siguientes tareas:  
  
1.  Exportar, registrar y hacer una copia de seguridad de los siguientes datos de configuración en la granja de AD FS existente. Para obtener instrucciones detalladas para completar estas tareas, vea [Migración del servidor proxy de federación de AD FS](migrate-ad-fs-fed-server-r2.md).  
  
La siguiente configuración se migra con los scripts ubicados en la carpeta \support\adfs del CD de instalación de Windows Server 2012 R2:  
  
-   Relaciones de confianza para proveedor de notificaciones, a excepción de las reglas de notificación personalizadas en la relación de confianza para proveedor de notificaciones de Active Directory. Para obtener más información, vea [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md).  
  
-   Relaciones de confianza para usuario autenticado.  
  
-   Certificados de firma y descifrado de tokens de AD FS autofirmados y generados internamente.  
  
Las opciones de configuración personalizadas siguientes se deben migrar manualmente:  
  
 -   Configuración de servicios:  
  
     -   Certificados de firma y descifrado de tokens no predeterminados emitidos por una empresa o una entidad de certificación pública.  
  
     -   El certificado de autenticación de servidor SSL utilizado por AD FS.  
  
     -   El certificado de comunicaciones de servicio utilizado por AD FS (de manera predeterminada, se trata del mismo certificado que el certificado SSL).  
  
      -   Valores no predeterminados de cualquier propiedad del Servicio de federación, como AutoCertificateRollover o la duración de SSO.  
  
      -   Configuración de extremos y descripciones de notificaciones de AD FS no predeterminadas.  
  
-   Reglas de notificación personalizadas en la relación de confianza para proveedor de notificaciones de Active Directory.  
  
    -   Personalizaciones de la página de inicio de sesión de AD FS.  
  
Para obtener más información, vea [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md).  
  
2.  Crear una granja de servidores de federación de Windows Server 2012 R2.  
  
3.  Importar los datos de configuración originales a esta nueva granja de AD FS de Windows Server 2012 R2.  
  
4.  Configurar y personalizar las páginas de inicio de sesión de AD FS.  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Nuevas funciones de AD FS en Windows Server 2012 R2  
 La siguiente funcionalidad de AD FS cambia en Windows Server 2012 R2 impacto una migración de AD FS 2.0 o AD FS en Windows Server 2012:  
  
**Dependencia IIS**  
   - AD FS en Windows Server 2012 R2 está autohospedado y no requiere instalar IIS. Asegúrese de tener en cuenta lo siguiente a consecuencia de este cambio:  
   -   Ahora, la administración de certificados SSL para los servidores de federación y los equipos proxy en la granja de AD FS se debe realizar mediante Windows PowerShell.  
  
**Cambios realizados en la configuración y las personalizaciones de inicio de sesión de páginas de AD FS**  
-   En AD FS en Windows Server 2012 R2, hay varios cambios a fin de mejorar la experiencia de inicio de sesión para administradores y usuarios. Las páginas web hospedadas por IIS de la versión anterior de AD FS se han eliminado. La apariencia de las páginas de inicio de sesión de AD FS, que ahora están autohospedadas en AD FS, se puede personalizar para adaptarse a la experiencia de usuario. Los cambios son:  
    -   Se puede personalizar la experiencia de inicio de sesión de AD FS, incluida la personalización del nombre de la compañía, el logotipo, la ilustración y la descripción del inicio de sesión.  
    -   Se puede personalizar los mensajes de error.  
    -   Se puede personalizar la experiencia de detección del dominio de inicio de AD FS, lo que incluye:  
        -   Configurar el proveedor de identidades para utilizar ciertos sufijos de correo electrónico  
        -   Configurar una lista de proveedores de identidades por relación de confianza para usuario autenticado  
        -   Omitir la detección del dominio de inicio para la intranet  
        -   Crear temas web personalizados  
  
Para obtener instrucciones detalladas para configurar la apariencia de las páginas de inicio de sesión de AD FS, vea [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
Si ha personalizado las páginas web de la granja de AD FS existente que se va a migrar a Windows Server 2012 R2, puede volver a crearlas como parte del proceso de migración con las nuevas características de personalización en Windows Server 2012 R2.  
  
-   **Otros cambios**  
  
    -   AD FS en Windows Server 2012 R2 se basa en Windows Identity Foundation (WIF) 3.5, no en WIF 4.5. Por lo tanto, no se admiten algunas características concretas de WIF 4.5 (por ejemplo, las notificaciones de Kerberos y control de acceso dinámico) de AD FS en Windows Server 2012 R2.  
  
    -   Servicio de registro de dispositivos (DRS) en Windows Server 2012 R2 funciona en el puerto 443; ClientTLS para la autenticación de certificados de usuario funciona en el puerto 49443  
  
        -   Para los clientes activos que no sean navegadores y que usen la autenticación de modo de transporte de certificados que estén codificados de forma rígida específicamente para apuntar al puerto 443, es necesario cambiar el código para seguir utilizando la autenticación de certificados de usuario en el puerto 49443.  
  
        -   Para las aplicaciones pasivas no hay que hacer cambios porque AD FS redirige al puerto correcto para la autenticación de certificados de usuario.  
  
        -   Los puertos de firewall entre el cliente y el proxy deben permitir el tráfico en el puerto 49443 para la autenticación de certificados de usuario.  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Requisitos de AD FS en Windows Server 2012 R2  
 Para poder migrar correctamente la granja de AD FS a Windows Server 2012 R2, debe cumplir los siguientes requisitos:  
  
 Para AD FS funcione, cada equipo que desee que sea una federación debe estar unido a un dominio.  
  
 Para AD FS que se ejecuta en Windows Server 2012 R2 a función, el dominio de Active Directory debe ejecutar cualquiera de las siguientes acciones:  
  
-   Windows Server 2012 R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 Si va a usar un grupo de cuenta de servicio administrada (gMSA) como la cuenta de servicio de AD FS, debe tener al menos un controlador de dominio en su entorno que se está ejecutando en el sistema operativo Windows Server 2012 o Windows Server 2012 R2.  
  
 Si tiene previsto implementar el servicio de registro de dispositivos (DRS) para el área de trabajo de AD como parte de la implementación de AD FS, el esquema de AD DS debe actualizarse en el nivel de Windows Server 2012 R2. Hay tres maneras de actualizar el esquema:  
  
1.  En un bosque de Active Directory existente, ejecute adprep /forestprep desde la carpeta \support\adprep del DVD del sistema de operativo de Windows Server 2012 R2 en cualquier servidor de 64 bits que ejecute Windows Server 2008 o posterior. En este caso, no es necesario instalar ningún controlador de dominio adicional ni actualizar ningún controlador de dominio existente.  
  
Para ejecutar adprep /forestprep, es necesario ser miembro de los grupos Administradores de esquema, Administradores de empresa y Administradores del dominio del dominio que hospeda el maestro de esquema.  
  
2.  En un bosque de Active Directory existente, instale un controlador de dominio que ejecuta Windows Server 2012 R2. En este caso, adprep /forestprep se ejecuta automáticamente como parte de la instalación del controlador de dominio.  
  
Durante la instalación del controlador de dominio, es posible que deba especificar credenciales adicionales para ejecutar adprep /forestprep.  
  
3.  Crear un nuevo bosque de Active Directory mediante la instalación de AD DS en un servidor que ejecuta Windows Server 2012 R2. En este caso, no es necesario ejecutar adprep /forestprep porque el esquema se creará inicialmente con todos los contenedores y objetos necesarios para admitir DRS.  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>Compatibilidad con SQL Server para AD FS en Windows Server 2012 R2  
 Si desea crear una granja de AD FS y usar SQL Server para almacenar los datos de configuración, puede usar SQL Server 2008 y versiones posteriores, incluido SQL Server 2012.  
  
##  <a name="increasing-your-windows-powershell-limits"></a>Cómo aumentar los límites de Windows PowerShell  
 Si tiene más de 1.000 relaciones de confianza para proveedor de notificaciones y relaciones de confianza para usuario autenticado en la granja de AD FS o si ve el siguiente error al intentar ejecutar la herramienta de exportación o importación de la migración de AD FS, debe aumentar los límites de Windows PowerShell:  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 Este error se produce porque el límite de memoria predeterminado de sesión de Windows PowerShell es demasiado bajo. En Windows PowerShell 2.0, la memoria predeterminada de sesión es de 150 MB. En Windows PowerShell 3.0, la memoria predeterminada de sesión es de 1024 MB. Puede comprobar el límite de memoria de sesión remota de Windows PowerShell mediante el siguiente comando: `Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`. Para aumentar el límite, ejecute el siguiente comando: `Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`.  
  
## <a name="other-migration-tasks-and-considerations"></a>Otras tareas y cuestiones a tener en cuenta al realizar la migración  
 Para migrar correctamente la granja de AD FS a Windows Server 2012 R2, tenga en cuenta lo siguiente:  
  
-   Los scripts de migración ubicados en la carpeta \support\adfs del CD de instalación de Windows Server 2012 R2 requieren que conservar el mismo nombre de granja de servidores de federación y el nombre de identidad de cuenta de servicio que usó en la granja de AD FS heredada al migrarla a Windows Server 2012 R2.  
  
-   Si desea migrar una granja de AD FS de SQL Server, tenga en cuenta que el proceso de migración implica crear una instancia de la base de datos de SQL en la que deberá importar los datos de configuración originales.  
  
## <a name="next-steps"></a>Pasos siguientes
 [Migrar servicios de rol de servicios de federación de Active Directory a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Migración del servidor de federación de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migración del servidor Proxy de federación de AD FS](migrate-fed-server-proxy-r2.md)   
 [Comprobar la migración de AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)