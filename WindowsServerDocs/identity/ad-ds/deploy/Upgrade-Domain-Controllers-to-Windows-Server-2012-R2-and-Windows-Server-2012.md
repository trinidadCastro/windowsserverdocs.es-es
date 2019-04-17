---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e317c5a939d417bac844c4080223d7b5e0eec149
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona información general sobre los servicios de dominio de Active Directory en Windows Server 2012 R2 y Windows Server 2012 y explica el proceso para actualizar los controladores de dominio de Windows Server 2008 o Windows Server 2008 R2.  
  
-   [Pasos de actualización de controlador de dominio](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradeWorkflow)  
  
-   [Novedades en Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewEight)  
  
-   [Novedades en AD DS en Windows Server 2012 R2](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_NewWS2012R2)  
  
-   [Novedades en AD DS en Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewAD)  
  
-   [Cambios de instalación del rol de servidor de AD DS](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_InstallationChanges)  
  
-   [Características en desuso y cambios de comportamiento relacionados a AD DS en Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)  
  
-   [Requisitos del sistema operativo](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_SysReqs)  
  
-   [En lugar de actualización admitidas](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradePaths)  
  
-   [Requisitos y características de nivel funcionales](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_FunctionalLevels)  
  
-   [Interoperabilidad de AD DS con otros roles de servidor y sistemas operativos Windows](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_ServerRoles)  
  
-   [Roles de maestro de operaciones](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_OpsMasters)  
  
-   [Virtualización de controladores de dominio que ejecutan Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Virtual)  
  
-   [Administración de los servidores de Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Admin)  
  
-   [Compatibilidad de aplicaciones](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)  
  
-   [Problemas conocidos](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_KnownIssues)  
  
## <a name="BKMK_UpgradeWorkflow"></a>Pasos de actualización de controlador de dominio  
El procedimiento recomendado para actualizar un dominio es promover controladores de dominio que ejecutan versiones más recientes de Windows Server y la degradación de los controladores de dominio anteriores según sea necesario. Ese método es preferible actualizar el sistema operativo de un controlador de dominio. Esta lista describe los pasos generales que seguir antes de promover un controlador de dominio que ejecute una versión más reciente de Windows Server:  
  
1.  Comprueba el servidor de destino cumpla [requisitos del sistema](https://technet.microsoft.com/library/dn303418.aspx).  
  
2.  Comprobar [compatibilidad de aplicaciones](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
  
3.  Comprueba la configuración de seguridad. Para obtener más información, consulta [relacionados con los cambios de comportamiento y características de desuso en AD DS en Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) y [seguro de la configuración predeterminada en Windows Server 2008 y Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
  
4.  Comprobar la conectividad al servidor de destino desde el equipo donde tiene previsto ejecutar la instalación.  
  
5.  Comprobar la disponibilidad de funciones de maestro de operación necesarias:  
  
    -   Para instalar el primer controlador de dominio que ejecuta Windows Server 2012 en un bosque y dominio de existente, el equipo donde se ejecuta la instalación debe conectividad con el patrón de esquema para poder ejecutar adprep /forestprep y el patrón de infraestructura para poder ejecutar adprep /domainprep.  
  
    -   Para instalar el primer controlador de dominio en un dominio donde ya ha ampliado el esquema del bosque, sólo necesita conectividad a maestro de infraestructura.  
  
    -   Para instalar o quitar un dominio en un bosque existente, necesita conectividad con el patrón de nomenclatura de dominio.  
  
    -   Cualquier instalación del controlador de dominio también requiere conectividad al maestro RID.  
  
    -   Si vas a instalar el primer controlador de dominio de solo lectura en un bosque existente, debes tener conectividad con el patrón de infraestructura para cada partición de directorio de aplicación, también conocido como un contexto de nomenclatura no del dominio o NDS NDNC.  
  
6.  Asegúrate de proporcionar las credenciales necesarias para ejecutar la instalación de AD DS.  
  
    |Acción de instalación|Requisitos de credenciales|  
    |-----------------------|---------------------------|  
    |Instala un bosque nuevo|Administrador local en el servidor de destino|  
    |Instalar un nuevo dominio en un bosque existente|Administradores de empresa|  
    |Instalar un controlador de dominio adicional en un dominio existente|Administradores de dominio|  
    |Ejecutar adprep /forestprep|Administradores de dominio, administradores de empresa y administradores de esquema|  
    |Ejecutar adprep /domainprep|Administradores de dominio|  
    |Ejecutar adprep /domainprep /gpprep|Administradores de dominio|  
    |Ejecutar adprep /rodcprep|Administradores de empresa|  
  
    Puede delegar permisos para instalar AD DS. Para obtener más información, consulta [tareas de administración de instalación](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  
  
Instrucciones de pasos paso a paso para promocionar nuevas y controladores de dominio de réplica de Windows Server 2012 mediante cmdlets de Windows PowerShell y el administrador del servidor pueden encontrarse en los siguientes vínculos:  
  
-   [Instalar servicios de dominio de Active Directory (nivel 100)](https://technet.microsoft.com/library/hh472162.aspx)  
  
-   [Instalar un bosque nuevo Windows Server 2012 Active Directory (nivel 200)](https://technet.microsoft.com/library/jj574166.aspx)  
  
-   [Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente (nivel 200)](https://technet.microsoft.com/library/jj574134.aspx)  
  
-   [Instalar un nuevo elemento secundario Active Directory de Windows Server 2012 o dominio del árbol (nivel 200)](https://technet.microsoft.com/library/jj574105.aspx)  
  
-   [Instalar Windows Server 2012 Active Directory Read-Only controlador de dominio (RODC) (nivel 200)](https://technet.microsoft.com/library/jj574152.aspx)  
  
-   [Guía paso a paso para la configuración de seguridad de Windows Server 2012 controlador de dominio (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  
  
## <a name="BKMK_WhatsNewEight"></a>Novedades en Windows Server 2012  
Nuevas características indican en el rol de servidor y área de tecnología se muestran en la siguiente tabla. Para obtener más documentos, vídeos de demostración y presentaciones sobre otras características en Windows Server 2012, consulta [Server y la plataforma en la nube](https://www.microsoft.com/server-cloud/default.aspx).  
  
||||  
|-|-|-|  
|[Servicios de certificados de Active Directory (AD CS)](https://technet.microsoft.com/library/hh831373.aspx)|[Active Directory Rights Management Services (AD RMS)](https://technet.microsoft.com/library/hh831554.aspx)|[Cifrado de unidad BitLocker](https://technet.microsoft.com/library/hh831412.aspx)|  
|[BranchCache](https://technet.microsoft.com/library/jj127252.aspx)|[Protocolo de configuración de Host dinámica (de host DHCP)](https://technet.microsoft.com/library/jj200226.aspx)|[Sistema de nombres de dominio (DNS)](https://technet.microsoft.com/library/jj200224.aspx)|  
|[Clústeres de conmutación por error](https://technet.microsoft.com/library/hh831414.aspx)|[Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/hh831746.aspx)|[Directiva de grupo](https://technet.microsoft.com/library/jj574108.aspx)|  
|[Hyper-V](https://technet.microsoft.com/library/hh831410.aspx)|[Administración de direcciones IP (IPAM)](https://technet.microsoft.com/library/jj200214.aspx)|[Autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx)|  
|[Las cuentas de servicio administradas](https://technet.microsoft.com/library/hh831451.aspx)|[Funciones de red](https://technet.microsoft.com/library/jj200215.aspx)|[Servicios de escritorio remoto](https://technet.microsoft.com/library/hh831527.aspx)|  
|[Las auditorías de seguridad](https://technet.microsoft.com/library/hh849638.aspx)|[Administrador del servidor](https://blogs.technet.com/b/servermanager/archive/2012/06/27/server-manager-power-of-many-simplicity-of-one.aspx)|[Tarjetas inteligentes](https://technet.microsoft.com/library/hh849637.aspx)|  
|[TLS/SSL (Schannel SSP)](https://technet.microsoft.com/library/hh831771.aspx)|[Servicios de implementación de Windows](https://technet.microsoft.com/library/hh974416.aspx)|[Windows PowerShell 3.0](https://technet.microsoft.com/library/hh857339)|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a>Automático mantenimiento y cambios de comportamiento de reinicio después de aplican las actualizaciones de Windows Update  
Antes del lanzamiento de Windows 8, Windows Update administra su propia programación interno para buscar actualizaciones y para descargarlos e instalarlos. Es necesario que el agente de actualización de Windows siempre se estaba ejecutando en segundo plano, consumiendo memoria y otros recursos del sistema.  
  
Windows 8 y Windows Server 2012 presentan una nueva característica denominada [mantenimiento automático](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). El mantenimiento automático consolida muchas características diferentes que usa para administrar su propia programación cada y lógica de ejecución. Esta consolidación permite que todos estos componentes usan mucho menos recursos del sistema, funcionan de manera coherente, respeta el nuevo [modo de espera conectado](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) de estado para nuevos tipos de dispositivos y consume menos batería en dispositivos portátiles.  
  
Dado que Windows Update es una parte de un mantenimiento automático en Windows 8 y Windows Server 2012, ya no es eficaz su propia programación interno para establecer una fecha y hora para instalar las actualizaciones. Para ayudar a garantizar coherente y predecible reiniciar comportamiento para todos los dispositivos y equipos de tu empresa, incluidas las que se ejecutan Windows 8 y Windows Server 2012, consulta el tema Microsoft KB artículo [2885694](https://support.microsoft.com/kb/2885694) (o consulta acumulativo de octubre de 2013 [2883201](https://support.microsoft.com/kb/2883201)), a continuación, configura la configuración de directiva se describe en la entrada de blog WSUS [lo que permite una experiencia de Windows Update más predecible para Windows 8 y Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx).  
  

## <a name="BKMK_NewWS2012R2"></a>Novedades en AD DS en Windows Server 2012 R2  
La siguiente tabla resume las nuevas características de AD DS en Windows Server 2012 R2, con un vínculo a información más detallada que está disponible. Para obtener una explicación más detallada de algunas características, como sus requisitos, consulta [Novedades en Active Directory en Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  
  
|Característica|Descripción|  
|-----------|---------------|  
|[Unirse a un área de trabajo](https://technet.microsoft.com/library/dn280945.aspx)|Permite a los trabajadores de información para unirte a sus dispositivos personales con su empresa para acceder a los servicios y recursos de la compañía.|  
|[Proxy de aplicación Web](https://technet.microsoft.com/library/dn280942.aspx)|Proporciona acceso a la aplicación web con un nuevo servicio de rol de acceso remoto.|  
|[Servicios de federación de Active Directory](https://technet.microsoft.com/library/hh831502.aspx)|AD FS tiene la implementación simplificada y mejoras para que los usuarios puedan acceder a recursos de dispositivos personales y ayudar a los departamentos de TI a administrar el control de acceso.|  
|[Exclusividad SPN y UPN](https://technet.microsoft.com/library/dn535779.aspx)|Controladores de dominio que ejecutan Windows Server 2012 R2 bloquean la creación de duplicados principal nombres de servicio (SPN) y nombres principales de usuario (UPN).|  
|[Winlogon automático reinicio sesión (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|Permite bloquea aplicaciones de la pantalla para que se reinicia y está disponible en dispositivos de Windows 8.1.|  
|[Atestación de clave de TPM](https://technet.microsoft.com/library/dn581921.aspx)|Permite que las entidades de certificación certificar criptográficamente en un certificado emitido que la clave privada solicitante realmente está protegida por un módulo de plataforma segura (TPM).|  
|[Administración y protección de credenciales](https://technet.microsoft.com/library/dn408190.aspx)|Credenciales protección y dominio de autenticación controles nuevos para reducir el robo de credenciales.|  
|[Desuso de servicio de replicación de archivos (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|El nivel funcional de dominio de Windows Server 2003 también está en desuso porque en el nivel funcional, FRS se usa para replicar SYSVOL. Que significa que cuando se crea un nuevo dominio en un servidor que ejecuta Windows Server 2012 R2, el nivel funcional del dominio debe Windows Server 2008 o posterior. Aún puedes agregar un controlador de dominio que ejecute Windows Server 2012 R2 a un dominio existente que tiene un nivel funcional del dominio de Windows Server 2003; simplemente no puedes crear un nuevo dominio en ese mismo nivel.|  
|[Nuevos niveles funcionales de dominios y bosques](../active-directory-functional-levels.md)|Existen nuevos niveles funcionales de Windows Server 2012 R2. Nuevas características están disponibles en Windows Server 2012 R2 DFL.|  
|[Cambios del optimizador de consultas LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Mejora del rendimiento en la eficacia de la búsqueda LDAP y la hora de búsqueda LDAP de consultas complejas.|  
|[Mejoras de evento 1644](https://technet.microsoft.com/library/dn535775.aspx)|Estadísticas de resultados de búsqueda LDAP se agregaron al evento 1644 Id. para facilitar la solución de problemas.|  
|[Mejora de rendimiento de Active Directory replicación](https://technet.microsoft.com/library/dn535775.aspx)|Ajusta el rendimiento máximo de replicación de AD de 40Mbps a alrededor de 600 Mbps|  
  
## <a name="BKMK_WhatsNewAD"></a>Novedades en AD DS en Windows Server 2012  
La siguiente tabla resume las nuevas características de AD DS en Windows Server 2012, con un vínculo a información más detallada que está disponible. Para obtener una explicación más detallada de algunas características, como sus requisitos, consulta [Novedades de servicios de dominio de Active Directory (AD DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Característica|Descripción|  
|-----------|---------------|  
|Consulta de Active Directory-Based Activation (AD BA) [información general sobre la activación de volumen](https://technet.microsoft.com/library/hh831612.aspx)|Simplifica la tarea de configurar la distribución y administración de licencias por volumen de software.|  
|[Servicios de federación de Active Directory (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|Agrega la instalación del rol mediante el administrador del servidor, simplificado el programa de instalación de confianza, administración de confianza automática, compatibilidad con el protocolo de SAML y mucho más.|  
|Eventos de vaciado de página perdido de Active Directory|Se registra el evento de NTDS ISAM 530 con error jet-1119 para detectar los eventos de vaciado página perdido bases de datos de Active Directory.|  
|[Interfaz de usuario de la Papelera de reciclaje de Active Directory](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Centro de administración de Active Directory (ADAC) agrega administración de la interfaz gráfica de usuario de la característica de la Papelera de reciclaje originalmente introducida en Windows Server 2008 R2.|  
|[Active cmdlets de replicación de directorio y topología de Windows PowerShell](https://technet.microsoft.com/library/hh831757.aspx)|Admite la creación y administración de sitios de Active Directory, vínculos a sitios, objetos de conexión y más mediante Windows PowerShell.|  
|[Control de acceso dinámico](https://technet.microsoft.com/library/hh831717.aspx)|Nueva plataforma de autorización basado en autenticaciones que mejora el modelo de control de acceso heredadas.|  
|[Interfaz de usuario de directivas de contraseña muy específicas](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC agrega compatibilidad de la interfaz gráfica de usuario para la creación, edición y la asignación de PSO agregado originalmente en Windows Server 2008.|  
|[Cuentas de servicio administradas de grupo (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Un nueva entidad de seguridad tipo de seguridad conocido como un gMSA. Servicios que se ejecutan en varios hosts pueden ejecutar en la misma cuenta de gMSA.|  
|[DirectAccess unirse a un dominio sin conexión](https://technet.microsoft.com/library/jj574150.aspx)|Incluidos los requisitos previos de DirectAccess para ampliar la unión al dominio sin conexión.|  
|[Implementación rápida a través de clonación del controlador de dominio virtual](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|Controladores de dominio virtualizadas se pueden implementar rápidamente duplicando los controladores de dominio virtual existentes mediante los cmdlets de Windows PowerShell.|  
|[ELIMINAR los cambios de grupo](https://technet.microsoft.com/library/jj574229.aspx)|Agrega nuevos eventos de supervisión y cuotas para protegerse de consumo excesivo del conjunto de RID global. Opcionalmente, duplica el tamaño del conjunto global de RID si se convierte en agotado el grupo original.|  
|Proteger el servicio de hora|Mejora la seguridad para W32tm quitar secretos de la conexión, quitando las funciones de hash MD5 y que requieren el servidor al autenticar con los clientes de tiempo de Windows 8|  
|[Protección de reversión de USN para controladores de dominio virtualizadas](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|Accidentalmente restaurar instantánea copias de seguridad de controladores de dominio virtualizadas ya no hace que la reversión de USN.|  
|[Visor de Windows PowerShell historial](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Los administradores pueden ver los comandos de Windows PowerShell que se ejecuta al usar ADAC.|  
  
### <a name="BKMK_"></a>Automático mantenimiento y cambios de comportamiento de reinicio después de aplican las actualizaciones de Windows Update  
Antes del lanzamiento de Windows 8, Windows Update administra su propia programación interno para buscar actualizaciones y para descargarlos e instalarlos. Es necesario que el agente de actualización de Windows siempre se estaba ejecutando en segundo plano, consumiendo memoria y otros recursos del sistema.  
  
Windows 8 y Windows Server 2012 presentan una nueva característica denominada [mantenimiento automático](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). El mantenimiento automático consolida muchas características diferentes que usa para administrar su propia programación cada y lógica de ejecución. Esta consolidación permite que todos estos componentes usan mucho menos recursos del sistema, funcionan de manera coherente, respeta el nuevo [modo de espera conectado](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) de estado para nuevos tipos de dispositivos y consume menos batería en dispositivos portátiles.  
  
Dado que Windows Update es una parte de un mantenimiento automático en Windows 8 y Windows Server 2012, ya no es eficaz su propia programación interno para establecer una fecha y hora para instalar las actualizaciones. Para ayudar a garantizar el comportamiento de reinicio coherente y predecible para todos los dispositivos y equipos de la empresa, incluidos aquellos que ejecutan Windows 8 y Windows Server 2012, puedes configurar la configuración de directiva de grupo siguiente:  
  
-   **Configuración del equipo | Directivas | Plantillas administrativas | Componentes de Windows | Windows Update | Configurar las actualizaciones automáticas**  
  
-   **Configuración del equipo | Directivas | Plantillas administrativas | Componentes de Windows | Windows Update | No reiniciar automáticamente con una sesión a los usuarios**  
  
-   **Configuración del equipo | Directivas | Plantillas administrativas | Componentes de Windows | Programador de mantenimiento | Retraso aleatorio de mantenimiento**  
  
La siguiente tabla enumeran algunos ejemplos de cómo configurar estas opciones de configuración para proporcionar un comportamiento de reinicio deseado.  
  
|||  
|-|-|  
|**Escenario**|**Configuraciones recomendadas**|  
|**WSUS administradas**<br /><br />-Instalar las actualizaciones de una vez por semana<br />-Reiniciar los viernes a las 11 P.M.|Configurar equipos para instalar automáticamente, evitar el reinicio automático hasta el tiempo deseado<br /><br />**Directiva**: configurar las actualizaciones automáticas (activadas)<br /><br />Configurar las actualizaciones automáticas: 4 - Descargar automáticamente y programar la instalación<br /><br />**Directiva**: No reiniciar automáticamente con los usuarios conectados (deshabilita)<br /><br />**Los plazos WSUS**: establecido en los viernes a las 11 P.M.|  
|**WSUS administradas**<br /><br />-El escalonamiento instala a través de distintas horas o días|Establecer grupos de destino para diferentes grupos de equipos que deben actualizarse juntos<br /><br />Usar por encima de los pasos para el escenario anterior<br /><br />Establecer plazos diferentes para distintos grupos de destino|  
|**No administrados para WSUS - plazos no es compatible con**<br /><br />-El escalonamiento instala en distintos momentos|**Directiva**: configurar las actualizaciones automáticas (activadas)<br /><br />Configurar las actualizaciones automáticas: 4 - Descargar automáticamente y programar la instalación<br /><br />**Clave del registro:** habilitar la clave del registro que se describen en el artículo de Microsoft KB [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**Directiva:** retraso aleatorio mantenimiento automático (activado)<br /><br />Establecer **retraso aleatorio de mantenimiento Regular** a PT6H 6 horas aleatorio retrasos para proporcionar el siguiente comportamiento:<br /><br />-Las actualizaciones se instalarán en el tiempo configurado mantenimiento más un retraso aleatorio<br /><br />-Para cada equipo llevará a cabo exactamente 3 días después de reiniciar<br /><br />Como alternativa, establecer un tiempo de mantenimiento diferentes para cada grupo de equipos|  
  
Para obtener más información acerca de por qué el equipo de ingeniería de Windows implementa estos cambios, vea [minimizar el equipo se reinicia tras la actualización automática en Windows Update](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).  
  
## <a name="BKMK_InstallationChanges"></a>Cambios de instalación del rol de servidor de AD DS  
En Windows Server 2003 a través de Windows Server 2008 R2, ejecutó la x86 o X64 versión de la herramienta de línea de comandos Adprep.exe antes de ejecutar el Asistente para instalación de Active Directory, Dcpromo.exe y Dcpromo.exe tenía variantes opcionales para instalar desde medios o para instalación desatendida.  
  
A partir de Windows Server 2012, se realizan las instalaciones de línea de comandos con el módulo de ADDSDeployment en Windows PowerShell. Promociones de interfaz gráfica de usuario se realizan en el administrador del servidor mediante un completamente nuevo Asistente de configuración de AD DS. Para simplificar el proceso de instalación, ADPREP se ha integrado en la instalación de AD DS y se ejecuta automáticamente según sea necesario. El Asistente de configuración basado en Windows PowerShell AD DS se destina automáticamente a las funciones de maestro de esquema e infraestructura en los dominios donde se agregan controladores de dominio y luego remotamente ejecuta los comandos ADPREP requiere la relevante en controladores de dominio.  
  
Comprobaciones de requisitos previos en el Asistente para la instalación de AD DS identifican posibles errores antes de que comience la instalación. Condiciones de error pueden corregirse para elimine los problemas de una actualización parcialmente completada. El asistente también exporta un script de PowerShell de Windows que contiene todas las opciones que se especificaron durante la instalación gráfica.  
  
Todas juntas, los cambios de instalación de AD DS simplifican el proceso de instalación de la función de controlador de dominio y reducen la probabilidad de errores administrativos, sobre todo cuando estás implementando varios controladores de dominio en regiones globales y dominios.  
Obtener más información sobre la interfaz gráfica de usuario e instalaciones basadas en Windows PowerShell, incluida la sintaxis de línea de comandos y las instrucciones del asistente paso a paso, consulta [instalar servicios de dominio de Active Directory](https://technet.microsoft.com/library/hh472162.aspx). Para los administradores que deseen controlar la introducción de cambios de esquema en un bosque de Active Directory independiente de la instalación de controladores de dominio de Windows Server 2012 en un bosque existente, comandos de Adprep.exe aún se pueden ejecutar en un símbolo del sistema con privilegios elevados.  
  
## <a name="BKMK_DeprecatedFeatures"></a>Características en desuso y cambios de comportamiento relacionados a AD DS en Windows Server 2012  
Hay algunos cambios relacionados con AD DS:  
  
-   **Desuso de Adprep32.exe**  
  
    Hay solo una versión de Adprep.exe y se puede ejecutar según sea necesario en los servidores de 64 bits que ejecutan Windows Server 2008 o posterior. Puede ejecutarse de forma remota y debe ejecutarse de forma remota si se hospeda el rol de maestro de operaciones en un sistema operativo de 32 bits o Windows Server 2003 de destino.  
  
-   **Desuso de Dcpromo.exe**  
  
    Está en desuso Dcpromo aunque en Windows Server 2012 solo se puede seguir ejecutar con parámetros de línea de comandos para dar tiempo a las organizaciones para realizar la transición automatización existente para las nuevas opciones de instalación de Windows PowerShell o un archivo de respuesta.  
  
-   **LMHash está deshabilitada en cuentas de usuario**  
  
    Proteger los valores predeterminados de seguridad plantillas en Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012 habilita la directiva de NoLMHash que está deshabilitada en las plantillas de seguridad de los controladores de dominio de Windows Server 2003 y Windows 2000. Deshabilita la directiva NoLMHash para los clientes dependientes LMHash según sea necesario, siguiendo los pasos del artículo de Knowledge Base [946405](https://support.microsoft.com/kb/946405).  
  
Controladores de dominio a partir de Windows Server 2008, también tienen la siguiente configuración segura de forma predeterminada, en comparación con los controladores de dominio que ejecutan Windows Server 2003 o Windows 2000.  
  
|||||  
|-|-|-|-|  
|Tipo de cifrado o la directiva|Valor predeterminado de Windows Server 2008|Valor predeterminado de Windows Server 2012 y Windows Server 2008 R2|Comentario|  
|AllowNT4Crypto|Deshabilitado|Deshabilitado|Clientes de bloque de mensajes de servidor (SMB) de terceros pueden ser compatibles con la configuración de seguridad predeterminada en controladores de dominio. En todos los casos, estas opciones de configuración pueden ser más flexible para permitir la interoperabilidad, pero solo a costa de seguridad. Para obtener más información, consulta [artículo 942564](https://go.microsoft.com/fwlink/?LinkId=164558) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Habilitado|Deshabilitado|[Artículo 977321](https://go.microsoft.com/fwlink/?LinkId=177717) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|Protección mediante un programa Didáctico/ampliada para la autenticación integrada|N/D|Habilitado|Consulta [aviso de seguridad de Microsoft (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) y [artículo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Revisar e instalar la revisión de [artículo 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) en Microsoft Knowledge Base según sea necesario.|  
|LMv2|Habilitado|Deshabilitado|[Artículo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=178251)|  
  
## <a name="BKMK_SysReqs"></a>Requisitos del sistema operativo  
En la siguiente tabla, se enumeran los requisitos mínimos del sistema para Windows Server 2012. Para obtener más información sobre los requisitos del sistema y la información de preinstalación, consulta [instalar Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). No existen requisitos adicionales del sistema para instalar un nuevo bosque de Active Directory, pero debes agregar memoria suficiente para almacenar en caché el contenido de la base de datos de Active Directory con el fin de mejorar el rendimiento de los controladores de dominio, las solicitudes de cliente LDAP y aplicaciones habilitadas para Active Directory. Si vas a actualizar un controlador de dominio existente o agregar un nuevo controlador de dominio a un bosque existente, revisa la siguiente sección para asegurarte de que el servidor cumple los requisitos de espacio en disco.  
  
|||  
|-|-|  
|Procesador|1,4 procesador de Ghz de 64 bits|  
|RAM|512 MB|  
|Requisitos de espacio libre en disco|32 GB|  
|Resolución de pantalla|800 x 600 o superior|  
|Varios|Unidad de DVD, el teclado, el acceso a Internet|  
  
### <a name="BKMK_DiskSpaceDCWin8"></a>Requisitos de espacio en disco para actualizar los controladores de dominio  
Esta sección describe requisitos de espacio en disco solo para actualizar los controladores de dominio de Windows Server 2008 o Windows Server 2008 R2. Para obtener más información sobre los requisitos de espacio en disco para actualizar los controladores de dominio a versiones anteriores de Windows Server, consulta [requisitos de espacio para la actualización a Windows Server 2008 en disco](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) o [requisitos de espacio para la actualización a Windows Server 2008 R2 disco](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
El tamaño del disco que hospeda los archivos de base de datos y el registro de Active Directory para adaptarse a las extensiones de esquema personalizados y controlados por la aplicación, aplicaciones e índices iniciadas por el administrador, además de espacio para los objetos y atributos que agregará al directorio sobre la duración de la implementación del controlador de dominio (normalmente 5 a 8 años). Normalmente es una buena inversión en comparación con los costos de mayor táctil necesarios para expandir el almacenamiento en el disco después de la implementación directamente de tamaño en tiempo de implementación. Para obtener más información, consulta [planeamiento de capacidad para los servicios de dominio de Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
En los controladores de dominio que vas a actualizar, asegúrate de que la unidad que hospeda el Active Directory base de datos (NTDS. DIT) tiene espacio libre en disco que representa al menos un 20% del tamaño del archivo NTDS. Archivo DIT antes de comenzar la actualización del sistema operativo. Si no hay suficiente espacio libre en el volumen, puede haber un error en la actualización y el informe de compatibilidad de actualización devuelve un error que indica que no hay suficiente espacio libre:  
  
En este caso, puede intentar una desfragmentación sin conexión de la base de datos de Active Directory recuperar espacio adicional y vuelva a intentar la actualización. Para obtener más información, consulta [compactar el archivo de base de datos del directorio (sin conexión desfragmentación)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>SKU disponibles  
Existen 4 ediciones de servidor de Windows: Foundation, Essentials, Standard y Datacenter.   
Las dos ediciones que admiten el rol de AD DS son Standard y Datacenter.  
  
En versiones anteriores, las ediciones de Windows Server difieren en su compatibilidad con los roles de servidor, los recuentos de procesador y soporte técnico de grandes cantidades de memoria. Las ediciones Standard y Datacenter de Windows Server admiten todas las características y el hardware subyacente pero varían en sus derechos de virtualización - dos instancias virtuales se permiten para Standard edition y un número ilimitados instancias virtuales se permiten para Datacenter edition.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Cliente de Windows y sistemas operativos Windows Server que se admiten para unirse a dominios de Windows Server  
El siguiente cliente de Windows y sistemas operativos Windows Server se admite para los equipos miembros del dominio con controladores de dominio que ejecutan Windows Server 2012 o posterior:  
  
-   Sistemas operativos: Windows 8.1, Windows 8, Windows 7, Windows Vista 
  
    Los equipos que ejecutan Windows 8.1 o Windows 8 también son capaces de unirse a dominios que tienen controladores de dominio esa ejecución versión anterior de Windows Server, incluidos Windows Server 2003 o posterior. En este caso, sin embargo, algunas características de Windows 8 pueden requerir una configuración adicional o no estén disponibles. Para obtener más información sobre las características y otras recomendaciones para la administración de clientes de Windows 8 en dominios de nivel inferior, consulta [equipos miembro de ejecutar Windows 8 en dominios de Windows Server 2003](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
  
-   Sistemas operativos de servidor: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003  
  
## <a name="BKMK_UpgradePaths"></a>En lugar de actualización admitidas  
Controladores de dominio que ejecutan versiones de 64 bits de Windows Server 2008 o Windows Server 2008 R2 se pueden actualizar a Windows Server 2012. No se puede actualizar los controladores de dominio que ejecutan Windows Server 2003 o versiones de 32 bits de Windows Server 2008. Reemplazarlos, instalar controladores de dominio que ejecuten una versión posterior de Windows Server en el dominio y, a continuación, quita los controladores de dominio de Windows Server 2003.  
  
|Si ejecutas estas ediciones|Puedes actualizar a estas ediciones|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard con SP2<br /><br />OR<br /><br />Windows Server 2008 Enterprise con SP2|Estándar de Windows Server 2012<br /><br />OR<br /><br />Windows Server 2012 R2 Datacenter|  
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 R2 Datacenter|  
|Web de Windows Server 2008|Estándar de Windows Server 2012|  
|Estándar de Windows Server 2008 R2 con SP1<br /><br />OR<br /><br />Windows Server 2008 R2 Enterprise con SP1|Estándar de Windows Server 2012<br /><br />OR<br /><br />Windows Server 2012 R2 Datacenter|  
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 R2 Datacenter|  
|Web de Windows Server 2008 R2|Estándar de Windows Server 2012|  
  
Para obtener más información sobre las rutas de actualización admitidas, consulta [versiones de evaluación y actualizar opciones para Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Ten en cuenta que no se puede convertir un controlador de dominio que ejecute una versión de evaluación de Windows Server 2012 directamente a una versión comercial. En su lugar, instala un controlador de dominio en un servidor que ejecute una versión comercial y quita el controlador de dominio que se ejecuta en la versión de evaluación en AD DS.  
  
Debido a un problema conocido, no se puede actualizar un controlador de dominio que se ejecuta una instalación Server Core de Windows Server 2008 R2 para una instalación Server Core de Windows Server 2012. La actualización se bloqueará en una pantalla en negro sólida tarde en el proceso de actualización. Reiniciar estos controladores de dominio, expone una opción en el archivo boot.ini revertir a la versión anterior del sistema operativo. Un reinicio adicional desencadena la reversión automática a la versión anterior del sistema operativo. Hasta que haya una solución disponible, se recomienda que instale un nuevo controlador de dominio que ejecutan una instalación Server Core de Windows Server 2012, en lugar de actualizar un controlador de dominio que se ejecuta una instalación Server Core de Windows Server 2008 R2 en contexto. Para obtener más información, consulte el artículo [2734222](https://support.microsoft.com/kb/2734222).  
  
## <a name="BKMK_FunctionalLevels"></a>Requisitos y características de nivel funcionales  
 Windows Server 2012 se requiere un nivel funcional del bosque de Windows Server 2003. Es decir, antes de poder agregar un controlador de dominio que ejecute Windows Server 2012 en un bosque de Active Directory existente, el nivel funcional del bosque debe ser Windows Server 2003 o superior. Esto significa que los controladores de dominio que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 pueden funcionar en el mismo bosque, pero los controladores de dominio que ejecutan Windows 2000 Server no se admite y bloqueará la instalación de un controlador de dominio que ejecute Windows Server 2012. Si el bosque contiene controladores de dominio que ejecutan Windows Server 2003 o posterior pero el funcional del bosque nivel sigue siendo Windows 2000, también se bloquea la instalación.  
  
Controladores de dominio de Windows 2000 deben quitarse antes de agregar controladores de dominio de Windows Server 2012 en el bosque. En este caso, ten en cuenta el flujo de trabajo siguiente:  
  
1.  Instalar controladores de dominio que ejecutan Windows Server 2003 o posterior. Estos controladores de dominio se pueden implementar en una versión de evaluación de Windows Server. Este paso también requiere [ejecutando adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) para ese sistema operativo liberar como requisito previo.  
  
2.  Quita los controladores de dominio de Windows 2000. Específicamente, correctamente degradar o forzar la eliminación de controladores de dominio de Windows Server 2000 del dominio y usadas usuarios de Active Directory y equipos para quitar las cuentas de controlador de dominio para todos los controladores de dominio eliminado.  
  
3.  Aumentar el nivel funcional del bosque en Windows Server 2003 o superior.  
  
4.  Instalar controladores de dominio que ejecutan Windows Server 2012.  
  
5.  Quitar controladores de dominio que ejecutan versiones anteriores de Windows Server.  
  
El nuevo nivel funcional de dominio de Windows Server 2012 permite una nueva característica: el **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos** directiva de plantilla administrativa de KDC tiene dos valores (**siempre proporcionar notificaciones** y **producirá un error en las solicitudes de autenticación unarmored**) que requiere nivel funcional del dominio de Windows Server 2012.  
  
El nivel funcional del bosque de Windows Server 2012 no proporciona ninguna característica nueva, pero garantiza que cualquier nuevo dominio creado en el bosque funcionará automáticamente en el nivel funcional de dominio de Windows Server 2012. El nivel funcional de dominio de Windows Server 2012 no proporciona otras características nuevas más allá de la compatibilidad KDC para notificaciones, autenticación compuesta y protección de Kerberos. Pero garantiza que cualquier controlador de dominio en el dominio ejecuta Windows Server 2012. Para obtener más información sobre otras características que están disponibles en diferentes niveles funcionales, consulta [niveles funcionales de conocimiento Active Directory Domain Services (AD DS)](../active-directory-functional-levels.md).  
  
Después de establecer el nivel funcional del bosque en un valor determinado, no se puede revertir o reducir el nivel funcional del bosque, con las siguientes excepciones: después de aumentar el nivel funcional del bosque a Windows Server 2012, se puede reducir a Windows Server 2008 R2. Si no se ha habilitado la Papelera de reciclaje de Active Directory, también puede reducir el nivel funcional de Windows Server 2012 para Windows Server 2008 R2 o Windows Server 2008 o Windows Server 2008 R2 del bosque a Windows Server 2008. Si se establece el nivel funcional del bosque a Windows Server 2008 R2, no se puede deshacer, por ejemplo, Windows Server 2003.  
  
Después de establecer el nivel funcional del dominio en un valor determinado, no puede revertir ni bajar el nivel funcional de dominio, con las siguientes excepciones: cuando eleva el nivel funcional de dominio a Windows Server 2008 R2 o Windows Server 2012, y si el nivel funcional del bosque es Windows Server 2008 o inferior, tienes la opción de revertir el nivel funcional del dominio vuelve a Windows Server 2008 o Windows Server 2008 R2. Puede reducir el nivel funcional del dominio solo de Windows Server 2012 para Windows Server 2008 R2 o Windows Server 2008 o Windows Server 2008 R2, Windows Server 2008. Si se establece el nivel funcional del dominio a Windows Server 2008 R2, no se puede deshacer, por ejemplo, Windows Server 2003.  
  
Para obtener más información sobre las características que están disponibles en los niveles inferiores de funcionales, consulta [niveles funcionales de conocimiento Active Directory Domain Services (AD DS)](../active-directory-functional-levels.md).  
  
Más allá de los niveles funcionales, un controlador de dominio que ejecute Windows Server 2012 proporciona características adicionales que no están disponibles en un controlador de dominio que ejecute una versión anterior de Windows Server. Por ejemplo, un controlador de dominio que ejecute Windows Server 2012 puede usarse para clonación del controlador de dominio virtual, mientras que un controlador de dominio que ejecute una versión anterior de Windows Server no. Pero no tienen requisitos de nivel funcionales clonación del controlador de dominio virtual y protecciones de controlador de dominio virtual en Windows Server 2012.  
  
> [!NOTE]  
> Microsoft Exchange Server 2013 requiere un nivel funcional del bosque de Windows server 2003 o superior.  
  
## <a name="BKMK_ServerRoles"></a>Interoperabilidad de AD DS con otros roles de servidor y sistemas operativos Windows  
AD DS no se admite en los siguientes sistemas operativos de Windows:  
  
-   Windows MultiPoint Server  
  
-   Windows Server 2012 Essentials  
  
No se puede instalar AD DS en un servidor que también se ejecuta los siguientes roles de servidor o los servicios de rol:  
  
-   Hyper-V Server  
  
-   Agente de conexión a Escritorio remoto  
  
## <a name="BKMK_OpsMasters"></a>Roles de maestro de operaciones  
Algunas características nuevas en Windows Server 2012 afectan a los roles de maestro de operaciones:  
  
-   El emulador PDC debe ejecutar Windows Server 2012 para admitir la clonación controladores de dominio virtual. Existen requisitos previos adicionales para clonar controladores de dominio. Para obtener más información, consulta [virtualización de los servicios de dominio de Active Directory (AD DS)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Cuando el emulador ejecuta Windows Server 2012, se crean nuevos principales de seguridad.  
  
-   El patrón LIBRARSE tiene nuevas emisión RID y la funcionalidad de supervisión. Las mejoras incluyen un mejor registro de eventos, límites más adecuados, y la capacidad de - de emergencia - aumentar el RID general grupo asignación en un bit. Para obtener más información, consulta [administrar emisión LIBRARSE](../../ad-ds/manage/Managing-RID-Issuance.md).  
  
> [!NOTE]  
> Aunque no sean los roles de maestro de operaciones, otro cambio en la instalación de AD DS es que el rol de servidor DNS y el catálogo global se instalan de forma predeterminada en todos los controladores de dominio que ejecutan Windows Server 2012.  
  
## <a name="BKMK_Virtual"></a>Controladores de dominio de virtualización  
Mejoras en el principio de AD DS en Windows Server 2012 habilitar la virtualización más segura de controladores de dominio y la capacidad para clonar controladores de dominio. Clonación controladores de dominio a su vez permite la implementación rápida de controladores de dominio adicionales en un dominio y otros beneficios. Para obtener más información, consulta [Introducción a los servicios de dominio de Active Directory & #40; AD DS & #41; Virtualización & #40; Nivel 100 & #41; ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
## <a name="BKMK_Admin"></a>Administración de los servidores de Windows Server 2012  
Usa el [remoto Server Administration Tools para Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) administrar controladores de dominio y otros servidores que ejecutan Windows Server 2012. Puedes ejecutar Windows Server 2012 Remote Server Administration Tools en un equipo que ejecute Windows 8.  
  
## <a name="BKMK_AppCompat"></a>Compatibilidad de aplicaciones  
En la siguiente tabla describe aplicaciones comunes de Microsoft integradas en Active Directory. La tabla muestra qué versiones de Windows Server que se pueden instalar en las aplicaciones y si la introducción de controladores de dominio de Windows Server 2012 afecta a la compatibilidad de la aplicación.  
  
|Producto|Notas|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2007 con Service Pack 2 (incluye Configuration Manager 2007 R2 y Configuration Manager 2007 R3):<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 estándar<br />-Windows Server 2012 R2 Datacenter **Nota:** aunque serán totalmente compatibles como clientes, no hay ningún plan para agregar compatibilidad para la implementación de estos elementos como sistemas operativos mediante la característica de implementación de sistema operativo de Configuration Manager 2007. Además, no servidores de sitio o sistemas de sitio se admitirán en las SKU de Windows Server 2012.|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Microsoft Office SharePoint Server 2007 no se admite para la instalación en Windows Server 2012.|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2 es necesaria para instalar y utilizar <br />SharePoint 2010 en servidores de Windows Server 2012<br /><br />SharePoint 2010 Foundation Service Pack 2 es necesaria para instalar y utilizar SharePoint 2010 Foundation en servidores de Windows Server 2012<br /><br />Se produce un error en el proceso de instalación de SharePoint Server 2010 (sin service packs) en Windows Server 2012<br /><br />El instalador de requisitos previo de SharePoint Server 2010 (PrerequisiteInstaller.exe) se produce el error "este programa tiene problemas de compatibilidad". Haz clic en "Ejecutar el programa sin obtener ayuda" muestra el error "comprobar si se puede instalar SharePoint & #124; SharePoint Server 2010 (sin service packs) no se puede instalar en Windows Server 2012."|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Requisitos mínimos de un servidor de bases de datos en una granja de servidores<br /><br />La edición de 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) estándar, Enterprise, Datacenter o la edición de 64 bits de Windows Server 2012 Standard o Datacenter<br /><br />Requisitos mínimos para un único servidor con la base de datos integrada:<br /><br />La edición de 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) estándar, Enterprise, Datacenter o la edición de 64 bits de Windows Server 2012 Standard o Datacenter<br /><br />Requisitos mínimos para servidores web front-end y aplicaciones en un conjunto de:<br /><br />La edición de 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) estándar, Enterprise, Datacenter o la edición de 64 bits de Windows Server 2012 Standard o Datacenter.|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1:<br /><br />Microsoft agregará los siguientes sistemas operativos a nuestra matriz de compatibilidad de cliente con el lanzamiento de Service Pack 1:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 estándar<br />-Windows Server 2012 R2 Datacenter<br /><br />Todos los roles de servidor (incluidos con los servidores del sitio, los proveedores SMS y los puntos de administración) se puede implementar en los servidores con las siguientes ediciones del sistema operativo:<br /><br />-Windows Server 2012 estándar<br />-Windows Server 2012 R2 Datacenter|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 se requiere con Windows Server 2008 R2 o Windows Server 2012. No se puede ejecutar en una instalación Server Core. Se puede ejecutar [los servidores virtuales](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 puede instalarse en una nueva instalación (no una actualización), Windows Server 2012 si [actualizaciones acumulativas de octubre de 2012 para Lync Server](https://support.microsoft.com/?kbid=2493736) están instalados. No se admite la actualización del sistema operativo a Windows Server 2012 para una instalación existente de Lync Server 2010. También, servidor de Chat de grupo de Microsoft Lync Server 2010 no se admite en Windows Server 2012.|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint protección Service Pack 1 se actualizará la matriz de soporte de cliente para incluir los siguientes sistemas operativos<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 estándar<br />-Windows Server 2012 R2 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|2010 FEP con Update 1 del paquete acumulativo de actualizaciones se actualizará la matriz de soporte de cliente para incluir los siguientes sistemas operativos:<br /><br />-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 estándar<br />-Windows Server 2012 R2 Datacenter|  
|Forefront Threat Management Gateway (TMG)|TMG se admite para ejecutarse solo en Windows Server 2008 y Windows Server 2008 R2. Para obtener más información, consulta [requisitos del sistema para Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|Windows Server Update Services|Esta versión de WSUS ya admite equipos basados en Windows 8 o equipos basados en Windows Server 2012 como clientes.|  
|Windows Server Update Services 3.0|Artículo de actualización de KB [2734608](https://support.microsoft.com/kb/2734608) permite que los servidores que ejecutan Windows Server Update Services (WSUS) 3.0 SP2 proporcionan actualizaciones para equipos que ejecutan Windows 8 o Windows Server 2012: **Nota:** los clientes con entornos independientes WSUS 3.0 SP2 o en entornos de System Center Configuration Manager 2007 Service Pack 2 con WSUS 3.0 SP2 necesitan [2734608](https://support.microsoft.com/kb/2734608) para administrar correctamente los equipos basados en Windows 8 o equipos basados en Windows Server 2012 como clientes.|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 Standard y Datacenter son compatibles con las siguientes funciones: maestro de esquema, el servidor de catálogo global, el controlador de dominio, el rol de servidor de acceso de cliente y el buzón<br /><br />Nivel funcional del bosque: Windows Server 2003 o superior<br /><br />Origen: Requisitos del sistema Exchange 2013|  
|Exchange 2010|[Fuente: Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />Exchange 2010 con Service Pack 3 puede instalarse en los servidores miembro de Windows Server 2012.<br /><br />[Requisitos del sistema de Exchange 2010](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) muestra el más reciente esquema admitido maestro, global catálogo y dominio controlador como Windows Server 2008 R2.<br /><br />Nivel funcional del bosque: Windows Server 2003 o superior|  
|SQL Server 2012|Fuente: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />SQL Server 2012 RTM se admite en Windows Server 2012.|  
|SQL Server 2008 R2|Fuente: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requiere SQL Server 2008 R2 con Service Pack 1 o posterior para instalar en Windows Server 2012.|  
|SQL Server 2008|Fuente: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requiere SQL Server 2008 con Service Pack 3 o posterior para instalar en Windows Server 2012.|  
|SQL Server 2005|Fuente: KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />No se admite para instalar en Windows Server 2012.|  
  
## <a name="BKMK_KnownIssues"></a>Problemas conocidos  
La siguiente tabla enumera los problemas conocidos relacionados con la instalación de AD DS.  
  
||||  
|-|-|-|  
|Título y el número de artículo KB|Área de tecnología que se ven afectado|Descripción del problema|  
|[2830145](https://support.microsoft.com/kb/2830145): SID S-1-18-1 y SID S-1-18-2 no se pueden asignar en equipos basados en Windows Server 2008 R2 en un entorno de dominio o de Windows 7|Compatibilidad de AD DS o aplicación de administración|Las aplicaciones que se asignan SID S-1-18-1 y SID S-1-18-2, que son nuevos en Windows Server 2012, pueden presentar errores porque no se puede resolver el SID en equipos basados en Windows 7 o Windows Server 2008 R2. Para resolver este problema, instala la revisión en los equipos basados en Windows Server 2008 R2 y Windows 7 en el dominio.|  
|[2737129](https://support.microsoft.com/kb/2737129): preparación para la directiva de grupo no se realiza al preparar automáticamente un dominio existente para Windows Server 2012|Instalación de AD DS|Adprep /domainprep /gpprep no se ejecuta automáticamente como parte de la instalación en el primer controlador de dominio que ejecuta Windows Server 2012 en un dominio. Si se nunca se ejecutó previamente en el dominio, se debe ejecutar manualmente.|  
|[2737416](https://support.microsoft.com/kb/2737416): implementación de controlador de dominio basados en PowerShell de Windows repite advertencias|Instalación de AD DS|Advertencias pueden aparecer durante la validación de requisitos previo y, a continuación, vuelve a aparecer durante la instalación.|  
|[2737424](https://support.microsoft.com/kb/2737424): "El formato de nombre de dominio especificado no es válido" error al tratar de quitar los servicios de dominio de Active Directory de un controlador de dominio|Instalación de AD DS|Este error aparece cuando se quita el último controlador de dominio en un dominio donde previamente RODC las cuentas siguen existan. Esto afecta a Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): no se inicia el controlador de dominio, se produce error c00002e2, o "Elegir una opción" se muestra|Instalación de AD DS|Un controlador de dominio no se inicia debido a que un administrador usa Dism.exe, Pkgmgr.exe o Ocsetup.exe para quitar la función DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): limitaciones de verificación IFM en el administrador del servidor de Windows Server 2012|Instalación de AD DS|Verificación IFM puede tener limitaciones como se explica en el artículo de KB.|  
|[2737535](https://support.microsoft.com/kb/2737535): instalar AddsDomainController cmdlet devuelve parámetro establecido error para RODC|Instalación de AD DS|Puede recibir un error al intentar adjuntar un servidor a una cuenta de RODC si especifica argumentos que se rellenan en la cuenta de RODC creada previamente.|  
|[2737560](https://support.microsoft.com/kb/2737560): "No se pudo realizar la comprobación de conflicto de esquema de Exchange" falla de comprobación de errores y requisitos previos|Instalación de AD DS|Comprobar los requisitos previos se produce un error al configurar el controlador de dominio de primer Windows Server 2012 en un dominio existente porque los controladores de dominio faltan la SeServiceLogonRight para el servicio de red o porque se bloquean los protocolos WMI o DCOM.|  
|[2737797](https://support.microsoft.com/kb/2737797): módulo de AddsDeployment con el argumento - Whatif muestra resultados incorrectos de DNS|Instalación de AD DS|El parámetro - WhatIf muestra DNS de servidor no se instalará, pero será.|  
|[2737807](https://support.microsoft.com/kb/2737807): botón siguiente no está disponible en la página Opciones de controlador de dominio|Instalación de AD DS|El botón siguiente está deshabilitado en la página Opciones de controlador de dominio, porque la dirección IP del controlador de dominio de destino no se asigna a un sitio o una subred existente, o porque la contraseña DSRM no se escribe y confirma correctamente.|  
|[2737935](https://support.microsoft.com/kb/2737935): instalación de active Directory se detiene en la fase de "Crear el objeto de configuración NTDS"|Instalación de AD DS|Porque la contraseña de administrador local coincide con la contraseña de administrador de dominio o porque problemas de red impiden la replicación crítica de finalización, se bloquea la instalación.|  
|[2738060](https://support.microsoft.com/kb/2738060): "Acceso denegado" mensaje de error cuando se crea un dominio secundario remotamente mediante la instalación AddsDomain|Instalación de AD DS|Recibes el error al ejecutar ADDSDomain de instalación con el cmdlet de invocar comandos si la DNSDelegationCredential tiene una contraseña incorrecta.|  
|[2738697](https://support.microsoft.com/kb/2738697): "el servidor no está operativo" dominio controlador error de configuración cuando se configura un servidor mediante el administrador del servidor|Instalación de AD DS|Recibes este error al intentar instalar AD DS en un equipo de grupo de trabajo porque está deshabilitada la autenticación NTLM.|  
|[2738746](https://support.microsoft.com/kb/2738746): recibes errores de acceso denegado después de iniciar sesión en una cuenta de dominio de administrador local|Instalación de AD DS|Cuando inicia sesión con una cuenta de administrador local en lugar de la cuenta predefinida de administrador y, a continuación, crear un nuevo dominio, la cuenta no se agrega al grupo Administradores de dominio.|  
|[2743345](https://support.microsoft.com/kb/2743345): "el sistema no encuentra el archivo especificado" Adprep /gpprep errores o bloqueos de la herramienta|Instalación de AD DS|Recibes este error al ejecutar adprep /gpprep porque el maestro de infraestructura es implementa un espacio de nombres discontinuo|  
|[2743367](https://support.microsoft.com/kb/2743367): Adprep "no una aplicación de Win32 válida" error en la versión de 64 bits de Windows Server 2003|Instalación de AD DS|Recibe este error porque Windows Server 2012 Adprep no se puede ejecutar en Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): ADMT 3.2 y archivos PE 3.1 errores de instalación en Windows Server 2012|ADMT|ADMT 3.2 no se puede instalar en Windows Server 2012 por diseño.|  
|[2750857](https://support.microsoft.com/kb/2750857): los informes de diagnósticos no se muestren correctamente en Internet Explorer 10 de replicación de DFS|Replicación de DFS|Informe de diagnóstico de replicación DFS no se muestre correctamente debido a cambios en Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): actualizaciones de la directiva de grupo remotos son visibles para los usuarios|Directiva de grupo|Esto es debido a las tareas programadas ejecutarse en el contexto de cada usuario que inicie sesión. El diseño de programador de tareas de Windows requiere un símbolo del sistema interactiva en este escenario.|  
|[2741591](https://support.microsoft.com/kb/2741591): archivos ADMX no están presentes en SYSVOL en la opción de estado de la infraestructura de GPMC|Directiva de grupo|Replicación GP puede notificar "replicación en curso" porque el estado de la infraestructura de GPMC no sigue las reglas de filtrado personalizadas.|  
|[2737880](https://support.microsoft.com/kb/2737880): "no se puede iniciar el servicio" error durante la configuración de AD DS|Clonación virtual de DC|Recibe este error durante la instalación o desinstalación de AD DS o clonación, porque el servicio de rol servidor de DS está deshabilitado.|  
|[2742836](https://support.microsoft.com/kb/2742836): dos concesiones DHCP se crean para cada controlador de dominio cuando usas el v CC clonación característica|Clonación virtual de DC|Esto ocurre porque el controlador de dominio clonados había recibido una concesión antes de clonar y otra vez en cuando clonación finalizó.|  
|[2742844](https://support.microsoft.com/kb/2742844): clonación produce un error y el servidor de controlador de dominio se reinicia en DSRM en Windows Server 2012|Clonación virtual de DC|El controlador de dominio clonado se inicia en DSRM porque clonación error para una variedad de razones que se indican en el artículo KB.|  
|[2742874](https://support.microsoft.com/kb/2742874): clonación del controlador de dominio no volver a crear todos los nombres de entidad de seguridad de servicio|Clonación virtual de DC|Algunos SPN de tres partes no se vuelven a crear en el DC clonado debido a una limitación del proceso de cambio de nombre del dominio.|  
|[2742908](https://support.microsoft.com/kb/2742908): "ningún servidor está disponibles" error después de la clonación el controlador de dominio|Clonación virtual de DC|Recibes este error al intentar iniciar sesión después de clonación un controlador de dominio virtualizada porque clonación error y se inicia el controlador de dominio en DSRM. Iniciar sesión como. \administrator a solucionar el error clonación.|  
|[2742916](https://support.microsoft.com/kb/2742916): clonación del controlador de dominio se produce el error 8610 en dcpromo.log|Clonación virtual de DC|Clonación se produce un error porque el emulador PDC no ha realizado la replicación de entrada de la partición de dominio, es probable que ya se ha transferido a la función.|  
|[2742927](https://support.microsoft.com/kb/2742927): "El índice está fuera del intervalo" error de nuevo AdDcCloneConfig|Clonación virtual de DC|Recibes el error después de ejecutar el cmdlet New-ADDCCloneConfigFile durante la clonación virtuales controladores de dominio, porque no se ejecutó el cmdlet desde un símbolo del sistema con privilegios elevados o porque el token de acceso no contiene el grupo de administradores.|  
|[2742959](https://support.microsoft.com/kb/2742959): clonación del controlador de dominio se produce un error con error 8437: "se ha especificado un parámetro no válido para esta operación de replicación"|Clonación virtual de DC|No se pudo realizar la clonación porque se ha especificado un nombre no válido clone o un nombre NetBIOS duplicado.|  
|[2742970](https://support.microsoft.com/kb/2742970): clonación del controlador de dominio se produce un error con ningún DSRM, equipo de origen y clone duplicados|Clonación virtual de DC|El controlador de dominio virtual clonado arranca en servicios de directorio reparación modo (DSRM), con un nombre duplicado como el origen de controlador de dominio porque no se creó el archivo DCCloneConfig.xml en la ubicación correcta o porque se reinició el DC de origen antes de clonación.|  
|[2743278](https://support.microsoft.com/kb/2743278): clonación error 0x80041005 el controlador de dominio|Clonación virtual de DC|El controlador de dominio clonado arranca en DSRM porque se especificó un solo servidor WINS. Si se especifica ningún servidor WINS, deben especificarse servidores preferidas y WINS alternativo.|  
|[2745013](https://support.microsoft.com/kb/2745013): aparece el mensaje de error "El servidor no está operativo" Si ejecutas AdDcCloneConfigFile de nuevo en Windows Server 2012|Clonación virtual de DC|Recibe este error después de ejecutar el cmdlet New-ADDCCloneConfigFile porque el servidor no puede ponerse en contacto con el servidor de catálogo global.|  
|[2747974](https://support.microsoft.com/kb/2747974): controlador de dominio clonación evento 2224 proporciona orientación incorrecta|Clonación virtual de DC|Identificador de evento 2224 afirma erróneamente que deben quitar cuentas de servicio administradas antes clonación. Debe quitarse independiente MSA pero MSA de grupo no bloquees la clonación.|  
|[2748266](https://support.microsoft.com/kb/2748266): no se puede desbloquear una unidad cifrada con BitLocker después de actualizar a Windows 8|BitLocker|Recibes un error "No se encuentra la aplicación" al intentar desbloquear una unidad en un equipo que se actualizó desde Windows 7.|  
  
## <a name="see-also"></a>Consulta también  
[Recursos de evaluación de Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guía de evaluación de Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Instalar e implementar Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
  


