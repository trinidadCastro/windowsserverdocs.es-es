---
description: Más información acerca de cómo instalar un controlador de dominio de Windows Server 2012 de réplica en un dominio existente (nivel 200)
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: Instalar una réplica del controlador de dominio de Windows Server 2012 en un dominio existente (nivel 200)
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6be71576dd8d31d50fac5527a1fab5b1631f27a0
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049513"
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Instalar una réplica del controlador de dominio de Windows Server 2012 en un dominio existente (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describen los pasos necesarios para actualizar un bosque o dominio existente a Windows Server 2012, bien con el Administrador del servidor o con Windows PowerShell. Se describe cómo agregar controladores de dominio que ejecuten Windows Server 2012 a un dominio existente.

-   [Flujo de trabajo de actualizaciones y réplicas](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)

-   [Actualización y réplica con Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)

-   [Implementación](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)

## <a name="upgrade-and-replica-workflow"></a><a name="BKMK_Workflow"></a>Flujo de trabajo de actualizaciones y réplicas
En el diagrama siguiente verás el proceso de configuración de Servicios de dominio de Active Directory cuando se ha instalado anteriormente el rol de AD DS y se ha iniciado el Asistente para configuración de Servicios de dominio de Active Directory con el Administrador del servidor para crear un nuevo controlador de dominio en un dominio existente.

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)

## <a name="upgrade-and-replica-windows-powershell"></a><a name="BKMK_PS"></a>Actualización y réplica con Windows PowerShell

| Cmdlet ADDSDeployment | Argumentos (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS). |
|--|--|
| Install-AddsDomainController | -SkipPreChecks<p>***-DomainName **_<p>_ -SafeModeAdministratorPassword* <p> *-siteName* <p> *-ADPrepCredential* <p> -ApplicationPartitionsToReplicate <p> *-AllowDomainControllerReinstall* <p> -CONFIRM <p> *-CreateDNSDelegation* <p>***-Credential **_<p> -CriticalReplicationOnly <p>_-DatabasePath*<p>*-DNSDelegationCredential*<p>-Force<p>*-InstallationMediaPath*<p>*-InstallDNS*<p>*-LogPath*<p>-MoveInfrastructureOperationMasterRoleIfNecessary<p>-NoDnsOnNetwork<p>*-NoGlobalCatalog*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>-SkipAutoConfigureDNS<p>-SiteName<p>*-SystemKey*<p>*-SYSVOLPath*<p>*-UseExistingAccount*<p>*-Whatif* |

> [!NOTE]
> El argumento **-credential** solo es necesario si no se ha iniciado sesión como un miembro de los grupos Administradores de empresas y Administradores de esquema (si se va a actualizar el bosque) o el grupo Administradores del dominio (si se va a agregar un nuevo DC a un dominio existente).

## <a name="deployment"></a><a name="BKMK_Dep"></a>Planta

### <a name="deployment-configuration"></a>Configuración de la implementación
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)

El Administrador del servidor comienza la promoción de cada controlador de dominio con la página **Configuración de implementación**. Las opciones restantes y los campos requeridos cambian en esta página y en las páginas siguientes, según qué operación de implementación se seleccione.

Para actualizar un bosque existente o agregar un controlador de dominio que permita la escritura a un dominio existente, haz clic en **Agregar un controlador de dominio a un dominio existente** y después en **Seleccionar** para **Especificar la información de dominio para este dominio**. El Administrador del servidor solicita que escribas credenciales válidas si es necesario.

Para actualizar un bosque se necesitan credenciales, como la pertenencia a los grupos Administradores de empresas y Administradores de esquema en Windows Server 2012. El Asistente para configuración de Servicios de dominio de Active Directory le pedirá confirmación si las credenciales actuales no tienen los permisos o la pertenencia a grupos adecuados.

El proceso automático Adprep es la única diferencia entre agregar un controlador de dominio a un dominio existente de Windows Server 2012 o a un dominio donde los controladores de dominio ejecuten una versión anterior de Windows Server.

Los argumentos del cmdlet de ADDSDeployment de la página Configuración de la implementación son los siguientes:

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)

Determinadas pruebas se ejecutan en todas las páginas, y algunas de ellas se repiten posteriormente como comprobaciones de requisitos previos discretas. Por ejemplo, si el dominio seleccionado no cumple con los niveles funcionales mínimos, no será necesario completar todo el proceso de promoción hasta la comprobación de requisitos previos para saberlo:

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)

### <a name="domain-controller-options"></a>Opciones del controlador de dominio
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)

La página **Opciones del controlador de dominio** especifica las funcionalidades del nuevo controlador de dominio. Las funcionalidades del controlador de dominio que se pueden configurar son **Servidor DNS**, **Catálogo global** y **Controlador de dominio de solo lectura**. Microsoft recomienda que todos los controladores de dominio proporcionen servicios de DNS y GC para alta disponibilidad en entornos distribuidos. GC siempre está seleccionado de forma predeterminada y el servidor DNS está seleccionado de forma predeterminada si el dominio actual ya hospeda un DNS en sus controladores de dominio según la consulta de Inicio de autoridad. La página **Opciones del controlador de dominio** también permite elegir el **nombre de sitio** lógico apropiado de Active Directory de la configuración del bosque. De forma predeterminada, selecciona el sitio con la subred más correcta. Si solo hay un sitio, lo seleccionará automáticamente.

> [!NOTE]
> Si el servidor no pertenece a una subred de Active Directory y hay más de un sitio de Active Directory, no se selecciona nada y el botón **Siguiente** no estará disponible hasta que elijas un sitio de la lista.

La **Contraseña del modo de restauración de servicios de directorio** debe cumplir la directiva de contraseñas aplicada al servidor. Siempre elija una contraseña segura y compleja o, preferentemente, una frase de contraseña.

Los argumentos de ADDSDeployment de **Opciones del controlador de dominio** son los siguientes:

```
-InstallDNS <{$false | $true}>
-NoGlobalCatalog <{$false | $true}>
-sitename <string>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> El nombre del sitio ya debe existir cuando se proporciona como un argumento para **-sitename**. El cmdlet **install-AddsDomainController** no crea sitios nuevos. Puedes usar el cmdlet **new-adreplicationsite** para crear nuevos sitios.

La operación del argumento **SafeModeAdministratorPassword** es especial:

-   Si *no se especifica* como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

    Por ejemplo, para crear un controlador de dominio adicional en el dominio treyresearch.net y que el sistema pida especificar y confirmar una contraseña enmascarada:

    ```
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)
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
> No se recomienda proporcionar ni almacenar contraseñas no cifradas. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo podría invertir la contraseña cifrada. Si conocen la contraseña, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y llegar a suplantar el controlador de dominio. Elevarían sus privilegios al nivel máximo en el bosque de Active Directory. Te recomendamos que uses un conjunto adicional de pasos con **System.Security.Cryptography** para cifrar los datos del archivo de texto, pero ese tema no se trata aquí. El procedimiento recomendado es no almacenar la contraseña en absoluto.

El cmdlet de ADDSDeployment ofrece una opción adicional para omitir la configuración automática de las opciones, los reenviadores y las sugerencias de raíz del cliente DNS. Al usar el Administrador del servidor, no se puede omitir esta opción de configuración. Este argumento solo es relevante si instalaste el rol del servidor DNS antes de configurar el controlador de dominio:

```
-SkipAutoConfigureDNS
```

En la página **Opciones del controlador de dominio** se advierte de que no se pueden crear controladores de dominio de solo lectura si los controladores de dominio existentes ejecutan Windows Server 2003. Este es el comportamiento esperado y se puede ignorar la advertencia.

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)

### <a name="dns-options-and-dns-delegation-credentials"></a>Opciones de DNS y credenciales de delegación DNS
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)

En la página **Opciones de DNS** puedes configurar la delegación DNS si seleccionaste la opción **Servidor DNS** en la página *Opciones del controlador de dominio* y si apuntaste a una zona donde se permitían las delegaciones DNS. Puede que tengas que proporcionar credenciales alternativas de un usuario que sea miembro del grupo **Administradores de DNS**.

Los argumentos del cmdlet de ADDSDeployment **Opciones de DNS** son los siguientes:

```
-creatednsdelegation
-dnsdelegationcredential <pscredential>
```

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)

Para obtener más información sobre si es necesario crear una delegación DNS, vea [Descripción de la delegación de zonas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11)).

### <a name="additional-options"></a>Opciones adicionales
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)

En la página **Opciones adicionales** se encuentra la opción de configuración para asignar un nombre a un controlador de dominio como el origen de replicación, o bien se puede usar cualquier controlador de dominio como el origen de replicación.

También puede eligir instalar el controlador de dominio con medios con copia de seguridad mediante la opción Instalar desde medios (IFM). Una vez activada, la casilla **Instalar desde el medio** proporciona una opción de exploración y debes hacer clic en **Comprobar** para asegurarte de que la ruta proporcionada es un medio válido. Los medios usados por la opción IFM solo se crean con Copias de seguridad de Windows Server o Ntdsutil.exe a partir de otro equipo de Windows Server 2012 existente; no se puede usar Windows Server 2008 R2 o un sistema operativo anterior para crear los medios para un controlador de dominio de Windows Server 2012. Para obtener más información sobre los cambios en IFM, consulta [Apéndice de administración simplificada](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Si usas medios protegidos con SYSKEY, el Administrador del servidor pregunta la contraseña de la imagen durante la comprobación.

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)

Los argumentos del cmdlet ADDSDeployment correspondientes a las **Opciones adicionales** son:

```
-replicationsourcedc <string>
-installationmediapath <string>
-syskey <secure string>
```

### <a name="paths"></a>Rutas de acceso
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)

La página **Rutas de acceso** permite reemplazar las ubicaciones de carpeta predeterminadas de la base de datos AD DS, los registros de transacciones de la base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas siempre están en subdirectorios de %systemroot%.

Los argumentos del cmdlet de ADDSDeployment Rutas de Active Directory son los siguientes:

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="preparation-options"></a>Preparation Options
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)

La página **Opciones de preparación** te alerta de que la configuración de AD DS incluye la extensión del esquema (forestprep) y la actualización del dominio (domainprep).  Esta página solo se muestra cuando el bosque y el dominio no se han preparado mediante una instalación anterior del controlador de dominio de Windows Server 2012 o mediante la ejecución manual de Adprep.exe. Por ejemplo, el Asistente para configuración de Active Directory Domain Services elimina esta página si se agrega un nuevo controlador de dominio a un dominio raíz del bosque de Windows Server 2012 existente.

Al hacer clic en **Siguiente** no se extiende el esquema ni se actualiza el dominio. Estos eventos solo se producen durante la fase de instalación. Esta página simplemente informa de los eventos que se producirán más adelante en la instalación.

Esta página también comprueba que las credenciales de usuario actuales pertenecen a los grupos Administradores de esquema o Administradores de organización, porque necesitas pertenecer a estos grupos para extender el esquema o preparar un dominio. Haz clic en **Cambiar** para proporcionar las credenciales de usuario adecuadas si la página te informa de que las credenciales actuales no proporcionan los permisos suficientes.

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)

El argumento del cmdlet de ADDSDeployment correspondiente a las Opciones adicionales es:

```
-adprepcredential <pscredential>
```

> [!IMPORTANT]
> Al igual que en versiones anteriores de Windows Server, la preparación automática de dominios para controladores de dominio que ejecuten Windows Server 2012 no ejecuta GPPREP. Ejecuta **adprep.exe /gpprep** manualmente para todos los dominios que Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 no prepararon anteriormente. Ejecuta GPPrep solo una vez en el historial de un dominio, no con cada actualización. Adprep.exe no ejecuta /gpprep automáticamente porque podría hacer que todos los archivos y carpetas que hay en la carpeta SYSVOL se vuelvan a replicar en todos los controladores de dominio.
>
> RODCPrep se ejecuta automáticamente cuando se promueve el primer RODC sin preconfigurar de un dominio. Esto no sucede cuando se promueve el primer controlador de dominio de Windows Server 2012 de escritura. Aun así, puedes ejecutar de forma manual **adprep.exe /rodcprep** si quieres implementar controladores de dominio de solo lectura.

### <a name="review-options-and-view-script"></a>Revisión de opciones y visualización del script
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)

La página **Revisar opciones** permite validar la configuración y asegura que esta cumpla con los requisitos antes de comenzar con la instalación. Esta no es la última oportunidad para detener la instalación con el Administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con esta.

La página **Revisar opciones** del Administrador del servidor también ofrece un botón **Ver script** opcional para crear un archivo de texto Unicode que contiene la configuración ADDSDeployment actual como un script Windows PowerShell único. Esto le permite usar la interfaz gráfica del Administrador del servidor como un estudio de implementación de Windows PowerShell. Use el Asistente para configuración de Servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, cancele el asistente.  Este proceso crea un ejemplo válido y sintácticamente correcto para realizar más modificaciones o la utilización directa.

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
> Normalmente, el Administrador del servidor rellena todos los argumentos con valores al realizar la promoción y no se basa en los valores predeterminados (dado que pueden cambiar entre las futuras versiones de Windows o los Service Pack). La excepción es el argumento **-safemodeadministratorpassword**. Para aplicar una pregunta de confirmación, pasa por alto el valor al ejecutar el cmdlet interactivamente.
>
> Use el argumento opcional **Whatif** con el cmdlet **Install-ADDSDomainController** para revisar la información de la configuración. Esto te permite ver los valores explícitos e implícitos de los argumentos de un cmdlet.

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)

### <a name="prerequisites-check"></a>Prerequisites Check
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)

La **Comprobación de requisitos previos** es una característica nueva en la configuración de dominios de AD DS. Esta nueva fase comprueba si el dominio y el bosque admiten un nuevo controlador de dominio de Windows Server 2012.

Al instalar un nuevo controlador de dominio, el Asistente para configuración de Servicios de dominio de Active Directory del Administrador del servidor invoca diversas pruebas modulares en serie. Las pruebas te envían alertas con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso del controlador de dominio no puede continuar hasta que se superen todas las pruebas de requisitos previos.

La **Comprobación de requisitos previos** también expone información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.

Para obtener más información sobre las comprobaciones específicas de requisitos previos, consulta [Comprobación de requisitos previos](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).

No puedes omitir la **Comprobación de requisitos previos** al usar el Administrador del servidor, pero sí al utilizar el cmdlet de implementación de AD DS con el siguiente argumento:

```
-skipprechecks

```

> [!WARNING]
> Microsoft no recomienda omitir la comprobación de requisitos previos: omitirla puede hacer que los controladores de dominio se promuevan parcialmente o puede provocar daños en el bosque de AD DS.

Haz clic en **Instalar** para comenzar el proceso de promoción del controlador de dominio. Esta es la última oportunidad para cancelar la instalación. Una vez iniciado, el proceso de promoción no se puede cancelar. El equipo se reiniciará automáticamente cuando se complete la promoción, independientemente de los resultados de esta. En la página **Comprobación de requisitos previos** se muestran los problemas detectados durante el proceso y las instrucciones para solucionarlos.

### <a name="installation"></a>Instalación
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)

Cuando se muestra la página **Instalación**, la configuración del controlador de dominio comienza y no se puede detener ni cancelar. Las operaciones detalladas se muestran en esta página y se escriben en los registros:

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

-   %systemroot%\debug\adprep\logs

-   %systemroot%\debug\netsetup.log (si el servidor se encuentra en un grupo de trabajo)

Para instalar un nuevo bosque de Active Directory con el módulo ADDSDeployment, utiliza este cmdlet:

```
Install-addsdomaincontroller
```

Consulta los argumentos obligatorios y opcionales en [Actualización y réplica con Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS).

El cmdlet **Install-AddsDomainController** solo tiene dos fases (comprobación de requisitos previos e instalación). En las dos ilustraciones siguientes se muestra la fase de instalación con el número mínimo de argumentos necesarios para **-domainname** y **-credential**. Ten en cuenta que la operación de Adprep se produce automáticamente como parte del proceso de agregar el primer controlador de dominio de Windows Server 2012 a un bosque de Windows Server 2003 existente:

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)

Al igual que el Administrador del servidor, **Install-ADDSDomainController** recuerda que la promoción reiniciará automáticamente el servidor. Para aceptar el aviso de reinicio de forma automática, utiliza los argumentos **-force** o **-confirm:$false** con cualquier cmdlet de ADDSDeployment de Windows PowerShell. Para impedir que el servidor se reinicie automáticamente al finalizar la promoción, utiliza el argumento **-norebootoncompletion**.

> [!WARNING]
> No se recomienda invalidar el reinicio. El controlador de dominio debe reiniciarse para funcionar correctamente.

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)

Para configurar un controlador de dominio de forma remota con Windows PowerShell, encapsula el cmdlet **install-addsdomaincontroller** *dentro* del cmdlet **Invoke-Command** . Para ello es necesario usar llaves.

```
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>
```

Por ejemplo:

![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)

> [!NOTE]
> Para obtener más información sobre el funcionamiento de la instalación y el proceso ADPrep, consulta [Solución de problemas de implementación de controladores de dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).

### <a name="results"></a>Results
![Instalación de una réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)

La página **Resultados** indica si la promoción se realizó correctamente o si se produjo algún error, junto con toda la información administrativa importante que corresponda. Si se completa correctamente, el controlador de dominio se reiniciará automáticamente en 10 segundos.

Al igual que en versiones anteriores de Windows Server, la preparación automática de dominios para controladores de dominio que ejecuten Windows Server 2012 no ejecuta GPPREP. Ejecuta **adprep.exe /gpprep** manualmente para todos los dominios que Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 no prepararon anteriormente. Ejecuta GPPrep solo una vez en el historial de un dominio, no con cada actualización. Adprep.exe no ejecuta /gpprep automáticamente porque podría hacer que todos los archivos y carpetas que hay en la carpeta SYSVOL se vuelvan a replicar en todos los controladores de dominio.

