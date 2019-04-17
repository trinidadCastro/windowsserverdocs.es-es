---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: "¿Qué & #39; s de dominio de Active Directory de servicios de instalación y desinstalación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 814a6f1af9144db28e81163b21cb5da4b6e51b3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services-installation-and-removal"></a>¿Qué & #39; s de dominio de Active Directory de servicios de instalación y desinstalación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema trata:  
  
-   [El Asistente para configuración de servicios de dominio de Active Directory](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_ADConfigurationWizard)  
  
-   [Integración de Adprep.exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)  
  
-   [Validación de AD DS instalación requisitos previo](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_PrereqCheck)  
  
-   [Requisitos del sistema](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_SystemReqs)  
  
-   [Problemas conocidos](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)  
  
Implementación de Active Directory servicios de dominio (AD DS) en Windows Server 2012 es más sencillo y más rápido que las versiones anteriores de Windows Server. El proceso de instalación de AD DS ahora está integrado en Windows PowerShell y se integra con el administrador del servidor. Se reduce el número de pasos necesarios para introducir los controladores de dominio en un entorno de Active Directory existente. Esto hace que el proceso para crear un nuevo entorno de Active Directory más sencilla y eficaz. El nuevo proceso de implementación de AD DS, minimiza el riesgo de errores que de lo contrario se habría bloqueado la instalación.  
  
Además, puede instalar los binarios de rol de servidor de AD DS (es decir, el rol de servidor de AD DS) en varios servidores al mismo tiempo. También puedes ejecutar al Asistente para la instalación de AD DS remotamente en un servidor individual. Estas mejoras proporcionan más flexibilidad para la implementación de controladores de dominio que ejecutan Windows Server 2012, especialmente para implementaciones a gran escala globales donde muchos controladores de dominio deben implementarse para oficinas en diferentes regiones.  
  
Instalación de AD DS incluye las siguientes características:  
  
-   **Integración de Adprep.exe en el proceso de instalación de AD DS.** Los pasos engorrosos necesarios para preparar un Active Directory existentes, como la necesidad de usar una variedad de credenciales diferentes, copia los archivos Adprep.exe o iniciar sesión en determinados controladores de dominio, se han simplificado todos o automáticamente. Esto reduce el tiempo necesario para instalar AD DS y minimiza el riesgo de errores que podrían bloquear promoción del controlador de dominio.  
  
    Para entornos donde es preferible ejecutar comandos adprep.exe antes de una nueva instalación del controlador de dominio, también puede ejecutar comandos adprep.exe por separado de la instalación de AD DS. La versión de Windows Server 2012 de adprep.exe se ejecuta de forma remota, por lo que puede ejecutar todos los comandos necesarios desde un servidor que ejecute una versión de 64 bits de Windows Server 2008 o posterior.  
  
-   **La nueva instalación de AD DS está integrada en Windows PowerShell y se puede invocar de forma remota.** La nueva instalación de AD DS se integra con el administrador del servidor, así que puedes usar la misma interfaz para instalar AD DS que usas al instalar otros roles de servidor. Para los usuarios de Windows PowerShell, los cmdlets de implementación de AD DS proporciona más funcionalidad y flexibilidad. Hay paridad funcional entre de línea de comandos y opciones de instalación de la interfaz gráfica de usuario.  
  
-   **La nueva instalación de AD DS incluye la validación de requisitos previo.** Antes de que comience la instalación, se identifican posibles errores. Antes de que se produzcan sin las preocupaciones procedentes de una actualización parcialmente completa se pueden corregir las condiciones de error. Por ejemplo, si se debe ejecutar adprep /domainprep, el Asistente para la instalación comprueba que el usuario tiene derechos necesarios para ejecutar la operación.  
  
-   **Páginas de configuración se agrupan en una secuencia que refleja los requisitos de las opciones más comunes de promoción con opciones relacionadas que se agrupan en menos páginas del asistente.** Esto proporciona una mejor contexto para seleccionar las opciones de instalación.  
  
-   **Puedes exportar un script de PowerShell de Windows que contiene todas las opciones que se especificaron durante la instalación gráfica.** Al final de la instalación o desinstalación, puedes exportar la configuración a un script de PowerShell de Windows para su uso con la automatización de la misma operación.  
  
-   **Solo crítica replicación se produce antes de reiniciar.** Nuevo conmutador para permitir la replicación de datos no sean críticas antes de reiniciar. Para obtener más información, consulta [ADDSDeployment cmdlet argumentos](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  
  
### <a name="BKMK_ADConfigurationWizard"></a>El Asistente para configuración de servicios de dominio de Active Directory  
A partir de Windows Server 2012, el Asistente para la configuración de los servicios de dominio de Active Directory reemplaza al heredado Active Directory dominio instalación Asistente para servicios como la opción de la interfaz de usuario para especificar la configuración cuando se instala un controlador de dominio. El Asistente para la configuración de los servicios de dominio de Active Directory comienza una vez completado el Asistente para agregar Roles.  
  
> [!WARNING]  
> Heredado dominio servicios instalación Asistente de Active Directory (dcpromo.exe) está en desuso a partir de Windows Server 2012.  
  
En [instalar servicios de dominio de Active Directory & #40; Nivel 100 & #41; ](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), los procedimientos de interfaz de usuario se muestra cómo iniciar el Asistente para agregar Roles para instalar el servidor de AD DS archivos binarios de rol y, a continuación, ejecuta el Asistente de configuración de servicios de Active Directory dominio para completar la instalación del controlador de dominio. Los ejemplos de Windows PowerShell muestran cómo completar los pasos con un cmdlet de implementación de AD DS.  
  
### <a name="BKMK_NewAdprep"></a>Integración de Adprep.exe  
A partir de Windows Server 2012, hay solo una versión de Adprep.exe (no hay ninguna versión de 32 bits, adprep32.exe). Comandos Adprep se ejecutan automáticamente según sea necesario cuando se instala un controlador de dominio que ejecute Windows Server 2012 a un dominio de Active Directory o bosque.  
  
Aunque las operaciones de adprep se ejecutan automáticamente, puedes ejecutar Adprep.exe por separado. Por ejemplo, si el usuario que instale AD DS no es miembro del grupo Administradores de empresa, que es necesario para poder ejecutar Adprep /forestprep, a continuación, debes ejecutar el comando por separado. Pero, solo tienes que ejecutar adprep.exe si vas a la actualización en contexto tu primer controlador de dominio de Windows Server 2012 (en otras palabras, piensa en el lugar actualizar el sistema operativo de un controlador de dominio que ejecute Windows Server 2012).  
  
Adprep.exe se encuentra en la carpeta \support\adprep del disco de instalación de Windows Server 2012. La versión de Windows Server 2012 de Adprep.exe es capaz de ejecutar de forma remota.  
  
La versión de Windows Server 2012 de adprep.exe puede ejecutar en cualquier servidor que ejecute una versión de 64 bits de Windows Server 2008 o posterior. El servidor tiene conectividad de red con el maestro de esquema para el bosque y el maestro de infraestructura del dominio donde desee agregar un controlador de dominio. Si alguno de esos roles está hospedado en un servidor que ejecuta Windows Server 2003, a continuación, adprep debe ejecutarse remotamente. El servidor donde se ejecuta adprep no tiene que ser un controlador de dominio. Puede ser dominio combinada o en un grupo de trabajo.  
  
> [!NOTE]  
> Si se intenta ejecutar la versión de Windows Server 2012 de adprep.exe en un servidor que ejecuta Windows Server 2003, aparece el error siguiente:  
>   
> Adprep.exe no es una aplicación de Win32 válida.  
  
![Novedades](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  
  
Para obtener información sobre cómo resolver otro error devuelto por Adprep.exe, vea [problemas conocidos](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  
  
#### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Verificación de pertenencia a grupo contra los roles de maestro de operaciones de Windows Server 2003  
Para cada comando (o forestprep, /domainprep o /rodcprep), Adprep realiza una comprobación de la pertenencia al grupo para determinar si la credencial especificada representa una cuenta en determinados grupos. Para realizar esta comprobación, Adprep se pone en contacto con el propietario de la función de maestro de operaciones. Si el maestro de operaciones ejecuta Windows Server 2003, debes especificar los parámetros de línea de comandos de /userdomain y/User Si ejecutas Adprep.exe para garantizar que la comprobación de pertenencia a grupo se realiza en todos los casos.  
  
La/User y /userdomain son nuevos parámetros de Adprep.exe en Windows Server 2012. Estos parámetros especifican el nombre de cuenta de usuario y el dominio del usuario, respectivamente, del usuario que se ejecuta el comando adprep. La utilidad de línea de comandos de Adprep.exe bloquea una especificación de /userdomain y/User, pero si se omite el otro.  
  
Sin embargo, también pueden ejecutar Adprep operaciones como parte de una instalación de AD DS mediante Windows PowerShell o el administrador del servidor. Estas experiencias comparten la misma implementación subyacente (adprep.dll) que adprep.exe. Las experiencias de Windows PowerShell y el administrador del servidor tienen sus entradas, que no impone los mismos requisitos como, por adprep.exe varias credenciales distintas. Con Windows PowerShell o el administrador del servidor, es posible pasar un valor para/User pero no /userdomain a adprep.dll. Si se especifica/user pero /userdomain no se especifica, el dominio de la máquina local se usa para realizar la comprobación. Si el equipo no está unido a un dominio, no se puede comprobar la pertenencia al grupo.  
  
Cuando no se puede comprobar la pertenencia al grupo, Adprep muestra un mensaje de advertencia en los archivos de registro adprep y continúa:  
  
```  
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```  
  
Si ejecutas Adprep.exe sin especificar los parámetros de /userdomain y/User y el maestro de operaciones se ejecuta Windows Server 2003, Adprep.exe se pone en contacto con un controlador de dominio en el dominio del usuario de inicio de sesión actual. Si el usuario de inicio de sesión actual no es una cuenta de dominio, Adprep.exe no puede realizar la comprobación de pertenencia a grupo. Adprep.exe tampoco puede realizar la comprobación de pertenencia a grupo si se usan las credenciales de tarjeta inteligente, incluso si se especifica/user y /userdomain.  
  
Si Adprep finaliza correctamente, no hay se requiere ninguna acción. Si se produce un error en Adprep durante la ejecución con errores de acceso, proporcionar una cuenta con la pertenencia a grupos correcta. Para obtener más información, consulta [requisitos para ejecutar Adprep.exe e instalar los servicios de dominio de Active Directory de credenciales](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
#### <a name="syntax-for-adprep-in-windows-server-2012"></a>Sintaxis de Adprep en Windows Server 2012  
Usa la siguiente sintaxis para ejecutar adprep por separado desde una instalación de AD DS:  
  
```  
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```  
  
Usar /logdsid en el comando para generar un registro más detallado. La adprep.log se encuentra en % windir%\System32\Debug\Adprep\Logs.  
  
#### <a name="running-adprep-using-smartcard"></a>Ejecutar adprep con tarjeta inteligente  
La versión de Windows Server 2012 de adprep.exe funciona con tarjeta inteligente como credenciales, pero no hay ninguna forma sencilla que especifique las credenciales de tarjeta inteligente a través de la línea de comandos. Una forma de hacerlo es obtener las credenciales de tarjeta inteligente mediante el cmdlet Get-Credential de PowerShell. A continuación, usar el nombre de usuario del objeto devuelto PSCredential, que aparece como `@@...`. La contraseña es el PIN de la tarjeta inteligente.  
  
Adprep.exe requiere /userdomain si se especifica/User. Las credenciales de tarjeta inteligente, la /userdomain debe ser el dominio de la cuenta de usuario subyacente representado por la tarjeta inteligente.  
  
#### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Comando de adprep /domainprep /gpprep no se ejecuta automáticamente  
No se ejecuta el comando adprep /domainprep /gpprep como parte de la instalación de AD DS. Este comando establece permisos requeridos para conjunto resultante de directivas (RSOP) funcionalidad del modo de diseño. Para obtener más información acerca de este comando, consulta [artículo de Microsoft Knowledge Base 324392](https://support.microsoft.com/kb/324392). Si el comando debe ejecutarse en el dominio de Active Directory, puede ejecutarlo por separado de la instalación de AD DS. Si ya se ejecutó el comando en preparación de la implementación de controladores de dominio que ejecutan Windows Server 2003 SP1 o posterior, el comando no es necesario volver a ejecutar.  
  
Puedes agregar controladores de dominio que ejecutan Windows Server 2012 a un dominio existente sin ejecución adprep /domainprep /gpprep forma segura, pero el modo de planeamiento RSOP no funcionará correctamente.  
  
### <a name="BKMK_PrereqCheck"></a>Validación de AD DS instalación requisitos previo  
El Asistente para la instalación de AD DS, se comprueba que se cumplan los siguientes requisitos previos antes de que comience la instalación. Esto proporciona una oportunidad de corregir problemas que posiblemente se pueden bloquear la instalación.  
  
Por ejemplo, requisitos previos relacionados con Adprep incluyen:  
  
-   Verificación de la credencial Adprep: Si adprep necesita para ejecutarse, el Asistente para la instalación comprueba que el usuario tiene derechos necesarios para ejecutar las operaciones de Adprep necesarias.  
  
-   Comprobación de la disponibilidad de maestro de esquema: si el Asistente para la instalación determina que adprep /forestprep debe ejecutarse, comprueba que el maestro de esquema está en línea y se produce un error de lo contrario.  
  
-   Comprobación de la disponibilidad de maestro de infraestructura: si el Asistente para la instalación determina que adprep /domainprep debe ejecutarse, comprueba que el maestro de infraestructura está en línea y se produce un error de lo contrario.  
  
Otras comprobaciones de requisitos previos que se transfieren desde heredado instalación Asistente de Active Directory (dcpromo.exe) incluyen:  
  
-   Comprobación del nombre del bosque: garantiza que el nombre del bosque es válido y no existe.  
  
-   Verificación de nombre NetBIOS: comprueba que proporciona el nombre NetBIOS es válido y no entre en conflicto con nombres existentes.  
  
-   Comprobación de componente de ruta de acceso: comprueba que las rutas de acceso de la base de datos de Active Directory, registros y SYSVOL son válidos y que hay suficiente espacio en disco disponible para ellos.  
  
-   Comprobación de nombre de dominio secundario: garantiza que el principal y los nuevos nombres de dominio secundarios son válidos y que no habrá conflictos con los dominios existentes.  
  
-   En el árbol de comprobación de nombre de dominio: garantiza que el nombre de árbol especificada sea válido y que no actualmente existe.  
  
## <a name="BKMK_SystemReqs"></a>Requisitos del sistema  
Requisitos del sistema de Windows Server 2012 han cambiado desde Windows Server 2008 R2. Para obtener más información, consulta [Windows Server 2008 R2 con SP1 requisitos del sistema](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  
  
Algunas características pueden tener requisitos adicionales. Por ejemplo, la característica clonación del controlador de dominio virtual requiere el emulador PDC ejecuta Windows Server 2012 y un equipo que ejecute Windows Server 2012 con el rol de Hyper-V instalado.  
  
## <a name="BKMK_KnownIssues"></a>Problemas conocidos  
En esta sección se enumera algunos de los problemas conocidos que afectan a la instalación de AD DS en Windows Server 2012. Para los problemas conocidos adicionales, consulta [solución de problemas de implementación del controlador de dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
-   Si el acceso WMI para el maestro de esquema está bloqueado por Firewall de Windows cuando se ejecuta de forma remota adprep /forestprep, el siguiente error se registra en el registro de adprep en % systemroot%\system32\debug\adprep:  
  
    ```  
    Adprep encountered a Win32 error.   
    Error code: 0x6ba Error message: The RPC server is unavailable.  
    ```  
  
    En este caso, puede solucionar el error por cada ejecución adprep /forestprep directamente en el maestro de esquema, o puedes ejecutar uno de los siguientes comandos para permitir el tráfico WMI a través de Firewall de Windows.  
  
    Para Windows Server 2008 o posterior:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes  
    ```  
  
    Para Windows Server 2003:  
  
    ```  
    netsh firewall set service RemoteAdmin enable  
    ```  
  
    Una vez finalizada adprep puede ejecutar cualquiera de los siguientes comandos para bloquear el tráfico WMI de nuevo:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no  
    ```  
  
    ```  
    netsh firewall set service remoteadmin disable  
    ```  
  
-   Puedes escribir Ctrl + C para cancelar el cmdlet ADDSForest de instalación. La cancelación detiene la instalación y se revertir los cambios realizados en el estado del servidor. Pero después de que se ejecuta el comando de cancelación, no se devuelve el control de Windows PowerShell y el cmdlet puede bloquearse indefinidamente.  
  
-   **Instalación de un controlador de dominio con credenciales de tarjeta inteligente se produce un error si el servidor de destino no está unido al dominio antes de la instalación.**  
  
    En este caso es el mensaje de error:  
  
    No se puede conectar al controlador de dominio de origen de replicación *nombre del controlador de dominio de origen*. (Excepción: Logonfailure: nombre de usuario desconocido o contraseña incorrecta)  
  
    Si el servidor de destino une al dominio y, a continuación, realizar la instalación con una tarjeta inteligente, la instalación se realiza correctamente.  
  
-   **El módulo de ADDSDeployment no se ejecuta en procesos de 32 bits.** Si automatizar la implementación y configuración de Windows Server 2012 con un script que incluya un cmdlet ADDSDeployment y cualquier otro cmdlet que no es compatible con los procesos de 64 bits nativos, puede haber un error en la secuencia de comandos con un error que indica que no se encuentra el cmdlet ADDSDeployment.  
  
    En este caso, debes ejecutar el cmdlet ADDSDeployment por separado desde el cmdlet compatible con los procesos de 64 bits nativos.  
  
-   Hay un nuevo sistema de archivos en Windows Server 2012 con el nombre de sistema de archivos resistente. No almacenar la base de datos de Active Directory, archivos de registro o SYSVOL en un volumen de datos con formato con el sistema de archivos resistente (ReFS). Para obtener más información acerca de referencias (Refs), consulta [crear el siguiente sistema de archivos de generación para Windows: ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx).  
  
-   En el administrador del servidor, los servidores que ejecutan AD DS o a otros roles de servidor en una instalación Server Core y se han actualizado a Windows Server 2012, el rol de servidor puede aparecer con el estado de rojo, aunque el estado y los eventos se recopilan según lo esperado. Servidores que ejecutan una instalación Server Core de una versión preliminar de que Windows Server 2012 se vean afectadas.  
  
### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>Instalación de Active Directory los servicios de dominio se bloquea si un error impide la duplicación crítica  
Si la instalación de AD DS, encuentra un error durante la fase de replicación crítica, la instalación puede bloquearse indefinidamente. Por ejemplo, si los errores de red evitan la replicación crítica de finalización, la instalación no seguirá adelante.  
  
Si vas a instalar con el administrador del servidor, puedes ver la página de progreso de instalación permanecer abierto, pero no tiene ningún error aparece en pantalla, y no puede cambiar el progreso durante unos 15 minutos. Si estás usando Windows PowerShell, el progreso que se muestra en la ventana de PowerShell de Windows no se modificarán durante más de 15 minutos.  
  
Si surge este problema, comprueba el archivo dcpromo.log en la carpeta %systemroot%\debug en el servidor de destino. Normalmente, el archivo de registro indicará errores repetidos para replicar. Algunas causas conocidas para este problema son:  
  
-   Problemas de red impiden la replicación fundamental entre el servidor de destino que se promueve y el controlador de dominio de origen de replicación.  
  
    Por ejemplo, se muestra la dcpromo.log:  
  
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
  
    Dado que el proceso de instalación vuelve a intentar la replicación crítica indefinidamente, la instalación del controlador de dominio continúa si se resuelven los problemas de red subyacente. Investiga el problema de red con herramientas como ipconfig, nslookup y netmon según sea necesario. Asegúrese de que existe una conexión entre el controlador de dominio que se promueve y el asociado de replicación seleccionada durante la instalación de AD DS. Además, asegúrate de que funciona la resolución de nombres.  
  
    Requisitos de instalación de AD DS para la resolución de conectividad y el nombre de red se validan durante la comprobación de requisitos previos antes de que comience la instalación. Sin embargo, algunas condiciones de error pueden producirse en el tiempo después de que se produzca la validación de requisitos previo y antes de que finalice la instalación, por ejemplo, si el asociado de replicación no está disponible durante la instalación.  
  
-   Durante la instalación del controlador de dominio de réplica, se especifica la cuenta de administrador local del servidor de destino de las credenciales de la instalación y la contraseña de la cuenta de administrador local coincide con la contraseña de una cuenta de administrador de dominio. En este caso, puedes completar al Asistente para la instalación y comenzar la instalación antes de que se encuentra el error "Acceso denegado".  
  
    Por ejemplo, se muestra la dcpromo.log:  
  
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
  
    Si el error se produce mediante la especificación de una cuenta de administrador local y la contraseña, para poder recuperar puedes volver a instalar el sistema operativo, [realice la limpieza de metadatos](https://technet.microsoft.com/library/cc816907(WS.10).aspx) de la cuenta para el controlador de dominio que no se pudo completar la instalación y, a continuación, vuelva a intentar la instalación de AD DS con credenciales de administrador de dominio. Reiniciar el servidor no corrige esta condición de error porque el servidor se indica que está instalado AD DS aunque la instalación no se completó correctamente.  
  
### <a name="BKMK_nonnormalDNSNameWarning"></a>Asistente para la configuración a los servicios de Active Directory dominio avisa cuando se especifica un nombre DNS no normalizada  
Si creas un nuevo dominio o bosque y se especifica un nombre de dominio DNS que incluye caracteres internacionales que no están normalizados, el Asistente para la configuración de los servicios de dominio de Active Directory muestra una advertencia que pueden producir un error en las consultas DNS para el nombre. Aunque el nombre de dominio DNS se especifica en la página de configuración de implementación, la advertencia aparece en la página de comprobación de requisitos previos más adelante en el asistente.  
  
Si no se especifica un nombre de dominio DNS usando un nombre sin normalizado como .com füßball.com o 'ΣΤ' (son las versiones normalizadas: füssball.com y βστα.com), las aplicaciones cliente que intentan acceder a él con WinHTTP normalizará el nombre antes de llamar a las API de resolución de nombre. Si el usuario escribe "'ΣΤ'.com" en el cuadro de diálogo de algunos, se enviará la consulta DNS como "βστα.com" y no hay ningún servidor DNS coincidirá con él con un registro de recursos para ".com 'ΣΤ'". El usuario podrá resolver el nombre.  
  
Uno de los problemas que pueden producirse cuando se usa un nombre IDN que no está normalizado explica en el siguiente ejemplo:  
  
1.  El dominio con un nombre no normalizada se crearon y registraron en servidor dns: füßball.com  
  
2.  Máquina "nps" está unido al dominio y obtiene su nombre registrado: nps.füßball.com  
  
3.  Una aplicación cliente intenta conectarse a la nps.füßball.com de servidor  
  
4.  La aplicación cliente intenta resolver el nps.füßball.com nombre llamar a las API de resolución de nombre.  
  
5.  Debido a la normalización, el nombre se convierte a nps.füssball.com y se consulta a través de la red como nps.füßball.com  
  
6.  La aplicación cliente es no se puede resolver el nombre, dado que el nombre registrado es nps.füßball.com  
  
Si la advertencia aparece en la página de comprobación de requisitos previos en el Asistente para la configuración de los servicios de dominio de Active Directory, volver a la página de configuración de implementación y especifica un nombre de dominio DNS normalizado. Si vas a instalar un nuevo dominio mediante Windows PowerShell, especifica un nombre DNS normalizado de la opción - NombreDominio.  
  
Para obtener más información sobre IDN, consulta [control Internacionalice los nombres de dominio (IDN)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
  

