---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: "Administración simplificada de AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6232e281c47f3b5b4627bc9d8ccf53269aafc390
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-simplified-administration"></a>Administración simplificada de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica las nuevas funcionalidades y las ventajas de implementación de controlador de dominio de Windows Server 2012 y administración y las diferencias entre la implementación de sistema operativo DC anterior y la nueva implementación de Windows Server 2012.  
  
Windows Server 2012 presenta la próxima generación de Active Directory servicios simplificada administración del dominio y es el más radical dominio volver a una visión desde Windows 2000 Server. Administración simplificada de AD DS toma lecciones aprendidas de 12 años de Active Directory y hace que sea una experiencia más intuitiva administrativa más flexible, ofrece una mayor compatibilidad para los arquitectos y administradores. Esto significa crear nuevas versiones de tecnologías de existente, así como para expandir las funcionalidades de los componentes que se lanzó en Windows Server 2008 R2.  
  
Administración simplificada de AD DS es un reimagining de implementación de dominio.  
  
-   Ahora es parte de la nueva arquitectura de administrador del servidor de implementación de rol de AD DS y permite la instalación remota  
  
-   El motor de implementación y configuración de AD DS es ahora Windows PowerShell, incluso cuando usando al nuevo Asistente de configuración de AD DS  
  
-   Extensión de esquema, preparación de bosque y preparación del dominio automáticamente forman parte de la promoción del controlador de dominio y ya no requieren tareas independientes en servidores especiales, como el maestro de esquema  
  
-   Promoción ahora incluye la comprobación de requisitos previos que valida la preparación de bosque y dominio para el nuevo controlador de dominio, reducir la posibilidad de error promociones  
  
-   Módulo de Active Directory para Windows PowerShell ahora incluye cmdlets de administración de la topología de replicación, Control de acceso dinámico y otras operaciones  
  
-   El bosque de Windows Server 2012 funcional nivel implementar nuevas características y el nivel funcional del dominio se requiere solo para un subconjunto de las nuevas características de Kerberos, lo que libera los administradores de las frecuentes necesita para un entorno de controlador de dominio homogéneo  
  
-   Compatibilidad completa con agregado virtualizan los controladores de dominio, para incluir la protección de reversión e implementación automatizada  
  
Para obtener más información acerca de los controladores de dominio virtualizado, consulta [Introducción a los servicios de dominio de Active Directory & #40; AD DS & #41; Virtualización & #40; Nivel 100 & #41; ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
Además, existen muchos administrativas y las mejoras de mantenimiento:  
  
-   El centro de administración de Active Directory incluye un gráfica Active Directory Papelera de reciclaje, la administración de directivas de contraseña muy específicas y el Visor de la historia de Windows PowerShell  
  
-   El administrador del servidor nueva tiene interfaces de AD DS específicos en la supervisión del rendimiento, mejor análisis de la práctica, los servicios críticos y los registros de eventos  
  
-   Cuentas de servicio administradas de grupo admite varios equipos con la misma entidades de seguridad  
  
-   Mejoras en la emisión de identificador relativo (RID) y la supervisión de mejor capacidad de administración de dominios de Active Directory para adultos  
  
Por último, AD DS beneficios de otras nuevas características incluidas en Windows Server 2012, como:  
  
-   Puente de centros de datos y el equipo NIC  
  
-   Seguridad de DNS y la disponibilidad de zona integrada de anuncios más rápido después del arranque  
  
-   Mejoras de confiabilidad y la escalabilidad de Hyper-V  
  
-   Desbloqueo de BitLocker en red  
  
-   Módulos adicionales de administración de componente de Windows PowerShell  
  
## <a name="technical-overview"></a>Información general técnica  
  
### <a name="adprep-integration"></a>Integración de ADPREP  
Bosque esquema extensión y dominio preparación de Active Directory ahora se integra en el proceso de configuración del controlador de dominio. Si promover un nuevo controlador de dominio en un bosque existente, el proceso de detecta el estado de actualización y las fases de preparación de la extensión y dominio de esquema que se producen automáticamente. El usuario que instalar el primer controlador de dominio de Windows Server 2012 todavía debe ser un administrador de la empresa y administradores de esquema o proporcionar credenciales alternativas válidas.  
  
Adprep.exe permanece en el DVD de bosque independiente y preparación del dominio. La versión de la herramienta se incluye con Windows Server 2012 es compatible con versiones anteriores a Windows Server 2008 x64 y Windows Server 2008 R2. Adprep.exe también admite remoto forestprep y domainprep, igual que las herramientas de configuración del controlador de dominio basados en ADDSDeployment.  
  
Para obtener información sobre Adprep y preparación del bosque de sistema operativo anterior, consulta [Adprep ejecutando (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
### <a name="server-manager-ad-ds-integration"></a>Integración de administrador del servidor AD DS  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
El administrador del servidor actúa como un hub para las tareas de administración de servidor. Su apariencia panel estilo actualiza periódicamente vistas de roles instalados y grupos de servidores remotos. El administrador del servidor proporciona administración centralizada de servidores locales y remotos, sin necesidad de acceso a la consola.  
  
Los servicios de dominio de Active Directory es uno de esos roles concentrador; ejecutando el administrador del servidor en un controlador de dominio o Remote Server Administration Tools en Windows 8, verá problemas recientes importantes en controladores de dominio en el bosque.  
  
Estas vistas incluyen:  
  
-   Disponibilidad del servidor  
  
-   Alertas de monitor de rendimiento para uso elevado de CPU y memoria  
  
-   El estado de servicios de Windows específicos en AD DS  
  
-   Recientes entradas de error y advertencia relacionada con los servicios de directorio en el registro de eventos  
  
-   Mejor análisis de procedimiento de un controlador de dominio contra un conjunto de reglas recomendada por Microsoft  
  
### <a name="active-directory-administrative-center-recycle-bin"></a>Papelera de reciclaje del centro de administración de Active Directory  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 introdujo la Active Directory Papelera de reciclaje, que recupera los objetos de Active Directory eliminados sin restaurar a partir de copia de seguridad, reiniciar el servicio de AD DS o reiniciar los controladores de dominio.  
  
Windows Server 2012 mejora las capacidades de restauración basado en Windows PowerShell existente con una interfaz gráfica de nuevo en el centro de administración de Active Directory. Esto permite a los administradores habilitar la Papelera de reciclaje y busque o restaurar objetos eliminados en los contextos de dominio del bosque, todo sin ejecutar directamente los cmdlets de Windows PowerShell. El centro de administración de Active Directory y la Papelera de reciclaje de Active Directory seguir usan Windows PowerShell en segundo plano, por lo que siguen siendo valiosos procedimientos y scripts anteriores.  
  
Para obtener información acerca de Active Directory [Papelera de reciclaje, consulta la Papelera de reciclaje de Active Directory Step-by-Step guía (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Directiva de contraseña específica del centro de administración de Active Directory  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 introdujo la directiva de contraseña muy específicas, lo que permite a los administradores configurar varias directivas de bloqueo de cuenta y la contraseña por dominio. Esto permite a los dominios de una solución flexible aplicar reglas de la contraseña más o menos restrictivas, en función de los usuarios y grupos. No tenía ninguna interfaz de administración y necesarios a los administradores configurar mediante Ldp.exe o Adsiedit.msc. Windows Server 2008 R2 introdujo el módulo de Active Directory para Windows PowerShell, que se concede a los administradores de una interfaz de línea de comandos para FGPP.  
  
Windows Server 2012, se ofrece una interfaz gráfica a la directiva de contraseña muy específicas. El centro de administración de Active Directory es la página principal de este cuadro de diálogo nuevo, que ofrece administración simplificada de FGPP a todos los administradores.  
  
Para obtener información acerca de la directiva de contraseña muy específicas, consulte [contraseñas específicas de AD DS y directiva de bloqueo de cuenta Step-by-Step guía (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visor de historial de PowerShell de Windows de centro de administración de Active Directory  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 introdujo el centro de administración de Active Directory, que reemplazados anteriores usuarios de Active Directory y equipos complemento creada en Windows 2000. El centro de administración de Active Directory crea una interfaz gráfica administrativa para el módulo de Active Directory, a continuación, nuevo para Windows PowerShell.  
  
Mientras que contiene el módulo de Active Directory sobre los cmdlets de cien, la curva de aprendizaje para que un administrador puede ser pendiente. Dado que Windows PowerShell se integra en gran medida en la estrategia de administración de Windows, el centro de administración de Active Directory ahora incluye un visor que te permite ver la ejecución de cmdlet en la interfaz gráfica. Puede buscar, copiar, borrar el historial y agregar notas con una interfaz sencilla. El objetivo es un administrador puede usar la interfaz gráfica para crear y modificar objetos y revisarlas, a continuación, en el Visor de historial para obtener más información acerca de scripting de Windows PowerShell y modificar los ejemplos.  
  
### <a name="ad-replication-windows-powershell"></a>Windows PowerShell de replicación de AD  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 agrega más cmdlets de réplica de Active Directory para el módulo de Active Directory de Windows PowerShell. Estos permiten la configuración de sitios nuevos o existentes, subredes, conexiones, vínculos a sitios y puentes. También devuelven metadatos de replicación Active Directory, los Queue Server, estado de replicación y la información de vector de versión de actualización. La introducción de los cmdlets de replicación - combinados con la implementación y otros cmdlets de AD DS existente - hace posible administrar un bosque mediante Windows PowerShell por sí solo. Esto crea nuevas oportunidades para los administradores que deseen aprovisionar y administrar Windows Server 2012 sin una interfaz gráfica, lo que, a continuación, reduce la superficie de ataque del sistema operativo y el mantenimiento requisitos. Esto es especialmente importante cuando la implementación de servidores en redes de alta seguridad como enrutador de protocolo de Internet (SIPR) de la clave secreta y DMZ corporativa.  
  
Para obtener más información sobre la topología de sitios de AD DS y replicación, consulta el [Windows Server Technical Reference](https://technet.microsoft.com/library/cc739127(WS.10).aspx).  
  
### <a name="rid-management-and-issuance-improvements"></a>Administración de DESHACERSE y mejoras de emisión  
Active Directory de Windows 2000 introdujo al patrón de eliminar, qué grupos de problemas de identificadores relativos a controladores de dominio para crear los identificadores de seguridad (SID) de confianza de seguridad, como los usuarios, grupos y equipos.  De manera predeterminada, este espacio RID global se limitan a 2<sup>30</sup> (o 1.073.741.823) SID total crearon en un dominio. SID no pueden devolver al grupo o ejecute de nuevo. Con el tiempo, puede comenzar a un dominio grande para que se agotarse RID o accidentes pueden producir agotamiento de RID innecesario y agotamiento final.  
  
Un número de problemas de emisión y administración de RID detectados por los clientes y soporte técnico al cliente de Microsoft como de AD DS evolucionaban desde la creación de los dominios de Active Directory primera en 1999 se ocupa de Windows Server 2012. Estos incluyen:  
  
-   Advertencias de consumo RID periódicas se escriben en el registro de eventos  
  
-   Registro de eventos cuando un administrador invalida un conjunto de RID  
  
-   Un límite máximo de la directiva RID que ahora se aplica el tamaño de bloque de eliminar  
  
-   Los límites máximos RID artificiales ahora se aplican y se registran cuando el espacio global de RID es bajo, permitiendo a un administrador realizar una acción antes de que se agote el espacio global  
  
-   Ahora se puede aumentar el espacio global de RID un bit, duplica el tamaño en 2<sup>31</sup> (2.147.483.648 SID)  
  
Para obtener más información sobre RID y el patrón de eliminar, revisar [cómo funcionan los identificadores de seguridad](https://technet.microsoft.com/library/cc778824(WS.10).aspx).  
  
## <a name="new-ad-ds-deployment-architecture"></a>Arquitectura de implementación de nuevas AD DS  
  
### <a name="ad-ds-role-deployment-and-management-architecture"></a>AD DS rol implementación y arquitectura de administración  
El administrador del servidor y ADDSDeployment Windows PowerShell se basan en los ensamblados principales siguientes para la funcionalidad que al implementar o administrar el rol de AD DS:  
  
-   Microsoft.ADroles.Aspects.dll  
  
-   Microsoft.ADroles.Instrumentation.dll  
  
-   Microsoft.ADRoles.ServerManager.Common.dll  
  
-   Microsoft.ADRoles.UI. Common.dll  
  
-   Microsoft.DirectoryServices.Deployment.Types.dll  
  
-   Microsoft.DirectoryServices.ServerManager.dll  
  
-   Addsdeployment.psm1  
  
-   Addsdeployment.psd1  
  
Ambos se basan en Windows PowerShell y sus comandos invocar remoto para la instalación de rol remoto y configuración.  
  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  
  
Windows Server 2012 refactoriza también un número de operaciones de promoción anteriores fuera LSASS.EXE, como parte de:  
  
-   Servicio de rol DS (DsRoleSvc)  
  
-   DSRoleSvc.dll (cargado por el servicio de DsRoleSvc)  
  
Este servicio debe estar presente y en ejecución para promocionar, degradar o clonar controladores de dominio virtual. Instalación del rol de AD DS agrega este servicio y establece un tipo de inicio del Manual, de manera predeterminada. No deshabilite este servicio.  
  
### <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep y arquitectura de comprobación de requisitos previos  
Adprep requiere ya no se ejecuta en el maestro de esquema. Se puede ejecutar de forma remota desde un equipo que ejecute Windows Server 2008 x64 o posterior.  
  
> [!NOTE]  
> Adprep utiliza LDAP para importar archivos Schxx.ldf y no volver a conectar automáticamente si se pierde la conexión con el maestro de esquema durante la importación. Como parte del proceso de importación, el maestro de esquema se establece en un modo determinado y está deshabilitada la reconexión automática porque si vuelve a conectar LDAP después de que se pierde la conexión, no sería la conexión de volver a establecer en el modo específico. En ese caso, el esquema no se actualizará correctamente.  
  
Comprobación de requisitos previos garantiza que ciertas condiciones es true. Estas condiciones son necesarias para la instalación correcta de AD DS. Si no se cumplen algunas condiciones necesarias, puede resolver antes de continuar con la instalación. También detecta que un dominio o bosque no están preparados aún, para que el código de implementación Adprep se ejecuta automáticamente.  
  
#### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep Executables, archivos DLL, LDFs,  
  
-   ADprep.dll  
  
-   Ldifde.dll  
  
-   Csvde.dll  
  
-   Sch14.ldf - Sch56.ldf  
  
-   Schupgrade.cat  
  
-   *Dcpromo.csv  
  
Se refactoriza el código de preparación de AD alojado anteriormente en ADprep.exe en adprep.dll. Esto permite ADPrep.exe y el módulo de PowerShell de Windows ADDSDeployment usar la biblioteca de las mismas tareas y tienen las mismas capacidades. Se incluye con los medios de instalación Adprep.exe pero procesos automatizados no la llames directamente: solo un administrador ejecuta manualmente. Solo se puede ejecutar en Windows Server 2008 x64 y sistemas operativos posteriores. Ldifde.exe y csvde.exe también han refactorizar versiones como archivos DLL que se cargan en el proceso de preparación. Extensión de esquema todavía usa los archivos LDF-comprobar la firma, como en versiones anteriores del sistema operativo.  
  
![Administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> No hay ninguna herramienta Adprep32.exe de 32 bits de Windows Server 2012. Debes tener al menos un equipo de Windows Server 2012, ejecuta como un controlador de dominio, un servidor miembro o en un grupo de trabajo para preparar el bosque y dominio, Windows Server 2008 R2 o Windows Server 2008 x64. Adprep.exe no se ejecuta en Windows Server 2003 x64.  
  
#### <a name="BKMK_PrereuisiteChecking"></a>Comprobación de requisitos previos  
El requisito previo comprobar sistema integrado en código administrado ADDSDeployment Windows PowerShell funciona en diferentes modos, en función de la operación. Las tablas siguientes describen cada prueba, cuando se utiliza y una explicación de cómo y lo valida. Estas tablas pueden ser útiles si hay problemas donde se produce un error en la validación y el error no es suficiente para solucionar el problema.  
  
Estas pruebas iniciar sesión el **DirectoryServices implementación** canal de registro operativo de eventos en la categoría de la tarea **Core**, siempre que el identificador de evento **103**.  
  
##### <a name="prerequisite-windows-powershell"></a>Requisitos previos Windows PowerShell  
Hay cmdlets de PowerShell de Windows de ADDSDeployment para todos los cmdlets de implementación del controlador de dominio. Tienen aproximadamente los mismos argumentos que sus cmdlets asociados.  
  
-   Test-ADDSDomainControllerInstallation  
  
-   Test-ADDSDomainControllerUninstallation  
  
-   Test-ADDSDomainInstallation  
  
-   Test-ADDSForestInstallation  
  
-   Test-ADDSReadOnlyDomainControllerAccountCreation  
  
No es necesario para ejecutar estos cmdlets, normalmente; ya automáticamente se ejecuten con los cmdlets de implementación de manera predeterminada.  
  
##### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>Requisitos previos pruebas  
  
||||  
|-|-|-|  
|Nombre de la prueba|Protocolos<br /><br />usa|Explicación y notas|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|Valida que tienes la opción "Habilitar cuentas de usuario y el equipo de confianza para delegación" privilegios (SeEnableDelegationPrivilege) en el controlador de dominio existente asociado. Esto requiere acceso a su atributo tokenGroups construido.<br /><br />No se usa cuando se ponga en contacto con los controladores de dominio de Windows Server 2003. Debe confirmar manualmente este privilegio antes de promoción|  
|VerifyADPrep<br /><br />Requisitos previos (bosque)|LDAP|Descubre y se pone en contacto con el rootDSE namingContexts atributos y esquema nomenclatura contexto fsmoRoleOwner el maestro de esquema. Determina las preparatorias operaciones (forestprep, domainprep o rodcprep) son necesarios para la instalación de AD DS. Valida el esquema que se espera objectVersion y si requiere más de extensión.|  
|VerifyADPrep<br /><br />Requisitos previos (dominio y RODC)|LDAP|Descubre y se pone en contacto con el patrón de infraestructura con el atributo de namingContexts rootDSE y el atributo de fsmoRoleOwner del contenedor de infraestructura. En el caso de una instalación RODC, esta prueba detecta al maestro de nombres de dominio y asegúrate de está en línea.|  
|CheckGroup<br /><br />Suscripción|LDAP,<br /><br />RPC sobre SMB (LSARPC)|Validar el usuario es miembro del grupo Administradores de dominio o administradores de empresa, dependiendo de la operación (DA para agregar o degradar un controlador de dominio, EA para agregar o quitar un dominio)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sobre SMB (LSARPC)|Validar el usuario es un miembro del grupo Administradores de esquema de grupos de administradores de empresa y tiene la auditoría administrar y registros de eventos de seguridad (SesScurityPrivilege) de privilegios en los controladores de dominio existente|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sobre SMB (LSARPC)|Comprobar si el usuario es un miembro del grupo de administradores de dominio y tiene la auditoría administrar y registros de eventos de seguridad (SesScurityPrivilege) de privilegios en los controladores de dominio existente|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sobre SMB (LSARPC)|Comprobar si el usuario es un miembro del grupo de administradores de empresa y tiene la auditoría administrar y registros de eventos de seguridad (SesScurityPrivilege) de privilegios en los controladores de dominio existente|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|Validar que el maestro de esquema se replica al menos una vez desde que reinició estableciendo un valor ficticio en rootDSE atributo becomeSchemaMaster|  
|VerifySFUHotFix<br /><br />Aplica|LDAP|Validar el bosque existente esquema carecerá de extensión de SFU2 problema conocido del atributo UID con OID 1.2.840.113556.1.4.7000.187.102<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP, WMI, DCOM, RPC|Validar el bosque existente esquema no contener problema Exchange 2000 extensiones ms-t.-Ayudante-nombre, ms-t.-LabeledURI y ms-t.-House-Identifier ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />Coherencia|LDAP|Validar el bosque existente tiene coherente de esquema (no incorrectamente modificado por un tercero) atributos y las clases principales.|  
|DCPromo|DRSR sobre RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC sobre SMB (: SAMR)|Validar la sintaxis de línea de comandos que se pasa a la promoción de prueba y el código de promoción. Validar el bosque o dominio ya no existe si creando un nuevo.|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP, DRSR en SMB, RPC sobre SMB (LSARPC)|Validar el controlador de dominio existente especificado como el asociado de replicación tiene replicación saliente habilitada comprobando el configuración NTDS atributo del objeto opciones para NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0 x 00000004)|  
|VerifyMachineAdmin<br /><br />Contraseña|DRSR sobre RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC sobre SMB (: SAMR)|Validar la contraseña de modo seguro establecer para DSRM cumple los requisitos de complejidad de dominio.|  
|VerifySafeModePassword|*N/D*|Validar los local Administrador contraseña conjunto cumple equipo seguridad Directiva requisitos de complejidad.|  
  


