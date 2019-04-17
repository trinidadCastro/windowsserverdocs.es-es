---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: Instalar Windows Server 2012 Active Directory Read-Only controlador de dominio (RODC) (nivel 200)
description: "Este tema explica cómo crear una cuenta RODC provisional y, después, adjunta un servidor a esa cuenta durante la instalación de RODC. Este tema también explica cómo instalar un RODC sin realizar una instalación por fases."
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 78281ca845f79955aaa25aa45394284c59e639cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Instalar Windows Server 2012 Active Directory Read-Only controlador de dominio (RODC) (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica cómo crear una cuenta RODC provisional y, después, adjunta un servidor a esa cuenta durante la instalación de RODC. Este tema también explica cómo instalar un RODC sin realizar una instalación por fases.  
  
## <a name="stage-rodc-workflow"></a>Flujo de trabajo de fase RODC  
Una por fases leer solo dominio funciona de instalación de controlador (RODC) en dos fases:  
  
1.  Copia intermedia de una cuenta de equipo libre  
  
2.  Adjuntar un RODC a esa cuenta durante la promoción  
  
El siguiente diagrama ilustra los servicios de dominio de Active Directory Read-Only copia intermedia del proceso, donde crear una cuenta de equipo RODC vacía en el dominio mediante el centro de administración de Active Directory de (Dsac.exe) el controlador de dominio.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>Fase RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Add-addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-El nombre de dominio***<br /><br />***-El nombre de sitio***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***-Credenciales***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-ReplicationSourceDC|  
  
> [!NOTE]  
> La **-credenciales** argumento solo es necesario si no ya sesión como miembro del grupo Domain Admins.  
  
## <a name="attach-rodc-workflow"></a>Asociar el flujo de trabajo RODC  
El siguiente diagrama ilustra el proceso de configuración de los servicios de dominio de Active Directory, donde ya instalado el rol de AD DS, ha almacenado provisionalmente la cuenta de RODC y comenzado **promover este servidor a un controlador de dominio** con el administrador del servidor para crear un nuevo RODC en un dominio existente, adjuntarlos a la cuenta de equipo provisional.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>Adjuntar RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Install-AddsDomaincontroller|-SkipPreChecks<br /><br />***-El nombre de dominio***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciales***<br /><br />-CriticalReplicationOnly<br /><br />*Ruta:*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> La **-credenciales** argumento solo es necesario si no ya sesión como miembro del grupo Domain Admins.  
  
## <a name="staging"></a>Almacenamiento provisional  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
Realizar la operación de copia intermedia de una cuenta de equipo del controlador de dominio de solo lectura, abre el centro de administración de Active Directory (**Dsac.exe**). Haz clic en el nombre del dominio en el panel de navegación. Haz doble clic en **controladores de dominio** en la lista de administración. Haz clic en **crea previamente una Read-only cuenta controlador de dominio** en el panel de tareas.  
  
Para obtener más información sobre el centro de administración de Active Directory, consulte [opciones avanzadas de AD DS administración usar Active Directory administrativas Center & #40; Nivel 200 & #41; ](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)y revisar [centro de administración de Active Directory: Introducción](https://technet.microsoft.com/library/dd560651(WS.10).aspx).  
  
Si tienes experiencia en la creación de controladores de dominio de solo lectura, detectará que el Asistente para la instalación tiene la misma interfaz gráfica, tal como se muestra al usar el complemento de equipos de Windows Server 2008 y usuarios anteriores de Active Directory y usa el mismo código, que incluye la configuración en el formato de archivo de instalación desatendida utilizado por el dcpromo obsoleto de exportación.  
  
Windows Server 2012 presenta un nuevo cmdlet ADDSDeployment a cuentas de equipo de escenario RODC, pero el Asistente para no usar el cmdlet para su funcionamiento. Las siguientes secciones muestran los argumentos y el cmdlet equivalente para que la información asociada a cada uno más fáciles de comprender.  
  
La **crea previamente una Read-only cuenta controlador de dominio** vínculo panel del Active Directory centro de administración de tareas es equivalente al cmdlet ADDSDeployment Windows PowerShell:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>Te damos la bienvenida  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
La **Bienvenido al Asistente para la instalación de los servicios de dominio de Active Directory** diálogo tiene una opción denominada **usa la instalación en modo avanzado**. Selecciona esta opción y haz clic en **siguiente** para mostrar las opciones de directiva de replicación de contraseña. Desactiva esta opción para usar los valores predeterminados para las opciones de directiva de replicación de contraseña (Esto se explica con más detalle más adelante en esta sección).  
  
### <a name="network-credentials"></a>Credenciales de red  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
La opción de nombre de dominio en el **credenciales de red** cuadro de diálogo muestra el dominio de destino mediante el centro de administración de Active Directory de manera predeterminada. De manera predeterminada, se usan sus credenciales actuales. Si no incluyen pertenencia al grupo Administradores de dominio, haz clic en **credenciales alternativas**y haz clic en **establecer** para proporcionar el Asistente con un nombre de usuario y contraseña que sea miembro del grupo Administradores de dominio de.  
  
Es el equivalente argumento ADDSDeployment Windows PowerShell:  
  
```  
-credential <pscredential>  
```  
  
Ten en cuenta que el sistema provisional es un puerto directo de Windows Server 2008 R2 y no proporciona la nueva funcionalidad Adprep. Si piensas implementar provisional cuentas RODC, debe implementar primero un RODC detenido provisional en ese dominio para que se ejecuta la operación de rodcprep automática o manualmente ejecutar adprep.exe /rodcprep primera.  
  
De lo contrario, recibirás error "No podrá instalar un controlador de dominio de solo lectura en este dominio porque no se ha ejecutado"adprep /rodcprep"".  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>Especificar el nombre del equipo  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
La **especificar el nombre del equipo** diálogo requiera que escribas la etiqueta única **nombre de equipo** de un controlador de dominio que no existe. El controlador de dominio, configurar y adjuntar más adelante en esta cuenta debe tener el mismo nombre o la operación de promoción no detectará la cuenta por fases.  
  
Es el equivalente argumento ADDSDeployment Windows PowerShell:  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>Selecciona un sitio  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
La **seleccione un sitio** cuadro de diálogo muestra una lista de sitios de Active Directory para el bosque actual. La operación de controlador de dominio de solo lectura provisional requiere que seleccione un único sitio de la lista. El RODC utiliza esta información para crear su objeto de configuración NTDS en la partición de configuración y unirse a sí mismo en el sitio correcto cuando se inicia por primera vez después de que se está implementando.  
  
Es el equivalente argumento ADDSDeployment Windows PowerShell:  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>Opciones del controlador de dominio adicional  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
La **opciones adicionales del controlador de dominio** cuadro de diálogo te permite especificar que un controlador de dominio incluir que se ejecuta como un **servidor DNS** y un **catálogo Global**. Microsoft recomienda que los controladores de dominio de solo lectura proporcionan servicios DNS y GC, por lo que ambas se instalan de forma predeterminada; una intención de la función RODC sucursales cuando no esté disponible la red de área extensa y sin esos DNS y servicios de catálogo global, equipos de la rama no podrán usar la funcionalidad y recursos de AD DS.  
  
La **el controlador de dominio de solo lectura (RODC)** opción ya está seleccionada y no se puede deshabilitar. Los argumentos de ADDSDeployment Windows PowerShell equivalentes son:  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> De manera predeterminada, la **- NoGlobalCatalog** valor es $false, lo que significa que el controlador de dominio será un servidor de catálogo global si no se especifica el argumento.  
  
### <a name="specify-the-password-replication-policy"></a>Especificar la directiva de replicación de contraseñas  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
La **especificar la directiva de replicación de contraseña** cuadro de diálogo te permite modificar la lista predeterminada de las cuentas que se pueden almacenar en caché sus contraseñas en este controlador de dominio de solo lectura. Cuentas de la lista configurada con **denegar** o que no están en la lista (implícita) no almacene en caché su contraseña. Las cuentas que no están permitidas en caché las contraseñas en el RODC y no pueden conectarse y autenticarse en un controlador de dominio grabable no pueden acceder a recursos o funcionalidad proporcionada por Active Directory.  
  
> [!IMPORTANT]  
> El asistente muestra este cuadro de diálogo solo si seleccionas la **el modo de instalación avanzada al utilizar** casilla de verificación de la pantalla de bienvenida. Si desactivas esta casilla, a continuación, el asistente se usa los siguientes valores y grupos predeterminados:  
>   
> -   Los administradores - denegar  
> -   Los operadores de servidor - denegar  
> -   Una copia de seguridad de los operadores - denegar  
> -   Cuenta operadores - denegar  
> -   Denegar denegado RODC contraseña Replication Group-  
> -   Permite RODC contraseña Replication Group - permitir  
  
Los argumentos de ADDSDeployment Windows PowerShell equivalentes son:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>Delegación de instalación de RODC y administración  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
La **delegación de instalación y RODC administración** cuadro de diálogo te permite configurar un usuario o grupo que contiene los usuarios que tienen permiso para asociar el servidor a la cuenta de equipo RODC. Haz clic en **establecer** examinar el dominio de un usuario o grupo. El usuario o grupo especificado en este cuadro de diálogo ganancias permisos administrativos locales en el RODC. El usuario especificado o miembros del grupo especificado pueden realizar operaciones en el RODC con privilegios equivalentes al grupo de administradores del equipo. Son *no* miembros de los administradores de dominio o grupos de administradores de dominio integrados.  
  
Usa esta opción para delegar la administración de office rama sin conceder la pertenencia al administrador de rama al grupo Administradores de dominio. Delegar la administración de RODC no es necesario.  
  
Es el equivalente argumento ADDSDeployment Windows PowerShell:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>Resumen  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
La **resumen** cuadro de diálogo te permite confirmar la configuración. Esta es la última oportunidad para detener la instalación antes de que el asistente crea la cuenta por fases. Haz clic en **siguiente** cuando estés listo para crear la cuenta de equipo RODC provisional.  Haz clic en **Exportar configuración** para guardar un archivo de respuesta en el formato de archivo de instalación desatendida de dcpromo obsoleto.  
  
### <a name="creation"></a>Creación  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
La **Asistente para la instalación a servicios de Active Directory dominio** crea al controlador de dominio de solo lectura en Active Directory. No se puede cancelar esta operación después de iniciarse.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
Agregar una cuenta de equipo del controlador de dominio de solo lectura con el módulo de ADDSDeployment Windows PowerShell, usa el cmdlet siguiente:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
Consulta [fase RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS) para argumentos necesarios y opcionales.  
  
Dado que **agregar addsreadonlydomaincontrolleraccount** solo tiene una acción con dos fases (requisitos previos comprobación e instalación), las capturas de pantalla siguiente muestran la fase de instalación con los argumentos requeridos mínimos.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
La fase de operación RODC crea la cuenta de equipo RODC en Active Directory. El centro de administración de Active Directory, se muestra la **tipo de controlador de dominio** como un **cuenta libre de controlador de dominio**. Estos tipos de controlador de dominio indica que provisional cuenta RODC está listo para que un servidor adjuntará a ella como un controlador de dominio de solo lectura.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> El centro de administración de Active Directory ya no es necesario para asociar un servidor a una cuenta de equipo del controlador de dominio de solo lectura. Usa el administrador del servidor y el Asistente para la configuración de los servicios de dominio de Active Directory o el cmdlet de módulo de PowerShell de Windows ADDSDeployment **instalación AddsDomainController** adjuntar un nuevo RODC con su provisionalmente cuenta. Los pasos son similares para agregar un nuevo controlador de dominio de escritura a un dominio existente, con la excepción de que la cuenta de equipo RODC provisional contiene opciones de configuración decididas en el momento en que se almacenan provisionalmente la cuenta de equipo RODC.  
  
## <a name="attaching"></a>Adjuntar  
  
### <a name="deployment-configuration"></a>Configuración de implementación  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
El administrador del servidor comienza cada promoción del controlador de dominio con la **configuración de implementación** página. El resto de opciones y los campos obligatorios cambian en esta página y las páginas siguientes, según qué operación de implementación que seleccione.  
  
Para agregar un controlador de dominio de solo lectura a un dominio existente, selecciona **agregar un controlador de dominio a un dominio existente** y haz clic en el **selecciona** botón **especificar la información de dominio para este dominio**. El administrador del servidor automáticamente le pide las credenciales válidas, o puedes hacer clic en **cambio**.  
  
Adjuntar un RODC requiere la pertenencia a los grupos Administradores de dominio en Windows Server 2012. El Asistente para la configuración de los servicios de dominio de Active Directory solicita más adelante si sus credenciales actuales no tienen los permisos adecuados o pertenencia a grupos.  
  
La **configuración de implementación** cmdlet de PowerShell de Windows ADDSDeployment y los argumentos son:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opciones del controlador de dominio  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
La **opciones del controlador de dominio** página muestra el dominio opciones del controlador para el nuevo controlador de dominio. Cuando se carga esta página, el Asistente para la configuración de los servicios de dominio de Active Directory envía una consulta LDAP a un controlador de dominio existente para buscar cuentas libres. Si la consulta encuentra un controlador de dominio sin ocupar cuenta de equipo que comparte el mismo nombre que el equipo actual, el asistente muestra un mensaje informativo en la parte superior de la página que dice "**una cuenta RODC creados previamente que coincida con el nombre del servidor de destino está en el directorio. Elige si deseas usar esta cuenta RODC existente o volver a instalar este controlador de dominio**. " Usa el Asistente para la **usar cuenta existente de RODC** como la configuración predeterminada.  
  
> [!IMPORTANT]  
> Puedes usar la **volver a instalar este controlador de dominio** opción cuando un controlador de dominio ha sufrido un problema físico y no puede devolver a la funcionalidad. Esto ahorra tiempo al configurar el controlador de dominio de reemplazo, dejando el controlador de dominio de cuenta de equipo y metadatos en Active Directory del objeto. Instalar el nuevo equipo con el *mismo nombre*y promuevas como un controlador de dominio del dominio. La **volver a instalar este controlador de dominio** opción no está disponible si se ha quitado los metadatos del objeto de controlador de dominio de Active Directory (limpieza de metadatos).  
  
No puedes configurar opciones de controlador de dominio cuando se vincula a una cuenta de equipo RODC un servidor. Configurar las opciones de controlador de dominio cuando se crea la cuenta de equipo RODC provisional.  
  
Especificado **contraseña de modo de restauración de servicios de directorio** deben cumplir con la directiva de contraseñas aplicada al servidor. Siempre elegir una contraseña segura y compleja o preferiblemente, una frase de contraseña.  
  
La **opciones del controlador de dominio** ADDSDeployment Windows PowerShell argumentos son:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> El nombre del sitio debe existir cuando se proporciona como un argumento a **- el nombre de sitio**. La **instalación AddsDomainController** cmdlet no crea los nombres de los sitios. Puedes usar el cmdlet **nueva adreplicationsite** para crear sitios nuevos.  
  
La **instalación ADDSDomainController** argumentos que siguen los valores predeterminados mismo como el administrador del servidor, si no se especifica.  
  
La **SafeModeAdministratorPassword** operación del argumento es especial:  
  
-   Si *no especifica* como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
    Por ejemplo crear un nuevo RODC en la corp.contoso.com y se pide para escribir y Confirmar contraseña enmascarada:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Si se especifica *con un valor*, el valor debe ser una cadena segura. No es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
Por ejemplo, puedes manualmente pedir contraseña utilizando la **Read-Host** cmdlet para pedir al usuario una cadena segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como la opción anterior confirma la contraseña, Ten mucho cuidado: la contraseña no está visible.  
  
También puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Por último, podrías almacenar la contraseña ofuscada en un archivo y, a continuación, volver a utilizarlo más tarde, sin la contraseña de texto no cifrado deja de aparecer. Por ejemplo:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar una contraseña de texto activa o desactiva ofuscados. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo puede invertir esa contraseña ofuscada. Con estos datos, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y finalmente suplantar el propio controlador de dominio, eleve sus privilegios para el nivel más alto en un bosque de AD. Un conjunto adicional de pasos mediante **System.Security.Cryptography** para cifrar el archivo de texto datos están recomendable pero fuera del ámbito. El procedimiento recomendado es evitar por completo el almacenamiento de contraseña.  
  
### <a name="additional-options"></a>Opciones adicionales  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
La **opciones adicionales** página proporciona opciones de configuración para el nombre de un controlador de dominio como el origen de replicación, o puedes usar cualquier controlador de dominio como el origen de replicación.  
  
También puedes instalar el controlador de dominio mediante una copia de seguridad de contenido multimedia con la instalación de la opción de medios (IFM). La **instalar desde medios** casilla proporciona una opción de examinar una vez seleccionados y debe hacer clic **comprobar** para garantizar la ruta de acceso proporcionado es multimedia válidos. Medios usados por la opción IFM se crean con copias de seguridad de Windows Server o Ntdsutil.exe desde otro equipo existente de Windows Server 2012. No puedes usar un Windows Server 2008 R2 o el sistema operativo anterior para crear medios para un controlador de dominio de Windows Server 2012. Para obtener más información sobre los cambios en IFM, consulta [instalación Ntdsutil.exe de cambios en los medios](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Si usando multimedia protegido con una SYSKEY, el administrador del servidor solicita contraseña de la imagen durante la comprobación.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
La **opciones adicionales** ADDSDeployment cmdlet argumentos son:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Rutas de acceso  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
La **rutas de acceso** página te permite invalidar las ubicaciones predeterminadas de la carpeta de la base de datos de AD DS, los registros de transacciones de base de datos, y compartir la carpeta SYSVOL. Las ubicaciones predeterminadas de siempre están en los subdirectorios de % systemroot %. La **rutas de acceso** ADDSDeployment cmdlet argumentos son:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Opciones de revisión y el Script de vista  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
La **opciones de revisión** página te permite validar la configuración y asegurarse de que cumplen los requisitos antes de iniciar la instalación. Esto no es la última oportunidad para detener la instalación con el administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con la configuración. La **opciones de revisión** también ofrece un opcional de la página en el administrador del servidor **ver Script** botón para crear un archivo de texto de Unicode que contiene la configuración actual de ADDSDeployment como una única secuencia de Windows PowerShell. Esto te permite usar la interfaz gráfica de administrador del servidor como un estudio de implementación de Windows PowerShell. Usar al Asistente para la configuración de los servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, Cancelar al asistente. Este proceso crea una muestra sintácticamente correcta y válida para modificaciones adicionales o su uso directo. Por ejemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "corp.contoso.com" `  
-LogPath "C:\Windows\NTDS" `  
-SYSVOLPath "C:\Windows\SYSVOL" `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> Por lo general, rellena el administrador del servidor en todos los argumentos con los valores de promoción y no se basa en valores predeterminados (como pueden cambiar entre las versiones futuras de Windows o service Pack). La única excepción a esto es la **- safemodeadministratorpassword** argumento. Para forzar un aviso de confirmación omitir el valor cuando se ejecuta el cmdlet de manera interactiva  
  
Usar opcional **Whatif** argumento con la **instalación ADDSDomainController** cmdlet para revisar la información de configuración. Esto te permite ver los valores implícitos y explícitos de los argumentos de un cmdlet.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>Comprobación de requisitos previos  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
La **comprobación de requisitos previos** es una nueva característica en la configuración de dominio de AD DS. Esta nueva fase valida que la configuración del servidor es capaz de admitir un nuevo bosque de AD DS.  
  
Al instalar un nuevo dominio raíz, el Asistente de configuración de servicios de Active Directory dominio de Server Manager invoca una serie de pruebas modulares serializadas. Estas pruebas avisan con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso de instalación del controlador de dominio no puede continuar hasta que todos los requisitos previos pruebas pasar.  
  
La **comprobación de requisitos previos** superficies también información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores. Para obtener más información acerca de las comprobaciones de requisitos previos, consulta [comprobación de requisito previo](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
No se puede omitir la **comprobar requisitos previos** cuando usa el administrador del servidor, pero puedes omitir el proceso cuando se usa el cmdlet de implementación de AD DS con el argumento siguiente:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft recomienda no omitiendo comprobar los requisitos previos como que puede provocar una promoción del controlador de dominio parcial o daña el bosque de AD DS.  
  
Haz clic en **instalar** para iniciar el proceso de promoción de controlador de dominio. Esta es la última oportunidad para cancelar la instalación. No se puede cancelar el proceso de promoción una vez que comienza. El equipo se reiniciará automáticamente al final de promoción, independientemente de los resultados de la promoción.  
  
### <a name="installation"></a>Instalación  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
Cuando se muestra la página de instalación, la configuración del controlador de dominio comienza y no se han detenido o cancela. Operaciones detalladas en esta página muestran y se escriben en registros:  
  
-   %systemroot%\Debug\Dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar un nuevo bosque de Active Directory mediante el módulo ADDSDeployment, usa el cmdlet siguiente:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Consulta [adjuntar RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) para argumentos necesarios y opcionales.  
  
La **instalación addsdomaincontroller** cmdlet solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con los argumentos requeridos mínimos de **- NombreDominio**, **- useexistingaccount**, y **-credenciales**. Ten en cuenta cómo hacerlo, al igual que el administrador del servidor, **instalación ADDSDomainController** te recuerda que promoción reiniciará automáticamente el servidor:  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
Para aceptar automáticamente la consulta de reinicio, usa el **-forzar** o **-confirmar: $false** argumentos con cualquier cmdlet ADDSDeployment Windows PowerShell. Para evitar que el servidor reiniciar automáticamente al final de promoción, usa el **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> No se recomienda reemplazar el reinicio. El controlador de dominio debe reiniciar para que funcione correctamente.  
  
### <a name="results"></a>Resultados  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La **resultados** página muestra el éxito o error de la promoción y cualquier información administrativa importante. El controlador de dominio se reiniciará automáticamente después de 10 segundos.  
  
## <a name="rodc-without-staging-workflow"></a>RODC sin el flujo de trabajo de prueba  
El siguiente diagrama ilustra el proceso de configuración de los servicios de dominio de Active Directory, cuando ya habías instalado el rol de AD DS y ha iniciado al dominio servicios de configuración Asistente para Active Directory mediante el administrador del servidor para crear un nuevo controlador de dominio de solo lectura no se almacenan en un dominio de Windows Server 2012 existente.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>RODC sin copia intermedia de Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-El nombre de dominio***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-El nombre de sitio***<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciales***<br /><br />*-CriticalReplicationOnly*<br /><br />*Ruta:*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> La **-credenciales** argumento solo es necesario si no ya sesión como miembro del grupo Domain Admins.  
  
## <a name="rodc-without-staging-deployment"></a>RODC sin la implementación de almacenamiento provisional  
  
### <a name="deployment-configuration"></a>Configuración de implementación  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
El administrador del servidor comienza cada promoción del controlador de dominio con la **configuración de implementación** página. El resto de opciones y los campos obligatorios cambian en esta página y las páginas siguientes, según qué operación de implementación que seleccione.  
  
Para agregar un controlador de dominio de solo lectura no provisional a un dominio de Windows Server 2012 existente, selecciona **agregar un controlador de dominio a un dominio existente** y haz clic en el **selecciona** botón **especificar la información de dominio para este dominio**. El administrador del servidor automáticamente le pide las credenciales válidas, o puedes hacer clic en **cambio**.  
  
Adjuntar un RODC requiere la pertenencia a los grupos Administradores de dominio en Windows Server 2012. El Asistente para la configuración de los servicios de dominio de Active Directory solicita más adelante si sus credenciales actuales no tienen los permisos adecuados o pertenencia a grupos.  
  
La **configuración de implementación** cmdlet de PowerShell de Windows ADDSDeployment y los argumentos son:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opciones del controlador de dominio  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
La **opciones del controlador de dominio** página especifica las funciones de controlador de dominio para el nuevo controlador de dominio. Las capacidades de controlador de dominio puede configurar son **servidor DNS**, **catálogo Global**, y **controlador de dominio de solo lectura**. Microsoft recomienda que todos los controladores de dominio proporcionan servicios DNS y GC alta disponibilidad en entornos distribuidos. GC siempre está activada de manera predeterminada y se selecciona el servidor DNS de manera predeterminada si DNS ya se encuentran en sus controladores de dominio de los hosts de dominio actual en función de consulta de inicio de autoridad.  
  
La **opciones del controlador de dominio** página también te permite elegir la lógica adecuada de Active Directory **nombre del sitio** desde la configuración del bosque. De manera predeterminada, selecciona el sitio con la subred más correcta. Si hay un único sitio, selecciona automáticamente ese sitio.  
  
> [!IMPORTANT]  
> Si el servidor no pertenece a una subred de Active Directory y no hay más de un sitio de Active Directory, se selecciona nada y la **siguiente** botón no está disponible hasta que haya elegido un sitio desde la lista.  
  
Especificado **contraseña de modo de restauración de servicios de directorio** deben cumplir con la directiva de contraseñas aplicada al servidor. Siempre elegir una contraseña segura y compleja o preferiblemente, una frase de contraseña passphrase.The **opciones del controlador de dominio** ADDSDeployment Windows PowerShell argumentos son:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> El nombre del sitio debe existir cuando se proporciona como un argumento a **- el nombre de sitio**. La **instalación AddsDomainController** cmdlet no crea los nombres de los sitios. Puedes usar el cmdlet **nueva adreplicationsite** para crear sitios nuevos.  
  
La **instalación ADDSDomainController** argumentos que siguen los valores predeterminados mismo como el administrador del servidor, si no se especifica.  
  
La **SafeModeAdministratorPassword** operación del argumento es especial:  
  
-   Si *no especifica* como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
    Por ejemplo crear un nuevo RODC en la corp.contoso.com y se pide para escribir y Confirmar contraseña enmascarada:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Si se especifica *con un valor*, el valor debe ser una cadena segura. No es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
Por ejemplo, puedes manualmente pedir contraseña utilizando la **Read-Host** cmdlet para pedir al usuario una cadena segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como la opción anterior confirma la contraseña, Ten mucho cuidado: la contraseña no está visible.  
  
También puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Por último, podrías almacenar la contraseña ofuscada en un archivo y, a continuación, volver a utilizarlo más tarde, sin la contraseña de texto no cifrado deja de aparecer. Por ejemplo:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar una contraseña de texto activa o desactiva ofuscados. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo puede invertir esa contraseña ofuscada. Con estos datos, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y finalmente suplantar el propio controlador de dominio, eleve sus privilegios para el nivel más alto en un bosque de AD. Un conjunto adicional de pasos mediante **System.Security.Cryptography** para cifrar el archivo de texto datos están recomendable pero fuera del ámbito. El procedimiento recomendado es evitar por completo el almacenamiento de contraseña.  
  
### <a name="rodc-options"></a>Opciones de RODC  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
La **RODC opciones** página te permite modificar la configuración:  
  
-   Cuenta de administrador delegado  
  
-   Cuentas que tienen permiso para replicar las contraseñas en el RODC  
  
-   Cuentas que se deniegan de replicación de contraseñas en el RODC  
  
Cuentas de administrador delegado tener permisos administrativos locales en el RODC. Estos usuarios pueden funcionar con privilegios equivalentes al grupo de administradores del equipo local.  No son miembros de los administradores de dominio o los grupos Administradores de dominio integrados. Esta opción es útil para delegar la administración de office rama sin dar un vistazo a los permisos de administrador de dominio. No es necesario configurar la delegación de administración.  
  
Es el equivalente argumento ADDSDeployment Windows PowerShell:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
Las cuentas que no están permitidas en caché las contraseñas en el RODC y no pueden conectarse y autenticarse en un controlador de dominio grabable no pueden acceder a recursos o funcionalidad proporcionada por Active Directory.  
  
> [!IMPORTANT]  
> Si no se modifican, se usa los grupos predeterminados y la configuración:  
>   
> -   Los administradores - denegar  
> -   Los operadores de servidor - denegar  
> -   Una copia de seguridad de los operadores - denegar  
> -   Cuenta operadores - denegar  
> -   Denegar denegado RODC contraseña Replication Group-  
> -   Permite RODC contraseña Replication Group - permitir  
  
Los argumentos de ADDSDeployment Windows PowerShell equivalentes son:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>Opciones adicionales  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
La **opciones adicionales** página proporciona opciones de configuración para el nombre de un controlador de dominio como el origen de replicación, o puedes usar cualquier controlador de dominio como el origen de replicación.  
  
También puedes instalar el controlador de dominio mediante una copia de seguridad de contenido multimedia con la instalación de la opción de medios (IFM). La **instalar desde medios** casilla proporciona una opción de examinar una vez seleccionados y debe hacer clic **comprobar** para garantizar la ruta de acceso proporcionado es multimedia válidos. Medios usados por la opción IFM se crean con copias de seguridad de Windows Server o Ntdsutil.exe desde otro equipo existente de Windows Server 2012. No puedes usar un Windows Server 2008 R2 o el sistema operativo anterior para crear medios para un controlador de dominio de Windows Server 2012.  Los apéndices proporciona más información sobre los cambios en IFM. Si usando multimedia protegido con una SYSKEY, el administrador del servidor solicita contraseña de la imagen durante la comprobación.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
Los argumentos de cmdlet adicionales ADDSDeployment opciones son:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Rutas de acceso  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
La **rutas de acceso** página te permite invalidar las ubicaciones predeterminadas de la carpeta de la base de datos de AD DS, los registros de transacciones de base de datos, y compartir la carpeta SYSVOL. Las ubicaciones predeterminadas de siempre están en los subdirectorios de % systemroot %. La **rutas de acceso** ADDSDeployment cmdlet argumentos son:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opciones de preparación  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
La **preparación opciones** página le avisa de que la configuración de AD DS incluye extender el esquema (forestprep) y actualización del dominio (domainprep). Solo verás esta página cuando el dominio o bosque no se ha preparado instalación anterior del controlador de dominio de Windows Server 2012 o ejecuten manualmente Adprep.exe. Por ejemplo, el Asistente para la configuración de los servicios de dominio de Active Directory suprime esta página, si agregas un nuevo controlador de dominio de réplica a un dominio de raíz del bosque existente de Windows Server 2012.  
  
Ampliar el esquema y actualización del dominio no se producen cuando haces clic en **siguiente**. Estos eventos se producen solo durante la fase de instalación. Esta página ofrece simplemente conocimiento sobre los eventos que se produzca más adelante en la instalación.  
  
Esta página también valida que las credenciales del usuario actual son miembros del grupo de administradores de esquema y administradores de empresa, como necesites pertenencia a estos grupos para ampliar el esquema o preparar un dominio. Haz clic en **cambio** para proporcionar las credenciales de usuario adecuado si la página indica que las credenciales actuales no proporcionan los permisos necesarios.  
  
Es el argumento de cmdlet ADDSDeployment de opciones adicionales:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Como con versiones anteriores de Windows Server, preparación de dominio automatizada de Windows Server 2012 no ejecuta GPPREP. Ejecutar **adprep.exe /gpprep** manualmente para todos los dominios que no se han preparado anteriormente para Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. Debes ejecutar solo una vez GPPrep en el historial de un dominio, no con cada actualización. No se ejecuta Adprep.exe /gpprep automáticamente porque su operación para hacer que todos los archivos y carpetas en la carpeta SYSVOL para volver a replicar en todos los controladores de dominio.  
>   
> RODCPrep automática se ejecuta cuando se promueve el primer RODC detenido provisional de un dominio. No se produce cuando se promueve el primer controlador de dominio grabable de Windows Server 2012. Puedes ejecutar también manualmente **adprep.exe /rodcprep** si vas a implementar controladores de dominio de solo lectura.  
  
### <a name="review-options-and-view-script"></a>Opciones de revisión y el Script de vista  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
La **opciones de revisión** página te permite validar la configuración y asegurarse de que cumplen los requisitos antes de iniciar la instalación. Esto no es la última oportunidad para detener la instalación con el administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con la configuración.  
  
La **opciones de revisión** también ofrece un opcional de la página en el administrador del servidor **ver Script** botón para crear un archivo de texto de Unicode que contiene la configuración actual de ADDSDeployment como una única secuencia de Windows PowerShell. Esto te permite usar la interfaz gráfica de administrador del servidor como un estudio de implementación de Windows PowerShell. Usar al Asistente para la configuración de los servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, Cancelar al asistente. Este proceso crea una muestra sintácticamente correcta y válida para modificaciones adicionales o su uso directo. Por ejemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @("CORP\Allowed RODC Password Replication Group", "CORP\Chicago RODC Admins", "CORP\Chicago RODC Users and Computers") `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DelegatedAdministratorAccountName "CORP\Chicago RODC Admins" `  
-DenyPasswordReplicationAccountName @("BUILTIN\Administrators", "BUILTIN\Server Operators", "BUILTIN\Backup Operators", "BUILTIN\Account Operators", "CORP\Denied RODC Password Replication Group") `  
-DomainName "corp.contoso.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-ReadOnlyReplica:$true `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Por lo general, rellena el administrador del servidor en todos los argumentos con los valores de promoción y no se basa en valores predeterminados (como pueden cambiar entre las versiones futuras de Windows o service Pack). La única excepción a esto es la **- safemodeadministratorpassword** argumento. Para forzar a un mensaje de confirmación, omite el valor cuando se ejecuta el cmdlet de manera interactiva.  
  
Usa el argumento Whatif opcional con el cmdlet Install-ADDSDomainController para revisar la información de configuración. Esto te permite ver los valores implícitos y explícitos de los argumentos de un cmdlet.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>Comprobación de requisitos previos  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
La **comprobación de requisitos previos** es una nueva característica en la configuración de dominio de AD DS. Esta nueva fase valida que la configuración del servidor es capaz de admitir un nuevo bosque de AD DS.  
  
Al instalar un nuevo dominio raíz, el Asistente de configuración de servicios de Active Directory dominio de Server Manager invoca una serie de pruebas modulares serializadas. Estas pruebas avisan con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso de controlador de dominio no puede continuar hasta que todos los requisitos previos pruebas pasar.  
  
La **comprobación de requisitos previos** superficies también información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.  
  
No se puede omitir la **comprobar requisitos previos** cuando usa el administrador del servidor, pero puedes omitir el proceso cuando se usa el cmdlet de implementación de AD DS con el argumento siguiente:  
  
```  
-skipprechecks  
  
```  
  
Haz clic en **instalar** para iniciar el proceso de promoción de controlador de dominio. Esta es la última oportunidad para cancelar la instalación. No se puede cancelar el proceso de promoción una vez que comienza. El equipo se reiniciará automáticamente al final de promoción, independientemente de los resultados de la promoción.  
  
### <a name="installation"></a>Instalación  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
Cuando la **instalación** muestra la página, la configuración del controlador de dominio comienza y no se han detenido o cancela. Operaciones detalladas en esta página muestran y se escriben en registros:  
  
-   %systemroot%\Debug\Dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar un nuevo bosque de Active Directory mediante el módulo ADDSDeployment, usa el cmdlet siguiente:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Consulta la **ADDSDeployment Cmdlet** tabla en la begininng de esta sección para argumentos necesarias y opcionales.  
  
La **instalación addsdomaincontroller** cmdlet solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con los argumentos requeridos mínimos de **- NombreDominio**, **- readonlyreplica**, **- el nombre de sitio**, y **-credenciales**. Ten en cuenta cómo hacerlo, al igual que el administrador del servidor, **instalación ADDSDomainController** te recuerda que promoción reiniciará automáticamente el servidor:  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
Para aceptar automáticamente la consulta de reinicio, usa el **-forzar** o **-confirmar: $false** argumentos con cualquier cmdlet ADDSDeployment Windows PowerShell. Para evitar que el servidor reiniciar automáticamente al final de promoción, usa el **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> No se recomienda reemplazar el reinicio. El controlador de dominio debe reiniciar para que funcione correctamente. Si inicias sesión desactivar el controlador de dominio, no puede iniciar atrás forma interactiva hasta que reinicie.  
  
### <a name="results"></a>Resultados  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
La **resultados** página muestra el éxito o error de la promoción y cualquier información administrativa importante. El controlador de dominio se reiniciará automáticamente después de 10 segundos.  
  

