---
title: Implementar la impresión en la nube híbrida de Windows Server
description: Cómo configurar la impresión en la nube híbrida de Microsoft
ms.prod: windows-server
ms.technology: windows server 2016
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
ms.topic: how-to
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: fe1f2b11921950ea725cb996ce58e75033aaae4a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470210"
---
# <a name="deploy-windows-server-hybrid-cloud-print"></a>Implementar la impresión en la nube híbrida de Windows Server

>Se aplica a: Windows Server 2016

En este tema, para los administradores de ti, se describe la implementación de un extremo a otro de la solución de impresión en la nube híbrida de Microsoft (HCP). Esta solución se encuentra en la parte superior de los servidores de Windows existentes que se ejecutan como servidor de impresión y permite que los dispositivos de Azure Active Directory (Azure AD) Unidos y administrados por MDM detecten e impriman en las impresoras administradas de la organización.

## <a name="pre-requisites"></a>Requisitos previos

Hay una serie de suscripciones, servicios y equipos que debe adquirir antes de iniciar esta instalación. Los pasos son los siguientes:

- Azure AD suscripción premium.

  Consulte Introducción a [una suscripción de Azure](https://azure.microsoft.com/trial/get-started-active-directory/) para una suscripción de prueba a Azure.

- Servicio MDM, como Intune.

  Consulte [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) para obtener una suscripción de prueba a Intune.

- Windows Server 2016 o posterior que ejecuta Active Directory.

  Consulte [Step-by-Step: Configuring up Active Directory in Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/) for Help Configuring up Active Directory.

- Una máquina de Windows Server 2016 o posterior unida a un dominio dedicada que se ejecute como servidor de impresión.

- Un equipo de Windows Server 2016 o posterior unido a un dominio dedicado que se ejecute como servidor de conector.

  Consulte [descripción Azure ad conectores del proxy de aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors) para obtener más información.

- Una actualización de Windows 10 Fall Creator o un equipo posterior para publicar impresoras.

- Nombre de dominio público.

  Puede usar el nombre de dominio que ha creado Azure (*domainname*. onmicrosoft.com) o adquirir su propio nombre de dominio. Consulte [Agregar un nombre de dominio personalizado mediante el portal de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain).

## <a name="deployment-steps"></a>Pasos de implementación

Los pasos siguientes son para una implementación típica de impresión en la nube híbrida.

### <a name="step-1---install-azure-ad-connect"></a>Paso 1: instalar Azure AD Connect

1. Azure AD Connect sincroniza Azure AD con AD local. En el equipo de Windows Server con Active Directory, descargue e instale el software de Azure AD Connect con la configuración rápida. Consulte [Introducción a Azure ad Connect mediante la configuración rápida](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express).

### <a name="step-2---install-application-proxy"></a>Paso 2: instalar el proxy de aplicación

1. Proxy de aplicación permite a los usuarios de su organización acceder a aplicaciones locales desde la nube. Instale el proxy de aplicación en el servidor del conector.
    - Para obtener instrucciones de instalación, consulte [Tutorial: agregar una aplicación local para el acceso remoto a través del proxy de aplicación en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).
    - Se recomienda un grupo de conectores dedicado si la organización tiene una topología de red compleja. Consulte [publicación de aplicaciones en redes y ubicaciones independientes mediante grupos de conectores](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connector-groups).

### <a name="step-3---register-and-configure-applications"></a>Paso 3: registro y configuración de aplicaciones

Para habilitar la comunicación autenticada con los servicios HCP, es necesario crear 3 aplicaciones: 2 aplicaciones web para representar los dos servicios HCP y 1 aplicación nativa para comunicarse con esos servicios.

1. Inicie sesión en Azure Portal para registrar aplicaciones Web.
    - En Azure Active Directory, vaya a **registros de aplicaciones**  >  **nuevo registro**.

    ![Registro de aplicación de AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration.png)

    - Escriba un nombre de aplicación para el servicio de detección de Mopria. Haga clic en **registrar** para finalizar.

    ![Registro de aplicaciones de AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria.png)

    - Repita el servicio de impresión en la nube empresarial.
    - Repita el procedimiento para la aplicación nativa.
    - Las tres aplicaciones se deben mostrar en **registros de aplicaciones**.

    ![Registro de aplicaciones de AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-AllApps.png)

2. Exponga la API de las dos aplicaciones Web.
    - Mientras sigue en la hoja **registros de aplicaciones** , haga clic en la aplicación de servicio de detección Mopria, seleccione **exponer una API**y luego haga clic en **establecer** junto a URI de ID. de aplicación.

    ![API 1 de AAD exponiendo](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI.png)

    - Haga clic en **Guardar** sin cambiar el valor predeterminado para URI de ID. de aplicación. Este valor solo debe establecerse ahora y se cambiará más adelante.

    ![API 2 de AAD exponiendo](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-Save.png)

    - Haga clic en **Agregar un ámbito**.

    ![API 3 de AAD exponiendo](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-AddScope.png)

    - Proporcione un nombre de ámbito, permita a los administradores y a los usuarios dar su consentimiento, escriba la descripción del consentimiento y, a continuación, haga clic en **Agregar ámbito** a fin.

    ![API 4 de AAD exponiendo](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-ScopeName.png)

    - Repita el servicio de impresión en la nube empresarial. Use un nombre de ámbito y una descripción de consentimiento diferentes.

    ![API 5 de AAD expóngalo](../media/hybrid-cloud-print/AAD-AppRegistration-ECP-ExposeAPI-ScopeName.png)

3. Adición de permisos de API
    - Vuelva a Registros de aplicaciones hoja. Haga clic en la aplicación nativa y seleccione permisos de la API. Haga clic en **Agregar un permiso**.

    ![Permiso de API de AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission.png)

    - Cambie a las **API que usa mi organización**y luego use el cuadro de búsqueda para encontrar el servicio de detección de Mopria agregado anteriormente. Haga clic en el servicio en el resultado de la búsqueda.

    ![Permiso de API de AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria.png)

    - Seleccione **permisos delegados**. Active la casilla situada junto al ámbito de la API. Haga clic en **Agregar permisos**.

    ![Permiso de API de AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria-Add.png)

    - Repita este procedimiento para agregar permisos al servicio de impresión en la nube de empresa.

    ![Permiso de API de AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-ECP-Add.png)

    - Una vez que se devuelva a la hoja permisos de la API, espere 10 segundos antes de hacer clic en **consentimiento de administrador general..**..

    ![Permiso 5 de la API de AAD](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent.png)

    - Haga clic en **sí** cuando se le solicite.

    ![Permiso de API de AAD 6](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent-Yes.png)

    - Compruebe que se muestra la columna Estado del permiso de la API con marcas de verificación verdes.

    ![Permiso de API de AAD 7](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Verify.png)

4. Configurar el proxy de aplicación para las aplicaciones Web
    - Vaya a **Azure Active Directory**  >  **aplicaciones empresariales**  >  **todas las aplicaciones**. Busque el servicio de detección de Mopria y haga clic en él.

    ![Proxy de aplicación de AAD 1](../media/hybrid-cloud-print/AAD-EnterpriseApp-AllApps.png)

    - Haga clic en **proxy de aplicación**. Escriba la dirección URL interna con el formato `https://<fully qualified domain name of the Print Server>/mcs/` . Haga clic en **Guardar** para finalizar.

    ![Proxy de aplicación de AAD 2](../media/hybrid-cloud-print/AAD-EnterpriseApp-Mopria-AppProxy.png)

    - Repita el servicio de impresión en la nube empresarial. Tenga en cuenta que la dirección URL interna es `https://<fully qualified domain name of the Print Server>/ecp/` .

    ![Proxy de aplicación de AAD 3](../media/hybrid-cloud-print/AAD-EnterpriseApp-ECP-AppProxy.png)

    - Seleccione **Azure Active Directory** > **Registros de aplicaciones**. Haga clic en el servicio de detección Mopria. En **información general**, tenga en cuenta que el URI del ID. de aplicación se ha cambiado del valor predeterminado a la dirección URL externa en **proxy de aplicación**. El URI se utilizará durante la instalación del servidor de impresión, en la Directiva MDM de cliente, y en la impresora de publicación.

    ![Proxy de aplicación de AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-Overview.png)

5. Asignar usuarios a aplicaciones
    - Vaya a **Azure Active Directory**  >  **aplicaciones empresariales**  >  **todas las aplicaciones**. Busque el servicio de detección de Mopria y haga clic en él.
    - Haga clic en **usuarios y grupos** y asigne usuarios, o haga clic en **propiedades** y **cambie se** requiere la **asignación de usuario** .
    - Repita el servicio de impresión en la nube empresarial.

6. Configuración del URI de redirección en la aplicación nativa
    - Seleccione **Azure Active Directory** > **Registros de aplicaciones**. Haga clic en la aplicación nativa. Vaya a **información general** y copie el identificador de la **aplicación (cliente)**.

    ![URI de redireccionamiento de AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Overview.png)

    - Vaya a **autenticación**. Cambie el cuadro desplegable **tipo** a `Public...` y escriba dos URI de redirección con el formato siguiente, donde `<NativeClientAppID>` es el paso anterior:

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/<NativeClientAppID>`

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

    ![URI de redireccionamiento de AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Authentication.png)

    - Haga clic en **Guardar** para finalizar.

### <a name="step-4---setup-the-print-server"></a>Paso 4: configurar el servidor de impresión

1. Asegúrese de que el servidor de impresión tenga todos los Windows Update disponibles. Nota: el servidor 2019 debe revisarse para compilar 17763,165 o posterior.
    - Instale los siguientes roles de servidor:
        - Rol del servidor de impresión
        - Internet Information Service (IIS)
    - Consulte [instalar roles, servicios de rol y características con el Asistente para agregar roles y características](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw) para más información sobre cómo instalar roles de servidor.

    ![Roles del servidor de impresión](../media/hybrid-cloud-print/PrintServer-Roles.png)

2. Instale los módulos de PowerShell de impresión en la nube híbrida.
    - Ejecute los siguientes comandos desde un símbolo del sistema de PowerShell con privilegios elevados:

        `find-module -Name PublishCloudPrinter`para confirmar que el equipo puede tener acceso al Galería de PowerShell (PSGallery)

        `install-module -Name PublishCloudPrinter`

    > Nota: es posible que vea un mensaje que indica que "PSGallery" es un repositorio que no es de confianza.  Escriba "y" para continuar con la instalación.

    ![Publicar impresora en la nube del servidor de impresión](../media/hybrid-cloud-print/PrintServer-PublishCloudPrinter.png)

3. Instale la solución de impresión en la nube híbrida.
    - En el mismo símbolo del sistema de PowerShell con privilegios elevados, cambie el directorio a uno de los siguientes (se necesitan comillas):

        `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`

    - Ejecutar

        `.\CloudPrintDeploy.ps1 -AzureTenant <Azure Active Directory domain name> -AzureTenantGuid <Azure Active Directory ID>`

    - Consulte la siguiente captura de pantalla para buscar el nombre de dominio de Azure Active Directory.

    ![Servidor de impresión cómo obtener el nombre de dominio de AAD](../media/hybrid-cloud-print/PrintServer-GetAADDomainName.png)

    - Consulte la siguiente captura de pantalla para buscar el identificador de Azure Active Directory.

    ![Implementación de impresión en la nube del servidor de impresión](../media/hybrid-cloud-print/PrintServer-GetAADId.png)

    - La salida del script CloudPrintDeploy tiene el siguiente aspecto:

    ![Implementación de impresión en la nube del servidor de impresión](../media/hybrid-cloud-print/PrintServer-CloudPrintDeploy.png)

    - Compruebe el archivo de registro para ver si hay algún error:`C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0\CloudPrintDeploy.log`

4. Ejecute **RegitEdit** en un símbolo del sistema con privilegios elevados. Vaya a equipo \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\EnterpriseCloudPrintService.
    - Asegúrese de que AzureAudience está establecido en el URI del ID. de aplicación de la aplicación de impresión en la nube de la empresa.
    - Asegúrese de que AzureTenant está establecido en el nombre de dominio Azure AD.

    ![Claves del registro de ECP del servidor de impresión](../media/hybrid-cloud-print/PrintServer-RegEdit-ECP.png)

5. Vaya a equipo \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\MopriaDiscoveryService.
    - Asegúrese de que AzureAudience es el URI de ID. de aplicación de la aplicación de servicio de detección Mopria.
    - Asegúrese de que AzureTenant es el nombre de dominio Azure AD.
    - Asegúrese de que la dirección URL es el URI del ID. de aplicación de la aplicación de servicio de detección Mopria.

    ![Claves del registro Mopria del servidor de impresión](../media/hybrid-cloud-print/PrintServer-RegEdit-Mopria.png)

6. Ejecute **iisreset** en un símbolo del sistema de PowerShell de elevación. Esto garantizará que cualquier cambio del registro realizado en el paso anterior surta efecto.

7. Configure los extremos de IIS para admitir SSL.
    - El certificado SSL puede ser un certificado autofirmado o uno emitido por una entidad de certificación (CA) de confianza.
    - Si usa el certificado autofirmado, asegúrese **de que el certificado se importe en las máquinas cliente**.
    - Si registra su dominio con un proveedor de terceros, tendrá que configurar los extremos de IIS con el certificado SSL. Consulte esta [Guía](https://www.sslsupportdesk.com/microsoft-server-2016-iis-10-10-5-ssl-installation/) para obtener información detallada.

8. Instale el paquete de SQLite.
   - Abra un símbolo del sistema de PowerShell con privilegios elevados.
   - Ejecute el siguiente comando para descargar los paquetes Nuget System. Data. SQLite.

        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`

   - Ejecute el siguiente comando para instalar los paquetes.

        `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Nota: se recomienda descargar e instalar la versión más reciente si se omite la opción-requiredversion.

    ![Claves del registro Mopria del servidor de impresión](../media/hybrid-cloud-print/PrintServer-InstallSQLite.png)

9. Copie los archivos dll de SQLite en la carpeta MopriaCloudService webapp bin (C:\inetpub\wwwroot\MopriaCloudService\bin).
    - Cree un archivo. PS1 que contenga el siguiente script de PowerShell.
    - Cambie la variable $version a la versión de SQLite instalada en el paso anterior.
    - Ejecute el archivo. PS1 en un símbolo del sistema de PowerShell con privilegios elevados.

    ```powershell
    $source = \Program Files\PackageManagement\NuGet\Packages
    $core = System.Data.SQLite.Core
    $linq = System.Data.SQLite.Linq
    $ef6 = System.Data.SQLite.EF6
    $version = x.x.x.x
    $target = C:\inetpub\wwwroot\MopriaCloudService\bin

    xcopy /y $source\$core.$version\lib\net46\System.Data.SQLite.dll $target\
    xcopy /y $source\$core.$version\build\net46\x86\SQLite.Interop.dll $target\x86\
    xcopy /y $source\$core.$version\build\net46\x64\SQLite.Interop.dll $target\x64\
    xcopy /y $source\$linq.$version\lib\net46\System.Data.SQLite.Linq.dll $target\
    xcopy /y $source\$ef6.$version\lib\net46\System.Data.SQLite.EF6.dll $target\
    ```

10. Actualice el archivo de c:\inetpub\wwwroot\MopriaCloudService\web.config para incluir la versión x. x. x de SQLite en las `<runtime>/<assemblyBinding>` secciones siguientes. Se trata de la misma versión utilizada en el paso anterior.

    ```xml
    ...
    <dependentAssembly>
    assemblyIdentity name=System.Data.SQLite culture=neutral publicKeyToken=db937bc2d44ff139 /
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Core culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.EF6 culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Linq culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    ...
    ```

11. Cree la base de datos SQLite.
    - Descargue e instale los archivos binarios de las herramientas de SQLite desde `https://www.sqlite.org/` .
    - Vaya al `c:\inetpub\wwwroot\MopriaCloudService\Database` directorio.
    - Ejecute el siguiente comando para crear la base de datos en este directorio:

        `sqlite3.exe MopriaDeviceDb.db .read MopriaSQLiteDb.sql`

    - En el explorador de archivos, abra las propiedades del archivo MopriaDeviceDb. DB para agregar usuarios o grupos que pueden publicar en la base de datos Mopria en la pestaña seguridad. Los usuarios o grupos deben existir en el Active Directory local y sincronizarse con Azure AD.
    - Si la solución se implementa en un dominio no enrutable (por ejemplo, *midominio*. local), el dominio Azure ad (por ejemplo, *domainname*. onmicrosoft.com, o uno adquirido de otro fabricante) debe agregarse como sufijo UPN a la Active Directory local. Esto es así para que el mismo usuario exacto que va a publicar impresoras (por ejemplo, admin@*domainname*. onmicrosoft.com) se pueda agregar en la configuración de seguridad del archivo de base de datos. Consulte [preparar un dominio no enrutable para la sincronización de directorios](https://docs.microsoft.com/office365/enterprise/prepare-a-non-routable-domain-for-directory-synchronization).

    ![Claves del registro Mopria del servidor de impresión](../media/hybrid-cloud-print/PrintServer-SQLiteDB.png)

### <a name="step-5-optional---configure-pre-authentication-with-azure-ad"></a>Paso 5 \[ opcional \] : configuración de la autenticación previa con Azure ad

1. Revise el documento [delegación restringida de Kerberos para el inicio de sesión único en las aplicaciones con el proxy de aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

2. Configurar la Active Directory local.
    - En el equipo de Active Directory, abra administrador del servidor y vaya a **herramientas**  >  **Active Directory usuarios y equipos**.
    - Navegue hasta el nodo **equipos** y seleccione el servidor del conector.
    - Haga clic con el botón **Properties**secundario en la  ->  pestaña**delegación**de propiedades   .
    - Seleccione **confiar en este equipo para la delegación solo a los servicios especificados**.
    - Seleccione **usar cualquier protocolo de autenticación**.
    - En **servicios en los que esta cuenta puede presentar credenciales delegadas**.
        - Agregue el nombre de entidad de seguridad de servicio (SPN) de la máquina del servidor de impresión.
        - Seleccione HOST para tipo de servicio.
    ![Delegación Active Directory](../media/hybrid-cloud-print/AD-Delegation.png)

3. Compruebe que la autenticación de Windows está habilitada en IIS.
    - En el servidor de impresión, abra Administrador del servidor herramientas de > > administrador de Internet Information Services (IIS).
    - Navegue al sitio.
    - Haga doble clic en **autenticación**.
    - Haga clic en **autenticación de Windows** y haga clic en **Habilitar** en **acciones**.
    ![Autenticación IIS del servidor de impresión](../media/hybrid-cloud-print/PrintServer-IIS-Authentication.png)

4. Configurar el inicio de sesión único.
    - En Azure portal, vaya a **Azure Active Directory**  >  **aplicaciones empresariales**  >  **todas las aplicaciones**.
    - Seleccione la aplicación MopriaDiscoveryService.
    - Vaya a **proxy de aplicación**. Cambie el método de autenticación previa a **Azure Active Directory**.
    - Vaya a **Inicio de sesión único**. Seleccione autenticación de Windows integrada como método de inicio de sesión único.
    - Establezca el SPN de la **aplicación interno** en el SPN del equipo del servidor de impresión.
    - Establezca **identidad de inicio de sesión delegada** en nombre principal de usuario.
    - Repita el procedimiento para la aplicación EntperiseCloudPrint.
    ![Inicio de sesión único de AAD IWA](../media/hybrid-cloud-print/AAD-SingleSignOn-IWA.png)

### <a name="step-6---configure-the-required-mdm-policies"></a>Paso 6: configurar las directivas de MDM necesarias

1. Inicie sesión en el proveedor de MDM.
2. Busque el grupo de directivas de impresión en la nube de empresa y configure las directivas según las directrices siguientes:
    - CloudPrintOAuthAuthority = `https://login.microsoftonline.com/<Azure AD Directory ID>` . El ID. de directorio se puede encontrar en Azure Active Directory propiedades de >.
    - CloudPrintOAuthClientId = \( \) valor de identificador de cliente de aplicación de la aplicación nativa. Puede encontrarlo en Azure Active Directory > Registros de aplicaciones > seleccione la aplicación nativa > información general.
    - CloudPrinterDiscoveryEndPoint = dirección URL externa de la aplicación de servicio de detección Mopria. Puede encontrarlo en Azure Active Directory > aplicaciones empresariales > seleccione la aplicación de servicio de detección de Mopria > proxy de aplicación. **Debe ser exactamente el mismo, pero sin la finalización/**.
    - MopriaDiscoveryResourceId = URI del ID. de aplicación de la aplicación del servicio de detección Mopria. Puede encontrarlo en Azure Active Directory > Registros de aplicaciones > seleccione la aplicación de servicio de detección de Mopria > información general. **Debe ser exactamente el mismo con el carácter/**.
    - CloudPrintResourceId = URI de ID. de aplicación de la aplicación de impresión en la nube de la empresa. Puede encontrarlo en Azure Active Directory > Registros de aplicaciones > seleccione la aplicación de impresión en la nube de la nube de > información general. **Debe ser exactamente el mismo con el carácter/**.
    - DiscoveryMaxPrinterLimit = \<a positive integer\> .

> Nota: Si usa Microsoft Intune servicio, puede encontrar esta configuración en la categoría impresora en la nube.

|Nombre para mostrar de Intune                     |Directiva                         |
|----------------------------------------|-------------------------------|
|URL de detección de impresora                   |CloudPrinterDiscoveryEndpoint  |
|Dirección URL de la autoridad de acceso de la impresora            |CloudPrintOAuthAuthority       |
|GUID de aplicación de cliente nativo de Azure            |CloudPrintOAuthClientId        |
|URI de recurso del servicio de impresión              |CloudPrintResourceId           |
|Número máximo de impresoras para consultar (solo móvil)  |DiscoveryMaxPrinterLimit       |
|URI de recurso del servicio de detección de impresoras  |MopriaDiscoveryResourceId      |

> Nota: Si el grupo de directivas de impresión en la nube no está disponible, pero el proveedor de MDM admite la configuración de OMA-URI, puede establecer las mismas directivas.  Consulte [esto](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority) para obtener información adicional.

    - Valores de OMA-URI
        - CloudPrintOAuthAuthority =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority
            - Valor =https://login.microsoftonline.com/<Azure AD Directory ID>
        - CloudPrintOAuthClientId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId
            - Valor = <ID. de aplicación de la aplicación nativa Azure AD>
        - CloudPrinterDiscoveryEndPoint =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint
            - Valor = dirección URL externa de la aplicación de servicio de detección Mopria (debe ser exactamente la misma pero sin el final/)
        - MopriaDiscoveryResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId
            - Valor = el URI del ID. de aplicación de la aplicación de servicio de detección Mopria
        - CloudPrintResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId
            - Valor = el URI del ID. de aplicación de la aplicación de impresión en la nube de la empresa
        - DiscoveryMaxPrinterLimit =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit
            - Valor = un entero positivo

### <a name="step-7---publish-the-shared-printer"></a>Paso 7: publicación de la impresora compartida

1. Instale la impresora deseada en el servidor de impresión.
2. Compartir la impresora a través de la interfaz de usuario de propiedades de la impresora.
3. Seleccione el conjunto de usuarios que quiera conceder acceso.
4. Guarde los cambios y cierre la ventana Propiedades de la impresora.
5. Preparar una actualización de Windows 10 Fall Creator o un equipo posterior. Una el equipo a Azure AD e inicie sesión como un usuario que esté sincronizado con Active Directory local y se le haya concedido el permiso adecuado para el archivo MopriaDeviceDb. dB.
6. En el equipo con Windows 10, abra un símbolo del sistema de Windows PowerShell con privilegios elevados.
    - Ejecute los comandos siguientes:
        - `find-module -Name PublishCloudPrinter`para confirmar que el equipo puede tener acceso al Galería de PowerShell (PSGallery)
        - `install-module -Name PublishCloudPrinter`

            > Nota: es posible que vea un mensaje que indica que "PSGallery" es un repositorio que no es de confianza.  Escriba "y" para continuar con la instalación.

        - `Publish-CloudPrinter`
        - Printer = el nombre de la impresora compartida. Este nombre debe coincidir exactamente con el nombre del recurso compartido que se muestra en la pestaña **uso compartido** de las propiedades de la impresora, abierta en el servidor de impresión.

        ![Propiedades de impresora del servidor de impresión](../media/hybrid-cloud-print/PrintServer-PrinterProperties.png)

        - Manufacturer = fabricante de la impresora.
        - Modelo = modelo de impresora.
        - OrgLocation = una cadena JSON que especifica la ubicación de la impresora, por ejemplo,

            `{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:Microsoft, depth:1}, {category:site, vs:Redmond, WA, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_number, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}`

        - SDDL = Cadena SDDL que representa los permisos de la impresora.
            - Inicie sesión como administrador en el servidor de impresión y, a continuación, ejecute el siguiente comando de PowerShell en la impresora que desea publicar: `(Get-Printer PrinterName -full).PermissionSDDL` .
            - Agregue **O:BA** como prefijo al resultado del comando anterior. Por ejemplo, Si la cadena devuelta por el comando anterior es G:DUD: (A; OICI; FA;;; WD), después SDDL = O:BAG: DUD: (A; OICI; FA;;; WD).
        - DiscoveryEndpoint = inicie sesión en Azure Portal y, a continuación, obtenga la cadena de aplicaciones empresariales > aplicación de servicio de detección de Mopria > proxy de aplicación > dirección URL externa. Omita el carácter/.
        - PrintServerEndpoint = inicie sesión en Azure Portal y, a continuación, obtenga la cadena de aplicaciones empresariales > aplicación de impresión en la nube de Enterprise > proxy de aplicación > dirección URL externa. Omita el carácter/.
        - AzureClientId = ID. de aplicación de la aplicación nativa registrada.
        - AzureTenantGuid = ID. de directorio del inquilino de Azure AD.
        - DiscoveryResourceId = URI de ID. de aplicación de la aplicación de servicio de detección Mopria.

    - También puede especificar todos los valores de parámetro necesarios en la línea de comandos. La sintaxis es la siguiente:

        `Publish-CloudPrinter -Printer <string> -Manufacturer <string> -Model <string> -OrgLocation <string> -Sddl <string> -DiscoveryEndpoint <string> -PrintServerEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Comando de ejemplo:

        `Publish-CloudPrinter -Printer HcpTestPrinter -Manufacturer Manufacturer1 -Model Model1 -OrgLocation '{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:MyCompany, depth:1}, {category:site, vs:MyCity, State, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_name, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}' -Sddl O:BAG:DUD:(A;OICI;FA;;;WD) -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -PrintServerEndpoint https://enterprisecloudprint-contoso.msappproxy.net/ecp -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

    - Use el siguiente comando para comprobar que la impresora está publicada.

        `Publish-CloudPrinter -Query -DiscoveryEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Comando de ejemplo:

        `Publish-CloudPrinter -Query -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

## <a name="verify-the-deployment"></a>Comprobar la implementación

En un Azure AD dispositivo Unido que tiene configuradas las directivas MDM:
- Abra un explorador Web y vaya a https://mopriadiscoveryservice- *tenant-name*. msappproxy.net/MCS/Services.
- Debería ver el texto JSON que describe el conjunto de funcionalidad de este punto de conexión.
- Vaya a **configuración**  >  **dispositivos**  >  **impresoras & escáneres**.
    - Haga clic en **Agregar impresora o escáner**.
    - Debería ver una búsqueda de impresoras en la nube (o buscar impresoras en mi organización en un vínculo más reciente de una máquina con Windows 10).
    - Haga clic en el vínculo.
    - Haga clic en el vínculo Seleccione una ubicación de búsqueda.
        - Debería ver la jerarquía de la ubicación del dispositivo.
    - Seleccione una ubicación y haga clic en **Aceptar** y, a continuación, haga clic en el botón **Buscar** para buscar las impresoras.
    - Seleccione impresora y haga clic en el botón **Agregar dispositivo** .
    - Después de la instalación correcta de la impresora, imprima en la impresora desde la aplicación favorita.

> Nota: Si usa la impresora EcpPrintTest, puede encontrar el archivo de salida en la máquina del servidor de impresión en la ubicación C: \\ ECPTestOutput \\ EcpTestPrint. XPS.

## <a name="troubleshooting"></a>Solución de problemas

A continuación se muestran problemas comunes durante la implementación de HCP

|Error |Pasos recomendados |
|------|------|
|Error de script de PowerShell de CloudPrintDeploy | <ul><li>Asegúrese de que Windows Server tiene la actualización más reciente.</li><li>Si se usa Windows Server Update Services (WSUS), consulte [Cómo hacer que las características a petición y los paquetes de idioma estén disponibles cuando se usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).</li></ul> |
|Error en la instalación de SQLite con el mensaje: se detectó un bucle de dependencia para el paquete ' System. Data. SQLite ' | Install-Package System. Data. SQLite. Core-ProviderName Nuget-SkipDependencies<br>Install-Package System. Data. SQLite. EF6-ProviderName Nuget-SkipDependencies<br>Install-Package System. Data. SQLite. Linq-ProviderName Nuget-SkipDependencies<br><br>Una vez que los paquetes se hayan descargado correctamente, asegúrese de que tienen la misma versión. Si no es así, agregue el parámetro-requiredversion a los comandos anteriores y establézcalo para que tenga la misma versión. |
|No se pudo publicar la impresora | <ul><li>Para la autenticación previa de paso a través, asegúrese de que el usuario que publica la impresora tiene los permisos adecuados para la base de datos de publicación.</li><li>Para Azure AD autenticación previa, asegúrese de que la autenticación de Windows está habilitada en IIS. Consulte el paso 5,3. Además, pruebe primero la autenticación de paso a través. Si funciona la autenticación previa de paso a través, es probable que el problema esté relacionado con el proxy de aplicación. Consulte [solución de problemas de proxy de aplicación y mensajes de error](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot). Tenga en cuenta que al cambiar a passthrough se restablece la configuración de inicio de sesión único; Vuelva a consultar el paso 5 para configurar Azure AD autenticación previa de nuevo.</li></ul> |
|Los trabajos de impresión permanecen en el estado de la impresora | <ul><li>Asegúrese de que TLS 1,2 está habilitado en el servidor del conector. Consulte el artículo vinculado en el paso 2,1.</li><li>Asegúrese de que HTTP2 esté deshabilitado en el servidor del conector. Consulte el artículo vinculado en el paso 2,1.</li></ul> |

A continuación se muestran las ubicaciones de los registros que pueden ayudarle a solucionar problemas

|Componente |Ubicación del registro |
|------|------|
|Cliente de Windows 10 | <ul><li>Use Visor de eventos para ver el registro de las operaciones de Azure AD. Haga clic en **Inicio** y escriba visor de eventos. Vaya a registros de aplicaciones y servicios > operación > de > Microsoft Windows > AAD.</li><li>Usar el centro de comentarios para recopilar registros. Consulte [envío de comentarios a Microsoft con la aplicación de centro de comentarios](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)</li></ul> |
|Servidor de conector | Use Visor de eventos para ver el registro del proxy de aplicación. Haga clic en **Inicio** y escriba visor de eventos. Vaya a registros de aplicaciones y servicios > Microsoft > AadApplicationProxy > conector > admin. |
|Servidor de impresión | Los registros de la aplicación de servicio de detección de Mopria y la aplicación de impresión de nube de empresa se pueden encontrar en C:\inetpub\logs\LogFiles\W3SVC1. |
