---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: "Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente (nivel 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e3151a8beee2870ecc747a64241df9d562ad4cc2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe los pasos necesarios para actualizar un dominio o bosque existentes a Windows Server 2012, con el administrador del servidor o Windows PowerShell. Hablaremos sobre cómo agregar controladores de dominio que ejecutan Windows Server 2012a un dominio existente.  
  
-   [Flujo de trabajo de réplica y actualización](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Actualización y réplica Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implementación](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>Flujo de trabajo de réplica y actualización  
El siguiente diagrama ilustra el proceso de configuración de los servicios de dominio de Active Directory cuando ya habías instalado el rol de AD DS y ha iniciado al dominio servicios de configuración de Asistente para Active Directory mediante el administrador del servidor para crear un nuevo controlador de dominio en un dominio existente.  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>Actualización y réplica Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-El nombre de dominio***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-El nombre de sitio*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirmar<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciales***<br /><br />-CriticalReplicationOnly<br /><br />*Ruta:*<br /><br />*-DNSDelegationCredential*<br /><br />-Force<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-El nombre de sitio<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> La **-credenciales** argumento solo es necesario si no ya sesión como miembro del grupo Domain Admins o de los grupos Administradores de empresa y administradores de esquema (si estás actualizando el bosque) (si vas a agregar un nuevo controlador de dominio a un dominio existente).  
  
## <a name="BKMK_Dep"></a>Implementación  
  
### <a name="deployment-configuration"></a>Configuración de implementación  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
El administrador del servidor comienza cada promoción del controlador de dominio con la **configuración de implementación** página. El resto de opciones y los campos obligatorios cambian en esta página y las páginas siguientes, según qué operación de implementación que seleccione.  
  
Para actualizar un bosque existente o agregar un controlador de dominio grabable a un dominio existente, haz clic en **agregar un controlador de dominio a un dominio existente** y haz clic en **selecciona** a **especificar la información de dominio para este dominio**. El administrador del servidor le solicita las credenciales válidas si es necesario.  
  
Actualizar el bosque requiere credenciales que incluyen las pertenencias a grupos en grupos de los administradores de empresa y la administradores de esquema de Windows Server 2012. El Asistente para la configuración de los servicios de dominio de Active Directory solicita más adelante si sus credenciales actuales no tienen los permisos adecuados o pertenencia a grupos.  
  
El proceso de Adprep automático es la diferencia entre agregar un controlador de dominio a un dominio existente de Windows Server 2012 y un dominio donde los controladores de dominio ejecutan una versión anterior de Windows Server solo operativa.  
  
El cmdlet ADDSDeployment de configuración de implementación y los argumentos son:  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
Algunas pruebas realizan en cada página, algunos de los cuales repiten más adelante como discretos comprobaciones de requisitos previos. Por ejemplo, si el dominio seleccionado no cumple con los niveles funcionales mínimos, no tienes que ir pasando promoción para comprobar los requisitos previos para averiguar:  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Opciones del controlador de dominio  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
La **opciones del controlador de dominio** página especifica las funciones de controlador de dominio para el nuevo controlador de dominio. Las capacidades de controlador de dominio puede configurar son **servidor DNS**, **catálogo Global**, y **controlador de dominio de solo lectura**. Microsoft recomienda que todos los controladores de dominio proporcionan servicios DNS y GC alta disponibilidad en entornos distribuidos. GC siempre está activada de manera predeterminada y se selecciona el servidor DNS de manera predeterminada si DNS ya se encuentran en sus controladores de dominio de los hosts de dominio actual en función de consulta de inicio de autoridad. La **opciones del controlador de dominio** página también te permite elegir la lógica adecuada de Active Directory **nombre del sitio** desde la configuración del bosque. De manera predeterminada, selecciona el sitio con la subred más correcta. Si hay un único sitio, se seleccionan automáticamente.  
  
> [!NOTE]  
> Si el servidor no pertenece a una subred de Active Directory y no hay más de un sitio de Active Directory, se selecciona nada y la **siguiente** botón no está disponible hasta que haya elegido un sitio desde la lista.  
  
Especificado **contraseña de modo de restauración de servicios de directorio** deben cumplir con la directiva de contraseñas aplicada al servidor. Siempre elegir una contraseña segura y compleja o preferiblemente, una frase de contraseña.  
  
La **opciones del controlador de dominio** ADDSDeployment argumentos son:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> El nombre del sitio debe existir cuando se proporciona como un argumento a **- el nombre de sitio**. La **instalación AddsDomainController** cmdlet no crea sitios. Puedes usar el cmdlet **nueva adreplicationsite** para crear sitios nuevos.  
  
La **SafeModeAdministratorPassword** operación del argumento es especial:  
  
-   Si *no especifica* como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
    Por ejemplo crear un controlador de dominio en el dominio treyresearch.net y se pide para escribir y Confirmar contraseña enmascarada:  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
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
> No se recomienda proporcionar ni almacenar una contraseña de texto activa o desactiva ofuscados. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo puede invertir esa contraseña ofuscada. Con estos datos, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y finalmente suplantar el propio controlador de dominio, eleve sus privilegios para el nivel más alto de un bosque de Active Directory. Un conjunto adicional de pasos mediante **System.Security.Cryptography** para cifrar el archivo de texto datos están recomendable pero fuera del ámbito. El procedimiento recomendado es evitar por completo el almacenamiento de contraseña.  
  
El cmdlet ADDSDeployment ofrece una opción adicional para omitir la configuración automática de configuración del cliente DNS, servidores de reenvío y sugerencias de raíz. No se puede omitir esta opción de configuración cuando se usa el administrador del servidor. Este argumento es importante solo si has instalado el rol de servidor DNS antes de configurar el controlador de dominio:  
  
```  
-SkipAutoConfigureDNS  
```  
  
La **opciones del controlador de dominio** página avisa que no se puede crear controladores de dominio de solo lectura si los controladores de dominio ejecutan Windows Server 2003. Esto se espera, y puede descartar la advertencia.  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opciones de DNS y las credenciales de la delegación de DNS  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
La **opciones DNS** página te permite configurar la delegación de DNS si seleccionaste la **servidor DNS** opción en la *opciones del controlador de dominio* página y, si apunta a una zona donde se permite delegación de DNS. Tienes que proporcionar credenciales alternativas de un usuario que es un miembro de la **administradores DNS** grupo.  
  
La **opciones DNS** ADDSDeployment cmdlet argumentos son:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
Para obtener más información acerca de si es necesario crear una delegación de DNS, consulta [acerca de la delegación zona](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opciones adicionales  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
La **opciones adicionales** página ofrece la opción de configuración de nombre de un controlador de dominio como el origen de replicación, o puedes usar cualquier controlador de dominio como el origen de replicación.  
  
También puedes instalar el controlador de dominio mediante una copia de seguridad de contenido multimedia con la instalación de la opción de medios (IFM). La **instalar desde medios** casilla proporciona una opción de examinar una vez seleccionados y debe hacer clic **comprobar** para garantizar la ruta de acceso proporcionado es multimedia válidos. Medios usados por la opción IFM se crean con copias de seguridad de Windows Server o Ntdsutil.exe desde otro equipo existente de Windows Server 2012. No puedes usar un Windows Server 2008 R2 o el sistema operativo anterior para crear medios para un controlador de dominio de Windows Server 2012. Para obtener más información sobre los cambios en IFM, consulta [apéndice de administración simplificada](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Si usando multimedia protegido con una SYSKEY, el administrador del servidor solicita contraseña de la imagen durante la comprobación.  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
La **opciones adicionales** ADDSDeployment cmdlet argumentos son:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Rutas de acceso  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
La **rutas de acceso** página te permite invalidar las ubicaciones predeterminadas de la carpeta de la base de datos de AD DS, los registros de transacciones de base de datos, y compartir la carpeta SYSVOL. Las ubicaciones predeterminadas de siempre están en los subdirectorios de % systemroot %.  
  
Los argumentos de cmdlet ADDSDeployment de rutas de acceso de Active Directory son:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opciones de preparación  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
La **preparación opciones** página le avisa de que la configuración de AD DS incluye extender el esquema (forestprep) y actualización del dominio (domainprep).  Solo verás esta página cuando el bosque y dominio no han preparado instalación anterior del controlador de dominio de Windows Server 2012 o ejecuten manualmente Adprep.exe. Por ejemplo, el Asistente para la configuración de los servicios de dominio de Active Directory suprime esta página, si agregas un nuevo controlador de dominio a un dominio de raíz del bosque existente de Windows Server 2012.  
  
Ampliar el esquema y actualización del dominio no se producen cuando haces clic en **siguiente**. Estos eventos se producen solo durante la fase de instalación. Esta página ofrece simplemente conocimiento sobre los eventos que se produzca más adelante en la instalación.  
  
Esta página también valida que las credenciales del usuario actual son miembros del grupo de administradores de esquema y administradores de empresa, como necesites pertenencia a estos grupos para ampliar el esquema o preparar un dominio. Haz clic en **cambio** para proporcionar las credenciales de usuario adecuado si la página indica que las credenciales actuales no proporcionan los permisos necesarios.  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
Es el argumento de cmdlet ADDSDeployment de opciones adicionales:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Como con versiones anteriores de Windows Server, preparación de dominio automatizada para controladores de dominio que ejecutan Windows Server 2012 no ejecuta GPPREP. Ejecutar **adprep.exe /gpprep** manualmente para todos los dominios que no se han preparado anteriormente para Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. Debes ejecutar solo una vez GPPrep en el historial de un dominio, no con cada actualización. No se ejecuta Adprep.exe /gpprep automáticamente porque su operación para hacer que todos los archivos y carpetas en la carpeta SYSVOL para volver a replicar en todos los controladores de dominio.  
>   
> RODCPrep automática se ejecuta cuando se promueve el primer RODC detenido provisional de un dominio. No se produce cuando se promueve el primer controlador de dominio grabable de Windows Server 2012. Puedes seguir manualmente **adprep.exe /rodcprep** si vas a implementar controladores de dominio de solo lectura.  
  
### <a name="review-options-and-view-script"></a>Opciones de revisión y el Script de vista  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
La **opciones de revisión** página te permite validar la configuración y asegurarse de que cumplen los requisitos antes de iniciar la instalación. Esto no es la última oportunidad para detener la instalación con el administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con la configuración.  
  
La **opciones de revisión** también ofrece un opcional de la página en el administrador del servidor **ver Script** botón para crear un archivo de texto de Unicode que contiene la configuración actual de ADDSDeployment como una única secuencia de Windows PowerShell. Esto te permite usar la interfaz gráfica de administrador del servidor como un estudio de implementación de Windows PowerShell. Usar al Asistente para la configuración de los servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, Cancelar al asistente.  Este proceso crea una muestra sintácticamente correcta y válida para modificaciones adicionales o su uso directo.  
  
Por ejemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "root.fabrikam.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Por lo general, rellena el administrador del servidor en todos los argumentos con los valores de promoción y no se basa en valores predeterminados (como pueden cambiar entre las versiones futuras de Windows o service Pack). La única excepción a esto es la **- safemodeadministratorpassword** argumento. Para forzar un aviso de confirmación omitir el valor cuando se ejecuta el cmdlet de manera interactiva  
>   
> Usar opcional **Whatif** argumento con la **instalación ADDSDomainController** cmdlet para revisar la información de configuración. Esto te permite ver los valores implícitos y explícitos de los argumentos de un cmdlet.  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Comprobación de requisitos previos  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
La **comprobación de requisitos previos** es una nueva característica en la configuración de dominio de AD DS. Esta nueva fase valida que los dominios y bosques son capaces de admitir un nuevo controlador de dominio de Windows Server 2012.  
  
Al instalar un nuevo controlador de dominio, el Asistente de configuración de servicios de Active Directory dominio de Server Manager invoca una serie de pruebas modulares serializadas. Estas pruebas avisan con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso de controlador de dominio no puede continuar hasta que todos los requisitos previos pruebas pasar.  
  
La **comprobación de requisitos previos** superficies también información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.  
  
Para obtener más información sobre los requisitos previos controles específicos, consulta [comprobación de requisito previo](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
No se puede omitir la **comprobar requisitos previos** cuando usa el administrador del servidor, pero puedes omitir el proceso cuando se usa el cmdlet de implementación de AD DS con el argumento siguiente:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft recomienda no omitiendo comprobar los requisitos previos como que puede provocar una promoción del controlador de dominio parcial o daña el bosque de AD DS.  
  
Haz clic en **instalar** para iniciar el proceso de promoción de controlador de dominio. Esta es la última oportunidad para cancelar la instalación. No se puede cancelar el proceso de promoción una vez que comienza. El equipo reiniciará automáticamente al final de promoción, independientemente de la promoción results.The **comprobación de requisitos previos** página muestra los problemas que encontró durante el proceso y las instrucciones para resolver el problema.  
  
### <a name="installation"></a>Instalación  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Cuando la **instalación** muestra la página, la configuración del controlador de dominio comienza y no se han detenido o cancela. Operaciones detalladas en esta página muestran y se escriben en registros:  
  
-   %systemroot%\Debug\Dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log (si el servidor está en un grupo de trabajo)  
  
Para instalar un nuevo bosque de Active Directory mediante el módulo ADDSDeployment, usa el cmdlet siguiente:  
  
```  
Install-addsdomaincontroller  
```  
  
Consulta [réplica Windows PowerShell y actualización](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS) para argumentos necesarios y opcionales.  
  
La **instalación AddsDomainController** cmdlet solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con los argumentos requeridos mínimos de **- NombreDominio** y **-credenciales**. Ten en cuenta cómo la operación de Adprep se produce automáticamente como parte de agregar el primer controlador de dominio de Windows Server 2012a un bosque existente de Windows Server 2003:  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Ten en cuenta cómo hacerlo, al igual que el administrador del servidor, **instalación ADDSDomainController** te recuerda que promoción reiniciará automáticamente el servidor. Para aceptar automáticamente la consulta de reinicio, usa el **-forzar** o **-confirmar: $false** argumentos con cualquier cmdlet ADDSDeployment Windows PowerShell. Para evitar que el servidor reiniciar automáticamente al final de promoción, usa el **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> No se recomienda reemplazar el reinicio. El controlador de dominio debe reiniciar para que funcione correctamente.  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Para configurar un controlador de dominio mediante Windows PowerShell de forma remota, ajustar el **instalación adddomaincontroller** cmdlet *dentro* de la **comando invocar** cmdlet. Esto requiere el uso de las llaves.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Por ejemplo:  
  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Para obtener más información sobre cómo la instalación y Adprep funciona el proceso, consulta el [solución de problemas de implementación del controlador de dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Resultados  
![Instalar una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La **resultados** página muestra el éxito o error de la promoción y cualquier información administrativa importante. Si se realiza correctamente, el controlador de dominio se reiniciará automáticamente después de 10 segundos.  
  
Al igual que con versiones anteriores de Windows Server, preparación de dominio automatizada para controladores de dominio que ejecutan Windows server 2012 no ejecuta GPPREP. Ejecutar **adprep.exe /gpprep** manualmente para todos los dominios que no se han preparado anteriormente para Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2. Debes ejecutar solo una vez GPPrep en el historial de un dominio, no con cada actualización. No se ejecuta Adprep.exe /gpprep automáticamente porque su operación para hacer que todos los archivos y carpetas en la carpeta SYSVOL para volver a replicar en todos los controladores de dominio.  
  

