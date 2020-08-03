---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f211acd5e93f3f4654983e2c61d6b1a460415655
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519383"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona información general sobre Active Directory Domain Services en Windows Server 2012 R2 y Windows Server 2012, y se explica el proceso para actualizar controladores de dominio de Windows Server 2008 o Windows Server 2008 R2.

## <a name="domain-controller-upgrade-steps"></a><a name="BKMK_UpgradeWorkflow"></a>Pasos de actualización de los controladores de dominio
El método recomendado de actualización de un dominio es promover controladores de dominio que ejecuten versiones más recientes de Windows Server y degradar controladores de dominio anteriores según sea necesario. Ese método es preferible a actualizar el sistema operativo de un controlador de dominios existente. En esta lista se describen los pasos generales a seguir antes de promover un controlador de dominio que ejecute una versión más reciente de Windows Server:

1. Compruebe que el servidor de destino cumple [los requisitos del sistema](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303418(v=ws.11)).
2. Compruebe la [compatibilidad de aplicaciones](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).
3. Compruebe la configuración de seguridad. Para obtener más información, vea [características desusadas y cambios de comportamiento relacionados con AD DS en Windows server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) y [configuración predeterminada segura en Windows Server 2008 y Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee522994(v=ws.10)#BKMK_SecureDefault).
4. Compruebe la conectividad del servidor de destino desde el equipo donde tenga intención de realizar la instalación.
5. Compruebe la disponibilidad de los roles de maestro de operaciones necesarios:

   - Para instalar el primer controlador de dominio que ejecuta Windows Server 2012 en un dominio y bosque existente, el equipo en el que se ejecuta la instalación necesita conectividad con el maestro de esquema para poder ejecutar Adprep/ForestPrep y el maestro de infraestructura para ejecutar Adprep/DomainPrep.
   - Para instalar el primer controlador de dominio en un dominio en el que el esquema del bosque ya esté extendido, solo es necesario conectarse al maestro de infraestructura.
   - Para instalar o quitar un dominio de un bosque existente, es necesario conectarse al maestro de nomenclatura de dominios.
   - Una instalación de controlador de dominio también requiere conectarse al maestro RID.
   - Si instala el primer controlador de dominio de solo lectura en un bosque existente, necesitará conectarse al maestro de infraestructura para cada partición de directorio de aplicaciones, que se conoce también como contexto de nomenclatura no de dominio (o NDNC).

6. Procure suministrar las credenciales que sean necesarias para ejecutar la instalación de AD DS.

   |Acción de instalación|Requisitos de credenciales|
   |-----------------------|---------------------------|
   |Instalar un bosque nuevo|Administrador local en el servidor de destino|
   |Instalar un dominio nuevo en un bosque existente|Administradores de empresas|
   |Instalar más controladores de dominio en un dominio existente|Administradores de dominio|
   |Ejecutar adprep /forestprep|Administradores de esquema, Administradores de empresas y Admins. del dominio|
   |Ejecutar adprep /domainprep|Administradores de dominio|
   |Ejecutar adprep /domainprep /gpprep|Administradores de dominio|
   |Ejecutar adprep /rodcprep|Administradores de empresas|

   Puede delegar permisos para instalar AD DS. Para obtener más información, consulte el tema sobre las [tareas de administración de la instalación](/previous-versions/windows/it-pro/windows-server-2003/cc773327(v=ws.10)).

En los siguientes vínculos encontrará instrucciones detalladas para promover controladores de dominio de Windows Server 2012 nuevos y réplicas de estos mediante cmdlets de Windows PowerShell y el Administrador del servidor:

- [Instalar Active Directory Domain Services (Nivel 100)](./install-active-directory-domain-services--level-100-.md)
- [Instalar un nuevo bosque de Active Directory de Windows Server 2012 (nivel 200)](./install-a-new-windows-server-2012-active-directory-forest--level-200-.md)
- [Instalar una réplica del controlador de dominio de Windows Server 2012 en un dominio existente (nivel 200)](./install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain--level-200-.md)
- [Instalar un nuevo dominio secundario o de árbol de Active Directory de Windows Server 2012 (nivel 200)](./install-a-new-windows-server-2012-active-directory-child-or-tree-domain--level-200-.md)
- [Instalar un controlador de dominio sólo servidor 2012 Active Directory lectura (RODC) (nivel 200) de Windows](./rodc/install-a-windows-server-2012-active-directory-read-only-domain-controller--rodc---level-200-.md)
- [Foro de Windows Server 2012 acerca de los controladores de dominio](https://docs.microsoft.com/answers/topics/windows-server-2012.html)

## <a name="windows-update-considerations"></a>Consideraciones sobre el Windows Update

Antes de la publicación de Windows 8, Windows Update administraba su propia programación interna para buscar actualizaciones y descargarlas e instalarlas. Requería la ejecución continua del Agente de Windows Update en segundo plano, con lo que se consumía memoria y otros recursos del sistema.

Windows 8 y Windows Server 2012 presentan una nueva característica denominada [Mantenimiento automático](/windows/win32/w8cookbook/automatic-maintenance). La característica Mantenimiento automático consolida muchas características distintas que solían administrar su lógica de ejecución y programación individualmente. Esta consolidación permite a todos estos componentes usar muchos menos recursos del sistema, funcionar de manera coherente, respetar el nuevo estado [Modo de espera conectado](/windows/win32/w8cookbook/automatic-maintenance) para tipos de dispositivo nuevos y consumir menos batería en dispositivos portátiles.

Como Windows Update forma parte del Mantenimiento automático en Windows 8 y Windows Server 2012, su programación interna para establecer un día y una hora para instalar las actualizaciones ya no tiene efecto. Para garantizar un comportamiento de reinicio coherente y predecible para todos los dispositivos y equipos de la empresa, incluidos los que ejecutan Windows 8 y Windows Server 2012, consulta el artículo de Microsoft Knowledge Base [2885694](https://support.microsoft.com/kb/2885694) (o bien, consulte el paquete acumulativo de octubre de 2013 [2883201](https://support.microsoft.com/kb/2883201)) y, a continuación, configure las opciones de directiva descritas en la entrada del blog de WSUS sobre cómo [habilitar una experiencia de usuario de Windows Update más predecible para Windows 8 y Windows Server 2012 (KB 2885694)](/archive/blogs/wsus/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694).

## <a name="whats-new-in-ad-ds-in-windows-server-2012-r2"></a><a name="BKMK_NewWS2012R2"></a>Novedades de AD DS en Windows Server 2012 R2

En la siguiente tabla se recogen las nuevas características de AD DS en Windows Server 2012 R2, con vínculos que llevan a información más detallada sobre dónde están disponibles. Consulte [Novedades de Active Directory en Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn268294(v=ws.11))para ver descripciones más completas sobre algunas de estas características y sus requisitos correspondientes.

|Característica|Descripción|
|-----------|---------------|
|[Unión al área de trabajo](../../ad-fs/operations/join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications.md)|Permite a los trabajadores de la información unir sus dispositivos personales a su compañía para tener acceso a los recursos y servicios de la compañía.|
|[Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280942(v=ws.11))|Proporciona acceso a la aplicación web con un nuevo servicio de rol de acceso remoto.|
|[Servicios de federación de Active Directory](../../active-directory-federation-services.md)|AD FS ha simplificado la implementación y las mejoras para permitir a los usuarios tener acceso a recursos de dispositivos personales y ayudar a los departamentos de TI a administrar el control de acceso.|
|[Unicidad de SPN y UPN](../manage/component-updates/spn-and-upn-uniqueness.md)|Los controladores de dominio en los que se ejecuta Windows Server 2012 R2 bloquean la creación de nombres de entidad de seguridad de servicio (SPN) y nombres principales de usuario (UPN) duplicados.|
|[Inicio de sesión con reinicio automático de Winlogon (ARSO)](../manage/component-updates/winlogon-automatic-restart-sign-on--arso-.md)|Permite que las aplicaciones de la pantalla de bloqueo se reinicien y estén disponibles en dispositivos Windows 8.1.|
|[Atestación de clave de TPM](../manage/component-updates/tpm-key-attestation.md)|Permite a las entidades de certificación (CA) atestar criptográficamente en un certificado emitido que la clave privada del solicitante del certificado está realmente protegida por un Módulo de plataforma segura (TPM).|
|[Protección y administración de credenciales](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn408190(v=ws.11))|Nuevos controles de protección de credenciales y autenticación de dominios para reducir el robo de credenciales.|
|[Degradación del servicio de replicación de archivos (FRS)](../manage/component-updates/directory-services-component-updates.md)|El nivel funcional de dominio de Windows Server 2003 también se desusa porque, en el nivel funcional, FRS usa para replicar SYSVOL. Esto significa que al crear un dominio en un servidor que ejecute Windows Server 2012 R2, el nivel funcional de dominio debe ser Windows Server 2008 o posterior. Todavía puede Agregar un controlador de dominio que ejecute Windows Server 2012 R2 a un dominio existente que tenga un nivel funcional de dominio de Windows Server 2003. no se puede crear un dominio nuevo en ese nivel.|
|[Nuevos niveles funcionales de dominios y bosques](../active-directory-functional-levels.md)|Hay nuevos niveles funcionales para Windows Server 2012 R2. Hay nuevas características disponibles en los niveles funcionales de dominio de Windows Server 2012 R2.|
|[Cambios en el optimizador de consultas LDAP](../manage/component-updates/directory-services-component-updates.md)|Se ha mejorado el rendimiento de la eficiencia de búsqueda LDAP y el tiempo de búsqueda LDAP de las consultas complejas.|
|[Mejoras en el evento 1644](../manage/component-updates/directory-services-component-updates.md)|Se han agregado estadísticas de resultados de búsqueda LDAP al identificador de evento 1644 para ayudar a solucionar problemas.|
|[Mejora del rendimiento de replicación de Active Directory](../manage/component-updates/directory-services-component-updates.md)|Ajusta el rendimiento máximo de replicación de AD de 40 Mbps a unos 600 Mbps.|

## <a name="whats-new-in-ad-ds-in-windows-server-2012"></a><a name="BKMK_WhatsNewAD"></a>Novedades de AD DS en Windows Server 2012

En la siguiente tabla se recogen las nuevas características de AD DS en Windows Server 2012, con vínculos que llevan a información más detallada sobre dónde están disponibles. Para obtener una explicación más detallada de algunas características, incluidos sus requisitos, consulte [novedades de Active Directory Domain Services (AD DS)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831477(v=ws.11)).

|Característica|Descripción|
|-----------|---------------|
|Activación basada en Active Directory (AD BA). Ver [Introducción a la activación por volumen](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612(v=ws.11))|Simplifica la tarea de configurar la distribución y administración de licencias de software por volumen.|
|[Servicios de federación de Active Directory (AD FS)](../../active-directory-federation-services.md)|Agrega, entre otras cosas, la instalación de roles por medio del Administrador del servidor, una configuración de confianzas simplificada, una administración de confianzas automática y compatibilidad con el protocolo SAML.|
|Eventos de vaciado de página perdida de Active Directory|El evento 530 NTDS ISAM con error de Jet -1119 se registra para detectar eventos de vaciado de página perdida en las bases de datos de Active Directory.|
|[Interfaz de usuario de la papelera de reciclaje de Active Directory](../get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md#ad_recycle_bin_mgmt)|El Centro de administración de Active Directory (ADAC) incorpora una característica de administración de GUI de la papelera de reciclaje que apareció inicialmente en Windows Server 2008 R2.|
|[Cmdlets de Windows PowerShell para la administración de la replicación y la topología de Active Directory](../manage/powershell/introduction-to-active-directory-replication-and-topology-management-using-windows-powershell--level-100-.md)|Permite crear y administrar sitios, vínculos de sitios y objetos de conexión de Active Directory (entre otras muchas cosas) mediante Windows PowerShell.|
|[Access Control dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md)|Nueva plataforma de autorización basada en notificaciones que mejora el modelo de control de acceso heredado.|
|[Interfaz de usuario de directiva de contraseña específica](../get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md#fine_grained_pswd_policy_mgmt)|ADAC incorpora compatibilidad de GUI para crear, modificar y asignar PSO agregados inicialmente en Windows Server 2008.|
|[Cuentas de servicio administradas de grupo (gMSA)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11))|Se trata de un nuevo tipo de entidad de seguridad denominada gMSA. Los servicios que se ejecutan en varios hosts pueden ejecutarse con la misma cuenta de gMSA.|
|[Unión a dominio sin conexión de DirectAccess](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574150(v=ws.11))|Se incluyen requisitos previos de DirectAccess para extender la unión a dominio sin conexión.|
|[Rápida implementación mediante la clonación de controladores de dominio virtuales](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574150(v=ws.11)#virtualized_dc_cloning)|Los controladores de dominio virtualizados se pueden implementar con enorme rapidez clonando los controladores de dominio virtuales existentes mediante cmdlets de Windows PowerShell.|
|[Cambios en el grupo RID](../manage/managing-rid-issuance.md)|Agrega nuevas cuotas y eventos de supervisión que protegen de un uso excesivo del grupo RID global. Duplica el tamaño del grupo RID global de manera opcional en caso de que el grupo original se agote.|
|Servicio de hora protegido|Mejora la seguridad de W32Tm eliminando secretos de la conexión, quitando las funciones hash MD5 y haciendo que el servidor se autentique con los clientes de hora de Windows 8.|
|[Protección de la reversión de USN para controladores de dominio virtualizados](../manage/managing-rid-issuance.md)|Ya no se produce una reversión de USN cuando se restauran por error copias de seguridad de instantánea de controladores de dominio virtualizados.|
|[Visor del historial de Windows PowerShell](../get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md#windows_powershell_history_viewer)|Permite que los administradores vean los comandos de Windows PowerShell ejecutados al usar ADAC.|

### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a><a name="BKMK_"></a>Mantenimiento automático y cambios en el comportamiento de reinicio después de que Windows Update aplique actualizaciones

Antes de la publicación de Windows 8, Windows Update administraba su propia programación interna para buscar actualizaciones y descargarlas e instalarlas. Requería la ejecución continua del Agente de Windows Update en segundo plano, con lo que se consumía memoria y otros recursos del sistema.

Windows 8 y Windows Server 2012 presentan una nueva característica denominada [Mantenimiento automático](/windows/win32/w8cookbook/automatic-maintenance). La característica Mantenimiento automático consolida muchas características distintas que solían administrar su lógica de ejecución y programación individualmente. Esta consolidación permite a todos estos componentes usar muchos menos recursos del sistema, funcionar de manera coherente, respetar el nuevo estado [Modo de espera conectado](/windows/win32/w8cookbook/automatic-maintenance) para tipos de dispositivo nuevos y consumir menos batería en dispositivos portátiles.

Como Windows Update forma parte del Mantenimiento automático en Windows 8 y Windows Server 2012, su programación interna para establecer un día y una hora para instalar las actualizaciones ya no tiene efecto. Para garantizar un comportamiento de reinicio coherente y predecible de todos los dispositivos y equipos de tu empresa, incluidos los que ejecutan Windows 8 y Windows Server 2012, puedes configurar las siguientes opciones de directiva de grupo:

- **Configuración del equipo|Directivas|Plantillas administrativas|Componentes de Windows|Windows Update|Configurar Actualizaciones automáticas**
- **Configuración del equipo|Directivas|Plantillas administrativas|Componentes de Windows|Windows Update|No reiniciar automáticamente con usuarios que hayan iniciado sesión**
- **Configuración del equipo|Directivas|Plantillas administrativas|Componentes de Windows|Programador de mantenimiento|Retraso aleatorio de mantenimiento**

En la siguiente tabla se incluyen algunos ejemplos sobre cómo configurar estas opciones para proporcionar el comportamiento de reinicio deseado.

|||
|-|-|
|**Escenario**|**Configuraciones recomendadas**|
|**Administrado por WSUS**<p>-Instalar actualizaciones una vez por semana<br />-Reinicie Fridays en 11 P.M.|Establecer equipos para instalar automáticamente e impedir el reinicio automático hasta la hora deseada<p>**Directiva**: Configurar actualizaciones automáticas (habilitada)<p>Configurar actualizaciones automáticas: 4-descargar automáticamente y programar la instalación<p>**Directiva**: no reiniciar automáticamente con usuarios que han iniciado sesión (deshabilitado)<p>**Fechas límite de WSUS**: establecer en viernes a las 23:00|
|**Administrado por WSUS**<p>-Escalonar instalaciones en distintas horas/días|Establecer grupos de destino para distintos grupos de equipos que deben actualizarse juntos<p>Usar los pasos del escenario anterior<p>Establecer distintas fechas límites para distintos grupos de destino|
|**No administrado por WSUS: no se admiten las fechas límite**<p>-Escalonar instalaciones en momentos diferentes|**Directiva**: Configurar actualizaciones automáticas (habilitada)<p>Configurar actualizaciones automáticas: 4-descargar automáticamente y programar la instalación<p>**Clave del Registro:** habilite la clave del registro que se describe en el artículo de la Microsoft Knowledge Base [2835627](https://support.microsoft.com/kb/2835627)<p>**Directiva:** Retraso aleatorio de mantenimiento automático (habilitada)<p>Establece **Retraso aleatorio de mantenimiento normal** en PT6H para indicar un retraso aleatorio de seis horas y así tener el siguiente comportamiento:<p>-Las actualizaciones se instalarán en el tiempo de mantenimiento configurado más un retraso aleatorio<p>-El reinicio de cada máquina tendrá lugar exactamente 3 días más tarde<p>Como alternativa, establece una hora de mantenimiento distinta para cada grupo de equipos.|

Para obtener más información sobre por qué el equipo de ingeniería de Windows ha implementado estos cambios, consulta la entrada sobre cómo [minimizar los reinicios después de las actualizaciones automáticas en Windows Update](https://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).

## <a name="ad-ds-server-role-installation-changes"></a><a name="BKMK_InstallationChanges"></a>Cambios de instalación del rol del servidor de AD DS

En Windows Server 2003 (y hasta Windows Server 2008 R2) se ejecutaba la versión x86 X64 de la herramienta de línea de comandos Adprep.exe antes de ejecutar el Asistente para instalación de Active Directory, mientras que Dcpromo.exe y Dcpromo.exe disponían de variantes opcionales para realizar la instalación desde un medio o una instalación silenciosa.

A partir de Windows Server 2012, las instalaciones de línea de comandos se llevan a cabo por medio del módulo ADDSDeployment en Windows PowerShell. Las promociones basadas en GUI se realizan en el Administrador del servidor por medio de un Asistente para configuración de AD DS completamente nuevo. A fin de simplificar el proceso de instalación, ADPREP ahora forma parte de la instalación de AD DS y se ejecuta automáticamente cuando sea necesario. El Asistente para configuración de AD DS basado en Windows PowerShell tiene como destino automáticamente los roles de maestro de esquema y de infraestructura en los dominios en los que se agregan los controladores de dominio y, a continuación, ejecuta de forma remota los comandos de Adprep necesarios en los controladores de dominio correspondientes.

Las comprobaciones de los requisitos previos en el Asistente para configuración de AD DS identifican los posibles errores antes de que comience la instalación. Las situaciones de error se pueden subsanar para eliminar los problemas surgidos de una actualización completada parcialmente. Este asistente también exporta un script de Windows PowerShell que contiene todas las opciones que se especificaron durante la instalación gráfica.

En conjunto, los cambios en la instalación de AD DS reducen la complejidad del proceso de instalación del rol de controlador de dominio, así como la probabilidad de que se produzcan errores administrativos, especialmente si se implementan varios controladores de dominio en varias regiones y dominios.
En [Instalar Servicios de dominio de Active Directory](./install-active-directory-domain-services--level-100-.md)encontrará información más detallada sobre las instalaciones basadas en GUI y Windows PowerShell, así como la sintaxis de línea de comandos e instrucciones paso a paso del asistente. Aquellos administradores que quieran controlar la incorporación de los cambios de esquema a un bosque de Active Directory independiente de la instalación de controladores de dominio de Windows Server 2012 en un bosque existente pueden seguir ejecutando los comandos de Adprep.exe desde un símbolo del sistema con privilegios elevados.

## <a name="deprecated-features-and-behavior-changes-related-to-ad-ds-in-windows-server-2012"></a><a name="BKMK_DeprecatedFeatures"></a>Características desusadas y cambios de comportamiento relativos a AD DS en Windows Server 2012

Existen algunos cambios relativos a AD DS:

- **Desuso de Adprep32.exe**
   - Solo hay una versión de Adprep.exe, que se puede ejecutar cuando sea necesario en servidores de 64 bits que ejecuten Windows Server 2008 o posterior. La ejecución puede ser remota y, de hecho, debe serlo cuando el rol de maestro de operaciones de destino esté hospedado en un sistema operativo de 32 bits o en Windows Server 2003.
- **Desuso de Dcpromo.exe**
   - Dcpromo está en desuso, aunque en Windows Server 2012 solo se puede ejecutar con un archivo de respuesta o parámetros de línea de comandos para proporcionar a las organizaciones el tiempo necesario para realizar la transición de la automatización existente a las nuevas opciones de instalación de Windows PowerShell.
- **LMHash está deshabilitado en las cuentas de usuario**
  - Las opciones predeterminadas de seguridad de las plantillas de seguridad en Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012 habilitan la directiva NoLMHash, que está deshabilitada en las plantillas de seguridad de los controladores de dominio de Windows 2000 y Windows Server 2003. Siga los pasos del artículo de KB [946405](https://support.microsoft.com/kb/946405)para deshabilitar la directiva NoLMHash en clientes dependientes de LMHash según sea necesario.

A partir de Windows Server 2008, los controladores de dominio también tienen la siguiente configuración predeterminada segura, en comparación con los controladores de dominio que ejecutan Windows Server 2003 o Windows 2000.

| Directiva o tipo de cifrado | Windows Server 2008 predeterminado | Windows Server 2012 y Windows Server 2008 R2 predeterminado | Comentario |
|--|--|--|--|
| AllowNT4Crypto | Disabled | Disabled | Los clientes de Bloque de mensajes del servidor (SMB) de terceros pueden no ser compatibles con la configuración predeterminada de seguridad en los controladores de dominio. En todos los casos, esta configuración se puede relajar para permitir la interoperabilidad, pero solamente a costa de la seguridad. Para obtener más información, vea el [artículo 942564](https://go.microsoft.com/fwlink/?LinkId=164558) de Microsoft Knowledge base ( https://go.microsoft.com/fwlink/?LinkId=164558) . |
| DES | habilitado | Disabled | [Artículo 977321](https://go.microsoft.com/fwlink/?LinkId=177717) de Microsoft Knowledge base (https://go.microsoft.com/fwlink/?LinkId=177717) |
| Protección extendida/CBT para autenticación integrada | N/D | habilitado | Vea el [aviso de seguridad de Microsoft (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) ( https://go.microsoft.com/fwlink/?LinkId=164559) y el [artículo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) de Microsoft Knowledge base () https://go.microsoft.com/fwlink/?LinkId=178251) .<p>Revise e instale la revisión del [artículo 977073](https://go.microsoft.com/fwlink/?LinkId=186394) ( https://go.microsoft.com/fwlink/?LinkId=186394) en Microsoft Knowledge base según sea necesario). |
| LMv2 | habilitado | Disabled | [Artículo 976918](https://go.microsoft.com/fwlink/?LinkId=178251) de Microsoft Knowledge base (https://go.microsoft.com/fwlink/?LinkId=178251) |

## <a name="operating-system-requirements"></a><a name="BKMK_SysReqs"></a>Requisitos de sistema operativo

En la tabla siguiente se enumeran los requisitos mínimos del sistema para Windows Server 2012. Para obtener más información sobre los requisitos de sistema, así como información de instalación previa, consulte [Instalar Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)). No hay requisitos de sistema adicionales para instalar un nuevo bosque de Active Directory, si bien deberá agregar memoria suficiente para almacenar en caché el contenido de la base de datos de Active Directory, ya que de este modo obtendrá un mejor rendimiento de los controladores de dominio, las solicitudes de cliente LDAP y las aplicaciones habilitadas para Active Directory. Si estás actualizando un controlador de dominio existente o agregando un nuevo controlador de dominio a otro bosque, revisa la sección siguiente para asegurarte de que el servidor cumpla con los requisitos de espacio en disco.

| Requisito | Valor |
|--|--|
| Procesador | Procesador de 64 bits a 1,4 GHz |
| RAM | 512 MB |
| Requisitos de espacio libre en disco | 32 GB |
| Resolución de pantalla | 800x600 o superior |
| Varios | Unidad de DVD, teclado y acceso a Internet |

### <a name="disk-space-requirements-for-upgrading-domain-controllers"></a><a name="BKMK_DiskSpaceDCWin8"></a>Requisitos de espacio en disco para actualizar controladores de dominio

En esta sección se describen los requisitos de espacio en disco solo para actualizar controladores de dominio de Windows Server 2008 o Windows Server 2008 R2. Para obtener más información sobre requisitos de espacio en disco para actualizar controladores de dominio a versiones anteriores de Windows Server, consulte el tema sobre los [requisitos de espacio en disco para actualizar a Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754463(v=ws.10)#BKMK_2008) o los [requisitos de espacio en disco para actualizar a Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754463(v=ws.10)#BKMK_2008R2).

Disponga de un disco para hospedar los archivos de registro y base de datos de Active Directory con un tamaño tal que dé cabida a las extensiones de esquema personalizadas y basadas en aplicaciones y los índices iniciados por aplicaciones y administradores, además de espacio para los objetos y atributos que se agregarán al directorio durante la vigencia de la implementación del controlador de dominio (que suele ser de entre 5 y 8 años). Adquirir un tamaño adecuado en el momento de la implementación suele ser una buena inversión en comparación con los costes (considerablemente más elevados) derivados de ampliar el almacenamiento en disco tras la implementación. Para obtener más información, consulte el tema sobre cómo [planear la capacidad de Servicios de dominio de Active Directory](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/active-directory-server/capacity-planning-for-active-directory-domain-services).

En los controladores de dominio que tengas previsto actualizar, procura que la unidad que hospeda la base de datos de Active Directory (NTDS.DIT) tenga un espacio disponible en disco de al menos el 20 % del archivo NTDS.DIT antes de comenzar con la actualización del sistema operativo. Si no hay suficiente espacio libre en disco, la actualización puede dar error y el informe de compatibilidad de actualización puede devolver un error que indica que el espacio libre en disco es insuficiente:

En este caso, puede intentar una desfragmentación sin conexión de la base de datos de Active Directory para recuperar espacio adicional y, a continuación, volver a intentar la actualización. Para obtener más información, consulte el tema sobre la [compactación del archivo de base de datos del directorio (desfragmentación sin conexión)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794920(v=ws.10)).

### <a name="available-skus"></a>SKU disponibles

Hay cuatro ediciones de Windows Server: Foundation, Essentials, Standard y Datacenter.
Las dos ediciones que admiten el rol de AD DS son Standard y Datacenter.

En las versiones anteriores, la ediciones de Windows Server diferían a la hora de admitir roles de servidor, recuentos de procesador y grandes cantidades de memoria. Las ediciones Standard y Datacenter de Windows Server admiten todas las características y el hardware subyacente, pero varían en función de los derechos de virtualización; se permiten dos instancias virtuales para Standard Edition y se permiten instancias virtuales ilimitadas para Datacenter Edition.

### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Sistemas operativos de cliente y servidor de Windows que pueden unirse a dominios de Windows Server

Los siguientes sistemas operativos de cliente Windows y Windows Server se admiten como equipos miembro de dominio en los controladores de dominio que ejecutan Windows Server 2012 o posterior:

- Sistemas operativos servidor: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003

## <a name="supported-in-place-upgrade-paths"></a><a name="BKMK_UpgradePaths"></a>Rutas de acceso de actualización en contexto admitidas

Los controladores de dominio que ejecutan versiones de 64 bits de Windows Server 2008 o Windows Server 2008 R2 se pueden actualizar a Windows Server 2012. pero no los controladores de dominio que ejecuten Windows Server 2003 o las versiones de 32 bits de Windows Server 2008. Para reemplazarlos, instala controladores de dominio que ejecuten una versión posterior de Windows Server en el dominio y, a continuación, quita los controladores de dominio que ejecuten Windows Server 2003.

| Si ejecuta estas ediciones | Puede actualizar a estas ediciones |
|--|--|
| Windows Server 2008 Standard con SP2<p>O<p>Windows Server 2008 Enterprise con SP2 | Windows Server 2012 Standard<p>O<p>Windows Server 2012 Datacenter |
| Windows Server 2008 Datacenter con SP2 | Windows Server 2012 Datacenter |
| Windows Web Server 2008 | Windows Server 2012 Standard |
| Windows Server 2008 R2 Standard con SP1<p>O<p>Windows Server 2008 R2 Enterprise con SP1 | Windows Server 2012 Standard<p>O<p>Windows Server 2012 Datacenter |
| Windows Server 2008 R2 Datacenter con SP1 | Windows Server 2012 Datacenter |
| Windows Web Server 2008 R2 | Windows Server 2012 Standard |

Para obtener más información sobre las rutas de actualización admitidas, consulte [Versiones de evaluación y opciones de actualización para Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Recuerde que no puede convertir un controlador de dominio que ejecuta una versión de evaluación de Windows Server 2012 directamente a una versión comercial. En su lugar, instale un controlador de dominio más en un servidor que ejecute una versión comercial y quite AD DS del controlador de dominio que se ejecuta en la versión de evaluación.

Debido a un problema conocido, no se puede actualizar un controlador de dominio que ejecute una instalación Server Core de Windows Server 2008 R2 a una instalación Server Core de Windows Server 2012. La actualización fallará y aparecerá una pantalla negra a final del proceso de actualización. Si se reinician estos controladores de dominio, el archivo boot.ini expone una opción para revertir a la versión de sistema operativo anterior. Un reinicio más activará la reversión automática a la versión de sistema operativo anterior. Hasta que haya una solución disponible, se recomienda instalar un nuevo controlador de dominio que ejecute una instalación Server Core de Windows Server 2012 en lugar de actualizar en contexto un controlador de dominio existente que ejecute una instalación Server Core de Windows Server 2008 R2. Para obtener más información, consulte el artículo de KB [2734222](https://support.microsoft.com/kb/2734222).

## <a name="functional-level-features-and-requirements"></a><a name="BKMK_FunctionalLevels"></a>Requisitos y características de niveles funcionales

Windows Server 2012 requiere un nivel funcional de bosque de Windows Server 2003. Es decir, para poder agregar un controlador de dominio que ejecute Windows Server 2012 a un bosque de Active Directory existente, el nivel funcional del bosque debe ser Windows Server 2003 o posterior. Esto significa que los controladores de dominio que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003 pueden funcionar en el mismo bosque, pero los controladores de dominio que ejecutan Windows 2000 Server no son compatibles y bloquearán la instalación de un controlador de dominio que ejecute Windows Server 2012. Si el bosque contiene controladores de dominio que ejecutan Windows Server 2003 o posterior, pero el nivel funcional de dicho bosque sigue siendo Windows 2000, la instalación se bloqueará igualmente.

Los controladores de dominio de Windows 2000 se deben quitar antes de agregar controladores de dominio de Windows Server 2012 al bosque. En este caso, considere el siguiente flujo de trabajo:

1. Instale controladores de dominio que ejecuten Windows Server 2003 o posterior. Estos controladores de dominio se pueden implementar en una versión de evaluación de Windows Server. En este paso también hay que [ejecutar adprep.exe](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)) para la versión de sistema operativo a modo de requisito previo.
2. Quite los controladores de dominio de Windows 2000. En concreto, quite a la fuerza o disminuya de nivel correctamente los controladores de dominio de Windows Server 2000 del dominio y utilice Usuarios y equipos de Active Directory para quitar las cuentas de controlador de dominio correspondientes a todos los controladores de dominio eliminados.
3. Eleve el nivel funcional del bosque a Windows Server 2003 o posterior.
4. Instala controladores de dominio que ejecuten Windows Server 2012.
5. Quite los controladores de dominio que ejecuten versiones anteriores de Windows Server.

El nuevo nivel funcional de dominio de Windows Server 2012 habilita una característica nueva: la **compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos de** la Directiva de plantillas administrativas de KDC tiene dos configuraciones (**proporcionar siempre notificaciones** y **error de solicitudes de autenticación sin blindar**) que requieren el nivel funcional de dominio de Windows Server 2012.

El nivel funcional del bosque de Windows Server 2012 no proporciona características nuevas, pero asegura que cualquier dominio nuevo creado en el bosque funcione automáticamente en el nivel funcional del dominio de Windows Server 2012. El nivel funcional de dominio de Windows Server 2012 no proporciona otras características nuevas más allá de la compatibilidad de KDC con notificaciones, autenticación compuesta y protección de Kerberos. No obstante, garantiza que cualquier controlador de dominio del dominio ejecute Windows Server 2012. Para obtener más información acerca de otras características que se encuentran disponibles en distintos niveles funcionales, consulte el tema sobre la [descripción de los niveles funcionales de los Servicios de dominio de Active Directory (AD DS)](../active-directory-functional-levels.md).

Después de establecer el nivel funcional del bosque en un determinado valor, no se puede revertir o bajar el nivel funcional del bosque, con las siguientes excepciones: después de elevar el nivel funcional del bosque a Windows Server 2012, puede reducirlo a Windows Server 2008 R2. Si no se ha habilitado la papelera de reciclaje de Active Directory, también puede reducir el nivel funcional del bosque de Windows Server 2012 a Windows Server 2008 R2 o Windows Server 2008 o de Windows Server 2008 R2 a Windows Server 2008. Si el nivel funcional del bosque está establecido en Windows Server 2008 R2, no se puede revertir, por ejemplo, a Windows Server 2003.

Después de establecer el nivel funcional del dominio en un determinado valor, no se puede revertir o bajar el nivel funcional del dominio, con las siguientes excepciones: cuando eleva el nivel funcional del dominio a Windows Server 2008 R2 o Windows Server 2012, y si el nivel funcional del bosque es Windows Server 2008 o inferior, tiene la opción de revertir el nivel funcional del dominio a Windows Server 2008 o Windows Server 2008 R2. Solo puede bajar el nivel funcional del dominio de Windows Server 2012 a Windows Server 2008 R2 o Windows Server 2008 o de Windows Server 2008 R2 a Windows Server 2008. Si el nivel funcional del dominio está establecido en Windows Server 2008 R2, no se puede revertir, por ejemplo, a Windows Server 2003.

Para obtener más información sobre las características disponibles a niveles funcionales inferiores, consulte [Descripción de los niveles funcionales de Servicios de dominio de Active Directory (AD DS)](../active-directory-functional-levels.md).

Más allá de los niveles funcionales, un controlador de dominio que ejecuta Windows Server 2012 proporciona características adicionales que no están disponibles en un controlador de dominio que ejecuta una versión anterior de Windows Server. Por ejemplo, un controlador de dominio que ejecuta Windows Server 2012 puede usarse para la clonación del controlador de dominio virtual, mientras que un controlador de dominio que ejecuta una versión anterior de Windows Server no puede hacerlo. Pero la clonación de controladores de dominio virtuales y las medidas de seguridad del controlador de dominio virtual en Windows Server 2012 no tienen ningún requisito de nivel funcional.

> [!NOTE]
> Microsoft Exchange Server 2013 requiere un nivel funcional de bosque de Windows Server 2003 como mínimo.

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a><a name="BKMK_ServerRoles"></a>Interoperabilidad de AD DS con otros roles del servidor y sistemas operativos Windows

AD DS no es compatible con los siguientes sistemas operativos:

- Windows MultiPoint Server
- Windows Server 2012 Essentials

AD DS no se puede instalar en un servidor donde también se ejecuten los siguientes roles o servicios del servidor:

- Servidor de Hyper-V
- Agente de conexión a Escritorio remoto

## <a name="operations-master-roles"></a><a name="BKMK_OpsMasters"></a>Roles de maestro de operaciones

Algunas características nuevas de Windows Server 2012 afectan a los roles de maestro de operaciones:

- El emulador de PDC debe ejecutar Windows Server 2012 para admitir la clonación de controladores de dominio virtuales. Existen requisitos previos adicionales para clonar DC. Para obtener más información, consulte el tema sobre la [virtualización de los Servicios de dominio de Active Directory (AD DS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)).
- Se crean nuevas entidades de seguridad cuando el emulador de PDC ejecuta Windows Server 2012.
- El maestro de RID tiene nueva emisión de RID y funcionalidad de supervisión. Las mejoras incluyen un mejor registro de eventos, límites más apropiados y, en caso de emergencia, la capacidad para incrementar la asignación general de bloque de RID en un bit. Para obtener más información, consulte el tema sobre la [administración de la emisión de RID](../../ad-ds/manage/Managing-RID-Issuance.md).

> [!NOTE]
> Aunque no son roles de maestro de operaciones, otro cambio en AD DS instalación es que el rol de servidor DNS y el catálogo global se instalan de forma predeterminada en todos los controladores de dominio que ejecutan Windows Server 2012.

## <a name="virtualizing-domain-controllers"></a><a name="BKMK_Virtual"></a>Virtualización de controladores de dominio

Las mejoras en AD DS a partir de Windows Server 2012 permiten una virtualización más segura de controladores de dominio y la capacidad de clonar controladores de dominio. A su vez, clonar controladores de dominio permite implementar rápidamente controladores de dominio adicionales en un nuevo dominio y otros beneficios. Para obtener más información, consulte [Introducción a la &#40;de Active Directory Domain Services AD DS&#41; virtualización &#40;nivel 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).

## <a name="administration-of-windows-server-2012-servers"></a><a name="BKMK_Admin"></a>Administración de servidores de Windows Server 2012

Use el [herramientas de administración remota del servidor para Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) para administrar controladores de dominio y otros servidores que ejecutan windows Server 2012. Puede ejecutar el Herramientas de administración remota del servidor de Windows Server 2012 en un equipo que ejecute Windows 8.

## <a name="application-compatibility"></a><a name="BKMK_AppCompat"></a>Compatibilidad de aplicaciones

En la siguiente tabla se recogen las aplicaciones de Microsoft comunes que se integran en Active Directory. En esta tabla se indica en qué versiones de Windows Server se pueden instalar estas aplicaciones y si la existencia de controladores de dominio de Windows Server 2012 afecta a la compatibilidad de las aplicaciones.

|Producto|Notas|
|-----------|---------|
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2 es necesario para que <br />SharePoint 2010 se instale y funcione en servidores con Windows Server 2012<p>SharePoint 2010 Foundation Service Pack 2 es necesario para que SharePoint 2010 Foundation se instale y funcione en servidores con Windows Server 2012.<p>El proceso de instalación de SharePoint Server 2010 (sin service packs) dará error en Windows Server 2012.<p>El instalador de requisitos previos de SharePoint Server 2010 (PrerequisiteInstaller.exe) produce el error "este programa tiene problemas de compatibilidad". Al hacer clic en "ejecutar el programa sin obtener ayuda", se muestra el error "comprobando si SharePoint se puede instalar &#124; SharePoint Server 2010 (sin Service Packs) no se puede instalar en Windows Server 2012."|
|[Microsoft SharePoint 2013](/SharePoint/install/hardware-and-software-requirements-0)|Requisitos mínimos para un servidor de base de datos en una granja de servidores:<p>La edición de 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter, o la versión de 64 bits de Windows Server 2012 Standard o Datacenter.<p>Requisitos mínimos para un único servidor con base de datos integrada:<p>La edición de 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter, o la versión de 64 bits de Windows Server 2012 Standard o Datacenter.<p>Requisitos mínimos para servidores web front-end y servidores de aplicación en una granja de servidores:<p>La edición de 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise o Datacenter, o la versión de 64 bits de Windows Server 2012 Standard o Datacenter.|
|[Configuration Manager 2012](/SharePoint/install/hardware-and-software-requirements-0)|Configuration Manager 2012 Service Pack 1:<p>Microsoft incluirá los siguientes sistemas operativos en nuestra matriz de compatibilidad de cliente con la versión de Service Pack 1:<p>-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter<p>Todos los roles del servidor de sitio (incluidos servidores de sitio, proveedores de SMS y puntos de administración) se pueden implementar en servidores con las siguientes ediciones de sistema operativo:<p>-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|
|[Configuration Manager de punto de conexión de Microsoft (rama actual)](/configmgr/core/plan-design/configs/supported-configurations)|[Sistemas operativos compatibles para servidores de sistema de sitio Configuration Manager](/configmgr/core/plan-design/configs/supported-operating-systems-for-site-system-servers).|
|[Microsoft Lync Server 2013](/lyncserver/lync-server-2013-server-and-tools-operating-system-support)|Lync Server 2013 requiere Windows Server 2008 R2 o Windows Server 2012. No se puede ejecutar en una instalación Server Core. Sí se puede ejecutar en [servidores virtuales](/lyncserver/lync-server-2013-running-lync-server-on-virtual-servers).|
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 se puede instalar en una instalación nueva (que no actualizada) de Windows Server 2012 si se han instalado las [actualizaciones acumulativas para Lync Server de octubre de 2012](https://support.microsoft.com/?kbid=2493736) . No se puede actualizar el sistema operativo a Windows Server 2012 para una instalación existente de Lync Server 2010. Windows Server 2012 tampoco admite el servidor de chat en grupo de Microsoft Lync Server 2010.|
|[System Center 2012 Endpoint Protection](/SharePoint/install/hardware-and-software-requirements-0)|System Center 2012 Endpoint Protection Service Pack 1 actualizará la matriz de compatibilidad de cliente de forma que incluya los siguientes sistemas operativos:<p>-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|
|[System Center 2012 Forefront Endpoint Protection](/SharePoint/install/hardware-and-software-requirements-0)|FEP 2010 con el paquete acumulativo de actualizaciones 1 actualizará la matriz de compatibilidad de cliente de forma que incluya los siguientes sistemas operativos:<p>-Windows 8 Pro<br />-Windows 8 Enterprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|
|Forefront Threat Management Gateway (TMG)|TMG se puede ejecutar únicamente en Windows Server 2008 y Windows Server 2008 R2. Para obtener más información, consulte [Requisitos del sistema para Forefront TMG](/previous-versions/tn-archive/dd896981(v=technet.10)).|
|Windows Server Update Services|Esta versión de WSUS ya admite equipos basados en Windows 8 o en Windows Server 2012 como clientes.|
|Windows Server Update Services 3.0|El artículo [2734608](https://support.microsoft.com/kb/2734608) de Knowledge base permite que los servidores que ejecutan Windows Server Update Services (WSUS) 3,0 SP2 proporcionen actualizaciones a los equipos que ejecutan Windows 8 o windows Server 2012: **Nota:** los clientes con entornos independientes de WSUS 3,0 SP2 o Configuration Manager 2007 entornos de Service Pack 2 con WSUS 3,0 SP2 requieren que [2734608](https://support.microsoft.com/kb/2734608) administre correctamente equipos basados en Windows 8 o Windows Server 2012 como clientes.|
|[Exchange 2013](/Exchange/plan-and-deploy/prerequisites?view=exchserver-2019)|Windows Server 2012 Standard y Datacenter son compatibles para los siguientes roles: maestro de esquema, servidor de catálogo global, controlador de dominio, buzón y servidor de acceso de cliente.<p>Nivel funcional de bosque: Windows Server 2003 o superior<p>Origen: Requisitos del sistema para Exchange 2013|
|Exchange 2010|[Origen: Exchange 2010 Service Pack 3](https://techcommunity.microsoft.com/t5/exchange-team-blog/bg-p/Exchange)<p>Exchange 2010 con Service Pack 3 se puede instalar en servidores miembro de Windows Server 2012.<p>En[Requisitos del sistema para Exchange 2010](/previous-versions/office/exchange-server-2010/aa996719(v=exchg.141)) se indican el maestro de esquema, catálogo global y controlador de dominio más recientes como Windows Server 2008 R2.<p>Nivel funcional de bosque: Windows Server 2003 o superior|
|SQL Server 2012|Origen: KB [2681562](https://support.microsoft.com/kb/2681562)<p>Windows Server 2012 admite SQL Server 2012 RTM.|
|SQL Server 2008 R2|Origen: KB [2681562](https://support.microsoft.com/kb/2681562)<p>Se necesita SQL Server 2008 R2 con Service Pack 1 o posterior para instalar en Windows Server 2012.|
|SQL Server 2008|Origen: KB [2681562](https://support.microsoft.com/kb/2681562)<p>Se necesita SQL Server 2008 con Service Pack 3 o posterior para instalar en Windows Server 2012.|
|SQL Server 2005|Origen: KB [2681562](https://support.microsoft.com/kb/2681562)<p>No se puede instalar en Windows Server 2012.|

## <a name="known-issues"></a><a name="BKMK_KnownIssues"></a>Problemas conocidos

La siguiente tabla contiene los problemas conocidos relacionados con la instalación de AD DS.

| Título y número del artículo de KB | Ámbito tecnológico afectado | Problema/descripción |
|--|--|--|
| [2830145](https://support.microsoft.com/kb/2830145): SID S-1-18-1 y SID S-1-18-2 no se pueden asignar en equipos basados en Windows 7 o Windows Server 2008 R2 en un entorno de dominio | Administración de AD DS/compatibilidad con aplicaciones | Las aplicaciones que asignan SID S-1-18-1 y SID S-1-18-2, que son nuevos en Windows Server 2012, podrían tener errores porque los SID no se pueden resolver en equipos basados en Windows 7 o Windows Server 2008 R2. Para resolver este problema, instale la revisión en los equipos basados en Windows 7 o Windows Server 2008 R2 en el dominio. |
| [2737129](https://support.microsoft.com/kb/2737129): la preparación de la directiva de grupo no se produce al preparar automáticamente un dominio existente para Windows Server 2012 | Instalación de AD DS | Adprep /domainprep /gpprep no se ejecuta automáticamente como parte de la instalación del primer controlador de dominio que ejecuta Windows Server 2012 en un dominio. Si nunca antes se ha ejecutado en el dominio, deberá ejecutarse manualmente. |
| [2737416](https://support.microsoft.com/kb/2737416): la implementación de controladores de dominio basados en Windows PowerShell no deja de mostrar advertencias | Instalación de AD DS | Durante la validación de los requisitos previos pueden aparecer advertencias, al igual que posteriormente, en el transcurso de la instalación. |
| [2737424](https://support.microsoft.com/kb/2737424): el error "El formato del nombre de dominio especificado no es válido" aparece al intentar quitar Servicios de dominio de Active Directory de un controlador de dominio | Instalación de AD DS | Este error aparece cuando se quiere quitar el último controlador de dominio de un dominio donde todavía hay cuentas RODC creadas con anterioridad. Esto afecta a Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008. |
| [2737463](https://support.microsoft.com/kb/2737463): el controlador de dominio no se inicia, se produce un error c00002e2 o aparece el mensaje "Elegir una opción" | Instalación de AD DS | Un controlador de dominio no se inicia porque un administrador ha usado Dism.exe, Pkgmgr.exe o Ocsetup.exe para quitar el rol DirectoryServices-DomainController. |
| [2737516](https://support.microsoft.com/kb/2737516): limitaciones de comprobación de IFM en el Administrador del servidor de Windows Server 2012 | Instalación de AD DS | Es posible que la comprobación de IFM presente limitaciones, como se explica en el artículo de KB. |
| [2737535](https://support.microsoft.com/kb/2737535): el cmdlet Install-AddsDomainController devuelve un error de conjunto de parámetros para RODC | Instalación de AD DS | Puede que se muestre un error cuando trate de adjuntar un servidor a una cuenta RODC, en caso de que especifique argumentos que ya se hayan rellenado en la cuenta RODC creada previamente. |
| [2737560](https://support.microsoft.com/kb/2737560): aparece el error "No se puede realizar la comprobación de conflicto de esquema de Exchange" y la comprobación de los requisitos previos da error | Instalación de AD DS | La comprobación de los requisitos previos no se lleva a cabo al configurar el primer controlador de dominio de Windows Server 2012 en un dominio existente, debido a que los controladores de dominio carecen del privilegio SeServiceLogonRight para servicio de red o a que los protocolos DCOM están bloqueados. |
| [2737797](https://support.microsoft.com/kb/2737797): el módulo AddsDeployment con el argumento -Whatif muestra resultados de DNS incorrectos | Instalación de AD DS | El parámetro-WhatIf indica que el servidor DNS no se instalará, pero será. |
| [2737807](https://support.microsoft.com/kb/2737807): el botón Siguiente no está disponible en la página Opciones del controlador de dominio | Instalación de AD DS | El botón Siguiente está deshabilitado en la página Opciones del controlador de dominio porque la dirección IP del controlador de dominio de destino no señala una subred o sitio existente, o porque la contraseña de DSRM no se ha escrito y confirmado bien. |
| [2737935](https://support.microsoft.com/kb/2737935): la instalación de Active Directory se detiene en la fase "Creando el objeto de configuración NTDS" | Instalación de AD DS | La instalación se cuelga porque la contraseña del administrador local coincide con la del administrador de dominio, o porque hay problemas de red que impiden que la replicación crítica se complete. |
| [2738060](https://support.microsoft.com/kb/2738060): aparece un mensaje de error "Acceso denegado" al crear un dominio secundario de manera remota mediante Install-AddsDomain | Instalación de AD DS | El error surge al ejecutar Install-ADDSDomain con el cmdlet Invoke-Command si la contraseña de DNSDelegationCredential no es correcta. |
| [2738697](https://support.microsoft.com/kb/2738697): aparece el error de configuración de controlador de dominio "El servidor no es funcional" al configurar un servidor mediante el Administrador del servidor | Instalación de AD DS | Este error aparece cuando se intenta instalar AD DS en un equipo de grupo de trabajo porque la autenticación NTLM está deshabilitada. |
| [2738746](https://support.microsoft.com/kb/2738746): se muestran errores de acceso denegado después de iniciar sesión en una cuenta de dominio de administrador local | Instalación de AD DS | Cuando se inicia sesión con una cuenta de administrador local en lugar de con la cuenta predefinida de administrador y se crea un dominio, la cuenta no se agrega al grupo Admins. del dominio. |
| [2743345](https://support.microsoft.com/kb/2743345): aparece el error de Adprep/ gpprep "No se puede encontrar el archivo especificado" o la herramienta se bloquea | Instalación de AD DS | Este error aparece al ejecutar adprep/gpprep debido a que el maestro de infraestructura ha implementado un espacio de nombres no contiguo. |
| [2743367](https://support.microsoft.com/kb/2743367): aparece el error de Adprep "no es una aplicación Win32 válida" en la versión de 64 bits de Windows Server 2003 | Instalación de AD DS | Este error aparece porque la herramienta Adprep de Windows Server 2012 no se puede ejecutar en Windows Server 2003. |
| [2753560](https://support.microsoft.com/kb/2753560): aparecen errores de instalación de ADMT 3.2 y PES 3.1 en Windows Server 2012 | ADMT | ADMT 3.2 no se puede instalar en Windows Server 2012 por cuestiones de diseño. |
| [2750857](https://support.microsoft.com/kb/2750857): los informes de diagnóstico de Replicación DFS no se muestran correctamente en Internet Explorer 10 | Replicación DFS | Los informes de diagnóstico de Replicación DFS no se ven bien debido a los cambios introducidos en Internet Explorer 10. |
| [2741537](https://support.microsoft.com/kb/2741537): las actualizaciones de directiva de grupo remota son visibles para los usuarios | Directiva de grupo | La causa es que las tareas programadas se ejecutan en el contexto de cada usuario que ha iniciado sesión. El diseño del Programador de tareas de Windows requiere una solicitud interactiva en este escenario. |
| [2741591](https://support.microsoft.com/kb/2741591): no existen archivos ADM en SYSVOL en la opción de estado de infraestructura de GPMC | Directiva de grupo | La replicación de GP puede informar de la "replicación en curso" porque el estado de infraestructura de GPMC no sigue las reglas de filtrado personalizadas. |
| [2737880](https://support.microsoft.com/kb/2737880): aparece el error "No se puede iniciar el servicio" durante la configuración de AD DS | Clonación de controladores de dominio virtuales | Este error se muestra al instalar o quitar AD DS, o al realizar una clonación, debido a que el servicio de servidor de roles de DS está deshabilitado. |
| [2742836](https://support.microsoft.com/kb/2742836): se crean dos concesiones DHCP por cada controlador de dominio cuando se usa la característica de clonación de controladores de dominio virtuales | Clonación de controladores de dominio virtuales | Esto sucede porque el controlador de dominio clonado ha recibido una concesión antes de la clonación y otra después de que la clonación haya finalizado. |
| [2742844](https://support.microsoft.com/kb/2742844): la clonación del controlador de dominio genera un error y el servidor se reinicia en DSRM en Windows Server 2012 | Clonación de controladores de dominio virtuales | El controlador de dominio clonado se inicia en DSRM porque la clonación ha dado error por una serie de razones, recogidas en el artículo de KB. |
| [2742874](https://support.microsoft.com/kb/2742874): la clonación del controlador de dominio no vuelve a crear todos los nombres de entidades de seguridad de servicio | Clonación de controladores de dominio virtuales | Algunos nombres principales de servicio compuestos de tres partes no se vuelven a crear en el controlador de dominio clonado debido a una limitación dentro del proceso de cambio de nombre del dominio. |
| [2742908](https://support.microsoft.com/kb/2742908): tras clonar un controlador de dominio, aparece un error que indica que no hay servidores de inicio de sesión disponibles | Clonación de controladores de dominio virtuales | Este error aparece cuando se intenta iniciar sesión después de clonar un controlador de dominio virtualizado debido a que la clonación ha dado error y el controlador de dominio se inicia en DSRM. Inicie sesión como .\administrador para solucionar el problema de clonación. |
| [2742916](https://support.microsoft.com/kb/2742916): la clonación del controlador de dominio registra un error 8610 en dcpromo.log | Clonación de controladores de dominio virtuales | La clonación no se lleva a cabo porque el emulador de controlador de dominio principal no ha efectuado una replicación entrante en la partición del dominio, probablemente porque el rol se haya transferido. |
| [2742927](https://support.microsoft.com/kb/2742927): aparece el error de New-AdDcCloneConfig "El índice está fuera del intervalo" | Clonación de controladores de dominio virtuales | Este error aparece después de ejecutar el cmdlet New-ADDCCloneConfigFile mientras se clonan controladores de dominio virtuales, ya sea porque el cmdlet no se ejecutó desde un símbolo del sistema con privilegios elevados o porque el token de acceso no contiene el grupo Administradores. |
| [2742959](https://support.microsoft.com/kb/2742959): la clonación del controlador de dominio genera el error 8437 "Se especificó un parámetro no válido en esta operación de replicación" | Clonación de controladores de dominio virtuales | La clonación no se produce porque se ha especificado un nombre de clon no válido o un nombre NetBIOS duplicado. |
| [2742970](https://support.microsoft.com/kb/2742970): la clonación del controlador de dominio da un error sin DSRM, un origen duplicado y equipo de clonación | Clonación de controladores de dominio virtuales | El controlador de dominio virtual clonado se inicia en Modo de reparación de servicios de directorio (DSRM) con un nombre duplicado igual al del controlador de dominio de origen porque el archivo DCCloneConfig.xml no se creó en la ubicación adecuada o porque el controlador de dominio de origen se reinició antes de la clonación. |
| [2743278](https://support.microsoft.com/kb/2743278): error de clonación de controlador de dominio 0x80041005 | Clonación de controladores de dominio virtuales | El controlador de dominio clonado se inicia en DSRM porque solo se ha especificado un servidor WINS. Si se especifica un servidor WINS, también deberán especificarse los servidores WINS preferido y alternativo correspondientes. |
| [2745013](https://support.microsoft.com/kb/2745013): se muestra el mensaje de error "El servidor no es funcional" si se ejecuta New-AdDcCloneConfigFile en Windows Server 2012 | Clonación de controladores de dominio virtuales | Este error aparece al ejecutar el cmdlet New-ADDCCloneConfigFile debido a que el servidor no puede ponerse en contacto con un servidor de catálogo global. |
| [2747974](https://support.microsoft.com/kb/2747974): el evento 2224 de clonación del controlador de dominio proporciona instrucciones incorrectas | Clonación de controladores de dominio virtuales | El identificador de evento 2224 indica erróneamente que las cuentas de servicio administradas se deben quitar antes de proceder a la clonación. Las cuentas de servicio administradas independientes se deben eliminar, pero las de grupo no impiden la clonación. |
| [2748266](https://support.microsoft.com/kb/2748266): tras actualizar a Windows 8, no se puede desbloquear una unidad cifrada con BitLocker | BitLocker | Recibirá un error de "aplicación no encontrada" al intentar desbloquear una unidad en un equipo que se ha actualizado desde Windows 7. |

## <a name="see-also"></a>Consulte también

Recursos de evaluación de [Windows Server 2012](https://www.microsoft.com/en-us/evalcenter/) 
 Guía de evaluación de [Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf) 
 [Instalación e implementación de Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831620(v=ws.11))
