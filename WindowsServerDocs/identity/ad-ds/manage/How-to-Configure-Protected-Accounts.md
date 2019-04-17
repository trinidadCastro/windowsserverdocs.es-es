---
ms.assetid: 70c99703-ff0d-4278-9629-b8493b43c833
title: "Cómo configurar cuentas protegidas"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4de7b1c9e3d12556f67c4515467bccd124e5e73e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-configure-protected-accounts"></a>Cómo configurar cuentas protegidas

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A través de ataques Pass-the-hash (PtH), un atacante puede autenticar a un servicio o un servidor remoto mediante el hash NTLM subyacente de una contraseña de usuario (o demás derivados de la caja). Microsoft ha anteriormente [publicado instrucciones](https://www.microsoft.com/download/details.aspx?id=36036) para mitigar los ataques pass-the-hash.  Windows Server 2012 R2 incluye nuevas características para ayudar a mitigar estos ataques aún más. Para obtener más información sobre otras características de seguridad que ayudan a proteger contra el robo de credenciales, consulta [protección de credenciales y administración](https://technet.microsoft.com/library/dn408190.aspx). En este tema se explica cómo configurar las siguientes características nuevas:

-   [Usuarios protegidos](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_AddtoProtectedUsers)

-   [Directivas de autenticación](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicies)

-   [Silos de directiva de autenticación](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicySilos)

Hay mitigaciones adicionales integradas en Windows 8.1 y Windows Server 2012 R2 para ayudar a proteger contra el robo de credenciales, que se tratan en los siguientes temas:

-   [Modo de administrador restringido para escritorio remoto](http://blogs.technet.com/b/kfalde/archive/2013/08/14/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)

-   [Protección de LSA](https://technet.microsoft.com/library/dn408187)

## <a name="BKMK_AddtoProtectedUsers"></a>Usuarios protegidos
Los usuarios protegidos es un nuevo grupo de seguridad global a los que puedes agregar usuarios nuevos o existentes. Dispositivos de Windows 8.1 y Windows Server 2012 R2 hosts tienen un comportamiento especial con miembros de este grupo para proporcionar una mejor protección contra el robo de credenciales. Para un miembro del grupo, un dispositivo de Windows 8.1 o un host de Windows Server 2012 R2 no almacena en caché las credenciales que no se admiten para los usuarios protegido. Los miembros de este grupo tienen protección adicional si inician sesión en un dispositivo que ejecuta una versión de Windows anteriores a Windows 8.1.

Miembros de los usuarios protegido de grupo que están firmados en dispositivos de Windows 8.1 y Windows Server 2012 R2 hosts pueden *ya no* usar:

-   Valor predeterminado de la delegación de credenciales (CredSSP) - texto no cifrado no se almacenan en caché las credenciales incluso cuando la **Permitir delegación de credenciales predeterminadas** directiva está habilitada

-   Resumen de Windows - texto no cifrado credenciales no están almacenados en caché incluso cuando están habilitados

-   No se almacena en caché de NTLM - NTOWF

-   Teclas de Kerberos a largo plazo - Kerberos vale de concesión de vales (TGT) se adquieren al inicio de sesión y no se vuelva a adquirir automáticamente

-   Inicio de sesión sin conexión: el Comprobador de inicio de sesión almacenada en caché no se ha creado

Si el nivel funcional del dominio es Windows Server 2012 R2, los miembros del grupo no pueden:

-   Autenticar mediante la autenticación NTLM

-   Usa el estándar de cifrado de datos (DES) o RC4 conjuntos de cifrado en la autenticación previa de Kerberos

-   Delegarse mediante el uso de la delegación restringida o sin restricciones

-   Renovación de vales (TGT) de usuario más allá de la duración de 4 horas inicial

Para agregar usuarios al grupo, puedes usar [herramientas de interfaz de usuario](https://technet.microsoft.com/library/cc753515.aspx) como Active Directory administrativas centro (ADAC) o los usuarios de Active Directory y los equipos o una herramienta de línea de comandos, como [Dsmod group](https://technet.microsoft.com/library/cc732423.aspx), o el de Windows PowerShell[agregar ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet. Cuentas de servicios y equipos *no debería* sean miembros del grupo usuarios protegido. Pertenencia a esas cuentas no proporciona ninguna protecciones locales porque la contraseña o certificado siempre está disponible en el equipo host.

> [!WARNING]
> Las restricciones de autenticación no tienen ninguna solución alternativa, lo que significa que los miembros de grupos con privilegios elevados como el grupo de administradores de empresa o el grupo de administradores de dominio están sujetas a las mismas restricciones de los otros miembros del grupo usuarios protegido. Si todos los miembros de dichos grupos se agregan al grupo usuarios protegidos, es posible para todas las cuentas se bloquee. Nunca debes agregar todas las cuentas con privilegios elevados al grupo usuarios protegida hasta que has probado exhaustivamente los posibles efectos.

Los miembros del grupo usuarios protegido deben poder autenticarse con Kerberos con los estándares de cifrado avanzado (AES). Este método requiere claves AES de la cuenta en Active Directory. El Administrador integrado no tiene una clave AES a menos que la contraseña se ha cambiado en un controlador de dominio que ejecute Windows Server 2008 o posterior. Además, cualquier cuenta, que tiene una contraseña que se ha cambiado en un controlador de dominio que se ejecuta una versión anterior de Windows Server, está bloqueada. Por lo tanto, sigue estos procedimientos recomendados:

-   No probar en dominios, a menos que **todos los controladores de dominio ejecutan Windows Server 2008 o posterior**.

-   **Cambiar contraseña** para todas las cuentas de dominio que se crearon *antes* creó el dominio. De lo contrario, no se puede autenticar estas cuentas.

-   **Cambiar contraseña** para cada usuario antes de agregar la cuenta a los usuarios protegido de grupo o asegurarse de que la contraseña se ha modificado recientemente en un controlador de dominio que ejecute Windows Server 2008 o posterior.

### <a name="BKMK_Prereq"></a>Requisitos para usar cuentas protegidas
Cuentas protegidas tienen los siguientes requisitos de implementación:

-   Para proporcionar las restricciones de cliente para que los usuarios protegido, hosts deben ejecutar Windows 8.1 o Windows Server 2012 R2. Un usuario solo se tiene que iniciar sesión con una cuenta que es un miembro de un grupo de usuarios protegido. En este caso, puede crearse el grupo usuarios protegido por [transferir el rol de emulador de dominio principal (PDC) del controlador](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a un controlador de dominio que ejecute Windows Server 2012 R2. Después de ese objeto de grupo se replica en otros controladores de dominio, el rol de emulador PDC puede estar alojado en un controlador de dominio que ejecute una versión anterior de Windows Server.

-   Para proporcionar restricciones del lado del controlador de dominio para los usuarios protegidos, es decir restringir el uso de autenticación NTLM, y otras restricciones, el nivel funcional del dominio debe ser Windows Server 2012 R2. Para obtener más información sobre niveles funcionales, consulta [niveles funcionales de conocimiento Active Directory Domain Services (AD DS)](../active-directory-functional-levels.md).

### <a name="BKMK_TrubleshootingEvents"></a>Solucionar problemas de eventos relacionados con los usuarios protegido
Esta sección tratan los registros de nuevo para ayudar a solucionar los eventos que están relacionados con los usuarios protegido y cómo pueden afectar a los usuarios protegido cambios para solucionar cualquier problemas de expiración o delegación de vales (TGT) de concesión de vales.

#### <a name="new-logs-for-protected-users"></a>Registros de nuevo para los usuarios protegido

Dos nuevas administrativas los registros operativos están disponibles para ayudar a solucionar los eventos que están relacionados con los usuarios protegido: registro de controlador de dominio de usuario protegido (registro del cliente y protegido errores del usuario). Estos registros nuevas se encuentran en el Visor de eventos y están deshabilitados de manera predeterminada. Para habilitar un registro, haz clic en **registros de aplicaciones y servicios**, haz clic en **Microsoft**, haz clic en **Windows**, haz clic en **autenticación**y, a continuación, haz clic en el nombre del registro y haga clic en **acción** (o haz clic en el registro) y haz clic en **Habilitar registro**.

Para obtener más información acerca de estos registros de eventos, consulta [Silos de directiva de autenticación y directivas de autenticación](https://technet.microsoft.com/library/dn486813.aspx).

#### <a name="troubleshoot-tgt-expiration"></a>Solucionar problemas de expiración TGT
Normalmente, el controlador de dominio establece la duración TGT y renovación en función de la directiva de dominio, como se muestra en la siguiente ventana del Editor de administración de directivas de grupo.

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

Para **protegido usuarios**, las siguientes opciones están codificadas:

-   Vigencia máxima del vale de usuario: 240 minutos

-   Vigencia máxima de renovación de vales de usuario: 240 minutos

#### <a name="troubleshoot-delegation-issues"></a>Solucionar problemas de delegación
Anteriormente, si se presenta una tecnología que utiliza la delegación Kerberos, la cuenta del cliente se comprueba para ver si **cuenta es confidencial y no se puede delegar** se estableció. Sin embargo, si la cuenta es miembro de **protegido usuarios**, no podría tener este valor configurado en Active Directory administrativas centro (ADAC). Como resultado, compruebe la configuración y la pertenencia al grupo para solucionar problemas de la delegación.

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

### <a name="BKMK_AuditAuthNattempts"></a>Auditar los intentos de autenticación
Para auditar los intentos de autenticación de forma explícita para los miembros de la **protegido usuarios** grupo, puedes seguir recopilar eventos de auditoría de registro de seguridad o recopilar los datos en los nuevos registros operativos administrativos. Para obtener más información sobre estos eventos, consulta [Silos de directiva de autenticación y directivas de autenticación](https://technet.microsoft.com/library/dn486813.aspx)

### <a name="BKMK_ProvidePUdcProtections"></a>Proporcionar protecciones de DC lado para equipos y servicios
Cuentas de servicios y equipos no pueden ser miembros de **protegido usuarios**. Esta sección explica qué protecciones basado en el controlador de dominio pueden comprarse por estas cuentas:

-   Rechazar la autenticación NTLM: solo puede configurarse mediante [directivas de bloqueo NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

-   Rechazar estándar de cifrado de datos (DES) en la autenticación previa de Kerberos: controladores de dominio de Windows Server 2012 R2 no los acepta DES para las cuentas de equipo a menos que están configurados para DES solo porque cada versión de Windows con Kerberos también admite el cifrado RC4.

-   Rechazar RC4 en la autenticación previa de Kerberos: no se puede configurar.

    > [!NOTE]
    > Aunque es posible [cambiar la configuración de tipos de cifrado compatibles](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx), no se recomienda cambiar esta configuración de cuentas de equipo sin realizar pruebas en el entorno de destino.

-   Restringir los vales (TGT) de usuario a una duración de 4 horas inicial: directivas de autenticación de uso.

-   Denegar delegación con la delegación restringida o sin restricciones: para restringir una cuenta, abre el centro de administración de directorio Active (ADAC) y selecciona la **cuenta es confidencial y no se puede delegar** casilla de verificación.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

## <a name="BKMK_CreateAuthNPolicies"></a>Directivas de autenticación
Directivas de autenticación es un contenedor nuevo en AD DS que contiene los objetos de directiva de autenticación. Especifican opciones de configuración que ayudan a reducir la exposición a robo de credenciales, como la restricción de vigencia TGT de cuentas o agregar otras condiciones reclamación relacionada con directivas de autenticación.

En Windows Server 2012, Control de acceso dinámico incorporó una clase de objeto de ámbito del bosque de Active Directory llamada la directiva de acceso Central para proporcionar una forma fácil de configurar los servidores de archivos a través de una organización. En Windows Server 2012 R2, una nueva clase de objeto denominada directiva de autenticación (objectClass msDS-AuthNPolicies) puede usarse para aplicar la configuración de autenticación a las clases de la cuenta en los dominios de Windows Server 2012 R2. Clases de cuenta de Active Directory son:

-   Usuario

-   Equipo

-   Cuenta de servicio administrada y grupo de cuenta de servicio administrada (GMSA)

### <a name="quick-kerberos-refresher"></a>Rápido repaso de Kerberos
El protocolo de autenticación de Kerberos consta de tres tipos de intercambios, también conocidos como subprotocols:

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbRefresher.gif)

-   El intercambio de autenticación (AS) de servicio (KRB_AS_ *)

-   El intercambio de servicio (TGS) concesión de vales (KRB_TGS_ *)

-   El intercambio de cliente/servidor (AP) (KRB_AP_ *)

El intercambio de AS es que el cliente usa la contraseña o la clave privada de la cuenta para crear un autenticador previo para solicitar un vale de concesión de vales (TGT). Esto sucede en el inicio de sesión de usuario o la primera vez que se necesita un vale de servicio.

El intercambio TGS es donde TGT de la cuenta se usa para crear un autenticador para solicitar un vale de servicio. Esto sucede cuando se necesita una conexión autenticada.

El intercambio de punto de acceso se produce como normalmente como datos de protocolo de aplicación y no se ve afectado por las directivas de autenticación.

Para obtener más información, consulta [Kerberos versión 5 autenticación protocolo funcionamiento del](https://technet.microsoft.com/library/cc772815(v=WS.10).aspx).

### <a name="overview"></a>Introducción
Directivas de autenticación complementan protegido usuarios proporcionando una manera para aplicar restricciones configurables a las cuentas y proporcionando restricciones para las cuentas de servicios y equipos. Se aplican directivas de autenticación durante el intercambio de AS o de TGS exchange.

Puedes restringir la autenticación inicial o el intercambio como mediante la configuración de:

-   Una duración TGT

-   Condiciones de control de acceso para restringir el usuario inicio de sesión único, que debe cumplirse por dispositivos desde el que proviene el intercambio como

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictAS.gif)

Puedes restringir las solicitudes de vales de servicio a través de un intercambio concesión de vales de servicio (TGS) mediante la configuración:

-   Condiciones de control de acceso que deben cumplirse por el cliente (usuario, servicio, equipo) o el dispositivo del que proviene el intercambio TGS

### <a name="BKMK_ReqForAuthnPolicies"></a>Requisitos para usar directivas de autenticación

|Directiva|Requisitos|
|----------|----------------|
|Proporcionar duraciones TGT personalizados| Dominios de cuentas de nivel funcional de dominio de Windows Server 2012 R2|
|Restringir el inicio de sesión de usuario|-Dominios de cuenta nivel funcional de dominio Windows Server 2012 R2 con soporte técnico de Control de acceso dinámico<br />-Admitan Windows 8, Windows 8.1, Windows Server 2012 o dispositivos de Windows Server 2012 R2 con Control de acceso dinámico|
|Restringir emisión de vales de servicio que se basa en los grupos de seguridad y la cuenta de usuario| Dominios de recursos en el nivel funcional de dominio de Windows Server 2012 R2|
|Restringir emisión de vale de servicio en función de notificaciones de usuario o la cuenta del dispositivo, grupos de seguridad o reclamaciones| Dominios de recursos en el nivel funcional de dominio de Windows Server 2012 R2 con el soporte técnico de Control de acceso dinámico|

### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>Restringir una cuenta de usuario para dispositivos específicos y los hosts
Una cuenta de gran valor con privilegios administrativos debe ser un miembro de la **protegido usuarios** grupo. De manera predeterminada, no hay cuentas son miembros de la **protegido usuarios** grupo. Antes de agregar cuentas al grupo, configurar la compatibilidad con controladores de dominio y crear una directiva de auditoría para garantizar que no haya ningún problemas de bloqueo.

#### <a name="configure-domain-controller-support"></a>Configurar la compatibilidad de controlador de dominio

Dominio de la cuenta del usuario debe estar en el nivel funcional del dominio de Windows Server 2012 R2 (DFL). Asegúrese de todos los controladores de dominio son Windows Server 2012 R2 y, a continuación, usan dominios de Active Directory y relaciones de confianza [generar el DFL](https://technet.microsoft.com/library/cc753104.aspx) a Windows Server 2012 R2.

**Para configurar la compatibilidad con Control de acceso dinámico**

1.  En la directiva de controladores de dominio predeterminada, haz clic en **habilitado** para habilitar **centro de distribución de claves (KDC) compatibilidad del cliente con reclamaciones, autenticación compuesta y protección de Kerberos** en configuración del equipo | Plantillas administrativas | Sistema | KDC.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)

2.  En **opciones**, en el cuadro de lista desplegable, selecciona **siempre proporcionar notificaciones**.

    > [!NOTE]
    > **Admite** también pueden configurarse, pero debido a que el dominio es en Windows Server 2012 R2 DFL, tener los controladores de dominio siempre proporcionar notificaciones permitirá hospeda para conectarse a servicios de notificaciones y protege producirse cuando se usan los dispositivos no reclamaciones de acceso de usuario basada en notificaciones.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)

    > [!WARNING]
    > Configurar **producirá un error en las solicitudes de autenticación unarmored** dará como resultado errores de autenticación desde cualquier sistema operativo que no es compatible con la protección de Kerberos, como Windows 7 y sistemas operativos anteriores, o sistemas operativos a partir de Windows 8, que no se ha configurado explícitamente para que lo admiten.

#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>Crear una auditoría de la cuenta de usuario para la directiva de autenticación con ADAC

1.  Abre el centro de administración de Active Directory (ADAC).

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_OpenADAC.gif)

    > [!NOTE]
    > El texto seleccionado **autenticación** nodo está visible para los dominios que están en Windows Server 2012 R2 DFL. Si el nodo no aparece, vuelve a intentarlo con una cuenta de administrador de dominio de un dominio que está en Windows Server 2012 R2 DFL.

2.  Haz clic en **directivas de autenticación**y, a continuación, haz clic en **nueva** para crear una nueva directiva.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)

    Las directivas de autenticaciones deben tener un nombre para mostrar y se aplican de manera predeterminada.

3.  Para crear una directiva de auditoría, haz clic en **restricciones de directiva de auditoría solo**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)

    Las directivas de autenticación se aplican según el tipo de cuenta de Active Directory. Una única directiva puede aplicar a los tres tipos de cuenta mediante la configuración de configuración para cada tipo. Tipos de cuenta son:

    -   Usuario

    -   Equipo

    -   Cuenta de servicio administran por grupos y cuentas de servicio administradas

    Si se ha ampliado el esquema con nuevos principales que se puede usar el centro de distribución de claves (KDC), el nuevo tipo de cuenta se clasifica desde el tipo de cuenta derivada más cercano.

4.  Para configurar una duración TGT de cuentas de usuario, seleccione la **especificar una duración de vale para cuentas de usuario** casilla de verificación y especifique la hora en minutos.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTLifetime.gif)

    Por ejemplo, si quieres una vigencia máxima del vale de 10 horas, escribe **600** como se muestra. Si no se configura ninguna duración TGT, a continuación, si la cuenta es miembro de la **protegido usuarios** de grupo, la duración TGT y renovación 4 horas. De lo contrario, renovación y la duración TGT se basan en la directiva de dominio como se muestra en la siguiente ventana del Editor de administración de directivas de grupo para un dominio con la configuración predeterminada.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

5.  Para restringir la cuenta de usuario para seleccionar los dispositivos, haz clic en **editar** para definir las condiciones necesarias para el dispositivo.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)

6.  En la **editar las condiciones de Control de acceso** ventana, haz clic en **agregar una condición**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCondition.png)

##### <a name="add-computer-account-or-group-conditions"></a>Agregar las condiciones de grupo o cuenta de equipo

1.  Para configurar cuentas de equipo o grupos, en la lista desplegable, selecciona el cuadro de lista desplegable **miembro de cada** y cambia a **miembro de cualquier**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompMember.png)

    > [!NOTE]
    > Este control de acceso define las condiciones del dispositivo o desde el que el usuario inicia sesión en el host. En la terminología de control de acceso, la cuenta del dispositivo o el host es el usuario, que es la razón por **usuario** es la única opción.

2.  Haz clic en **agregar elementos**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddItems.png)

3.  Para cambiar los tipos de objeto, haz clic en **tipos de objeto**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjects.gif)

4.  Para seleccionar objetos de equipo en Active Directory, haga clic en **equipos**y, a continuación, haz clic en **Aceptar**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)

5.  Escribe el nombre de los equipos para restringir el usuario y, a continuación, haz clic en **comprobar nombres**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)

6.  Haz clic en Aceptar y crear todas las demás condiciones de la cuenta de equipo.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddConditions.png)

7.  Cuando termine, a continuación, haz clic en **Aceptar** y se mostrarán las condiciones definidas para la cuenta de equipo.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompDone.png)

##### <a name="add-computer-claim-conditions"></a>Agregar las condiciones de notificación del equipo

1.  Para configurar el equipo reclamaciones, lista desplegable grupo para seleccionar la reclamación.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaim.gif)

    Reclamaciones solo están disponibles si ya se suministran en el bosque.

2.  Escribe el nombre de la unidad organizativa, la cuenta de usuario deben limitarse al inicio de sesión.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimOUName.gif)

3.  Cuando termine, a continuación, haz clic en Aceptar y mostrará las condiciones definidas en el cuadro.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimComplete.gif)

##### <a name="troubleshoot-missing-computer-claims"></a>Solucionar problemas de falta de equipo de notificaciones
Si la notificación se ha aprovisionado, pero no está disponible, solo se puede configurar para **equipo** clases.

Supongamos que quieres restringir la autenticación basada en la unidad organizativa (OU) del equipo, que ya se ha configurado, pero solo para **equipo** clases.

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictComputers.gif)

Para que la reclamación esté disponible para restringir en el inicio de sesión de usuario en el dispositivo, selecciona el **usuario** casilla de verificación.

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)

#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>Aprovisionar una cuenta de usuario con una directiva de autenticación con ADAC

1.  Desde el **usuario** cuenta y, a continuación, haz clic en **directiva**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicy.gif)

2.  Selecciona el **asignar una directiva de autenticación a esta cuenta** casilla de verificación.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)

3.  A continuación, selecciona la directiva de autenticación para aplicar al usuario.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicySelect.png)

#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>Configurar la compatibilidad de Control de acceso dinámico en dispositivos y hosts
Puedes configurar duraciones TGT sin configurar el Control de acceso dinámico (DAC). DAC solo se necesita para comprobar AllowedToAuthenticateFrom y AllowedToAuthenticateTo.

Mediante la directiva de grupo o Editor de directivas de grupo Local, habilitar **compatibilidad del cliente Kerberos con reclamaciones, autenticación compuesta y protección de Kerberos** en configuración del equipo | Plantillas administrativas | Sistema | Kerberos:

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)

### <a name="BKMK_TroubleshootAuthnPolicies"></a>Solucionar problemas de directivas de autenticación

#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>Determinar las cuentas que se asignan directamente a una directiva de autenticación
La sección de cuentas en la directiva de autenticación muestra las cuentas que directamente aplicar la directiva.

![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AccountsAssigned.gif)

#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>Usar los errores de la directiva de autenticación - registro administrativa del controlador de dominio
Un nuevo **errores de la directiva de autenticación - controlador de dominio** administrativa registro bajo **registros de aplicaciones y servicios** > **Microsoft** > **Windows** > **autenticación** se ha creado para que sea más fácil descubrir errores debido a directivas de autenticación. El registro está deshabilitado de manera predeterminada. Para habilitarlo, haz clic en el nombre del registro y haga clic en **Habilitar registro**. Los nuevos eventos son muy similares en el contenido a la existente TGT y Kerberos vale de servicio eventos de auditoría. Para obtener más información sobre estos eventos, consulta [Silos de directiva de autenticación y directivas de autenticación](https://technet.microsoft.com/library/dn486813.aspx).

### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>Administrar directivas de autenticación mediante Windows PowerShell
Este comando crea una directiva de autenticación denominada **TestAuthenticationPolicy**. La **UserAllowedToAuthenticateFrom** parámetro especifica los dispositivos desde el que los usuarios puedan autenticarse con una cadena SDDL en el archivo denominado someFile.txt.

```
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl
```

Este comando obtiene todas las directivas de autenticación que coincidan con el filtro que la **filtro** parámetro especifica.

```
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com

```

Este comando modifica la descripción y la **UserTGTLifetimeMins** propiedades de la directiva de autenticación especificado.

```
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45
```

Este comando quita la directiva de autenticación que la **identidad** parámetro especifica.

```
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1
```

Este comando usa el **Get ADAuthenticationPolicy** cmdlet con la **filtro** parámetro para obtener todas las directivas de autenticación que no se aplican. El conjunto de resultados se transfiere a la **quitar ADAuthenticationPolicy** cmdlet.

```
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy
```

## <a name="BKMK_CreateAuthNPolicySilos"></a>Silos de directiva de autenticación
Silos de directiva de autenticación es un contenedor nuevo (objectClass msDS-AuthNPolicySilos) en AD DS para las cuentas de usuario, equipo y servicio. Ayudan a proteger cuentas de gran valor. Mientras que todas las organizaciones necesitan proteger a miembros de grupos de administradores de empresa, los administradores de dominio y administradores de esquema, ya que un atacante podrían usar esas cuentas para acceder a cualquier elemento en el bosque, otras cuentas podrán necesitar también protección.

Algunas organizaciones aislar las cargas de trabajo mediante la creación de cuentas que son únicas para ellos y aplicando la configuración de directiva de grupo para limitar el inicio de sesión interactivo local y remoto y privilegios administrativos. Silos de directiva de autenticación complementan este trabajo mediante la creación de una forma de definir una relación entre el usuario, el equipo y las cuentas de servicio administradas. Cuentas sólo pueden pertenecer a un silo. Puedes configurar la directiva de autenticación para cada tipo de cuenta para controlar:

1.  Ciclo de vida TGT no renovables

2.  Acceder a las condiciones de control para devolver TGT (Nota: no se puede aplicar a sistemas porque es necesaria la protección de Kerberos)

3.  Condiciones de control de acceso para devolver el vale de servicio

Además, las cuentas de un silo de directiva de autenticación tienen una reclamación silo, que puede usarse en los recursos de notificaciones, como los servidores de archivos para controlar el acceso.

Un descriptor de seguridad nuevas puede configurarse para emitir el vale de servicio basado en el control:

-   Usuario, grupos de seguridad del usuario o reclamaciones de usuario

-   Dispositivo, el grupo de seguridad del dispositivo o reclamaciones del dispositivo

Obtener esta información a los controladores de dominio del recurso requiere el Control de acceso dinámico:

-   Notificaciones de usuario:

    -   Windows 8 y versiones posteriores clientes compatibilidad con el Control de acceso dinámico

    -   Dominio de la cuenta admite el Control de acceso dinámico y notificaciones

-   Dispositivo o grupo de seguridad del dispositivo:

    -   Windows 8 y versiones posteriores clientes compatibilidad con el Control de acceso dinámico

    -   Recursos configurados para la autenticación compuesta

-   Notificaciones de dispositivo:

    -   Windows 8 y versiones posteriores clientes compatibilidad con el Control de acceso dinámico

    -   Dominio de dispositivo es compatible con el Control de acceso dinámico y notificaciones

    -   Recursos configurados para la autenticación compuesta

Directivas de autenticación pueden aplicarse a todos los miembros de un silo de directiva de autenticación en lugar de a las cuentas individuales o las directivas de autenticación independiente pueden aplicarse a los diferentes tipos de cuentas dentro de un silo. Por ejemplo, una directiva de autenticación puede aplicarse a cuentas de usuario con privilegios elevados y otra directiva puede aplicarse a cuentas de servicios. Directiva de autenticación al menos un debe crearse antes de poder crear una silo de directiva de autenticación.

> [!NOTE]
> Se puede aplicar una directiva de autenticación a los miembros de un silo de directiva de autenticación o se puede aplicar independientemente silos para restringir el ámbito de la cuenta específica. Por ejemplo, para proteger una sola cuenta o un pequeño conjunto de cuentas, se puede establecer una directiva en esas cuentas sin agregar las cuentas a un silo.

Puedes crear un silo de directiva de autenticación mediante el centro de administración de Active Directory o Windows PowerShell. De manera predeterminada, un silo de directiva de autenticación solo las auditorías de errores silo directivas, que es el equivalente a especificar la **WhatIf** parámetro de cmdlets de Windows PowerShell. En este caso, no se aplican las restricciones de directiva silo, pero se generan auditorías para indicar si se produce un error si se aplican las restricciones.

#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>Para crear un silo de directiva de autenticación mediante el centro de administración de Active Directory

1.  Abre **centro de administración de Active Directory**, haz clic en **autenticación**, haz clic en **Silos de directiva de autenticación**, haz clic en **nueva**y, a continuación, haz clic en **Silo de directiva de autenticación**.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)

2.  En **nombre para mostrar**, escribe un nombre para del sitio. En **permitido cuentas**, haz clic en **agregar**, escriba los nombres de las cuentas y, a continuación, haz clic en **Aceptar**. Puedes especificar usuarios, equipos o cuentas de servicio. A continuación, especificar si se usa una sola directiva para todos los principales o una directiva independiente para cada tipo de entidad de seguridad y el nombre de la directiva o directivas.

    ![cuentas protegidas](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)

### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>Administrar silos de directiva de autenticación mediante Windows PowerShell
Este comando crea un objeto de silo de directiva de autenticación y aplica.

```
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce
```

Este comando obtiene las de autenticación silos de directiva que coincidan con el filtro especificado por el **filtro** parámetro. El resultado se pasa a la **formato de tabla** cmdlet para mostrar el nombre de la directiva y el valor de **aplicar** en cada directiva.

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize

Name  Enforce
----  -------
silo     True
silos   False

```

Usa este comando la **Get ADAuthenticationPolicySilo** cmdlet con la **filtro** parámetro para obtener las silos de directiva de autenticación que no se aplican y canalización el resultado del filtro a la **quitar ADAuthenticationPolicySilo** cmdlet.

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo
```

Este comando concede acceso a del sitio de la directiva de autenticación denominado *Silo* para la cuenta de usuario denominado *usuario01*.

```
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01
```

Este comando revoque el acceso a del sitio de la directiva de autenticación denominado *Silo* para la cuenta de usuario denominado *usuario01*. Dado que la **confirmar** parámetro se establece en **$False**, no aparece ningún mensaje de confirmación.

```
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False
```

Este ejemplo se usa primero la **Get ADComputer** cmdlet para obtener todas las cuentas de equipo que coinciden con el filtro que la **filtro** parámetro especifica. El resultado de este comando se pasa a **conjunto ADAccountAuthenticatinPolicySilo** para asignar del sitio de la directiva de autenticación denominado *Silo* y la directiva de autenticación denominado *AuthenticationPolicy02* a ellos.

```
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02
```



