---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: Instalar un controlador de dominio sólo servidor 2012 Active Directory lectura (RODC) (nivel 200) de Windows
description: En este tema se explica cómo crear una cuenta de RODC preconfigurada y, después, cómo conectar un servidor a esa cuenta durante la instalación del RODC. En este tema también se explica cómo instalar un RODC sin una instalación preconfigurada.
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: eef460ae26195982ac9387013de03dd2882b2e54
ms.sourcegitcommit: 2ede79efbadd109099bb6fdb744796adde123922
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98923706"
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Instalar un controlador de dominio sólo servidor 2012 Active Directory lectura (RODC) (nivel 200) de Windows

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explica cómo crear una cuenta de RODC preconfigurada y, después, cómo conectar un servidor a esa cuenta durante la instalación del RODC. En este tema también se explica cómo instalar un RODC sin una instalación preconfigurada.

## <a name="stage-rodc-workflow"></a>Flujo de trabajo de preconfiguración del RODC
Una instalación de controlador de dominio preconfigurado de solo lectura (RODC) se realiza en dos fases distintas:

1.  Preconfigurar una cuenta de equipo no ocupada

2.  Conectar un RODC a esa cuenta durante la promoción

El diagrama siguiente ilustra el proceso de preconfiguración del controlador de dominio de solo lectura de los Servicios de dominio de Active Directory, en el que se crea una cuenta de equipo de RODC vacía en el dominio usando el Centro de administración de Active Directory (Dsac.exe).

![Diagrama que muestra el Active Directory Domain Services Read-Only proceso de almacenamiento provisional del controlador de dominio descrito anteriormente.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)

## <a name="stage-rodc-windows-powershell"></a><a name=BKMK_StagePS></a>Preconfigurar un RODC en Windows PowerShell

| Cmdlet de ADDSDeployment | Argumentos (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS). |
|--|--|
| Add-addsreadonlydomaincontrolleraccount | -SkipPreChecks<p>***-DomainControllerAccountName * *_<p>_* _-nombreDeDominio_ *_<p>_* _-siteName_*_<p>_ -AllowPasswordReplicationAccountName* <p>***-Credential **_<p>_ -DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>*-NoGlobalCatalog*<p>*-InstallDNS*<p>-ReplicationSourceDC |

> [!NOTE]
> El argumento **-credential** solamente es necesario cuando no has iniciado sesión como miembro del grupo Admins. del dominio.

## <a name="attach-rodc-workflow"></a>Flujo de trabajo de conexión del RODC
El diagrama siguiente ilustra el proceso de configuración de Active Directory Domain Services, en el que ya has instalado el rol AD DS, has preconfigurado la cuenta de RODC y has iniciado **Promover este servidor a controlador de dominio** usando el Administrador del servidor para crear un nuevo RODC en un dominio existente y conectarlo a la cuenta de equipo preconfigurada.

![Diagrama que muestra el proceso de configuración de Active Directory Domain Services descrito anteriormente.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)

## <a name="attach-rodc-windows-powershell"></a><a name=BKMK_AttachPS></a>Asociar un RODC en Windows PowerShell

| Cmdlet de ADDSDeployment | Arguments (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS). |
|--|--|
| Install-AddsDomaincontroller | -SkipPreChecks<p>***-DomainName **_<p>_ -SafeModeAdministratorPassword* <p> *-ApplicationPartitionsToReplicate* <p> *-CreateDNSDelegation* <p>***-Credential **_<p> -CriticalReplicationOnly <p>_-DatabasePath *<p>* -DNSDelegationCredential *<p>* -InstallationMediaPath *<p>* -LogPath *<p> -Norebootoncompletion <p>*-ReplicationSourceDC *<p>* -SystemKey *<p>* -SYSVOLPath * <p> * * *-UseExistingAccount** _ |

> [!NOTE]
> El argumento _ *-Credential** solo es necesario si aún no ha iniciado sesión como miembro del grupo Admins. del dominio.

## <a name="staging"></a>Ensayo
![Captura de pantalla del Centro de administración de Active Directory en el que se muestra la opción crear previamente una cuenta de controlador de dominio de solo lectura resaltada en el Panel de tareas.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)

Para realizar la operación de preconfiguración de una cuenta de equipo de controlador de dominio de solo lectura, abre el Centro de administración de Active Directory (**Dsac.exe**). Haz clic en el nombre del dominio en el panel de navegación. Haz doble clic en **Controladores de dominio** en la lista de administración. Haz clic en **Crear previamente una cuenta de controlador de dominio de solo lectura** en el panel de tareas.

Para obtener más información sobre el Centro de administración de Active Directory, consulte [Administración avanzada de AD DS con Centro de administración de Active Directory &#40;de nivel 200&#41;](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) y revisión [centro de administración de Active Directory: Introducción](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560651(v=ws.10)).

Si ya tienes experiencia en la creación de controladores de dominio de solo lectura, descubrirás que el asistente para instalación tiene la misma interfaz gráfica que se ve cuando se usa el antiguo complemento Usuarios y equipos de Active Directory de Windows Server 2008, y usa el mismo código, que incluye exportar la configuración en el formato de archivo desatendido que usa el obsoleto dcpromo.

Windows Server 2012 incorpora un nuevo cmdlet de ADDSDeployment para preconfigurar cuentas de equipo de RODC, pero el asistente no usa el cmdlet para esta operación. Las secciones siguientes muestran el cmdlet y los argumentos equivalentes para que la información asociada a cada uno sea más fácil de comprender.

El vínculo **crear previamente una cuenta de controlador de dominio de solo lectura** en el panel de tareas del centro de administración de Active Directory es equivalente al cmdlet de Windows PowerShell ADDSDeployment:

```
Add-addsreadonlydomaincontrolleraccount

```

### <a name="welcome"></a>Pantalla de inicio
![Captura de pantalla de la Página principal del Asistente para la instalación de Azure Directory Domain Services que muestra la opción usar la instalación en modo avanzado seleccionada.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)

El cuadro de diálogo **Asistente para la instalación de Active Directory Domain Services** tiene una opción llamada **Usar la instalación en modo avanzado**. Activa esta opción y haz clic en **Siguiente** para mostrar las opciones de directiva de replicación de contraseñas. Desactiva esta opción para usar los valores predeterminados de las opciones de directiva de replicación de contraseñas (este punto se trata más adelante en esta sección).

### <a name="network-credentials"></a>Credenciales de red
![Captura de pantalla de la página credenciales de red del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)

En el cuadro de diálogo **Credenciales de red**, la opción de nombre de dominio muestra el dominio de destino predeterminado para el Centro de administración de Active Directory. De forma predeterminada se usan las credenciales actuales. Si no incluyen la pertenencia al grupo Admins. del dominio, haz clic en **Credenciales alternativas** y en **Establecer** para proporcionar al asistente un nombre de usuario y una contraseña que sea miembro de Admins. del dominio.

El argumento equivalente en el módulo ADDSDeployment de Windows PowerShell es:

```
-credential <pscredential>
```

Recuerda que el sistema de preconfiguración es un puerto directo desde Windows Server 2008 R2 y no proporciona la nueva funcionalidad Adprep. Si tienes previsto implementar cuentas de RODC preconfiguradas, primero tienes que implementar un RODC sin preconfigurar en ese dominio para que la operación rodcprep automática funcione, o ejecutar primero manualmente adprep.exe /rodcprep.

De lo contrario, recibirá el mensaje de error no podrá instalar un controlador de dominio de solo lectura en este dominio porque todavía no se ha ejecutado Adprep/rodcprep.

![Captura de pantalla del mensaje de advertencia del Asistente para la instalación de Azure Directory Domain Services que indica que Adprep/rodcprep no se ha ejecutado todavía.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)

### <a name="specify-the-computer-name"></a>Especifique el nombre del equipo
![Captura de pantalla de la página especificar el nombre de equipo del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)

En el cuadro de diálogo **Especifique el nombre del equipo** tienes que especificar el **Nombre de equipo**, con una sola etiqueta, de un controlador de dominio que no existe. El controlador de dominio que configurarás y asociarás más adelante a esta cuenta tendrá el mismo nombre, o la operación de promoción no detectará la cuenta preconfigurada.

El argumento equivalente en el módulo ADDSDeployment de Windows PowerShell es:

```
-domaincontrolleraccountname <string>
```

### <a name="select-a-site"></a>Seleccione un sitio
![Captura de pantalla de la página seleccionar un sitio del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)

El cuadro de diálogo **Seleccione un sitio** muestra una lista de los sitios de Active Directory para el bosque actual. Para que el controlador de dominio preconfigurado de solo lectura funcione, tienes que seleccionar un único sitio en la lista. El RODC usa esta información para crear su objeto de configuración NTDS en la partición Configuración, y para unirse al sitio correcto cuando se inicie por primera vez después de la implementación.

El argumento equivalente en el módulo ADDSDeployment de Windows PowerShell es:

```
-sitename <string>
```

### <a name="additional-domain-controller-options"></a>Opciones adicionales del controlador de dominio
![Captura de pantalla de la página especificar las opciones del controlador de dominio del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)

El cuadro de diálogo **Opciones adicionales del controlador de dominio** permite especificar que un controlador de dominio incluya la ejecución como **Servidor DNS** y **Catálogo global**. Microsoft recomienda que los controladores de dominio de solo lectura proporcionen servicios de DNS y GC, por lo que ambos se instalan de forma predeterminada; uno de los objetivos del rol RODC es los escenarios de sucursales, en los que puede que la red de área extensa no esté disponible y, sin esos servicios de DNS y catálogo global, los equipos de la sucursal no podrán usar los recursos y la funcionalidad de AD DS.

La opción **Controlador de dominio de solo lectura (RODC)** está preseleccionada y no se puede deshabilitar. Los argumentos equivalentes en el módulo ADDSDeployment de Windows PowerShell son:

```
-installdns <string>
-NoGlobalCatalog <{$true | $false}>

```

> [!NOTE]
> De forma predeterminada, el valor **-NoGlobalCatalog** es $false, lo que significa que el controlador de dominio será un servidor de catálogo global si no se especifica el argumento.

### <a name="specify-the-password-replication-policy"></a>Especificar la directiva de replicación de contraseñas
![Captura de pantalla de la página especificar la Directiva de replicación de contraseñas del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)

El cuadro de diálogo **Especificar la directiva de replicación de contraseñas** permite modificar la lista predeterminada de cuentas que tienen permisos para almacenar en caché sus contraseñas en este controlador de dominio de solo lectura. Las cuentas de la lista configuradas como **Denegar** o las cuentas que no están en la lista (implícitas) no almacenan en caché sus contraseñas. Las cuentas que no tienen permiso para almacenar en caché las contraseñas en el RODC y que no pueden conectarse ni autenticarse en un controlador de dominio de escritura, no pueden acceder a los recursos ni la funcionalidad de Active Directory.

> [!IMPORTANT]
> El asistente muestra este cuadro de diálogo solo si activas la casilla **Usar la instalación en modo avanzado** en la pantalla inicial. Si desactivas esta casilla, el asistente usará los siguientes grupos y valores predeterminados:
>
> -   Administradores: Denegar
> -   Operadores de servidor: Denegar
> -   Operadores de copia de seguridad: Denegar
> -   Operadores de cuenta: Denegar
> -   Grupo de replicación de contraseña RODC denegada: Denegar
> -   Grupo de replicación de contraseña RODC permitida: Denegar

Los argumentos equivalentes en el módulo ADDSDeployment de Windows PowerShell son:

```
-allowpasswordreplicationaccountname <string []>
-denypasswordreplicationaccountname <string []>
```

![Captura de pantalla del cuadro de diálogo agregar grupos, usuarios y equipos.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)

### <a name="delegation-of-rodc-installation-and-administration"></a>Delegación de instalación y administración de RODC
![Captura de pantalla de la página delegación de instalación y administración de RODC del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)

El cuadro de diálogo **Delegación de instalación y administración de RODC** permite configurar un usuario o un grupo de usuarios con permisos para asociar el servidor a la cuenta de equipo de RODC. Haz clic en **Establecer** para examinar el dominio de un usuario o grupo. El usuario o grupo especificado en este cuadro de diálogo obtiene permisos administrativos locales en el RODC. El usuario o los miembros especificados del grupo especificado pueden realizar operaciones en el RODC con privilegios equivalentes al grupo administradores del equipo. *No* son miembros del grupo Admins. del dominio ni del grupo Administradores integrado del dominio.

Usa esta opción para delegar la administración de sucursales sin conceder al administrador de la sucursal la pertenencia al grupo Admins. del dominio. No es necesario delegar la administración de RODC.

El argumento equivalente en el módulo ADDSDeployment de Windows PowerShell es:

```
-delegatedadministratoraccountname <string>
```

### <a name="summary"></a>Resumen
![Captura de pantalla de la página Resumen del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)

El cuadro de diálogo **Resumen** te permite confirmar la configuración. Esta es la última oportunidad de detener la instalación antes de que el asistente cree la cuenta preconfigurada. Haz clic en **Siguiente** cuando estés listo para crear la cuenta de equipo de RODC preconfigurada.  Haz clic en **Exportar configuración** para guardar un archivo de respuesta con el obsoleto formato de archivo desatendido dcpromo.

### <a name="creation"></a>Creación
![Captura de pantalla de la página progreso del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)

El **Asistente para instalación de Active Directory Domain Services** crea el controlador de dominio preconfigurado de solo lectura en Active Directory. Esta operación no se puede cancelar una vez iniciada.

![Captura de pantalla de la última página del Asistente para la instalación de Azure Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)

Usa el siguiente cmdlet para preconfigurar una cuenta de equipo de controlador de dominio de solo lectura usando el módulo ADDSDeployment de Windows PowerShell:

```
Add-addsreadonlydomaincontrolleraccount

```

En [Preconfigurar un RODC en Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS) encontrarás los argumentos necesarios y opcionales.

Dado que **Add-addsreadonlydomaincontrolleraccount** solo tiene una acción con dos fases (comprobación de requisitos previos e instalación), las siguientes capturas de pantallas muestran la fase de instalación con los argumentos mínimos necesarios.

![Captura de pantalla de la ventana de PowerShell que muestra el cmdlet Add-addsreadonlydomaincontrolleraccount completo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)

![Captura de pantalla de la ventana de PowerShell que muestra el resultado del cmdlet Add-addsreadonlydomaincontrolleraccount.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)

La operación de preconfiguración de RODC crea la cuenta de equipo de RODC en Active Directory. El Centro de administración de Active Directory muestra el **Tipo de controlador de dominio** como una **Cuenta de controlador de dominio no ocupada**. Este tipo de controlador de dominio indica que la cuenta de RODC preconfigurada está lista para que un servidor se asocie a ella como controlador de dominio de solo lectura.

![Captura de pantalla del Centro de administración de Active Directory que muestra la cuenta de controlador de dominio no ocupada resaltada.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)

> [!IMPORTANT]
> El Centro de administración de Active Directory ya no necesita asociar un servidor a una cuenta de equipo de controlador de dominio de solo lectura. Usa el Administrador del servidor y el Asistente para configuración de Active Directory Domain Services o el cmdlet **Install-AddsDomainController** del módulo ADDSDeployment de Windows PowerShell para asociar un nuevo RODC a su cuenta preconfigurada. Los pasos son similares a agregar un nuevo controlador de dominio de escritura a un dominio existente, excepto que las cuentas de equipo de RODC preconfiguradas contienen opciones de configuración que se deciden en el momento de preconfigurar la cuenta de equipo de RODC.

## <a name="attaching"></a>asociación

### <a name="deployment-configuration"></a>Configuración de la implementación
![Captura de pantalla de la página Configuración de implementación del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)

El Administrador del servidor comienza la promoción de cada controlador de dominio con la página **Configuración de implementación**. Las opciones restantes y los campos requeridos cambian en esta página y en las páginas siguientes, según qué operación de implementación se seleccione.

Para agregar un controlador de dominio de solo lectura a un dominio existente, selecciona **Agregar un controlador de dominio a un dominio existente** y haz clic en el botón **Seleccionar** para **Especificar la información de dominio para esta operación**. El Administrador del servidor pide automáticamente unas credenciales válidas, o puedes hacer clic en **Cambiar**.

Para asociar un RODC es necesario pertenecer al grupo Admins. del dominio en Windows Server 2012. El Asistente para configuración de Servicios de dominio de Active Directory le pedirá confirmación si las credenciales actuales no tienen los permisos o la pertenencia a grupos adecuados.

El cmdlet y los argumentos de **Configuración de implementación** en el módulo ADDSDeployment de Windows PowerShell son:

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

### <a name="domain-controller-options"></a>Opciones del controlador de dominio
![Captura de pantalla de la página Opciones del controlador de dominio del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)

La página **Opciones del controlador de dominio** muestra las opciones del nuevo controlador de dominio. Cuando esta página se carga, el Asistente para configuración de Active Directory Domain Services envía una consulta LDAP a un controlador de dominio existente para comprobar las cuentas no ocupadas. Si la consulta encuentra una cuenta de equipo de controlador de dominio no ocupada que comparte el mismo nombre que el equipo actual, el asistente muestra un mensaje informativo en la parte superior de la página que lee **una cuenta RODC creada previamente que coincide con el nombre del servidor de destino en el directorio. Elija si desea usar esta cuenta de RODC existente o reinstalar este controlador de dominio**. El asistente usa la opción **Usar cuenta RODC existente** como configuración predeterminada.

> [!IMPORTANT]
> Puedes usar la opción **Volver a instalar el controlador de dominio** si el controlador de dominio ha sufrido un problema físico y no puede volver a funcionar correctamente. Dejar la cuenta de equipo de dominio y los metadatos del objeto en Active Directory permite ahorrar tiempo a la hora de configurar el controlador de dominio de sustitución. Instala el nuevo equipo con el *mismo nombre* y promuévelo a controlador de dominio en el dominio. La opción **reinstalar este controlador de dominio** no está disponible si quitó los metadatos del objeto de controlador de dominio de Active Directory (limpieza de metadatos).

No puedes configurar las opciones de controlador de dominio si estás conectando un servidor a una cuenta de equipo de RODC. Puedes configurar las opciones de controlador de dominio al crear la cuenta de equipo de RODC preconfigurada.

La **Contraseña del modo de restauración de servicios de directorio** debe cumplir la directiva de contraseñas aplicada al servidor. Siempre elija una contraseña segura y compleja o, preferentemente, una frase de contraseña.

Los argumentos correspondientes a las **Opciones del controlador de dominio** en el módulo ADDSDeployment de Windows PowerShell son:

```
-UseExistingAccount <{$true | $false}>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> El nombre del sitio ya debe existir cuando se proporciona como un argumento para **-sitename**. El cmdlet **install-AddsDomainController** no crea nombres de sitios. Puedes usar el cmdlet **new-adreplicationsite** para crear nuevos sitios.

Si no se especifican, los argumentos de **Install-ADDSDomainController** tienen los mismos valores predeterminados que el Administrador del servidor.

La operación del argumento **SafeModeAdministratorPassword** es especial:

-   Si *no se especifica* como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

    Por ejemplo, para crear un nuevo RODC en corp.contoso.com y que te pida que escribas y confirmes una contraseña enmascarada:

    ```
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)
    ```

-   Si se especifica *con un valor*, este debe ser una cadena segura. Este no es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

Por ejemplo, puedes solicitar una contraseña de forma manual con el cmdlet **Read-Host** para pedirle al usuario que escriba una cadena segura:

```
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)

```

> [!WARNING]
> Como la opción anterior no confirma la contraseña, tenga mucho cuidado: la contraseña no es visible.

También puedes proporcionar una cadena segura como una variable no cifrada convertida, pero te recomendamos encarecidamente que no lo hagas.

```
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)
```

Por último, puedes almacenar la contraseña cifrada en un archivo y reutilizarla más adelante, sin que la contraseña aparezca descifrada en ningún momento. Por ejemplo:

```
$file = c:\pw.txt
$pw = read-host -prompt Password: -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> No se recomienda proporcionar ni almacenar contraseñas no cifradas. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo podría invertir la contraseña cifrada. Con esa información, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y suplantar al propio controlador de dominio, elevando sus privilegios al máximo nivel en el bosque de AD. Te recomendamos que uses un conjunto adicional de pasos con **System.Security.Cryptography** para cifrar los datos del archivo de texto, pero ese tema no se trata aquí. El procedimiento recomendado es no almacenar la contraseña en absoluto.

### <a name="additional-options"></a>Opciones adicionales
![Captura de pantalla de la Página opciones adicionales del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)

La página **Opciones adicionales** proporciona opciones de configuración para asignar un nombre a un controlador de dominio como origen de replicación, o puedes usar cualquier controlador de dominio como origen de replicación.

También puede eligir instalar el controlador de dominio con medios con copia de seguridad mediante la opción Instalar desde medios (IFM). Una vez activada, la casilla **Instalar desde el medio** proporciona una opción de exploración y debes hacer clic en **Comprobar** para asegurarte de que la ruta proporcionada es un medio válido.

Instrucciones para el origen de IFM:
*    Los medios usados por la opción IFM se crean con Copias de seguridad de Windows Server o Ntdsutil.exe de otro controlador de dominio de Windows Server existente con la misma versión del sistema operativo únicamente. Por ejemplo, no puede usar un sistema operativo Windows Server 2008 R2 o anterior para crear medios para un controlador de dominio de Windows Server 2012.
*    Los datos de origen de IFM deben ser de un controlador de dominio de escritura. Mientras que un origen de RODC funciona técnicamente para crear un nuevo RODC, hay falsas advertencias de replicación positivas que indican que el RODC de origen IFM no se está replicando.

Para obtener más información sobre los cambios en IFM, consulta [Cambios en Instalar desde el medio de Ntdsutil.exe](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Si usas medios protegidos con SYSKEY, el Administrador del servidor pregunta la contraseña de la imagen durante la comprobación.

![Captura de pantalla de la ventana del símbolo del sistema que muestra los resultados de la ejecución de Ntdsutil.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)

Los argumentos del cmdlet ADDSDeployment correspondientes a las **Opciones adicionales** son:

```
-replicationsourcedc <string>
-installationmediapath <string>
-systemkey <secure string>
```

### <a name="paths"></a>Rutas de acceso
![Captura de pantalla de la página rutas de acceso del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)

La página **Rutas de acceso** permite reemplazar las ubicaciones de carpeta predeterminadas de la base de datos AD DS, los registros de transacciones de la base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas siempre están en subdirectorios de %systemroot%. Los argumentos del cmdlet ADDSDeployment correspondientes a **Rutas de acceso** son:

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="review-options-and-view-script"></a>Revisión de opciones y visualización del script
![Captura de pantalla de la página Opciones de revisión del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)

La página **Revisar opciones** permite validar la configuración y asegura que esta cumpla con los requisitos antes de comenzar con la instalación. Esta no es la última oportunidad para detener la instalación con el Administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con esta. La página **Revisar opciones** del Administrador del servidor también ofrece un botón **Ver script** opcional para crear un archivo de texto Unicode que contiene la configuración ADDSDeployment actual como un script Windows PowerShell único. Esto le permite usar la interfaz gráfica del Administrador del servidor como un estudio de implementación de Windows PowerShell. Use el Asistente para configuración de Servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, cancele el asistente. Este proceso crea un ejemplo válido y sintácticamente correcto para realizar más modificaciones o la utilización directa. Por ejemplo:

```
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSDomainController `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath C:\Windows\NTDS `
-DomainName corp.contoso.com `
-LogPath C:\Windows\NTDS `
-SYSVOLPath C:\Windows\SYSVOL `
-UseExistingAccount:$true `
-Norebootoncompletion:$false
-Force:$true

```

> [!NOTE]
> Normalmente, el Administrador del servidor rellena todos los argumentos con valores al realizar la promoción y no se basa en los valores predeterminados (dado que pueden cambiar entre las futuras versiones de Windows o los Service Pack). La excepción es el argumento **-safemodeadministratorpassword**. Para aplicar una pregunta de confirmación, pasa por alto el valor al ejecutar el cmdlet interactivamente.

Use el argumento opcional **Whatif** con el cmdlet **Install-ADDSDomainController** para revisar la información de la configuración. Esto te permite ver los valores explícitos e implícitos de los argumentos de un cmdlet.

![Captura de pantalla de la ventana de PowerShell que muestra los resultados del cmdlet Install-ADDSDomainController.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)

### <a name="prerequisites-check"></a>Prerequisites Check
![Captura de pantalla de la página de comprobación de requisitos previos del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)

La **Comprobación de requisitos previos** es una característica nueva en la configuración de dominios de AD DS. Esta nueva fase valida la capacidad de la configuración del servidor para admitir un nuevo bosque de AD DS.

Al instalar un nuevo dominio raíz de bosque, el Asistente para configuración de Servicios de dominio de Active Directory del Administrador del servidor invoca una serie de pruebas modulares en serie. Las pruebas te envían alertas con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso de instalación del controlador de dominio no puede continuar hasta que todas las comprobaciones de los requisitos previos sean correctas.

La **Comprobación de requisitos previos** también expone información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores. Para obtener más información sobre las comprobaciones de requisitos previos, consulta [Comprobación de requisitos previos](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).

No puedes omitir la **Comprobación de requisitos previos** al usar el Administrador del servidor, pero sí al utilizar el cmdlet de implementación de AD DS con el siguiente argumento:

```
-skipprechecks

```

> [!WARNING]
> Microsoft no recomienda omitir la comprobación de requisitos previos: omitirla puede hacer que los controladores de dominio se promuevan parcialmente o puede provocar daños en el bosque de AD DS.

Haz clic en **Instalar** para comenzar el proceso de promoción del controlador de dominio. Esta es la última oportunidad para cancelar la instalación. Una vez iniciado, el proceso de promoción no se puede cancelar. El equipo se reiniciará automáticamente al finalizar la promoción, independientemente del resultado de la misma.

### <a name="installation"></a>Instalación
![Captura de pantalla de la página instalación del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)

Cuando se muestra la página Instalación, la configuración del controlador de dominio comienza y no se puede detener ni cancelar. Las operaciones detalladas se muestran en esta página y se escriben en los registros:

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

Para instalar un nuevo bosque de Active Directory con el módulo ADDSDeployment, utiliza este cmdlet:

```
Install-addsdomaincontroller

```

En [Asociar un RODC en Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) encontrarás los argumentos necesarios y opcionales.

El cmdlet **Install-addsdomaincontroller** solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con los argumentos mínimos necesarios **-domainname**, **-useexistingaccount** y **-credential**. Observa cómo, al igual que el Administrador del servidor, **Install-ADDSDomainController** te recuerda que la promoción reiniciará el servidor automáticamente:

![Captura de pantalla de la ventana de PowerShell que muestra el resultado del cmdlet install-addsdomaincontroller.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)

![Captura de pantalla de la ventana de PowerShell que muestra el progreso de la validación y la instalación.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)

Para aceptar el aviso de reinicio de forma automática, utiliza los argumentos **-force** o **-confirm:$false** con cualquier cmdlet de ADDSDeployment de Windows PowerShell. Para impedir que el servidor se reinicie automáticamente al finalizar la promoción, utiliza el argumento **-norebootoncompletion**.

> [!WARNING]
> No se recomienda invalidar el reinicio. El controlador de dominio debe reiniciarse para funcionar correctamente.

### <a name="results"></a>Results
![Captura de pantalla de la página resultados del Asistente para configuración de Active Directory Domain Services.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)

La página **Resultados** indica si la promoción se realizó correctamente o si se produjo algún error, junto con toda la información administrativa importante que corresponda. El controlador de dominio se reiniciará automáticamente 10 segundos después.

## <a name="rodc-without-staging-workflow"></a>Flujo de trabajo de un RODC no preconfigurado
El diagrama siguiente ilustra el proceso de configuración de los Servicios de dominio de Active Directory, cuando ya se ha instalado el rol AD DS y se ha iniciado el Asistente para configuración de Servicios de dominio de Active Directory usando el Administrador del servidor para crear un nuevo controlador de dominio de solo lectura no preconfigurado en un dominio de Windows Server 2012 existente.

![Diagrama que muestra el Active Directory Domain Services Read-Only proceso de controlador de dominio, como se ha descrito anteriormente, sin el flujo de trabajo de almacenamiento provisional.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)

## <a name="rodc-without-staging-windows-powershell"></a>RODC no preconfigurado en Windows PowerShell

| Cmdlet de ADDSDeployment | Arguments (los argumentos en **negrita** son obligatorios. Los argumentos en *cursiva* se pueden especificar con Windows PowerShell o con el Asistente para configuración de AD DS). |
|--|--|
| Install-AddsDomainController | -SkipPreChecks<p>***- <p> Domainname **_<p>_ -SafeModeAdministratorPassword****-siteName **_<p>_ -ApplicationPartitionsToReplicate *<p>* -CreateDNSDelegation * * * <p> *-Credential** _<p>_ -CriticalReplicationOnly *<p>* -DatabasePath *<p>* -DNSDelegationCredential *<p> -DNSOnNetwork <p>*-InstallationMediaPath *<p>* -InstallDNS *<p>* -LogPath *<p> -MoveInfrastructureOperationMasterRoleIfNecessary <p>*-NoGlobalCatalog *<p> -Norebootoncompletion <p>*-ReplicationSourceDC *<p> -SkipAutoConfigureDNS <p>*-SystemKey *<p>* -SYSVOLPath *<p>* -AllowPasswordReplicationAccountName *<p>* -DelegatedAdministratorAccountName *<p>* -DenyPasswordReplicationAccountName *<p>***-ReadOnlyReplica** _ |

> [!NOTE]
> El argumento _ *-Credential** solo es necesario si aún no ha iniciado sesión como miembro del grupo Admins. del dominio.

## <a name="rodc-without-staging-deployment"></a>Implementación de un RODC no preconfigurado

### <a name="deployment-configuration"></a>Configuración de la implementación
![Captura de pantalla de la página Configuración de implementación del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)

El Administrador del servidor comienza la promoción de cada controlador de dominio con la página **Configuración de implementación**. Las opciones restantes y los campos requeridos cambian en esta página y en las páginas siguientes, según qué operación de implementación se seleccione.

Para agregar un controlador de dominio de solo lectura no preconfigurado a un dominio de Windows Server 2012 existente, selecciona **Agregar un controlador de dominio a un dominio existente** y haz clic en el botón **Seleccionar** para **Especificar la información de dominio para esta operación**. El Administrador del servidor pide automáticamente unas credenciales válidas, o puedes hacer clic en **Cambiar**.

Para asociar un RODC es necesario pertenecer al grupo Admins. del dominio en Windows Server 2012. El Asistente para configuración de Servicios de dominio de Active Directory le pedirá confirmación si las credenciales actuales no tienen los permisos o la pertenencia a grupos adecuados.

El cmdlet y los argumentos de **Configuración de implementación** en el módulo ADDSDeployment de Windows PowerShell son:

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

### <a name="domain-controller-options"></a>Opciones del controlador de dominio
![Captura de pantalla de la página Opciones del controlador de dominio del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)

La página **Opciones del controlador de dominio** especifica las funcionalidades del nuevo controlador de dominio. Las funcionalidades del controlador de dominio que se pueden configurar son **Servidor DNS**, **Catálogo global** y **Controlador de dominio de solo lectura**. Microsoft recomienda que todos los controladores de dominio proporcionen servicios de DNS y GC para alta disponibilidad en entornos distribuidos. GC siempre está seleccionado de forma predeterminada y el servidor DNS está seleccionado de forma predeterminada si el dominio actual ya hospeda un DNS en sus controladores de dominio según la consulta de Inicio de autoridad.

La página **Opciones del controlador de dominio** también permite elegir el **nombre de sitio** lógico apropiado de Active Directory de la configuración del bosque. De forma predeterminada, selecciona el sitio con la subred más correcta. Si solo hay un sitio, lo selecciona automáticamente.

> [!IMPORTANT]
> Si el servidor no pertenece a una subred de Active Directory y hay más de un sitio de Active Directory, no se selecciona nada y el botón **Siguiente** no estará disponible hasta que elijas un sitio de la lista.

La **Contraseña del modo de restauración de servicios de directorio** debe cumplir la directiva de contraseñas aplicada al servidor. Elige siempre una contraseña segura y compleja o, preferentemente, una frase de contraseña. Los argumentos correspondientes a las **Opciones del controlador de dominio** en el módulo ADDSDeployment de Windows PowerShell son:

```
-UseExistingAccount <{$true | $false}>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> El nombre del sitio ya debe existir cuando se proporciona como un argumento para **-sitename**. El cmdlet **install-AddsDomainController** no crea nombres de sitios. Puedes usar el cmdlet **new-adreplicationsite** para crear nuevos sitios.

Si no se especifican, los argumentos de **Install-ADDSDomainController** tienen los mismos valores predeterminados que el Administrador del servidor.

La operación del argumento **SafeModeAdministratorPassword** es especial:

-   Si *no se especifica* como un argumento, el cmdlet le pide que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

    Por ejemplo, para crear un nuevo RODC en corp.contoso.com y que te pida que escribas y confirmes una contraseña enmascarada:

    ```
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)
    ```

-   Si se especifica *con un valor*, este debe ser una cadena segura. Este no es el uso preferido cuando se ejecuta el cmdlet en forma interactiva.

Por ejemplo, puedes solicitar una contraseña de forma manual con el cmdlet **Read-Host** para pedirle al usuario que escriba una cadena segura:

```
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)

```

> [!WARNING]
> Como la opción anterior no confirma la contraseña, tenga mucho cuidado: la contraseña no es visible.

También puedes proporcionar una cadena segura como una variable no cifrada convertida, pero te recomendamos encarecidamente que no lo hagas.

```
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)
```

Por último, puedes almacenar la contraseña cifrada en un archivo y reutilizarla más adelante, sin que la contraseña aparezca descifrada en ningún momento. Por ejemplo:

```
$file = c:\pw.txt
$pw = read-host -prompt Password: -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> No se recomienda proporcionar ni almacenar contraseñas no cifradas. Cualquier persona que ejecute este comando en un script o que mire sobre su hombro sabrá la contraseña de DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo podría invertir la contraseña cifrada. Con esa información, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y suplantar al propio controlador de dominio, elevando sus privilegios al máximo nivel en el bosque de AD. Te recomendamos que uses un conjunto adicional de pasos con **System.Security.Cryptography** para cifrar los datos del archivo de texto, pero ese tema no se trata aquí. El procedimiento recomendado es no almacenar la contraseña en absoluto.

### <a name="rodc-options"></a>RODC Options
![Captura de pantalla de la página de opciones de RODC del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)

La página **Opciones de RODC** te permiten modificar las opciones de configuración:

-   Cuenta de administrador delegada

-   Cuentas con permiso para replicar contraseñas en el RODC

-   Cuentas sin permiso para replicar contraseñas en el RODC

Las cuentas de administrador delegadas obtienen permisos administrativos locales para RODC. Estos usuarios pueden trabajar con privilegios equivalentes al grupo de administradores del equipo local.  No son miembros de los grupos Admins del dominio o de las cuentas predefinidas de administrador del dominio. Esta opción es útil para delegar la administración de la sucursal sin dar permisos administrativos de dominio. No es necesario configurar la delegación de la administración.

El argumento equivalente en el módulo ADDSDeployment de Windows PowerShell es:

```
-delegatedadministratoraccountname <string>
```

Las cuentas que no tienen permiso para almacenar en caché las contraseñas en el RODC y que no pueden conectarse ni autenticarse en un controlador de dominio de escritura, no pueden acceder a los recursos ni la funcionalidad de Active Directory.

> [!IMPORTANT]
> Si no se modifican, se usan los grupos y opciones de configuración predeterminados:
>
> -   Administradores: Denegar
> -   Operadores de servidor: Denegar
> -   Operadores de copia de seguridad: Denegar
> -   Operadores de cuenta: Denegar
> -   Grupo de replicación de contraseña RODC denegada: Denegar
> -   Grupo de replicación de contraseña RODC permitida: Denegar

Los argumentos equivalentes en el módulo ADDSDeployment de Windows PowerShell son:

```
-allowpasswordreplicationaccountname <string []>
-denypasswordreplicationaccountname <string []>
```

![Captura de pantalla del cuadro de diálogo Seleccionar usuario, equipo, cuenta de servicio.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)

### <a name="additional-options"></a>Opciones adicionales
![Captura de pantalla de la Página opciones adicionales del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)

La página **Opciones adicionales** proporciona opciones de configuración para asignar un nombre a un controlador de dominio como origen de replicación, o puedes usar cualquier controlador de dominio como origen de replicación.

También puede eligir instalar el controlador de dominio con medios con copia de seguridad mediante la opción Instalar desde medios (IFM). Una vez activada, la casilla **Instalar desde el medio** proporciona una opción de exploración y debes hacer clic en **Comprobar** para asegurarte de que la ruta proporcionada es un medio válido.

Instrucciones para el origen de IFM:
*    Los medios usados por la opción IFM se crean con Copias de seguridad de Windows Server o Ntdsutil.exe de otro controlador de dominio de Windows Server existente con la misma versión del sistema operativo únicamente. Por ejemplo, no puede usar un sistema operativo Windows Server 2008 R2 o anterior para crear medios para un controlador de dominio de Windows Server 2012.
*    Los datos de origen de IFM deben ser de un controlador de dominio de escritura. Mientras que un origen de RODC funciona técnicamente para crear un nuevo RODC, hay falsas advertencias de replicación positivas que indican que el RODC de origen IFM no se está replicando.

Para obtener más información sobre los cambios en IFM, consulta [Cambios en Instalar desde el medio de Ntdsutil.exe](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Si usas medios protegidos con SYSKEY, el Administrador del servidor pregunta la contraseña de la imagen durante la comprobación.

![Captura de pantalla de la ventana del símbolo del sistema que muestra los resultados de la ejecución de Ntdsutil cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)

Los argumentos del cmdlet de ADDSDeployment correspondientes a las Opciones adicionales son:

```
-replicationsourcedc <string>
-installationmediapath <string>
-systemkey <secure string>
```

### <a name="paths"></a>Rutas de acceso
![Captura de pantalla de la página rutas de acceso del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)

La página **Rutas de acceso** permite reemplazar las ubicaciones de carpeta predeterminadas de la base de datos AD DS, los registros de transacciones de la base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas siempre están en subdirectorios de %systemroot%. Los argumentos del cmdlet ADDSDeployment correspondientes a **Rutas de acceso** son:

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="preparation-options"></a>Preparation Options
![Captura de pantalla de la página Opciones de preparación del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)

La página **Opciones de preparación** te alerta de que la configuración de AD DS incluye la extensión del esquema (forestprep) y la actualización del dominio (domainprep). Solo verás esta página si el bosque o dominio no han sido preparados en una instalación de controlador de dominio anterior de Windows Server 2012 o mediante la ejecución manual de Adprep.exe. Por ejemplo, el Asistente para configuración de Servicios de dominio de Active Directory suprime esta página si agregas un nuevo controlador de dominio de réplica al dominio raíz de un bosque de Windows Server 2012 existente.

Al hacer clic en **Siguiente** no se extiende el esquema ni se actualiza el dominio. Estos eventos solo se producen durante la fase de instalación. Esta página simplemente informa de los eventos que se producirán más adelante en la instalación.

Esta página también comprueba que las credenciales de usuario actuales pertenecen a los grupos Administradores de esquema o Administradores de organización, porque necesitas pertenecer a estos grupos para extender el esquema o preparar un dominio. Haz clic en **Cambiar** para proporcionar las credenciales de usuario adecuadas si la página te informa de que las credenciales actuales no proporcionan los permisos suficientes.

El argumento del cmdlet de ADDSDeployment correspondiente a las Opciones adicionales es:

```
-adprepcredential <pscredential>
```

> [!IMPORTANT]
> Al igual que con las versiones anteriores de Windows Server, la preparación automatizada de dominios de Windows Server 2012 no ejecuta GPPREP. Ejecuta **adprep.exe /gpprep** manualmente para todos los dominios que Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 no prepararon anteriormente. Ejecuta GPPrep solo una vez en el historial de un dominio, no con cada actualización. Adprep.exe no ejecuta /gpprep automáticamente porque podría hacer que todos los archivos y carpetas que hay en la carpeta SYSVOL se vuelvan a replicar en todos los controladores de dominio.
>
> RODCPrep se ejecuta automáticamente cuando se promueve el primer RODC sin preconfigurar de un dominio. Esto no sucede cuando se promueve el primer controlador de dominio de Windows Server 2012 de escritura. También puedes ejecutar manualmente **adprep.exe /rodcprep** si planeas implementar controladores de dominio de solo lectura.

### <a name="review-options-and-view-script"></a>Revisión de opciones y visualización del script
![Captura de pantalla de la página Opciones de revisión del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)

La página **Revisar opciones** permite validar la configuración y asegura que esta cumpla con los requisitos antes de comenzar con la instalación. Esta no es la última oportunidad para detener la instalación con el Administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con esta.

La página **Revisar opciones** del Administrador del servidor también ofrece un botón **Ver script** opcional para crear un archivo de texto Unicode que contiene la configuración ADDSDeployment actual como un script Windows PowerShell único. Esto le permite usar la interfaz gráfica del Administrador del servidor como un estudio de implementación de Windows PowerShell. Use el Asistente para configuración de Servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, cancele el asistente. Este proceso crea un ejemplo válido y sintácticamente correcto para realizar más modificaciones o la utilización directa. Por ejemplo:

```
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSDomainController `
-AllowPasswordReplicationAccountName @(CORP\Allowed RODC Password Replication Group, CORP\Chicago RODC Admins, CORP\Chicago RODC Users and Computers) `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath C:\Windows\NTDS `
-DelegatedAdministratorAccountName CORP\Chicago RODC Admins `
-DenyPasswordReplicationAccountName @(BUILTIN\Administrators, BUILTIN\Server Operators, BUILTIN\Backup Operators, BUILTIN\Account Operators, CORP\Denied RODC Password Replication Group) `
-DomainName corp.contoso.com `
-InstallDNS:$true `
-LogPath C:\Windows\NTDS `
-ReadOnlyReplica:$true `
-SiteName Default-First-Site-Name `
-SYSVOLPath C:\Windows\SYSVOL
-Force:$true

```

> [!NOTE]
> Normalmente, el Administrador del servidor rellena todos los argumentos con valores al realizar la promoción y no se basa en los valores predeterminados (dado que pueden cambiar entre las futuras versiones de Windows o los Service Pack). La excepción es el argumento **-safemodeadministratorpassword**. Para forzar la aparición de un aviso de confirmación, omite el valor al ejecutar el cmdlet de forma interactiva.

Usa el argumento Whatif opcional con el cmdlet Install-ADDSDomainController para revisar la información de la configuración. Esto te permite ver los valores explícitos e implícitos de los argumentos de un cmdlet.

![Captura de pantalla de la ventana de PowerShell que muestra los resultados del cmdlet Install-ADDSDomainController cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)

### <a name="prerequisites-check"></a>Prerequisites Check
![Captura de pantalla de la página de comprobación de requisitos previos del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)

La **Comprobación de requisitos previos** es una característica nueva en la configuración de dominios de AD DS. Esta nueva fase valida la capacidad de la configuración del servidor para admitir un nuevo bosque de AD DS.

Al instalar un nuevo dominio raíz de bosque, el Asistente para configuración de Servicios de dominio de Active Directory del Administrador del servidor invoca una serie de pruebas modulares en serie. Las pruebas te envían alertas con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso del controlador de dominio no puede continuar hasta que se superen todas las pruebas de requisitos previos.

La **Comprobación de requisitos previos** también expone información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.

No puedes omitir la **Comprobación de requisitos previos** al usar el Administrador del servidor, pero sí al utilizar el cmdlet de implementación de AD DS con el siguiente argumento:

```
-skipprechecks

```

Haz clic en **Instalar** para comenzar el proceso de promoción del controlador de dominio. Esta es la última oportunidad para cancelar la instalación. Una vez iniciado, el proceso de promoción no se puede cancelar. El equipo se reiniciará automáticamente al finalizar la promoción, independientemente del resultado de la misma.

### <a name="installation"></a>Instalación
![Captura de pantalla de la página de instalación del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)

Cuando se muestra la página **Instalación**, la configuración del controlador de dominio comienza y no se puede detener ni cancelar. Las operaciones detalladas se muestran en esta página y se escriben en los registros:

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

Para instalar un nuevo bosque de Active Directory con el módulo ADDSDeployment, utiliza este cmdlet:

```
Install-addsdomaincontroller

```

Vea la tabla del **cmdlet de ADDSDeployment** al principio de esta sección para ver los argumentos obligatorios y opcionales.

El cmdlet **Install-addsdomaincontroller** solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con los argumentos mínimos necesarios **-domainname**, **-readonlyreplica**, **-sitename** y **-credential**. Observa cómo, al igual que el Administrador del servidor, **Install-ADDSDomainController** te recuerda que la promoción reiniciará el servidor automáticamente:

![Captura de pantalla de la ventana de PowerShell que muestra el resultado del cmdlet install-addsdomaincontroller cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)

![Captura de pantalla de la ventana de PowerShell que muestra el progreso de la validación y la instalación cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)

Para aceptar el aviso de reinicio de forma automática, utiliza los argumentos **-force** o **-confirm:$false** con cualquier cmdlet de ADDSDeployment de Windows PowerShell. Para impedir que el servidor se reinicie automáticamente al finalizar la promoción, utiliza el argumento **-norebootoncompletion**.

> [!WARNING]
> Se recomienda no invalidar el reinicio. El controlador de dominio debe reiniciarse para funcionar correctamente. Si cierras sesión en el controlador de dominio, no puedes volver a iniciar sesión interactivamente hasta que lo reinicies.

### <a name="results"></a>Results
![Captura de pantalla de la página resultados del Asistente para configuración de Active Directory Domain Services cuando no hay ninguna implementación de ensayo.](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)

La página **Resultados** indica si la promoción se realizó correctamente o si se produjo algún error, junto con toda la información administrativa importante que corresponda. El controlador de dominio se reiniciará automáticamente 10 segundos después.
