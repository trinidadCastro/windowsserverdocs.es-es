---
ms.assetid: 276a7f7d-5faa-4c00-a51c-3fa511fe52f9
title: Configurar un entorno de laboratorio de AD FS
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 53d0e24f7fcb9efc64406dc6ed01f5bb1deb2277
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="set-up-an-ad-fs-lab-environment"></a>Configurar un entorno de laboratorio de AD FS

>Se aplica a: Windows Server 2012 R2

En este tema se describe los pasos para configurar un entorno de prueba que puede usarse para completar los tutoriales de las siguientes guías Tutorial:  
  
-   [Tutorial: Unirse un área de trabajo con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)  
  
-   [Tutorial: Unión a un área de trabajo con un dispositivo de Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)  
  
-   [Guía paso a paso: Administrar el riesgo con Control de acceso condicional](Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)  
  
-   [Guía paso a paso: Administrar el riesgo con la autenticación multifactor adicional para aplicaciones con información confidencial](Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)  
  
> [!NOTE]  
> No recomendamos instalar el servidor web y el servidor de federación en el mismo equipo.  
  
Para configurar este entorno de prueba, completa los siguientes pasos:  
  
1.  [Paso 1: Configurar el controlador de dominio (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)  
  
2.  [Paso 2: Configurar el servidor de federación (ADFS1) con el servicio de registro de dispositivo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)  
  
3.  [Paso 3: Configurar el servidor web (WebServ1) y una aplicación de muestra basada en notificaciones](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)  
  
4.  [Paso 4: Configurar el equipo cliente (CLIENTE1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)  
  
## <a name="BKMK_1"></a>Paso 1: Configurar el controlador de dominio (DC1)  
Para los fines de este entorno de prueba, puedes llamar a tu dominio de Active Directory raíz **contoso.com** y especificar ** pass@word1 ** como la contraseña de administrador.  
  
-   Instalar el servicio de rol de AD DS y los servicios de dominio de Active Directory (AD DS) para hacer que el equipo un controlador de dominio en Windows Server 2012 R2. Esta acción actualiza el esquema de AD DS como parte de la creación de controlador de dominio. Para obtener más información e instrucciones paso a paso, consulta[https://technet.microsoft.com/ library/hh472162.aspx ](https://technet.microsoft.com/library/hh472162.aspx).  
  
### <a name="BKMK_2"></a>Crear cuentas de prueba de Active Directory  
Después de que el controlador de dominio es funcional, puedes crear cuentas de usuario de grupo y prueba de prueba en este dominio y agregar la cuenta de usuario a la cuenta de grupo. Usa estas cuentas para completar los tutoriales de las guías de tutorial que se hace referencia anteriormente en este tema.  
  
Crear las siguientes cuentas:  
  
-   Usuario: **Robert Hatley** con las credenciales siguientes: nombre de usuario: **RobertH** y la contraseña:**P@ssword**  
  
-   Grupo: **Finanzas**  
  
Para obtener información sobre cómo crear cuentas de usuario y grupo en Active Directory (AD), consulta [https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx ](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).  
  
Agregar el **Robert Hatley** cuenta a la **finanzas** grupo. Para obtener información sobre cómo agregar un usuario a un grupo en Active Directory, consulte [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx ](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx).  
  
### <a name="create-a-gmsa-account"></a>Crear una cuenta GMSA  
La cuenta de la cuenta de servicio administrada (GMSA) de grupo es necesaria durante la instalación de los servicios de federación de Active Directory (AD FS) y configuración.  
  
##### <a name="to-create-a-gmsa-account"></a>Para crear una cuenta GMSA  
  
1.  Abre una ventana de comandos de Windows PowerShell y escribe:  
  
    ```  
    Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)  
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com  
  
    ```  
  
## <a name="BKMK_4"></a>Paso 2: Configurar el servidor de federación (ADFS1) mediante el servicio de registro del dispositivo  
Para configurar otra máquina virtual, instalar Windows Server 2012 R2 y conectarlo al dominio **contoso.com**. Configurar el equipo después de que se ha unido al dominio y, a continuación, pasar a instalar y configurar el rol de AD FS.  
  
Para ver un vídeo, [federación servicios procedimientos vídeo serie de Active Directory: instalar una granja de servidores de AD FS ](https://technet.microsoft.com/video/dn469436).  
  
### <a name="install-a-server-ssl-certificate"></a>Instalar un certificado de servidor SSL  
Debes instalar un certificado de capa de sockets seguros (SSL) del servidor en el servidor ADFS1 en el almacén de equipo local. El certificado debe tener los siguientes atributos:  
  
-   Nombre (CN) de asunto: adfs1.contoso.com  
  
-   Nombre alternativo del (dominio DNS) de asunto: adfs1.contoso.com  
  
-   Nombre alternativo del (dominio DNS) de asunto: enterpriseregistration.contoso.com  
  
Para obtener más información sobre la configuración de certificados SSL, consulta [configurar SSL/TLS en un sitio Web en el dominio con una entidad de certificación de la empresa ](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx).  
  
[Serie de vídeos paso a paso de servicios de federación de Active Directory: Actualización de certificados ](https://technet.microsoft.com/video/adfs-updating-certificates).  
  
### <a name="install-the-ad-fs-server-role"></a>Instalar el rol de servidor de AD FS  
  
##### <a name="to-install-the-federation-service-role-service"></a>Para instalar el servicio de rol de servicios de federación  
  
1.  Iniciar sesión en el servidor mediante la cuenta de administrador de dominio administrator@contoso.com.  
  
2.  Inicia el administrador del servidor. Para iniciar el administrador del servidor, haz clic en **administrador del servidor** en el Windows **inicio** pantalla o haz clic en **administrador del servidor** en la barra de tareas de Windows en el escritorio de Windows. En la **inicio rápido** ficha de la **bienvenida** icono en la **panel** página, haz clic en **agregar roles y características**. Como alternativa, puedes hacer clic en **agregar Roles y características** en la **administrar** menú.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basada en rol o característica**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, comprueba que el equipo de destino está seleccionado y, a continuación, haz clic en **siguiente**.  
  
6.  En la **seleccionar roles de servidor** página, haz clic en **los servicios de federación de Active Directory**y, a continuación, haz clic en **siguiente**.  
  
7.  En la **Select features** página, haz clic en **siguiente**.  
  
8.  En la **los servicios de federación de Active Directory (AD FS)** página, haz clic en **siguiente**.  
  
9. Después de comprobar la información en la **Confirmar selecciones de instalación** página, seleccione la **reiniciar automáticamente el servidor de destino en caso necesario** y, a continuación, haz clic en **instalar**.  
  
10. En la **progreso de la instalación** página, comprueba que todo lo que ha instalado correctamente y, a continuación, haz clic en **cerrar**.  
  
### <a name="configure-the-federation-server"></a>Configurar el servidor de federación  
El siguiente paso es configurar el servidor de federación.  
  
##### <a name="to-configure-the-federation-server"></a>Para configurar el servidor de federación  
  
1.  En el administrador del servidor **panel** página, haz clic en el **notificaciones** marcar y, a continuación, haz clic en **configurar el servicio de federación en el servidor**.  
  
    La **Asistente para la configuración a servicios de Active Directory federación** se abre.  
  
2.  En la **bienvenida** página, seleccione **crea el primer servidor de federación en una granja de servidores de federación**y, a continuación, haz clic en **siguiente**.  
  
3.  En la **conectarse a AD DS** página, especifica una cuenta con derechos de administrador de dominio para el **contoso.com** dominio de Active Directory que este equipo está unido a y, a continuación, haz clic en **siguiente**.  
  
4.  En la **especificar propiedades del servicio** página, haz lo siguiente y, a continuación, haz clic en **siguiente**:  
  
    -   Importa el certificado SSL que has obtenido anteriormente. Este certificado es el certificado de autenticación del servicio requerido. Busca la ubicación de su certificado SSL.  
  
    -   Para proporcionar un nombre para el servicio de federación, escriba **adfs1.contoso.com**. Este valor es el mismo valor que proporcionaste cuando inscribir un certificado SSL en los servicios de certificados de Active Directory (AD CS).  
  
    -   Para proporcionar un nombre para mostrar para tu servicio de federación, escriba **Contoso Corporation**.  
  
5.  En la **especificar la cuenta de servicio** página, seleccione **utilizar una cuenta de usuario de dominio existente o grupo de la cuenta de servicio administrada**y, a continuación, especifica la cuenta GMSA **fsgmsa** que creó cuando creaste el controlador de dominio.  
  
6.  En la **especificar base de datos de configuración** página, seleccione **crear una base de datos en este servidor mediante Windows Internal Database**y, a continuación, haz clic en **siguiente**.  
  
7.  En la **opciones de revisión** página, compruebe las selecciones de configuración y, a continuación, haz clic en **siguiente**.  
  
8.  En la **requisito previo comprueba** página, comprueba que todas las comprobaciones de requisitos previos se completaron correctamente y, a continuación, haz clic en **configurar**.  
  
9. En la **resultados** página, revisar los resultados, comprueba si se ha completado correctamente la configuración y, a continuación, haz clic en **siguiente pasos necesarios para completar la implementación de servicios de federación**.  
  
### <a name="configure-device-registration-service"></a>Configurar el servicio de registro del dispositivo  
El siguiente paso es configurar el servicio de registro de dispositivo en el servidor ADFS1. Para ver un vídeo, [federación servicios procedimientos vídeo serie de Active Directory: habilitar el servicio de registro de dispositivo ](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service).  
  
##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Para configurar el dispositivo de registro de servicio para Windows Server 2012 RTM  
  
1.  > [!IMPORTANT]  
    > **El siguiente paso afecta a la compilación de Windows Server 2012 R2 RTM.**  
  
    Abre una ventana de comandos de Windows PowerShell y escribe:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
    Cuando se te pide una cuenta de servicio, escribe **contosofsgmsa$**.  
  
    Ahora, ejecuta el cmdlet de Windows PowerShell.  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  En el servidor ADFS1, en la **AD FS administración** de consola, navega a **directivas de autenticación**. Selecciona **Editar autenticación principal Global**. Selecciona la casilla de verificación junto a **habilitar la autenticación de dispositivo**y, a continuación, haz clic en **Aceptar**.  
  
### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Agregar Host (A) y registros de recursos de Alias (CNAME) a DNS  
En DC1, debes asegurarte de que se crean los siguientes registros de sistema de nombres de dominio (DNS) para el servicio de registro del dispositivo.  
  
|Entrada|Tipo|Dirección|  
|---------|--------|-----------|  
|adfs1|Host (A)|Dirección IP del servidor de AD FS|  
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|  
  
Puedes usar el siguiente procedimiento para agregar un registro de recursos de host (A) a los servidores de nombre DNS corporativos del servidor de federación y el servicio de registro del dispositivo.  
  
Pertenencia al grupo de administradores o equivalente es el requisito mínimo para completar este procedimiento. Revisar detalles sobre el uso de las cuentas y adecuadas pertenencias a grupos en el hipervínculo "https://go.microsoft.com/fwlink/?LinkId=83477" Local y los grupos de dominio predeterminada (https://go.microsoft.com/fwlink/p/?LinkId=83477).  
  
##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Agregar un host (A) y registros de recursos de alias (CNAME) en DNS para el servidor de federación  
  
1.  En DC1, desde el administrador del servidor, en la **herramientas** menú, haz clic en **DNS** para abrir el complemento de DNS.  
  
2.  En el árbol de consola, expanda DC1, **zonas de búsqueda directa**, haz clic en **contoso.com**y, a continuación, haz clic en **nuevo Host (A o AAAA)**.  
  
3.  En **nombre,** escribe el nombre que quieras usar para el conjunto de AD FS. Para este tutorial, escribe **adfs1**.  
  
4.  En **dirección IP**, escribe la dirección IP del servidor ADFS1. Haz clic en **agregar Host**.  
  
5.  Haz clic en **contoso.com**y, a continuación, haz clic en **nuevo Alias (CNAME)**.  
  
6.  En la **nuevo registro de recursos** cuadro de diálogo, escribe **enterpriseregistration** en la **nombre Alias** cuadro.  
  
7.  En el dominio nombre completo (FQDN) del cuadro de host de destino, escriba **adfs1.contoso.com**y, a continuación, haz clic en **Aceptar**.  
  
    > [!IMPORTANT]  
    > En una implementación real, si tu empresa tiene varios sufijos de nombre principal (UPN) de usuario, debes crear varios registros CNAME, uno para cada uno de esos sufijos en DNS.  
  
## <a name="BKMK_5"></a>Paso 3: Configurar el servidor web (WebServ1) y una aplicación de muestra basada en notificaciones  
Configurar una máquina virtual (WebServ1) al instalar el sistema operativo de Windows Server 2012 R2 y conectar al dominio **contoso.com**. Después de está unido al dominio, puede continuar para instalar y configurar el rol de servidor Web.  
  
Para completar los tutoriales que se hizo referencia anteriormente en este tema, debes tener una aplicación de muestra que está protegida por el servidor de federación (ADFS1).  
  
Puedes descargar el SDK de Windows identidad Foundation ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451), que incluye una aplicación de muestra basada en notificaciones.  
  
Debes completar los siguientes pasos para configurar un servidor web con esta aplicación de muestra basada en notificaciones.  
  
> [!NOTE]  
> Estos pasos se han probado en un servidor web que se ejecuta el sistema operativo de Windows Server 2012 R2.  
  
1.  [Instalar el rol de servidor Web y la identidad de Windows Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)  
  
2.  [Instalar el SDK de Windows identidad Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)  
  
3.  [Configurar la aplicación reclamaciones simples en IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)  
  
4.  [Crear una confianza de terceros de confianza en el servidor de federación](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)  
  
### <a name="BKMK_15"></a>Instalar el rol de servidor Web y la identidad de Windows Foundation  
  
1.  > [!NOTE]  
    > Debes tener acceso a los medios de instalación de Windows Server 2012 R2.  
  
    Inicio de sesión WebServ1 usando ** administrator@contoso.com ** y la contraseña ** pass@word1 **.  
  
2.  Desde el administrador del servidor, en la **inicio rápido** ficha de la **bienvenida** icono en la **panel** página, haz clic en **agregar roles y características**. Como alternativa, puedes hacer clic en **agregar Roles y características** en la **administrar** menú.  
  
3.  En la **antes de comenzar** página, haz clic en **siguiente**.  
  
4.  En la **selecciona el tipo de instalación** página, haz clic en **instalación basada en rol o característica**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **servidor de destino selecciona** página, haz clic en **seleccionar un servidor desde el grupo de servidores**, comprueba que el equipo de destino está seleccionado y, a continuación, haz clic en **siguiente**.  
  
6.  En la **seleccionar roles de servidor** página, selecciona la casilla de verificación junto a **servidor Web (IIS)**, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
7.  En la **Select features** , seleccione **identidad de Windows Foundation 3.5**y, a continuación, haz clic en **siguiente**.  
  
8.  En la **rol de servidor Web (IIS)** página, haz clic en **siguiente**.  
  
9. En la **seleccione los servicios de rol** página, selecciona y expande **desarrollo de aplicaciones**. Selecciona **ASP.NET 3.5**, haz clic en **agregar características**y, a continuación, haz clic en **siguiente**.  
  
10. En la **Confirmar selecciones de instalación** página, haz clic en **especificar una ruta de acceso de origen alternativo**. Escribe la ruta de acceso al directorio Sxs que se encuentra en los medios de instalación de Windows Server 2012 R2. Por ejemplo, D:SourcesSxs. Haz clic en **Aceptar**y, a continuación, haz clic en **instalar**.  
  
### <a name="BKMK_13"></a>Instalar el SDK de Windows identidad Foundation  
  
1.  Ejecutar WindowsIdentityFoundation SDK-3.5.msi para instalar el SDK de Windows identidad Foundation 3.5 (https://www.microsoft.com/download/details.aspx?id=4451). Elegir todas las opciones predeterminadas.  
  
### <a name="BKMK_9"></a>Configurar la aplicación reclamaciones simples en IIS  
  
1.  Instalar un certificado SSL válido en el almacén de certificados de equipo. El certificado debe contener el nombre del servidor web, **webserv1.contoso.com**.  
  
2.  Copia el contenido de c archivos (x86) Windows identidad Foundation SDKv3.5SamplesQuick StartWeb ApplicationPassiveRedirectBasedClaimsAwareWebApp en C:InetpubClaimapp.  
  
3.  Editar el **Default.aspx.cs** para que ninguna notificación filtrado toma el lugar del archivo. Este paso se realiza para asegurarse de que la aplicación de ejemplo muestra todas las notificaciones que se emiten por el servidor de federación. Haz lo siguiente:  
  
    1.  Abre **Default.aspx.cs** en un editor de texto.  
  
    2.  Buscar el archivo de la segunda instancia de `ExpectedClaims`.  
  
    3.  Comenta toda la `IF` declaración y sus llaves. Indicar comentarios escribiendo "/ /" (sin las comillas) al comienzo de una línea.  
  
    4.  Tu `FOREACH` declaración debería ser similar a este ejemplo de código.  
  
        ```  
        Foreach (claim claim in claimsIdentity.Claims)  
        {  
           //Before showing the claims validate that this is an expected claim  
           //If it is not in the expected claims list then don’t show it  
           //if (ExpectedClaims.Contains( claim.ClaimType ) )  
           // {  
              writeClaim( claim, table );  
           //}  
        }  
  
        ```  
  
    5.  Guarda y cierra **Default.aspx.cs**.  
  
    6.  Abre **web.config** en un editor de texto.  
  
    7.  Quitar todo el `<microsoft.identityModel>` sección. Quitar todo desde `including <microsoft.identityModel>` y hasta e incluyendo `</microsoft.identityModel>`.  
  
    8.  Guarda y cierra **web.config**.  
  
4.  **Configurar el Administrador de IIS**  
  
    1.  Abre **Internet Information Services (IIS) Manager**.  
  
    2.  Ve a **Application Pools**, haz clic en **DefaultAppPool** seleccionar **configuración avanzada**. Establecer **perfil de usuario de carga** a **True**y, a continuación, haz clic en **Aceptar**.  
  
    3.  Haz clic en **DefaultAppPool** seleccionar **configuración básica**. Cambiar la **versión de .NET CLR** a **.NET CLR versión v2.0.50727**.  
  
    4.  Haz clic en **Default Web Site** seleccionar **Editar enlaces**.  
  
    5.  Agregar una **HTTPS** enlace al puerto **443** con el certificado SSL que ha instalado.  
  
    6.  Haz clic en **Default Web Site** seleccionar **Agregar aplicación**.  
  
    7.  Establece el alias en **claimapp** y la ruta de acceso física **c:inetpubclaimapp**.  
  
5.  Para configurar **claimapp** para trabajar con el servidor de federación, haz lo siguiente:  
  
    1.  Ejecutar FedUtil.exe, que se encuentra en **c archivos (x86) Windows identidad Foundation SDKv3.5**.  
  
    2.  Establece la ubicación de la configuración de aplicación en **C:inetputclaimappweb.config** y establece el URI de aplicación en la dirección URL del sitio, **https://webserv1.contoso.com /claimapp/**. Haz clic en **siguiente**.  
  
    3.  Selecciona **Use STS existente** y vaya a la dirección URL de metadatos de tu servidor de AD FS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml**. Haz clic en **siguiente**.  
  
    4.  Selecciona **deshabilitar la validación de la cadena de certificados**y, a continuación, haz clic en **siguiente**.  
  
    5.  Selecciona **sin cifrado**y, a continuación, haz clic en **siguiente**. En la **ofrece reclamaciones** página, haz clic en **siguiente**.  
  
    6.  Selecciona la casilla de verificación junto a **programar una tarea para realizar actualizaciones diarias de metadatos de federación de WS**. Haz clic en **finalizar**.  
  
    7.  La aplicación de muestra ahora está configurada. Si la dirección URL de la aplicación que probar **https://webserv1.contoso.com/claimapp**, se le debe redirigir al servidor de federación. El servidor de federación debe mostrar una página de error porque aún no has configurado la confianza de terceros de confianza. En otras palabras, no ha protegido esta aplicación de prueba por AD FS.  
  
Ahora debes proteger la aplicación de muestra que se ejecuta en el servidor web con AD FS. Para ello, agrega una confianza de terceros de confianza en el servidor de federación (ADFS1). Para ver un vídeo, [federación servicios procedimientos vídeo serie de Active Directory: agrega una confianza de terceros confiar ](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust).  
  
### <a name="BKMK_11"></a>Crear una confianza de terceros de confianza en el servidor de federación  
  
1.  En el servidor de federación (ADFS1), en la **consola de administración de AD FS**, navegar a **confiar confía en las partes**y, a continuación, haz clic en **agregar confiar terceros de confianza**.  
  
2.  En la **Seleccionar origen de datos** , seleccione **importar datos sobre el usuario de confianza publicación en línea o en una red local**, escribe la dirección URL de metadatos para **claimapp**y, a continuación, haz clic en **siguiente**. Ejecuta FedUtil.exe crea un archivo .xml de metadatos. Se encuentra en   
    **https://webserv1.contoso.com/claimapp/FederationMetadata/2007-06/federationmetadata.XML**.  
  
3.  En la **especificar nombre para mostrar** página, especifica la **nombre para mostrar** para la confianza de terceros confianza, **claimapp**y, a continuación, haz clic en **siguiente**.  
  
4.  ¿En la **configurar la autenticación multifactor ahora? ** página, seleccione **no quieras especificar la configuración de la autenticación multifactor para esta confianza de terceros de confianza en este momento**y, a continuación, haz clic en **siguiente**.  
  
5.  En la **elegir las reglas de autorización de emisión** página, seleccione **permitir que todos los usuarios para acceder a este usuario de confianza**y, a continuación, haz clic en **siguiente**.  
  
6.  En la **listo para agregar confianza** página, haz clic en **siguiente**.  
  
7.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Agregar regla**.  
  
8.  En la **elegir un tipo de regla** página, seleccione **enviar notificaciones usando una regla personalizada**y, a continuación, haz clic en **siguiente**.  
  
9. En la **configurar la regla de Reclamación** página, en la **nombre de la regla de Reclamación** , escriba **todas las reclamaciones**. En la **regla personalizada** , escriba la siguiente regla de notificación.  
  
    ```  
    c:[ ]  
    => issue(claim = c);  
  
    ```  
  
10. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar**.  
  
## <a name="BKMK_10"></a>Paso 4: Configurar el equipo cliente (CLIENTE1)  
Configurar otra máquina virtual e instalar Windows 8.1. Esta máquina virtual debe estar en la misma red virtual que los demás equipos. Este equipo no debe estar unido al dominio de Contoso.  
  
El cliente debe confiar en el certificado SSL que se usa para el servidor de federación (ADFS1), que estableciste en [paso 2: configurar el servidor de federación (ADFS1) con el servicio de registro de dispositivo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). También debe ser capaz de validar la información de revocación de certificados para el certificado.  
  
También debes configurar y usar una cuenta de Microsoft para iniciar sesión en CLIENTE1.  
  
## <a name="see-also"></a>Consulta también  
[Active Directory federación servicios procedimientos serie de vídeos: Instalar una granja de servidores de FS de anuncios](https://technet.microsoft.com/video/dn469436)  
[Serie de vídeos paso a paso de servicios de federación de Active Directory: Actualización de certificados](https://technet.microsoft.com/video/adfs-updating-certificates)  
[Active Directory federación servicios procedimientos serie de vídeos: Agregar un usuario de confianza](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)  
[Serie de vídeos paso a paso de servicios de federación de Active Directory: Habilitar el servicio de registro de dispositivo](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)  
[Serie de vídeos paso a paso de servicios de federación de Active Directory: Instalar el Proxy de aplicación Web](https://technet.microsoft.com/video/dn469438)  
  
