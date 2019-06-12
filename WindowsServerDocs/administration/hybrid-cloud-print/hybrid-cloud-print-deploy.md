---
title: Implementar la impresión de Windows Server híbrida en la nube
description: Cómo configurar la impresión de nube híbrida de Microsoft
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6e7bb2138afa159f945125d3fc20e4fa365340d5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435731"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>Implementar la impresión en la nube híbrida de Windows Server con la autenticación previa

>Se aplica a: Windows Server 2016

En este tema, los administradores de TI, se describe la implementación to-end de la solución de impresión de nube híbrida de Microsoft. Capas de esta solución encima de servidores de Windows existentes que se ejecuta como servidor de impresión y permite Unidos a Azure Active Directory y administrados por MDM, los dispositivos para detectar e imprimir en la organización administrar impresoras.

## <a name="pre-requisites"></a>Requisitos previos

Hay un número de equipos, deberá adquirir antes de iniciar esta instalación, servicios y suscripciones. Son las siguientes:

-   Suscripción a Azure AD premium
    
    Consulte [empezar a trabajar con una suscripción de Azure](https://azure.microsoft.com/trial/get-started-active-directory/), para una suscripción de prueba a Azure. 

-   Servicio MDM, como Intune
    
    Consulte [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune), para una suscripción de prueba a Intune.

-   Windows Server que se ejecuta como Active Directory

    Consulte [paso a paso: Configuración de Active Directory en Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/), para obtener ayuda de configuración de Active Directory.

-   Que se ejecuta como servidor de impresión de Windows Server 2016 Unidos a un dominio
    
    Consulte [instalar roles, servicios de rol y características mediante el agregar Roles y características Asistente](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw), para obtener información sobre cómo instalar roles y servicios de rol en Windows Server.

-   Azure AD Connect
    
    Consulte [instalación personalizada de Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom), para obtener ayuda sobre cómo configurar Azure AD Connect.

-   Conector de Proxy de aplicación de Azure en un dominio independiente Unidos a un equipo de Windows Server
    
    Consulte [empezar a trabajar con el Proxy de aplicación e instalar el conector](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable), para obtener ayuda al configurar el conector del Proxy de aplicación de Azure.

-   Nombre de dominio orientados al público
    
    Puede usar el nombre de dominio creado automáticamente por Azure, o adquirir su propio nombre de dominio.

## <a name="deployment-steps"></a>Pasos de implementación

Esta guía describe los pasos de instalación de cinco (5):

- Paso 1: Instalar Azure AD Connect para sincronizar entre Azure AD y AD local
- Paso 2: Instale el paquete de impresión de nube híbrida en el servidor de impresión
- Paso 3: Instalar Azure Application Proxy (AAP) con Kerberos delegación limitada (KCD)
- Paso 4: Configurar las directivas MDM necesarias
- Paso 5: Publicar las impresoras compartidas

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Paso 1: instalación de Azure AD Connect para sincronizar entre Azure AD y AD local
1. En el equipo de Windows Server Active Directory, descargue el software de Azure AD Connect
2. Inicie el paquete de instalación de Azure AD Connect y seleccione **usar configuración rápida**
3. Escriba la información solicitada en el proceso de instalación y haga clic en **instalar**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Paso 2: paquete de instalación híbrida en la nube impresión en el servidor de impresión

1. Instalar los módulos de PowerShell de impresión en la nube híbrida
   - Ejecute los siguientes comandos desde un símbolo de PowerShell con privilegios elevados
      - `find-module -Name "PublishCloudPrinter"` para confirmar que la máquina puede llegar a la Galería de PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > Nota: Es posible que vea una mensajería que indica que 'PSGallery' es un repositorio no confiable.  Escriba 'y' para continuar con la instalación.

2. Instalar la solución de impresión de la nube híbrida
    - En el mismo símbolo con privilegios elevados PowerShell, cambie el directorio a `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Ejecución <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurar los puntos de 2 conexión de IIS para admitir SSL
   -   El certificado SSL puede ser un certificado autofirmado o uno emitido desde algunos confianza entidad de certificación (CA)
   -  Si usa un certificado autofirmado, asegúrese de que el certificado se importa a los equipos cliente
4. Instalar paquete SQLite
   - Abra un símbolo de PowerShell con privilegios elevados
   - Ejecute el siguiente comando para descargar los paquetes de nuget System.Data.SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Ejecute el siguiente comando para instalar los paquetes<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Nota: Se recomienda descargar e instalar la versión más reciente si se omite el "-requiredversion" opción.

5. Copie los archivos DLL de SQLite en la MopriaCloudService Webapp \<bin\> carpeta (**C:\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
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

   > Nota: x.x.x.x es instalada por encima de la versión de SQLite.

6. Actualización de la `c:\inetpub\wwwroot\MopriaCloudService\web.config` que incluya la versión de SQLite x.x.x.x en el siguiente archivo \<en tiempo de ejecución\>/\<assemblyBinding\> secciones:

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

7. Crear la base de datos de SQLite:
    -  Descargue e instale los archivos binarios de las herramientas de SQLite desde <https://www.sqlite.org/>
    -  Vaya a **c:\\inetpub\\wwwroot\\MopriaCloudService\\base de datos** directorio
    -  Ejecute el siguiente comando para crear la base de datos en este directorio:  `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  Desde el Explorador de archivos, abra las propiedades del archivo MopriaDeviceDb.db para agregar usuarios o grupos que se puede publicar en la base de datos de Mopria en la ficha seguridad
        - Recomienda agregar solo el grupo de usuarios de administración necesario.
8. Registrar la aplicación 2 web con Azure AD para admitir la autenticación de OAuth2
   - Inicio de sesión como administrador Global para el portal de administración de inquilinos de Azure AD
     1. Vaya la ficha "Registros de aplicación" para agregar el punto de conexión de impresión
        - Agregar aplicación, seleccione "Nuevo registro de aplicaciones"
        - Proporcione un nombre adecuado y seleccione "aplicación Web / API"
        - Dirección URL de inicio de sesión = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Repita para el extremo de detección
        - Dirección URL de inicio de sesión = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Repita para la aplicación cliente nativa
        -   Al proporcionar el nombre de la aplicación, asegúrese de que seleccionar "Aplicación de cliente nativo" como el "tipo de aplicación"
        -   Redirect URI = "https://\<services-machine-endpoint\>/RedirectUrl"
     4. Vaya a la aplicación cliente nativa "Configuración"
        -   Copie el valor "Id. de aplicación" que se usará en pasos posteriores de instalación
        -   Seleccione "Permisos necesarios"
            1.  Haga clic en "Agregar"
            2.  Haga clic en "Seleccionar una API"
            3.  Busque el punto de conexión de impresión y el extremo de detección con el nombre definido al crear el punto de conexión de la aplicación
            4.  Agregue los puntos de 2 conexión
            5.  Asegúrese de que está habilitada la opción "Permisos delegados" para cada extremo de aplicación
            6.  Asegúrese de que haga clic en el botón "Listo" en la parte inferior
            7.  Haga clic en "Conceder permisos", una vez que se han agregado ambos puntos de conexión.  Seleccione "Sí" cuando se le solicita que apruebe la solicitud.
        -   Vaya a "URI de REDIRECCIÓN" y agregue el siguiente URI de redireccionamiento a la lista: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>Paso 3: instalar el Proxy de aplicación de Azure (AAP) con Kerberos (KCD) de delegación restringida
1. Inicie sesión en el portal de administración de inquilinos de Azure AD (AAD)
    - En la lista del menú AAD, seleccione a "Proxy de aplicación"
    - Haga clic en "Habilitar proxy de aplicación" en la parte superior de la pantalla
    - Descargue el "conector del Proxy de aplicación" en una máquina de Windows Server unido al dominio que actuará como el Proxy de aplicación Web (WAP).
2. En la máquina WAP, inicie sesión como administrador y el "conector del Proxy de aplicación" de la instalación
    - Durante la instalación, asigne el conector del proxy de aplicación las credenciales para el principio básico de Azure que desea habilitar AAP en
    - Asegúrese de que la máquina WAP está unido a Active Directory local
3. En el equipo de Active Directory, vaya a **Herramientas -> usuarios y equipos**
    - Seleccione la máquina que ejecuta el conector del Proxy de aplicación
    - Haga clic en y seleccione **Propiedades -> delegación** ficha
    - Seleccione **confiar en este equipo para la delegación solo a servicios especificados.**
    - Seleccione **usar cualquier protocolo de autenticación.**
    - Bajo **servicios a la que esta cuenta puede presentar credenciales delegadas**
        - Agregue el valor de la identidad SPN del equipo que ejecuta los servicios (servicio MopriaDiscoveryService y MicrosoftEnterpriseCloudPrint)
            - Para el SPN, escriba el SPN de la propia máquina, es decir "HOST /\<MachineName\>.\< Dominio\>"<br>
                `HOST/appServer.Contoso.com`
4. Vuelva al portal de administración de inquilinos AAD y agregue a los servidores proxy de aplicación
   - Vaya a la **aplicaciones empresariales** ficha
   - Haga clic en **nueva aplicación**
   - Seleccione **aplicación local** y rellene los campos
       - Nombre: Nombre que desee
       - Dirección URL interna: Se trata de la dirección URL interna para el servicio de nube de detección de Mopria que puede tener acceso la máquina WAP
       - Dirección URL externa: Configure según corresponda para su organización
       - Método de autenticación previa: Azure Active Directory

     >   Nota: Si no encuentra la configuración anterior, agregue el proxy con las opciones disponibles y, a continuación, seleccione el proxy de aplicación que acaba de crear y vaya a la **proxy de aplicación** pestaña y agregar toda la información anterior.

   - Una vez creado, vuelva a **aplicaciones empresariales** -> **todas las aplicaciones**, seleccione la nueva aplicación que acaba de crear
   - Vaya a **inicio de sesión único**, asegúrese de que el "inicio de sesión en modo único" se establece en "Autenticación integrada de Windows"
   - Establece el "interno SPN de la aplicación" en el SPN especificado anteriormente en el paso 3.3,
   - Asegúrese de que la "identidad de inicio de sesión delegada" se establece en "Nombre principal de usuario"

5. Repetir 4, anteriormente, para el servicio de impresión en la nube de empresa y proporcionar la dirección URL interna a su servicio de impresión en la nube empresarial
6. Vuelva al portal de administración de inquilinos de Azure AD y vaya a **registros de aplicaciones** y seleccione la aplicación cliente nativa -> "Configuración"
    - Seleccione **permisos necesarios**
        - Agregar el 2 nuevas aplicaciones de proxy que acaba de crear
        - Concesión de permisos delegados para estas 2 aplicaciones
        - Una vez que se han agregado dos aplicaciones de proxy, haga clic en "Conceder permisos".  Seleccione "Sí" cuando se le solicita que apruebe la solicitud.

7. Habilitar la autenticación de Windows en IIS para las máquinas de servicio en la nube de Mopria y servicio de impresión en la nube de empresa
    - Asegúrese de que se instala la característica de autenticación de Windows:
        1. Abrir el Administrador del servidor
        2. Haga clic en **administrar**
        3. Haga clic en **agregar Roles y características**
        4. Seleccione **instalación basada en roles o basada en características**
        5. Seleccione el servidor
        6. En el servidor Web (IIS) -> servidor Web -> seguridad, seleccione **autenticación de Windows**
        7. Haga clic en Siguiente hasta que finalice la instalación
    - Habilitar la autenticación de Windows en IIS:
        1. Abra el Administrador de Internet Information Services (IIS)
        2. Para cada servicio o el sitio:
            1.  Seleccione el sitio de servicios
            2.  Haga doble clic en **autenticación**
            3.  Haga clic en **Windows autenticación** y haga clic en **habilitar** en **acciones**

### <a name="step-4---configure-the-required-mdm-policies"></a>Paso 4: configurar las directivas MDM necesarias
- Inicie sesión en su proveedor de MDM
- Busque el grupo de directivas de impresión en la nube de empresa y configurar las directivas siguiendo las directrices siguientes:
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD Directory ID\>
  - CloudPrintOAuthClientId = valor "Id. de aplicación" de la aplicación Web nativo que registró en el portal de administración de Azure AD
  - CloudPrinterDiscoveryEndPoint = dirección URL externa del Proxy de aplicación Mopria detección de servicio de Azure creada en el paso 3.3 (debe ser exactamente el mismo pero sin la terminación /)
  - MopriaDiscoveryResourceId = dirección URL externa del Proxy de aplicación Mopria detección de servicio de Azure creada en el paso 3.4 (debe ser exactamente el mismo incluyendo la terminación /)
  - CloudPrintResourceId = dirección URL externa del Proxy de aplicación empresarial en la nube impresión servicio de Azure que creó en paso 3.5 (debe ser exactamente el mismo incluyendo la terminación /)
  - DiscoveryMaxPrinterLimit = \<un entero positivo\>

>   Nota: Si usa el servicio de Microsoft Intune, puede encontrar esta configuración bajo la categoría "Impresora en la nube".

|Nombre para mostrar de Intune                     |Directiva                         |
|----------------------------------------|-------------------------------|
|Dirección URL de detección de impresora                   |CloudPrinterDiscoveryEndpoint  |
|Dirección URL de autoridad de acceso de la impresora            |CloudPrintOAuthAuthority       |
|GUID de la aplicación cliente nativa de Azure            |CloudPrintOAuthClientId        |
|URI de recurso de servicio de impresión              |CloudPrintResourceId           |
|Máximo de impresoras para consultar (solo móvil)  |DiscoveryMaxPrinterLimit       |
|URI de recurso del servicio de detección de impresora  |MopriaDiscoveryResourceId      |

>   Nota: Si el grupo de directivas de impresión en la nube no está disponible, pero el proveedor de MDM admite la configuración de OMA-URI, a continuación, puede establecer las mismas directivas.  Consulte esta <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">artículo</a> para obtener más información.

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valor = `https://login.microsoftonline.com/` \<Id. de directorio de Azure AD\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valor = \<Id. de aplicación de la aplicación nativa de Azure AD\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valor = dirección URL externa del Proxy de aplicación Mopria detección de servicio de Azure creada en el paso 3.3 (debe ser exactamente el mismo pero sin la terminación /)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valor = dirección URL externa del Proxy de aplicación Mopria detección de servicio de Azure creada en el paso 3.4 (debe ser exactamente el mismo incluyendo la terminación /)
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valor = dirección URL externa del Proxy de aplicación empresarial en la nube impresión servicio de Azure que creó en paso 3.5 (debe ser exactamente el mismo incluyendo la terminación /)
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valor = \<un entero positivo\>

### <a name="step-5---publish-desired-shared-printers"></a>Paso 5: publicar impresoras compartidas deseadas
1. Instalar a la impresora deseada en el servidor de impresión
2. Compartir la impresora a través de la interfaz de usuario de las propiedades de impresora
3. Seleccione el conjunto deseado de los usuarios para conceder acceso
4. Guardar los cambios y cierre la ventana de propiedades de impresora
5. Desde un equipo Windows 10 Fall Creator Update, abra un símbolo de Windows PowerShell con privilegios elevados
   1. Ejecute los siguientes comandos
      - `find-module -Name "PublishCloudPrinter"` para confirmar que la máquina puede llegar a la Galería de PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   Nota: Es posible que vea una mensajería que indica que 'PSGallery' es un repositorio no confiable.  Escriba 'y' para continuar con la instalación.

      - Publish-CloudPrinter
        - Impresora = el nombre de impresora compartida que se definió
        - Fabricante = fabricante de la impresora
        - Modelo = modelo de impresora
        - OrgLocation = cadena JSON A especificar la ubicación de la impresora, p. ej.:   `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - Cadena SDDL = cadena SDDL que representa los permisos para la impresora. Esto se puede obtener mediante la modificación de la ficha seguridad de las propiedades de impresora correctamente y, a continuación, ejecute el siguiente comando en un símbolo del sistema: `(Get-Printer PrinterName -full).PermissionSDDL`
            P. ej. "G:DUD: (A; OICI; FA;; WD)"

          > Nota: Deberá agregar **`O:BA`** como prefijo para el resultado desde el comando de línea de comandos anterior antes de establecer el valor de la configuración de SDDL.  Por ejemplo: CADENA SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = dirección URL externa del Proxy de aplicación Mopria detección de servicio de Azure creada en el paso 3.4
        - PrintServerEndpoint = dirección URL externa del Proxy de aplicación empresarial en la nube impresión servicio de Azure que creó en paso 3.5
        - AzureClientId = Id. de aplicación del valor de aplicaciones Web nativas registrado desde arriba
        - AzureTenantGuid = identificador de directorio del inquilino de Azure AD
        - DiscoveryResourceId = [opcional] Id. de aplicación de servicio en la nube con proxy para la detección de Mopria

        > Nota: En la línea de comandos también puede especificar todos los valores de parámetro necesario.<br>
        **CloudPrinter publicar** sintaxis de comando de PowerShell: <br>
        CloudPrinter publicar-impresora \<cadena\> -fabricante \<cadena\> -modelo \<cadena\> - OrgLocation \<cadena\> - Sddl \<cadena\> - DiscoveryEndpoint \<cadena\> - PrintServerEndpoint \<cadena\> - AzureClientId \<cadena\> - AzureTenantGuid \<cadena\> [-DiscoveryResourceId \<cadena\>] <br>
        Comando de ejemplo: `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>Comprobación de la implementación
En un dispositivo unido a Azure AD que tiene las directivas MDM configuradas:
- Abra un explorador web e ir a https://&lt;servicios-machine-endpoint &gt; /mcs/services (la dirección URL externa para el extremo de detección)
- Debería ver el texto JSON que describe el conjunto de funcionalidad de este punto de conexión
- Vaya a "configuración del sistema operativo -\> dispositivos -\> , impresoras y escáneres"
    - Debería ver un vínculo "Buscar impresoras en la nube"
    - Haga clic en el vínculo
    - Haga clic en el vínculo "Seleccione una ubicación de búsqueda"
        - Debería ver la jerarquía de ubicación del dispositivo
    - Elija una ubicación y haga clic en **Aceptar** y, a continuación, haga clic en **búsqueda** para buscar las impresoras
    - Seleccione la impresora y haga clic en **Agregar dispositivo** botón
    - Después de la instalación correcta de impresora, imprimir en la impresora desde la aplicación favorita

>   Nota: Si usa la impresora "EcpPrintTest", puede encontrar el archivo de salida en el equipo del servidor de impresión en "C:\\ECPTestOutput\\EcpTestPrint.xps" ubicación.
