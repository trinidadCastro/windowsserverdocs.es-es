---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: Degradar controladores de dominio y dominios (nivel 200)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c254a01da5534c1ddc673bc1e60382c166ddeda7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="demoting-domain-controllers-and-domains-level-200"></a>Degradar controladores de dominio y dominios (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica cómo quitar AD DS, con el administrador del servidor o Windows PowerShell.  
  
-   [Flujo de trabajo de eliminación de AD DS](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Workflow)  
  
-   [Degradación de rol eliminación Windows PowerShell](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_PS)  
  
-   [Volver al estado anterior](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Demote)  
  
## <a name="BKMK_Workflow"></a>Flujo de trabajo de eliminación de AD DS  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  
  
> [!CAUTION]  
> Quita los roles de AD DS con Dism.exe o el módulo DISM de PowerShell de Windows después de promoción para un controlador de dominio no se admite y evitará que el servidor de arranque con normalidad.  
>   
> A diferencia del administrador del servidor o el módulo de ADDSDeployment para Windows PowerShell, DISM es un sistema de mantenimiento nativo que tiene ningún conocimiento inherente de AD DS o su configuración. No uses Dism.exe o el módulo DISM de Windows PowerShell para desinstalar el rol de AD DS, a menos que el servidor ya no es un controlador de dominio.  
  
## <a name="BKMK_PS"></a>Degradación de rol eliminación Windows PowerShell  
  
|||  
|-|-|  
|**ServerManager Cmdlets y ADDSDeployment**|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirmar<br /><br />***-Credenciales***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Desinstalar-WindowsFeature/Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-Reinicio*<br /><br />-Quitar<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credenciales<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> La **-credenciales** argumento es solo requiere si no ya sesión como miembro del grupo Administradores de empresa (degradar el último controlador de dominio en un dominio) o el grupo de administradores de dominio (degradar una réplica DC).The **- includemanagementtools** argumento solo es necesaria si desea quitar todas las utilidades de administración de AD DS.  
  
## <a name="BKMK_Demote"></a>Volver al estado anterior  
  
### <a name="remove-roles-and-features"></a>Quitar Roles y características  
El administrador del servidor ofrece dos interfaces para quitar la función de los servicios de dominio de Active Directory:  
  
-   La **administrar** menú en el panel principal, con **quitar Roles y características**  
  
 ![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  
  
-   Haz clic en **AD DS** o **todos los servidores** en el panel de navegación. Desplázate hacia abajo hasta la **Roles y características** sección. Haz clic en **los servicios de dominio de Active Directory** en la **Roles y características** y haga clic en **quitar rol o característica **. Esta interfaz se omite la **selección de servidor** página.  
  
 ![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  
  
Los cmdlets ServerManager **desinstalar-WindowsFeature** y **Remove-WindowsFeature** impedir que quite el rol de AD DS hasta degradar el controlador de dominio.  
  
### <a name="server-selection"></a>Selección de servidor  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  
  
La **selección de servidor** cuadro de diálogo te permite elegir uno de los servidores que se agregó al grupo, siempre que es accesible. El servidor local con el administrador del servidor siempre está disponible automáticamente.  
  
### <a name="server-roles-and-features"></a>Características y los Roles de servidor  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  
  
Borrar la **los servicios de dominio de Active Directory** casilla degradar un controlador de dominio; Si el servidor está actualmente un controlador de dominio, esto no elimina el rol de AD DS y en su lugar, cambia a un **resultados de la validación** cuadro de diálogo con la oferta degradar. De lo contrario, simplemente quita los archivos binarios como cualquier otra característica de rol.  
  
-   Si tienes previsto promover el controlador de dominio nuevo inmediatamente, no quitar otros roles de AD DS relacionados con ni características - como DNS, GPMC o las herramientas RSAT. Eliminación de características y funciones adicionales aumenta el tiempo necesario para volver a promover, como el administrador del servidor se vuelve a instalar estas características al reinstalar el rol.  
  
-   Quitar innecesarios de AD DS roles y características en su propio criterio si tienes previsto degradar el controlador de dominio de forma permanente. Para ello, desactive las casillas de los roles y características.  
  
    La lista completa de AD DS relacionados con roles y características se incluyen:  
  
    -   Característica de Active Directory módulo para Windows PowerShell  
  
    -   Característica de AD DS y AD LDS herramientas  
  
    -   Característica de centro de administración de Active Directory  
  
    -   Complementos de AD DS y herramientas de línea de comandos de la característica  
  
    -   Servidor DNS  
  
    -   Consola de administración de directivas de grupo  
  
Son los equivalentes cmdlets ADDSDeployment y ServerManager Windows PowerShell:  
  
```  
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
  
```  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  
  
### <a name="credentials"></a>Credenciales  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  
  
Configurar las opciones de degradación en el **credenciales** página. Proporcionar las credenciales necesarias para realizar la degradación de la siguiente lista:  
  
-   Degradar un controlador de dominio requiere credenciales de administrador de dominio. Seleccionar **forzar la eliminación de este controlador de dominio** devolverá el controlador de dominio sin quitar los metadatos del objeto de controlador de dominio de Active Directory.  
  
    > [!WARNING]  
    > No actives esta opción a menos que el controlador de dominio no puede ponerse en contacto con otros controladores de dominio y no hay *ninguna forma razonable* para resolver el problema de esa red. Degradación forzada deja huérfana metadatos en Active Directory en los controladores de dominio restantes en el bosque. Además, todos los cambios sin duplicados en el controlador de dominio, como contraseñas o nuevas cuentas de usuario, se perderán para siempre. Metadatos huérfana están la causa raíz en un gran porcentaje de los casos de atención al cliente de Microsoft para AD DS, Exchange, SQL y otro software.  
    >   
    > Si forzar la degradación de un controlador de dominio, puedes *debe* manualmente limpiar los metadatos inmediatamente. Para conocer los pasos, revisar [limpia metadatos de servidor](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
   ![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
-   Degradar el último controlador de dominio en un dominio requiere la pertenencia al grupo de administradores de empresa, como esta directiva quita del propio dominio (si el último dominio del bosque, esta directiva quita el bosque). El administrador del servidor le informa de si el controlador de dominio actual es el último controlador de dominio del dominio. Selecciona el **último controlador de dominio en el dominio** casilla de verificación para confirmar el controlador de dominio es el último controlador de dominio del dominio.  
  
Los argumentos de ADDSDeployment Windows PowerShell equivalentes son:  
  
```  
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
  
```  
  
### <a name="warnings"></a>Advertencias  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  
  
La **advertencias** página le alerta sobre las posibles consecuencias quitar este controlador de dominio. Para continuar, debes seleccionar **continuar con la eliminación de**.  
  
> [!WARNING]  
> Si anteriormente seleccionaste **forzar la eliminación de este controlador de dominio** en la **credenciales** página, la **advertencias** página muestra todos los roles de operaciones de maestro único Flexible hospedados por este controlador de dominio. Puedes *debe* asumir las funciones de otro controlador de dominio *inmediatamente* después de la devolución de este servidor. Para obtener más información sobre asumir funciones FSMO, consulta [asumir la función de maestro de operaciones](https://technet.microsoft.com/library/cc816779(WS.10).aspx).  
  
Esta página no tiene un equivalente argumento ADDSDeployment Windows PowerShell.  
  
### <a name="removal-options"></a>Opciones de eliminación  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  
  
La **opciones de eliminación** aparecerá la página según anteriormente seleccionar **último controlador de dominio en el dominio** en la **credenciales** página. Esta página te permite configurar las opciones adicionales de eliminación. Selecciona **ignorar último servidor DNS para zona**, **quitar particiones de la aplicación**, y **quitar delegación de DNS** para exponer el **siguiente** botón.  
  
Las opciones solo aparecen si corresponde a este controlador de dominio. Por ejemplo, si no hay ninguna delegación de DNS para este servidor esa casilla de verificación no se mostrará.  
  
Haz clic en **cambio** a especificar otras credenciales administrativas de DNS. Haz clic en **ver particiones** para ver el asistente quita durante la degradación de particiones adicionales. De manera predeterminada, las particiones adicionales solo son de dominio DNS y las zonas de DNS de bosque. Todas las demás particiones son particiones no son de Windows.  
  
Los argumentos de cmdlet ADDSDeployment equivalentes son:  
  
```  
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```  
  
### <a name="new-administrator-password"></a>Nueva contraseña de administrador  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  
  
La **nueva contraseña de administrador** página requiere que proporciones una contraseña de cuenta de administrador del equipo local integrada, una vez que finalice la degradación y el equipo se convierte en un servidor miembro del dominio o grupo de trabajo.  
  
La **desinstalar ADDSDomainController** cmdlet y argumentos siguen los valores predeterminados mismo como el administrador del servidor si no se especifica.  
  
La **LocalAdministratorPassword** argumento es especial:  
  
-   Si *no especifica* como un argumento, a continuación, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva  
  
-   Si se especifica *con un valor*, a continuación, el valor debe ser una cadena segura. Esto no es el uso preferido cuando se ejecuta el cmdlet de manera interactiva  
  
Por ejemplo, puedes manualmente pedir contraseña utilizando la **Read-Host** cmdlet para pedir al usuario una cadena segura  
  
```  
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como las dos opciones anteriores no confirmación la contraseña, Ten mucho cuidado: la contraseña no está visible  
  
También puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda. Por ejemplo:  
  
```  
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar una contraseña de texto sin cifrar. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña de administrador local de ese equipo. Con estos datos, tienen acceso a todos sus datos y puede suplantar al propio servidor.  
  
### <a name="confirmation"></a>Confirmación  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)  
  
La **confirmación** página muestra la degradación planeada; la página no enumera las opciones de configuración degradación. Esta es la última página que el asistente muestra antes de que comience la degradación. El botón de vista Script crea un script de degradación de Windows PowerShell.  
  
Haz clic en **degradar** para ejecutar el cmdlet de AD DS implementación siguiente:  
  
```  
Uninstall-DomainController  
  
```  
  
Usar opcional **Whatif** argumento con la **desinstalar ADDSDomainController** y cmdlet para revisar la información de configuración. Esto te permite ver los valores implícitos y explícitos de argumentos de un cmdlet.  
  
Por ejemplo:  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)  
  
El símbolo del sistema para reiniciar es tu última oportunidad para cancelar la operación al usar ADDSDeployment Windows PowerShell. Para invalidar esa símbolo del sistema, usa el **-forzar** o **confirmar: $false** argumentos.  
  
### <a name="demotion"></a>Degradación  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  
  
Cuando la **degradación** muestra la página, la configuración del controlador de dominio comienza y no se han detenido o cancela. Operaciones detalladas en esta página muestran y escriben en los registros:  
  
-   %systemroot%\Debug\Dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Dado que **desinstalar AddsDomainController** y **desinstalar-WindowsFeature** solo tienen un precio por unidad de una acción, aquí se muestran en la fase de confirmación con los argumentos requeridos mínimos. Presiona ENTRAR inicia el proceso de degradación irrevocable y reinicia el equipo.  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)  
  
Para aceptar automáticamente la consulta de reinicio, usa el **-forzar** o **-confirmar: $false** argumentos con cualquier cmdlet ADDSDeployment Windows PowerShell. Para evitar que el servidor reiniciar automáticamente al final de promoción, usa el **- norebootoncompletion: $false** argumento.  
  
> [!WARNING]  
> No se recomienda reemplazar el reinicio. El servidor miembro debe reiniciar para que funcione correctamente.  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)  
  
Este es un ejemplo de forzar la degradación con los argumentos requeridos mínimos de **- forceremoval** y **- demoteoperationmasterrole**. La **-credenciales** argumento no es necesario porque el usuario ha iniciado sesión como miembro del grupo Administradores de empresa:  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)  
  
Este es un ejemplo de quitar el último controlador de dominio en el dominio con los argumentos requeridos mínimos de **- lastdomaincontrollerindomain** y **- removeapplicationpartitions**:  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)  
  
Si se intenta quitar el rol de AD DS antes de degradar al servidor, Windows PowerShell se bloquea con un error de forma intencionado:  
  
```  
Uninstall-WindowsFeature : An uninstallation prerequisite step failed duringthe removal of AD-Domain-Services, and uninstallation cannot continue.1. The domain controller needs to be demoted before the Active DirectoryDomain Services Role can be uninstalled.  
```  
  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)  
  
> [!IMPORTANT]  
> Debes reiniciar el equipo después de la devolución del servidor antes de quitar los archivos binarios de rol de servicios de dominio de AD.  
  
### <a name="results"></a>Resultados  
![Degradar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)  
  
La **resultados** página muestra el éxito o error de la promoción y cualquier información administrativa importante. El controlador de dominio se reiniciará automáticamente después de 10 segundos.  
  

