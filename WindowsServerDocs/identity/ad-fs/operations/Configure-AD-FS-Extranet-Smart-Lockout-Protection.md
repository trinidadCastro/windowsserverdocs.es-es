---
ms.assetid: 777aab65-c9c7-4dc9-a807-9ab73fac87b8
title: Configurar la protección de bloqueo de Extranet de AD FS
description: ''
author: billmath
ms.author: billmath
manager: mtilman
ms.date: 02/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 904b563da2f1404d873c7352db9eadb7bfe252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869766"
---
# <a name="ad-fs-extranet-lockout-and-extranet-smart-lockout"></a>AD FS Extranet bloqueo inteligente y bloqueo de Extranet

# <a name="overview"></a>Información general

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

En AD FS en Windows Server 2012 R2, introdujimos una característica de seguridad denominada [bloqueo temporal de la Extranet](configure-ad-fs-extranet-soft-lockout-protection.md).  Con esta característica, AD FS dejará de autenticar a los usuarios desde la extranet durante un período de tiempo.  Esto evita que las cuentas de usuario que se bloquee en Active Directory. Además de proteger a los usuarios de un bloqueo de cuenta de AD, bloqueo de extranet de AD FS también protege contra los ataques de averiguación de contraseña por fuerza bruta.

En junio de 2018, AD FS en Windows Server 2016 introdujo **bloqueo inteligente Extranet (ESL)**.  ESL permite que AD FS diferenciar entre los intentos de inicio de sesión que parecen indicar que el usuario válido e inicios de sesión de lo que puede ser un atacante. Como resultado, AD FS puede bloquear los atacantes al permitir que usuarios válidos de seguir usando sus cuentas. Esto evita la denegación de servicio en el usuario y protege contra ataques dirigidos como ataques de "aerosol de contraseña".  
ESL está disponible para AD FS en Windows Server 2016 y está integrada en AD FS en Windows Server 2019.

> [!NOTE]
> Esta característica solo funciona para la **escenario de extranet** en que las solicitudes de autenticación proceden a través del Proxy de aplicación Web y solo se aplica a **autenticación de nombre de usuario y contraseña**.

## <a name="advantages-of-extranet-smart-lockout-in-ad-fs-2016"></a>Ventajas de bloqueo inteligente de la Extranet de AD FS 2016
Bloqueo temporal de la extranet de AD FS 2012 R2 proporciona las siguientes ventajas principales:
- Protege las cuentas de usuario de **ataques por fuerza bruta** en que un atacante intenta adivinar una contraseña de usuario mediante el envío continuamente las solicitudes de autenticación y de **ataques a contraseñas aerosol** donde los atacantes intentan usar contraseñas comunes con muchas cuentas diferentes
- Protege las cuentas de usuario de **bloqueo de cuenta de Active Directory** de las solicitudes de autenticación malintencionado con contraseñas incorrectas. En este caso, aunque la cuenta de usuario se bloqueará para acceso de extranet, el usuario todavía puede iniciar sesión en AD de la red corporativa. Esto se conoce como un **bloqueo temporal**.

Bloqueo inteligente de la extranet se basa en las ventajas de bloqueo temporal de la extranet agregando lo siguiente:
- Evita que los usuarios experimentan **bloqueo de extranet cuenta** de las solicitudes de autenticación malintencionado.  Bloqueo inteligente impedirá que solicitudes potencialmente malintencionadas desde ubicaciones desconocidas permitiendo que el usuario real iniciar sesión desde ubicaciones conocidas desde la extranet (desde el que el usuario ha iniciado sesión correctamente antes de ubicaciones).
- Tiene un modo de solo registro para que el sistema puede aprender la actividad de inicio de sesión buena y potencialmente malintencionado sin necesidad de deshabilitar las cuentas

## <a name="additional-advantages-of-extranet-smart-lockout-in-ad-fs-2019"></a>Ventajas adicionales de bloqueo inteligente de la Extranet de AD FS de 2019
Bloqueo inteligente de la extranet de AD FS 2019 agrega las siguientes ventajas en comparación con AD FS 2016:
- Establecer umbrales de bloqueo independientes para las ubicaciones conocidas y desconocidas para que los usuarios en ubicaciones buenos conocidos pueden tener más espacio para el error que las solicitudes desde ubicaciones sospechosas
- Habilitar el modo de auditoría para el bloqueo inteligente sin dejar de aplicar un comportamiento de bloqueo temporal anterior

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2016"></a>Requisitos previos para el bloqueo inteligente de la Extranet de AD FS 2016
Los siguientes requisitos previos son necesarios para ESL con AD FS de 2019.

### <a name="install-updates-on-all-nodes-in-the-farm"></a>Instalar actualizaciones en todos los nodos de la granja de servidores
En primer lugar, asegúrese de todos los servidores de Windows Server 2016 AD FS están actualizados a partir de las actualizaciones de Windows de junio de 2018 y que se está ejecutando la granja de servidores de AD FS 2016 en el nivel de granja 2016 comportamiento.

### <a name="update-artifact-database-permissions"></a>Actualizar los permisos de base de datos de artefacto
Bloqueo inteligente extranet requiere la cuenta de servicio de AD FS tenga permisos para una nueva tabla en la base de datos del artefacto ADFS.  Este permiso se concede mediante el comando siguiente en una ventana de comandos de PowerShell:
``` powershell
PS C:\>$cred = Get-Credential
PS C:\>Update-AdfsArtifactDatabasePermission -Credential $cred
```
Donde `$cred` es una cuenta con permisos de administrador de AD FS (permisos de administrador de AD FS son necesarios para realizar cambios en la base de datos).

>[!NOTE]
>En una granja de servidores múltiples que usa la base de datos WID, el cmdlet anterior requiere que la administración remota de Windows esté habilitado en todos los servidores de AD FS

Si no tiene permisos de administrador de AD FS, puede configurar los permisos de base de datos manualmente en WID o SQL, ejecute el comando siguiente cuando se conecta a la base de datos AdfsArtifactStore.
```
sp_addrolemember 'db_owner', 'db_genevaservice'
```
### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Asegúrese de que está habilitado el registro de auditoría de seguridad de AD FS
Esta característica hace uso de la auditoría de seguridad de registros, por lo que la auditoría deben estar habilitados en AD FS, así como la directiva local en todos los servidores de AD FS.

## <a name="pre-requisites-for-extranet-smart-lockout-in-ad-fs-2019"></a>Requisitos previos para el bloqueo inteligente de la Extranet de AD FS de 2019
Los siguientes requisitos previos son necesarios para ESL con AD FS 2016.

### <a name="ensure-ad-fs-security-audit-logging-is-enabled"></a>Asegúrese de que está habilitado el registro de auditoría de seguridad de AD FS
Esta característica hace uso de la auditoría de seguridad de registros, por lo que la auditoría deben estar habilitados en AD FS, así como la directiva local en todos los servidores de AD FS.

## <a name="lockout-settings"></a>Configuración de bloqueo
Bloqueo inteligente extranet consta de un conjunto de nuevas capacidades que se rige por las propiedades de AD FS nuevas y existentes.

### <a name="extranet-lockout-enabled"></a>Habilitada el bloqueo de extranet
Bloqueo inteligente extranet usa la misma propiedad de AD FS que se usó previamente para controlar el bloqueo de extranet "soft".  La propiedad se denomina ExtranetLockoutEnabled y puede verse a través de Get-AdfsProperties.

### <a name="extranet-smart-lockout-mode"></a>Modo de bloqueo inteligente de extranet
Se agregó una nueva propiedad de AD FS denominada ExtranetLockoutMode para controlar el comportamiento de bloqueo "soft" vs inteligente.  Se pueden establecer mediante Set-AdfsProperties y contiene 3 valores:

    - **ADPasswordCounter** – es heredado en modo de "bloqueo de extranet temporal" de ADFS que no distingue según la ubicación.  Este es el valor predeterminado.

    - **ADFSSmartLockoutLogOnly** : se trata de Extranet de bloqueo inteligente, pero en vez de rechazar las solicitudes de autenticación, AD FS le solo administración y auditoría de eventos de escritura.

    - **ADFSSmartLockoutEnforce** -se trata de Extranet de bloqueo inteligente con compatibilidad total para bloquear solicitudes familiarizadas cuando se alcanzan los umbrales.

En AD FS 2019, se pueden combinar los valores ADPasswordCounter y ADFSSmartLockoutLogOnly por lo que el bloqueo temporal se sigue aplicando mientras se está preparando para el bloqueo inteligente.

### <a name="lockout-threshold-and-observation-window"></a>Umbral de bloqueo y la ventana de observación
Bloqueo inteligente de AD FS 2019 usa el mismo dos propiedades de AD FS como el bloqueo temporal usado anteriormente: ExtranetObservationWindow y ExtranetLockoutThreshold.

- **ExtranetLockoutThreshold &lt;entero&gt;**  define el número máximo de intentos con contraseñas incorrectas. Una vez que se alcanza el umbral, en ADFSSmartLockoutEnforce modo AD FS rechazará las solicitudes desde la extranet hasta que haya transcurrido la ventana de observación.  En el modo de ADFSSmartLockoutLogOnly, AD FS escribirá las entradas de registro.  
- **ExtranetObservationWindow &lt;TimeSpan&gt;**  determina cuánto nombre de usuario y contraseña se bloqueará las solicitudes desde ubicaciones desconocidas. AD FS se iniciará realizar la autenticación de nombre de usuario y contraseña de nuevo cuando se pasa a la ventana.

> [!NOTE]
> Funciones de bloqueo de extranet de AD FS independientemente de las directivas de bloqueo de AD. Se recomienda que establezca el **ExtranetLockoutThreshold** valor del parámetro en un valor que es menor que el umbral de bloqueo de cuentas de AD. Lo contrario, daría como resultado en AD FS no puede impedir que las cuentas que se bloquee en Active Directory. 

En AD FS 2019, hemos introducido un nuevo umbral de bloqueo específico a ubicaciones buena conocidas: ExtranetLockoutThresholdFamiliarLocation.
- **ExtranetLockoutThresholdFamiliarLocation &lt;entero&gt;**  define el número máximo de intentos con contraseñas incorrectas desde ubicaciones conocidas. En AD FS 2019, el parámetro original ExtranetLockoutThreshold se aplica a las ubicaciones desconocidas (las direcciones IP no conocidas sea bueno).

### <a name="primary-domain-controller-requirement"></a>Requisito de controlador de dominio principal
AD FS 2016 ofrece un parámetro que permite volver a otro controlador de dominio al controlador de dominio principal no está disponible.

- **ExtranetLockoutRequirePDC &lt;booleano&gt;**  cuando se habilita, el bloqueo de extranet requiere un controlador de dominio principal (PDC). Cuando está deshabilitado, bloqueo de extranet retrocederá para otro controlador de dominio en caso de que el PDC no está disponible.

   El ejemplo siguiente muestra el cmdlet para habilitar el bloqueo con el requisito de PDC deshabilitado:

    ```powershell
    Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow (new-timespan -Minutes 30) -ExtranetLockoutRequirePDC $false
    ```

## <a name="configuring-ad-fs-with-smart-lockout-in-log-only-mode"></a>Configuración de AD FS con bloqueo inteligente en modo de solo registro

### <a name="ad-fs-2016"></a>AD FS 2016
Se recomienda establecer el comportamiento de bloqueo para iniciar sesión sólo si se ejecuta el siguiente cmdlet:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartlockoutLogOnly
 ```

En este modo, AD FS rellena la información de ubicación familiar a los usuarios y escribe los eventos de auditoría de seguridad, pero no bloquea las solicitudes.  Este modo se utiliza para validar que se está ejecutando el bloqueo inteligente y para habilitar AD FS para "aprender" ubicaciones conocidas para los usuarios antes de habilitar el "modo de aplicación".
Tal como se entera de AD FS, almacena la actividad de inicio de sesión por usuario (ya sea en modo de solo registro o el modo de aplicación). 

>[!NOTE]
>Configurar `ExtranetLockoutMode` a `AdfsSmartlockoutLogOnly` hará que el comportamiento heredado de FS "suave bloqueos de extranet" AD ya no estará en vigor, incluso si la `EnableExtranetLockout` propiedad está establecida en True.  Esto significa que no se tendrán acceso los usuarios que superan los umbrales de bloqueo desde direcciones IP desconocidas o que ya conocidas por bloqueo inteligente de AD FS. Sin embargo, en el entorno local AD, es posible que el usuario según la configuración de bloqueo.   Consulte [directiva de bloqueo de cuenta](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/account-lockout-policy) para obtener información sobre cómo local AD pueden los usuarios de bloqueo. "  Esto está pensado para ser un estado temporal para que el sistema puede aprender el comportamiento de inicio de sesión antes de volver a introducir el cumplimiento de bloqueo con el nuevo comportamiento de bloqueo inteligente.

Para que el nuevo modo surta efecto, reinicie el servicio AD FS en todos los nodos en la granja de servidores
  
  ``` powershell
PS C:\>Restart-service adfssrv
  ```
Una vez configurado el modo, puede habilitar el bloqueo inteligente usando la `EnableExtranetLockout` parámetro


``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $true
```

Tenga en cuenta que puede usar el mismo cmdlet para deshabilitar el bloqueo

Por ejemplo: Deshabilitar bloqueo

``` powershell
PS C:\>Set-AdfsProperties -EnableExtranetLockout $false
```
### <a name="ad-fs-2019"></a>AD FS DE 2019
Si actualmente no usa AD FS Extranet Lockout temporal, se recomienda que siga las mismas instrucciones que para AD FS 2016 anterior.
Si usas un bloqueo temporal, sin embargo, recomendamos que establezca el comportamiento de bloqueo de AD FS de 2019 para iniciar sesión para el bloqueo inteligente, pero tenga que aplicar un bloqueo temporal, con el siguiente powershell:

 ```powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode 3
 ```

Una vez que ejecute este cmdlet, a continuación, puede usar Get-AdfsProperties para consultar el valor de la propiedad ExtranetLockoutMode AD FS.  Verá que su valor se ha actualizado a una combinación bit a bit de ADPasswordCounter y ADFSSmartLockoutLogOnly.

## <a name="observing-audit-events"></a>Observa los eventos de auditoría
AD FS escribirá los eventos de bloqueo de extranet en el registro de auditoría de seguridad:
-   Cuando un usuario está bloqueado fuera (alcanza el umbral de bloqueo de intentos de inicio de sesión incorrecto)
-   Cuando AD FS recibe un intento de inicio de sesión para un usuario que ya está en estado de bloqueo

Mientras esté en modo de solo registro, puede comprobar el registro de auditoría de seguridad para los eventos de bloqueo.  Para cualquier evento que se encuentra, puede comprobar el estado de usuario mediante el cmdlet Get-ADFSAccountActivity para determinar si el bloqueo se ha producido desde direcciones IP desconocidas o que ya conocidas y consultar la lista de las direcciones IP conocidas para ese usuario.

Evento de ejemplo:
```
Log Name:      Security
Source:        AD FS Auditing
Date:          5/21/2018 12:55:59 AM
Event ID:      1210
Task Category: (3)
Level:         Information
Keywords:      Classic,Audit Failure
User:          CONTOSO\adfssvc
Computer:      ADFS2016FS1.corp.contoso.com
Description:
An extranet lockout event has occurred. See XML for failure details. 

Activity ID: fa7a8052-0694-48f0-84e2-b51cde40ac3d 

Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>
Event Xml:
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="AD FS Auditing" />
    <EventID Qualifiers="0">1210</EventID>
    <Level>0</Level>
    <Task>3</Task>
    <Keywords>0x8090000000000000</Keywords>
    <TimeCreated SystemTime="2018-05-21T00:55:59.921880300Z" />
    <EventRecordID>35521235</EventRecordID>
    <Channel>Security</Channel>
    <Computer>ADFS2016FS1.contoso.com</Computer>
    <Security UserID="S-1-5-21-1156273042-1594504307-2076964089-1104" />
  </System>
  <EventData>
    <Data>fa7a8052-0694-48f0-84e2-b51cde40ac3d</Data>
    <Data><?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://fs.contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>CONTOSO\user</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Extranet</NetworkLocation>
      <IpAddress>64.187.173.10</IpAddress>
      <ForwardedIpAddress>64.187.173.10</ForwardedIpAddress>
      <ProxyIpAddress>N/A</ProxyIpAddress>
      <NetworkIpAddress>N/A</NetworkIpAddress>
      <ProxyServer>ADFS2016PROXY2</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls/</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>05/21/2018 00:55:05</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase></Data>
  </EventData>
</Event>
```

## <a name="observing-user-activity"></a>Observar la actividad del usuario
AD FS proporciona cmdlets de powershell para ver y administrar datos de actividad de cuentas de usuario.  Para leer la actividad de la cuenta actual para una cuenta de usuario.  Use el cmdlet siguiente

``` powershell
PS C:\>Get-ADFSAccountActivity user@contoso.com
```

Salida de ejemplo
```
Identifier             : CONTOSO\user
BadPwdCountFamiliar    : 0
BadPwdCountUnknown     : 0
LastFailedAuthFamiliar : 1/1/0001 12:00:00 AM
LastFailedAuthUnknown  : 1/1/0001 12:00:00 AM
FamiliarLockout        : False
UnknownLockout         : False
FamiliarIps            : {}
```

El resultado de la actividad actual contiene los datos siguientes:

**Identificador**: este es el nombre de usuario

**BadPwdCountFamiliar**: éste es el recuento actual de intentos de inicio de sesión de contraseña incorrecta de direcciones IP que estaban en la lista de "FamiliarIps" en el momento del intento de

**BadPwdCountUnknown**: éste es el recuento actual de intentos de inicio de sesión de contraseña incorrecta de direcciones IP que no estaban en la lista de "FamiliarIps" en el momento del intento de

**LastFailedAuthFamiliar**: se trata de la hora del último intento de inicio de sesión de contraseña incorrecta desde una dirección IP que estaba en la lista de "FamiliarIps" en el momento del intento

**LastFailedAuthUnknown**: se trata de la hora del último intento de inicio de sesión de contraseña incorrecta desde una dirección IP que no estaba en la lista de "FamiliarIps" en el momento del intento

**FamiliarLockout**: indica si el usuario está en un estado de bloqueo para intentos de contraseña correcta desde direcciones IP en la lista de "FamiliarIps" 

**UnknownLockout**: indica si el usuario está en un estado de bloqueo para intentos de contraseña correcta desde direcciones IP no están en la lista de "FamiliarIps" FamiliarIps: esta es la lista de direcciones IP conocidas del usuario actual

## <a name="adjust-threshold-and-window"></a>Ajustar el umbral y la ventana
Una vez que se ejecuta en modo de solo registro durante un período suficiente tiempo para AD FS obtener información sobre las ubicaciones de inicio de sesión, es posible que desee ajustar la ventana de observación o el umbral de la configuración predeterminada.  Esto se realiza mediante `Set-AdfsProperties` como se muestra en los ejemplos siguientes:

Se establece la ventana de observación mediante `ExtranetObservationWindow`:

Por ejemplo: 

``` powershell
PS C:\>Set-AdfsProperties -ExtranetObservationWindow ( new-timespan -minutes 30 )
```

Donde el valor es un intervalo de tiempo

### <a name="setting-threshold-value-in-ad-fs-2016"></a>Valor de umbral de configuración de AD FS 2016
En AD FS 2016, el umbral se establece mediante ExtranetLockoutThreshold:

Por ejemplo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

### <a name="setting-threshold-values-in-ad-fs-2019"></a>Establecer valores de umbral de AD FS de 2019
En AD FS 2019, hay valores de umbral distintos para ubicaciones desconocidas y buena conocidos

Para establecer el valor de umbral de ubicaciones desconocidas, utilice la misma propiedad que se usa para AD FS 2016 anteriormente:

Por ejemplo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThreshold 5
```

Para establecer el valor de umbral para las ubicaciones buena conocidas, use la nueva propiedad ExtranetLockoutThresholdFamiliarLocation, tal como se muestra en el ejemplo siguiente:

Por ejemplo:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutThresholdFamiliarLocation 10
```


## <a name="enable-enforce-mode"></a>Habilitar el modo de aplicación
Una vez que ha estado ejecutando en modo de solo registro suficiente tiempo para AD FS para obtener información sobre las ubicaciones de inicio de sesión y para observar cualquier actividad de bloqueo, y una vez que esté familiarizado con la ventana de observación y de umbral de bloqueo, bloqueo inteligente puede moverse a "Aplicar" modo mediante el Cmdlet PSH siguiente:

``` powershell
PS C:\>Set-AdfsProperties -ExtranetLockoutMode AdfsSmartLockoutEnforce
```

Para que el nuevo modo surta efecto, reinicie el servicio AD FS en todos los nodos en la granja de servidores

``` powershell
PS C:\>Restart-service adfssrv
```


## <a name="manage-user-account-activity"></a>Administrar la actividad de la cuenta de usuario
AD FS proporciona 3 cmdlets para administrar datos de actividad de cuentas de usuario.  Estos cmdlets conectarse automáticamente al nodo de la granja de servidores que contiene el rol de maestro (aunque este comportamiento se puede invalidar pasando el parámetro - Server).

> [!NOTE] 
> Para obtener información acerca de cómo delegar permisos para usar estos cmdlets, consulte [delegado AD FS Powershell Commandlet acceso a los usuarios sin privilegios de administrador](delegate-ad-fs-pshell-access.md)

Estos cmdlets son:

`Get-ADFSAccountActivity`

Lea la actividad de la cuenta actual para una cuenta de usuario.  El cmdlet se conecta siempre automáticamente con el patrón de granja de servidores mediante el extremo REST de la actividad de cuenta, por lo que todos los datos siempre deben ser coherentes

``` powershell
Get-ADFSAccountActivity user@contoso.com
```
`
Set-ADFSAccountActivity
`

Actualizar la actividad de la cuenta para una cuenta de usuario.  Esto puede usarse para agregar nuevas ubicaciones conocidas o borrar el estado de cualquier cuenta

``` powershell
Set-ADFSAccountActivity user@upnsuffix.com -FamiliarLocation “1.2.3.4”
```
`Reset-ADFSAccountLockout`

Restablece el contador de bloqueo para una cuenta de usuario

``` powershell
Reset-ADFSAccountLockout user@upnsuffix.com -Familiar
```

## <a name="troubleshooting-esl"></a>Solución de problemas de ESL
El siguiente puede ayudarle a solucionar problemas de la característica de bloqueo inteligente de la extranet.

### <a name="updating-database-permissions-for-esl"></a>Actualizando permisos de base de datos para ESL
Si los errores se devuelven desde el `Update-AdfsArtifactDatabasePermission` cmdlet, compruebe lo siguiente

1.  La lista de nodos de la granja es correcta.  Si un nodo está en la lista de la granja de servidores de AD FS, pero ya no se producirá un error de comprobación de revisión activa.  Esto puede resolverse mediante la ejecución `remove-adfsnode <node name >`
2.  Compruebe que la revisión se implementa en todos los nodos de la granja de servidores
3.  Compruebe las credenciales que se pasa al cmdlet tienen permiso para modificar el propietario del esquema de base de datos de ad fs artefacto.  

### <a name="logging--auditing"></a>Registro y auditoría
Cuando se rechaza una solicitud de autenticación porque la cuenta supera el umbral de bloqueo, AD FS se escribirá un `ExtranetLockoutEvent` en la secuencia de auditoría de seguridad.  

Evento de ejemplo:

Se ha producido un evento de bloqueo de extranet. Ver el XML para obtener información detallada del error. 

**Id. de actividad: 172332e1-1301-4e56-0e00-0080000000db**

```
Additional Data 
XML: <?xml version="1.0" encoding="utf-16"?>
<AuditBase xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="ExtranetLockoutAudit">
  <AuditType>ExtranetLockout</AuditType>
  <AuditResult>Failure</AuditResult>
  <FailureType>ExtranetLockoutError</FailureType>
  <ErrorCode>AccountRestrictedAudit</ErrorCode>
  <ContextComponents>
    <Component xsi:type="ResourceAuditComponent">
      <RelyingParty>http://contoso.com/adfs/services/trust</RelyingParty>
      <ClaimsProvider>N/A</ClaimsProvider>
      <UserId>TQDFTD\Administrator</UserId>
    </Component>
    <Component xsi:type="RequestAuditComponent">
      <Server>N/A</Server>
      <AuthProtocol>WSFederation</AuthProtocol>
      <NetworkLocation>Intranet</NetworkLocation>
      <IpAddress>4.4.4.4</IpAddress>
      <ForwardedIpAddress />
      <ProxyIpAddress>1.2.3.4</ProxyIpAddress>
      <NetworkIpAddress>1.2.3.4</NetworkIpAddress>
      <ProxyServer>N/A</ProxyServer>
      <UserAgentString>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36</UserAgentString>
      <Endpoint>/adfs/ls</Endpoint>
    </Component>
    <Component xsi:type="LockoutConfigAuditComponent">
      <CurrentBadPasswordCount>5</CurrentBadPasswordCount>
      <ConfigBadPasswordCount>5</ConfigBadPasswordCount>
      <LastBadAttempt>02/07/2018 21:47:44</LastBadAttempt>
      <LockoutWindowConfig>00:30:00</LockoutWindowConfig>
    </Component>
  </ContextComponents>
</AuditBase>

```

## <a name="banned-ip-addresses"></a>Direcciones IP no permitidas
Además de las funcionalidades de bloqueo inteligente de extranet, la actualización de junio de 2018 de AD FS le permite configurar un conjunto de direcciones IP global en AD FS, para que las solicitudes procedentes de esas direcciones IP, o que tengan esas direcciones IP el **x-forwarded-for**  o **x-ms-reenviados-client-ip** encabezados, se bloqueará por AD FS.

##### <a name="adding-banned-ips"></a>Adición de direcciones IP no permitidas
Para agregar direcciones IP no permitidas en la lista global, use el siguiente cmdlet de Powershell:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formatos permitidos

1.  IPv4
2.  IPv6
3.  Formato CIDR con IPv4 o v6
4.  Intervalo de IP con IPv4 o v6 (es decir, 1.2.3.4-1.2.3.6)

#### <a name="removing-banned-ips"></a>Eliminación de direcciones IP no permitidas
Para quitar direcciones IP no permitidas en la lista global, use el siguiente cmdlet de Powershell:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Leer direcciones IP no permitidas
Para leer el conjunto actual de direcciones IP no permitidas, use el siguiente cmdlet de Powershell:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Ejemplo de resultado:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Referencias adicionales  
[Procedimientos recomendados para proteger los servicios de federación de Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)

    
