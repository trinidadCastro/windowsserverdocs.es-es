---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar la protección de bloqueo de Extranet de AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 03/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dd6dea2fb8a16bfdbe93f93fbdd1dc5ac47af4be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189897"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet bloqueo inteligente y bloqueo de Extranet

## <a name="overview"></a>Información general

Bloqueo inteligente extranet (ESL) protege a los usuarios experimenten el bloqueo de cuenta de extranet frente a actividades malintencionadas.  

ESL permite que AD FS diferenciar entre los intentos de inicio de sesión desde una ubicación conocida para un usuario y los intentos de inicio de sesión de lo que puede ser un atacante. AD FS puede bloquear los atacantes al permitir que usuarios válidos de seguir usando sus cuentas. Esto impide y protege frente a denegación de servicio y ciertas clases de ataques a contraseñas aerosol en el usuario. ESL está disponible para AD FS en Windows Server 2016 y está integrada en AD FS en Windows Server 2019. 

ESL solo está disponible para el nombre de usuario y que vienen a través de la extranet con el Proxy de aplicación Web o una de las solicitudes de autenticación de contraseña 3rd proxy de entidad.   

## <a name="additional-features-in-ad-fs-2019"></a>Características adicionales de AD FS de 2019 
Bloqueo inteligente de la extranet de AD FS 2019 agrega las siguientes ventajas en comparación con AD FS 2016: 
- Establecer umbrales de bloqueo independientes para las ubicaciones conocidas y desconocidas para que los usuarios en ubicaciones buenos conocidos pueden tener más espacio para el error que las solicitudes desde ubicaciones sospechosas 
- Habilitar el modo de auditoría para el bloqueo inteligente sin dejar de aplicar un comportamiento de bloqueo temporal anterior. Esto le permite obtener información sobre ubicaciones conocidas de usuario y la protección de la característica de bloqueo de extranet que está disponible en 2012 R2 AD FS.  

## <a name="how-it-works"></a>Cómo funciona 
### <a name="configuration-information"></a>información de configuración 
Cuando se habilita ESL, se crea una nueva tabla en la base de datos de artefacto, AdfsArtifactStore.AccountActivity, y se selecciona un nodo en la granja de AD FS como el patrón "Actividad del usuario". En una configuración de WID, este nodo siempre es el nodo principal. En una configuración de SQL, se selecciona un nodo sea el patrón de actividad del usuario.  

Para ver el nodo seleccionado como el patrón de actividad del usuario. Get-AdfsFarmInformation.FarmRoles 

Todos los nodos secundarios se póngase en contacto con el nodo principal en cada nuevo inicio de sesión a través del puerto 80 para obtener información sobre el valor más reciente de los recuentos de contraseñas erróneas y nuevos valores de ubicación conocida y actualizar ese nodo después de procesa el inicio de sesión. 

![configuración](media/configure-ad-fs-extranet-smart-lockout-protection/esl1.png)

 Si el nodo secundario no puede ponerse en contacto con el maestro, escribirá los eventos de error en el registro de administración de AD FS. Autenticaciones continuará procesarse, pero AD FS solo escribirá el estado actualizado de forma local. AD FS volverá a intentar ponerse en contacto con el maestro de cada 10 minutos y volverá a cambiar al maestro de una vez que el maestro está disponible. 

### <a name="terminology"></a>Terminología 
- **FamiliarLocation**: Durante una solicitud de autenticación, ESL comprueba que todos presentan las direcciones IP. Estas direcciones IP será una combinación de dirección IP de red, reenvía la dirección IP y la IP de x-forwarded-for opcional. Si la solicitud se realiza correctamente, todas las direcciones IP se agregan a la tabla de la actividad de la cuenta como "direcciones IP conocida". Si la solicitud tiene las direcciones IP presente en las "direcciones IP conocida", la solicitud se tratará como un lugar "Conocido".
- **UnknownLocation**: Si una solicitud que entra en juego tiene al menos una dirección IP no está presente en la lista existente de "FamiliarLocation", la solicitud se tratará como una ubicación de "Desconocida". Esto sirve para controlar los escenarios de proxy como Exchange Online autenticación heredados en las direcciones de Exchange Online controlan solicitudes correctas e incorrectas.  
- **badPwdCount**: Un valor que representa el número de veces que se ha enviado una contraseña incorrecta y la autenticación no tuvo éxito. Para cada usuario, se mantienen contadores independientes para las ubicaciones conocidas y ubicaciones desconocidas. 
- **UnknownLockout**: Un valor booleano por usuario si el usuario está bloqueado obtengan acceso a desde ubicaciones desconocidas. Este valor se calcula en función de los valores de ExtranetLockoutThreshold y badPwdCountUnfamiliar. 
- **ExtranetLockoutThreshold**: Este valor determina el número máximo de intentos con contraseñas incorrectas. Cuando se alcanza el umbral, ADFS rechazará las solicitudes desde la extranet hasta que haya transcurrido la ventana de observación.
- **ExtranetObservationWindow**: Este valor determina la duración que se bloquearán las solicitudes de usuario y la contraseña desde ubicaciones desconocidas. Cuando ha transcurrido la ventana, se iniciará ADFS realizar la autenticación de usuario y la contraseña desde ubicaciones desconocidas nuevo. 
- **ExtranetLockoutRequirePDC**: Cuando se habilita, el bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando está deshabilitado, bloqueo de extranet retrocederá para otro controlador de dominio en caso de que el PDC no está disponible.  
- **ExtranetLockoutMode**: Modo de vs que se aplican solo de Extranet de bloqueo inteligente de registro de controles 
    - **ADFSSmartLockoutLogOnly**: Bloqueo inteligente extranet está habilitado, pero AD FS se sólo escribir admin y eventos de auditoría, pero no las solicitudes de autenticación de rechazo. Este modo está diseñado para habilitarse inicialmente para que FamiliarLocation rellenarse antes de 'ADFSSmartLockoutEnforce' está habilitada.
    - **ADFSSmartLockoutEnforce**: Compatibilidad completa para bloquear las solicitudes de autenticación no conoce cuando se alcanzan los umbrales. 

Se admiten direcciones IPv4 e IPv6. 

### <a name="anatomy-of-a-transaction"></a>Anatomía de una transacción 
- **Comprobación de pre-Auth**: Durante una solicitud de autenticación, ESL comprueba que todos presentan las direcciones IP. Estas direcciones IP será una combinación de dirección IP de red, reenvía la dirección IP y la IP de x-forwarded-for opcional. En los registros de auditoría, se enumeran estas direcciones IP en el <IpAddress> campo en el orden de x-ms-reenviados-ip del cliente, x-forwarded-for, x-ms-proxy-client-ip. 
 
  En función de estas direcciones IP, ADFS determina si la solicitud proviene de una ubicación desconocida o que ya conoce y, a continuación, comprueba si lo respectivo badPwdCount es menor que el límite de umbral establecido o si la última **no se pudo** intento se ha ocurrido más de la período de tiempo de la ventana de observación. Si se cumple una de estas condiciones, ADFS permite esta transacción para su posterior procesamiento y validación de credenciales. Si ambas condiciones son false, la cuenta ya está en un estado bloqueado hasta que se pasa de la ventana de observación. Después de la ventana de observación pasa, se permite al usuario un intento de autenticación. Tenga en cuenta que en 2019, ADFS comprobará con el límite de umbral apropiado en función de si la dirección IP coincide con una ubicación conocida o no.
- **Inicio de sesión correcto**: Si el inicio de sesión se realiza correctamente, las direcciones IP de la solicitud se agregan a la lista IP de ubicación conocida del usuario.  
- **Inicio de sesión incorrecto**: Si el inicio de sesión falla el badPwdCount aumenta. El usuario pasará a un estado de bloqueo si el atacante envía más contraseñas incorrectas en el sistema que permite el umbral. (badPwdCount > ExtranetLockoutThreshold)  

![configuración](media/configure-ad-fs-extranet-smart-lockout-protection/esl2.png)

El valor "UnknownLockout" será igual a true cuando la cuenta está bloqueada. Esto significa que badPwdCount del usuario a través de que el umbral, es decir, alguien ha intentado más contraseñas que se han permitido por el sistema. En este estado, hay 2 formas que puede iniciar sesión un usuario válido. 
- El usuario debe esperar que transcurra el tiempo de ObservationWindow o
- Para restablecer el estado de bloqueo, restablecer el badPwdCount a cero con 'Reset-ADFSAccountLockout'. 

Si se produce ningún restablece, se permitirá la cuenta de un intento de contraseña única en AD para cada ventana de observación. La cuenta se devolverá al estado bloqueado después de que se reiniciarán el intento y la ventana de observación. El valor badPwdCount solo restablecerá automáticamente después de un inicio de sesión de contraseña correcta. 

### <a name="log-only-mode-versus-enforce-mode"></a>Modo de solo registro frente al modo 'Aplicar' 
Se rellena la tabla AccountActivity tanto durante el modo "Solo registro" y el modo de 'Aplicar'. Si se omite el modo "Solo registro" y se mueve ESL directamente en modo 'Aplicar' sin el período de espera recomendada, no se conocerá las direcciones IP conocida de los usuarios para ADFS. En este caso, ESL comportaría como 'ADBadPasswordCounter' potencialmente bloqueando el tráfico de un usuario legítimo si la cuenta de usuario está bajo un ataque por fuerza bruta activo. Si se omite el modo 'Solo registro' y el usuario escribe un bloqueado el estado con "UnknownLockout" = TRUE e intenta iniciar sesión con una buena contraseña desde una dirección IP que no está en la lista IP "conocida", a continuación, no podrán iniciar sesión. Se recomienda el modo de solo registro de 3 a 7 días evitar esta situación. Si las cuentas de forma activa están siendo atacado, un mínimo de 24 horas del modo 'Solo registro' es necesario para evitar bloqueos a los usuarios legítimos.  

## <a name="extranet-smart-lockout-configuration"></a>Configuración de extranet de bloqueo inteligente  
 
### <a name="prerequisites-for-ad-fs-2016"></a>Requisitos previos para AD FS 2016 
 
1. **Instalar actualizaciones en todos los nodos de la granja de servidores**

   En primer lugar, asegúrese de todos los servidores de Windows Server 2016 AD FS están actualizados a partir de las actualizaciones de Windows de junio de 2018 y que se está ejecutando la granja de servidores de AD FS 2016 en el nivel de granja 2016 comportamiento.
1. **Comprobar los permisos** 

   Bloqueo inteligente extranet requiere que la administración remota de Windows esté habilitado en todos los servidores de AD FS.
3. **Actualizar los permisos de base de datos de artefacto** 
 
   Bloqueo inteligente extranet requiere la cuenta de servicio de AD FS tenga permisos para crear una nueva tabla en la base de datos del artefacto de AD FS. Inicie sesión en cualquier servidor de AD FS como un administrador de AD FS y, a continuación, este permiso se concede mediante la ejecución de los siguientes comandos en una ventana del símbolo del sistema de PowerShell: 

   ``` powershell
   PS C:\>$cred = Get-Credential 
   PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred 
   ``` 
   >[!NOTE]
   >El marcador de posición $cred es una cuenta que tenga permisos de administrador de AD FS. Esto debería proporcionar los permisos de escritura para crear la tabla. 

   Los comandos anteriores pueden producir un error debido a la falta de permisos suficientes, porque la granja de AD FS usa SQL Server y las credenciales proporcionadas anteriormente no tiene permiso de administrador en SQL server. En este caso, se pueden configurar los permisos de base de datos manualmente en la base de datos de SQL Server, ejecute el comando siguiente cuando esté conectado a la base de datos AdfsArtifactStore. 
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
Esta característica hace uso de la auditoría de seguridad de registros, por lo que la auditoría deben estar habilitados en AD FS, así como la directiva local en todos los servidores de AD FS. 
 
### <a name="configuration-instructions"></a>Instrucciones de configuración 
Bloqueo inteligente extranet usa la propiedad ADFS **ExtranetLockoutEnabled**. Esta propiedad se usó anteriormente para el control "Bloqueo de Extranet temporal" en Server 2012 R2. Si se ha habilitado el bloqueo temporal de la Extranet, para ver la configuración de la propiedad actual, ejecute ` Get-AdfsProperties` . 

### <a name="configuration-recommendations"></a>Recomendaciones de configuración 
Al configurar el bloqueo inteligente de Extranet, siga las prácticas recomendadas para configurar los umbrales:  

`ExtranetObservationWindow (new-timespan -Minutes 30)` 

`ExtranetLockoutThreshold: – 2x AD Threshold Value` 

Valor de AD: 20, ExtranetLockoutThreshold: 10 

Bloqueo de Active Directory funciona independientemente de bloqueo inteligente de la Extranet. Sin embargo, si está habilitado el bloqueo de Active Directory, ExtranetLockoutThreshold en AD FS < umbral de bloqueo de cuentas de AD 

`ExtranetLockoutRequirePDC - $false`
 
Cuando se habilita, el bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando se deshabilita y configura como false, bloqueo de extranet retrocederá para otro controlador de dominio en caso de que el PDC no está disponible. 
 
Para establecer esta propiedad ejecutar: 

``` powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false 
```
### <a name="enable-log-only-mode"></a>Habilitar el modo de solo registro 
 
En modo de solo registro, AD FS rellena la información de ubicación familiar a los usuarios y escribe los eventos de auditoría de seguridad, pero no bloquea las solicitudes. Este modo se utiliza para validar que se está ejecutando el bloqueo inteligente y para habilitar AD FS para "aprender" ubicaciones conocidas para los usuarios antes de habilitar el "modo de aplicación". Tal como se entera de AD FS, almacena la actividad de inicio de sesión por usuario (ya sea en modo de solo registro o el modo de aplicación). Establecer el comportamiento de bloqueo para iniciar sesión sólo si se ejecuta el siguiente cmdlet.  

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly` 

Modo de registro solo está pensado para ser un estado temporal para que el sistema puede aprender el comportamiento de inicio de sesión antes de la presentación de cumplimiento de bloqueo con el comportamiento de bloqueo inteligente. La duración recomendada para el modo de solo registro es de 3 a 7 días. Si las cuentas activamente están sufriendo un ataque, se debe ejecutar el modo de solo registro durante un período mínimo de 24 horas. 

En AD FS 2016, si se habilita el comportamiento de 'Bloqueo de Extranet temporal' 2012R2 antes de habilitar el bloqueo de Extranet inteligente, modo de solo registro deshabilitará el comportamiento de 'Bloqueo de Extranet temporal'. Bloqueo inteligente de AD FS no se bloqueará a los usuarios en modo de solo registro. Sin embargo, en el entorno local AD puede bloquear al usuario según la configuración de AD. Revise las directivas de bloqueo de AD para obtener información sobre cómo local AD pueden los usuarios del bloqueo. 

En AD FS 2019, una ventaja adicional es para poder habilitar el modo de solo registro de bloqueo inteligente sin dejar de aplicar el comportamiento de bloqueo temporal anterior mediante el siguiente Powershell. 

`Set-AdfsProperties -ExtranetLockoutMode 3` 
 
Para que el nuevo modo surta efecto, reinicie el servicio AD FS en todos los nodos en la granja de servidores 

`Restart-service adfssrv` 
 
Una vez configurado el modo, puede habilitar mediante el parámetro EnableExtranetLockout de bloqueo inteligente 
 
`Set-AdfsProperties -EnableExtranetLockout $true` 

### <a name="enable-enforce-mode"></a>Habilitar el modo de aplicación 
 
Después de que se siente cómodo con la ventana de observación y de umbral de bloqueo, se puede mover ESL "Aplicar" modo mediante el siguiente cmdlet PSH: 

`Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce` 

Para que el nuevo modo surta efecto, reinicie el servicio de AD FS en todos los nodos en la granja de servidores mediante el comando siguiente. 

`Restart-service adfssrv` 

## <a name="manage-user-account-activity"></a>Administrar la actividad de la cuenta de usuario 
AD FS proporciona tres cmdlets para administrar los datos de actividad de cuenta. Estos cmdlets automáticamente conectarse al nodo de la granja que contiene el rol de maestro. 
>[!NOTE] 
>Just Enough Administration (JEA) puede utilizarse para delegar los cmdlets de AD FS para restablecer los bloqueos de cuentas. Por ejemplo, personal de soporte técnico puede tener delegados permisos para usar los cmdlets de ESL. Para obtener información acerca de cómo delegar permisos para usar estos cmdlets, consulte [delegado AD FS Powershell Commandlet acceso a los usuarios sin privilegios de administrador](delegate-ad-fs-pshell-access.md)

Este comportamiento se puede invalidar pasando el parámetro - Server. 

- Get-ADFSAccountActivity 

  Lea la actividad de la cuenta actual para una cuenta de usuario. El cmdlet se conecta siempre automáticamente al maestro de granja de servidores mediante el punto de conexión de REST de la actividad de cuenta. Por lo tanto, todos los datos siempre deben ser coherentes. 

  Por ejemplo: Get-ADFSAccountActivity user@contoso.com 

  Propiedades: 
    - BadPwdCountFamiliar: Se incrementa cuando se realiza correctamente una autenticación desde una ubicación conocida.
    - BadPwdCountUnknown: Incrementa cuando se realizó correctamente una autenticación desde una ubicación desconocida
    - LastFailedAuthFamiliar: Si la autenticación se realizó correctamente desde una ubicación conocida, LastFailedAuthUnknown se establece en tiempo de autenticación incorrecta 
    - LastFailedAuthUnknown: Si la autenticación se realizó correctamente desde una ubicación desconocida, LastFailedAuthUnknown se establece en tiempo de autenticación incorrecta 
    - FamiliarLockout: Valor booleano que será "True" si el "BadPwdCountFamiliar" > ExtranetLockoutThreshold 
    - UnknownLockout: Valor booleano que será "True" si el "BadPwdCountUnknown" > ExtranetLockoutThreshold  
    - FamiliarIPs: el máximo de 20 direcciones IP que son familiares para el usuario. Cuando se supera este límite, se quitará la dirección IP más antigua en la lista. 
-    Set-ADFSAccountActivity 
     
     Agrega nuevas ubicaciones conocidas. La lista de IP familiar tiene un máximo de 20 entradas, si se supera este límite, la dirección IP más antigua en la lista se quitará. 

     Por ejemplo: Conjunto ADFSAccountActivity user@contoso.com - AdditionalFamiliarIps "1.2.3.4"

- Reset-ADFSAccountLockout 
   
  Restablece el contador de bloqueo de una cuenta de usuario para cada ubicación conocida (badPwdCountFamiliar) o los contadores de una ubicación desconocida (badPwdCountUnfamiliar). Al restablecer un contador, se actualizará el valor "FamiliarLockout" o "UnfamiliarLockout", como restablecer el contador será menor que el umbral.  

   Por ejemplo: Restablecer ADFSAccountLockout user@contoso.com -ubicación Familiar por ejemplo:  Restablecer ADFSAccountLockout user@contoso.com -ubicación desconocida 

## <a name="event-logging--user-activity-information-for-ad-fs-extranet-lockout"></a>Información de la actividad de usuario de bloqueo de Extranet de AD FS y el registro de eventos 

### <a name="connect-health"></a>Connect Health 
La manera recomendada para supervisar la actividad de la cuenta de usuario es a través de Connect Health. Connect Health genera descargable de informes en IP de riesgo e intentos con contraseñas incorrectas. Cada elemento en el informe de direcciones IP de riesgo muestra información agregada sobre AD FS inicio de sesión de las actividades con errores que sobrepasan el umbral designado. Notificaciones por correo electrónico se pueden establecer alertar a los administradores en cuanto esto sucede con la configuración de correo electrónico personalizables. Para obtener más información e instrucciones de instalación, visite la [documentación de Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-adfs). 

### <a name="ad-fs-extranet-smart-lockout-events"></a>Eventos de AD FS Extranet Lockout inteligente. 
Para los eventos de bloqueo inteligente de la Extranet se escriban, ESL debe estar habilitado en modo 'sólo registro' o 'Aplicar' y está habilitada la auditoría de seguridad de AD FS. AD FS escribirá los eventos de bloqueo de extranet en el registro de auditoría de seguridad: 
- Cuando un usuario está bloqueado fuera (alcanza el umbral de bloqueo de intentos de inicio de sesión incorrecto) 
- Cuando AD FS recibe un intento de inicio de sesión para un usuario que ya está en estado de bloqueo 

Mientras esté en modo de solo registro, puede comprobar el registro de auditoría de seguridad para los eventos de bloqueo. Para cualquier evento que se encuentra, puede comprobar el estado de usuario mediante el cmdlet Get-ADFSAccountActivity para determinar si el bloqueo se ha producido desde direcciones IP desconocidas o que ya conocidas y consultar la lista de las direcciones IP conocidas para ese usuario. 


|Id. de evento|Descripción|
|-----|-----| 
|1203|Este evento se escribe para cada intento de contraseña incorrecta. Tan pronto como el badPwdCount alcanza el valor especificado en ExtranetLockoutThreshold, la cuenta se bloqueará en AD FS para la duración especificada en ExtranetObservationWindow.</br>Id. de actividad: %1</br>XML: %2|
|1201|Este evento se escribe cada vez que un usuario está bloqueada. </br>Id. de actividad: %1</br>XML: %2| 
|557 (ADFS 2019)| Se produjo un error al intentar comunicarse con el servicio de rest de almacén de cuentas en el nodo %1. Si se trata de una granja WID el nodo principal puede estar sin conexión. Si se trata de una granja de servidores SQL ADFS seleccionará automáticamente un nuevo nodo para hospedar el rol de maestro de almacén de usuario.| 
|562 (ADFS 2019)|Se produjo un error al comunicar con la cuenta de almacenar el punto de conexión en el servidor %1.</br>Mensaje de excepción: %2| 
|563 (ADFS 2019)|Se produjo un error al calcular el estado de bloqueo de extranet. Debido a que el valor de la %1 se permitirá la autenticación de la configuración para este usuario y continuará la emisión de tokens. Si se trata de una granja WID el nodo principal puede estar sin conexión. Si se trata de una granja de servidores SQL ADFS seleccionará automáticamente un nuevo nodo para hospedar el rol de maestro de almacén de usuario.</br>Nombre del servidor de almacén de cuentas: %2</br>Id. de usuario: %3</br>Mensaje de excepción: %4|
|512|La cuenta para el usuario siguiente está bloqueada. Se permite un intento de inicio de sesión debido a la configuración del sistema.</br>Id. de actividad: %1 </br>Usuario: %2 </br>Cliente IP: %3 </br>Número de contraseña incorrecta: %4  </br>Último intento de contraseña incorrecta: %5|
|515|La siguiente cuenta de usuario se encontraba en un estado bloqueado y solo se ha proporcionado la contraseña correcta. Esta cuenta puede suponer un riesgo.</br>Datos adicionales </br>Id. de actividad: %1 </br>Usuario: %2 </br>Cliente IP: %3 |
|516|La siguiente cuenta de usuario se bloqueó debido a demasiados intentos con contraseñas incorrectas.</br>Id. de actividad: %1  </br>Usuario: %2  </br>Cliente IP: %3  </br>Número de contraseña incorrecta: %4  </br>Último intento de contraseña incorrecta: %5|

## <a name="esl-frequently-asked-questions"></a>Preguntas más frecuentes sobre ESL 
 
**¿Aplicará una granja de AD FS mediante el bloqueo inteligente Extranet en modo de bloqueos de un usuario malintencionado pueda ver?**  

R: Si el bloqueo inteligente de ADFS se establece en 'Aplicar' modo, nunca verá cuenta del usuario legítimo bloqueada por fuerza bruta o de denegación de servicio. La única forma de un bloqueo de cuenta malintencionado puede impedir que un usuario de inicio de sesión es si el actor perjudicial tiene la contraseña de usuario o puede enviar solicitudes de una dirección IP (familiar) buena conocida para ese usuario.  
 
**¿Qué ocurre ESL está habilitado y el actor perjudicial tiene una contraseña de usuario?**  

R: El objetivo del escenario de ataque por fuerza bruta típico es adivinar la contraseña y para iniciar sesión correctamente.  Si un usuario es suplantar las identidades o si se ha adivinado una contraseña, a continuación, la característica ESL no bloqueará el acceso desde el inicio de sesión cumple los criterios "correctas" de contraseña correcta más nueva dirección IP. La dirección IP de los actores no válidos, a continuación, aparecería como una "conocida". La mitigación mejor en este escenario es borrar la actividad del usuario en AD FS y requerir autenticación multifactor para los usuarios. Se recomienda encarecidamente instalar la protección con contraseña AAD que garantiza que las contraseñas de adivinar no entren en el sistema. 
 
**¿Si mi usuario nunca inició sesión correctamente desde una dirección IP y, a continuación, intenta con una contraseña incorrecta varias veces podrán iniciar sesión una vez, por último, escriba su contraseña correctamente?**  

R: Si un usuario envía varias contraseñas incorrectas (es decir, legítimamente mal al escribir) y en el intento siguiente obtiene la contraseña correcta, el usuario se realizará inmediatamente para iniciar sesión.  Esto borrará el recuento de contraseñas incorrectas y agregar esa dirección IP a la lista FamiliarIPs.  Sin embargo, si no se supera el umbral de inicios de sesión erróneos desde la ubicación desconocida, entran en estado de bloqueo y será necesario esperar más allá de la ventana de observación e inicie sesión con una contraseña válida o requieren la intervención del administrador para restablecer su cuenta.  
 
**¿ESL funciona en intranet demasiado?**    R: Si los clientes se conectan directamente a los servidores ADFS y no a través de servidores Proxy de aplicación Web no se aplicará el comportamiento de ESL.  
 
**Veo direcciones IP de Microsoft en el campo de dirección IP del cliente. ¿ESL bloque EXO procesadas por fuerza bruta ataques?**   

R: ESL funcionará bien para impedir que Exchange Online o en otros escenarios de ataque de fuerza bruta de autenticación heredados. Una autenticación heredada tiene un "identificador de actividad" de 00000000-0000-0000-0000-000000000000. En estos ataques, el actor no deseado está aprovechando las ventajas de la autenticación básica Exchange Online (también conocida como autenticación heredados) para que aparezca la dirección IP del cliente como Microsoft. Los servidores de Exchange online en el proxy en la nube la comprobación de autenticación en nombre del cliente de Outlook. En estos casos, la dirección IP del remitente malintencionado estará en la x-ms-reenviados-ip del cliente y el servidor Microsoft Exchange Online que IP estará en el valor de x-ms-client-ip. Bloqueo inteligente extranet comprueba la red IP, reenviado las direcciones IP, la x reenviados-IP del cliente y el valor de x-ms-client-ip. Si la solicitud se realiza correctamente, todas las direcciones IP se agregan a la lista familiar. Si llega una solicitud y cualquiera de las direcciones IP presentada no están en la lista conocida, a continuación, la solicitud se marcará como desconocida. El usuario conocida podrá iniciar sesión correctamente, mientras que las solicitudes desde las ubicaciones desconocidas se bloqueará.  


## <a name="additional-references"></a>Referencias adicionales  
[Procedimientos recomendados para proteger los servicios de federación de Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
