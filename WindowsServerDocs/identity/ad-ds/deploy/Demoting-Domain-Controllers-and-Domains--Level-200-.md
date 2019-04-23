---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: Degradación de controladores de dominio y de dominios (nivel 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9b3db1390e8191451ef270ce29a2a37463b1de3c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869776"
---
# <a name="demoting-domain-controllers-and-domains"></a>Degradación de controladores de dominio y dominios

>Se aplica a: Windows Server

En este tema se explica cómo quitar AD DS usando el Administrador del servidor o Windows PowerShell.
  
## <a name="ad-ds-removal-workflow"></a>Flujo de trabajo de eliminación de AD DS

![Gráfico de flujo de trabajo de eliminación de AD DS](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  

> [!CAUTION]
> No se admite la eliminación de roles de AD DS con Dism.exe ni con el módulo DISM de Windows PowerShell después de la promoción a controlador de dominio. Si se hace, el servidor no podrá arrancar con normalidad.
>
> Al contrario que el Administrador del servidor o el módulo ADDSDeployment de Windows PowerShell, DISM es un sistema de mantenimiento nativo que no tiene un conocimiento inherente de AD DS o de su configuración. No utilices Dism.exe ni el módulo DISM de Windows PowerShell para desinstalar el rol de AD DS, salvo que el servidor ya no sea un controlador de dominio.

## <a name="demotion-and-role-removal-with-powershell"></a>Eliminación de rol y degradación con PowerShell

|||  
|-|-|  
|**Cmdlets ADDSDeployment y ServerManager**|Argumentos (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS).|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirm<br /><br />***-Credential***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Uninstall-WindowsFeature/Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-Restart*<br /><br />-Remove<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credential<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> El argumento **-credential** solo es obligatorio si no se ha iniciado sesión como miembro del grupo Administradores de empresas (disminuyendo de nivel el último DC en un dominio) o del grupo Admins. del dominio (disminuyendo de nivel una réplica de DC). El argumento **-includemanagementtools** solo es obligatorio si se quieren quitar las utilidades de administración de AD DS.  
  
## <a name="demote"></a>Disminuir nivel  
  
### <a name="remove-roles-and-features"></a>Quitar roles y funciones

El Administrador del servidor ofrece dos interfaces para quitar el rol de Servicios de dominio de Active Directory:  
  
* En el menú **Administrar** del panel principal, selecciona **Quitar roles y funciones**  

   ![Administrador del servidor - quitar Roles y características](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  

* Haz clic en **AD DS** o en **Todos los servidores** en el panel de navegación. Desplázate hacia abajo hasta llegar a la sección **Roles y características**. Haz clic con el botón secundario en **Servicios de dominio de Active Directory** en la lista **Roles y características** y haz clic en **Quitar rol o característica**. Esta interfaz omite la página **Selección de servidor**.  

   ![Administrador del servidor - todos los servidores o quitar Roles y características](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  

Los cmdlets ServerManager **Uninstall-WindowsFeature** y **Remove-WindowsFeature** , no podrá quitar el rol de AD DS hasta que se degrada el controlador de dominio.
  
### <a name="server-selection"></a>Selección de servidores

![Quitar características Asistente para Roles y seleccione el servidor de destino](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  

El diálogo **Selección de servidor** te permite elegir uno de los servidores agregados anteriormente al grupo, siempre que esté accesible. El servidor local donde se ejecuta el Administrador del servidor está siempre disponible automáticamente.  

### <a name="server-roles-and-features"></a>Características y roles del servidor

![Quitar Roles y características Asistente - seleccione los roles para quitar](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  

Desactiva la casilla **Servicios de dominio de Active Directory** para disminuir un controlador de dominio de nivel; si el servidor es un controlador de dominio actualmente, esto no hace que se elimine el rol de AD DS, sino que se pasa al cuadro de diálogo **Resultados de la validación**, donde se propone la disminución de nivel. De lo contrario, simplemente se quitarán los archivos binarios, como ocurre con cualquier otra característica de rol.  

* No quites ningún otro rol o característica relacionada con AD DS (como DNS, GPMC o las herramientas RSAT) si vas a volver a promover el controlador de dominio. Quitar más roles y características hace que aumente el tiempo que debas invertir en promoverlo, dado que el Administrador del servidor reinstala estas características cuando el rol se reinstala.  
* Quita los roles y características de AD DS que no necesites según tus propios criterios si vas a disminuir el controlador de dominio de nivel permanentemente. Para ello, tienes que desactivar todas las casillas correspondientes a esos roles y características.  

   La lista completa de roles y características relativos a AD DS incluye lo siguiente:  
  
   * La característica Módulo de Active Directory para Windows PowerShell  
   * La característica Herramientas de AD DS y AD LDS  
   * La característica Centro de administración de Active Directory  
   * La característica Complementos y herramientas de línea de comandos de AD DS  
   * Servidor DNS  
   * Consola de administración de directivas de grupo  
  
Los cmdlets equivalentes en Windows PowerShell para ADDSDeployment y ServerManager son:  
  
```
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
```

![Quitar Roles y características Asistente - cuadro de diálogo de confirmación](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  

![Quitar Roles y características Wizard: validación](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  

### <a name="credentials"></a>Credenciales

![Active Directory Domain servicios Asistente para configuración - selección de credenciales](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  

Las opciones de degradación se configuran en la página **Credenciales** . Proporcione las credenciales necesarias para realizar la degradación de la siguiente lista:  

* La degradación de un controlador de dominio adicional requiere credenciales de Administrador de dominio. Seleccionar **forzar la eliminación de este controlador de dominio** degrada el controlador de dominio sin quitar los metadatos del objeto de controlador de dominio de Active Directory.  

   > [!WARNING]  
   > No seleccione esta opción a menos que el controlador de dominio no pueda establecer contacto con otros controladores de dominio y no haya una *forma razonable* para resolver el problema de red. La degradación forzada deja metadatos huérfanos en Active Directory en los controladores de dominio restantes del bosque. Además, todos los cambios no replicados en ese controlador de dominio, como por ejemplo contraseñas o cuentas de usuario nuevas, se pierden para siempre. Los metadatos huérfanos son la causa raíz en un porcentaje significativo de casos del Soporte al cliente de Microsoft para AD DS, Exchange, SQL y otro software.  
   >
   > Si degrada a la fuerza un controlador de dominio, *debe* realizar la limpieza manual de los metadatos en forma inmediata. Para conocer los pasos necesarios, consulta el tema [Limpiar metadatos de servidor](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  

   ![Active Directory Domain Services Asistente para configuración - credenciales forzar la eliminación](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
* La disminución de nivel del último controlador de dominio en un dominio requiere la pertenencia al grupo Administradores de empresas, ya que esto quita el dominio en sí (y, si es el último dominio en el bosque, se quita el bosque). El Administrador del servidor le informa si el controlador de dominio actual es el último controlador de dominio en el dominio. Activa la casilla **Último controlador de dominio en el dominio** para confirmar que el controlador de dominio es el último en el dominio.  

Los argumentos equivalentes en el módulo ADDSDeployment de Windows PowerShell son:  

```
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
```

### <a name="warnings"></a>Warnings

![Asistente para configuración Active Directory Domain Services: impacto de las funciones FSMO de credenciales](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  

En la página **Advertencias** verás alertas de las posibles consecuencias que conlleva eliminar este controlador de dominio. Para proseguir, debes seleccionar **Continuar con la eliminación**.

> [!WARNING]  
> Si activaste antes **Forzar la eliminación de este controlador de dominio** en la página **Credenciales**, la página **Advertencias** mostrará todos los roles de Operaciones de maestro único flexible que este controlador de dominio hospeda. *Debes* asumir los roles de otro controlador de dominio *de inmediato* tras disminuir este servidor de nivel. Para obtener más información sobre cómo asumir roles de FSMO, consulta [Asumir el rol de maestro de operaciones](https://technet.microsoft.com/library/cc816779(WS.10).aspx).

Esta página carece de argumento equivalente en Windows PowerShell para ADDSDeployment.

### <a name="removal-options"></a>Opciones de eliminación

![Active Directory dominio servicios Asistente para configuración - DNS de quitar las credenciales y las particiones de aplicación](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  

La página **Opciones de eliminación** se muestra en función de si antes se ha seleccionado **Último controlador de dominio en el dominio** en la página **Credenciales**. En esta página podrás configurar más opciones de eliminación. Activa las casillas **Quitar esta zona de DNS (el último servidor de DNS que hospeda la zona)**, **Quitar particiones de aplicación**y **Quitar delegación DNS** para mostrar el botón **Siguiente** .

Las opciones solo aparecen si procede en el caso de este controlador de dominio. Por ejemplo, si no hay delegación DNS para este servidor, la casilla correspondiente no se mostrará.

Haz clic en **Cambiar** para especificar otras credenciales administrativas de DNS. Haz clic en **Ver particiones** para ver más particiones que el asistente haya quitado durante la disminución de nivel. Las únicas particiones adicionales son las zonas DNS de dominio y DNS de bosque de forma predeterminada. El resto de particiones no son de Windows.

Los argumentos del cmdlet ADDSDeployment equivalentes son:  

```
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```

### <a name="new-administrator-password"></a>Nueva contraseña de administrador

![Asistente de configuración de Active Directory Domain Services: nueva contraseña de administrador de credenciales](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  

El **nueva contraseña de administrador** página requiere que proporcione una contraseña para la cuenta de administrador del equipo local integrada, una vez que la degradación se complete y el equipo se convierte en un servidor miembro de dominio o equipo de grupo de trabajo.

Si no se especifican, el cmdlet **Uninstall-ADDSDomainController** y sus argumentos correspondientes siguen los mismos valores predeterminados del Administrador del servidor.

El argumento **LocalAdministratorPassword** es especial:

* Si *no se especifica* como argumento, el cmdlet te pedirá que escribas y confirmes una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.
* Si se especifica *con un valor*, debe ser una cadena segura. Este no es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

Por ejemplo, puede solicitar una contraseña manual mediante el uso de la **Read-Host** cmdlet para pedir al usuario una cadena segura.

```
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> Como las dos opciones anteriores no confirman la contraseña, tenga mucho cuidado: la contraseña no está visible.

También puedes proporcionar una cadena segura como una variable no cifrada convertida, pero te recomendamos encarecidamente que no lo hagas. Por ejemplo:

```
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

> [!WARNING]
> No se recomienda proporcionar o almacenar una contraseña no cifrada. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de administrador local de ese equipo y, conociéndola, tendrá acceso a todos los datos y podrá suplantar al servidor.

### <a name="confirmation"></a>Confirmación

![Asistente de configuración de Active Directory Domain Services - opciones de revisión](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)

La página **Confirmación** refleja la disminución de nivel planeada, pero no muestra las opciones de configuración de disminución de nivel. Se trata de la última página del asistente antes de que la disminución de nivel se inicie. Con el botón Ver script se crea un script de disminución de nivel de Windows PowerShell.

Haz clic en **Disminuir nivel** para ejecutar el siguiente cmdlet de implementación de AD DS:

```
Uninstall-DomainController
```

Emplea el argumento opcional **Whatif** con el cmdlet **Uninstall-ADDSDomainController** para repasar la información de configuración. Te permitirá ver los valores explícitos e implícitos de los argumentos de un cmdlet.

Por ejemplo:

![Ejemplo de PowerShell desinstalar-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)

El mensaje para reiniciar es tu última oportunidad de cancelar esta operación cuando uses ADDSDeployment de Windows PowerShell. Para invalidar ese mensaje, usa los argumentos **-force** o **confirm:$false**.  

### <a name="demotion"></a>Degradación

![Active Directory Domain Services Asistente para configuración - degradación en curso](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  

Cuando la página **Degradación** se abre, se inicia la configuración del controlador de dominio, que no se puede interrumpir ni cancelar. En dicha página se muestran las operaciones detalladas, que escriben en los registros:  

* %systemroot%\debug\dcpromo.log
* %systemroot%\debug\dcpromoui.log

Como **Uninstall-AddsDomainController** y **Uninstall-WindowsFeature** solo tienen una acción cada una, se muestran aquí en la fase de confirmación con los argumentos necesarios mínimos. Si presionas ENTRAR, se inicia el proceso de disminución de nivel irrevocable y el equipo se reinicia.

![Ejemplo de PowerShell desinstalar-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)

![Ejemplo de PowerShell desinstalar-WindowsFeature](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)

Para aceptar el aviso de reinicio de forma automática, utiliza los argumentos **-force** o **-confirm:$false** con cualquier cmdlet de ADDSDeployment de Windows PowerShell. Para evitar que el servidor se reinicie automáticamente al final de la promoción, usa el argumento **-norebootoncompletion:$false**.

> [!WARNING]
> No se recomienda invalidar el reinicio. El servidor miembro debe reiniciarse para funcionar correctamente.

![Ejemplo de PowerShell desinstalar-ADDSDomainController Force](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)

Aquí se presenta un ejemplo de degradación forzada con los argumentos mínimos requeridos de **-forceremoval** y **-demoteoperationmasterrole**. El argumento **-credential** no es necesario porque el usuario inició sesión como miembro del grupo Administradores de empresas:

![Ejemplo de PowerShell desinstalar-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)

Aquí te mostramos un ejemplo de eliminación del último controlador de dominio en un dominio con los argumentos mínimos requeridos **-lastdomaincontrollerindomain** y **-removeapplicationpartitions**:

![Ejemplo de PowerShell desinstalar-ADDSDomainController - LastDomainControllerInDomain](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)

Si intenta quitar el rol de AD DS antes de degradar al servidor, Windows PowerShell lo impedirá y mostrará un error:

![Un paso de requisitos previos de desinstalación no se pudo durante la eliminación de servicios de dominio de AD y no puede continuar la desinstalación. 1. El controlador de dominio debe disminuir antes de que se puede desinstalar el rol de servicios DirectoryDomain activo.](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)

> [!IMPORTANT]
> Para poder eliminar los archivos binarios del rol AD-Domain-Services, debes reiniciar el equipo después de disminuir el servidor de nivel.

### <a name="results"></a>Results

![Está a punto se aprobó la advertencia después de la eliminación de AD DS](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)

La página **Resultados** indica si la promoción se realizó correctamente o si se produjo algún error, junto con toda la información administrativa importante que corresponda. El controlador de dominio se reiniciará automáticamente 10 segundos después.
