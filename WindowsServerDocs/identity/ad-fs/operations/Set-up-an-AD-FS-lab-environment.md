---
description: Más información acerca de cómo configurar un entorno de laboratorio AD FS
ms.assetid: 276a7f7d-5faa-4c00-a51c-3fa511fe52f9
title: Configuración de un entorno de laboratorio de AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 0b7c4c40ea37c9d8f7f2e7dd5639245a98a25aab
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039693"
---
# <a name="set-up-an-ad-fs-lab-environment"></a>Configuración de un entorno de laboratorio de AD FS

En este tema se describen los pasos para configurar un entorno de prueba que puede utilizarse para completar los tutoriales de las siguientes guías:

-   [Tutorial: Workplace Join con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [Tutorial: Workplace Join con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)

-   [Guía de tutorial: administración de riesgos con control de acceso condicional](Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [Guía de tutorial: administración de riesgos con Multi-Factor Authentication adicionales para aplicaciones confidenciales](Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> No recomendamos instalar el servidor web y el servidor de federación en el mismo equipo.

Para configurar este entorno de prueba, realiza los siguientes pasos:

1.  [Paso 1: Configurar el controlador de dominio (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [Paso 2: configurar el servidor de Federación (ADFS1) con el servicio de registro de dispositivos](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [Paso 3: Configurar el servidor web (WebServ1) y una aplicación de ejemplo basada en notificaciones](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [Paso 4: Configurar el equipo cliente (Client1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="step-1-configure-the-domain-controller-dc1"></a><a name="BKMK_1"></a>Paso 1: Configurar el controlador de dominio (DC1)
Para los fines de este entorno de prueba, puede llamar a la raíz Active Directory dominio **contoso.com** y especificar <strong>pass@word1</strong> como contraseña de administrador.

-   Instale el servicio de rol de AD DS e instale Active Directory Domain Services (AD DS) para convertir su equipo en un controlador de dominio en Windows Server 2012 R2. Esta acción actualiza el esquema de AD DS como parte de la creación del controlador de dominio. Para obtener más información e instrucciones paso a paso, vea[ https://technet.microsoft.com/ Library/hh472162. aspx](../../ad-ds/deploy/install-active-directory-domain-services--level-100-.md).

### <a name="create-test-active-directory-accounts"></a><a name="BKMK_2"></a>Crear cuentas de prueba de Active Directory
Cuando tu controlador de dominio esté operativo, puedes crear un grupo de prueba y cuentas de usuario de prueba en dicho dominio, y agregar la cuenta de usuario a la cuenta de grupo. Utilizarás estas cuentas para completar los tutoriales de las guías mencionadas anteriormente en este tema.

Crea las cuentas siguientes:

- Usuario: **Robert Hatley** con las siguientes credenciales: nombre de usuario: **RobertH** y contraseña: <strong>P@ssword</strong>

- Grupo: **Finance**

Para obtener información acerca de cómo crear cuentas de usuario y de grupo en Active Directory (AD), vea [https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx](/previous-versions/windows/it-pro/windows-server-2003/cc783323(v=ws.10)) .

Agrega la cuenta **Robert Hatley** al grupo **Finance**. Para obtener información acerca de cómo agregar un usuario a un grupo en Active Directory, vea [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx](/previous-versions/windows/it-pro/windows-server-2003/cc737130(v=ws.10)) .

### <a name="create-a-gmsa-account"></a>Crear una cuenta de GMSA
La cuenta de servicio administrada de grupo (GMSA) es necesaria durante la instalación y configuración de Servicios de federación de Active Directory (AD FS) (AD FS).

##### <a name="to-create-a-gmsa-account"></a>Cómo crear una cuenta de GMSA

1.  Abre una ventana del símbolo del sistema de Windows PowerShell y escribe:

    ```
    Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="step-2-configure-the-federation-server-adfs1-by-using-device-registration-service"></a><a name="BKMK_4"></a>Paso 2: Configurar el servidor de federación (ADFS1) con el Servicio de registro de dispositivos
Para configurar otra máquina virtual, instale Windows Server 2012 R2 y conéctela al dominio **contoso.com**. Configure el equipo después de unirlo al dominio y, a continuación, continúe con la instalación y configuración del rol de AD FS.

Para ver un vídeo, consulte [Serie de vídeos prácticos sobre los Servicios de federación de Active Directory: instalar una granja de servidores de AD FS](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search).

### <a name="install-a-server-ssl-certificate"></a>Instalar un certificado SSL de servidor
Debes instalar un certificado de Capa de sockets seguros (SSL) de servidor en el servidor ADFS1 en el almacén del equipo local. El certificado DEBE tener los siguientes atributos:

-   Nombre del firmante (CN): adfs1.contoso.com

-   Nombre alternativo del firmante (DNS): adfs1.contoso.com

-   Nombre alternativo del firmante (DNS): enterpriseregistration.contoso.com

[Guía de Servicio web de inscripción de certificados](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

[Serie de vídeos prácticos sobre los Servicios de federación de Active Directory: actualizar certificados](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search).

### <a name="install-the-ad-fs-server-role"></a>Instalar el rol de servidor de AD FS

##### <a name="to-install-the-federation-service-role-service"></a>Cómo instalar el servicio de rol de servicio de federación

1. Inicie sesión en el servidor con la cuenta de administrador de dominio administrator@contoso.com .

2. Inicie el Administrador del servidor. Para iniciar el Administrador del servidor, haz clic en **Administrador del servidor** en la pantalla **Inicio** de Windows, o bien en el **Administrador del servidor** en la barra de tareas de Windows en el escritorio. En la pestaña **Inicio rápido** del icono **Página principal** en la página **Panel**, haz clic en **Agregar roles y características**. Como alternativa, puede hacer clic en **Agregar roles y características** en el menú **Administrar**.

3. En la página **Antes de comenzar** , haga clic en **Siguiente**.

4. En la página **Seleccionar tipo de instalación**, haga clic en **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.

5. En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que está seleccionado el equipo de destino y haz clic en **Siguiente**.

6. En la página **Seleccionar roles de servidor**, haz clic en **Servicios de dominio de Active Directory** y en **Siguiente**.

7. En la página **Seleccionar características**, haz clic en **Siguiente**.

8. En la página **Servicios de federación de Active Directory (AD FS)**, haz clic en **Siguiente**.

9. Después de comprobar la información de la página **Confirmar selecciones de instalación**, active la casilla **Reiniciar automáticamente el servidor de destino en caso necesario** y luego haga clic en **Instalar**.

10. En la página **Progreso de la instalación**, comprueba que todo se instala correctamente y haz clic en **Cerrar**.

### <a name="configure-the-federation-server"></a>Configurar el servidor de federación
El siguiente paso consiste en configurar el servidor de federación.

##### <a name="to-configure-the-federation-server"></a>Cómo configurar el servidor de federación

1.  En la página **Panel** del Administrador de servidores, haz clic en el marcador **Notificaciones** y en **Configure el servicio de federación en este servidor**.

    Se abre el **Asistente para configuración de Servicios de federación de Active Directory**.

2.  En la **Página principal**, selecciona **Crear el primer servidor de federación en una granja de servidores de federación** y haz clic en **Siguiente**.

3.  En la página **Conectar a AD DS**, especifica una cuenta con derechos de administrador de dominio para el dominio de Active Directory **contoso.com** al que está unido este dominio, y haz clic en **Siguiente**.

4.  En la página **Especificar propiedades del servicio**, haz lo que se explica a continuación y, después, haz clic en **Siguiente**:

    -   Importa el certificado SSL que obtuviste previamente. Este certificado es el certificado de autenticación de servicio necesario. Busca la ubicación del certificado SSL.

    -   Para darle un nombre al servicio de federación, escribe **adfs1.contoso.com**. Este valor es el mismo que proporcionaste cuando inscribiste un certificado SSL en los Servicios de certificados de Active Directory (AD CS).

    -   Para darle un nombre para mostrar al servicio de federación, escribe **Contoso Corporation**.

5.  En la página **Especificar cuenta de servicio**, selecciona **Usar una cuenta de usuario de dominio o una cuenta de servicio administrada de grupo existente** y especifica la cuenta de GMSA **fsgmsa** que creaste al crear el controlador de dominio.

6.  En la página **Especificar base de datos de configuración**, selecciona **Crear una base de datos en este servidor que usa Windows Internal Database** y haz clic en **Siguiente**.

7.  En la página **Revisar opciones**, comprueba la configuración que has seleccionado y haz clic en **Siguiente**.

8.  En la página **Comprobación de requisitos previos**, comprueba que se hayan completado correctamente todos los requisitos previos y haz clic en **Configurar**.

9. En la página **Resultados**, revisa los resultados, comprueba que la configuración se haya completado correctamente y haz clic en **Hay que realizar los siguientes pasos para completar la implementación del servicio de federación**.

### <a name="configure-device-registration-service"></a>Configurar el Servicio de registro de dispositivos
El siguiente paso consiste en configurar el Servicio de registro de dispositivos en el servidor ADFS1. Para ver un vídeo, consulte [Serie de vídeos prácticos sobre los Servicios de federación de Active Directory: habilitar el Servicio de registro de dispositivos](https://channel9.msdn.com/).

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Cómo configurar el Servicio de registro de dispositivos para Windows Server 2012 RTM

1.  > [!IMPORTANT]
    > **Los siguientes pasos se aplican a la compilación de Windows Server 2012 R2 RTM.**

    Abre una ventana del símbolo del sistema de Windows PowerShell y escribe:

    ```
    Initialize-ADDeviceRegistration
    ```

    Cuando se le solicite una cuenta de servicio, escriba **contosofsgmsa $**.

    Ahora, ejecute el cmdlet de Windows PowerShell.

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  En el servidor ADFS1, en la consola de **Administración de AD FS**, ve a **Directivas de autenticación**. Selecciona **Editar autenticación principal global**. Selecciona la casilla situada junto a **Habilitar la autenticación de dispositivos** y haz clic en **Aceptar**.

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Agregar registros de recursos de host (A) y alias (CNAME) a DNS
En DC1, debes asegurarte de que se crean los siguientes registros del Sistema de nombres de dominio (DNS) para el Servicio de registro de dispositivos.

|Entrada|Tipo|Dirección|
|---------|--------|-----------|
|adfs1|Host (A)|Dirección IP del servidor de AD FS|
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|

Puedes usar el siguiente procedimiento para agregar un registro de recursos de host (A) a los servidores de nombres DNS corporativos para el servidor de federación y el Servicio de registro de dispositivos.

Para completar este procedimiento, debes pertenecer como mínimo al grupo Administradores o un grupo equivalente. Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en el hipervínculo " <https://go.microsoft.com/fwlink/?LinkId=83477> " grupos predeterminados locales y de dominio ( <https://go.microsoft.com/fwlink/p/?LinkId=83477> ).

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Cómo agregar registros de recursos de host (A) y alias (CNAME) a DNS para el servidor de federación

1.  En DC1, desde el Administrador de servidor, en el menú **Herramientas**, haz clic en **DNS** para abrir el complemento de DNS.

2.  En el árbol de consola, expande DC1, expande **Zonas de búsqueda directa**, haz clic con el botón secundario en **contoso.com** y haz clic en **Host nuevo (A o AAAA)**.

3.  En **Nombre,** escribe el nombre que deseas usar para tu granja de AD FS. Para este tutorial, escribe **adfs1**.

4.  En **Dirección IP**, escribe la dirección IP del servidor ADFS1. Haz clic en **Agregar host**.

5.  Haz clic con el botón secundario en **contoso.com** y, a continuación, haz clic en **Nuevo alias (CNAME)**.

6.  En el cuadro de diálogo **Nuevo registro de recursos**, escribe **enterpriseregistration** en el cuadro **Nombre de alias**.

7.  En el nombre de dominio completo (FQDN) del cuadro del host de destino, escribe **adfs1.contoso.com** y haz clic en **Aceptar**.

    > [!IMPORTANT]
    > En una implementación real, si tu empresa tiene varios sufijos de nombre principal de usuario (UPN), debes crear varios registros CNAME, uno para cada sufijo UPN en DNS.

## <a name="step-3-configure-the-web-server-webserv1-and-a-sample-claims-based-application"></a><a name="BKMK_5"></a>Paso 3: Configurar el servidor web (WebServ1) y una aplicación de ejemplo basada en notificaciones
Configure una máquina virtual (WebServ1) instalando el sistema operativo Windows Server 2012 R2 y conéctela al dominio **contoso.com**. Cuando esté unida al dominio, puedes instalar y configurar el rol de servidor web.

Para completar los tutoriales mencionados al principio de este tema, debes tener una aplicación de ejemplo protegida por tu servidor de federación (ADFS1).

Puede descargar el SDK de Windows Identity Foundation ( [https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451) , que incluye una aplicación de ejemplo basada en notificaciones).

Debes llevar a cabo los siguientes pasos para configurar un servidor web con esta aplicación de ejemplo basada en notificaciones.

> [!NOTE]
> Estos pasos se han probado en un servidor Web que ejecuta el sistema operativo Windows Server 2012 R2.

1.  [Instalar el rol de servidor Web y Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [Instalar el SDK de Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [Configurar la aplicación de notificaciones sencillas en IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [Crear una relación de confianza para usuario autenticado en el servidor de federación](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="install-the-web-server-role-and-windows-identity-foundation"></a><a name="BKMK_15"></a>Instalar el rol de servidor web y Windows Identity Foundation

1. > [!NOTE]
   > Debe tener acceso a los medios de instalación de Windows Server 2012 R2.

   Inicie sesión en WebServ1 con <strong>administrator@contoso.com</strong> y la contraseña <strong>pass@word1</strong> .

2. Desde el Administrador del servidor, en la pestaña **Inicio rápido** del icono **Página principal** en la página **Panel**, haz clic en **Agregar roles y características**. Como alternativa, puede hacer clic en **Agregar roles y características** en el menú **Administrar**.

3. En la página **Antes de comenzar** , haga clic en **Siguiente**.

4. En la página **Seleccionar tipo de instalación**, haga clic en **Instalación basada en características o en roles** y, a continuación, en **Siguiente**.

5. En la página **Seleccionar servidor de destino**, haz clic en **Seleccionar un servidor del grupo de servidores**, comprueba que está seleccionado el equipo de destino y haz clic en **Siguiente**.

6. En la página **Seleccionar roles de servidor**, selecciona la casilla situada junto a **Servidor web (IIS)**, haz clic en **Agregar características** y, a continuación, haz clic en **Siguiente**.

7. En la página **Seleccionar características**, selecciona **Windows Identity Foundation 3.5** y haz clic en **Siguiente**.

8. En la página **Rol Servidor web (IIS)**, haz clic en **Siguiente**.

9. En la página **Seleccionar servicios de rol**, selecciona y expande **Desarrollo de aplicaciones**. Selecciona **ASP.NET 3.5**, haz clic en **Agregar características** y, a continuación, haz clic en **Siguiente**.

10. En la página **Confirmar selecciones de instalación**, haz clic en **Especifique una ruta de acceso de origen alternativa**. Escriba la ruta de acceso al directorio SxS que se encuentra en el medio de instalación de Windows Server 2012 R2. Por ejemplo D:SourcesSxs. Haz clic en **Aceptar** y, a continuación, en **Instalar**.

### <a name="install-windows-identity-foundation-sdk"></a><a name="BKMK_13"></a>Instalar el SDK de Windows Identity Foundation

1.  Ejecute WindowsIdentityFoundation-SDK-3.5.msi para instalar el SDK 3,5 de Windows Identity Foundation ( https://www.microsoft.com/download/details.aspx?id=4451) . Elige todas las opciones predeterminadas.

### <a name="configure-the-simple-claims-app-in-iis"></a><a name="BKMK_9"></a>Configurar la aplicación de notificaciones sencillas en IIS

1.  Instala un certificado SSL válido en el almacén de certificados del equipo. El certificado debe contener el nombre de tu servidor web, **webserv1.contoso.com**.

2.  Copie el contenido de C:Program Files (x86) Windows Identity Foundation SDKv 3.5 SamplesQuick StartWeb ApplicationPassiveRedirectBasedClaimsAwareWebApp en C:InetpubClaimapp.

3.  Edita el archivo **Default.aspx.cs** para que no se produzca el filtrado de notificaciones. Este paso se lleva a cabo para garantizar que la aplicación de ejemplo muestra todas las notificaciones que emite el servidor de federación. Haga lo siguiente:

    1.  Abre **Default.aspx.cs** en un editor de texto.

    2.  Busque el archivo para la segunda instancia de `ExpectedClaims`.

    3.  Convierta en comentario todo el argumento `IF` y el contenido de las llaves. Indique los comentarios escribiendo "//" (sin las comillas) al principio de una línea.

    4.  El argumento `FOREACH` debe parecerse a este ejemplo de código.

        ```
        Foreach (claim claim in claimsIdentity.Claims)
        {
           //Before showing the claims validate that this is an expected claim
           //If it is not in the expected claims list then don't show it
           //if (ExpectedClaims.Contains( claim.ClaimType ) )
           // {
              writeClaim( claim, table );
           //}
        }

        ```

    5.  Guarda y cierra **Default.aspx.cs**.

    6.  Abre **web.config** en un editor de texto.

    7.  Elimina toda la sección `<microsoft.identityModel>` . Elimine todo desde `including <microsoft.identityModel>` hasta `</microsoft.identityModel>`incluido.

    8.  Guarda y cierra **web.config**.

4.  **Configurar el Administrador de IIS**

    1.  Abra el **Administrador de Internet Information Services (IIS)** .

    2.  Ve a **Grupos de aplicaciones**, haz clic con el botón secundario en **DefaultAppPool** y selecciona **Configuración avanzada**. Configura **Cargar perfil de usuario** como **Verdadero** y haz clic en **Aceptar**.

    3.  Haz clic con el botón secundario en **DefaultAppPool** y selecciona **Configuración básica**. Cambia **Versión .NET CLR** a **Versión .NET CLR v2.0.50727**.

    4.  Haz clic con el botón secundario en **Sitio web predeterminado** y selecciona **Modificar enlaces**.

    5.  Agrega un enlace **HTTPS** al puerto **443** con el certificado SSL que has instalado.

    6.  Haz clic con el botón secundario en **Sitio web predeterminado** y selecciona **Agregar aplicación**.

    7.  Establezca el alias en **claimapp** y la ruta de acceso física en **c:inetpubclaimapp**.

5.  Para configurar **claimapp** de modo que funcione con tu servidor de federación, haz lo siguiente:

    1.  Ejecute FedUtil.exe, que se encuentra en **C:Program Files (x86) Windows Identity Foundation sdkv 3.5**.

    2.  Establezca la ubicación de configuración de la aplicación en **C:inetputclaimappweb.config** y establezca el URI de la aplicación en la dirección URL del sitio, **https://webserv1.contoso.com /claimapp/**. Haga clic en **Next**.

    3.  Seleccione **usar un STS existente** y vaya a la dirección URL de metadatos del servidor de AD FS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml** . Haga clic en **Next**.

    4.  Selecciona **Deshabilitar la validación de la cadena de certificados** y haz clic en **Siguiente**.

    5.  Selecciona **Sin cifrado** y haz clic en **Siguiente**. En la página **Notificaciones ofrecidas**, haga clic en **Siguiente**.

    6.  Selecciona la casilla situada junto a **Programar una tarea para realizar diariamente actualizaciones de metadatos de WS-Federation**. Haga clic en **Finalizar**

    7.  La aplicación de ejemplo ya está configurada. Si prueba la dirección URL de la aplicación **https://webserv1.contoso.com/claimapp** , debe redirigirle a su servidor de Federación. El servidor de federación debería mostrar una página de error, ya que todavía no has configurado la relación de confianza para usuario autenticado. En otras palabras, esta aplicación de prueba no se ha protegido mediante AD FS.

Ahora debe proteger la aplicación de ejemplo que se ejecuta en el servidor Web con AD FS. Puedes hacerlo agregando una relación de confianza para usuario autenticado en el servidor de federación (ADFS1). Para ver un vídeo, consulte [Serie de vídeos prácticos sobre los Servicios de federación de Active Directory: agregar una relación de confianza para usuario autenticado](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search).

### <a name="create-a-relying-party-trust-on-your-federation-server"></a><a name="BKMK_11"></a>Crear una relación de confianza para usuario autenticado en el servidor de federación

1.  En el servidor de federación (ADFS1), en la **Consola de administración de AD FS**, ve a **Relaciones de confianza para usuario autenticado** y haz clic en **Agregar relación de confianza para usuario autenticado**.

2.  En la página **Seleccionar origen de datos**, selecciona **Importar los datos acerca del usuario autenticado publicados en línea o en una red local**, escribe la dirección URL de los metadatos para **claimapp** y haz clic en **Siguiente**. Al ejecutarse FedUtil.exe, creó un archivo de metadatos .xml. Se encuentra en **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml** .

3.  En la página **Especificar nombre para mostrar**, especifica el **nombre para mostrar** de la relación de confianza para usuario autenticado, **claimapp**, y haz clic en **Siguiente**.

4.  En la página **¿Configurar ahora autenticación multifactor?**, selecciona **No deseo especificar ahora una configuración de autenticación multifactor para esta relación de confianza para usuario autenticado** y haz clic en **Siguiente**.

5.  En la página **Elegir reglas de autorización de emisión**, selecciona **Permitir que todos los usuarios accedan a este usuario autenticado** y haz clic en **Siguiente**.

6.  En la página **Listo para agregar confianza**, haz clic en **Siguiente**.

7.  En el cuadro de diálogo **Editar reglas de notificación**, haz clic en **Agregar regla**.

8.  En la página **Elegir el tipo de regla**, selecciona **Enviar notificaciones mediante una regla personalizada** y haz clic en **Siguiente**.

9. En la página **Configurar regla de notificación**, en el cuadro **Nombre de regla de notificación**, escribe **All Claims**. En el cuadro **Regla personalizada**, escribe la siguiente regla de notificación.

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. Haz clic en **Finalizar** y, a continuación, en **Aceptar**.

## <a name="step-4-configure-the-client-computer-client1"></a><a name="BKMK_10"></a>Paso 4: Configurar el equipo cliente (Client1)
Configure otra máquina virtual e instale Windows 8.1. Esta máquina virtual deber encontrarse en la misma red virtual que las demás máquinas. Esta máquina NO debe estar unida al dominio Contoso.

El cliente DEBE confiar en el certificado SSL que se usó para el servidor de federación (ADFS1), que configuró en [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). También debe poder validar la información de revocación de certificados de dicho certificado.

Asimismo, debes configurar y utilizar una cuenta Microsoft para iniciar sesión en Client1.

## <a name="see-also"></a>Consulte también
[Servicios de federación de Active Directory (AD FS) How-To serie de vídeos: instalación de una granja](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search) 
 de servidores de AD FS [Servicios de federación de Active Directory (AD FS) How-To serie de vídeos: actualizar certificados](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search) 
 [Servicios de federación de Active Directory (AD FS) How-To serie de vídeos: agregar una relación de confianza](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search) 
 para usuario autenticado [Servicios de federación de Active Directory (AD FS) How-To serie de vídeos: habilitar el servicio](https://channel9.msdn.com/) 
 de registro de dispositivos [Servicios de federación de Active Directory (AD FS) How-To serie de vídeos: instalar el proxy de aplicación web](https://channel9.msdn.com/Search?term=Active%20Directory%20Federation%20Services#pubDate=year&ch9Search)

