---
title: Implementar la impresión en la nube híbrida de Windows Server
description: Cómo configurar la impresión en la nube híbrida de Microsoft
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: Windows Server 2016
ms.tgt_pltfrm: na
ms.topic: ''
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: af5cd5f83633df7e704f4b768baf8dc6d78546aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370444"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>Implementar la impresión en la nube híbrida de Windows Server con la autenticación previa

>Se aplica a: Windows Server 2016

En este tema, para los administradores de ti, se describe la implementación de un extremo a otro de la solución de impresión en la nube híbrida de Microsoft. Esta solución se encuentra en la parte superior de los servidores de Windows existentes que se ejecutan como servidor de impresión y Azure Active Directory permite que los dispositivos Unidos y administrados por MDM detecten e impriman en las impresoras administradas de la organización.

## <a name="pre-requisites"></a>Requisitos previos

Hay una serie de suscripciones, servicios y equipos que debe adquirir antes de iniciar esta instalación. Son las siguientes:

-   Suscripción a Azure AD Premium
    
    Consulte Introducción a [una suscripción de Azure](https://azure.microsoft.com/trial/get-started-active-directory/)para obtener una suscripción de prueba a Azure. 

-   Servicio MDM, como Intune
    
    Consulte [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)para obtener una suscripción de prueba a Intune.

-   Windows Server que se ejecuta como Active Directory

    Consulte [paso a paso: configuración de Active Directory en Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/), para obtener ayuda para la configuración de Active Directory.

-   Windows Server 2016 unido a un dominio que se ejecuta como servidor de impresión
    
    Consulte [instalar roles, servicios de rol y características con el Asistente para agregar roles y características](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw)para obtener información sobre cómo instalar roles y servicios de rol en Windows Server.

-   Azure AD Connect
    
    Consulte [instalación personalizada de Azure ad Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom), para obtener ayuda para la configuración de Azure ad Connect.

-   Aplicación de Azure conector del proxy en un equipo con Windows Server unido a un dominio independiente
    
    Consulte Introducción al [proxy de aplicación e instalación del conector](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)para obtener ayuda para configurar aplicación de Azure conector del proxy.

-   Nombre de dominio público
    
    Puede usar el nombre de dominio que ha creado Azure o adquirir su propio nombre de dominio.

## <a name="deployment-steps"></a>Pasos de implementación

En esta guía se describen cinco (5) pasos de instalación:

- Paso 1: instalar Azure AD Connect para sincronizar entre Azure AD y AD local
- Paso 2: instalar el paquete de impresión en la nube híbrida en el servidor de impresión
- Paso 3: instalación del proxy de Aplicación de Azure (AAP) con la delegación restringida de Kerberos (KCD)
- Paso 4: configurar las directivas de MDM necesarias
- Paso 5: publicar impresoras compartidas

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Paso 1: instalar Azure AD Connect para sincronizar entre Azure AD y AD local
1. En el equipo de Windows Server Active Directory, descargue el software de Azure AD Connect
2. Inicie el paquete de instalación de Azure AD Connect y seleccione **Usar configuración rápida** .
3. Escriba la información solicitada en el proceso de instalación y haga clic en **instalar** .

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Paso 2: instalar el paquete de impresión en la nube híbrida en el servidor de impresión

1. Instalación de los módulos de PowerShell de impresión en la nube híbrida
   - Ejecute los siguientes comandos desde un símbolo del sistema de PowerShell con privilegios elevados
      - `find-module -Name "PublishCloudPrinter"` para confirmar que el equipo puede tener acceso al Galería de PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > Nota: es posible que vea un mensaje que indica que "PSGallery" es un repositorio que no es de confianza.  Escriba "y" para continuar con la instalación.

2. Instalar la solución de impresión en la nube híbrida
    - En el mismo símbolo del sistema de PowerShell con privilegios elevados, cambie el directorio a `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Ejecución <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurar los dos extremos de IIS para admitir SSL
   -   El certificado SSL puede ser un certificado autofirmado o uno emitido por una entidad de certificación de confianza (CA).
   -  Si usa un certificado autofirmado, asegúrese de que el certificado se importa en las máquinas cliente.
4. Instalar el paquete de SQLite
   - Abra un símbolo del sistema de PowerShell con privilegios elevados
   - Ejecute el siguiente comando para descargar los paquetes Nuget de System. Data. SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Ejecute el siguiente comando para instalar los paquetes<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Nota: se recomienda descargar e instalar la versión más reciente si se omite la opción "-requiredversion".

5. Copie los archivos dll de SQLite en la carpeta MopriaCloudService webapp \<bin\> (**C:\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
   - Los archivos binarios de SQLite deben estar en "\\archivos de programa\\PackageManagement\\NuGet\\paquetes"

           \\System.Data.SQLite.**Core**.x.x.x.x\\lib\\net46\\System.Data.SQLite.dll
           --\> \<bin\>\\System.Data.SQLite.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x86\\SQLite.Interop.dll
           --\> \<bin\>\\**x86**\\SQLite.Interop.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x64\\SQLite.Interop.dll
           --\> \<bin\>\\**x64**\\SQLite.Interop.dll
           \\System.Data.SQLite.**Linq**.x.x.x.x\\lib\\net46\\System.Data.SQLite.Linq.dll
           --\> \<bin\>\\System.Data.SQLite.Linq.dll  
           \\System.Data.SQLite.**EF6**.x.x.x.x\\lib\\net46\\System.Data.SQLite.EF6.dll
           --\> \<bin\>\\System.Data.SQLite.EF6.dll

   > Nota: x. x. x es la versión de SQLite instalada anteriormente.

6. Actualice el archivo de `c:\inetpub\wwwroot\MopriaCloudService\web.config` para incluir la versión de SQLite x. x. x. x en el siguiente \<en tiempo de ejecución\>/\<assemblyBinding\> secciones:

       <dependentAssembly>
       assemblyIdentity name="System.Data.SQLite" culture="neutral" publicKeyToken="db937bc2d44ff139" /
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Core" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.EF6" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Linq" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>

7. Crear la base de datos SQLite:
    -  Descargue e instale los archivos binarios de las herramientas de SQLite desde <https://www.sqlite.org/>
    -  Vaya a **c:\\inetpub\\wwwroot\\directorio de base de datos de MopriaCloudService\\**
    -  Ejecute el siguiente comando para crear la base de datos en este directorio: `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  En el explorador de archivos, abra las propiedades del archivo MopriaDeviceDb. DB para agregar usuarios o grupos que pueden publicar en la base de datos Mopria en la pestaña seguridad.
        - Solo se recomienda agregar el grupo de usuarios de Administrador requerido.
8. Registro de la aplicación Web 2 con Azure AD para admitir la autenticación OAuth2
   - Inicie sesión como administrador global en el Azure AD portal de administración de inquilinos.
     1. Vaya a la pestaña "Registros de aplicaciones" para agregar un extremo de impresión
        - Agregar aplicación, seleccione "nuevo registro de aplicación"
        - Proporcione un nombre adecuado y seleccione "aplicación web/API".
        - URL de inicio de sesión = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Repetir para el punto de conexión de detección
        - URL de inicio de sesión = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Repetir para la aplicación cliente nativa
        -   Al proporcionar el nombre de la aplicación, asegúrese de seleccionar "aplicación cliente nativa" como "tipo de aplicación".
        -   URI de redirección = "https://\<Services-Machine-Endpoint\>/RedirectUrl"
     4. Vaya a la aplicación cliente nativa "configuración".
        -   Copie el valor de "Application ID" que se usará para pasos de configuración posteriores.
        -   Seleccione "permisos necesarios".
            1.  Haga clic en "agregar".
            2.  Haga clic en "seleccionar una API".
            3.  Busque el punto de conexión de impresión y el punto de conexión de detección por el nombre que definió al crear el punto de conexión de la aplicación.
            4.  Agregar los dos puntos de conexión
            5.  Asegúrese de que la opción "permisos delegados" de cada punto de conexión de la aplicación está habilitada
            6.  Asegúrese de hacer clic en el botón "listo" situado en la parte inferior.
            7.  Haga clic en "conceder permisos" cuando se hayan agregado ambos extremos.  Seleccione "sí" cuando se le pida que apruebe la solicitud.
        -   Vaya a "URI de redireccionamiento" y agregue los siguientes URI de redireccionamiento a la lista: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>Paso 3: instalación del proxy de Aplicación de Azure (AAP) con la delegación restringida de Kerberos (KCD)
1. Inicie sesión en el portal de administración de inquilinos de Azure AD (AAD)
    - En la lista de menús de AAD, seleccione "proxy de aplicación".
    - Haga clic en "habilitar proxy de aplicación" en la parte superior de la pantalla.
    - Descargue el "conector del proxy de aplicación" en un equipo de Windows Server unido a un dominio que actuará como el proxy de aplicación web (WAP).
2. En el equipo WAP, inicie sesión como administrador e instale el "conector del proxy de aplicación".
    - Durante la instalación, proporcione al conector del proxy de aplicación las credenciales para el principio de Azure en las que quiere habilitar AAP
    - Asegúrese de que el equipo WAP está unido a un dominio en el Active Directory local
3. En el equipo Active Directory, vaya a **herramientas-> usuarios y equipos** .
    - Seleccione el equipo que ejecuta el conector del proxy de aplicación
    - Haga clic con el botón derecho y seleccione **Propiedades-> pestaña delegación**
    - Seleccione **confiar en este equipo para la delegación solo a los servicios especificados.**
    - Seleccione **usar cualquier protocolo de autenticación.**
    - En **servicios a los que esta cuenta puede presentar credenciales delegadas**
        - Agregue el valor de la identidad de SPN del equipo que ejecuta los servicios (MopriaDiscoveryService y MicrosoftEnterpriseCloudPrint Service).
            - En el caso de SPN, escriba el SPN de la propia máquina, es decir, "HOST/\<MachineName\>.\<\>de dominio "<br>
                `HOST/appServer.Contoso.com`
4. Vuelva al portal de administración de inquilinos de AAD y agregue los proxies de aplicación.
   - Vaya a la pestaña **aplicaciones empresariales** .
   - Haga clic en **nueva aplicación** .
   - Seleccione la **aplicación local** y rellene los campos.
       - Nombre: cualquier nombre que desee.
       - Dirección URL interna: esta es la dirección URL interna del servicio en la nube de detección de Mopria al que puede tener acceso la máquina WAP.
       - Dirección URL externa: configure según corresponda para su organización
       - Método de autenticación previa: Azure Active Directory

     >   Nota: Si no encuentra la configuración anterior, agregue el proxy con la configuración disponible y, a continuación, seleccione el proxy de aplicación que acaba de crear, vaya a la pestaña **proxy de aplicación** y agregue toda la información anterior.

   - Una vez creado, vuelva a **aplicaciones empresariales** -> **todas las aplicaciones**, seleccione la nueva aplicación que acaba de crear.
   - Vaya al **Inicio de sesión único**, asegúrese de que el "modo de inicio de sesión único" esté establecido en "autenticación de Windows integrada"
   - Establezca el "SPN de la aplicación interna" en el SPN especificado en el paso 3,3, más arriba.
   - Asegúrese de que la "identidad de inicio de sesión delegada" esté establecida en "nombre principal de usuario".

5. Repita 4 para el servicio de impresión en la nube empresarial y proporcione la dirección URL interna para el servicio de impresión en la nube de su empresa.
6. Vuelva al portal de administración de inquilinos de Azure AD, vaya a **registros de aplicaciones** y seleccione la aplicación cliente nativa > "configuración".
    - Selección de **los permisos necesarios**
        - Agregue las dos nuevas aplicaciones de proxy que acaba de crear.
        - Conceder permisos delegados para estas dos aplicaciones
        - Una vez agregadas las dos aplicaciones de proxy, haga clic en "conceder permisos".  Seleccione "sí" cuando se le pida que apruebe la solicitud.

7. Habilitar la autenticación de Windows en IIS para el servicio en la nube Mopria y las máquinas del servicio de impresión en la nube de empresa
    - Asegúrese de que está instalada la característica de autenticación de Windows:
        1. Abrir el Administrador del servidor
        2. Haga clic en **administrar**
        3. Haga clic en **Agregar roles y características**
        4. Seleccionar **instalación basada en características o en roles**
        5. Seleccionar el servidor
        6. En servidor Web (IIS)-> servidor Web-> Seguridad, seleccione **autenticación de Windows** .
        7. Haga clic en siguiente hasta finalizar la instalación
    - Habilitar la autenticación de Windows en IIS:
        1. Abrir el administrador de Internet Information Services (IIS)
        2. Para cada servicio o sitio:
            1.  Seleccionar el servicio o sitio
            2.  Doble clic en **autenticación**
            3.  Haga clic en **autenticación de Windows** y haga clic en **Habilitar** en **acciones** .

### <a name="step-4---configure-the-required-mdm-policies"></a>Paso 4: configurar las directivas de MDM necesarias
- Inicio de sesión en el proveedor de MDM
- Busque el grupo de directivas de impresión en la nube de empresa y configure las directivas según las directrices siguientes:
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure ID. de directorio de AD\>
  - CloudPrintOAuthClientId = valor "Application ID" de la aplicación web nativa que registró en Azure AD portal de administración
  - CloudPrinterDiscoveryEndPoint = dirección URL externa del servicio de detección de Mopria Aplicación de Azure proxy creado en el paso 3,3 (debe ser exactamente el mismo pero sin el final/)
  - MopriaDiscoveryResourceId = dirección URL externa del servicio de detección de Mopria Aplicación de Azure proxy creado en el paso 3,4 (debe ser exactamente el mismo, incluida la finalización/)
  - CloudPrintResourceId = dirección URL externa del proxy de Aplicación de Azure del servicio de impresión en la nube empresarial creado en el paso 3,5 (debe ser exactamente el mismo incluyendo el carácter final)
  - DiscoveryMaxPrinterLimit = \<un entero positivo\>

>   Nota: Si usa Microsoft Intune servicio, puede encontrar esta configuración en la categoría "impresora en la nube".

|Nombre para mostrar de Intune                     |Directiva                         |
|----------------------------------------|-------------------------------|
|URL de detección de impresora                   |CloudPrinterDiscoveryEndpoint  |
|Dirección URL de la autoridad de acceso de la impresora            |CloudPrintOAuthAuthority       |
|GUID de aplicación de cliente nativo de Azure            |CloudPrintOAuthClientId        |
|URI de recurso del servicio de impresión              |CloudPrintResourceId           |
|Número máximo de impresoras para consultar (solo móvil)  |DiscoveryMaxPrinterLimit       |
|URI de recurso del servicio de detección de impresoras  |MopriaDiscoveryResourceId      |

>   Nota: Si el grupo de directivas de impresión en la nube no está disponible, pero el proveedor de MDM admite la configuración de OMA-URI, puede establecer las mismas directivas.  Consulte este <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">artículo</a> para obtener información adicional.

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valor = `https://login.microsoftonline.com/`\<ID. de directorio Azure AD\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valor = \<ID. de aplicación de la aplicación nativa Azure AD\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valor = dirección URL externa del servicio de detección de Mopria Aplicación de Azure proxy creado en el paso 3,3 (debe ser exactamente el mismo pero sin el final/)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valor = dirección URL externa del servicio de detección de Mopria Aplicación de Azure proxy creado en el paso 3,4 (debe ser exactamente el mismo, incluida la finalización/)
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valor = dirección URL externa del proxy de Aplicación de Azure del servicio de impresión en la nube empresarial creado en el paso 3,5 (debe ser exactamente el mismo, incluido el carácter final)
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valor = \<un entero positivo\>

### <a name="step-5---publish-desired-shared-printers"></a>Paso 5: publicar las impresoras compartidas deseadas
1. Instalar la impresora deseada en el servidor de impresión
2. Compartir la impresora a través de la interfaz de usuario de propiedades de la impresora
3. Seleccione el conjunto de usuarios que quiera conceder acceso
4. Guardar los cambios y cerrar la ventana Propiedades de la impresora
5. Desde un equipo de actualización de Windows 10 Fall Creator, abra un símbolo del sistema de Windows PowerShell con privilegios elevados.
   1. Ejecuta los siguientes comandos:
      - `find-module -Name "PublishCloudPrinter"` para confirmar que el equipo puede tener acceso al Galería de PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   Nota: es posible que vea un mensaje que indica que "PSGallery" es un repositorio que no es de confianza.  Escriba "y" para continuar con la instalación.

      - Publish-CloudPrinter
        - Printer = el nombre de la impresora compartida que se definió
        - Fabricante = fabricante de la impresora
        - Modelo = modelo de impresora
        - OrgLocation = una cadena JSON que especifica la ubicación de la impresora, por ejemplo: `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = Cadena SDDL que representa los permisos de la impresora. Para ello, modifique la ficha Seguridad de las propiedades de la impresora de forma adecuada y, a continuación, ejecute el siguiente comando en un símbolo del sistema: `(Get-Printer PrinterName -full).PermissionSDDL`
            por ejemplo, "G:DUD: (A; OICI; FA;;; WD) "

          > Nota: tendrá que agregar **`O:BA`** como prefijo al resultado desde el comando anterior del símbolo del sistema antes de establecer el valor como la configuración de SDDL.  Ejemplo: SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = dirección URL externa del servicio de detección de Mopria Aplicación de Azure proxy creado en el paso 3,4
        - PrintServerEndpoint = dirección URL externa del servicio de impresión en la nube empresarial Aplicación de Azure proxy creado en el paso 3,5
        - AzureClientId = ID. de aplicación del valor de aplicación Web nativo registrado desde arriba
        - AzureTenantGuid = ID. de directorio del inquilino de Azure AD
        - DiscoveryResourceId = [opcional] ID. de aplicación del servicio en la nube de detección de Mopria de proxy

        > Nota: también puede escribir todos los valores de parámetros necesarios en la línea de comandos.<br>
        **Publish-CloudPrinter** Sintaxis de comandos de PowerShell: <br>
        Publish-CloudPrinter-Printer \<cadena\>-manufacturer \<cadena\>-Model \<cadena\>-OrgLocation \<cadena\>-SDDL \<cadena\>-DiscoveryEndpoint \<cadena\>-PrintServerEndpoint \<cadena\>-AzureClientId \<cadena\>-AzureTenantGuid \<cadena\> [-DiscoveryResourceId \<cadena\>] <br>
        Comando de ejemplo: `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>Comprobación de la implementación
En un Azure AD dispositivo Unido que tiene configuradas las directivas MDM:
- Abra un explorador Web y vaya a https://&lt;Services-Machine-Endpoint&gt;/MCS/Services (la dirección URL externa para el punto de conexión de detección)
- Debería ver el texto JSON que describe el conjunto de funcionalidad de este punto de conexión
- Vaya a "configuración del sistema operativo: dispositivos de\>-\> impresoras & escáneres"
    - Debería ver el vínculo "buscar impresoras en la nube"
    - Haga clic en el vínculo
    - Haga clic en el vínculo "Seleccione una ubicación de búsqueda".
        - Debería ver la jerarquía de ubicación del dispositivo
    - Elija una ubicación y haga clic en **Aceptar** y, a continuación, haga clic en el botón **Buscar** para buscar las impresoras.
    - Seleccione impresora y haga clic en el botón **Agregar dispositivo**
    - Después de la instalación correcta de la impresora, imprima en la impresora desde la aplicación favorita.

>   Nota: Si usa la impresora "EcpPrintTest", puede encontrar el archivo de salida en la máquina del servidor de impresión en la ubicación "C:\\ECPTestOutput\\EcpTestPrint. XPS".
