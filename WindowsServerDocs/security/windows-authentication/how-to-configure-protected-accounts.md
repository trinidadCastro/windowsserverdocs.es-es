---
title: Cómo configurar cuentas protegidas
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 28296b588c87bddb0364b9c3e10ad443870dc52f
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266822"
---
# <a name="how-to-configure-protected-accounts"></a>Cómo configurar cuentas protegidas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los ataques del tipo "pasar el hash" (PtH), un atacante puede autenticarse en un servidor o servicio remoto usando el hash de NTLM subyacente de la contraseña de un usuario (u otros derivados de las credenciales). Microsoft ya ha [publicado instrucciones](https://www.microsoft.com/download/details.aspx?id=36036) para mitigar los ataques pass-the-hash.  Windows Server 2012 R2 incluye nuevas características para ayudar a mitigar aún más estos ataques. Para obtener más información sobre otras características de seguridad que ayudan a proteger contra el robo de credenciales, consulte [Protección y administración de credenciales](https://technet.microsoft.com/library/dn408190.aspx). En este tema se explica cómo configurar las siguientes características nuevas:  
  
-   [Usuarios protegidos](#protected-users)  
  
-   [Directivas de autenticación](#authentication-policies)  
  
-   [Silos de directivas de autenticación](#authentication-policy-silos)  
  
Hay otras soluciones integradas en Windows 8.1 y Windows Server 2012 R2 para ayudar a proteger contra el robo de credenciales, que se tratan en los siguientes temas:  
  
-   [Modo de administración restringida para escritorio remoto](http://blogs.technet.com/b/kfalde/archive/20../restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)  
  
-   [Protección LSA](https://technet.microsoft.com/library/dn408187)  
  
## <a name="protected-users"></a>Usuarios protegidos  
Usuarios protegidos es un nuevo grupo de seguridad global al que puedes agregar usuarios nuevos o existentes. Dispositivos Windows 8.1 y Windows Server 2012 R2 hosts tienen un comportamiento especial con los miembros de este grupo para proporcionar una mejor protección contra el robo de credenciales. Para un miembro del grupo, un dispositivo Windows 8.1 o un host de Windows Server 2012 R2 no almacena en caché las credenciales que no son compatibles para usuarios protegidos. Los miembros de este grupo no tienen protección adicional si ha iniciado sesión en un dispositivo que ejecuta una versión de Windows anteriores a Windows 8.1.  
  
Agrupar miembros de usuarios protegidos que se inicien sesión en dispositivos Windows 8.1 y Windows Server 2012 R2 hosts pueden *ya no* utilizar:  
  
-   Delegación de credenciales predeterminada (CredSSP): las credenciales de texto sin formato no se almacenan en caché aunque la directiva **Permitir delegación de credenciales predeterminadas** esté habilitada.  
  
-   Autenticación implícita de Windows: las credenciales de texto sin formato no se almacenan en caché aunque estén habilitadas.  
  
-   NTLM: NTOWF no se almacena en caché.  
  
-   Claves a largo plazo de Kerberos: el vale de concesión de vales (TGT) de Kerberos se adquiere al iniciar sesión y no se puede volver a adquirir automáticamente.  
  
-   Inicio de sesión sin conexión: no se crea el comprobador de inicio de sesión en caché.  
  
Si el nivel funcional del dominio es Windows Server 2012 R2, los miembros del grupo ya no lo puede:  
  
-   Autenticarse mediante autenticación NTLM.  
  
-   Usar los conjuntos de cifrado Estándar de cifrado de datos (DES) o RC4 en la autenticación previa de Kerberos.  
  
-   Ser delegados mediante delegación limitada o sin limitar.  
  
-   Renovar los vales de usuario (TGT) más allá de las 4 horas de vigencia inicial.  
  
Para agregar usuarios al grupo, puede usar [herramientas de interfaz de usuario](https://technet.microsoft.com/library/cc753515.aspx) como centro de administración de Active Directory (ADAC) o usuarios de Active Directory y los equipos o una herramienta de línea de comandos como [Dsmod group](https://technet.microsoft.com/library/cc732423.aspx), o el de Windows PowerShell [Add-ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet. Las cuentas de servicio y de equipo *no deben* ser miembros del grupo Usuarios protegidos. En estas cuentas, la pertenencia no proporciona protección local porque la contraseña o el certificado siempre están disponibles en el host.  
  
> [!WARNING]  
> Las restricciones de autenticación no tienen solución alternativa, lo que significa que los miembros de los grupos de privilegios elevados, como el grupo Administradores de organización o el grupo Admins. del dominio están sujetos a las mismas restricciones que los demás miembros del grupo Usuarios protegidos. Si se agregan todos los miembros de estos grupos al grupo Usuarios protegidos, es posible que todas esas cuentas se bloqueen. No agregues nunca cuentas con privilegios elevados al grupo Usuarios protegidos hasta que hayas probado exhaustivamente sus posibles efectos.  
  
Los miembros del grupo Usuarios protegidos deben poder autenticarse mediante Kerberos con Estándar de cifrado avanzado (AES). Este método requiere claves de AES para la cuenta en Active Directory. El Administrador integrado no tiene una clave AES a menos que la contraseña se ha cambiado en un controlador de dominio que ejecute Windows Server 2008 o posterior. Además, se bloquean todas las cuentas que tengan una contraseña que se cambió en un controlador de dominio que ejecute una versión anterior de Windows Server. Por lo tanto, sigue estos procedimientos recomendados:  
  
-   No realice pruebas en dominios a menos que **todos los controladores de dominio ejecutan Windows Server 2008 o posterior**.  
  
-   Usa**Cambiar contraseña** para todas las cuentas de dominio que se crearon *antes* de crear el dominio. De lo contrario, estas cuentas no se podrán autenticar.  
  
-   **Cambiar contraseña** para cada usuario antes de agregar la cuenta de usuarios protegidos, grupo o asegúrese de que la contraseña ha cambiado recientemente en un controlador de dominio que ejecute Windows Server 2008 o posterior.  
  
### <a name="requirements-for-using-protected-accounts"></a>Requisitos para usar cuentas protegidas  
Las cuentas protegidas tienen los siguientes requisitos de implementación:  
  
-   Para proporcionar restricciones del lado cliente para usuarios protegidos, los hosts deben ejecutar Windows 8.1 o Windows Server 2012 R2. Un usuario solo tiene que iniciar sesión con una cuenta que sea miembro del grupo Usuarios protegidos. En este caso, se puede crear el grupo de usuarios protegidos por [transfiriendo el rol de emulador de controlador (PDC) de dominio principal](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a un controlador de dominio que ejecute Windows Server 2012 R2. Una vez replicado ese grupo a otros controladores de dominio, el rol de emulador de PDC se puede hospedar en un controlador de dominio que ejecuta una versión anterior de Windows Server.  
  
-   Para proporcionar restricciones de lado del controlador de dominio para los usuarios protegidos, es decir restringir el uso de la autenticación NTLM, y otras restricciones, el nivel funcional del dominio debe ser Windows Server 2012 R2. Para obtener más información sobre los niveles funcionales inferiores, consulte [Descripción de los niveles funcionales de Servicios de dominio de Active Directory (AD DS)](../../identity/ad-ds/active-directory-functional-levels.md).  
  
### <a name="troubleshoot-events-related-to-protected-users"></a>Solucionar problemas de eventos relativos a usuarios protegidos  
En esta sección se describen los nuevos registros que ayudan a solucionar problemas de eventos relativos a los usuarios protegidos, y cómo los usuarios protegidos pueden afectar a los cambios para solucionar problemas como la expiración de los vales de concesión de vales (TGT) o los problemas de delegación.  
  
#### <a name="new-logs-for-protected-users"></a>Nuevos registros para usuarios protegidos  
Hay dos nuevos registros administrativos operativos para ayudar a solucionar problemas de eventos que están relacionados con los usuarios operativos: Registro de controladores de dominio de usuario protegido (registro de cliente y errores de usuarios protegidos). Estos nuevos registros están ubicados en el Visor de eventos y están deshabilitados de forma predeterminada. Para habilitar un registro, haz clic en **Registros de aplicaciones y servicios**, en **Microsoft**, en **Windows**, en **Autenticación**y haz clic en el nombre del registro, en **Acción** (o haz clic con el botón derecho en el registro) y en **Habilitar registro**.  
  
Para obtener más información acerca de los eventos de estos registros, consulte [Directivas de autenticación y silos de directivas de autenticación](https://technet.microsoft.com/library/dn486813.aspx).  
  
#### <a name="troubleshoot-tgt-expiration"></a>Solucionar problemas de expiración de TGT  
Normalmente, el controlador de dominio establece la vigencia del TGT y su renovación según la directiva del dominio, tal y como se muestra en la siguiente ventana del Editor de administración de directivas de grupo.  
  
![Captura de pantalla de la ventana del Editor de administración de directivas de grupo que muestra cómo el controlador de dominio establece la vigencia de TGT y renovación según la directiva de dominio](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
Para los **Usuarios protegidos**, las siguientes opciones se codifican de forma rígida:  
  
-   Vigencia máxima del vale de usuario: 240 minutos  
  
-   Vigencia máxima de renovación de vales de usuario: 240 minutos  
  
#### <a name="troubleshoot-delegation-issues"></a>Solucionar problemas de delegación  
Anteriormente, si una tecnología que usa delegación de Kerberos producía un error, se comprobaba la cuenta de cliente para ver si la opción **La cuenta es importante y no se puede delegar** estaba establecida. Sin embargo, si la cuenta es miembro de **Usuarios protegidos**, quizás no tenga esta opción configurada en el Centro de administración de Active Directory (ADAC). Por lo tanto, comprueba la opción y la pertenencia al grupo cuando soluciones problemas de delegación.  
  
![Captura de pantalla que muestra dónde debe comprobar ** cuenta es importante y no se puede delegar ** de interfaz de usuario](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
### <a name="audit-authentication-attempts"></a>Auditar los intentos de autenticación  
Para auditar los intentos de autenticación explícitamente para los miembros del grupo **Usuarios protegidos**, puedes continuar recopilando eventos de auditoría del registro de seguridad o puedes recopilar los datos de los nuevos registros administrativos operativos. Para obtener más información acerca de estos eventos, consulte [Directivas de autenticación y silos de directivas de autenticación](https://technet.microsoft.com/library/dn486813.aspx).  
  
### <a name="provide-dc-side-protections-for-services-and-computers"></a>Proporcionar protección en el lado del controlador de dominio para servicios y equipos  
Las cuentas de servicios y equipos no pueden ser miembros de **Usuarios protegidos**. En esta sección se explica qué protecciones de controlador de dominio se pueden ofrecer para estas cuentas:  
  
-   Rechazar la autenticación NTLM: Solo se puede configurar a través de [directivas de bloqueo de NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx).  
  
-   Rechazar el Estándar de cifrado de datos (DES) en la autenticación previa de Kerberos:  Los controladores de dominio de Windows Server 2012 R2 no aceptan DES para cuentas de equipo a menos que se configuren para DES sólo porque todas las versiones de Windows lanzadas con Kerberos también admiten RC4.  
  
-   Rechazar RC4 en la autenticación previa de Kerberos: no configurable.  
  
    > [!NOTE]  
    > Aunque se puede [cambiar la configuración de los tipos de cifrado admitidos](http://blogs.msdn.com/b/openspecification/archive/20../windows-configurations-for-kerberos-supported-encryption-type.aspx), no se recomienda cambiar esta configuración para las cuentas de equipo sin probarla en el entorno de destino.  
  
-   Restringir los vales de usuario (TGT) a las 4 horas de vigencia inicial: usa directivas de autenticación.  
  
-   Denegar la delegación con delegación limitada o no limitada: Para restringir una cuenta, abre el Centro de administración de Active Directory (ADAC) y activa la casilla **La cuenta es importante y no se puede delegar**.  
  
    ![Captura de pantalla que muestra dónde debe restringir una cuenta](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TshootDelegation.gif)  
  
## <a name="authentication-policies"></a>Directivas de autenticación  
Directivas de autenticación es un nuevo contenedor de AD DS que contiene objetos de directiva de autenticación. Las directivas de autenticación pueden especificar configuraciones que ayuden a mitigar la exposición a robos de credenciales, como restringir la vigencia de los TGT para las cuentas o agregar otras condiciones relativas a notificaciones.  
  
En Windows Server 2012, Control de acceso dinámico presentó una clase de objeto de ámbito del bosque de Active Directory llamada directiva de acceso Central para proporcionar una manera fácil de configurar los servidores de archivos de una organización. En Windows Server 2012 R2, se puede usar una nueva clase de objeto llamada directiva de autenticación (objectClass msDS-AuthNPolicies) para aplicar la configuración de autenticación a clases de cuentas en dominios de Windows Server 2012 R2. Las clases de cuentas de Active Directory son las siguientes:  
  
-   Usuario  
  
-   Computer  
  
-   Cuenta de servicio administrada y cuenta de servicio administrada de grupo (GMSA)  
  
### <a name="quick-kerberos-refresher"></a>Actualizador de Kerberos rápido  
El protocolo de autenticación Kerberos consiste en tres tipos de intercambios, también conocidos como subprotocolos:  
  
![Captura de pantalla muestra a los tres tipos de autenticación de Kerberos intercambios del protocolo, también conocidos como subprotocolos](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbRefresher.gif)  
  
-   Intercambio del servicio de autenticación (AS) (KRB_AS_*)  
  
-   Intercambio del servicio de concesión de vales (TGS) (KRB_TGS_*)  
  
-   Intercambio cliente/servidor (AP) (KRB_AP_*)  
  
El intercambio AS es donde el cliente usa la contraseña de la cuenta o la clave privada para crear un autenticador previo para solicitar un vale de concesión de vales (TGT). Esto sucede durante el inicio de sesión o la primera vez que se necesita un vale de servicio.  
  
El intercambio TGS es donde se usa el TGT de una cuenta para crear un autenticador para solicitar un vale de servicio. Esto sucede cuando se necesita una conexión autenticada.  
  
El intercambio AP se produce con la misma frecuencia que los datos dentro del protocolo de la aplicación, y no se ve afectado por las directivas de autenticación.  
  
Para obtener información más detallada, consulte [Cómo funciona el protocolo de autenticación Kerberos versión 5](https://technet.microsoft.com/library/cc772815(v=WS.10.aspx)).  
  
### <a name="overview"></a>Información general  
Las directivas de autenticación complementan el grupo de usuarios protegidos porque proporcionan una manera de aplicar restricciones configurables a las cuentas, además de restricciones a las cuentas de servicios y equipos. Las directivas de autenticación se aplican durante el intercambio AS o el intercambio TGS.  
  
Puedes configura lo siguiente para limitar la autenticación inicial o el intercambio AS:  
  
-   Una vigencia de TGT.  
  
-   Condiciones de control de acceso para restringir el inicio de sesión de usuarios, que deben cumplir los dispositivos desde los que procede el intercambio AS.  
  
![Captura de pantalla que muestra cómo restringir la autenticación inicial mediante la configuración de condiciones de control de una duración y el acceso TGT para restringir el inicio de sesión de usuario](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictAS.gif)  
  
Puedes configurar lo siguiente para restringir las solicitudes de vales de servicio mediante un intercambio de servicio de concesión de vales (TGS):  
  
-   Condiciones de control de acceso que deben cumplir el cliente (usuario, servicio, equipo) o los dispositivos desde los que procede el intercambio TGS.  
  
### <a name="requirements-for-using-authentication-policies"></a>Requisitos para usar directivas de autenticación  
  
|Directiva|Requisitos|  
|-----|--------|  
|Proporcionar vigencias de TGT personalizadas| Dominios de cuentas con nivel funcional de dominio de Windows Server 2012 R2|  
|Restringir el inicio de sesión de usuarios|: Compatibilidad con dominios de cuentas con nivel funcional de dominio Windows Server 2012 R2 con el Control de acceso dinámico<br />-Compatibilidad con dispositivos Windows 8, Windows 8.1, Windows Server 2012 o Windows Server 2012 R2 con el Control de acceso dinámico|  
|Restringir la emisión de vales de servicio en función de cuentas de usuario o grupos de seguridad| Dominios de recursos con nivel funcional de dominio de Windows Server 2012 R2|  
|Restringir la emisión de vales de servicio en función de notificaciones de usuario o cuentas de dispositivo, grupos de seguridad o notificaciones| Compatibilidad con dominios de recursos con nivel funcional de dominio de Windows Server 2012 R2 con el Control de acceso dinámico|  
  
### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>Restringir una cuenta de usuario a dispositivos y hosts específicos  
Una cuenta de gran valor con privilegios administrativos debe ser miembro del grupo **Usuarios protegidos** . De forma predeterminada, ninguna cuenta es miembro del grupo **Usuarios protegidos** . Antes de agregar cuentas al grupo, configura la compatibilidad del controlador de dominio y crea una directiva de auditoría para asegurarte de que no haya problemas de bloqueo.  
  
#### <a name="configure-domain-controller-support"></a>Configurar la compatibilidad del controlador de dominio  
Dominio de la cuenta del usuario debe estar en el nivel funcional del dominio de Windows Server 2012 R2 (DFL). Asegúrese de todos los controladores de dominio son Windows Server 2012 R2 y, a continuación, usar dominios de Active Directory y las confianzas para [elevar el nivel funcional](https://technet.microsoft.com/library/cc753104.aspx) a Windows Server 2012 R2.  
  
**Para configurar la compatibilidad con Control de acceso dinámico**  
  
1.  En la Directiva predeterminada de controladores de dominio, haz clic en **Habilitada** para habilitar **Compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos** en Configuración del equipo | Plantillas administrativas | Sistema | KDC.  
  
    ![En la directiva de controladores de dominio predeterminado, haga clic en ** activado ** Habilitar ** soporte de cliente de centro de distribución de claves (KDC) para notificaciones, autenticación compuesta y protección de Kerberos ** en la configuración del equipo | Plantillas administrativas | Sistema | KDC](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)  
  
2.  En **Opciones**, en el cuadro de lista desplegable, selecciona **Proporcionar notificaciones siempre**.  
  
    > [!NOTE]  
    > **Admite** también se puede configurar, pero dado que es el dominio en Windows Server 2012 R2 DFL, tener los controladores de dominio siempre proporcionar notificaciones permitirá el acceso de usuario basada en notificaciones comprueba que se produzca cuando se usa dispositivos compatibles con no basada en notificaciones y los hosts para conectarse a Servicios de notificaciones.  
  
    ![En ** Opciones **, en el cuadro de lista desplegable, seleccione ** proporcionar notificaciones siempre](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)  
  
    > [!WARNING]  
    > Configurar **producirá un error en las solicitudes de autenticación sin blindar** se producirán errores de autenticación desde cualquier sistema operativo que no admite la protección, como Windows 7 y sistemas operativos anteriores, ni el funcionamiento de Kerberos sistemas a partir de Windows 8, que no hayan sido configurados explícitamente para admitirla.  
  
#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>Crear una auditoría de cuenta de usuario para directiva de autenticación con ADAC  
  
1.  Abre el Centro de administración de Active Directory (ADAC).  
  
    ![Captura de pantalla con el centro de administración de Active Directory](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_OpenADAC.gif)  
  
    > [!NOTE]  
    > Seleccionado **autenticación** nodo es visible para los dominios que están en el nivel funcional de dominio de Windows Server 2012 R2. Si el nodo no aparece, vuelva a intentarlo con una cuenta de administrador de dominio desde un dominio que se encuentra en el nivel funcional de dominio de Windows Server 2012 R2.  
  
2.  Haz clic en **Directivas de autenticación** y, después, haz clic en **Nueva** para crear una nueva directiva.  
  
    ![Authentication Policies](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)  
  
    Las directivas de autenticación deben tener un nombre para mostrar y se aplican de forma predeterminada.  
  
3.  Para crear una directiva solo de auditoría, haz clic en **Solo restricciones de directiva de auditoría**.  
  
    ![Solo las restricciones de directivas de auditoría](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)  
  
    Las directivas de autenticación se aplican en función del tipo de cuenta de Active Directory. Una única directiva se puede aplicar a los tres tipos de cuentas configurando las opciones de cada tipo. Los tipos de cuentas son:  
  
    -   Usuario  
  
    -   Computer  
  
    -   Cuenta de servicio administrada y cuenta de servicio administrada de grupo  
  
    Si has extendido el esquema con nuevas entidades de seguridad que el Centro de distribución de claves (KDC) pueda usar, el nuevo tipo de cuenta se clasifica a partir del tipo de cuenta derivado más próximo.  
  
4.  Para configurar una vigencia de TGT para cuentas de usuario, activa la casilla **Especifique la duración de un vale de concesión de vales para las cuentas de usuario** y especifica el tiempo en minutos.  
  
    ![Especificar una duración de vale de concesión de vales para las cuentas de usuario](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTLifetime.gif)  
  
    Por ejemplo, si quieres una vigencia máxima de TGT de 10 horas, especifica **600** tal y como se indica. Si no hay configurada una vigencia de TGT, si la cuenta es miembro del grupo **Protected Users** , la vigencia y la renovación del TGT es de 4 horas. De lo contrario, la vigencia y la renovación del TGT se basan en la directiva de dominio tal y como se observa en la siguiente ventana del Editor de administración de directivas de grupo para un dominio con configuración predeterminada.  
  
    ![Ventana del Editor de administración de directivas de grupo para un dominio con la configuración predeterminada](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_TGTExpiration.png)  
  
5.  Para restringir la cuenta de usuario a los dispositivos seleccionados, haz clic en **Editar** para definir las condiciones necesarias para el dispositivo.  
  
    ![Para restringir la cuenta de usuario para seleccionar los dispositivos, haga clic en ** edición **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)  
  
6.  En la ventana **Editar condiciones del control de acceso** , haz clic en **Agregar una condición**.  
  
    ![Editar las condiciones de Control de acceso](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCondition.png)  
  
##### <a name="add-computer-account-or-group-conditions"></a>Agregar condiciones para cuentas de equipo o grupos  
  
1.  Para configurar cuentas de equipo o grupos, en la lista desplegable, selecciona el cuadro de lista desplegable **Miembro de cada** y cámbialo a **Miembro de cualquiera**.  
  
    ![Configurar cuentas de equipo o grupos](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompMember.png)  
  
    > [!NOTE]  
    > Este control de acceso define las condiciones del servicio o host desde el que el usuario inicia sesión. En la terminología del control de acceso, la cuenta de equipo para el dispositivo o host es el usuario, motivo por el que **Usuario** es la única opción.  
  
2.  Haz clic en **Agregar elementos**.  
  
    ![Agregar elementos](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddItems.png)  
  
3.  Para cambiar los tipos de objeto, haz clic en **Tipos de objeto**.  
  
    ![Tipos de objeto](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjects.gif)  
  
4.  Para seleccionar objetos de equipo en Active Directory, haz clic en **Equipos** y, después, haz clic en **Aceptar**.  
  
    ![Equipos](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)  
  
5.  Escribe el nombre de los equipos para restringir el usuario y haz clic en **Comprobar nombres**.  
  
    ![Haga clic en Comprobar nombres](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)  
  
6.  Haz clic en Aceptar y crea otras condiciones para la cuenta de equipo.  
  
    ![Haga clic en Aceptar y crear otras condiciones para la cuenta de equipo](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompAddConditions.png)  
  
7.  Una vez hecho, haz clic en **Aceptar** y las condiciones definidas aparecerán para la cuenta de equipo.  
  
    ![Cuando haya terminado, haga clic en ** Aceptar **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AddCompDone.png)  
  
##### <a name="add-computer-claim-conditions"></a>Agregar condiciones de notificaciones de equipo  
  
1.  Para configurar notificaciones de equipo, abre la lista desplegable Grupo para seleccionar la notificación.  
  
    ![Para configurar notificaciones de equipo, lista desplegable grupo para seleccionar la notificación](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaim.gif)  
  
    Las notificaciones solo están disponibles si ya se han aprovisionado en el bosque.  
  
2.  Escribe el nombre de la OU; la cuenta de usuario debe restringirse al inicio de sesión.  
  
    ![Escribe el nombre de la OU; la cuenta de usuario debe restringirse al inicio de sesión.](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimOUName.gif)  
  
3.  Una vez hecho, haz clic en Aceptar y el cuadro mostrará las condiciones definidas.  
  
    ![Cuando haya finalizado la haga clic en Aceptar](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CompClaimComplete.gif)  
  
##### <a name="troubleshoot-missing-computer-claims"></a>Solucionar problemas de notificaciones de equipo que faltan  
Si la notificación se ha aprovisionado pero no está disponible, quizás solo se ha configurado para las clases **Equipo**.  
  
Supongamos que querías restringir la autenticación basada en la unidad organizativa (OU) del equipo, que ya estaba configurado, pero solo para **equipo** clases.  
  
![Captura de pantalla muestra cómo restringir la autenticación basada en la unidad organizativa (OU) del equipo](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictComputers.gif)  
  
Para que la notificación esté disponible para restringir el inicio de usuarios en el dispositivo, activa la casilla **Usuario**.  
  
![Captura de pantalla muestra cómo restringir el usuario inicie sesión en el dispositivo comprobando el, seleccione ** casilla de verificación de usuario **.  ](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)  
  
#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>Aprovisionar una cuenta de usuario con una directiva de autenticación con ADAC  
  
1.  En la cuenta **Usuario** , haz clic en **Directiva**.  
  
    ![Desde el ** cuenta de usuario **, haga clic en ** Directiva **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicy.gif)  
  
2.  Activa la casilla **Asigne una directiva de autenticación a esta cuenta**.  
  
    ![Seleccione el ** asignar una directiva de autenticación a esta casilla de verificación de cuenta **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)  
  
3.  Después, selecciona la directiva de autenticación que se aplicará al usuario.  
  
    ![Seleccione la directiva de autenticación que se aplicará al usuario](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_UserPolicySelect.png)  
  
#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>Configurar la compatibilidad con el control de acceso dinámico en dispositivos y hosts  
Puedes configurar la vigencia de los TGT sin configurar el control de acceso dinámico (DAC). DAC solo se necesita para comprobar AllowedToAuthenticateFrom y AllowedToAuthenticateTo.  
  
Usa el Editor de directivas de grupo o el Editor de directivas de grupo local para habilitar **Compatibilidad del cliente Kerberos con notificaciones, autenticación compuesta y protección de Kerberos** en Configuración del equipo | Plantillas administrativas | Sistema | Kerberos:  
  
![Captura de pantalla que muestra cómo usar la directiva de grupo o el Editor de directivas de grupo Local para habilitar ** soporte técnico del cliente Kerberos para notificaciones, autenticación compuesta y protección de Kerberos **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)  
  
### <a name="troubleshoot-authentication-policies"></a>Solucionar problemas de directivas de autenticación  
  
#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>Determina las cuentas que están directamente asignadas a una directiva de autenticación  
La sección de cuentas de la directiva de autenticación muestra las cuentas a las que se ha aplicado directamente la directiva.  
  
![Captura de pantalla de la sección cuentas en la directiva de autenticación que muestra las cuentas que se han aplicado directamente la directiva](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_AccountsAssigned.gif)  
  
#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>Uso de los errores de directiva de autenticación - registro administrativo del controlador de dominio  
Un nuevo **errores de directiva de autenticación: controlador de dominio** registro administrativo en **registros de aplicaciones y servicios** > **Microsoft**  >  **Windows** > **autenticación** se ha creado para que sea más fácil detectar errores debidos a directivas de autenticación. El registro está deshabilitado de forma predeterminada. Para habilitarlo, haz clic con el botón derecho en el nombre del registro y haz clic en **Habilitar registro**. El contenido de los nuevos eventos es muy similar al de los eventos de auditoría de TGT y de vales de servicio de Kerberos existentes. Para obtener más información acerca de estos eventos, consulte [Directivas de autenticación y silos de directivas de autenticación](https://technet.microsoft.com/library/dn486813.aspx).  
  
### <a name="manage-authentication-policies-by-using-windows-powershell"></a>Administrar directivas de autenticación mediante Windows PowerShell  
Este comando crea una directiva de autenticación llamada **TestAuthenticationPolicy**. El parámetro **UserAllowedToAuthenticateFrom** especifica los dispositivos desde los que los usuarios pueden autenticarse mediante una cadena SDDL en el archivo someFile.txt.  
  
```  
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl  
```  
  
Este comando obtiene todas las directivas de autenticación que coinciden con el filtro especificado por el parámetro **Filter**.  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com  
  
```  
  
Este comando modifica la descripción y las propiedades de **UserTGTLifetimeMins** de la directiva de autenticación especificada.  
  
```  
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45  
```  
  
Este comando quita la directiva de autenticación especificada por el parámetro **Identity** .  
  
```  
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1  
```  
  
Este comando usa el cmdlet **Get-ADAuthenticationPolicy** con el parámetro **Filter** para obtener todas las directivas de autenticación que no se aplican. El conjunto resultante se canaliza al cmdlet **Remove-ADAuthenticationPolicy**.  
  
```  
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy  
```  
  
## <a name="authentication-policy-silos"></a>Silos de directivas de autenticación  
Silos de directivas de autenticación es un nuevo contenedor (objectClass msDS-AuthNPolicySilos) de AD DS para cuentas de usuario, equipo y servicio. Ayudan a proteger las cuentas de gran valor. Aunque todas las organizaciones necesitan proteger a los miembros de los grupos Administradores de organización, Admins. del dominio y Administradores de esquema porque esas cuentas podrían ser usadas por un atacante para acceder al bosque, otras cuentas también pueden necesitar protección.  
  
Algunas organizaciones aíslan las cargas de trabajo; para ello, crean cuentas únicas para ellas y aplican configuraciones de directiva de grupo para limitar el inicio de sesión interactivo local y remoto, y los privilegios administrativos. Los silos de directivas de autenticación complementan este trabajo y crean una manera de definir una relación entre las cuentas de usuario, de equipo y de servicio administradas. Las cuentas solo pueden pertenecer a un silo. Puedes configurar la directiva de autenticación para cada tipo de cuenta para controlar lo siguiente:  
  
1.  Una vigencia de TGT no renovable.  
  
2.  Las condiciones de control de acceso para devolver TGT. (Nota: no se puede aplicar a sistemas porque se necesita la protección de Kerberos).  
  
3.  Las condiciones de control de acceso para devolver vales de servicio.  
  
Además, las cuentas de un silo de directivas de autenticación tienen una notificación de silo, que los recursos habilitados para notificaciones, como los servidores de archivos, pueden usar para controlar el acceso.  
  
Se puede configurar un nuevo descriptor de seguridad para controlar la emisión de vales de servicio en función de lo siguiente:  
  
-   Usuario, grupos de seguridad del usuario y notificaciones del usuario  
  
-   Dispositivo, grupos de seguridad y notificaciones del dispositivo  
  
Obtener esta información para los controladores de dominio del recurso requiere el Control de acceso dinámico:  
  
-   Notificaciones de usuario:  
  
    -   Los clientes de Windows 8 y versiones posteriores admiten control de acceso dinámico.  
  
    -   Los dominios de cuenta admiten control de acceso dinámico y notificaciones  
  
-   Dispositivo o grupo de seguridad del dispositivo:  
  
    -   Los clientes de Windows 8 y versiones posteriores admiten control de acceso dinámico.  
  
    -   Recurso configurado para autenticación compuesta  
  
-   Notificaciones de dispositivo:  
  
    -   Los clientes de Windows 8 y versiones posteriores admiten control de acceso dinámico.  
  
    -   Los dominios de dispositivo admiten control de acceso dinámico y notificaciones  
  
    -   Recurso configurado para autenticación compuesta  
  
Las directivas de autenticación se pueden aplicar a todos los miembros de un silo de directivas de autenticación en lugar de a cuentas individuales, o se pueden aplicar directivas de autenticación diferentes a los distintos tipos de cuentas de un silo. Por ejemplo, se puede aplicar una directiva de autenticación a cuentas de usuario con privilegios elevados, y se puede aplicar una directiva diferente a las cuentas de servicios. Se debe crear al menos una directiva de autenticación antes de poder crear un silo de directivas de autenticación.  
  
> [!NOTE]  
> Se puede aplicar una directiva de autenticación a los miembros de un silo de directivas de autenticación, o se puede aplicar de forma independiente de los silos para restringir el ámbito de cuenta específico. Por ejemplo, para proteger una sola cuenta o un conjunto pequeño de cuentas, se puede establecer una directiva para esas cuentas sin agregar las cuentas a un silo.  
  
Puede crear un silo de directivas de autenticación mediante el centro de administración de Active Directory o Windows PowerShell. De forma predeterminada, un silo de directivas de autenticación solo audita directivas de silo, lo que equivale a especificar la **WhatIf** parámetros en cmdlets de Windows PowerShell. En este caso, no se aplican restricciones de silo de directivas, pero se generan auditorías para indicar si se producirán errores si se aplican las restricciones.  
  
#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>Para crear un silo de directivas de autenticación usando el Centro de administración de Active Directory  
  
1.  Abre el **Centro de administración de Active Directory**, haz clic en **Autenticación**, haz clic con el botón derecho en **Silos de directivas de autenticación**, haz clic en **Nuevo**y, después, en **Silo de directivas de autenticación**.  
  
    ![Abra ** Active Directory administrativo Center **, haga clic en ** autenticación **, haga clic en ** autenticación directiva Silos **, haga clic en ** New ** y, a continuación, haga clic en ** Silo de directivas de autenticación **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)  
  
2.  En **Nombre para mostrar**, escribe un nombre para el silo. En **Cuentas permitidas**, haz clic en **Agregar**, escribe el nombre de las cuentas y, después, haz clic en **Aceptar**. Puedes especificar cuentas de usuario, de equipo o de servicio. Después, especifica si se usará una única directiva para todas las entidades de seguridad, o una directiva diferente para cada tipo de entidad de seguridad, y el nombre de la directiva o directivas.  
  
    ![En ** ** nombre para mostrar, escriba un nombre para el silo. En ** permiten cuentas **, haga clic en ** Agregar **, escriba los nombres de las cuentas y, a continuación, haga clic en ** Aceptar **](../media/how-to-configure-protected-accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)  
  
### <a name="manage-authentication-policy-silos-by-using-windows-powershell"></a>Administrar silos de directivas de autenticación mediante Windows PowerShell  
Este comando crea un objeto de silo de directivas de autenticación y lo aplica.  
  
```  
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce  
```  
  
Este comando obtiene todos los silos de directivas de autenticación que coinciden con el filtro especificado por el parámetro **Filter** . Después, el resultado se pasa al cmdlet **Format-Table** para mostrar el nombre de la directiva y el valor de **Enforce** en cada directiva.  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize  
  
Name  Enforce  
--  ----  
silo     True  
silos   False  
  
```  
  
Este comando usa el cmdlet **Get-ADAuthenticationPolicySilo** con el parámetro **Filter** para obtener todos los silos de directiva de autenticación que no se aplican y canalizar el resultado del filtro al cmdlet **Remove-ADAuthenticationPolicySilo** .  
  
```  
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo  
```  
  
Este comando concede al silo de directivas de autenticación *Silo* el acceso a la cuenta de usuario *User01*.  
  
```  
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01  
```  
  
Este comando revoca al silo de directivas de autenticación *Silo* el acceso a la cuenta de usuario *User01*. Como el parámetro **Confirm** está establecido en **$False**, no aparece ningún mensaje de confirmación.  
  
```  
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False  
```  
  
Este ejemplo usa primero el cmdlet **Get-ADComputer** para obtener todas las cuentas de equipo que coinciden con el filtro especificado por el parámetro **Filter**. El resultado de este comando se pasa a **Set-ADAccountAuthenticatinPolicySilo** para asignarles el silo de directivas de autenticación *Silo* y la directiva de autenticación *AuthenticationPolicy02* .  
  
```  
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02  
```  
  
