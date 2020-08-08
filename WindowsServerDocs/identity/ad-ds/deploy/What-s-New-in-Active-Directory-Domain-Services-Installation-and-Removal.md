---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: Novedades sobre la instalación y eliminación de Active Directory Domain Services (AD DS)
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: 7444fcc6807e43192e68c006dcd49464a503976b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953302"
---
# <a name="whats-new-in-active-directory-domain-services-installation-and-removal"></a>Novedades sobre la instalación y eliminación de Active Directory Domain Services (AD DS)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La implementación de Active Directory Domain Services (AD DS) en Windows Server 2012 es más sencilla y más rápida que las versiones anteriores de Windows Server. El proceso de instalación de AD DS ahora se genera en Windows PowerShell y está integrado con Administrador del servidor. Se reduce el número de pasos necesarios para introducir controladores de dominio en un entorno de Active Directory. De este modo, el proceso de creación de un nuevo entorno de Active Directory resulta más sencillo y más eficiente. El nuevo proceso de implementación de AD DS minimiza las posibilidades de error que podrían bloquear a la instalación.

Además, puede instalar los binarios de rol del servidor de AD DS (es decir, el rol del servidor de AD DS) en varios servidores al mismo tiempo. También puede ejecutar el asistente para la instalación de AD DS de forma remota en un servidor individual. Estas mejoras proporcionan más flexibilidad para implementar controladores de dominio que ejecutan Windows Server 2012, especialmente para implementaciones globales a gran escala donde es necesario implementar muchos controladores de dominio en oficinas de diferentes regiones.

En la instalación de AD DS se incluyen las siguientes características:

- **Integración de adprep.exe en el proceso de instalación de AD DS.** Los pasos engorrosos y necesarios para preparar un Active Directory ya existente, tales como la necesidad de usar varias credenciales diferentes, copiar los archivos adprep.exe o iniciar sesión en controladores de dominio específicos, se simplifican o suceden automáticamente. De este modo, se reduce el tiempo necesario para instalar AD DS y disminuyen las posibilidades de errores que podrían bloquear la promoción del controlador de dominio.

   Para entornos donde es preferible ejecutar comandos adprep.exe antes de la instalación de un nuevo controlador de dominio, aún puede ejecutar comandos adprep.exe separadamente de la instalación de AD DS. La versión 2012 de Windows Server de adprep.exe se ejecuta de forma remota, por lo que puede ejecutar todos los comandos necesarios desde un servidor que ejecute una versión de 64 bits de Windows Server 2008 o posterior.

- **La nueva instalación de AD DS se genera en Windows PowerShell y se puede invocar remotamente.** La nueva instalación de AD DS está integrada con Administrador del servidor, de modo que, para instalar AD DS, se puede utilizar la misma interfaz que se utiliza para instalar otros roles del servidor. Para los usuarios de Windows PowerShell, los cmdlets de implementación de AD DS proporcionan mayor funcionalidad y flexibilidad. Existe paridad funcional entre las opciones de instalación de GUI y la línea de comandos.
- **La nueva instalación de AD DS incluye la validación de requisitos previos.** Los posibles errores se identifican antes de que comience la instalación. Se pueden corregir los errores antes de que ocurran y evitar así los riesgos de una actualización parcialmente completada. Por ejemplo, si es necesario ejecutar adprep /domainprep, el asistente para la instalación comprueba que el usuario tenga derechos suficientes como para ejecutar la operación.
- **Las páginas de configuración se agrupan en una secuencia que refleja los requisitos de las opciones de promoción más comunes con las opciones relacionadas agrupadas en menos páginas de asistente.** Esto ofrece un mejor contexto para tomar decisiones de instalación.
- **Puede exportar un script de Windows PowerShell que contenga todas las opciones que se especificaron durante la instalación gráfica.** Al final de la instalación o eliminación, puede exportar la configuración a un script de Windows PowerShell para su uso con la automatización de la misma operación.
- **Solo la replicación crítica se produce antes del reinicio.** El nuevo conmutador permite la replicación de datos no críticos antes del reinicio. Para obtener más información, vea [Argumentos del cmdlet ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).

## <a name="the-active-directory-domain-services-configuration-wizard"></a><a name="BKMK_ADConfigurationWizard"></a>El Asistente para configuración de Servicios de dominio de Active Directory

A partir de Windows Server 2012, el Asistente para configuración de Active Directory Domain Services reemplaza el Asistente para instalación de Active Directory Domain Services heredado como la opción de interfaz de usuario (UI) para especificar la configuración cuando se instala un controlador de dominio. El Asistente para configuración de Servicios de dominio de Active Directory comienza después de que el Asistente para agregar roles ha terminado.

> [!WARNING]
> El Asistente para instalación de Active Directory Domain Services heredado (dcpromo.exe) está en desuso a partir de Windows Server 2012.

En [instalar Active Directory Domain Services &#40;de nivel 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), los procedimientos de la interfaz de usuario muestran cómo iniciar el Asistente para agregar roles para instalar los archivos binarios del rol AD DS Server y, a continuación, ejecutar el Asistente para configuración de Active Directory Domain Services para completar la instalación del controlador de dominio. En los ejemplos de Windows PowerShell se muestra cómo completar ambos pasos con un cmdlet de implementación de AD DS.

## <a name="adprepexe-integration"></a><a name="BKMK_NewAdprep"></a>Integración de adprep.exe

A partir de Windows Server 2012, solo hay una versión de Adprep.exe (no hay ninguna versión de 32 bits, adprep32.exe). Los comandos adprep se ejecutan automáticamente según sea necesario al instalar un controlador de dominio que ejecuta Windows Server 2012 en un dominio o bosque de Active Directory existente.

A pesar de que las operaciones adprep se ejecutan automáticamente, puede ejecutar Adprep.exe por separado. Por ejemplo, si el usuario que instala AD DS no es miembro del grupo Administradores de empresas, lo cual es necesario para poder ejecutar Adprep /forestprep, entonces quizás necesite ejecutar el comando por separado. No obstante, solo tiene que ejecutar adprep.exe si planea actualizar en contexto el primer controlador de dominio de Windows Server 2012 (es decir, si planea actualizar en contexto el sistema operativo de un controlador de dominio que ejecuta Windows Server 2012).

Adprep.exe se encuentra en la carpeta \support\adprep del disco de instalación de Windows Server 2012. La versión 2012 de Windows Server de Adprep es capaz de ejecutarse de forma remota.

La versión 2012 de Windows Server de adprep.exe puede ejecutarse en cualquier servidor que ejecute una versión de 64 bits de Windows Server 2008 o posterior. El servidor necesita conectividad de red al maestro de esquema para el bosque y el maestro de infraestructura del dominio donde quiere agregar un controlador de dominio. Si alguno de estos roles se hospeda en un servidor que ejecuta Windows Server 2003, adprep se deberá ejecutar remotamente. No es necesario que el servidor donde ejecuta adprep sea un controlador de dominio. Puede estar unido al dominio o en un grupo de trabajo.

> [!NOTE]
> Si intenta ejecutar la versión 2012 de Windows Server de adprep.exe en un servidor que ejecuta Windows Server 2003, aparece el siguiente error:
>
> Adprep.exe no es una aplicación Win32 válida.

![Novedades](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)

Para obtener más información sobre cómo resolver otros errores devueltos por Adprep.exe, consulte [Problemas conocidos](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).

### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Comprobación de pertenencia a grupos con los roles de maestro de operaciones de Windows Server 2003

Para cada comando (/forestprep, /domainprep o /rodcprep), Adprep realiza una comprobación de la pertenencia a grupos para determinar si las credenciales especificadas representan una cuenta en algunos grupos. Para llevar a cabo esta comprobación, Adprep se pone en contacto con el propietario del rol de maestro de operaciones. Si el maestro de operaciones está ejecutando Windows Server 2003, se deben especificar los parámetros de la línea de comandos /user y /userdomain si se ejecuta Adprep.exe para asegurarse de que la comprobación de la pertenencia a grupos se realice en todas las clases.

Los parámetros/User y/userdomain son nuevos para Adprep.exe en Windows Server 2012. Estos parámetros especifican el nombre de cuenta de usuario y el dominio de usuario, respectivamente, del usuario que ejecuta el comando adprep. La utilidad de la línea de comandos Adprep.exe bloquea y especifica uno de /userdomain y /user pero omite el otro.

Sin embargo, las operaciones Adprep también se pueden ejecutar como parte de una instalación de AD DS con Windows PowerShell o Administrador del servidor. Esas experiencias comparten la misma implementación subyacente (adprep.dll) como adprep.exe. Las experiencias de Windows PowerShell y Administrador del servidor tienen entradas de credenciales separadas, lo cual no impone los mismos requisitos que adprep.exe. Con Windows PowerShell o Administrador del servidor, es posible pasar un valor para /user pero no para /userdomain a adprep.dll. Si se especifica/User pero no se especifica/userdomain, se usa el dominio del equipo local para realizar la comprobación. Si el equipo no está unido al dominio, no se puede comprobar la pertenencia a grupos.

Cuando la pertenencia a grupos no se puede comprobar, Adprep muestra un mensaje de advertencia en los archivos de registro de adprep y continúa:

```
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.
```

Si ejecuta Adprep.exe sin especificar los parámetros /user y /userdomain y el maestro de operaciones está ejecutando Windows Server 2003, Adprep.exe se pone en contacto con un controlador de dominio en el dominio del usuario conectado actualmente. Si el usuario conectado actualmente no es una cuenta de dominio, Adprep.exe no puede realizar la comprobación de pertenencia a grupos. Adprep.exe tampoco puede realizar la comprobación de pertenencia a grupos si se usan credenciales de tarjeta inteligente, incluso aunque se especifiquen /user y /userdomain.

Si Adprep finaliza correctamente, no se requiere ninguna acción. Si se producen errores de acceso en Adprep durante la ejecución, proporcione una cuenta con la pertenencia correcta. Para obtener más información, consulte [Requisitos de credenciales para ejecutar Adprep.exe e instalar Active Directory Domain Services](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).

### <a name="syntax-for-adprep-in-windows-server-2012"></a>Sintaxis para Adprep en Windows Server 2012

Utilice la siguiente sintaxis para ejecutar adprep independientemente de una instalación AD DS:

```
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *
```

Utilice /logdsid en el comando para generar registros más detallados. Adprep.log está ubicado en %windir%\System32\Debug\Adprep\Logs.

### <a name="running-adprep-using-smartcard"></a>Ejecución de adprep con tarjeta inteligente

La versión 2012 de Windows Server de adprep.exe funciona con tarjetas inteligentes como credenciales, pero no hay ninguna manera fácil de especificar las credenciales de la tarjeta inteligente a través de la línea de comandos. Una forma de hacerlo es obtener la tarjeta inteligente a través del cmdlet Get-Credential de PowerShell. A continuación, use el nombre de usuario del objeto PSCredential devuelto, el cual aparece como `@@...`. La contraseña es el PIN de la tarjeta inteligente.

Adprep.exe requiere /userdomain si se especifica /user. Para las credenciales de tarjetas inteligentes, el /userdomain debería ser el dominio de la cuenta de usuario subyacente representada por la tarjeta inteligente.

### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>El comando adprep /domainprep /gpprep no se ejecuta automáticamente

El comando adprep /domainprep /gpprep no se ejecuta como parte de la instalación de AD DS. Este comando establece permisos que son necesarios para la funcionalidad del modo de planeamiento del Conjunto resultante de directivas (RSOP). Para más información sobre este comando, consulte el [artículo 324392 de Microsoft Knowledge Base](https://support.microsoft.com/kb/324392). Si es necesario que el comando se ejecute en el dominio de Active Directory, puede ejecutarlo independientemente de la instalación de AD DS. Si el comando ya se ejecutó en la preparación de la implementación de controladores de dominio que ejecutan Windows Server 2003 SP1 o posterior, no es necesario ejecutar el comando otra vez.

Puede Agregar de forma segura controladores de dominio que ejecutan Windows Server 2012 a un dominio existente sin ejecutar Adprep/DomainPrep/gpprep, pero el modo de planeamiento de RSOP no funcionará correctamente.

## <a name="ad-ds-installation-prerequisite-validation"></a><a name="BKMK_PrereqCheck"></a>Validación de requisitos previos para la instalación de AD DS

El asistente para la instalación de AD DS comprueba que se cumplan los siguientes requisitos previos antes de que comience la instalación. Esto le ofrece la oportunidad de corregir problemas que podrían obstruir la instalación.

Por ejemplo, los requisitos previos relacionados con Adprep incluyen:

- Comprobación de credenciales de adprep: si es necesario ejecutar adprep, el asistente para la instalación comprueba que el usuario tiene derechos suficientes como para ejecutar las operaciones de adprep necesarias.
- Comprobación de disponibilidad de maestro de esquema: si el asistente para la instalación determina que es necesario ejecutar adprep /forestprep, comprueba que el maestro de esquema está conectado y, de lo contrario, da error.
- Comprobación de disponibilidad de maestro de infraestructura: si el asistente para la instalación determina que es necesario ejecutar adprep /domainprep, comprueba que el maestro de infraestructura está conectado y, de lo contrario, da error.

Otras comprobaciones de requisitos previos traídos del Asistente para la instalación de Active Directory heredado (dcpromo.exe) incluyen:

- Comprobación de nombre de bosque: asegura que el nombre de bosque sea válido y no exista en la actualidad.
- Comprobación de nombre NetBIOS: comprueba que el nombre NetBIOS proporcionado es válido y no tiene conflictos con otros nombres.
- Comprobación de ruta de acceso del componente: comprueba que las rutas de acceso para SYSVOL, registros y la base de datos de Active Directory, son válidas y que hay suficiente espacio disponible en disco para ellas.
- Comprobación de nombre de dominio secundario: asegura que el nombre de dominio primario y el nuevo secundario son válidos, y que no tienen conflictos con otros dominios.
- Comprobación de nombre de dominio de árbol: asegura que el nombre de árbol especificado sea válido y no exista en la actualidad.

## <a name="system-requirements"></a><a name="BKMK_SystemReqs"></a>Requisitos del sistema

Los requisitos del sistema para Windows Server 2012 no se han modificado en Windows Server 2008 R2. Para obtener más información, consulte [requisitos del sistema de Windows Server 2008 R2 con SP1](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) ( https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) .

Algunas características pueden tener requisitos adicionales. Por ejemplo, la característica de clonación del controlador de dominio virtual requiere que el emulador de PDC ejecute Windows Server 2012 y un equipo que ejecute Windows Server 2012 con el rol de Hyper-V instalado.

## <a name="known-issues"></a><a name="BKMK_KnownIssues"></a>Problemas conocidos

En esta sección se enumeran algunos de los problemas conocidos que afectan a la instalación de AD DS en Windows Server 2012. Para obtener información sobre otros problemas conocidos, consulte el tema sobre [solución de problemas con la implementación de controladores de dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).

- Si el acceso WMI al maestro de esquema está bloqueado por el Firewall de Windows cuando ejecuta adprep /forestprep remotamente, se registra el siguiente error en el registro de adprep en %systemroot%\system32\debug\adprep:

   ```
   Adprep encountered a Win32 error.
   Error code: 0x6ba Error message: The RPC server is unavailable.
   ```

   En este caso, para solucionar el error puede ejecutar adprep /forestprep directamente en el maestro de esquema, o puede ejecutar uno de los siguientes comandos para permitir el tráfico WMI a través del Firewall de Windows.

   Para Windows Server 2008 o posterior:

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes
   ```

   Para Windows Server 2003:

   ```
   netsh firewall set service RemoteAdmin enable
   ```

   Una vez que adprep haya terminado, puede ejecutar cualquiera de los siguientes comandos para volver a bloquear el tráfico WMI:

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no
   ```

   ```
   netsh firewall set service remoteadmin disable
   ```

- Puede escribir Ctrl + C para cancelar el cmdlet Install-ADDSForest. La cancelación detiene la instalación y se revierte cualquier cambio que se haya producido en el estado del servidor. Pero después de que se haya emitido el comando de cancelación, el control no vuelve a Windows PowerShell, y el cmdlet puede fallar indefinidamente.
- **La instalación de un controlador de dominio adicional mediante credenciales de tarjeta inteligente da error si el servidor de destino no se ha unido al dominio antes de la instalación.**

   El mensaje de error devuelto en este caso es:

   No se puede conectar al controlador de dominio del origen de la replicación *nombre del controlador de dominio del origen*. (Excepción: error de inicio de sesión: nombre de usuario desconocido o contraseña incorrecta)

   Si une el servidor de destino al dominio y después lleva a cabo la instalación con una tarjeta inteligente, la instalación se realizará correctamente.

- **El módulo ADDSDeployment no se ejecuta en procesos de 32 bits.** Si está automatizando la implementación y configuración de Windows Server 2012 con un script que incluye un cmdlet de ADDSDeployment y cualquier otro cmdlet que no admite procesos nativos de 64 bits, el script puede generar un error que indica que no se puede encontrar el cmdlet ADDSDeployment.

   En este caso, debe ejecutar el cmdlet de ADDSDeployment independientemente del cmdlet que no admite procesos nativos de 64 bits.

- Hay un nuevo sistema de archivos en Windows Server 2012 llamado sistema de archivos resistente. No almacene SYSVOL, archivos de registro o la base de datos de Active Directory en un volumen de datos formateado con el Sistema de archivos resistente (ReFS). Para obtener más información sobre ReFS, consulte el tema sobre cómo [crear el sistema de archivos de la próxima generación para Windows: ReFS](https://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx)(en inglés).
- En Administrador del servidor, los servidores que ejecutan AD DS u otros roles de servidor en una instalación Server Core y se han actualizado a Windows Server 2012, el rol de servidor puede aparecer con estado rojo, aunque los eventos y el estado se recopilen según lo previsto. Los servidores que ejecutan una instalación Server Core de una versión preliminar de Windows Server 2012 también pueden verse afectados.

### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>Si un error impide la replicación crítica, la instalación de Servicios de dominio de Active Directory no responde

Si la instalación de AD DS encuentra un error durante la fase de replicación crítica, la instalación puede fallar indefinidamente. Por ejemplo, si se producen errores de red que impiden que se complete la replicación crítica, la instalación no continuará.

Si está intentando instalar mediante el Administrador del servidor, es posible que vea que la página de progreso de la instalación permanece abierta, pero en la pantalla no se notifica de ningún error, y puede que el progreso no cambie en unos 15 minutos. Si usa Windows PowerShell, el progreso que se muestra en la ventana de Windows PowerShell no cambiará por más de 15 minutos.

Si experimenta este problema, vea el archivo dcpromo.log en la carpeta %systemroot%/debug en el servidor de destino. Seguramente, archivo de registro indicará reiterados errores al replicar. Algunas causas conocidas de este problema son:

- Problemas de red impiden la replicación crítica entre el servidor de destino que se promociona y el controlador de dominio del origen de la replicación.

   Por ejemplo, el dcpromo.log muestra:

   ```
   05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963
   Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.
   Process ID:
   500
   Reported error information:
   Error value:
   Could not find the domain controller for this domain. (1908)
   directory service:
   <domain>.com
   Extensive error information:
   Error value:
   A security package specific error occurred. 1825
   directory service:
   <DC Name>
   ```

   Debido a que el proceso de instalación vuelve a intentar llevar a cabo la replicación crítica indefinidamente, la instalación del controlador de dominio continúa si los problemas de red subyacentes se solucionan. Investigue el problema de red mediante el uso de herramienta como ipconfig, nslookup, y netmon según la necesidad. Asegúrese de que existe conectividad entre el controlador de dominio que está promoviendo y el asociado de replicación seleccionado durante la instalación de AD DS. También asegúrese de que la resolución de nombres esté funcionando.

   Los requisitos de instalación de AD DS para la conectividad de red y la resolución de nombres se validan durante la comprobación de requisitos previos antes de que comience la instalación. No obstante, pueden aparecer algunas condiciones de error en el momento posterior a que se produzca la validación de requisitos previos y antes de que se complete la instalación, como por ejemplo, el asociado de replicación deja de estar disponible durante la instalación.

- Durante la instalación de controladores de dominio de réplica, la cuenta de administrador local del servidor de destino se especifica para las credenciales de instalación y la contraseña de la cuenta de administrador local coincide con la contraseña de una cuenta de administrador de dominio. En este caso, puede completar el Asistente para la instalación e iniciar la instalación antes de que se produzca el error "acceso denegado".

   Por ejemplo, el dcpromo.log muestra:

   ```
   03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...
   03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.
   Process ID:
   508
   Reported error information:
   Error value:
   Access is denied. (5)
   directory service:
   DC2.contoso.com
   ```

   Para poder recuperarse si se produce un error al especificar una contraseña o cuenta de administrador local, debe reinstalar el sistema operativo, [realizar una limpieza de metadatos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816907(v=ws.10)) de la cuenta para el controlador de dominio que presentó el error al completar la instalación y después vuelva a intentar la instalación de AD DS usando credenciales de administrador de dominio. Si reinicia el servidor no se corregirá esta condición de error porque el servidor indicará que AD DS está instalado aunque la instalación no haya terminado correctamente.

### <a name="active-directory-domain-services-configuration-wizard-warns-when-a-non-normalized-dns-name-is-specified"></a><a name="BKMK_nonnormalDNSNameWarning"></a>El Asistente para configuración de Servicios de dominio de Active Directory advierte cuando se especifica un nombre DNS no normalizado

Si crea un nuevo dominio o bosque y especifica un nombre de dominio DNS que incluye caracteres internacionalizados que no están normalizados, el Asistente para configuración de Servicios de dominio de Active Directory mostrará una advertencia de que las consultas DNS del nombre pueden dar errores. Aunque el nombre de dominio DNS se especifica en la página de configuración de implementación, la advertencia aparece en la página de comprobación de requisitos previos más adelante en el asistente.

Si se especifica un nombre de dominio DNS con un nombre no normalizado como füßball. com o ' ΣΤ '. com (las versiones normalizadas son: füssball.com y βστα. com), las aplicaciones cliente que intentan obtener acceso a ella con WinHTTP normalizarán el nombre antes de llamar a las API de resolución de nombres. Si el usuario escribe "ΣΤ". com "en algún cuadro de diálogo, la consulta DNS se enviará como" βστα. com "y ningún servidor DNS coincidirá con un registro de recursos para" "ΣΤ". com ". El usuario no podrá resolver nombre.

En el siguiente ejemplo se explica uno de los problemas que puede ocurrir cuando se utiliza un nombre IDN que no está normalizado:

1. El dominio que utiliza un nombre no normalizado se crea y registra en el servidor DNS: füßball. com
2. El equipo "NPS" está unido al dominio y obtiene su nombre registrado: NPS. füßball. com
3. Una aplicación cliente intenta conectarse al servidor NPS. füßball. com
4. La aplicación cliente intenta resolver el nombre NPS. füßball. com llamando a las API de resolución de nombres.
5. Debido a la normalización, el nombre se convierte en nps.füssball.com y se consulta a través de la conexión como NPS. füßball. com
6. La aplicación cliente no puede resolver el nombre porque el nombre registrado es NPS. füßball. com

Si aparece la advertencia en la página de comprobación de requisitos previos en el Asistente para configuración de Servicios de dominio de Active Directory, regrese a la página de configuración de implementación y especifique un nombre de dominio DNS normalizado. Si está instalando un nuevo dominio con Windows PowerShell, especifique un nombre DNS normalizado para la opción -DomainName.

Para obtener más información sobre IDN, consulte el tema sobre el [tratamiento de nombres de dominio internacionalizados (IDN)](/windows/win32/intl/handling-internationalized-domain-names--idns).
