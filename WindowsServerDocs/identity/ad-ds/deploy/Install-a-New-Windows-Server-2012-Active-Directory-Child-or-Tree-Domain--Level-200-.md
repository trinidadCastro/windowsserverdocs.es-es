---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: Instalar un nuevo dominio secundario o de árbol de Active Directory de Windows Server 2012 (nivel 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7a3ea43b513535d6b7d5a815039869b71c7d7c92
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965787"
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Instalar un nuevo dominio secundario o de árbol de Active Directory de Windows Server 2012 (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica cómo agregar dominios secundarios y de árbol a un bosque de Windows Server 2012 existente mediante el Administrador del servidor o Windows PowerShell.  
  
-   [Flujo de trabajo de dominios secundarios y de árbol](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Windows PowerShell de dominios secundarios y de árbol](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implementación](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="child-and-tree-domain-workflow"></a><a name="BKMK_Workflow"></a>Flujo de trabajo de dominios secundarios y de árbol  
En el diagrama siguiente se muestra el proceso de configuración de Servicios de dominio de Active Directory cuando se ha instalado anteriormente el rol de AD DS y se ha iniciado el Asistente para configuración de Servicios de dominio de Active Directory con el Administrador del servidor para crear un nuevo dominio en un bosque existente.  
  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="child-and-tree-domain-windows-powershell"></a><a name="BKMK_PS"></a>Windows PowerShell de dominios secundarios y de árbol  
  
|||  
|-|-|  
|**Cmdlet ADDSDeployment**|Argumentos (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS).|  
|**Install-AddsDomain**|-SkipPreChecks<p>***-NewDomainName***<p>***-ParentDomainName***<p>***-SafeModeAdministratorPassword***<p>*-ADPrepCredential*<p>-AllowDomainReinstall<p>-Confirm<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-NoDNSOnNetwork<p>*-DomainMode*<p>***-DomainType***<p>-Force<p>*-InstallDNS*<p>*-LogPath*<p>*-NewDomainNetBIOSName*<p>*-NoGlobalCatalog*<p>-NoNorebootoncompletion<p>*-ReplicationSourceDC*<p>*-SiteName*<p>-SkipAutoConfigureDNS<p>*-SYSVOLPath*<p>*-Whatif*|  
  
> [!NOTE]  
> El argumento **-credential** solo es necesario si no has iniciado sesión como miembro del grupo Administradores de empresas. El argumento **-NewDomainNetBIOSName** solo es necesario para cambiar el nombre de 15 caracteres que se genera automáticamente según el prefijo del nombre de dominio DNS, o bien si el nombre supera los 15 caracteres.  
  
## <a name="deployment"></a><a name="BKMK_Deployment"></a>Planta  
  
### <a name="deployment-configuration"></a>Configuración de la implementación  
En la siguiente captura de pantalla verás las opciones para agregar un dominio secundario:  
  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
En la siguiente captura de pantalla verás las opciones para agregar un dominio de árbol:  
  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
El Administrador del servidor comienza la promoción de cada controlador de dominio con la página **Configuración de implementación**. Las opciones restantes y los campos requeridos cambian en esta página y en las páginas siguientes, según qué operación de implementación se seleccione.  
  
En este tema se combinan dos operaciones discretas: la promoción de dominios secundarios y la promoción de dominios de árbol. La única diferencia entre las dos operaciones es el tipo de dominio que crees. El resto de pasos son idénticos en las dos operaciones.  
  
-   Para crear un nuevo dominio secundario, haz clic en **Agregar un nuevo dominio a un bosque existente** y selecciona **Dominio secundario**. Para un **nombre de dominio primario**, escribe o selecciona el nombre del dominio primario. A continuación, escribe el nombre del nuevo dominio en el cuadro **Nuevo nombre de dominio**. Especifica un nombre de dominio secundario de etiqueta única y válido; el nombre debe cumplir los requisitos de nombre de dominio DNS.  
  
-   Para crear un nuevo dominio de árbol, haz clic en **Agregar un nuevo dominio a un bosque existente** y selecciona **Dominio de árbol**. Escribe el nombre del dominio raíz del bosque y después el nombre del nuevo dominio. Especifica un nombre de dominio raíz completo y válido; el nombre no puede ser de etiqueta única y debe cumplir los requisitos de nombre de dominio DNS.  
  
Para obtener más información sobre nombres DNS, consulte [Convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264).  
  
El Asistente para configuración de Servicios de dominio de Active Directory del Administrador del servidor pide confirmación de las credenciales de dominio si las credenciales actuales no se corresponden con el dominio. Haz clic en **Cambiar** para proporcionar las credenciales de dominio para la operación de promoción.  
  
Los argumentos del cmdlet de ADDSDeployment de la página Configuración de la implementación son los siguientes:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opciones del controlador de dominio  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
En la página **Opciones del controlador de dominio** se especifican las opciones del controlador de dominio para el nuevo controlador de dominio. Las opciones del controlador de dominio que pueden configurarse incluyen **Servidor DNS** y **Catálogo global**; el controlador de dominio de solo lectura no puede configurarse como el primer controlador de dominio de un dominio nuevo.  
  
Microsoft recomienda que todos los controladores de dominio proporcionen servicios de DNS y GC para alta disponibilidad en entornos distribuidos. GC siempre está seleccionado de manera predeterminada, excepto si el dominio actual ya hospeda DNS en sus controladores de dominio basándose en la consulta Inicio de autoridad, en cuyo caso la opción predeterminada será Servidor DNS. También es necesario especificar un **Nivel funcional de dominio**. El nivel funcional predeterminado es Windows Server 2012 y se puede elegir cualquier otro valor que sea igual o superior al nivel funcional del bosque actual.  
  
La página **Opciones del controlador de dominio** también permite elegir el **nombre de sitio** lógico apropiado de Active Directory de la configuración del bosque. De manera predeterminada, se selecciona el sitio con la subred más correcta. Si solo hay un sitio, este se seleccionará automáticamente.  
  
> [!IMPORTANT]  
> Si el servidor no pertenece a una subred de Active Directory y hay más de un sitio de Active Directory, no se selecciona nada y el botón **Siguiente** no estará disponible hasta que elijas un sitio de la lista.  
  
La **Contraseña del modo de restauración de servicios de directorio** debe cumplir la directiva de contraseñas aplicada al servidor. Siempre elija una contraseña segura y compleja o, preferentemente, una frase de contraseña.  
  
Los argumentos del cmdlet ADDSDeployment **Opciones del controlador de dominio** son los siguientes:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> El nombre del sitio ya debe existir cuando se proporciona como valor para el argumento **-sitename**. El cmdlet **install-AddsDomainController** no crea nombres de sitios. Se puede usar el cmdlet **new-adreplicationsite** para crear sitios nuevos.  
  
Si no se especifican, los argumentos del cmdlet **Install-ADDSDomainController** tendrán los mismos valores predeterminados que el Administrador del servidor.  
  
La operación del argumento **SafeModeAdministratorPassword** es especial:  
  
-   Si *no se especifica* como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.  
  
    Por ejemplo, para crear un nuevo dominio secundario llamado NorthAmerica en el bosque de Contoso.com y que el sistema pida especificar y confirmar una contraseña enmascarada:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
    ```  
  
-   Si se especifica *con un valor*, este debe ser una cadena segura. Este no es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.  
  
Por ejemplo, puedes solicitar una contraseña de forma manual con el cmdlet **Read-Host** para pedirle al usuario que escriba una cadena segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como la opción anterior no confirma la contraseña, tenga mucho cuidado: la contraseña no es visible.  
  
También puedes proporcionar una cadena segura como una variable no cifrada convertida, pero te recomendamos encarecidamente que no lo hagas.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
Por último, puedes almacenar la contraseña cifrada en un archivo y reutilizarla más adelante, sin que la contraseña aparezca descifrada en ningún momento. Por ejemplo:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar contraseñas no cifradas. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo podría invertir la contraseña cifrada. Con esa información, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y suplantar al propio controlador de dominio, elevando sus privilegios al máximo nivel en el bosque de AD. Te recomendamos que uses un conjunto adicional de pasos con **System.Security.Cryptography** para cifrar los datos del archivo de texto, pero ese tema no se trata aquí. El procedimiento recomendado es no almacenar la contraseña en absoluto.  
  
ADDSDeployment ofrece una opción adicional para omitir la configuración automática de las opciones del cliente DNS, los reenviadores y las sugerencias de raíz. Esto no es configurable al usar el Administrador del servidor. Este argumento solo es relevante si ya has instalado el servicio Servidor DNS antes de configurar el controlador de dominio:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opciones de DNS y credenciales de delegación DNS  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
La página **Opciones de DNS** permite especificar credenciales de administrador de DNS alternativas para la delegación.  
  
Al instalar un nuevo dominio en un bosque existente (si seleccionas la instalación de DNS en la página **Opciones del controlador de dominio**) no se pueden configurar otras opciones; la delegación se produce automáticamente y de forma irrevocable. Puedes especificar credenciales administrativas de DNS alternativas con derechos para actualizar la estructura.  
  
Los argumentos de ADDSDeployment para Windows PowerShell **Opciones de DNS** son los siguientes:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Para obtener más información acerca de la delegación de DNS, consulte [Descripción de tipos de confianza](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11)).  
  
### <a name="additional-options"></a>Opciones adicionales  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
La página **Opciones adicionales** muestra el nombre NetBIOS del dominio y te permite reemplazarlo. De forma predeterminada, el nombre del dominio NetBIOS coincide con la etiqueta del extremo izquierdo del nombre de dominio completo indicada en la página **Configuración de implementación**. Por ejemplo, si indicaste como nombre de dominio completo corp.contoso.com, el nombre del dominio NetBIOS predeterminado será CORP.  
  
Si el nombre tiene 15 caracteres o menos y no entra en conflicto con otro nombre NetBIOS, no se modificará. Si entra en conflicto con otro nombre NetBIOS, se anexará un número al nombre. Si el nombre tiene más de 15 caracteres, el asistente proporcionará una sugerencia truncada que sea única. En cualquiera de estos casos, el asistente validará primero si el nombre no se está usando ya con una búsqueda WINS y difusión NetBIOS.  
  
Para obtener más información sobre nombres DNS, consulte [Convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264).  
  
Si no se especifican, los argumentos de **Install-AddsDomain** tendrán los mismos valores predeterminados que el Administrador del servidor. La operación de **DomainNetBIOSName** es especial:  
  
1.  Si no se especifica el argumento **NewDomainNetBIOSName** con un nombre de dominio NetBIOS y el nombre de dominio con prefijo de etiqueta única en el argumento **DomainName** tiene 15 caracteres o menos, la promoción continuará con un nombre generado automáticamente.  
  
2.  Si no se especifica el argumento **NewDomainNetBIOSName** con un nombre de dominio NetBIOS y el nombre de dominio con prefijo de etiqueta única en el argumento **DomainName** tiene 16 caracteres o más, la promoción producirá errores.  
  
3.  Si se especifica el argumento **NewDomainNetBIOSName** con un nombre de dominio NetBIOS de 15 caracteres o menos, la promoción continuará con el nombre especificado.  
  
4.  Si se especifica el argumento **NewDomainNetBIOSName** con un nombre de dominio NetBIOS de 16 caracteres o más, la promoción producirá errores.  
  
El argumento del cmdlet de ADDSDeployment correspondiente a las **Opciones adicionales** es el siguiente:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Rutas de acceso  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
La página **Rutas de acceso** permite reemplazar las ubicaciones de carpeta predeterminadas de la base de datos AD DS, los registros de transacciones de la base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas siempre están en subdirectorios de %systemroot%.  
  
Los argumentos del cmdlet ADDSDeployment correspondientes a **Rutas de acceso** son:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Revisión de opciones y visualización del script  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
La página **Revisar opciones** te permite validar la configuración y comprobar que coincide con lo que necesitas antes de iniciar la instalación. No es la última oportunidad para detener la instalación con el Administrador del servidor. Es, simplemente, una opción para confirmar la configuración antes de continuar.  
  
La página **Revisar opciones** del Administrador del servidor también ofrece un botón **Ver script** opcional para crear un archivo de texto Unicode que contiene la configuración ADDSDeployment actual como un script Windows PowerShell único. Esto le permite usar la interfaz gráfica del Administrador del servidor como un estudio de implementación de Windows PowerShell. Use el Asistente para configuración de Servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, cancele el asistente.  Este proceso crea un ejemplo válido y sintácticamente correcto para realizar más modificaciones o la utilización directa. Por ejemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Normalmente, el Administrador del servidor rellena todos los argumentos con valores al realizar la promoción y no se basa en los valores predeterminados (dado que pueden cambiar entre las futuras versiones de Windows o los Service Pack). La única excepción de esta regla es el argumento **-safemodeadministratorpassword** (que se omite del script deliberadamente). Para forzar la aparición de un aviso de confirmación, omite el valor al ejecutar el cmdlet de forma interactiva.  
  
Utiliza el argumento opcional **Whatif** con el cmdlet **Install-ADDSForest** para revisar la información de configuración. Esto te permite ver los valores explícitos e implícitos de los argumentos de un cmdlet.  
  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Prerequisites Check  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
La **Comprobación de requisitos previos** es una característica nueva en la configuración de dominios de AD DS. En esta nueva fase se comprueba si la configuración del servidor admite un nuevo dominio de AD DS.  
  
Al instalar un nuevo dominio raíz de bosque, el Asistente para configuración de Servicios de dominio de Active Directory del Administrador del servidor invoca una serie de pruebas modulares en serie. Las pruebas te envían alertas con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso del controlador de dominio no puede continuar hasta que se superen todas las pruebas de requisitos previos.  
  
La **Comprobación de requisitos previos** también expone información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.  
  
Para más información sobre las comprobaciones de requisitos previos específicas, consulta [Comprobación de requisitos previos](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
No puedes omitir la **Comprobación de requisitos previos** al usar el Administrador del servidor, pero sí al utilizar el cmdlet de implementación de AD DS con el siguiente argumento:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft no recomienda omitir la comprobación de requisitos previos: omitirla puede hacer que los controladores de dominio se promuevan parcialmente o puede provocar daños en el bosque de AD DS.  
  
Haz clic en **Instalar** para comenzar el proceso de promoción del controlador de dominio. Esta es la última oportunidad para cancelar la instalación. Una vez iniciado, el proceso de promoción no se puede cancelar. El equipo se reiniciará automáticamente al finalizar la promoción, independientemente del resultado de la misma.  
  
### <a name="installation"></a>Instalación  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Cuando se muestra la página **Instalación**, la configuración del controlador de dominio comienza y no se puede detener ni cancelar. Las operaciones detalladas se muestran en esta página y se escriben en los registros:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar un nuevo dominio de Active Directory con ADDSDeployment, usa el siguiente cmdlet:  
  
```  
Install-addsdomain  
```  
  
Consulta los argumentos obligatorios y opcionales en [Windows PowerShell de dominios secundarios y de árbol](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS). El cmdlet **Install-addsdomain** solo tiene dos fases (comprobación de requisitos previos e instalación). En las dos ilustraciones siguientes verás la fase de instalación con el número mínimo de argumentos necesarios para **-domaintype**, **-newdomainname**, **-parentdomainname** y **-credential**. Al igual que el Administrador del servidor, **Install-ADDSDomain** te recuerda que la promoción reiniciará el servidor automáticamente.  
  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Para aceptar el aviso de reinicio de forma automática, utiliza los argumentos **-force** o **-confirm:$false** con cualquier cmdlet de ADDSDeployment de Windows PowerShell. Para impedir que el servidor se reinicie automáticamente al finalizar la promoción, utiliza el argumento **-norebootoncompletion**.  
  
> [!WARNING]  
> Se recomienda no invalidar el reinicio. El controlador de dominio debe reiniciarse para funcionar correctamente  
  
### <a name="results"></a>Results  
![Instalación de un nuevo elemento secundario de AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La página **Resultados** indica si la promoción se realizó correctamente o si se produjo algún error, junto con toda la información administrativa importante que corresponda. El controlador de dominio se reiniciará automáticamente 10 segundos después.  
  
