---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: Instalar Servicios de dominio de Active Directory (Nivel 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fcb6d90832ed032302ceb0b3c4ec6a0eaff7d807
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886876"
---
# <a name="install-active-directory-domain-services-level-100"></a>Instalar Servicios de dominio de Active Directory (Nivel 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica cómo instalar AD DS en Windows Server 2012 mediante el uso de cualquiera de los métodos siguientes:  
  
-   [Requisitos de credenciales para ejecutar Adprep.exe e instalar Active Directory Domain Services](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [Instalación de AD DS mediante Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [Instalación de AD DS mediante el administrador del servidor](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [Instalación de RODC por fases mediante la interfaz gráfica de usuario](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>Requisitos de credenciales para ejecutar Adprep.exe e instalar Active Directory Domain Services  
Se necesitan las siguientes credenciales para ejecutar Adprep.exe e instalar AD DS.  
  
-   Para instalar un nuevo bosque, debe iniciar sesión como Administrador local del equipo.  
  
-   Para instalar un nuevo dominio secundario o un nuevo árbol de dominios, debe haber iniciado sesión como miembro del grupo Administradores de empresas.  
  
-   Para instalar un controlador de dominio adicional en un dominio existente, debe ser miembro del grupo Admins. del dominio.  
  
    > [!NOTE]  
    > Si no ejecuta comandos de adprep.exe por separado y va a instalar el primer controlador de dominio que ejecuta Windows Server 2012 en un dominio o bosque existente, se le pedirá que proporcione las credenciales para ejecutar los comandos Adprep. Los requisitos de credenciales son los siguientes:  
    >   
    > -   Para introducir el primer controlador de dominio de Windows Server 2012 en el bosque, debe proporcionar credenciales para un miembro del grupo de administradores de empresas, el grupo de administradores de esquema y grupo de administradores del dominio en el dominio que hospeda el maestro de esquema.  
    > -   Para introducir el primer controlador de dominio de Windows Server 2012 en un dominio, debe proporcionar credenciales para un miembro del grupo Admins. del dominio.  
    > -   Para introducir el primer controlador de dominio de solo lectura (RODC) en el bosque, debe proporcionar credenciales para un miembro del grupo Administradores de empresas.  
    >   
    >     > [!NOTE]  
    >     > Si ya ha ejecutado adprep /rodcprep en Windows Server 2008 o Windows Server 2008 R2, no es necesario para volver a ejecutarlo para Windows Server 2012.  
  
## <a name="BKMK_PS"></a>Instalación de AD DS mediante Windows PowerShell  
A partir de Windows Server 2012, puede instalar AD DS mediante Windows PowerShell. Dcpromo.exe está en desuso a partir de Windows Server 2012, pero aún puede ejecutar dcpromo.exe mediante un archivo de respuesta (dcpromo / unattend:<answerfile> o dcpromo/answer:<answerfile>). La capacidad de continuar ejecutando dcpromo.exe con un archivo de respuesta les proporciona a las organizaciones que cuentan con los recursos invertidos en automatización existente el tiempo para convertir la automatización de dcpromo.exe a Windows PowerShell. Para obtener más información sobre la ejecución de dcpromo.exe con un archivo de respuesta, consulte [ https://support.microsoft.com/kb/947034 ](https://support.microsoft.com/kb/947034).  
  
Para obtener más información acerca de cómo quitar AD DS mediante Windows PowerShell, consulte el tema sobre la [eliminación de AD DS mediante Windows PowerShell](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS).  
  
Comience por agregar el rol mediante Windows PowerShell. Este comando instala el rol de servidor de AD DS y las herramientas de administración de AD DS y del servidor de AD LDS, incluidas las herramientas basadas en la GUI como Usuarios y equipos de Active Directory y herramientas de la línea de comandos como dcdia.exe. Las herramientas de administración del servidor no se instalan de forma predeterminada al usar Windows PowerShell. Debe especificar **"IncludeManagementTools** para administrar el servidor local o instalar [herramientas de administración remota del servidor](https://www.microsoft.com/download/details.aspx?id=28972) para administrar un servidor remoto.  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
No es necesario reiniciar hasta después de que la instalación de AD DS esté completa.  
  
Después podrá ejecutar este comando para ver los cmdlets disponibles en el módulo ADDSDeployment.  
  
```  
Get-Command -Module ADDSDeployment
```  
  
Para ver la lista de argumentos que se pueden especificar para cmdlets y la sintaxis:  
  
```  
Get-Help <cmdlet name>  
```  
  
Por ejemplo, para ver los argumentos para crear una cuenta de controlador de dominio de solo lectura (RODC) no ocupado, escriba  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
Los argumentos opcionales aparecen entre corchetes.  
  
También puede descargar los ejemplos de Ayuda más recientes y conceptos para cmdlets de Windows PowerShell. Para obtener más información, consulte [about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx).  
  
Puede ejecutar los cmdlets de Windows PowerShell contra servidores remotos:  
  
-   En Windows PowerShell, use Invoke-Command con el cmdlet de ADDSDeployment. Por ejemplo, para instalar AD DS en un servidor remoto denominado ConDC3 en el dominio contoso.com, escriba:  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
-o bien-  
  
-   En el Administrador del servidor, cree un grupo de servidores que incluya el servidor remoto. Haga clic con el botón secundario en el nombre del servidor remoto y haga clic en **Windows PowerShell**.  
  
En las secciones siguientes se explica cómo ejecutar los cmdlets del módulo ADDSDeployment para instalar AD DS.  
  
-   [Argumentos del cmdlet ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [Especificar las credenciales de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [Uso de cmdlets de prueba](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [Instalación de un dominio raíz del bosque mediante Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [Instalar un nuevo dominio secundario o árbol mediante Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [Instalar un controlador de dominio adicional (réplica) mediante Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>Argumentos del cmdlet ADDSDeployment  
En la tabla siguiente se muestran los argumentos para los cmdlets ADDSDeployment de Windows PowerShell. Los argumentos en negrita son obligatorios. Los argumentos equivalentes para dcpromo.exe se muestran entre paréntesis en la lista si se denominan de otra manera en Windows PowerShell.  
  
Los conmutadores de Windows PowerShell aceptan los argumentos $TRUE o $FALSE. No es necesario especificar los argumentos que son $TRUE de forma predeterminada.  
  
Para reemplazar los valores predeterminados, se puede especificar el argumento con un valor $False. Por ejemplo, como **-installdns** se ejecuta automáticamente para la instalación de un bosque nuevo si no se especifica, la única forma para *impedir* la instalación de DNS cuando se instala un bosque nuevo consiste en usar :  
  
```  
-InstallDNS:$false  
```  
  
De forma similar, porque **"installdns** tiene un valor predeterminado de $False si instala un controlador de dominio en un entorno que no hospeda el servidor DNS de Windows server, debe especificar el argumento siguiente con el fin de instalar el servidor DNS:  
  
```  
-InstallDNS:$true  
```  
  
|Argumento|Descripción|  
|------------|---------------|  
|**ADPrepCredential <PS Credential>**  **Nota:** Necesario si va a instalar el primer controlador de dominio de Windows Server 2012 en un dominio o bosque y las credenciales del usuario actual son insuficientes para realizar la operación.|Especifica la cuenta con la pertenencia de grupo de administradores de empresas y administradores de esquema que puede preparar el bosque, según las reglas de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) y un objeto PSCredential.<br /><br />Si se especifica ningún valor, el valor de la **"credencial** se usa el argumento.|  
|AllowDomainControllerReinstall|Especifica si se debe continuar con la instalación de este controlador de dominio de escritura a pesar de que se detecte otra cuenta de controlador de dominio de escritura con el mismo nombre.<br /><br />Use **$True** únicamente si está seguro de que la cuenta no se encuentra en uso por otro controlador de dominio de escritura.<br /><br />El valor predeterminado es **$False**.<br /><br />Este argumento no es válido para un RODC.|  
|AllowDomainReinstall|Especifica si se crea de nuevo un dominio existente.<br /><br />El valor predeterminado es **$False**.|  
|AllowPasswordReplicationAccountName <cadena []>|Especifica los nombres de las cuentas de usuario, de grupo y de equipo cuyas contraseñas se pueden replicar en este controlador de dominio de solo lectura (RODC). Use una cadena vacía "" si desea mantener el valor vacío. De forma predeterminada, solo se permite el Grupo de replicación de contraseña RODC permitida, y originalmente se crea vacío.<br /><br />Proporcione los valores como una matriz de cadenas. Por ejemplo:<br /><br />Código - AllowPasswordReplicationAccountName "JSmith", "JSmithPC", "Usuarios de la sucursal"|  
|ApplicationPartitionsToReplicate < string [] > **Nota:** No existe ninguna opción equivalente en la UI. Si se instala mediante la UI, o con IFM, todas las particiones de la partición se replicarán.|Especifica las particiones de directorio de la aplicación que se replicarán. Este argumento se aplica únicamente cuando se especifica el argumento **-InstallationMediaPath** para instalar desde los medios (IFM). De forma predeterminada, todas las aplicaciones de la partición se replicarán según sus propios ámbitos.<br /><br />Proporcione los valores como una matriz de cadenas. Por ejemplo:<br /><br />Código:<br /><br />-ApplicationPartitionsToReplicate "Partición1", "partición2", "partition3"|  
|Confirm|Solicita confirmación antes de ejecutar el cmdlet.|  
|CreateDnsDelegation **Nota:** Este argumento no se puede especificar cuando se ejecuta el cmdlet Add-ADDSReadOnlyDomainController.|Indica si se creará una delegación DNS que hace referencia al nuevo servidor DNS que se está instalando junto con el controlador de dominio. Válido para "integrado en Active Directory DNS solo. Los registros de delegación se pueden crear únicamente en servidores DNS de Microsoft DNS que están en línea y son accesibles. Los registros de delegación no se pueden crear para dominios que se subordinan inmediatamente a dominios de nivel superior como .com, .gov, .biz, .edu o dominios de código de país de dos letras como .nz y .au.<br /><br />El valor predeterminado se calcula automáticamente según el entorno.|  
|**Credencial <PS Credential>**  **Nota:** Es obligatorio únicamente si las credenciales del usuario actual son insuficientes para realizar la operación.|Especifica la cuenta de dominio que se puede iniciar sesión en el dominio, según las reglas de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) y un objeto PSCredential.<br /><br />Si no se especifica ningún valor, se usan las credenciales del usuario actual.|  
|CriticalReplicationOnly|Especifica si la operación de instalación de AD DS realiza solo replicación crítica antes del reinicio y, a continuación, continúa. La replicación no crítica tiene lugar tras finalizar la instalación y reiniciar el equipo.<br /><br />No se recomienda el uso de este argumento.<br /><br />No existe ningún equivalente para esta opción en la interfaz de usuario (UI).|  
|DatabasePath <string>|Especifica el completo, que no sea "ruta de acceso de convención de nomenclatura Universal (UNC) a un directorio en un disco fijo del equipo local que contiene la base de datos de dominio, por ejemplo, **C:\Windows\NTDS.**<br /><br />El valor predeterminado es **%SYSTEMROOT%\NTDS**. **Importante:** Si bien puede almacenar la base de datos de AD DS y archivos de registro en un volumen cuyo formato se aplicó con el Sistema de archivos resistente (ReFS), no existen beneficios específicos para hospedar AD DS en ReFS, más que los beneficios normales de resistencia que puede obtener hospedando cualquier dato en ReFS.|  
|DelegatedAdministratorAccountName <string>|Especifica el nombre del usuario o del grupo que puede instalar y administrar el RODC.<br /><br />De forma predeterminada, únicamente los miembros del grupo Admins. del dominio pueden administrar un RODC.|  
|DenyPasswordReplicationAccountName <cadena []>|Especifica los nombres de las cuentas de usuario, de grupo y de equipo cuyas contraseñas no se van a replicar en este RODC. Use una cadena vacía "" si no quiere denegar la replicación de credenciales de ningún usuario ni equipo. De forma predeterminada, se deniegan los Administradores, Operadores de servidores, Operadores de copia de seguridad, Operadores de cuentas y el Grupo de replicación de contraseña RODC denegada. De forma predeterminada, el Grupo de replicación de contraseña RODC denegada incluye Publicadores de certificados, Admins. del dominio, Administradores de empresas, Enterprise Domain Controllers, Enterprise Domain Controllers de solo lectura, Propietarios del creador de directivas de grupo, la cuenta krbtgt y los Administradores de esquema.<br /><br />Proporcione los valores como una matriz de cadenas. Por ejemplo:<br /><br />Código:<br /><br />-DenyPasswordReplicationAccountName "RegionalAdmins","AdminPCs"|  
|DnsDelegationCredential <PS Credential> **Nota:** Este argumento no se puede especificar cuando se ejecuta el cmdlet Add-ADDSReadOnlyDomainController.|Especifica el nombre de usuario y contraseña para crear la delegación DNS, según las reglas de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) y un objeto PSCredential.|  
|DomainMode <DomainMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<br /><br />O bien<br /><br />DomainMode <DomainMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}|Especifica el nivel funcional del dominio durante la creación de un dominio nuevo.<br /><br />El nivel funcional del dominio no puede ser inferior que el del bosque, pero puede ser superior.<br /><br />El valor predeterminado se calcula automáticamente y se establece en el nivel funcional del bosque existente, o el valor que se establece para **-ForestMode**.|  
|**DomainName**<br /><br />Obligatorio para los cmdlets Install-ADDSForest e Install-ADDSDomainController.|Especifica el FQDN del dominio en el que se quiere instalar un controlador de dominio adicional.|  
|**DomainNetbiosName <string>**<br /><br />Obligatorio para Install-ADDSForest si el nombre del prefijo FQDN tiene más de 15 caracteres.|Úselo con Install-ADDSForest. Asigna un nombre NetBIOS al nuevo dominio raíz del bosque.|  
|DomainType <DomainType> {ChildDomain &#124; TreeDomain} o {secundarios &#124; árbol}|Indica el tipo de dominio que se quiere crear: un nuevo árbol de dominios en un bosque existente, un elemento secundario en un dominio existente, o un bosque nuevo.<br /><br />El valor predeterminado para DomainType es ChildDomain.|  
|Force|Cuando se especifica este parámetro, todas las advertencias que normalmente aparecerían durante la instalación y adición del controlador de dominio se suprimirán para permitir que el cmdlet complete su ejecución. Puede ser útil incluir este parámetro cuando se automatiza la instalación.|  
|ForestMode <ForestMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<br /><br />O bien<br /><br />ForestMode <ForestMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}|Especifica el nivel funcional del bosque cuando se crea un nuevo bosque.<br /><br />El valor predeterminado es Win2012.|  
|InstallationMediaPath|Indica la ubicación de los medios de instalación que se usarán para instalar un nuevo controlador de dominio.|  
|InstallDns|Especifica si el servicio del servidor DNS debe instalarse y configurarse en el controlador de dominio.<br /><br />Para un bosque nuevo, el valor predeterminado es **$True** y el servidor DNS se instala.<br /><br />Para un dominio secundario nuevo o un árbol de dominios, si el dominio primario (o dominio raíz del bosque para un árbol de dominios) ya hospeda y almacena los nombres DNS para el dominio, el valor predeterminado para este parámetro será $True.<br /><br />Para la instalación de un controlador de dominio en un dominio existente, si no se especifica este parámetro y el dominio actual ya hospeda y almacena los nombres DNS para el dominio, el valor predeterminado para este parámetro será **$True**. De lo contrario, si los nombres de dominio DNS se hospedan fuera de Active Directory, el valor predeterminado es **$False** y no se instala ningún servidor DNS.|  
|LogPath <string>|Especifica la ruta de acceso no UNC completa del directorio en un disco fijo del equipo local que contiene los archivos de registro del dominio; por ejemplo, **C:\Windows\Logs**.<br /><br />El valor predeterminado es **%SYSTEMROOT%\NTDS**. **Importante:** No almacene los archivos de registro de Active Directory en un volumen de datos formateado con el Sistema de archivos resistente (ReFS).|  
|MoveInfrastructureOperationMasterRoleIfNecessary|Especifica si se debe transferir la infraestructura principal rol maestro de operaciones (operaciones de maestro únicas también conocido como flexible o FSMO) al controlador de dominio que va a crear "en el caso de está hospedado en un servidor de catálogo global" y no tiene previsto Asegúrese del controlador de dominio que va a crear un servidor de catálogo global. Especifique este parámetro para transferir el rol maestro de infraestructura al controlador de dominio que se está creando en caso de que sea necesaria la transferencia. En este caso, especifique la opción **NoGlobalCatalog** si quiere que el rol maestro de infraestructura permanezca donde se encuentra actualmente.|  
|**NewDomainName <string>**  **Nota:** Obligatoria solo para Install-ADDSDomain.|Especifica el nombre de dominio único para el nuevo dominio.<br /><br />Por ejemplo, si quiere crear un dominio secundario nuevo llamado **emea.corp.fabrikam.com**, debe especificar **emea** como valor para este argumento.|  
|**NewDomainNetbiosName <string>**<br /><br />Obligatorio para Install-ADDSDomain si el nombre del prefijo FQDN tiene más de 15 caracteres.|Úselo con Install-ADDSDomain. Asigna un nombre NetBIOS al nuevo dominio. El valor predeterminado se deriva del valor de **"NewDomainName**.|  
|NoDnsOnNetwork|Especifica que el servicio DNS no se encuentra disponible en la red. Este parámetro únicamente se usa cuando la configuración IP del adaptador de red de este equipo no está configurado con el nombre de un servidor DNS para la resolución de nombres. Indica que un servidor DNS se instalará en este equipo para la resolución de nombres. De lo contrario, la configuración IP del adaptador de la red debe configurarse primero con la dirección de un servidor DNS.<br /><br />La omisión de este parámetro (valor predeterminado) indica que la configuración de cliente TCP/IP del adaptador de red en este equipo servidor se usará para establecer contacto con un servidor DNS. Por lo tanto, si no especifica este parámetro, asegúrese de que la configuración de cliente TCP/IP se establezca con una dirección de servidor DNS preferida.|  
|NoGlobalCatalog|Especifica que no se quiere que el nuevo controlador de dominio sea un servidor de catálogo global.<br /><br />De forma predeterminada, los controladores de dominio que ejecutan Windows Server 2012 se instalan con el catálogo global. En otras palabras, esto se ejecuta automáticamente sin cálculos, salvo que se especifique:<br /><br />Código:<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|Especifica si se va a reiniciar el equipo tras completar el comando, sin tener en cuenta si se ha realizado correctamente. De forma predeterminada, el equipo se reiniciará. Para impedir que el servidor se reinicie, especifique:<br /><br />Código:<br /><br />-NoRebootOnCompletion:$True<br /><br />No existe ningún equivalente para esta opción en la interfaz de usuario (UI).|  
|**ParentDomainName <string>**  **Nota:** Obligatorio para el cmdlet Install-ADDSDomain|Especifica el FQDN de un dominio primario existente. Este argumento se usa cuando se instala un dominio secundario o un árbol de dominios nuevo.<br /><br />Por ejemplo, si quiere crear un dominio secundario nuevo llamado **emea.corp.fabrikam.com**, debe especificar **corp.fabrikam.com** como valor para este argumento.|  
|ReadOnlyReplica|Especifica si se va a instalar el controlador de dominio de solo lectura (RODC).|  
|ReplicationSourceDC <string>|Indica el FQDN del controlador de dominio asociado desde el cual replica la información de dominio. El valor predeterminado se calcula automáticamente.|  
|**SafeModeAdministratorPassword <securestring>**|Proporciona la contraseña de la cuenta de administrador cuando el equipo se inicia en el modo seguro o una variante de este, como el modo de restauración del servicio de directorio.<br /><br />El valor predeterminado es una contraseña vacía. Debe proporcionar una contraseña. La contraseña debe proporcionarse en un formato System.Security.SecureString, como la proporcionada por read-host -assecurestring o ConvertTo-SecureString.<br /><br />La operación del argumento SafeModeAdministratorPassword es especial: si no se especifica como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva. Si se especifica sin un valor y no hay otros argumentos especificados para el cmdlet, este le pide que escriba una contraseña enmascarada sin confirmación. Este no es el uso preferido cuando el cmdlet se ejecuta en forma interactiva. Si se especifica con un valor, el valor debe ser una cadena segura. Este no es el uso preferido cuando el cmdlet se ejecuta en forma interactiva. Por ejemplo, puede solicitar en forma manual una contraseña mediante el cmdlet Read-Host para pedirle al usuario una cadena segura: -safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring). También puede proporcionar una cadena segura como una variable no cifrada convertida, pero esto no se recomienda. -safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)|  
|**Nombre del sitio <string>**<br /><br />Obligatorio para el cmdlet Add-addsreadonlydomaincontrolleraccount|Especifica el sitio en el que se instalará el controlador de dominio. No hay ningún **"sitename** argumento al ejecutar **Install-ADDSForest** porque el primer sitio creado es Default-First-Site-Name.<br /><br />El nombre del sitio ya debe existir cuando se proporciona como un argumento para **-sitename**. El cmdlet no creará el sitio.|  
|SkipAutoConfigureDNS|Omite la configuración automática de la configuración de cliente DNS, reenviadores y sugerencias de raíz. Este argumento se activa únicamente si el servicio del servidor DNS ya está instalado o se instala automáticamente con **-InstallDNS**.|  
|SystemKey <string>|Especifica la clave del sistema para los medios desde los que replica los datos.<br /><br />El valor predeterminado es **none**.<br /><br />Los datos deben estar en el formato proporcionado por read-host -assecurestring o ConvertTo-SecureString.|  
|SysvolPath <string>|Especifica la ruta de acceso no UNC completa del directorio en un disco fijo del equipo local; por ejemplo, **C:\Windows\SYSVOL**.<br /><br />El valor predeterminado es **%SYSTEMROOT%\SYSVOL**. **Importante:** SYSVOL no puede almacenarse en un volumen de datos formateado con el Sistema de archivos resistente (ReFS).|  
|SkipPreChecks|No ejecuta las comprobaciones de requisitos previos antes de comenzar la instalación. No se recomienda usar esta configuración.|  
|Whatif|Muestra lo que sucedería si se ejecutara el cmdlet. El cmdlet no se ejecuta.|  
  
### <a name="BKMK_PSCreds"></a>Especificar las credenciales de Windows PowerShell  
Puede especificar las credenciales sin revelarlas en texto sin formato en pantalla mediante [Get-credential](https://technet.microsoft.com/library/dd315327.aspx).  
  
El funcionamiento para los argumentos -SafeModeAdministratorPassword y LocalAdministratorPassword es especial:  
  
-   Si no se especifica como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.  
  
-   Si se especifica con un valor, este debe ser una cadena segura. Este no es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.  
  
Por ejemplo, puede solicitar una contraseña en forma manual mediante el cmdlet **Read-Host** para pedirle al usuario una cadena segura  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> Como la opción anterior no confirma la contraseña, tenga mucho cuidado: la contraseña no es visible.  
  
También puede proporcionar una cadena segura como una cadena segura como una variable no cifrada convertida, pero esto no se recomienda:  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> No se recomienda proporcionar o almacenar una contraseña no cifrada. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de DSRM de ese controlador de dominio. Si saben esto, podrán suplantar el controlador de dominio y elevar su privilegio al nivel máximo en un bosque de Active Directory.  
  
### <a name="BKMK_TestCmdlets"></a>Uso de cmdlets de prueba  
Cada cmdlet ADDSDeployment tiene un cmdlet de prueba correspondiente. Los cmdlets de prueba ejecutan solo las comprobaciones de requisitos previos para la operación de instalación; no se configura la instalación. Los argumentos para cada cmdlet de prueba son los mismos que para el cmdlet de instalación correspondiente, pero **"SkipPreChecks** no está disponible para los cmdlets de prueba.  
  
|Cmdlet de prueba|Descripción|  
|---------------|---------------|  
|Test-ADDSForestInstallation|Ejecuta los requisitos previos para la instalación de un bosque nuevo de Active Directory.|  
|Test-ADDSDomainInstallation|Ejecuta los requisitos previos para la instalación de un dominio nuevo de Active Directory.|  
|Test-ADDSDomainControllerInstallation|Ejecuta los requisitos previos para la instalación de un controlador de dominio de Active Directory.|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|Ejecuta los requisitos previos para agregar una cuenta de controlador de dominio de solo lectura (RODC).|  
  
### <a name="BKMK_PSForest"></a>Instalación de un dominio raíz del bosque mediante Windows PowerShell  
La sintaxis de comando para la instalación de un busque nuevo es la siguiente. Los argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> Se requiere el argumento -DomainNetBIOSName si se quiere cambiar el nombre de 15 caracteres que se genera automáticamente según el prefijo de nombre DNS o si el nombre excede los 15 caracteres.  
  
Por ejemplo, para instalar un nuevo bosque llamado corp.contoso.com y que se pida en forma segura que se proporcione la contraseña de DSRM, escriba:  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> El servidor DNS se instala de forma predeterminada cuando se ejecuta Install-ADDSForest.  
  
Para instalar un bosque nuevo llamado corp.contoso.com, cree una delegación DNS en el dominio contoso.com, configure el nivel funcional del dominio en Windows Server 2008 R2 y establezca el nivel funcional del bosque en Windows Server 2008, instale la base de datos de Active Directory y SYSVOL en la unidad D:\, instale los archivos de registro en la unidad E:\, y ese le pedirá que proporcione la contraseña de modo de restauración de servicios de directorio y escriba:  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>Instalar un nuevo dominio secundario o árbol mediante Windows PowerShell  
La sintaxis de comando para la instalación de un dominio nuevo es la siguiente. Los argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> El argumento **-credential** solamente es necesario cuando no se encuentra con una sesión iniciada actualmente como miembro del grupo Administradores de empresas.  
>   
> Se requiere el argumento **-NewDomainNetBIOSName** si se quiere cambiar el nombre de 15 caracteres que se genera automáticamente según el prefijo de nombre DNS o si el nombre excede los 15 caracteres.  
  
Por ejemplo, para usar las credenciales de corp\EnterpriseAdmin1 para crear un nuevo dominio secundario llamado child.corp.contoso.com, instale el servidor DNS, cree una delegación DNS en el dominio corp.contoso.com, establezca el nivel funcional de dominio en Windows Server 2003, convierta el control de dominio en un servidor de catálogo global en un sitio llamado Houston, use DC1.corp.contoso.com como el controlador de dominio de origen de replicación, instale la base de datos de Active Directory y SYSVOL en la unidad D:\, instale los archivos de registro en la unidad E:\, y se le pedirá que proporcione la contraseña de modo de restauración de servicios de directorio, pero no se le pedirá que confirme el comando, escriba:  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>Instalar un controlador de dominio adicional (réplica) mediante Windows PowerShell  
La sintaxis de comando para la instalación de un controlador de dominio adicional es la siguiente. Los argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Para instalar un controlador de dominio y servidor DNS en el dominio corp.contoso.com y se le pida que proporcione las credenciales de Administrador de dominio y la contraseña DSRM, escriba:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
Si el equipo ya se ha unido a un dominio y usted es miembro del grupo Admins. del dominio, puede usar:  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
Para que se le pida el nombre del dominio, escriba:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
El siguiente comando usará las credenciales de Contoso\EnterpriseAdmin1 para instalar un controlador de dominio de escritura y un servidor de catálogo global en un sitio llamado Boston, instale el servidor DNS, cree una delegación DNS en el dominio contoso.com, instale desde los medios almacenados en la carpeta c:\ADDS IFM, instale la base de datos de Active Directory y SYSVOL en la unidad D:\, instale los archivos de registro en la unidad E:\, haga que el servidor se reinicie automáticamente después de que termine la instalación de AD DS, y se le pedirá que proporcione la contraseña de modo de restauración de servicios de directorio:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>Instalación por fases de un RODC con Windows PowerShell  
La sintaxis de comando para crear una cuenta RODC es la siguiente. Los argumentos opcionales aparecen entre corchetes.  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
La sintaxis de comando para adjuntar un servidor a una cuenta RODC es la siguiente. Los argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Por ejemplo, para crear una cuenta RODC denominada RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
A continuación, ejecute los siguientes comandos en el servidor que quiera adjuntar a la cuenta RODC1. El servidor no se puede unir al dominio. Instale primero el rol de servidor de AD DS y las herramientas de administración:  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
Para ejecutar el siguiente comando para crear el RODC:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
Presione **Y** para confirmar o incluir el **"Confirmar** argumento para evitar que el mensaje de confirmación.  
  
## <a name="BKMK_GUI"></a>Instalación de AD DS mediante el administrador del servidor  
AD DS se puede instalar en Windows Server 2012 usando el Asistente para agregar Roles de administrador del servidor, seguido por el Asistente de configuración de Active Directory Domain Services, que es nuevo a partir de Windows Server 2012. El Asistente de instalación de servicios de dominio de Active Directory (dcpromo.exe) está en desuso a partir de Windows Server 2012.  
  
En las siguientes secciones se explica cómo crear grupos de servidores para instalar y administrar AD DS en varios servidores, y cómo usar los asistentes para instalar AD DS.  
  
### <a name="BKMK_ServerPools"></a>Creación de grupos de servidores  
El Administrador del servidor puede agrupar otros servidores de la red siempre y cuando estén accesibles desde el equipo que ejecuta el Administrador del servidor. Una vez agrupados, elige aquellos servidores para la instalación remota de AD DS o cualquier otra opción de configuración posible dentro del Administrador del servidor. El equipo que ejecuta el Administrador del servidor se agrupa automáticamente. Para obtener más información acerca de los grupos de servidor, consulte [agregar servidores al administrador del servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
> [!NOTE]  
> Para administrar un equipo unidos a dominios con el Administrador del servidor en un servidor del grupo de trabajo, o viceversa, se necesitan pasos de configuración adicionales. Para obtener más información, vea "Agregar y administrar servidores en grupos de trabajo" en [agregar servidores al administrador del servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
### <a name="BKMK_installADDSGUI"></a>Instalación de AD DS  
**Credenciales administrativas**  
  
Los requisitos de credenciales para instalar AD DS varían según la configuración de implementación que se elija. Para obtener más información, consulte [Requisitos de credenciales para ejecutar Adprep.exe e instalar Active Directory Domain Services](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
Use los siguientes procedimientos para instalar AD DS con el método de la GUI. Los pasos se pueden realizar en forma remota o local. Para obtener una explicación más detallada de estos pasos, consulte los siguientes temas:  
  
-   [Implementación de un bosque con el administrador del servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [Instalar un nuevo secundarios de Active Directory de Windows Server 2012 o un dominio de árbol &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [Instalar un controlador de dominio de solo lectura de Active Directory Server 2012 Windows &#40;RODC&#41; &#40;nivel 200&#41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>Para instalar AD DS mediante el Administrador del servidor  
  
1.  En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características** para iniciar el Asistente para agregar roles.  
  
2.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
3.  En la página **Seleccionar tipo de instalación**, haga clic en **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.  
  
4.  En la página **Seleccionar servidor de destino**, haga clic en **Seleccionar un servidor del grupo de servidores**, en el nombre del servidor donde quiere instalar AD DS y, a continuación, en **Siguiente**.  
  
    Para seleccionar servidores remotos, primero cree un grupo de servidores y agréguele los servidores remotos. Para obtener más información acerca de cómo crear grupos de servidores, consulte [agregar servidores al administrador del servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
5.  En la página **Seleccionar roles de servidor**, haga clic en **Servicios de dominio de Active Directory**, después en el cuadro de diálogo **Asistente para agregar roles y características**, haga clic en **Agregar características** y, a continuación, en **Siguiente**.  
  
6.  En la página **Seleccionar características**, seleccione las características adicionales que quiera instalar y haga clic en **Siguiente**.  
  
7.  En la página **Servicios de dominio de Active Directory**, revise la información y haga clic en **Siguiente**.  
  
8.  En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
9. En la página **Resultados**, compruebe que la instalación se haya realizado correctamente y haga clic en **Promover este servidor a controlador de dominio** para iniciar el Asistente para configuración de Servicios de dominio de Active Directory.  
  
    ![Instalar AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > Si cierra el Asistente para agregar roles en este momento sin iniciar el Asistente para configuración de Servicios de dominio de Active Directory, podrá hacer clic en Tareas en el Administrador del servidor para reiniciarlo.  
  
    ![Instalar AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. En la página **Configuración de implementación**, elija una de las siguientes opciones:  
  
    -   Si va a instalar un controlador de dominio adicional en un dominio existente, haga clic en **agregar un controlador de dominio a un dominio existente**y escriba el nombre del dominio (por ejemplo, emea.corp.contoso.com) o haga clic en **seleccionar ...**  para elegir un dominio y las credenciales (por ejemplo, especificar una cuenta que sea miembro del grupo Administradores del dominio) y, a continuación, haga clic en **siguiente**.  
  
        > [!NOTE]  
        > El nombre del dominio y las credenciales de usuario actuales se proporcionan de forma predeterminada solo si la máquina está unida a un dominio y se está realizando una instalación local. Si está instalando AD DS en un servidor remoto, es necesario que especifique las credenciales, por diseño. Si las credenciales del usuario actual no son suficientes para realizar la instalación, haga clic en **cambios...**  con el fin de especificar unas credenciales distintas.  
  
        Para obtener más información, consulte [instalar un controlador de dominio de réplica de Windows Server 2012 en un dominio existente &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   Si está instalando un nuevo dominio secundario, haga clic en **Agregar un nuevo dominio a un bosque existente**, para **Seleccionar tipo de dominio**, seleccione **Dominio secundario**, escriba o busque el nombre del dominio DNS primario (por ejemplo, corp.contoso.com), escriba el nombre relativo del dominio secundario nuevo (por ejemplo, emea), escriba las credenciales que quiera usar para crear el dominio nuevo y, a continuación, haga clic en **Siguiente**.  
  
        Para obtener más información, consulte [instalar un nuevo Windows Server 2012 Active Directory secundario o un dominio de árbol &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si va a instalar un nuevo árbol de dominios, haga clic en **Agregar un nuevo dominio a un bosque existente**, en **Seleccionar tipo de dominio** elija **Dominio de árbol**, escriba el nombre del dominio raíz (por ejemplo, corp.contoso.com), escriba el nombre de DNS del dominio nuevo (por ejemplo, fabrikam.com), escriba las credenciales que quiera usar para crear el dominio nuevo y, a continuación, haga clic en **Siguiente**.  
  
        Para obtener más información, consulte [instalar un nuevo Windows Server 2012 Active Directory secundario o un dominio de árbol &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si está instalando un nuevo bosque, haga clic en **Agregar un nuevo bosque** y, a continuación, escriba el nombre del dominio raíz (por ejemplo, corp.contoso.com).  
  
        Para obtener más información, consulte [instalar un nuevo Windows Server 2012 bosque de Active Directory &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. En la página **Opciones del controlador de dominio**, elija una de las siguientes opciones:  
  
    -   Si está creando un nuevo bosque o dominio, seleccione los niveles funcionales del dominio y del bosque, haga clic en **Servidor de Sistema de nombres de dominio (DNS)**, especifique la contraseña DSRM y, a continuación, haga clic en **Siguiente**.  
  
    -   Si está agregando un controlador de dominio a un dominio existente, haga clic en **Servidor de Sistema de nombres de dominio (DNS)**, **Catálogo global (GC)** o **Controlador de dominio de solo lectura (RODC)** según sea necesario, elija el nombre del sitio, escriba la contraseña DSRM y, a continuación, haga clic en **Siguiente**.  
  
    Para obtener más información acerca de qué opciones de esta página están o no están disponibles en distintas condiciones, vea [Opciones del controlador de dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage).  
  
12. En la página **Opciones de DNS** (que solo aparece si instala un servidor DNS), haga clic en **Actualizar delegación DNS** según sea necesario. Si es necesario, proporcione las credenciales que tengan permiso para crear registros de delegación DNS en la zona DNS primaria.  
  
    Si no se puede entrar en contacto con un servidor DNS que hospeda la zona primaria, la opción **Actualizar delegación DNS** no estará disponible.  
  
    Para obtener más información sobre si es necesario actualizar la delegación DNS, consulte [descripción de delegación de zona](https://technet.microsoft.com/library/cc771640.aspx). Si intenta actualizar la delegación DNS y se produce un error, consulte [Opciones de DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
13. En la página **Opciones de RODC** (que solo aparece si instala RODC), especifique el nombre de un grupo o usuario a cargo de la administración de RODC, agregue o quite cuentas a los grupos de replicación de contraseña permitida o denegada y, a continuación, haga clic en **Siguiente**.  
  
    Para obtener más información, consulte [directiva de replicación de contraseñas](https://technet.microsoft.com/library/cc730883(v=ws.10)).  
  
14. En la página **Opciones adicionales**, elija una de las siguientes opciones:  
  
    -   Si va a crear un dominio nuevo, escriba un nuevo nombre NetBIOS o compruebe el nombre NetBIOS predeterminado y, a continuación, haga clic en **Siguiente**.  
  
    -   Si está agregando un controlador de dominio a un dominio existente, seleccione el controlador de dominio del que quiere replicar los datos de instalación de AD DS (o permitir que el asistente seleccione cualquier controlador de dominio). Si está efectuando la instalación desde medios, haga clic en **Instalar desde ruta de acceso a medios**, escriba y compruebe la ruta de acceso a los archivos de origen de la instalación y, a continuación, haga clic en **Siguiente**.  
  
        No se puede usar Instalar desde medios (IFM) para instalar el primer controlador de dominio en un dominio. IFM no funciona en diferentes versiones de sistemas operativos. En otras palabras, para instalar un controlador de dominio adicional que ejecuta Windows Server 2012 mediante el uso de IFM, debe crear los medios de copia de seguridad en un controlador de dominio de Windows Server 2012. Para obtener más información acerca de IFM, vea [instalar un controlador de dominio adicional mediante IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx).  
  
15. En la página **Rutas de acceso**, escriba las ubicaciones para la carpeta SYSVOL, archivos de registro o la base de datos de Active Directory (o acepte las ubicaciones predeterminadas) y, a continuación, haga clic en **Siguiente**.  
  
    > [!IMPORTANT]  
    > No almacene SYSVOL, archivos de registro o la base de datos de Active Directory en un volumen de datos formateado con el Sistema de archivos resistente (ReFS).  
  
16. En la página **Opciones de preparación**, escriba las credenciales que sean suficientes para ejecutar adprep. Para obtener más información, consulte [Requisitos de credenciales para ejecutar Adprep.exe e instalar Active Directory Domain Services](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
17. En la página **Opciones de revisión**, confirme las selecciones, haga clic en **Ver script** si desea exportar la configuración a un script de Windows PowerShell y, a continuación, haga clic en **Siguiente**.  
  
18. En la página **Comprobación de requisitos previos**, confirme que se haya completado la validación de los requisitos previos y, a continuación, haga clic en **Instalar**.  
  
19. En la página **Resultados**, verifique que el servidor se haya configurado correctamente como controlador de dominio. El servidor se reiniciará automáticamente para completar la instalación de AD DS.  
  
## <a name="BKMK_UIStaged"></a>Instalación de RODC por fases mediante la interfaz gráfica de usuario  
Una instalación por fases de un RODC permite la creación de un RODC en dos fases. En la primera fase, un miembro del grupo Admins. del dominio crea una cuenta RODC. En la segunda fase, se adjunta un servidor a la cuenta RODC. Un miembro del grupo Admins. del dominio, un grupo o un usuario del dominio delegado puede completar la segunda fase.  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>Para crear una cuenta RODC mediante las herramientas de administración de Active Directory  
  
1.  Puede crear la cuenta RODC mediante el Centro de administración de Active Directory o Usuarios y equipos de Active Directory.  
  
    1.  Haga clic en **Inicio**, después en **Herramientas administrativas** y, a continuación, en **Centro de administración de Active Directory**.  
  
    2.  En el panel de navegación (panel izquierdo), haga clic en el nombre del dominio.  
  
    3.  En la lista de administración (panel central), haga clic en la unidad organizativa **Domain Controllers**.  
  
    4.  En el panel de tareas (panel derecho), haga clic en **Crear previamente una cuenta de controlador de dominio de solo lectura**.  
  
    O bien:  
  
    1.  Haga clic en **Inicio**, luego en **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**.  
  
    2.  Haga clic con el botón secundario en la unidad organizativa (OU) **Domain Controllers** o haga clic en la OU **Domain Controllers** y, a continuación, en **Acción**.  
  
    3.  Haga clic en **Crear previamente una cuenta de controlador de dominio de solo lectura**.  
  
2.  En la página **Asistente para la instalación de los Servicios de dominio de Active Directory**, si desea modificar la Directiva de replicación de contraseñas (PRP) predeterminada, seleccione **Usar la instalación en modo avanzado** y, a continuación, haga clic en **Siguiente**.  
  
3.  En la página **Credenciales de red**, en **Especifique las credenciales de cuenta que se usarán para la instalación**, haga clic en **Mis credenciales de inicio de sesión actuales** o en **Credenciales alternativas** y, a continuación, haga clic en **Establecer**. En el cuadro de diálogo **Seguridad de Windows**, escriba el nombre de usuario y la contraseña para una cuenta que pueda instalar el controlador de dominio adicional. Para instalar un controlador de dominio adicional, debe ser miembro del grupo Administradores de empresas o del grupo Administradores de dominio. Cuando termine de proporcionar las credenciales, haga clic en **Siguiente**.  
  
4.  En la página **Especifique el nombre del equipo**, escriba el nombre del equipo del servidor que será el RODC.  
  
5.  En la página **Seleccione un sitio**, seleccione un sitio de la lista o seleccione la opción para instalar el controlador de dominio en el sitio que corresponda a la dirección IP del equipo en el cual ejecuta el asistente; a continuación, haga clic en **Siguiente**.  
  
6.  En la página **Opciones adicionales del controlador de dominio**, realice las siguientes selecciones y, a continuación, haga clic en **Siguiente**:  
  
    -   **Servidor DNS**: esta opción está activada de forma predeterminada para que el controlador de dominio pueda funcionar como un servidor de Sistema de nombres de dominio (DNS) Si no desea que el controlador de dominio sea un servidor DNS, desactive esta opción. Sin embargo, si no instala el rol del servidor DNS en el RODC y este es el único controlador de dominio de la sucursal, los usuarios de esta sucursal no podrán realizar la resolución de nombres cuando la red de área extensa (WAN) conectada al sitio del concentrador esté sin conexión.  
  
    -   **Catálogo global**: Esta opción está seleccionada de forma predeterminada. Agregue el catálogo global y las particiones del directorio de solo lectura al controlador de dominio, y activa la funcionalidad de búsqueda del catálogo global. Si no desea que el nuevo controlador de dominio sea un servidor de catálogo global, desactive esta opción. Sin embargo, si no instala un servidor de catálogo global en la sucursal o si no habilita el almacenamiento en caché de la pertenencia al grupo universal para el sitio que incluye el RODC, los usuarios de la sucursal no podrán iniciar sesión en el dominio cuando la WAN conectada al sitio del controlador esté sin conexión.  
  
    -   **Controlador de dominio de solo lectura**: al crear una cuenta RODC, esta opción se selecciona de forma predeterminada y no se puede desactivar.  
  
7.  Si seleccionó la casilla **Usar la instalación en modo avanzado** en la página de **Bienvenida**, aparece la página **Especificar la directiva de replicación de contraseñas**. No se replican contraseñas de cuentas en el RODC de manera predeterminada y se impide explícitamente la replicación de las contraseñas de las cuentas que son críticas para la seguridad (como las que son miembros del grupo Admins. del dominio) en el RODC.  
  
    Para agregar cuentas a la directiva, haga clic en **Agregar**, después haga clic en **Permitir la replicación en este RODC de contraseñas de la cuenta** o en **Denegar la replicación en este RODC de contraseñas de la cuenta** y, a continuación, seleccione las cuentas.  
  
    Cuando haya terminado (o para aceptar la configuración predeterminada), haga clic en **Siguiente**.  
  
8.  En la página **Delegación de instalación y administración de RODC**, escriba el nombre del usuario o del grupo que asociará el servidor a la cuenta RODC que cree. Puede escribir el nombre de solo una entidad de seguridad.  
  
    Para buscar el directorio de un usuario o grupo específico, haga clic en **Establecer**. En **Seleccionar usuario o grupo**, escriba el nombre del usuario o grupo. Recomendamos que delegue la instalación y administración del RODC en un grupo.  
  
    Este usuario o grupo también tendrá derechos administrativos locales en el RODC una vez completada la instalación. Si no se especifica un usuario o un grupo, únicamente podrán asociar el servidor a la cuenta los miembros del grupo Admins. del dominio o Administradores de empresas.  
  
    Cuando haya terminado, haga clic en **Siguiente**.  
  
9. En la página **Resumen**, revise las selecciones realizadas. Haga clic en **Atrás** para cambiar cualquier selección, si fuera necesario.  
  
    Para guardar la configuración que ha seleccionado en un archivo de respuesta que puede usar para automatizar las operaciones posteriores de AD DS, haga clic en **Exportar configuración**. Escriba un nombre para el archivo de respuesta y, a continuación, haga clic en **Guardar**.  
  
    Cuando esté seguro de que sus selecciones son precisas, haga clic en **Siguiente** para crear la cuenta RODC.  
  
10. En la página **Finalización del Asistente para la instalación de Active Directory Domain Services**, haga clic en **Finalizar**.  
  
Después de crear una cuenta RODC, se puede adjuntar un servidor a la cuenta para completar la instalación del RODC. Esta segunda fase se puede completar en la sucursal donde se ubicará el RODC. El servidor en el que se realiza este procedimiento no podrá estar unido al dominio. A partir de Windows Server 2012, use el Asistente para agregar Roles de administrador del servidor para asociar un servidor a una cuenta de RODC.  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>Para asociar un servidor a una cuenta RODC mediante el Administrador del servidor  
  
1.  Inicie sesión como Administrador local.  
  
2.  En el Administrador del servidor, haga clic en **Agregar roles y características**.  
  
3.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  
  
4.  En la página **Seleccionar tipo de instalación**, haga clic en **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.  
  
5.  En la página **Seleccionar servidor de destino**, haga clic en **Seleccionar un servidor del grupo de servidores**, en el nombre del servidor donde quiere instalar AD DS y, a continuación, en **Siguiente**.  
  
6.  En la página **Seleccionar roles de servidor**, haga clic en **Servicios de dominio de Active Directory**, después en **Agregar características** y, a continuación, en **Siguiente**.  
  
7.  En la página **Seleccionar características**, seleccione las características adicionales que quiera instalar y haga clic en **Siguiente**.  
  
8.  En la página **Servicios de dominio de Active Directory**, revise la información y haga clic en **Siguiente**.  
  
9. En la página **Confirmar selecciones de instalación** , haga clic en **Instalar**.  
  
10. En la página **Resultados**, compruebe **Instalación correcta** y haga clic en **Promover este servidor a controlador de dominio** para iniciar el Asistente para configuración de Active Directory Domain Services.  
  
    > [!IMPORTANT]  
    > Si cierra el Asistente para agregar roles en este momento sin iniciar el Asistente para configuración de Servicios de dominio de Active Directory, podrá hacer clic en Tareas en el Administrador del servidor para reiniciarlo.  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. En la página **Configuración de implementación**, haga clic en **Agregar un controlador de dominio a un dominio existente**, escriba el nombre del dominio (por ejemplo, emea.contoso.com) y las credenciales (por ejemplo, especifique una cuenta que se delegue para administrar e instalar el RODC) y, a continuación, haga clic en **Siguiente**.  
  
12. En la página **Opciones del controlador de dominio**, haga clic en **Usar cuenta RODC existente**, escriba y confirme la contraseña del modo de restauración de servicios de directorio y, a continuación, haga clic en **Siguiente**.  
  
13. En la página **Opciones adicionales**, si está instalando desde medios, haga clic en **Instalar desde ruta de acceso a medios**, escriba y verifique la ruta de acceso a los archivos de origen de la instalación, seleccione el controlador de dominio desde el que quiera replicar los datos de instalación de AD DS (o permita que el asistente seleccione cualquier controlador de dominio) y, a continuación, haga clic en **Siguiente**.  
  
14. En la página **Rutas de acceso**, escriba las ubicaciones para la carpeta SYSVOL, archivos de registro o la base de datos de Active Directory (o acepte las ubicaciones predeterminadas) y, a continuación, haga clic en **Siguiente**.  
  
15. En la página **Opciones de revisión**, confirme las selecciones, haga clic en **Ver script** si desea exportar la configuración a un script de Windows PowerShell y, a continuación, haga clic en **Siguiente**.  
  
16. En la página **Comprobación de requisitos previos**, confirme que se haya completado la validación de los requisitos previos y, a continuación, haga clic en **Instalar**.  
  
    Para completar la instalación de AD DS, el servidor se reiniciará automáticamente.  
  
## <a name="see-also"></a>Vea también  
[Solución de problemas de implementación del controlador de dominio](Troubleshooting-Domain-Controller-Deployment.md)  
[Instalar un nuevo bosque de Active Directory de Windows Server 2012 &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[Instalar un nuevo secundarios de Active Directory de Windows Server 2012 o un dominio de árbol &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente &#40;nivel 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



