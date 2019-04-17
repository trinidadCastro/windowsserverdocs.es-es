---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: Instalar servicios de dominio de Active Directory (nivel 100)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f76aa1e5200a9fc2f47a559c4a318aa619d31557
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-active-directory-domain-services-level-100"></a>Instalar servicios de dominio de Active Directory (nivel 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica cómo instalar AD DS en Windows Server 2012 con cualquiera de los siguientes métodos:  
  
-   [Requisitos de las credenciales para ejecutar Adprep.exe e instalar los servicios de dominio de Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [Instalar AD DS mediante Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [Instalar AD DS con el administrador del servidor](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [Realizar una instalación de RODC provisionalmente mediante la interfaz gráfica de usuario](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>Requisitos de las credenciales para ejecutar Adprep.exe e instalar los servicios de dominio de Active Directory  
Las siguientes credenciales son necesarias para ejecutar Adprep.exe e instalar AD DS.  
  
-   Para instalar un nuevo bosque, debe iniciar sesión como administrador local para el equipo.  
  
-   Para instalar un nuevo dominio secundario o un nuevo árbol de dominios, debes iniciar sesión como miembro del grupo Administradores de empresa.  
  
-   Para instalar un controlador de dominio en un dominio existente, debe ser miembro del grupo Domain Admins.  
  
    > [!NOTE]  
    > Si no se ejecuta el comando de adprep.exe por separado y vas a instalar el primer controlador de dominio que se ejecuta Windows Server 2012 en un dominio existente o bosque, se te pedirá que proporcionar credenciales para ejecutar comandos Adprep. Los requisitos de credenciales son los siguientes:  
    >   
    > -   Para presentar el primer controlador de dominio de Windows Server 2012 en el bosque, deberás proporcionar credenciales para un miembro del grupo de administradores de empresa, el grupo de administradores de esquema, y grupo de administradores de dominio del dominio que hospeda el maestro de esquema.  
    > -   Para presentar el primer controlador de dominio de Windows Server 2012 en un dominio, deberás proporcionar credenciales para un miembro del grupo Administradores de dominio.  
    > -   Para presentar el primer controlador de dominio de solo lectura (RODC) en el bosque, deberás proporcionar credenciales para un miembro del grupo Administradores de empresa.  
    >   
    >     > [!NOTE]  
    >     > Si ya se ha ejecutado adprep /rodcprep en Windows Server 2008 o Windows Server 2008 R2, no es necesario volver a ejecutar para Windows Server 2012.  
  
## <a name="BKMK_PS"></a>Instalar AD DS mediante Windows PowerShell  
A partir de Windows Server 2012, puedes instalar AD DS mediante Windows PowerShell. Dcpromo.exe está en desuso a partir de Windows Server 2012, pero puede ejecutar dcpromo.exe mediante un archivo de respuesta (dcpromo//unattend:<answerfile> o dcpromo//answer:<answerfile>). La capacidad para continuar ejecutando dcpromo.exe con un archivo de respuesta proporciona a las organizaciones que tienen recursos invertidos en la hora de automatización existente para convertir la automatización de dcpromo.exe en Windows PowerShell. Para obtener más información acerca de cómo ejecutar dcpromo.exe con un archivo de respuesta, consulta [https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034).  
  
Para obtener más información sobre cómo quitar AD DS mediante Windows PowerShell, consulta [quitar AD DS mediante Windows PowerShell ](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS).  
  
Empieza con la adición del rol mediante Windows PowerShell. Este comando instala el rol de servidor de AD DS e instala las AD DS y AD LDS herramientas de administración, como herramientas basadas en la interfaz gráfica de usuario, como equipos y usuarios de Active Directory y herramientas de línea de comandos como dcdia.exe. Herramientas de administración de servidor no se instalan de forma predeterminada al usar Windows PowerShell. Debes especificar **"IncludeManagementTools** para administrar el servidor local o instalar [Remote Server Administration Tools](https://www.microsoft.com/download/details.aspx?id=28972) para administrar un servidor remoto.  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
No hay sin necesidad de reiniciar hasta una vez completada la instalación de AD DS.  
  
A continuación, puede ejecutar este comando para ver los cmdlets disponibles en el módulo ADDSDeployment.  
  
```  
Get-Command -Module ADDSDeployment
```  
  
Para ver la lista de argumentos que se pueden especificar para cmdlets y la sintaxis:  
  
```  
Get-Help <cmdlet name>  
```  
  
Por ejemplo, para ver los argumentos para crear una cuenta de dominio de solo lectura libre controlador (RODC), escribe lo siguiente:  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
Argumentos opcionales aparecen entre corchetes.  
  
También puedes descargar las últimas ejemplos de ayuda y conceptos relacionados con los cmdlets de Windows PowerShell. Para obtener más información, consulta [about_Updatable_Help ](https://technet.microsoft.com/library/hh847735.aspx).  
  
Se pueden ejecutar los cmdlets de Windows PowerShell en servidores remotos:  
  
-   En Windows PowerShell, usa Invoke-Command con el cmdlet ADDSDeployment. Por ejemplo, para instalar AD DS en un servidor remoto denominado ConDC3 en el dominio contoso.com, escribe:  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
- o bien-  
  
-   En el administrador del servidor, crea un grupo de servidores que incluya el servidor remoto. Haz clic en el nombre del servidor remoto y haz clic en **Windows PowerShell **.  
  
Las siguientes secciones explican cómo ejecutar ADDSDeployment module cmdlets para instalar AD DS.  
  
-   [ADDSDeployment cmdlet argumentos](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [Especifica las credenciales de Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [Cmdlets de prueba](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [Instalar un nuevo dominio raíz mediante Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [Instalar un nuevo dominio secundario o árbol mediante Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [Instalar un controlador de dominio (réplica) adicional mediante Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>ADDSDeployment cmdlet argumentos  
La siguiente tabla enumera los argumentos de los cmdlets de ADDSDeployment en Windows PowerShell. Se requieren argumentos en negrita. Si se denominan diferentes en Windows PowerShell, se enumeran los argumentos equivalentes para dcpromo.exe entre paréntesis.  
  
Windows PowerShell aceptan argumentos $TRUE o $FALSE. No es necesario especificar argumentos que son $TRUE de manera predeterminada.  
  
Para reemplazar los valores predeterminados, puedes especificar el argumento con un valor de $False. Por ejemplo, porque **- installdns** se ejecuta automáticamente para la instalación de un bosque nuevo si no se especifica, la única forma *evitar* instalación de DNS, cuando instalas un bosque nuevo es usar:  
  
```  
-InstallDNS:$false  
```  
  
Del mismo modo, porque **"installdns** tiene un valor predeterminado de $False si instalas un controlador de dominio en un entorno que no hospeda el servidor DNS de Windows server, debes especificar el siguiente argumento para instalar el servidor DNS:  
  
```  
-InstallDNS:$true  
```  
  
|Argumento|Descripción|  
|------------|---------------|  
|**ADPrepCredential <PS Credential>****Nota:** requiere si vas a instalar el primer controlador de dominio de Windows Server 2012 en un dominio o bosque y las credenciales del usuario actual no son suficientes para realizar la operación.|Especifica la cuenta con la suscripción de grupo de administradores de empresa y administradores de esquema que puedes preparar el bosque, según las reglas de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) y un objeto PSCredential.<br /><br />Si se especifica ningún valor, el valor de la **"credenciales** se usa el argumento.|  
|AllowDomainControllerReinstall|Especifica si se debe seguir instalar este controlador de dominio grabable, a pesar de que se ha detectado otra cuenta de controlador de dominio de escritura con el mismo nombre.<br /><br />Usa **$True** solo si estás seguro de que la cuenta no se utiliza actualmente por otro controlador de dominio de escritura.<br /><br />El valor predeterminado es **$False**.<br /><br />Este argumento no es válido para un RODC.|  
|AllowDomainReinstall|Especifica si se vuelve a crear un dominio existente.<br /><br />El valor predeterminado es **$False**.|  
|AllowPasswordReplicationAccountName < string [] >|Especifica los nombres de las cuentas de usuario, las cuentas de grupo y cuentas de equipo cuyas contraseñas pueden replicarse en este RODC. Usar una cadena vacía "" Si quieres mantener el valor vacío. De manera predeterminada, se permite solo permite RODC contraseña grupo de replicación y que se creó vacío.<br /><br />Proporcionar valores como una matriz de cadenas. Por ejemplo:<br /><br />Código - AllowPasswordReplicationAccountName "JVallejo", "JSmithPC", "Rama usuarios"|  
|ApplicationPartitionsToReplicate < string [] > **Nota:** no hay ninguna opción equivalente en la interfaz de usuario. Si instalas mediante la interfaz de usuario o IFM, se replicarán todas las particiones de la aplicación.|Especifica las particiones para replicar. Este argumento se aplica solo cuando especifiques el **- InstallationMediaPath** argumento para instalar desde medios (IFM). De manera predeterminada, todas las aplicaciones se replicarán particiones en función de sus propios ámbitos.<br /><br />Proporcionar valores como una matriz de cadenas. Por ejemplo:<br /><br />Código:<br /><br />-ApplicationPartitionsToReplicate "Partición1", "partición 2", "partition3"|  
|Confirmar|Solicita confirmación antes de ejecutar el cmdlet.|  
|CreateDnsDelegation **Nota:** no se puede especificar este argumento cuando se ejecuta el cmdlet Add-ADDSReadOnlyDomainController.|Indica si se debe crear una delegación de DNS que hace referencia el nuevo servidor DNS que se está instalando junto con el controlador de dominio. Válido para "integradas de Active Directory solo. Registros de delegación pueden crearse únicamente en los servidores DNS de Microsoft en línea y accesibles. Registros de delegación no se pueden crear para dominios que pertenecen inmediatamente para dominios de nivel superior como .com, .gov, .biz, .edu o dominios de código de país de dos letras como .nz y .au.<br /><br />El valor predeterminado se calcula automáticamente según el entorno.|  
|**Credenciales <PS Credential>****Nota:** solo se necesita si las credenciales del usuario actual no son suficientes para realizar la operación.|Especifica la cuenta de dominio que se puede iniciar sesión en el dominio, según las reglas de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) y un objeto PSCredential.<br /><br />Si se especifica ningún valor, se usan las credenciales del usuario actual.|  
|CriticalReplicationOnly|Especifica si la operación de instalación de AD DS realiza la replicación solo crítica antes de reiniciar el sistema y, luego, continúe. La replicación no crítica ocurre después de que finalice la instalación y se reinicie el equipo.<br /><br />Sin embargo, no se recomienda usar este argumento.<br /><br />Existe un equivalente para esta opción en la interfaz de usuario (UI).|  
|Ruta <string>|Especifica completo, no "ruta de acceso de convención de nomenclatura Universal (UNC) en un directorio en un disco duro del equipo local que contiene la base de datos de dominio, por ejemplo, **C:\Windows\NTDS.**<br /><br />El valor predeterminado es **%SYSTEMROOT%\NTDS**. **Importante:** mientras se pueden almacenar los archivos de base de datos y registro de AD DS en volumen formateado con el sistema de archivos resistente (ReFS), no hay ningún ventajas específicas para hospedar AD DS en ReFS, que no sean los beneficios normales de resistencia obtendrás para hospedar los datos de referencias.|  
|DelegatedAdministratorAccountName <string>|Especifica el nombre del usuario o grupo que se puede instalar y administrar el RODC.<br /><br />De manera predeterminada, solo los miembros del grupo Administradores de dominio pueden administrar un RODC.|  
|DenyPasswordReplicationAccountName < string [] >|Especifica los nombres de las cuentas de usuario, las cuentas de grupo y cuentas de equipo cuyas contraseñas no son replicarse en este RODC. Usar una cadena vacía "" Si no desea denegar la replicación de credenciales de los usuarios o equipos. De manera predeterminada, se deniegan los administradores, operadores de servidor, operadores de copia de seguridad, operadores de cuentas y el grupo de replicación denegado RODC contraseña. De manera predeterminada, el grupo de replicación denegado RODC contraseña incluye publicadores, administradores de dominio, administradores de empresa, Enterprise Domain Controllers, empresa Read-Only controladores de dominio, propietarios del creador de directivas de grupo, la cuenta krbtgt y administradores de esquema.<br /><br />Proporcionar valores como una matriz de cadenas. Por ejemplo:<br /><br />Código:<br /><br />-DenyPasswordReplicationAccountName "RegionalAdmins", "AdminPCs"|  
|DnsDelegationCredential <PS Credential>**Nota:** no se puede especificar este argumento cuando se ejecuta el cmdlet Add-ADDSReadOnlyDomainController.|Especifica el nombre de usuario y contraseña para crear la delegación de DNS, según las reglas de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) y un objeto PSCredential.|  
|DomainMode <DomainMode> {Windows 2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />O<br /><br />DomainMode <DomainMode> {2 & #124; 3 & #124; 4 & #124; 5 & #124; 6}|Especifica el nivel funcional del dominio durante la creación de un dominio.<br /><br />El nivel funcional del dominio no puede ser menor que el nivel funcional del bosque, pero puede ser mayor.<br /><br />El valor predeterminado es calculado automáticamente y se establece en el nivel funcional del bosque existente o el valor que se establece para **- ForestMode**.|  
|**DomainName**<br /><br />Es necesaria para cmdlets Install-ADDSForest e Install-ADDSDomainController.|Especifica el FQDN del dominio en el que desea instalar un controlador de dominio.|  
|**Valor de DomainNetbiosName <string>**<br /><br />Necesario para Install-ADDSForest si el nombre del prefijo FQDN tiene más de 15 caracteres.|Uso con Install-ADDSForest. Asigna un nombre NetBIOS al dominio raíz del bosque de nuevo.|  
|DomainType <DomainType> {ChildDomain & #124; TreeDomain} o {secundarios & #124; árbol}|Indica el tipo de dominio que quieres crear: bosque de un árbol de dominios en una existente, un elemento secundario de un dominio existente, o en un bosque de nuevo.<br /><br />El valor predeterminado de DomainType es ChildDomain.|  
|Fuerza|Cuando se especifica este parámetro se suprimirán las advertencias que normalmente aparecería durante la instalación y la incorporación del controlador de dominio para permitir que el cmdlet completar su ejecución. Este parámetro puede ser útil para incluir al scripting de instalación.|  
|ForestMode <ForestMode> {Windows 2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />O<br /><br />ForestMode <ForestMode> {2 & #124; 3 & #124; 4 & #124; 5 & #124; 6}|Especifica el nivel funcional del bosque cuando creas un bosque nuevo.<br /><br />El valor predeterminado es Win2012.|  
|InstallationMediaPath|Indica la ubicación de los medios de instalación que se usará para instalar un nuevo controlador de dominio.|  
|InstallDns|Especifica si el servicio de servidor DNS debe estar instalado y configurado en el controlador de dominio.<br /><br />Para un bosque nuevo, el valor predeterminado es **$True** y se instala el servidor DNS.<br /><br />Para un nuevo dominio secundario o árbol de dominio, si el dominio principal (o dominio raíz del bosque de un árbol de dominio) ya hospeda y almacena los nombres DNS para el dominio, el valor predeterminado de este parámetro es $True.<br /><br />Para una instalación del controlador de dominio en un dominio existente, si este parámetro es no se especifica izquierda y el dominio actual ya hospeda y almacena los nombres DNS para el dominio, el valor predeterminado de este parámetro es **$True**. De lo contrario, si están hospedados los nombres de dominio DNS fuera de Active Directory, el valor predeterminado es **$False** y no está instalado ningún servidor DNS.|  
|LogPath <string>|Especifica la ruta de acceso completo, no UNC a un directorio en un disco duro del equipo local que contiene los archivos de registro de dominio, por ejemplo, **C:\Windows\Logs**.<br /><br />El valor predeterminado es **%SYSTEMROOT%\NTDS**. **Importante:** no almacenar los archivos de registro de Active Directory en un volumen de datos con formato con el sistema de archivos resistente (ReFS).|  
|MoveInfrastructureOperationMasterRoleIfNecessary|Especifica si se debe transferir la infraestructura principal función (operaciones de maestro único también conocido como flexibles o FSMO) al controlador de dominio que estás creando "en el caso actualmente está hospedado en un servidor de catálogo global" y no tienes previsto hacer que el controlador de dominio que vas a crear un servidor de catálogo global. Especifica este parámetro para transferir el rol de maestro de infraestructura al controlador de dominio que vas a crear en caso de que se necesita la transferencia; en este caso, especificar el **NoGlobalCatalog** opción si desea que el rol de maestro de infraestructura a permanecer donde está actualmente.|  
|**NewDomainName <string>****Nota:** sólo es necesario para Install-ADDSDomain.|Especifica el nombre de dominio único para el nuevo dominio.<br /><br />Por ejemplo, si quieres crear un nuevo dominio secundario denominado **emea.corp.fabrikam.com**, debes especificar **emea** como el valor de este argumento.|  
|**NewDomainNetbiosName <string>**<br /><br />Necesario para Install-ADDSDomain si el nombre del prefijo FQDN tiene más de 15 caracteres.|Uso con Install-ADDSDomain. Asigna un nombre NetBIOS al nuevo dominio. El valor predeterminado se deriva del valor del **"NewDomainName**.|  
|NoDnsOnNetwork|Especifica que el servicio DNS no está disponible en la red. Este parámetro se usa solo cuando la configuración IP del adaptador de red para este equipo no está configurada con el nombre de un servidor DNS para resolver el nombre. Indica que se instalará en este equipo para la resolución de nombre de un servidor DNS. De lo contrario, la configuración IP del adaptador de red debe configurarse primero con la dirección del servidor DNS.<br /><br />Si se omite este parámetro (predeterminado) indica que se usará la configuración de cliente de TCP/IP del adaptador de red en este equipo servidor ponerse en contacto con un servidor DNS. Por lo tanto, si no se especifica este parámetro, asegúrate de que la configuración de cliente de TCP/IP primero está configurados con la dirección del servidor DNS preferido.|  
|NoGlobalCatalog|Especifica que no desea que el controlador de dominio como un servidor de catálogo global.<br /><br />Controladores de dominio que ejecutan Windows Server 2012 se instalan con el catálogo global de forma predeterminada. En otras palabras, este se ejecuta automáticamente sin cálculos, a menos que especifiques:<br /><br />Código:<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|Especifica si se reinicia el equipo tras finalizar el comando, independientemente del éxito. De manera predeterminada, el equipo se reiniciará. Para evitar que reiniciar el servidor, especifica:<br /><br />Código:<br /><br />-NoRebootOnCompletion: $True<br /><br />Existe un equivalente para esta opción en la interfaz de usuario (UI).|  
|**ParentDomainName <string>****Nota:** necesarios para Install-ADDSDomain cmdlet|Especifica el FQDN de un dominio principal existente. Usa este argumento cuando instalas un dominio secundario o un nuevo árbol de dominio.<br /><br />Por ejemplo, si quieres crear un nuevo dominio secundario denominado **emea.corp.fabrikam.com**, debes especificar **corp.fabrikam.com** como el valor de este argumento.|  
|ReadOnlyReplica|Especifica si se debe instalar un controlador de dominio de solo lectura (RODC).|  
|ReplicationSourceDC <string>|Indica el nombre completo del controlador de dominio asociado desde el que duplicar la información del dominio. El valor predeterminado se calcula automáticamente.|  
|**SafeModeAdministratorPassword <securestring>**|Proporciona la contraseña de la cuenta de administrador cuando se inicia el equipo en modo seguro o una variante de modo seguro, como el modo de restauración de servicios de directorio.<br /><br />El valor predeterminado es una contraseña en blanco. Debe proporcionar una contraseña. La contraseña debe proporcionarse en un formato System.Security.SecureString, como el que proporciona el host de lectura - assecurestring o ConvertTo-SecureString.<br /><br />Operación del argumento SafeModeAdministratorPassword es especial: si que no se especifica como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet interactively.If sin un valor y no hay ningún otro argumento especificada el cmdlet, el cmdlet te pedirá que escribas una contraseña enmascarada sin pedir confirmación. No es el uso preferido cuando se ejecuta el cmdlet interactively.If con un valor, el valor debe ser una cadena segura. No es el uso preferido cuando se ejecuta el cmdlet interactively.For, manualmente pedir una contraseña mediante el cmdlet de Read-Host para pedir al usuario una cadena segura:-safemodeadministratorpassword (host de lectura: símbolo del sistema "contraseña:" - assecurestring) también puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda. -safemodeadministratorpassword (convertto-securestring "Existente1" - asplaintext-forzar)|  
|**Nombre de sitio <string>**<br /><br />Necesario para el cmdlet Add-addsreadonlydomaincontrolleraccount|Especifica el sitio donde se instalará el controlador de dominio. No hay ningún **"sitio** argumento cuando ejecutas **ADDSForest de instalación** porque el primer sitio creado es Default-First-Site-Name.<br /><br />El nombre del sitio debe existir cuando se proporciona como un argumento a **- el nombre de sitio**. El cmdlet no creará el sitio.|  
|SkipAutoConfigureDNS|Omite la configuración automática de la configuración del cliente DNS, servidores de reenvío y sugerencias de raíz. Este argumento está en vigor solo si el servicio de servidor DNS está instalado o instala automáticamente con **- InstallDNS**.|  
|SystemKey <string>|Especifica la clave del sistema para los elementos multimedia desde el cual replica los datos.<br /><br />El valor predeterminado es **ninguno**.<br /><br />Datos deben estar en formato de host de lectura - assecurestring o ConvertTo-SecureString.|  
|SysvolPath <string>|Especifica la ruta de acceso completo, no UNC a un directorio en un disco duro del equipo local, por ejemplo, **C:\Windows\SYSVOL**.<br /><br />El valor predeterminado es **%SYSTEMROOT%\SYSVOL**. **Importante:** SYSVOL no se pueden almacenar en un volumen de datos con formato con el sistema de archivos resistente (ReFS).|  
|SkipPreChecks|No se ejecuta las comprobaciones de requisitos previos antes de iniciar la instalación. No es aconsejable usar esta opción de configuración.|  
|WhatIf|Muestra lo que sucedería si se ejecuta el cmdlet. No se ejecuta el cmdlet.|  
  
### <a name="BKMK_PSCreds"></a>Especifica las credenciales de Windows PowerShell  
Puedes especificar sin mostrar texto sin formato en pantalla mediante el uso de credenciales [Get-credential](https://technet.microsoft.com/library/dd315327.aspx).  
  
La operación de los argumentos de LocalAdministratorPassword y - SafeModeAdministratorPassword es especial:  
  
-   Si no se especifica como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
-   Si se especifica con un valor, el valor debe ser una cadena segura. No es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
Por ejemplo, puedes manualmente pedir contraseña utilizando la **Read-Host** cmdlet para pedir al usuario una cadena segura  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> Como la opción anterior confirma la contraseña, Ten mucho cuidado: la contraseña no está visible.  
  
También puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda:  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar una contraseña de texto sin cifrar. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña DSRM de ese controlador de dominio. Con estos datos, pueden suplantar el propio controlador de dominio y elevar sus privilegios para el nivel más alto de un bosque de Active Directory.  
  
### <a name="BKMK_TestCmdlets"></a>Cmdlets de prueba  
Cada cmdlet ADDSDeployment tiene correspondiente probar cmdlet. Cmdlets se ejecuta la prueba solo las comprobaciones de requisitos previos para la operación de instalación. no se configura ninguna configuración de instalación. Los argumentos de cada cmdlet de prueba son los mismos que el cmdlet de instalación correspondiente, pero **"SkipPreChecks** no está disponible para los cmdlets de prueba.  
  
|Cmdlet de prueba|Descripción|  
|---------------|---------------|  
|Test-ADDSForestInstallation|Ejecuta los requisitos previos para instalar un nuevo bosque de Active Directory.|  
|Test-ADDSDomainInstallation|Ejecuta los requisitos previos para instalar un nuevo dominio de Active Directory.|  
|Test-ADDSDomainControllerInstallation|Ejecuta los requisitos previos para instalar un controlador de dominio de Active Directory.|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|Ejecuta los requisitos previos para agregar un controlador de dominio de solo lectura cuenta (RODC).|  
  
### <a name="BKMK_PSForest"></a>Instalar un nuevo dominio raíz mediante Windows PowerShell  
La sintaxis de comandos para la instalación de un bosque nuevo es la siguiente. Argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> Si quieres cambiar el nombre de 15 caracteres que se genera automáticamente según el prefijo de nombre de dominio DNS o si el nombre es superior a 15 caracteres, se requiere el argumento de valor de DomainNetBIOSName.  
  
Por ejemplo, para instalar un nuevo bosque denominado corp.contoso.com y pedirá de forma segura para proporcionar la contraseña DSRM, escribe:  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> Servidor DNS está instalado de manera predeterminada cuando se ejecuta Install-ADDSForest.  
  
Para instalar un nuevo bosque denominado corp.contoso.com, crear una delegación DNS del dominio contoso.com, Establece el nivel funcional del dominio en Windows Server 2008 R2 y establece el nivel funcional del bosque en Windows Server 2008, instalar la base de datos de Active Directory y SYSVOL en la unidad D:\, instalar los archivos de registro en la unidad E:\ y ser se le pide la contraseña de modo de restauración de servicios de directorio y tipo :  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>Instalar un nuevo dominio secundario o árbol mediante Windows PowerShell  
La sintaxis de comando para instalar un nuevo dominio es la siguiente. Argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> La **-credenciales** argumento sólo es necesario cuando no actualmente sesión como miembro del grupo Administradores de empresa.  
>   
> La **- NewDomainNetBIOSName** argumento es obligatorio si quieres cambiar el nombre de 15 caracteres generado automáticamente según el prefijo de nombre de dominio DNS o si el nombre es superior a 15 caracteres.  
  
Por ejemplo, para usar las credenciales de corp\EnterpriseAdmin1 para crear un nuevo dominio secundario denominado child.corp.contoso.com, instalar el servidor DNS, crear una delegación DNS del dominio corp.contoso.com, Establece el nivel funcional del dominio en Windows Server 2003, convertir un servidor catálogo global en el controlador de dominio en un sitio denominado Houston, usar DC1.corp.contoso.com como el controlador de dominio de origen de replicación, instalar la base de datos de Active Directory y SYSVOL en la unidad D:\, instalar los archivos de registro en la unidad E:\ y se te pida que proporcione la contraseña de modo de restauración de servicios de directorio, pero no se te pide que confirmes el comando, escribe:  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>Instalar un controlador de dominio (réplica) adicional mediante Windows PowerShell  
La sintaxis de comando para instalar un controlador de dominio es la siguiente. Argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Para instalar un controlador de dominio y el servidor DNS del dominio corp.contoso.com y se te pida que proporcione las credenciales de administrador de dominio y la contraseña de DSRM, escribe:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
Si el equipo está unido a un dominio y eres miembro del grupo Administradores de dominio, puedes usar:  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
Para que se le solicite el nombre de dominio, escribe:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
El siguiente comando usará las credenciales de Contoso\EnterpriseAdmin1 para instalar un controlador de dominio grabable y un servidor de catálogo global en un sitio denominado Boston, instalar el servidor DNS, crear una delegación DNS del dominio contoso.com, instalar desde el medio que se almacena en la carpeta IFM c:\ADDS instalar la base de datos de Active Directory y SYSVOL en la unidad D:\, instalar los archivos de registro en la unidad E:\, se el servidor reiniciará automáticamente cuando se complete la instalación de AD DS y se te pida que proporcione la contraseña de modo de restauración de servicios de directorio:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>Realizar una instalación de RODC por fases mediante Windows PowerShell  
La sintaxis de comandos para crear una cuenta de RODC es la siguiente. Argumentos opcionales aparecen entre corchetes.  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
La sintaxis del comando para asociar un servidor a una cuenta de RODC es la siguiente. Argumentos opcionales aparecen entre corchetes.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Por ejemplo, para crear una cuenta de RODC denominado RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
A continuación, ejecuta los siguientes comandos en el servidor que quieras asociar a la cuenta RODC1. El servidor no puede estar unido al dominio. En primer lugar, instala las herramientas de administración y el rol de servidor de AD DS:  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
La ejecución el siguiente comando para crear el RODC:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
Presiona **Y** deben confirmar o incluir la **"Confirmar** argumento para evitar el aviso de confirmación.  
  
## <a name="BKMK_GUI"></a>Instalar AD DS con el administrador del servidor  
AD DS puede instalarse en Windows Server 2012 mediante el Asistente para agregar Roles en Server Manager, seguido por el Asistente de configuración de Active Directory dominio servicios, que es nueva en Windows Server 2012. El Asistente para la instalación a servicios de Active Directory dominio de (dcpromo.exe) está en desuso a partir de Windows Server 2012.  
  
Las siguientes secciones explican cómo crear grupos de servidores para poder instalar y administrar AD DS en varios servidores y cómo usar a los asistentes para instalar AD DS.  
  
### <a name="BKMK_ServerPools"></a>Creación de grupos de servidor  
El administrador del servidor permite agrupar otros servidores en la red siempre que sean accesibles desde el equipo que ejecuta el administrador del servidor. Una vez agrupado, elegir los servidores para la instalación remota de AD DS u otras opciones de configuración posibles en el administrador del servidor. El equipo que ejecuta el administrador del servidor automáticamente agrupa sí. Para obtener más información acerca de los grupos de servidor, consulta [agregar servidores al administrador del servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
> [!NOTE]  
> Para administrar un equipo unido al dominio mediante el administrador del servidor en un servidor de grupo de trabajo, o viceversa, son necesarios los pasos de configuración adicionales. Para obtener más información, consulta "Agregar y administrar los servidores en grupos de trabajo" en [agregar servidores al administrador del servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
### <a name="BKMK_installADDSGUI"></a>Instalar AD DS  
**Credenciales administrativas**  
  
Los requisitos de credenciales para instalar AD DS varían según la configuración de implementación que elijas. Para obtener más información, consulta [requisitos para ejecutar Adprep.exe e instalar los servicios de dominio de Active Directory de credenciales](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
Usa los siguientes procedimientos para instalar AD DS con el método de la interfaz gráfica de usuario. Los pasos pueden realizarse de forma local o remota. Para obtener información más detallada de estos pasos, consulta los siguientes temas:  
  
-   [Implementación de un bosque con el administrador del servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [Instalar un nuevo secundario de Active Directory de Windows Server 2012 o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [Instalar Windows Server 2012 Active Directory Read-Only controlador de dominio & #40; RODC & #41; & #40; Nivel 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>Para instalar AD DS mediante el administrador del servidor  
  
1.  En el administrador del servidor, haz clic en **administrar** y haz clic en **agregar Roles y características** para iniciar el Asistente para agregar Roles.  
  
2.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
3.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basada en rol o característica** y, a continuación, haz clic en **siguiente**.  
  
4.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, haz clic en el nombre del servidor donde desea instalar AD DS y, a continuación, haz clic en **siguiente**.  
  
    Para seleccionar servidores remotos, primero crea un grupo de servidores y agregar los servidores remotos. Para obtener más información sobre la creación de grupos de servidores, consulta [agregar servidores al administrador del servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
5.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de dominio de Active Directory**, a continuación, en la **agregar Roles and Features Wizard** cuadro de diálogo, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
6.  En la **Select features**, seleccione las características adicionales que quieras instalar y haz clic en **siguiente**.  
  
7.  En la **los servicios de dominio de Active Directory** página, revisa la información y, a continuación, haz clic en **siguiente**.  
  
8.  En la **Confirmar selecciones de instalación** página, haz clic en **instalar**.  
  
9. En la **resultados** página, comprueba que la instalación se realizó correctamente y haz clic en **promover este servidor a un controlador de dominio** para iniciar el Asistente para la configuración de los servicios de dominio de Active Directory.  
  
    ![Instalar AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > Si cierras el Asistente para agregar Roles en este punto sin iniciar al Asistente para la configuración de los servicios de dominio de Active Directory, puede reiniciar haciendo clic en las tareas en el administrador del servidor.  
  
    ![Instalar AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. En la **configuración de implementación** página, elige una de las siguientes opciones:  
  
    -   Si vas a instalar un controlador de dominio en un dominio existente, haz clic en **agregar un controlador de dominio a un dominio existente**y escribe el nombre del dominio (por ejemplo, emea.corp.contoso.com) o haz clic en **selecciona... **para elegir un dominio y las credenciales (por ejemplo, especificar una cuenta que sea miembro del grupo Administradores del dominio) y, a continuación, haz clic en **siguiente**.  
  
        > [!NOTE]  
        > El nombre del dominio y las credenciales de usuario actual se proporcionan de manera predeterminada, solo si el equipo está unido al dominio y va a realizar una instalación local. Si vas a instalar AD DS en un servidor remoto, debes especificar las credenciales, por diseño. Si las credenciales del usuario actual no son suficientes para realizar la instalación, haz clic en **cambio... **para especificar credenciales diferentes.  
  
        Para obtener más información, consulta [instalar un controlador de dominio de réplica de Windows Server 2012 en un dominio existente & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   Si vas a instalar un nuevo dominio secundario, haz clic en **agregar un dominio nuevo a un bosque existente**, para **selecciona el tipo de dominio**, selecciona **dominio secundario**, escribe o busca el nombre del nombre DNS de dominio principal (por ejemplo, corp.contoso.com), escribe el nombre relativo del nuevo dominio secundario (por ejemplo emea), las credenciales de tipo usar para crear el nuevo dominio y, a continuación, haz clic en **siguiente**.  
  
        Para obtener más información, consulta [instalar un Windows Server 2012 Active Directory un menor nueva o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si vas a instalar un nuevo árbol de dominio, haz clic en **Agregar nuevo dominio a un bosque existente**, para **selecciona el tipo de dominio**, elige **dominio árbol**, escribe el nombre del dominio raíz (por ejemplo, corp.contoso.com), escriba el nombre DNS del dominio (por ejemplo, fabrikam.com) nuevas credenciales de tipo que se usan para crear el nuevo dominio y, a continuación, haz clic en **siguiente**.  
  
        Para obtener más información, consulta [instalar un Windows Server 2012 Active Directory un menor nueva o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Si vas a instalar un bosque nuevo, haz clic en **agregar un bosque nuevo** y, a continuación, escribe el nombre del dominio raíz (por ejemplo, corp.contoso.com).  
  
        Para obtener más información, consulta [instalar un nuevo Windows Server 2012 bosque de Active Directory & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. En la **opciones del controlador de dominio** página, elige una de las siguientes opciones:  
  
    -   Si vas a crear un dominio o bosque nuevo, selecciona los niveles funcionales de bosque y dominio, haz clic en **servidor de sistema de nombres de dominio (DNS)**, especificar la contraseña DSRM y, a continuación, haz clic en **siguiente**.  
  
    -   Si vas a agregar un controlador de dominio a un dominio existente, haz clic en **servidor de sistema de nombres de dominio (DNS)**, **Global catálogo**, o **lectura solo controlador de dominio (RODC)** según sea necesario, elige el nombre del sitio y escribe la contraseña DSRM y, a continuación, haz clic en **siguiente**.  
  
    Para obtener más información sobre qué opciones de esta página están disponibles o no está disponible en diferentes condiciones, consulta [opciones del controlador de dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage).  
  
12. En la **opciones DNS** página (que solo aparece si instalas un servidor DNS), haz clic en **delegación DNS de actualización** según sea necesario. Si lo haces, proporcionar las credenciales que tienen permiso para crear los registros de delegación DNS en la zona DNS principal.  
  
    Si no se puede conectar con un servidor DNS que hospeda la zona principal, la **actualización DNS delegación** opción no está disponible.  
  
    Para obtener más información acerca de si es necesario actualizar la delegación de DNS, consulta [acerca de la delegación zona](https://technet.microsoft.com/library/cc771640.aspx). Si intentas actualizar la delegación DNS y se producirá un error, consulta [opciones DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
13. En la **RODC opciones** página (que solo aparece si instalas un RODC), especifica el nombre de un grupo o usuario que administrará el RODC, agregar o quitar cuentas de los grupos de replicación de contraseña permitidas o denegadas y, a continuación, haz clic en cuentas de **siguiente**.  
  
    Para obtener más información, consulta [directiva de replicación de contraseñas](https://technet.microsoft.com/library/cc730883(v=ws.10)).  
  
14. En la **opciones adicionales** página, elige una de las siguientes opciones:  
  
    -   Si vas a crear un nuevo dominio, escribe un nuevo nombre NetBIOS o comprueba el nombre NetBIOS predeterminado del dominio y, a continuación, haz clic en **siguiente**.  
  
    -   Si vas a agregar un controlador de dominio a un dominio existente, selecciona el controlador de dominio que quieras duplicar los datos de instalación de AD DS de (o permitir que el Asistente seleccionar cualquier controlador de dominio). Si vas a instalar desde medios, haz clic en **instalar desde la ruta del archivo multimedia** escribe y comprueba la ruta de acceso a los archivos de origen de instalación y, a continuación, haz clic en **siguiente**.  
  
        No puedes usar instalar desde medios (IFM) para instalar el primer controlador de dominio en un dominio. IFM no funciona en las versiones de sistema operativo diferente. En otras palabras, para instalar un controlador de dominio que se ejecuta Windows Server 2012 con IFM, debes crear el medio de copia de seguridad en un controlador de dominio de Windows Server 2012. Para obtener más información sobre IFM, consulta [instalar un controlador de dominio adicionales por uso IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx).  
  
15. En la **rutas de acceso** página, escribe las ubicaciones de la base de datos de Active Directory, archivos de registro y carpeta SYSVOL (o Aceptar las ubicaciones predeterminadas) y haz clic en **siguiente**.  
  
    > [!IMPORTANT]  
    > No almacenar la base de datos de Active Directory, archivos de registro o carpeta SYSVOL en un volumen de datos con formato con el sistema de archivos resistente (ReFS).  
  
16. En la **preparación opciones** página, las credenciales de tipo que son suficientes para ejecutar adprep. Para obtener más información, consulta [requisitos para ejecutar Adprep.exe e instalar los servicios de dominio de Active Directory de credenciales](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
17. En la **opciones de revisión** página, confirma las opciones seleccionadas, haz clic en **Ver script** si quieres exportar la configuración a un script de PowerShell de Windows y, a continuación, haz clic en **siguiente**.  
  
18. En la **comprobación de requisitos previos** página, confirma que la validación requisitos previa completada y, a continuación, haz clic en **instalar**.  
  
19. En la **resultados**, compruebe que el servidor se ha configurado correctamente como un controlador de dominio. El servidor se reiniciará automáticamente para completar la instalación de AD DS.  
  
## <a name="BKMK_UIStaged"></a>Realizar una instalación de RODC provisionalmente mediante la interfaz gráfica de usuario  
Una instalación de RODC por fases permite crear un RODC en dos fases. En la primera fase, un miembro del grupo Administradores de dominio crea una cuenta de RODC. En la segunda fase, un servidor está conectado a la cuenta de RODC. La segunda fase se puede llevar a cabo por un miembro del grupo Administradores de dominio o un usuario de dominio delegado o el grupo.  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>Para crear una cuenta de RODC mediante las herramientas de administración de Active Directory  
  
1.  Puedes crear la cuenta de RODC con centro de administración de Active Directory o usuarios de Active Directory y equipos.  
  
    1.  Haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **centro de administración de Active Directory**.  
  
    2.  En el panel de navegación (panel izquierdo), haz clic en el nombre del dominio.  
  
    3.  En la lista de administración (panel central), haz clic en el **controladores de dominio** unidad organizativa.  
  
    4.  En el panel de tareas (panel derecho), haz clic en **previamente crear una cuenta de controlador de dominio de solo lectura**.  
  
    - O bien-  
  
    1.  Haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
  
    2.  Ya sea con el botón derecho el **controladores de dominio** unidad organizativa (OU) o haz clic en el **controladores de dominio** unidad organizativa y, a continuación, haz clic en **acción**.  
  
    3.  Haz clic en **crea previamente Read-only cuenta de controlador de dominio**.  
  
2.  En la **Bienvenido al Asistente para la instalación de los servicios de dominio de Active Directory** página si quieres modificar el valor predeterminado de la directiva de replicación de contraseñas (PRP), selecciona **usa la instalación en modo avanzado**y, a continuación, haz clic en **siguiente**.  
  
3.  En la **credenciales de red** página, debajo **especifica las credenciales de cuenta para usar para realizar la instalación**, haz clic en **mi... actual credenciales sesión** o haz clic en **credenciales adicionales**y, a continuación, haz clic en **establecer**. En el **Windows Security** diálogo cuadro, proporciona el nombre de usuario y contraseña de una cuenta que puede instalar el controlador de dominio adicional. Para instalar un controlador de dominio, debe ser miembro del grupo Administradores de empresa o el grupo de administradores de dominio. Cuando hayas terminado de proporcionar credenciales, haz clic en **siguiente**.  
  
4.  En la **especificar el nombre del equipo**, escriba el nombre del equipo del servidor que será el RODC.  
  
5.  En la **seleccione un sitio** página, selecciona un sitio de la lista o selecciona la opción para instalar el controlador de dominio del sitio que corresponde a la dirección IP del equipo en el que se ejecuta el asistente y, a continuación, haz clic en **siguiente**.  
  
6.  En la **opciones adicionales del controlador de dominio** página, realice las siguientes selecciones y, a continuación, haz clic en **siguiente**:  
  
    -   **Servidor DNS**: esta opción está seleccionada de manera predeterminada para que el controlador de dominio puede funcionar como un servidor de sistema de nombres de dominio (DNS). Si no desea que el controlador de dominio que un servidor DNS, desactiva esta opción. Sin embargo, si no instala el rol de servidor DNS en el RODC y éste es el único controlador de dominio en la sucursal, los usuarios de la sucursal no podrá realizar la resolución de nombres cuando la red de área extensa (WAN) para el sitio del concentrador está sin conexión.  
  
    -   **Catálogo global**: esta opción está seleccionada de manera predeterminada. Agrega el catálogo global, particiones de directorio de sólo lectura al controlador de dominio, y habilita la funcionalidad de búsqueda de catálogo global. Si no desea que el controlador de dominio como un servidor de catálogo global, desactiva esta opción. Sin embargo, si no instalar a un servidor de catálogo global en las sucursales o permitir el almacenamiento en caché de la pertenencia a grupos universales para el sitio que incluya el RODC, los usuarios en la sucursal no podrán iniciar sesión en el dominio cuando esté sin conexión WAN al sitio concentrador.  
  
    -   **Controlador de dominio de solo lectura**. Cuando creas una cuenta de RODC, esta opción está seleccionada de manera predeterminada y no pueden desactivarla.  
  
7.  Si has seleccionado la **usa la instalación en modo avanzado** casilla de verificación de la **bienvenida** página, la **especificar la directiva de replicación de contraseña** aparecerá la página. De manera predeterminada, se replica ninguna contraseña de cuenta en el RODC y cuentas de seguridad (por ejemplo, los miembros del grupo Administradores del dominio) se ha denegado explícitamente desde las contraseñas que se replica en el RODC.  
  
    Para agregar otras cuentas a la directiva, haz clic en **agregar**, a continuación, haz clic en **permitir que las contraseñas de la cuenta replicar en este RODC** o haz clic en **denegará las contraseñas de la cuenta de replicación a este RODC** y, a continuación, selecciona las cuentas.  
  
    Cuando se completen (o, para aceptar el valor predeterminado), haz clic en **siguiente**.  
  
8.  En la **delegación de instalación y RODC administración**, escriba el nombre del usuario o el grupo que se conectará el servidor a la cuenta RODC que vas a crear. Puede escribir el nombre de entidad de solo seguridad.  
  
    Para buscar el directorio de un usuario o grupo específico, haga clic en **establecer**. En **Seleccionar usuarios o grupos**, escribe el nombre del usuario o grupo. Te recomendamos que delegado RODC instalación y administración a un grupo.  
  
    Este usuario o grupo también tendrá derechos administrativos locales en el RODC después de la instalación. Si no especificas un usuario o grupo, solo los miembros del grupo Administradores de dominio o el grupo de administradores podrán asociar el servidor a la cuenta.  
  
    Cuando hayas terminado, haz clic en **siguiente**.  
  
9. En la **resumen** página, revisa las selecciones. Haz clic en **Atrás** cambiar las selecciones, si es necesario.  
  
    Para guardar la configuración que seleccionaste en un archivo de respuesta que puedes usar para automatizar operaciones posteriores de AD DS, haz clic en **exportar la configuración de**. Escribe un nombre para el archivo de respuesta y, a continuación, haz clic en **guardar**.  
  
    Cuando está seguro de que sean precisas tus selecciones, haz clic en **siguiente** para crear la cuenta RODC.  
  
10. En la **completar el Asistente para la instalación de los servicios de dominio de Active Directory** página, haz clic en **finalizar**.  
  
Después de un creada, puedes adjuntar un servidor de cuenta para completar la instalación de RODC. Esta segunda fase puede llevar a cabo en las sucursales donde se ubicarán RODC. El servidor que realizar este procedimiento no debe estar unido al dominio. A partir de Windows Server 2012, usan al Asistente para agregar Roles en el administrador del servidor para asociar un servidor a una cuenta de RODC.  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>Para asociar un servidor a una cuenta RODC mediante el administrador del servidor  
  
1.  Inicia sesión como administrador local.  
  
2.  En el administrador del servidor, haz clic en **agregar roles y características**.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basada en rol o característica** y, a continuación, haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, haz clic en el nombre del servidor donde desea instalar AD DS y, a continuación, haz clic en **siguiente**.  
  
6.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de dominio de Active Directory**, haz clic en **agregar características** y, a continuación, haz clic en **siguiente**.  
  
7.  En la **Select features**, seleccione las características adicionales que desee instalar y haga clic en **siguiente**.  
  
8.  En la **los servicios de dominio de Active Directory** página, revisa la información y, a continuación, haz clic en **siguiente**.  
  
9. En la **Confirmar selecciones de instalación** página, haz clic en **instalar**.  
  
10. En la **resultados**, compruebe **instalación se realizó correctamente**y haz clic en **promover este servidor a un controlador de dominio** para iniciar el Asistente para la configuración de los servicios de dominio de Active Directory.  
  
    > [!IMPORTANT]  
    > Si cierras el Asistente para agregar Roles en este punto sin iniciar al Asistente para la configuración de los servicios de dominio de Active Directory, puede reiniciar haciendo clic en las tareas en el administrador del servidor.  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. En la **configuración de implementación** página, haz clic en **agregar un controlador de dominio a un dominio existente**, escribe el nombre del dominio (por ejemplo, emea.contoso.com) y las credenciales (por ejemplo, especifican una cuenta delegada para administrar e instalar el RODC) y, a continuación, haz clic en **siguiente**.  
  
12. En la **opciones del controlador de dominio** página, haz clic en **usar cuenta existente de RODC**, escribe y confirma la contraseña de modo de restauración de servicios de directorio y, a continuación, haz clic en **siguiente**.  
  
13. En la **opciones adicionales** página, si vas a instalar desde un medio, haz clic en **instalar desde la ruta del archivo multimedia** escribe y comprueba la ruta de acceso a los archivos de origen de instalación, selecciona el controlador de dominio que quieras duplicar los datos de instalación de AD DS de (o permitir que el Asistente seleccionar cualquier controlador de dominio) y, a continuación, haz clic en **siguiente**.  
  
14. En la **rutas de acceso** página, escribe las ubicaciones de la base de datos de Active Directory, archivos de registro y la carpeta SYSVOL, o Aceptar las ubicaciones predeterminadas y, a continuación, haz clic en **siguiente**.  
  
15. En la **opciones de revisión** página, confirma las opciones seleccionadas, haz clic en **ver Script** exporta la configuración a un script de PowerShell de Windows y, a continuación, haz clic en **siguiente**.  
  
16. En la **comprobación de requisitos previos** página, confirma que la validación requisitos previa completada y, a continuación, haz clic en **instalar**.  
  
    Para completar la instalación de AD DS, el servidor se reiniciará automáticamente.  
  
## <a name="see-also"></a>Consulta también  
[Solución de problemas de implementación del controlador de dominio](Troubleshooting-Domain-Controller-Deployment.md)  
[Instalar un nuevo bosque de Active Directory de Windows Server 2012 & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[Instalar un nuevo secundario de Active Directory de Windows Server 2012 o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[Instalar un controlador de dominio de réplica Windows Server 2012 en un dominio existente & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



