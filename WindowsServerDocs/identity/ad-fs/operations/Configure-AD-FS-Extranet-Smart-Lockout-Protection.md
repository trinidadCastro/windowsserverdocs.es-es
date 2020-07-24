---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar AD FS protección de bloqueo de extranet
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 05/20/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5cb6246b00d891bd18f30b75b591dd4aaae021f5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962657"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet Lockout and Extranet Smart Lockout (Bloqueo de extranet de AD FS y bloqueo inteligente de extranet)

## <a name="overview"></a>Información general

El bloqueo inteligente de extranet (ESL) evita que los usuarios experimenten el bloqueo de cuentas de extranet de actividades malintencionadas.  

ESL permite a los AD FS diferenciar los intentos de inicio de sesión de una ubicación conocida para un usuario y los intentos de inicio de sesión de lo que puede ser un atacante. AD FS puede bloquear a los atacantes y permitir que los usuarios válidos sigan usando sus cuentas. Esto evita que y proteja contra la denegación de servicio y ciertas clases de ataques de pulverización de contraseñas para el usuario. ESL está disponible para AD FS en Windows Server 2016 y está integrado en AD FS en Windows Server 2019.

ESL solo está disponible para las solicitudes de autenticación de nombre de usuario y contraseña que llegan a través de la extranet con el proxy de aplicación web o un proxy de terceros. Cualquier proxy de terceros debe admitir el protocolo MS-ADFSPIP para su uso en lugar del proxy de aplicación Web, como [F5 BIG-IP Access Policy Manager](https://devcentral.f5.com/s/articles/ad-fs-proxy-replacement-on-f5-big-ip-30191). Consulte la documentación del proxy de terceros para determinar si el proxy es compatible con el protocolo MS-ADFSPIP.   

## <a name="additional-features-in-ad-fs-2019"></a>Características adicionales en AD FS 2019
El bloqueo inteligente de extranet en AD FS 2019 agrega las siguientes ventajas en comparación con AD FS 2016:
- Establezca umbrales de bloqueo independientes para ubicaciones conocidas y desconocidas para que los usuarios de ubicaciones válidas conocidas puedan tener más espacio para el error que las solicitudes de ubicaciones sospechosas.
- Habilite el modo de auditoría para el bloqueo inteligente y siga aplicando el comportamiento de bloqueo temporal anterior. Esto le permite obtener información sobre las ubicaciones conocidas de los usuarios y seguir protegiendo la característica de bloqueo de la extranet que está disponible en AD FS 2012R2.  

## <a name="how-it-works"></a>Cómo funciona
### <a name="configuration-information"></a>Información de configuración
Cuando ESL está habilitado, se crea una nueva tabla en la base de datos de artefactos, AdfsArtifactStore. AccountActivity, y se selecciona un nodo en la granja de AD FS como el maestro de "actividad de usuario". En una configuración de WID, este nodo es siempre el nodo principal. En una configuración de SQL, se selecciona un nodo para que sea el maestro de actividad del usuario.  

Para ver el nodo seleccionado como maestro de actividad del usuario. (Get-AdfsFarmInformation). FarmRoles

Todos los nodos secundarios se pondrá en contacto con el nodo maestro en cada inicio de sesión nuevo a través del puerto 80 para conocer el valor más reciente de los recuentos de contraseñas no válidas y los nuevos valores de ubicación conocidos, y actualizar ese nodo una vez procesado el inicio de sesión.

![configuración](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Si el nodo secundario no puede ponerse en contacto con el maestro, escribirá los eventos de error en el registro de administración de AD FS. Se seguirán procesando las autenticaciones, pero AD FS solo escribirá el estado actualizado localmente. AD FS intentará ponerse en contacto con el maestro cada 10 minutos y volverá a la maestra una vez que el maestro esté disponible.

### <a name="terminology"></a>Terminología
- **FamiliarLocation**: durante una solicitud de autenticación, ESL comprueba todas las direcciones IP presentadas. Estas direcciones IP serán una combinación de IP de red, IP reenviada y el x-forwarded-for IP opcional. Si la solicitud es correcta, todas las direcciones IP se agregan a la tabla de actividad de la cuenta como "direcciones IP conocidas". Si la solicitud tiene todas las direcciones IP presentes en las "direcciones IP conocidas", la solicitud se tratará como una ubicación "familiar".
- **UnknownLocation**: Si una solicitud que entra en tiene al menos una dirección IP que no está presente en la lista de "FamiliarLocation" existente, la solicitud se tratará como una ubicación "desconocida". Esto es para controlar escenarios de proxy, como la autenticación heredada de Exchange Online, donde las direcciones de Exchange Online controlan las solicitudes correctas y erróneas.  
- **badPwdCount**: un valor que representa el número de veces que se envió una contraseña incorrecta y que la autenticación no se realizó correctamente. Para cada usuario, se mantienen contadores independientes para ubicaciones conocidas y ubicaciones desconocidas.
- **UnknownLockout**: valor booleano por usuario si se bloquea el acceso del usuario desde ubicaciones desconocidas. Este valor se calcula en función de los valores de badPwdCountUnfamiliar y ExtranetLockoutThreshold.
- **ExtranetLockoutThreshold**: este valor determina el número máximo de intentos de contraseña incorrectos. Cuando se alcanza el umbral, ADFS rechazará las solicitudes de la extranet hasta que se haya superado la ventana de observación.
- **ExtranetObservationWindow**: este valor determina la duración del bloqueo de las solicitudes de nombre de usuario y contraseña de ubicaciones desconocidas. Cuando se haya superado la ventana, ADFS comenzará de nuevo a realizar la autenticación de nombre de usuario y contraseña desde ubicaciones desconocidas.
- **ExtranetLockoutRequirePDC**: cuando está habilitado, el bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando está deshabilitada, el bloqueo de extranet se reservará a otro controlador de dominio en caso de que el PDC no esté disponible.  
- **ExtranetLockoutMode**: controla únicamente el modo en que se aplica el modo de bloqueo inteligente de extranet
    - **ADFSSmartLockoutLogOnly**: el bloqueo inteligente de extranet está habilitado, pero AD FS solo escribirá eventos de administración y de auditoría, pero no rechazará las solicitudes de autenticación. Este modo está diseñado para habilitarse inicialmente para que FamiliarLocation se rellenen antes de que ' ADFSSmartLockoutEnforce ' esté habilitado.
    - **ADFSSmartLockoutEnforce**: compatibilidad total para el bloqueo de solicitudes de autenticación desconocidas cuando se alcanzan los umbrales.

Se admiten las direcciones IPv4 e IPv6.

### <a name="anatomy-of-a-transaction"></a>Anatomía de una transacción
- **Comprobación previa a la**autenticación: durante una solicitud de autenticación, ESL comprueba todas las direcciones IP presentadas. Estas direcciones IP serán una combinación de IP de red, IP reenviada y el x-forwarded-for IP opcional. En los registros de auditoría, estas direcciones IP se enumeran en el <IpAddress> campo en el orden x-MS-forwarded-Client-IP, x-forwarded-for, x-MS-Proxy-Client-IP.

  En función de estas direcciones IP, ADFS determina si la solicitud proviene de una ubicación conocida o desconocida y, a continuación, comprueba si el valor de badPwdCount respectivo es menor que el límite de umbral establecido o si el último intento **erróneo** se produjo más tiempo que el intervalo de tiempo de la ventana de observación. Si se cumple una de estas condiciones, ADFS permite que esta transacción se realice en el procesamiento y la validación de credenciales. Si ambas condiciones son false, la cuenta ya está en estado bloqueado hasta que la ventana de observación pase. Una vez que se pasa la ventana de observación, se permite que el usuario se autentique. Tenga en cuenta que, en 2019, ADFS comprobará el límite de umbral adecuado en función de si la dirección IP coincide o no con una ubicación conocida.
- **Inicio de sesión correcto**: Si el inicio de sesión se realiza correctamente, las direcciones IP de la solicitud se agregan a la lista de direcciones IP de ubicación conocida del usuario.  
- **Error de inicio de sesión**: si se produce un error en el inicio de sesión, se aumenta el badPwdCount. El usuario pasará a un estado de bloqueo si el atacante envía más contraseñas incorrectas al sistema de las que permite el umbral. (badPwdCount > ExtranetLockoutThreshold)  

![configuración](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

El valor "UnknownLockout" será igual a true cuando se bloquee la cuenta. Esto significa que el badPwdCount del usuario es superior al umbral, es decir, alguien intentó más contraseñas de las permitidas por el sistema. En este estado, hay dos maneras en las que un usuario válido puede iniciar sesión.
- El usuario debe esperar a que transcurra el tiempo de ObservationWindow o
- Para restablecer el estado de bloqueo, restablezca el valor de badPwdCount a cero con "RESET-ADFSAccountLockout".

Si no se produce ningún restablecimiento, a la cuenta se le permitirá un único intento de contraseña en AD para cada ventana de observación. La cuenta volverá al estado bloqueado después de ese intento y se reiniciará la ventana de observación. El valor badPwdCount solo se restablecerá automáticamente después de un inicio de sesión de contraseña correcto.

### <a name="log-only-mode-versus-enforce-mode"></a>Modo de solo registro frente al modo "exigir"
La tabla AccountActivity se rellena durante el modo ' solo registro ' y el modo ' exigir '. Si se omite el modo ' solo registro ' y ESL se mueve directamente al modo ' exigir ' sin el período de espera recomendado, ADFS no conocerá las direcciones IP conocidas de los usuarios. En este caso, ESL se comportaría como ' ADBadPasswordCounter ', lo que podría bloquear el tráfico de usuario legítimo si la cuenta de usuario se encuentra en un ataque por fuerza bruta activo. Si el modo "solo registro" se omite y el usuario entra en un estado bloqueado con "UnknownLockout" = TRUE e intenta iniciar sesión con una contraseña correcta desde una dirección IP que no está en la lista de direcciones IP "familiar", no podrá iniciar sesión. El modo de solo registro se recomienda durante 3-7 días para evitar este escenario. Si las cuentas se están realizando activamente ataques, se necesita un mínimo de 24 horas de modo de "solo registro" para evitar bloqueos en los usuarios legítimos.  

## <a name="extranet-smart-lockout-configuration"></a>Configuración de bloqueo inteligente de extranet  

### <a name="prerequisites-for-ad-fs-2016"></a>Requisitos previos para AD FS 2016

1. **Instalar actualizaciones en todos los nodos de la granja de servidores**

   En primer lugar, asegúrese de que todos los servidores de Windows Server 2016 AD FS estén actualizados a partir de las actualizaciones de Windows del 2018 de junio y de que la granja de AD FS 2016 se esté ejecutando en el nivel de comportamiento de la granja de 2016.
1. **Comprobar permisos**

   El bloqueo inteligente de extranet requiere que la administración remota de Windows esté habilitada en cada servidor de AD FS.
3. **Actualizar permisos de base de datos de artefactos**

   El bloqueo inteligente de extranet requiere que la cuenta de servicio de AD FS tenga permisos para crear una nueva tabla en la base de datos de artefactos de AD FS. Inicie sesión en cualquier AD FS Server como administrador de AD FS y, a continuación, conceda este permiso mediante la ejecución de los siguientes comandos en una ventana del símbolo del sistema de PowerShell:

   ``` powershell
   PS C:\>$cred = Get-Credential
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
   ```
   >[!NOTE]
   >El marcador de posición $cred es una cuenta que tiene permisos de administrador AD FS. Debe proporcionar los permisos de escritura para crear la tabla.

   Los comandos anteriores pueden producir un error debido a la falta de permisos suficientes porque la granja de AD FS usa SQL Server y la credencial proporcionada anteriormente no tiene permiso de administrador en el servidor SQL Server. En este caso, puede configurar los permisos de base de datos manualmente en SQL Server base de datos mediante la ejecución del comando siguiente cuando esté conectado a la base de datos AdfsArtifactStore.
    ```  
    # when prompted with “Are you sure you want to perform this action?”, enter Y.

    [CmdletBinding(SupportsShouldProcess=$true,ConfirmImpact = 'High')]
    Param()

    $fileLocation = "$env:windir\ADFS\Microsoft.IdentityServer.Servicehost.exe.config"

    if (-not [System.IO.File]::Exists($fileLocation))
    {
    write-error "Unable to open ADFS configuration file."
    return
    }

    $doc = new-object Xml
    $doc.Load($fileLocation)
    $connString = $doc.configuration.'microsoft.identityServer.service'.policystore.connectionString
    $connString = $connString -replace "Initial Catalog=AdfsConfigurationV[0-9]*", "Initial Catalog=AdfsArtifactStore"

    if ($PSCmdlet.ShouldProcess($connString, "Executing SQL command sp_addrolemember 'db_owner', 'db_genevaservice' "))
    {
    $cli = new-object System.Data.SqlClient.SqlConnection
    $cli.ConnectionString = $connString
    $cli.Open()

    try
    {     

    $cmd = new-object System.Data.SqlClient.SqlCommand
    $cmd.CommandText = "sp_addrolemember 'db_owner', 'db_genevaservice'"
    $cmd.Connection = $cli
    $rowsAffected = $cmd.ExecuteNonQuery()  
    if ( -1 -eq $rowsAffected )
    {
    write-host "Success"
    }
    }
    finally
    {
    $cli.CLose()
    }
    }
    ```

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Asegúrese de que está habilitado el registro de auditoría de seguridad de AD FS
Esta característica hace uso de los registros de auditoría de seguridad, por lo que la auditoría debe estar habilitada en AD FS, así como en la directiva local en todos los servidores de AD FS.

### <a name="configuration-instructions"></a>Instrucciones de configuración
El bloqueo inteligente de extranet usa la propiedad **ExtranetLockoutEnabled**de ADFS. Esta propiedad se usaba previamente para controlar el "bloqueo flexible de extranet" en el servidor 2012R2. Si se ha habilitado el bloqueo automático de extranet, para ver la configuración de propiedades actual, ejecute ` Get-AdfsProperties` .

### <a name="configuration-recommendations"></a>Recomendaciones para la configuración
Al configurar el bloqueo inteligente de extranet, siga las prácticas recomendadas para establecer umbrales:  

`ExtranetObservationWindow (new-timespan -Minutes 30)`

`ExtranetLockoutThreshold: Half of AD Threshold Value`

Valor de AD: 20, ExtranetLockoutThreshold: 10

Active Directory el bloqueo funciona independientemente del bloqueo inteligente de extranet. Sin embargo, si está habilitado el bloqueo de Active Directory, ExtranetLockoutThreshold en AD FS < umbral de bloqueo de cuenta en AD

`ExtranetLockoutRequirePDC - $false`

Cuando está habilitada, el bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando está deshabilitada y configurada como falsa, el bloqueo de extranet se reservará a otro controlador de dominio en caso de que el PDC no esté disponible.

Para establecer esta propiedad, ejecute:

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
```
### <a name="enable-log-only-mode"></a>Habilitar el modo de solo registro

En el modo de solo registro, AD FS rellena la información de ubicación conocida de los usuarios y escribe los eventos de auditoría de seguridad, pero no bloquea las solicitudes. Este modo se usa para validar que se está ejecutando el bloqueo inteligente y para habilitar AD FS para "aprender" las ubicaciones conocidas de los usuarios antes de habilitar el modo "aplicar". Como AD FS aprende, almacena la actividad de inicio de sesión por usuario (ya sea en modo de solo registro o en modo de aplicación).
Establezca el comportamiento de bloqueo en solo registro mediante la ejecución de los siguientes commandlet.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly`

El modo de solo registro está pensado para ser un estado temporal, de modo que el sistema pueda conocer el comportamiento de inicio de sesión antes de introducir la aplicación de bloqueo con el comportamiento de bloqueo inteligente. La duración recomendada para el modo de solo registro es de 3-7 días. Si las cuentas están activamente en ataque, el modo de solo registro debe ejecutarse durante un período mínimo de 24 horas.

En AD FS 2016, si está habilitado el comportamiento de "bloqueo automático de extranet" de 2012R2 antes de habilitar el bloqueo inteligente de extranet, el modo de solo registro deshabilitará el comportamiento de "bloqueo automático de extranet". AD FS el bloqueo inteligente no bloqueará a los usuarios en modo de solo registro. Sin embargo, AD local puede bloquear al usuario en función de la configuración de AD. Revise las directivas de bloqueo de AD para saber cómo AD local puede bloquear a los usuarios.

En AD FS 2019, una ventaja adicional es poder habilitar el modo de solo registro para el bloqueo inteligente y, al mismo tiempo, seguir aplicando el comportamiento de bloqueo flexible anterior mediante el siguiente PowerShell.

`Set-AdfsProperties -ExtranetLockoutMode 3`

Para que el nuevo modo surta efecto, reinicie el servicio AD FS en todos los nodos de la granja.

`Restart-service adfssrv`

Una vez configurado el modo, puede habilitar el bloqueo inteligente mediante el parámetro EnableExtranetLockout

`Set-AdfsProperties -EnableExtranetLockout $true`

### <a name="enable-enforce-mode"></a>Habilitar el modo de aplicación

Una vez que esté familiarizado con la ventana umbral de bloqueo y observación, ESL se puede pasar al modo "exigir" mediante el siguiente cmdlet de PSH:

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce`

Para que el nuevo modo surta efecto, reinicie el servicio AD FS en todos los nodos de la granja de servidores mediante el siguiente comando.

`Restart-service adfssrv`

## <a name="manage-user-account-activity"></a>Administrar la actividad de la cuenta de usuario
AD FS proporciona tres cmdlets para administrar los datos de actividad de la cuenta. Estos cmdlets se conectan automáticamente al nodo de la granja que contiene el rol maestro.
>[!NOTE]
>Solo se puede usar la administración suficiente (JEA) para delegar AD FS commandlets para restablecer los bloqueos de cuentas. Por ejemplo, se puede delegar permisos al personal del Departamento de soporte técnico para usar ESL commandlets. Para obtener información sobre cómo delegar permisos para usar estos cmdlets, vea [delegar AD FS PowerShell Commandlet Access to usuarios que no son administradores](delegate-ad-fs-pshell-access.md) .

Este comportamiento se puede invalidar pasando el parámetro-Server.

- Get-ADFSAccountActivity-UserPrincipalName

  Lea la actividad de la cuenta actual para una cuenta de usuario. El cmdlet se conecta siempre automáticamente al maestro de la granja mediante el punto de conexión REST de la actividad de la cuenta. Por lo tanto, todos los datos siempre deben ser coherentes.

`Get-ADFSAccountActivity user@contoso.com`

  Propiedades:
    - BadPwdCountFamiliar: se incrementa cuando una autenticación se realiza correctamente desde una ubicación conocida.
    - BadPwdCountUnknown: se incrementa cuando una autenticación no se realiza correctamente desde una ubicación desconocida
    - LastFailedAuthFamiliar: Si la autenticación no se realizó correctamente desde una ubicación conocida, LastFailedAuthUnknown se establece en hora de la autenticación incorrecta.
    - LastFailedAuthUnknown: Si la autenticación no se realizó correctamente desde una ubicación desconocida, LastFailedAuthUnknown se establece en la hora de la autenticación incorrecta.
    - FamiliarLockout: valor booleano que será "true" si "BadPwdCountFamiliar" > ExtranetLockoutThreshold
    - UnknownLockout: valor booleano que será "true" si "BadPwdCountUnknown" > ExtranetLockoutThreshold  
    - FamiliarIPs: máximo de 20 direcciones IP que son conocidas para el usuario. Cuando esto se supera, se quitará la dirección IP más antigua de la lista.
-    Set-ADFSAccountActivity

     Agrega nuevas ubicaciones conocidas. La lista de direcciones IP conocida tiene un máximo de 20 entradas; si se supera este valor, se quitará la dirección IP más antigua de la lista.

`Set-ADFSAccountActivity user@contoso.com -AdditionalFamiliarIps “1.2.3.4”`

- RESET-ADFSAccountLockout

  Restablece el contador de bloqueo de una cuenta de usuario para cada ubicación familiar (badPwdCountFamiliar) o los contadores de ubicación desconocidos (badPwdCountUnfamiliar). Al restablecer un contador, el valor "FamiliarLockout" o "UnfamiliarLockout" se actualizará, ya que el contador de restablecimiento será menor que el umbral.  

`Reset-ADFSAccountLockout user@contoso.com -Location Familiar`
`Reset-ADFSAccountLockout user@contoso.com -Location Unknown`

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>Registro de eventos & información de actividad del usuario para el bloqueo de AD FS extranet

### <a name="connect-health"></a>Connect Health
La manera recomendada de supervisar la actividad de la cuenta de usuario es a través de Connect Health. Connect Health genera informes descargables sobre direcciones IP de riesgo y intentos de contraseña incorrectos. Cada elemento del informe de direcciones IP de riesgo muestra información agregada sobre las actividades de inicio de sesión de AD FS con errores que sobrepasan el umbral designado. Las notificaciones por correo electrónico se pueden establecer para alertar a los administradores en cuanto esto suceda con la configuración de correo electrónico personalizable. Para obtener más información e instrucciones de configuración, visite la [documentación de Connect Health](/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="ad-fs-extranet-smart-lockout-events"></a>AD FS eventos de bloqueo inteligente de extranet.

>[!NOTE]
> Solucionar problemas de bloqueo inteligente de extranet con la [ayuda AD FS guía de solución de problemas de bloqueo de extranet](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/a73d5843-9939-4c03-80a1-adcbbf3ccec8)

Para que se escriban los eventos de bloqueo inteligente de extranet, ESL debe estar habilitado en el modo "solo registro" o "exigir" y la auditoría de seguridad de ADFS está habilitada.
AD FS escribirá eventos de bloqueo de extranet en el registro de auditoría de seguridad:
- Cuando se bloquea un usuario (alcanza el umbral de bloqueo para los intentos de inicio de sesión incorrectos)
- Cuando AD FS recibe un intento de inicio de sesión para un usuario que ya está en el estado de bloqueo

En el modo de solo registro, puede comprobar si hay eventos de bloqueo en el registro de auditoría de seguridad. En el caso de los eventos encontrados, puede comprobar el estado de usuario mediante el cmdlet Get-ADFSAccountActivity para determinar si el bloqueo se ha producido desde direcciones IP conocidas o desconocidas, y para doblar la lista de direcciones IP conocidas para ese usuario.


|Id. de evento|Descripción|
|-----|-----|
|1203|Este evento se escribe para cada intento de contraseña incorrecta. En cuanto el badPwdCount alcance el valor especificado en ExtranetLockoutThreshold, la cuenta se bloqueará en ADFS durante el tiempo especificado en ExtranetObservationWindow.</br>ID. de actividad: %1</br>XML: %2|
|1201|Este evento se escribe cada vez que se bloquea un usuario. </br>ID. de actividad: %1</br>XML: %2|
|557 (ADFS 2019)| Error al intentar comunicarse con el servicio Rest del almacén de cuentas en el nodo %1. Si se trata de una granja WID, puede que el nodo principal esté sin conexión. Si se trata de una granja de servidores SQL, ADFS seleccionará automáticamente un nuevo nodo para hospedar el rol de maestro de almacén de usuarios.|
|562 (ADFS 2019)|Se produjo un error al communcating con el punto de conexión del almacén de cuentas en el servidor %1.</br>Mensaje de excepción: %2|
|563 (ADFS 2019)|Error al calcular el estado de bloqueo de la extranet. Debido al valor de la opción %1, se permitirá la autenticación para este usuario y la emisión de tokens continuará. Si se trata de una granja WID, puede que el nodo principal esté sin conexión. Si se trata de una granja de servidores SQL, ADFS seleccionará automáticamente un nuevo nodo para hospedar el rol de maestro de almacén de usuarios.</br>Nombre del servidor de almacén de cuentas: %2</br>ID. de usuario: %3</br>Mensaje de excepción: %4|
|512|La cuenta para el siguiente usuario está bloqueada. Se permite un intento de inicio de sesión debido a la configuración del sistema.</br>ID. de actividad: %1 </br>Usuario: %2 </br>IP de cliente: %3 </br>Recuento de contraseñas no válidas: %4  </br>Último intento de contraseña incorrecta: %5|
|515|La siguiente cuenta de usuario se encontraba en un estado bloqueado y se ha proporcionado la contraseña correcta. Esta cuenta puede estar en peligro.</br>Datos adicionales </br>ID. de actividad: %1 </br>Usuario: %2 </br>IP de cliente: %3 |
|516|La siguiente cuenta de usuario se ha bloqueado debido a demasiados intentos de contraseña incorrectos.</br>ID. de actividad: %1  </br>Usuario: %2  </br>IP de cliente: %3  </br>Recuento de contraseñas no válidas: %4  </br>Último intento de contraseña incorrecta: %5|

## <a name="esl-frequently-asked-questions"></a>Preguntas más frecuentes sobre ESL

**¿Una granja de AD FS que usa el bloqueo inteligente de extranet en el modo de aplicación ve nunca bloqueos de usuario malintencionados?** 

R: Si el bloqueo inteligente de ADFS está establecido en el modo "aplicar", nunca verá la cuenta de usuario legítimo bloqueada por fuerza bruta o por denegación de servicio. La única manera en que un bloqueo de cuenta malintencionado puede impedir que un usuario inicie sesión es si el actor incorrecto tiene la contraseña de usuario o puede enviar solicitudes desde una dirección IP conocida (conocida) del usuario. 

**¿Qué ocurre si ESL está habilitado y el actor incorrecto tiene una contraseña de usuario?** 

R: el objetivo típico del escenario de ataque por fuerza bruta es adivinar una contraseña e iniciar sesión correctamente.Si un usuario está phish o si se adivina una contraseña, la característica ESL no bloqueará el acceso, ya que el inicio de sesión satisfará los criterios "correctos" de la contraseña correcta y la nueva dirección IP. La dirección IP de actores no válidos aparecería como una "conocida".La mejor mitigación en este escenario es borrar la actividad del usuario en ADFS y requerir autenticación multifactor para los usuarios. Se recomienda encarecidamente instalar la protección de contraseña de AAD para garantizar que las contraseñas adivinables no lleguen al sistema.

**Si el usuario nunca ha iniciado sesión correctamente desde una dirección IP y, a continuación, intenta con la contraseña incorrecta varias veces, podrá iniciar sesión una vez que escriba la contraseña correctamente.** 

R: Si un usuario envía varias contraseñas no válidas (es decir, sin escribir legítimamente) y en el siguiente intento obtiene la contraseña correcta, el usuario se ejecutará inmediatamente para iniciar sesión. Esto borrará el recuento de contraseñas no válidas y agregará esa dirección IP a la lista FamiliarIPs.Sin embargo, si superan el umbral de inicios de sesión erróneos de la ubicación desconocida, entrarán en el estado de bloqueo y tendrán que esperar más allá de la ventana de observación e iniciar sesión con una contraseña válida o requerir la intervención del administrador para restablecer su cuenta.  
 
**¿ESL funciona también en la intranet?**

R: si los clientes se conectan directamente a los servidores de ADFS y no a través de los servidores proxy de aplicación Web, no se aplicará el comportamiento de ESL.  

**Veo direcciones IP de Microsoft en el campo IP de cliente. ¿ESL bloquea los ataques por fuerza bruta en proxy de EXO?**  

R: ESL funcionará bien para evitar escenarios de ataque por fuerza bruta de Exchange Online u otros. Una autenticación heredada tiene un "ID. de actividad" de 00000000-0000-0000-0000-000000000000.En estos ataques, el actor incorrecto está aprovechando la autenticación básica de Exchange Online (también conocida como autenticación heredada) para que la dirección IP del cliente aparezca como una Microsoft. Los servidores de Exchange online en el proxy en la nube la comprobación de autenticación en nombre del cliente de Outlook. En estos casos, la dirección IP del remitente malintencionado estará en x-MS-forwarded-Client-IP y la dirección IP del servidor Microsoft Exchange Online se incluirá en el valor x-MS-Client-IP.
El bloqueo inteligente de extranet comprueba las direcciones IP de red, las IP reenviadas, x-forwarded-Client-IP y el valor x-MS-Client-IP. Si la solicitud es correcta, todas las direcciones IP se agregan a la lista familiar. Si entra una solicitud y cualquiera de las direcciones IP presentadas no está en la lista familiar, la solicitud se marcará como desconocida. El usuario conocido podrá iniciar sesión correctamente mientras se bloquearán las solicitudes de las ubicaciones desconocidas.  

**¿Puedo calcular el tamaño de ADFSArtifactStore antes de habilitar ESL?**

R: con ESL habilitado, AD FS realiza un seguimiento de la actividad de la cuenta y de las ubicaciones conocidas de los usuarios en la base de datos ADFSArtifactStore. Esta base de datos se escala en función del número de usuarios y de las ubicaciones conocidas a las que se realiza un seguimiento. Al planificar la habilitación de ESL, puedes calcular el tamaño de la base de datos ADFSArtifactStore para que crezca a una velocidad de hasta 1 GB por 100 000 usuarios. Si la granja de AD FS usa Windows Internal Database (WID), la ubicación predeterminada de los archivos de base de datos es C:\Windows\WID\Data\. Para evitar que esta unidad se llene, asegúrate de tener un mínimo de 5 GB de almacenamiento disponible antes de habilitar ESL. Además del almacenamiento en disco, planifica un aumento de la memoria de proceso total después de habilitar ESL de hasta un 1 GB de RAM adicional para los rellenados de usuarios de 500 000 o menos.


## <a name="additional-references"></a>Referencias adicionales  
[Prácticas recomendadas para proteger Servicios de federación de Active Directory (AD FS)](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](/powershell/module/adfs/set-adfsproperties?view=win10-ps)

[Operaciones de AD FS](../ad-fs-operations.md)
