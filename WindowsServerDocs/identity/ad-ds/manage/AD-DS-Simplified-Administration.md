---
description: 'Más información acerca de: AD DS la administración simplificada'
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: Administración simplificada de AD DS
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: 680664d1ef24e714b86661d5fb334b82250137f5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043113"
---
# <a name="ad-ds-simplified-administration"></a>Administración simplificada de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explican las capacidades y ventajas de la implementación y administración de controladores de dominio de Windows Server 2012, así como las diferencias entre la implementación del controlador de dominio del sistema operativo anterior y la nueva implementación de Windows Server 2012.

Windows Server 2012 presentó la próxima generación de Active Directory Domain Services administración simplificada y era la reutilización más radical del dominio desde el servidor Windows 2000. La Administración simplificada de AD DS se ha inspirado en lo aprendido a lo largo de doce años de Active Directory para crear una experiencia de administración más compatible, flexible e intuitiva para arquitectos y administradores. Esto ha conllevado la creación de nuevas versiones de las tecnologías existentes y la ampliación de las capacidades de los componentes incluidos en Windows Server 2008 R2.

La Administración simplificada de AD DS supone un nuevo concepto de implementación de dominio.

- La implementación de roles de AD DS ahora forma parte de la nueva arquitectura de Administrador del servidor y permite la instalación remota
- El motor de implementación y configuración de AD DS es ahora Windows PowerShell, incluso cuando se utiliza el nuevo asistente de configuración de AD DS
- La extensión de esquema, la preparación de bosques y la preparación de dominios forman parte automáticamente de la promoción de controladores de dominio y ya no requieren tareas separadas en servidores especiales, como el maestro de esquema
- La promoción ahora incluye la comprobación de requisitos previos, que valida la disponibilidad del bosque y del dominio para el nuevo controlador de dominio, de modo que se reduce la posibilidad de promociones erróneas
- El módulo de Active Directory para Windows PowerShell ahora incluye cmdlets para la administración de la topología de replicación, el control de acceso dinámico y otras operaciones
- El nivel funcional de bosque de Windows Server 2012 no implementa nuevas características, y el nivel funcional de dominio se requiere únicamente para un subconjunto de características nuevas de Kerberos, lo que exime a los administradores de la necesidad frecuente de un entorno de controlador de dominio homogéneo
- Se ha añadido compatibilidad completa con controladores de dominio virtualizados, para incluir la implementación automatizada y la protección de la reversión
   - Para obtener más información acerca de los controladores de dominio virtualizados, consulte la [Introducción a la &#40;de Active Directory Domain Services AD DS&#41; virtualización &#40;nivel 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).

Además, se han introducido numerosas mejoras administrativas y de mantenimiento:

- El Centro de administración de Active Directory incluye una papelera de reciclaje gráfica de Active Directory, la administración de la directiva de contraseña específica y el visor del historial de Windows PowerShell
- El nuevo Administrador del servidor tiene interfaces específicas de AD DS para la supervisión del rendimiento, análisis de procedimientos recomendados, servicios críticos y registros de eventos
- Las cuentas de servicio administradas de grupo son compatibles con varios equipos que utilicen las mismas entidades de seguridad
- Las mejoras en la emisión y la supervisión de los identificadores relativos (RID) mejoran la capacidad de administración en dominios desarrollados de Active Directory

AD DS beneficios de otras características nuevas incluidas en Windows Server 2012, como:

- Equipos NIC y protocolo de puente del centro de datos
- Seguridad de DNS y disponibilidad de zona integrada en AD más rápida tras el arranque
- Mejoras en la fiabilidad y la escalabilidad de Hyper-V
- Desbloqueo de BitLocker en red
- Más módulos de administración de componentes de Windows PowerShell

## <a name="adprep-integration"></a>Integración de ADPREP

La extensión de esquema de bosque y la preparación de dominios de Active Directory están ahora integradas en el proceso de configuración del controlador de dominio. Si promueves un nuevo controlador de dominio en un bosque existente, el proceso detecta el estado de actualización, y las fases de extensión de esquema y preparación de dominios se producen automáticamente. El usuario que instale el primer controlador de dominio de Windows Server 2012 debe ser Administrador de organización y Administrador de esquema, o bien proporcionar credenciales alternativas válidas.

Adprep.exe permanece en el DVD para la preparación independiente de bosques y de dominios. La versión de la herramienta incluida en Windows Server 2012 es compatible con las versiones anteriores (Windows Server 2008 x64 y Windows Server 2008 R2). Adprep.exe también es compatible con forestprep y domainprep remotos, igual que las herramientas de configuración de controladores de dominio basadas en ADDSDeployment.

Para obtener más información sobre Adprep y la preparación de bosques de sistemas operativos anteriores, consulte [Ejecutar Adprep (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)).

## <a name="server-manager-ad-ds-integration"></a>Integración de AD DS con el Administrador del servidor

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)

El Administrador del servidor actúa como concentrador de las tareas de administración del servidor. Su aspecto semejante a un panel actualiza periódicamente las vistas de los roles instalados y de los grupos de servidores remotos. El Administrador del servidor permite una administración centralizada de los servidores locales y remotos, sin necesidad de acceder mediante la consola.

Active Directory Domain Services es uno de esos roles de concentrador; al ejecutar Administrador del servidor en un controlador de dominio o en el Herramientas de administración remota del servidor en Windows 8, verá problemas importantes recientes en los controladores de dominio del bosque.

Estas vistas incluyen:

- Disponibilidad del servidor
- Alertas del monitor de rendimiento por un elevado uso de CPU y memoria
- Estado de los servicios de Windows específicos de AD DS
- Entradas recientes en el registro de eventos con advertencias y errores relacionados con los servicios de directorio
- Análisis de los procedimientos recomendados de un controlador de dominio comparados con una serie de reglas recomendadas por Microsoft

## <a name="active-directory-administrative-center-recycle-bin"></a>Papelera de reciclaje del Centro de administración de Active Directory

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)

Windows Server 2008 R2 introdujo la papelera de reciclaje de Active Directory, que recupera objetos eliminados de Active Directory sin restaurarlos desde una copia de seguridad, reiniciar el servicio AD DS o reiniciar los controladores de dominio.

Windows Server 2012 mejora las capacidades existentes de restauración basadas en Windows PowerShell con una nueva interfaz gráfica en el Centro de administración de Active Directory. Esto permite a los administradores habilitar la papelera de reciclaje y ubicar o restaurar objetos eliminados en los contextos de dominio del bosque, sin ejecutar directamente cmdlets de Windows PowerShell. El Centro de administración de Active Directory y la papelera de reciclaje de Active Directory siguen usando Windows PowerShell a un nivel más profundo, por lo que los scripts y procedimientos anteriores todavía resultan valiosos.

Para obtener más información sobre la papelera de reciclaje de Active Directory, consulte la [Guía paso a paso de la papelera de reciclaje de Active Directory (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd392261(v=ws.10)).

## <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Directiva de contraseña específica del Centro de administración de Active Directory

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)

Windows Server 2008 introdujo la directiva de contraseña específica (FGPP), que permite a los administradores configurar varias directivas de contraseñas y bloqueo de cuentas por dominio. Esto ofrece a los dominios una solución flexible para aplicar reglas de contraseñas más o menos restrictivas, basadas en usuarios y grupos. No contaba con una interfaz de administración y obligaba a los administradores a realizar la configuración con Ldp.exe o Adsiedit.msc. Windows Server 2008 R2 introdujo el módulo de Active Directory para Windows PowerShell, que ofrecía a los administradores una interfaz de línea de comandos para FGPP.

Windows Server 2012 aporta una interfaz gráfica para la directiva de contraseña específica. El Centro de administración de Active Directory es donde se encuentra este nuevo cuadro de diálogo, que ofrece una administración de FGPP simplificada para todos los administradores.

Para obtener más información sobre la directiva de contraseña específica, consulte la [Guía paso a paso para la configuración de directivas de bloqueo de cuenta y contraseña específica de AD DS (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770842(v=ws.10)).

## <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visor del historial de Windows PowerShell del Centro de administración de Active Directory

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)

Windows Server 2008 R2 introdujo el Centro de administración de Active Directory, que reemplazó el anterior complemento de Usuarios y equipos de Active Directory creado en Windows 2000. El Centro de administración de Active Directory ha creado una interfaz de administración gráfica en el módulo de Active Directory para Windows PowerShell, por aquel entonces nuevo.

Aunque el módulo de Active Directory contiene más de un centenar de cmdlets, el aprendizaje puede resultarle dificultoso a un administrador. Dado que Windows PowerShell se integra en gran medida en la estrategia de administración de Windows, el Centro de administración de Active Directory ahora incluye un visor que permite ver la ejecución de un cmdlet en la interfaz gráfica. Puedes buscar, copiar, borrar el historial y agregar notas con una sencilla interfaz. Lo que se pretende es que el administrador utilice la interfaz gráfica para crear y modificar objetos, y después revisarlos en el visor del historial para obtener más información sobre el scripting de Windows PowerShell y modificar los ejemplos.

## <a name="ad-replication-windows-powershell"></a>Windows PowerShell con replicación de AD

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)

Windows Server 2012 agrega más cmdlets de replicación de Active Directory al módulo de Windows PowerShell de Active Directory. Estos permiten la configuración de sitios, subredes, conexiones, vínculos a sitios y puentes nuevos o existentes. Asimismo, devuelven metadatos de replicación, estado de replicación, puesta en cola e información sobre vectores de versión de actualización de Active Directory. La introducción de los cmdlets de replicación (combinados con los cmdlets de implementación y otros cmdlets existentes de AD DS) permite administrar un bosque utilizando solo Windows PowerShell. De este modo se ofrecen nuevas oportunidades a los administradores que desean aprovisionar y administrar Windows Server 2012 sin una interfaz gráfica, lo que reduce la superficie de ataque del sistema operativo y los requisitos de mantenimiento. Esto es especialmente importante cuando se implementan servidores en redes de alta seguridad, como el Enrutador de protocolo de Internet secreto (SIPR) y DMZ corporativas.

Para obtener más información sobre la replicación y la topología de sitio de AD DS, consulte la [Referencia técnica de Windows Server](/previous-versions/windows/it-pro/windows-server-2003/cc739127(v=ws.10)).

## <a name="rid-management-and-issuance-improvements"></a>Mejoras en la emisión y la administración de RID

Active Directory de Windows 2000 introdujo el maestro RID, que emite grupos de identificadores relativos para controladores de dominio, con el fin de crear identificadores de seguridad (SID) de elementos de confianza de seguridad, como usuarios, grupos y equipos.  De manera predeterminada, este espacio global de RID se limita a 2<sup>30</sup> (o 1.073.741.823) SID totales creados en un dominio. Los SID no pueden regresar al grupo o volver a emitirse. Con el paso del tiempo, los RID podrían empezar a escasear en un dominio grande, o podrían producirse accidentes que conllevaran la disminución innecesaria de los RID y su agotamiento final.

Windows Server 2012 aborda una serie de problemas de emisión y administración de RID que descubrieron los clientes y el Servicio de soporte al cliente de Microsoft a medida que AD DS se iban desarrollando desde la creación de los primeros dominios de Active Directory en 1999. Entre ellas se incluyen las siguientes:

- Las advertencias de consumo de RID periódico se escriben en el registro de eventos
- Los eventos se registran cuando un administrador invalida un grupo de RID
- Ahora se aplica un tope máximo al tamaño de bloque de RID de la directiva de RID
- Ahora se aplican y se registran límites de RID artificiales cuando el espacio global de RID es reducido, lo que permite al administrador tomar medidas cuando se agota el espacio global
- El espacio global de RID ya puede aumentarse en un bit, lo que duplica el tamaño a 2<sup>31</sup> (2.147.483.648 SID)

Para obtener más información sobre los RID y el maestro RID, consulte [Funcionamiento de los identificadores de seguridad](/previous-versions/windows/it-pro/windows-server-2003/cc778824(v=ws.10)).

## <a name="ad-ds-role-deployment-and-management-architecture"></a>Arquitectura de administración e implementación del rol de AD DS

El Administrador del servidor y el módulo ADDSDeployment de Windows PowerShell confían en los siguientes ensamblados básicos para la funcionalidad cuando implementan o administran el rol de AD DS:

- Microsoft.ADroles.Aspects.dll
- Microsoft.ADroles.Instrumentation.dll
- Microsoft.ADRoles.ServerManager.Common.dll
- Microsoft.ADRoles.UI.Common.dll
- Microsoft.DirectoryServices.Deployment.Types.dll
- Microsoft.DirectoryServices.ServerManager.dll
- Addsdeployment.psm1
- Addsdeployment.psd1

Ambos confían en Windows PowerShell y en su Invoke-Command remoto para la instalación y la configuración remotas de roles.

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)

Windows Server 2012 también refactoriza una serie de operaciones de promoción anteriores fuera de LSASS.EXE, como parte de:

- Servicio de servidor de roles de DS (DsRoleSvc)
- DSRoleSvc.dll (cargado por el servicio DsRoleSvc)

Este servicio debe estar presente y en ejecución para promover, disminuir de nivel o clonar controladores de dominio virtuales. La instalación de roles de AD DS agrega este servicio y establece de manera predeterminada un tipo de inicio manual. No deshabilites este servicio.

## <a name="adprep-and-prerequisite-checking-architecture"></a>Arquitectura de comprobación de requisitos previos y ADPrep

Ya no es necesario ejecutar Adprep en el maestro de esquema. Puede ejecutarse remotamente desde un equipo que ejecute Windows Server 2008 x64 o versiones posteriores.

> [!NOTE]
> Adprep utiliza LDAP para importar archivos Schxx.ldf y no se vuelve a conectar automáticamente si la conexión al maestro de esquema se pierde durante la importación. Como parte del proceso de importación, el maestro de esquema se establece en un modo específico y la reconexión automática se deshabilita porque, si LDAP vuelve a conectarse después de que se haya perdido la conexión, la conexión restablecida no se encontraría en ese modo específico. En ese caso, el esquema no se actualizaría correctamente.

La comprobación de los requisitos previos garantiza que se cumplan ciertas condiciones. Dichas condiciones son necesarias para la instalación correcta de AD DS. Si no se cumplen algunas condiciones necesarias, pueden resolverse antes de continuar con la instalación. Asimismo, detecta si un bosque o un dominio todavía no están preparados, para que el código de implementación de Adprep se ejecute automáticamente.

### <a name="adprep-executables-dlls-ldfs-files"></a>Ejecutables de ADPrep, DLL, LDF, archivos

- ADprep.dll
- Ldifde.dll
- Csvde.dll
- Sch14.ldf - Sch56.ldf
- Schupgrade.cat
- *dcpromo.csv

El código para la preparación de AD anteriormente almacenado en ADprep.exe se ha refactorizado en adprep.dll. Esto permite que tanto ADPrep.exe como el módulo ADDSDeployment de Windows PowerShell usen la biblioteca para las mismas tareas y tengan las mismas capacidades. Adprep.exe se incluye en los medios de instalación, pero los procesos automatizados no lo llaman directamente; lo ejecuta manualmente un administrador. Solo puede ejecutarse en Windows Server 2008 x64 y sistemas operativos posteriores. Ldifde.exe y csvde.exe también tienen versiones refactorizadas como DLL que carga el proceso de preparación. La extensión de esquema sigue usando los archivos LDF comprobados por firma, como en las versiones de sistemas operativos anteriores.

![administración simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)

> [!IMPORTANT]
> No existe ninguna herramienta Adprep32.exe de 32 bits para Windows Server 2012. Debes tener por lo menos un equipo con Windows Server 2008 x64, Windows Server 2008 R2 o Windows Server 2012, que se ejecute como controlador de dominio, servidor miembro o en un grupo de trabajo, para preparar el bosque y el dominio. Adprep.exe no se ejecuta en Windows Server 2003 x64.

## <a name="prerequisite-checking"></a><a name="BKMK_PrereuisiteChecking"></a>Comprobación de requisitos previos

El sistema de comprobación de requisitos previos integrado en el código administrado de ADDSDeployment de Windows PowerShell funciona de maneras diferentes, en función de la operación. Las tablas incluidas a continuación describen cada prueba, cuándo se utiliza y una explicación de cómo y qué comprueba. Estas tablas pueden resultar muy útiles si se produce algún problema cuando la comprobación es incorrecta y el error no es suficiente para encontrar una solución.

Estas pruebas se registran en el canal de registro de eventos operativos **DirectoryServices-Deployment** en la categoría de tarea **Básica**, siempre con el identificador de evento **103**.

### <a name="prerequisite-windows-powershell"></a>Windows PowerShell con requisitos previos

Existen cmdlets de ADDSDeployment de Windows PowerShell para todos los cmdlets de implementación de controladores de dominio. Tienen aproximadamente los mismos argumentos que sus cmdlets asociados.

- Test-ADDSDomainControllerInstallation
- Test-ADDSDomainControllerUninstallation
- Test-ADDSDomainInstallation
- Test-ADDSForestInstallation
- Test-ADDSReadOnlyDomainControllerAccountCreation

En general, no es necesario ejecutar estos cmdlets; se ejecutan automáticamente con los cmdlets de implementación de manera predeterminada.

#### <a name="prerequisite-tests"></a><a name="BKMK_ADDSInstallPrerequisiteTests"></a>Pruebas de requisitos previos

| Nombre de la prueba | Protocolos<p>usados | Explicación y notas |
|--|--|--|
| VerifyAdminTrusted<p>ForDelegationProvider | LDAP | Comprueba que tienes el privilegio "Habilitar confianza con el equipo y las cuentas de usuario para delegación" (SeEnableDelegationPrivilege) en el controlador de dominio asociado existente. Para ello es necesario tener acceso al atributo tokenGroups construido.<p>No se utiliza al contactar con controladores de dominio de Windows Server 2003. Debes confirmar manualmente este privilegio antes de la promoción. |
| VerifyADPrep<p>Prerequisites (forest) | LDAP | Detecta y contacta con el maestro de esquema mediante el atributo namingContexts de rootDSE y el atributo fsmoRoleOwner de contexto de nombre de esquema. Determina qué operaciones de preparación (forestprep, domainprep o rodcprep) son necesarias para la instalación de AD DS. Comprueba que se espera el objectVersion de esquema y si necesita más extensión. |
| VerifyADPrep<p>Prerequisites (domain and RODC) | LDAP | Detecta y contacta con el maestro de infraestructura mediante el atributo namingContexts de rootDSE y el atributo fsmoRoleOwner de contenedor de infraestructura. En el caso de una instalación de RODC, esta prueba descubre el maestro de nomenclatura de dominios y se asegura de que está en línea. |
| CheckGroup<p>Pertenencia | LDAP,<p>RPC a través de SMB (LSARPC) | Comprueba que el usuario pertenece al grupo Administradores del dominio o Administradores de organización, en función de la operación (el primero para agregar o disminuir de nivel un controlador de dominio, el segundo para agregar o quitar un dominio). |
| CheckForestPrep<p>GroupMembership | LDAP,<p>RPC a través de SMB (LSARPC) | Comprueba que el usuario pertenece a los grupos Administradores de esquema y Administradores de organización y que tiene el privilegio Administrar registros de eventos de auditoría y seguridad (SesScurityPrivilege) en los controladores de dominio existentes. |
| CheckDomainPrep<p>GroupMembership | LDAP,<p>RPC a través de SMB (LSARPC) | Comprueba que el usuario pertenece al grupo Administradores de dominio y que tiene el privilegio Administrar registros de eventos de auditoría y seguridad (SesScurityPrivilege) en los controladores de dominio existentes. |
| CheckRODCPrep<p>GroupMembership | LDAP,<p>RPC a través de SMB (LSARPC) | Comprueba que el usuario pertenece al grupo Administradores de organización y que tiene el privilegio Administrar registros de eventos de auditoría y seguridad (SesScurityPrivilege) en los controladores de dominio existentes. |
| VerifyInitSync<p>AfterReboot | LDAP | Comprueba que el maestro de esquema se ha replicado por lo menos una vez desde que se reinició estableciendo un valor ficticio en el atributo rootDSE becomeSchemaMaster. |
| VerifySFUHotFix<p>Aplicado | LDAP | Comprueba que el esquema de bosque existente no contiene la extensión SFU2 de problema conocido para el atributo UID con el OID 1.2.840.113556.1.4.7000.187.102.<p>([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732)) |
| VerifyExchange<p>SchemaFixed | LDAP, WMI, DCOM, RPC | Validar que el esquema de bosque existente todavía no contiene las extensiones Exchange 2000 de problema MS-Exch-Assistant-Name, MS-Exch-LabeledURI y MS-Exch-House-Identifier ( [https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649) ) |
| VerifyWin2KSchema<p>Coherencia | LDAP | Comprueba que el esquema de bosque existente tiene clases y atributos básicos coherentes (no modificados incorrectamente por terceros). |
| DCPromo | DRSR a través de RPC,<p>LDAP,<p>DNS<p>RPC a través de SMB (SAMR) | Comprueba la sintaxis de línea de comandos que ha pasado al código de promoción y a la promoción de prueba. Comprueba que el bosque o dominio todavía no existe cuando se crea uno nuevo. |
| VerifyOutbound<p>ReplicationEnabled | LDAP, DRSR a través de SMB, RPC a través de SMB (LSARPC) | Comprueba que el controlador de dominio existente especificado como asociado de replicación tiene la replicación de salida habilitada mediante la comprobación del atributo de opciones del objeto de configuración NTDS para NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) |
| VerifyMachineAdmin<p>Contraseña | DRSR a través de RPC,<p>LDAP,<p>DNS<p>RPC a través de SMB (SAMR) | Comprueba que la contraseña del modo seguro establecida para DSRM cumple los requisitos de complejidad del dominio. |
| VerifySafeModePassword | *N/D* | Comprueba que la contraseña establecida para el administrador local cumple los requisitos de complejidad de la directiva de seguridad del equipo. |
