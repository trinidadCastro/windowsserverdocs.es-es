---
title: Crear un método de autenticación personalizado para AD FS en Windows Server
description: En este escenario se describe cómo crear un método de autenticación personalizado para AD FS en Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.openlocfilehash: f1b3e687b9fd49052dd3087fdf21084278e43804
ms.sourcegitcommit: 3c6c257526b243e876aed59e3f2dec42697f232d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92418142"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Crear un método de autenticación personalizado para AD FS en Windows Server

En este tutorial se proporcionan instrucciones para implementar un método de autenticación personalizado para AD FS en Windows Server 2012 R2. Para obtener más información, consulte [métodos de autenticación adicionales](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)).

> [!WARNING]
> El ejemplo que se puede compilar aquí es &nbsp; solo con fines educativos. &nbsp;Estas instrucciones son para la implementación más sencilla y mínima posible para exponer los elementos necesarios del modelo. &nbsp; No hay ningún back-end de autenticación, procesamiento de errores o datos de configuración.

## <a name="setting-up-the-development-box"></a>Configuración del cuadro de desarrollo

En este tutorial se usa Visual Studio 2012. El proyecto se puede compilar con cualquier entorno de desarrollo que pueda crear una clase .NET para Windows. El proyecto debe tener como destino .NET 4,5 porque los métodos **BeginAuthentication** y **TryEndAuthentication** usan el tipo **System. Security. Claims. Claim**, parte de .NET Framework versión 4.5. hay una referencia necesaria para el proyecto:

| DLL de referencia | Dónde encontrarla | Requerido para |
|--|--|--|
| Microsoft.IdentityServer.Web.dll | El archivo dll se encuentra en% WINDIR% ADFS en un servidor Windows Server 2012 R2 en el que se ha instalado AD FS.<p>Este archivo DLL debe copiarse en el equipo de desarrollo y una referencia explícita creada en el proyecto. | Tipos de interfaz, incluidos IAuthenticationContext, IProofData |

## <a name="create-the-provider"></a>Crear el proveedor

1. En Visual Studio 2012: elegir archivo->nuevo->proyecto...

2. Seleccione biblioteca de clases y asegúrese de que tiene como destino .NET 4,5.

    ![creación del proveedor](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "creación del proveedor")

3. Realice una copia de **Microsoft.IdentityServer.Web.dll** desde% WINDIR% ADFS en el servidor de Windows Server 2012 R2, donde se ha instalado AD FS y péguelo en la carpeta del proyecto en el equipo de desarrollo.

4. En **Explorador de soluciones**, haga clic con el botón derecho en **referencias** y **agregue referencia..** .

5. Vaya a la copia local de **Microsoft.IdentityServer.Web.dll** y **agregue...**

6. Haga clic en **Aceptar** para confirmar la nueva referencia:

    ![creación del proveedor](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "creación del proveedor")

    Ahora debería estar configurado para resolver todos los tipos necesarios para el proveedor.

7. Agregue una nueva clase al proyecto (haga clic con el botón derecho en el proyecto, **Agregar... Clase...**) y asígnele un nombre como **adaptador**, que se muestra a continuación:

    ![creación del proveedor](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "creación del proveedor")

8. En el nuevo archivo MyAdapter.cs, reemplace el código existente por lo siguiente:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Xml.Serialization;
    using Microsoft.IdentityServer.Web.Authentication.External;
    using Claim = System.Security.Claims.Claim;

    namespace MFAadapter
    {
        class MyAdapter : IAuthenticationAdapter
        {
            public IAuthenticationAdapterMetadata Metadata
            {
                //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
            }

            public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
            {
                //return new instance of IAdapterPresentationForm derived class
            }

            public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
            {
                return true; //its all available for now
            }

            public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
            {
                //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
            }

            public void OnAuthenticationPipelineUnload()
            {

            }

            public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
            {
                //return new instance of IAdapterPresentationForm derived class
            }

            public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
            {
                //return new instance of IAdapterPresentationForm derived class
            }

        }
    }
    ```

9. Todavía no estamos listos para compilar... Hay dos interfaces más para ir.

    Agregue dos clases más al proyecto: una es para los metadatos y la otra para el formulario de presentación.  Puede agregarlos en el mismo archivo que la clase anterior.

    ```csharp
    class MyMetadata : IAuthenticationAdapterMetadata
    {

    }

    class MyPresentationForm : IAdapterPresentationForm
    {

    }
    ```         

10. A continuación, puede Agregar los miembros necesarios para cada uno. En primer lugar, los metadatos (con comentarios en línea útiles)

    ```csharp
    class MyMetadata : IAuthenticationAdapterMetadata
    {
        //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
        public string AdminName
        {
            get { return "My Example MFA Adapter"; }
        }

        //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter 
        /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
        /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
        /// one of the methods listed in this property, the authentication attempt will fail.
        public virtual string[] AuthenticationMethods 
        {
            get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
        }

        /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
        /// to determine the best language\locale to display to the user.
        public int[] AvailableLcids
        {
            get
            {
                return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
            }
        }

        /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid. 
        /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than 
        /// one secondary authentication provider available.
        public Dictionary<int, string> FriendlyNames
        {
            get
            {
                Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
                _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
                _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
                return _friendlyNames;
            }
        }

        /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid. 
        /// These descriptions are displayed in the "choice page" offered to the user when there is more than one 
        /// secondary authentication provider available.
        public Dictionary<int, string> Descriptions
        {
            get 
            {
                Dictionary<int, string> _descriptions = new Dictionary<int, string>();
                _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
                _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
                return _descriptions; 
            }
        }

        /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
        /// Note that although the property is an array, only the first element is currently used.
        /// MUST BE ONE OF THE FOLLOWING
        /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
        /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
        /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
        /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
        public string[] IdentityClaims
        {
            get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
        }

        //All external providers must return a value of "true" for this property.
        public bool RequiresIdentity
        {
            get { return true; }
        }
    }
    ```

    Ahora debería poder usar F12 (haga clic con el botón derecho – ir a definición) en IAuthenticationAdapter para ver el conjunto de miembros de interfaz necesarios.

    A continuación, puede realizar una implementación sencilla de estos.

11. Reemplace todo el contenido de la clase por lo siguiente:

    ```csharp
    namespace MFAadapter
    {
        class MyAdapter : IAuthenticationAdapter
        {
            public IAuthenticationAdapterMetadata Metadata
            {
                //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
            }

            public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
            {
                //return new instance of IAdapterPresentationForm derived class
            }

            public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
            {
                return true; //its all available for now
            }

            public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
            {
                //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
            }

            public void OnAuthenticationPipelineUnload()
            {

            }

            public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
            {
                //return new instance of IAdapterPresentationForm derived class
            }

            public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
            {
                //return new instance of IAdapterPresentationForm derived class
            }
        }
    }
    ```

12. Todavía no estamos listos para compilar... Hay dos interfaces más para ir.

    Agregue dos clases más al proyecto: una es para los metadatos y la otra para el formulario de presentación. Puede agregarlos en el mismo archivo que la clase anterior.

    ```csharp
    class MyMetadata : IAuthenticationAdapterMetadata
    {
    }
    class MyPresentationForm : IAdapterPresentationForm
    {
    }
    ```

13. A continuación, puede Agregar los miembros necesarios para cada uno. En primer lugar, los metadatos (con comentarios en línea útiles)

    ```csharp
    class MyMetadata : IAuthenticationAdapterMetadata
    {
        //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
        public string AdminName
        {
            get { return "My Example MFA Adapter"; }
        }

        //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter
        /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
        /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
        /// one of the methods listed in this property, the authentication attempt will fail.
        public virtual string[] AuthenticationMethods
        {
            get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
        }

        /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
        /// to determine the best languagelocale to display to the user.
        public int[] AvailableLcids
        {
            get
            {
                return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
            }
        }

        /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid.
        /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than
        /// one secondary authentication provider available.
        public Dictionary<int, string> FriendlyNames
        {
            get
            {
                Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
                _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
                _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
                return _friendlyNames;
            }
        }

        /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid.
        /// These descriptions are displayed in the "choice page" offered to the user when there is more than one
        /// secondary authentication provider available.
        public Dictionary<int, string> Descriptions
        {
            get
            {
                Dictionary<int, string> _descriptions = new Dictionary<int, string>();
                _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
                _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
                return _descriptions;
            }
        }

        /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
        /// Note that although the property is an array, only the first element is currently used.
        /// MUST BE ONE OF THE FOLLOWING
        /// "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
        /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
        /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
        /// "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
        public string[] IdentityClaims
        {
            get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
        }

        //All external providers must return a value of "true" for this property.
        public bool RequiresIdentity
        {
            get { return true; }
        }
    }
    ```

    A continuación, el formulario de presentación:

    ```csharp
    class MyPresentationForm : IAdapterPresentationForm
    {
        /// Returns the HTML Form fragment that contains the adapter user interface. This data will be included in the web page that is presented
        /// to the cient.
        public string GetFormHtml(int lcid)
        {
            string htmlTemplate = Resources.FormPageHtml; //todo we will implement this
            return htmlTemplate;
        }

        /// Return any external resources, ie references to libraries etc., that should be included in
        /// the HEAD section of the presentation form html.
        public string GetFormPreRenderHtml(int lcid)
        {
            return null;
        }

        //returns the title string for the web page which presents the HTML form content to the end user
        public string GetPageTitle(int lcid)
        {
            return "MFA Adapter";
        }
    }
    ```

14.  Tenga en cuenta que "todo" para el elemento **Resources. FormPageHtml** anterior. Puede corregirlo en un minuto, pero primero vamos a agregar las instrucciones Return requeridas finales, en función de los tipos recién implementados, a la clase de adaptador inicial. Para ello, agregue lo siguiente a la implementación de IAuthenticationAdapter existente:

    ```csharp
    class MyAdapter : IAuthenticationAdapter
    {
        public IAuthenticationAdapterMetadata Metadata
        {
            //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
            get { return new MyMetadata(); }
        }

        public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
        {
            //return new instance of IAdapterPresentationForm derived class
            return new MyPresentationForm();
        }

        public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
        {
            return true; //its all available for now
        }

        public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
        {
            //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
        }

        public void OnAuthenticationPipelineUnload()
        {

        }

        public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
        {
            //return new instance of IAdapterPresentationForm derived class
            return new MyPresentationForm();
        }

        public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
        {
            //return new instance of IAdapterPresentationForm derived class
            outgoingClaims = new Claim[0];
            return new MyPresentationForm();
        }

    }
    ```

15. Ahora para el archivo de recursos que contiene el fragmento HTML. Cree un nuevo archivo de texto en la carpeta del proyecto con el siguiente contenido:

    ```html
    <div id="loginArea">
        <form method="post" id="loginForm" >
            <!-- These inputs are required by the presentation framework. Do not modify or remove -->
            <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%" />
            <input id="context" type="hidden" name="Context" value="%Context%" />
            <!-- End inputs are required by the presentation framework. -->
            <p id="pageIntroductionText">This content is provided by the MFA sample adapter. Challenge inputs should be presented below.</p>
            <label for="challengeQuestionInput" class="block">Question text</label>
            <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
            <div id="submissionArea" class="submitMargin">
                <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
            </div>
        </form>
        <div id="intro" class="groupMargin">
            <p id="supportEmail">Support information</p>
        </div>
        <script type="text/javascript" language="JavaScript">
            //<![CDATA[
            function AuthPage() { }
            AuthPage.submitAnswer = function () { return true; };
            //]]>
        </script>
    </div>
    ```

16. Después, seleccione **proyecto- \> Agregar componente... ** El archivo de recursos y el nombre de los **recursos**de archivo y haga clic en **Agregar:**

   ![creación del proveedor](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "creación del proveedor")

17. Después, en el archivo **Resources. resx** , elija **Agregar recurso... Agregar archivo existente**. Navegue hasta el archivo de texto (que contiene el fragmento html) que guardó anteriormente.

   Asegúrese de que el código de GetFormHtml resuelva el nombre del nuevo recurso correctamente en el prefijo de nombre del archivo de recursos (archivo. resx) seguido del nombre del propio recurso:

    ```csharp
    public string GetFormHtml(int lcid)
    {
        string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename
        return htmlTemplate;
    }
    ```

   Ahora debería poder compilar.

## <a name="build-the-adapter"></a>Crear el adaptador

El adaptador debe estar integrado en un ensamblado .NET con nombre seguro que se puede instalar en la GAC en Windows. Para conseguirlo en un proyecto de Visual Studio, complete los pasos siguientes:

1. Haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones y haga clic en **propiedades**.

2. En la pestaña **firma** , Active **firmar el ensamblado** y elija **<nuevo... >** en **elegir un archivo de clave de nombre seguro:**  escriba un nombre de archivo de clave y una contraseña y haga clic en **Aceptar**. A continuación, asegúrese **de que la opción firmar el ensamblado** está activada y desactivado **solo firmar** . La página de **firma** de propiedades debe tener el siguiente aspecto:

    ![creación del proveedor](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "creación del proveedor")

3. A continuación, compile la solución.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Implementación del adaptador en la máquina de pruebas de AD FS

Antes de que AD FS pueda invocar a un proveedor externo, debe estar registrado en el sistema. Los proveedores de adaptadores deben proporcionar un instalador que lleve a cabo las acciones de instalación necesarias, incluida la instalación en la GAC, y el instalador debe admitir el registro en AD FS. Si no se hace esto, el administrador debe ejecutar los pasos de Windows PowerShell siguientes. Estos pasos se pueden usar en el laboratorio para habilitar las pruebas y la depuración.

### <a name="prepare-the-test-ad-fs-machine"></a>Preparar la máquina AD FS de pruebas

Copie los archivos y agréguelos a la GAC.

1. Asegúrese de que tiene un equipo con Windows Server 2012 R2 o una máquina virtual.

2. Instale el servicio de rol de AD FS y configure una granja de servidores con al menos un nodo.

    Para obtener pasos detallados para configurar un servidor de Federación en un entorno de laboratorio, consulte la [Guía de implementación de AD FS de Windows server 2012 R2](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)).

3. Copie las herramientas de Gacutil.exe en el servidor.

    Gacutil.exe se puede encontrar en **% HOMEDRIVE% Program Files (x86) Microsoft sdkswindowsv 8.0 abinnetfx 4,0 Tools** en un equipo con Windows 8. Necesitará el propio archivo de **gacutil.exe** , así como el **1033**, **en-US**, y la otra carpeta de recursos localizada debajo de la ubicación de las **herramientas de NETFX 4,0** .

4. Copie los archivos de proveedor (uno o varios archivos. dll firmados con nombre seguro) en la misma ubicación de carpeta que **gacutil.exe** (la ubicación es solo para mayor comodidad).

5. Agregue los archivos. dll a la GAC en cada AD FS servidor de Federación de la granja:

    Ejemplo: usar la herramienta de línea de comandos GACutil.exe para agregar un archivo DLL a la GAC: `C:>.gacutil.exe /if .<yourdllname>.dll`

    Para ver la entrada resultante en la GAC:`C:>.gacutil.exe /l <yourassemblyname>`


### <a name="register-your-provider-in-ad-fs"></a>Registrar el proveedor en AD FS

Una vez cumplidos los requisitos previos anteriores, abra una ventana de comandos de Windows PowerShell en el servidor de Federación y escriba los siguientes comandos (tenga en cuenta que si está usando una granja de servidores de Federación que usa Windows Internal Database, debe ejecutar estos comandos en el servidor de Federación principal de la granja de servidores):

1. `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    Donde YourTypeName es el nombre de tipo seguro de .NET: "YourDefaultNamespace. YourIAuthenticationAdapterImplementationClassName, YourAssemblyName, version = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL"

    Se registrará el proveedor externo en AD FS, con el nombre que proporcionó como AnyNameYouWish anterior.

2. Reinicie el servicio AD FS (mediante el complemento Servicios de Windows, por ejemplo).

3. Ejecute el siguiente comando: `Get-AdfsAuthenticationProvider`.

    Esto muestra el proveedor como uno de los proveedores del sistema.

    Ejemplo:

    ```powershell
    $typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
    Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
    net stop adfssrv
    net start adfssrv
    ```

    Si tiene el servicio de registro de dispositivos habilitado en el entorno de AD FS, ejecute también el siguiente comando de PowerShell: `net start drs`

    Para comprobar el proveedor registrado, use el siguiente comando de PowerShell: `Get-AdfsAuthenticationProvider` .

    Esto muestra el proveedor como uno de los proveedores del sistema.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Crear la Directiva de autenticación AD FS que invoca el adaptador

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Crear la Directiva de autenticación con el complemento Administración de AD FS

1. Abra el complemento Administración de AD FS (en el menú **herramientas** administrador del servidor).

2. Haga clic en **directivas de autenticación**.

3. En el panel central, en **multi-factor Authentication**, haga clic en el vínculo **Editar** situado a la derecha de **configuración global**.

4. En **seleccionar métodos de autenticación adicionales** en la parte inferior de la página, active la casilla de adminName del proveedor. Haga clic en **Aplicar**.

5. Para proporcionar un "desencadenador" para invocar MFA con el adaptador, en **ubicaciones** , Compruebe la **extranet** y la **intranet**, por ejemplo. Haga clic en **OK**. (Para configurar desencadenadores por usuario de confianza, vea "crear la Directiva de autenticación mediante Windows PowerShell" a continuación).

6. Compruebe los resultados con los siguientes comandos:

    Primer uso `Get-AdfsGlobalAuthenticationPolicy` . Debería ver el nombre del proveedor como uno de los valores de AdditionalAuthenticationProvider.

    A continuación, use `Get-AdfsAdditionalAuthenticationRule` . Debería ver las reglas de la extranet y la intranet configuradas como resultado de la selección de la Directiva en la interfaz de usuario del administrador.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Crear la Directiva de autenticación mediante Windows PowerShell

1. En primer lugar, habilite el proveedor en la directiva global:

    ```powershell
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`
    ```

    > [!NOTE]
    > Tenga en cuenta que el valor proporcionado para el parámetro AdditionalAuthenticationProvider corresponde al valor proporcionado para el parámetro "Name" en el cmdlet Register-AdfsAuthenticationProvider anterior y a la propiedad "Name" de Get-AdfsAuthenticationProvider salida del cmdlet.

    ```powershell
    Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
    ```

2. A continuación, configure reglas globales o específicas del usuario de confianza para desencadenar MFA:

   Ejemplo 1: crear una regla global para requerir MFA para solicitudes externas:
   
   ```powershell
   Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'
   ```
   Ejemplo 2: crear reglas de MFA para requerir MFA para solicitudes externas a un usuario de confianza específico. (Tenga en cuenta que los proveedores individuales no se pueden conectar a usuarios de confianza individuales en AD FS en Windows Server 2012 R2).

    ```powershell
    $rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'
    ```

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticación con MFA mediante el adaptador

Por último, realice los pasos siguientes para probar el adaptador:

1. Asegúrese de que la AD FS tipo de autenticación principal global esté configurado como autenticación de formularios para la extranet y la intranet (esto hace que la demostración sea más sencilla de autenticarse como un usuario específico).

    1. En el complemento AD FS, en directivas de **autenticación**, en el área **autenticación principal** , haga clic en **Editar** junto a **configuración global**.

        1. O simplemente haga clic en la pestaña **principal** de la interfaz de usuario de la **Directiva multifactor** .

2. Asegúrese de que la **autenticación mediante formularios** es la única opción comprobada para la extranet y el método de autenticación de la intranet. Haga clic en **OK**.

3. Abra la página HTML de inicio de sesión iniciado por IDP (https:// \<fsname\> /adfs/ls/idpinitiatedsignon.htm) e inicie sesión como un usuario de ad válido en el entorno de prueba.

4. Escriba las credenciales para la autenticación principal.

5. Debería ver la página de formularios de MFA, donde aparecen preguntas de desafío de ejemplo.

    Si tiene más de un adaptador configurado, verá la página de opciones MFA con el nombre descriptivo anterior.

    ![autenticación con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "autenticación con adaptador")

    ![autenticación con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "autenticación con adaptador")

Ahora tiene una implementación en funcionamiento de la interfaz y tiene conocimientos sobre cómo funciona el modelo. Puede Trym como ejemplo adicional para establecer puntos de interrupción en BeginAuthentication y TryEndAuthentication. Observe cómo se ejecuta BeginAuthentication cuando el usuario entra por primera vez en el formulario MFA, mientras que TryEndAuthentication se desencadena en cada envío del formulario.

## <a name="update-the-adapter-for-successful-authentication"></a>Actualizar el adaptador para la autenticación correcta

Pero espere: el adaptador de ejemplo nunca se autenticará correctamente.  Esto se debe a que nada del código devuelve null para TryEndAuthentication.

Al completar los procedimientos anteriores, ha creado una implementación de adaptador básica y la ha agregado a un servidor de AD FS. Puede obtener la página de formularios MFA, pero aún no se puede autenticar porque todavía no se ha colocado la lógica correcta en la implementación de TryEndAuthentication. Vamos a agregar eso.

Recuerde la implementación de TryEndAuthentication:

    ```csharp
    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
    {
        //return new instance of IAdapterPresentationForm derived class
        outgoingClaims = new Claim[0];
        return new MyPresentationForm();
    }
    ```

Vamos a actualizarlo para que no siempre devuelva MyPresentationForm (). Para ello, puede crear un sencillo método de utilidad dentro de la clase:

    ```csharp
    static bool ValidateProofData(IProofData proofData, IAuthenticationContext authContext)
    {
        if (proofData == null || proofData.Properties == null || !proofData.Properties.ContainsKey("ChallengeQuestionAnswer"))
        {
            throw new ExternalAuthenticationException("Error - no answer found", authContext);
        }

        if ((string)proofData.Properties["ChallengeQuestionAnswer"] == "adfabric")
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    ```

A continuación, actualice TryEndAuthentication como se indica a continuación:

```csharp
public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
{
    outgoingClaims = new Claim[0];
    if (ValidateProofData(proofData, authContext))
    {
        //authn complete - return authn method
        outgoingClaims = new[]
        {
            // Return the required authentication method claim, indicating the particulate authentication method used.
            new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", "http://example.com/myauthenticationmethod1" )
        };
        return null;
    }
    else
    {
        //authentication not complete - return new instance of IAdapterPresentationForm derived class
        return new MyPresentationForm();
    }
}
```

Ahora tiene que actualizar el adaptador en el cuadro prueba. Primero debe deshacer la Directiva de AD FS, después anular el registro de AD FS y reiniciar AD FS, después quitar el archivo. dll de la GAC, agregar el archivo. dll nuevo a la GAC, registrarlo en AD FS, reiniciar AD FS y volver a configurar la Directiva de AD FS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Implementar y configurar el adaptador actualizado en el equipo de AD FS de pruebas

### <a name="clear-ad-fs-policy"></a>Borrar Directiva de AD FS

Desactive todas las casillas relacionadas con MFA en la interfaz de usuario de MFA, que se muestra a continuación, y haga clic en Aceptar.

![desactivación de directiva](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "desactivación de directiva")

### <a name="unregister-provider-windows-powershell"></a>Anular el registro del proveedor (Windows PowerShell)

`PS C:> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Ejemplo:`PS C:> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Tenga en cuenta que el valor que se pasa para "Name" es el mismo valor que "Name" que proporcionó al cmdlet Register-AdfsAuthenticationProvider. También es la propiedad "Name" que se genera a partir de Get-AdfsAuthenticationProvider.

Tenga en cuenta que, antes de anular el registro de un proveedor, debe quitar el proveedor del AdfsGlobalAuthenticationPolicy (desactivando las casillas que protegió en el complemento de administración de AD FS o mediante Windows PowerShell).

Tenga en cuenta que el servicio de AD FS debe reiniciarse después de esta operación.

### <a name="remove-assembly-from-gac"></a>Quitar ensamblado de GAC

1. En primer lugar, use el siguiente comando para buscar el nombre seguro completo de la entrada:`C:>.gacutil.exe /l <yourAdapterAssemblyName>`

    Ejemplo:`C:>.gacutil.exe /l mfaadapter`

2. A continuación, use el siguiente comando para quitarlo de la GAC:`.gacutil /u “<output from the above command>”`

    Ejemplo:`C:>.gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Agregar el ensamblado actualizado a la GAC

Asegúrese de pegar el archivo. dll actualizado en primer lugar. `C:>.gacutil.exe /if .MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Ver ensamblado en la GAC (línea de CMD)

`C:> .gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Registrar el proveedor en AD FS

1. `PS C:>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2. `PS C:>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3. Reinicie el servicio AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Crear la Directiva de autenticación con el complemento Administración de AD FS

1. Abra el complemento Administración de AD FS (en el menú **herramientas** administrador del servidor).

2. Haga clic en **directivas de autenticación**.

3. En **multi-factor Authentication**, haga clic en el vínculo **Editar** a la derecha de **configuración global**.

4. En **seleccionar métodos de autenticación adicionales**, active la casilla de adminName de su proveedor. Haga clic en **Aplicar**.

5. Para proporcionar un "desencadenador" para invocar MFA con el adaptador, en ubicaciones, Compruebe la **extranet** y la **intranet**, por ejemplo. Haga clic en **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticación con MFA mediante el adaptador

Por último, realice los pasos siguientes para probar el adaptador:

1. Asegúrese de que AD FS tipo de autenticación principal global esté configurado como **autenticación de formularios** para la extranet y la intranet (esto facilita la autenticación como un usuario específico).

    1. En el complemento Administración de AD FS, en **directivas de autenticación**, en el área **autenticación principal** , haga clic en **Editar** junto a **configuración global**.

        1. O simplemente haga clic en la pestaña **principal** de la interfaz de usuario de la Directiva multifactor.

2. Asegúrese de que la **autenticación mediante formularios** es la única opción comprobada para la **extranet** y el método de autenticación de la **intranet** . Haga clic en **OK**.

3. Abra la página HTML de inicio de sesión iniciado por IDP (https:// \<fsname\> /adfs/ls/idpinitiatedsignon.htm) e inicie sesión como un usuario de ad válido en el entorno de prueba.

4. Escriba las credenciales para la autenticación principal.

5. Debería ver la página de formularios de MFA, donde aparece un texto de desafío de ejemplo.

    1. Si tiene más de un adaptador configurado, verá la página de opciones MFA con el nombre descriptivo.

Debería ver un inicio de sesión correcto al escribir "adfabric" en la página de autenticación de MFA.

![inicio de sesión con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "inicio de sesión con adaptador")

![inicio de sesión con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "inicio de sesión con adaptador")

## <a name="see-also"></a>Consulte también

#### <a name="other-resources"></a>Otros recursos

Métodos de autenticación [adicionales](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11)) 
 [Administración de riesgos con multi-factor Authentication adicionales para aplicaciones confidenciales](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))
