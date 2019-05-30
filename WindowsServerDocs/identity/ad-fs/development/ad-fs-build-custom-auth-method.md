---
title: Creación de un método de autenticación personalizada para AD FS en Windows Server
description: Este escenario describe cómo crear un método de autenticación personalizado para AD FS en Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5cff8ea1a7792906e5fd74981772e4760a2808c
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66314717"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Creación de un método de autenticación personalizada para AD FS en Windows Server

Este tutorial proporciona instrucciones para implementar un método de autenticación personalizado para ADFS en Windows Server 2012 R2. Para obtener más información, consulte [métodos de autenticación adicionales](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> El ejemplo que se puede generar aquí es&nbsp;solo con fines educativos. &nbsp;Estas instrucciones son para la implementación más sencilla y más mínima posible exponer los elementos necesarios del modelo.&nbsp; No hay ningún back-end de la autenticación, procesamiento de errores o datos de configuración. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Configurar el equipo del desarrollador

Este tutorial usa Visual Studio 2012.  El proyecto puede compilarse utilizando cualquier entorno de desarrollo que puede crear una clase .NET para Windows. El proyecto debe tener como destino .NET 4.5 porque el **BeginAuthentication** y **TryEndAuthentication** métodos usan el tipo **System.Security.Claims.Claim**, que forma parte de .NET 4.5.There de versión de Framework es una referencia necesaria para el proyecto:


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Archivo dll de referencia</strong></p></td>
<td><p><strong>Dónde encontrarla</strong></p></td>
<td><p><strong>Necesario para</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft.IdentityServer.Web.dll</p></td>
<td><p>El archivo dll se encuentra en %windir%\ADFS en un servidor de Windows Server 2012 R2 en el que se ha instalado AD FS.</p>
<p></p>
<p>Este archivo dll debe copiarse en el equipo de desarrollo y una referencia explícita en el proyecto.</p></td>
<td><p>Tipos de interfaz incluidos IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Crear el proveedor

1.  En Visual Studio 2012: Seleccione archivo -\>New -\>proyecto...

2.  Seleccione la biblioteca de clases y asegúrese de que tiene como destino .NET 4.5.
    
    ![crear el proveedor](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "crear el proveedor")

3.  Realizar una copia de **Microsoft.IdentityServer.Web.dll** desde % windir %\\ADFS en el servidor donde se ha instalado AD FS y lo pega en la carpeta del proyecto en el equipo de desarrollo de Windows Server 2012 R2.

4.  En **el Explorador de soluciones**, haga clic en **referencias** y **Agregar referencia...**

5.  Vaya a la copia local del **Microsoft.IdentityServer.Web.dll** y **agregar...**

6.  Haga clic en **Aceptar** para confirmar la nueva referencia:
    
    ![crear el proveedor](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "crear el proveedor")
    
    Es deberían ahora esté configurado para resolver todos los tipos necesarios para el proveedor. 

7.  Agregue una nueva clase al proyecto (a la derecha, haga clic en el proyecto, **Add... Clase...** ) y asígnele un nombre como **MyAdapter**, como se muestra a continuación:
    
    ![crear el proveedor](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "crear el proveedor")

8.  En el nuevo archivo MyAdapter.cs, reemplace el código existente con lo siguiente:
    
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
         
         }
         }
    
    Ahora debería poder F12 (haga clic: Ir a definición) en IAuthenticationAdapter para ver el conjunto de miembros de interfaz necesaria. 
    
    A continuación, puede hacer una implementación sencilla de estos.

9.  Reemplace todo el contenido de la clase con lo siguiente:
    
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

10. No estamos listos para compilar todavía... hay más de dos interfaces para ir.
    
    Agregar más de dos clases al proyecto: uno es para los metadatos y otro para el formulario de presentación.  Puede agregar estos elementos dentro del mismo archivo como la clase anterior.
    
        class MyMetadata : IAuthenticationAdapterMetadata
         {
         
         }
         
         class MyPresentationForm : IAdapterPresentationForm
         {
         
         }

11. A continuación, puede agregar a los miembros necesarios para cada uno. En primer lugar, los metadatos (con comentarios útiles alineado)
    
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
         
         /// Returns an array indicating the type of claim that that the adapter uses to identify the user being authenticated.
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
    
    A continuación, el formulario de presentación:
    
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

12. Tenga en cuenta el 'todo' para el **Resources.FormPageHtml** elemento anterior. 
    
    Puede corregirlo en un minuto, pero primero vamos a agregar las instrucciones return finales necesarias, según los tipos recién implementados, a la clase MyAdapter inicial.  Para ello, agregue los elementos de *cursiva* a continuación para su implementación IAuthenticationAdapter existente:
    
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

13. Ahora para el archivo de recursos que contiene el fragmento de html. Cree un nuevo archivo de texto en la carpeta del proyecto con el siguiente contenido:
    
        <div id="loginArea">
         <form method="post" id="loginForm" >
         <!-- These inputs are required by the presentation framework. Do not modify or remove -->
         <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
         <input id="context" type="hidden" name="Context" value="%Context%"/>
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
         </script></div>

14. A continuación, seleccione **proyecto -\>Agregar componente... Recursos** de archivo y asigne el nombre **recursos**y haga clic en **agregar:**
    
    ![crear el proveedor](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "crear el proveedor")

15. A continuación, en el **Resources.resx** de archivos, elija **Agregar recurso... Agregar archivo existente**.  Navegue hasta el archivo de texto (que contiene el fragmento de html) que ha guardado anteriormente.
    
    Asegúrese de que el código GetFormHtml resuelve el nombre del nuevo recurso correctamente por el prefijo de nombre de recursos (archivo .resx) seguido del nombre de recurso en Sí:
    
        public string GetFormHtml(int lcid)
        {
         string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename
         return htmlTemplate;
        }
    
    Ahora podrá compilar.

## <a name="build-the-adapter"></a>Crear el adaptador

El adaptador debe generarse en un ensamblado con nombre seguro de .NET que puede instalarse en la GAC en Windows.  Para lograr esto en un proyecto de Visual Studio, complete los pasos siguientes:

1.  Haga clic el nombre del proyecto en el Explorador de soluciones y haga clic en **propiedades**.

2.  En el **firma** pestaña **firmar el ensamblado** y elija **\<nuevo... \>** en **elegir un archivo de clave de nombre seguro:**   Escriba un nombre de archivo de clave y la contraseña y haga clic en **Aceptar**.  A continuación, asegúrese de **firmar el ensamblado** está activada y **Retrasar firma solo** está desactivada.  Las propiedades **firma** página debería ser similar al siguiente:
    
    ![el proveedor de compilación](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "el proveedor de compilación")

3.  A continuación, compile la solución.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Implementar el adaptador en la máquina de prueba de AD FS

Antes de que un proveedor externo puede invocarse por AD FS, se debe registrar en el sistema.  Proveedores de adaptador deben proporcionar a un instalador que realiza las acciones de instalación necesarios, incluida la instalación en la GAC y el programa de instalación debe admitir el registro de AD FS.  Si no que se hace, el administrador debe ejecutar los siguientes pasos de Windows PowerShell.  Estos pasos se pueden usar en el laboratorio para habilitar las pruebas y depuración.

### <a name="prepare-the-test-ad-fs-machine"></a>Prepare la máquina de prueba de AD FS

Copiar archivos y agregar a GAC.

1.  Asegúrese de que tiene un equipo de Windows Server 2012 R2 o una máquina virtual.

2.  Instalar el servicio de rol de AD FS y configurar una granja de servidores con al menos un nodo.
    
    Para que obtener pasos detallados para configurar un servidor de federación en un entorno de laboratorio, consulte el [la Guía de implementación de Windows Server 2012 R2 AD FS](https://msdn.microsoft.com/en-us/library/dn486820\(v=msdn.10\)).

3.  Copie las herramientas Gacutil.exe en el servidor.
    
    Gacutil.exe se puede encontrar en **% homedrive %\\archivos de programa (x86)\\Microsoft SDKs\\Windows\\v8.0A\\bin\\NETFX 4.0 herramientas\\** en una máquina con Windows 8.  Necesitará el **gacutil.exe** propio archivo, así como la **1033**, **en-US**y la carpeta recursos localizados a continuación el **NETFX 4.0 herramientas** ubicación.

4.  Copie los archivos de proveedor (archivos .dll con signo de nombre seguro de uno o más) en la misma ubicación de carpeta como **gacutil.exe** (la ubicación es simplemente por comodidad)

5.  Agregue los archivos .dll en GAC en cada servidor de federación de AD FS en la granja de servidores:
    
    Ejemplo: usar la herramienta de línea de comandos GACutil.exe para agregar un archivo dll a la GAC: `C:\>.\gacutil.exe /if .\<yourdllname>.dll`
    
    Para ver la entrada resultante en la GAC:`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Registre el proveedor en AD FS

Una vez que se cumplen los requisitos previos anteriores, abra una ventana de comandos de Windows PowerShell en el servidor de federación y escriba los comandos siguientes (tenga en cuenta que si usa la granja de servidores de federación que usa Windows Internal Database, debe ejecutar estos comandos en el servidor de federación principal de la granja de servidores):

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`
    
    Donde YourTypeName es el nombre de tipo seguro. NET: "YourDefaultNamespace.YourIAuthenticationAdapterImplementationClassName, Nombre_del_ensamblado, versión = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL"
    
    Esto registrará el proveedor externo en AD FS, con el nombre proporcionado como AnyNameYouWish anterior.

2.  Reinicie el servicio de AD FS (mediante el complemento Servicios de Windows, por ejemplo).

3.  Ejecute el siguiente comando: `Get-AdfsAuthenticationProvider`.
    
    Esto muestra el proveedor como uno de los proveedores en el sistema.
    
    Por ejemplo:
    
        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv
    
    Si tiene el servicio de registro de dispositivos habilitado en el entorno de AD FS, ejecute también lo siguiente:  `PS C:\>net start drs`
    
    Para comprobar el proveedor registrado, utilice el siguiente comando:`PS C:\>Get-AdfsAuthenticationProvider`.
    
    Esto muestra el proveedor como uno de los proveedores en el sistema.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Crear la directiva de autenticación de AD FS que invoca el adaptador

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Crear la directiva de autenticación mediante el complemento de administración de AD FS

1.  Abra el complemento de administración de AD FS (desde el administrador del servidor **herramientas** menú).

2.  Haga clic en **las directivas de autenticación**.

3.  En el panel central, en **Multi-factor Authentication**, haga clic en el **editar** vínculo a la derecha del **configuración Global**.

4.  En **seleccionar métodos de autenticación adicionales** en la parte inferior de la página, active la casilla para AdminName su proveedor. Haga clic en **Aplicar**.

5.  Para proporcionar un "desencadenador" para invocar MFA mediante el adaptador, en **ubicaciones** comprobar ambos **Extranet** y **Intranet**, por ejemplo. Haga clic en **Aceptar**. (Para configurar desencadenadores por usuarios de confianza de terceros, consulte "Creación de la directiva de autenticación mediante Windows PowerShell" a continuación.)

6.  Comprobar los resultados mediante los siguientes comandos:
    
    En primer lugar utilice `Get-AdfsGlobalAuthenticationPolicy`. Debería ver el nombre del proveedor como uno de los valores de AdditionalAuthenticationProvider.
    
    A continuación, utilice `Get-AdfsAdditionalAuthenticationRule`. Debería ver las reglas de Extranet y de Intranet configurado como resultado de la selección de directiva en la interfaz de usuario del administrador.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Crear la directiva de autenticación mediante Windows PowerShell

1.  En primer lugar, habilite al proveedor en la directiva global:
    
    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`
    

    > [!NOTE]
    > Tenga en cuenta que el valor proporcionado para el parámetro AdditionalAuthenticationProvider se corresponde con el valor proporcionado para el parámetro "Name" en el cmdlet Register-AdfsAuthenticationProvider por encima y a la propiedad "Name" de Salida del cmdlet Get-AdfsAuthenticationProvider. 
    > <P></P>

    
    Ejemplo:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`

2.  A continuación, configure las reglas globales o específicos de entidades de usuario de confianza para activar MFA:
    
    Ejemplo 1: crear la regla global para requerir MFA para las solicitudes externas:`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`
    
    Ejemplo 2: crear MFA reglas para requerir MFA para las solicitudes externas a un usuario de confianza específico de terceros.  (Tenga en cuenta que los proveedores individuales no se puede conectar a individuales de confianza de AD FS en Windows Server 2012 R2).
    
        PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
        PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticarse con MFA mediante el adaptador

Por último, realice los pasos siguientes para probar el adaptador:

1.  Asegúrese de que el tipo de autenticación principal global de AD FS está configurado como autenticación de formularios para la Extranet y de Intranet (Esto facilita la demostración para autenticarse como un usuario específico)
    
    1.  En la instancia de AD FS complemento, en **las directivas de autenticación**, en el **autenticación principal** área, haga clic en **editar** junto a **configuración Global**.
        
        1.  O simplemente haga clic en el **principal** pestaña desde la **directiva multifactor** la interfaz de usuario.

2.  Asegúrese de **autenticación mediante formularios** es la única opción activada para el método de autenticación de la Intranet y de la Extranet.  Haga clic en **Aceptar**.

3.  Página de inicio de sesión html abierto iniciado por el IDP (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) e inicie sesión como un usuario válido de AD en el entorno de prueba.

4.  Escriba las credenciales para la autenticación principal.

5.  Debería ver los formularios MFA, aparece la página con preguntas de ejemplo del desafío. 
    
    Si tiene más de un adaptador configurado, verá la página de elección MFA con su nombre descriptivo anterior.
    
    ![autenticación con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "autenticación con adaptador")
    
    ![autenticación con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "autenticación con adaptador")

Ahora tiene una implementación funcional de la interfaz y tendrá el conocimiento de cómo funciona el modelo. Puede trym como ejemplo adicional para establecer puntos de interrupción en el BeginAuthentication, así como el TryEndAuthentication.  Tenga en cuenta cómo BeginAuthentication se ejecuta cuando el usuario introduce primero el formulario MFA, mientras que TryEndAuthentication se desencadena en cada envío del formulario.

## <a name="update-the-adapter-for-successful-authentication"></a>Actualización del adaptador para la autenticación correcta

Pero espera – correctamente nunca se autenticará el adaptador de ejemplo\!  Esto es porque no hay nada en el código devuelve null para TryEndAuthentication.

Siguiendo los procedimientos anteriores, creó una implementación básica de adaptador y agregarlo a un servidor de AD FS.  Puede obtener la página de formularios MFA, pero no puede autenticar aún porque todavía no ha colocado la lógica correcta en su implementación TryEndAuthentication.  Así que agreguemos.

Recuerde que su implementación TryEndAuthentication:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();
     
     }

Vamos a actualizar, por lo que no siempre devuelve MyPresentationForm().  Para esto puede crear un método de utilidad simple dentro de la clase:

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

A continuación, actualice TryEndAuthentication como sigue:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     outgoingClaims = new Claim[0];
     if (ValidateProofData(proofData, authContext))
     {
     //authn complete - return authn method
     outgoingClaims = new[] 
     {
     // Return the required authentication method claim, indicating the particulate authentication method used.
     new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", 
     "http://example.com/myauthenticationmethod1" )
     };
     return null;
     }
     else
     {
     //authentication not complete - return new instance of IAdapterPresentationForm derived class
     return new MyPresentationForm();
     }
     }

Ahora tiene que actualizar el adaptador en el cuadro de prueba.  Debe primero deshacer las directivas de AD FS, a continuación, anular el registro de AD FS y reiniciar AD FS, a continuación, quite el archivo .dll de la GAC, a continuación, agregar el archivo .dll nuevo a la GAC, a continuación, registrarlo en AD FS, reiniciar AD FS y volver a configurar la directiva de AD FS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Implementar y configurar el adaptador actualizado en la prueba de la máquina AD FS

### <a name="clear-ad-fs-policy"></a>Desactive la directiva de AD FS

Borrar MFA todos los relacionados con las casillas de verificación en la UI de MFA, se muestra a continuación, a continuación, haga clic en Aceptar.

![Borrar directiva](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "Borrar directiva")

### <a name="unregister-provider-windows-powershell"></a>Anular el registro de proveedor (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Ejemplo:`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Tenga en cuenta que el valor pasado para "Name" es el mismo valor que "Name" proporcionado para el cmdlet Register-AdfsAuthenticationProvider.  También es la propiedad "Name" que es la salida de Get-AdfsAuthenticationProvider.

Tenga en cuenta que, antes de anular el registro de un proveedor, debe quitar el proveedor de la AdfsGlobalAuthenticationPolicy (bien desactivando las casillas que registró en el complemento Administración de AD FS o mediante Windows PowerShell).

Tenga en cuenta que el servicio AD FS debe reiniciarse después de esta operación.

### <a name="remove-assembly-from-gac"></a>Quitar ensamblados de GAC

1.  En primer lugar, use el siguiente comando para buscar el nombre seguro completo de la entrada:`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`
    
    Ejemplo:`C:\>.\gacutil.exe /l mfaadapter`

2.  A continuación, use el siguiente comando para quitarlo de la GAC:`.\gacutil /u “<output from the above command>”`
    
    Ejemplo:`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Agregar el ensamblado actualizado en la GAC

Asegúrese de que pegar el archivo .dll actualizados localmente en primer lugar. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Ensamblado de la vista en la GAC (línea de comandos)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Registre el proveedor en AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Reinicie el servicio AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Crear la directiva de autenticación mediante el complemento de administración de AD FS

1.  Abra el complemento de administración de AD FS (desde el administrador del servidor **herramientas** menú).

2.  Haga clic en **las directivas de autenticación**.

3.  En **Multi-factor Authentication**, haga clic en el **editar** vínculo a la derecha del **configuración Global**.

4.  En **seleccionar métodos de autenticación adicionales**, active la casilla para AdminName su proveedor. Haga clic en **Aplicar**.

5.  Para proporcionar un "desencadenador" para invocar MFA mediante el adaptador, en ubicaciones de comprobar ambos **Extranet** y **Intranet**, por ejemplo. Haga clic en **Aceptar**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticarse con MFA mediante el adaptador

Por último, realice los pasos siguientes para probar el adaptador:

1.  Asegúrese de que el tipo de autenticación principal global de AD FS está configurado como **autenticación mediante formularios** de Extranet y de Intranet (así resulta más fácil para autenticarse como un usuario específico).
    
    1.  En la administración de AD FS complemento, en **las directivas de autenticación**, en el **autenticación principal** área, haga clic en **editar** junto a **laconfiguraciónGlobal**.
        
        1.  O simplemente haga clic en el **principal** pestaña del interfaz de usuario de directiva de Multi-factor Authentication.

2.  Asegúrese de **autenticación mediante formularios** es la única opción activada para ambos el **Extranet** y **Intranet** método de autenticación.  Haga clic en **Aceptar**.

3.  Página de inicio de sesión html abierto iniciado por el IDP (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) e inicie sesión como un usuario válido de AD en el entorno de prueba.

4.  Escriba las credenciales para la autenticación principal.

5.  Debería ver los formularios MFA, aparece la página con el texto de ejemplo desafío.
    
    1.  Si tiene más de un adaptador configurado, verá la página de elección MFA con su nombre descriptivo.

Debería ver un inicio de sesión correcto al escribir "adfabric" en la página de autenticación de MFA.

![Inicie sesión con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "inicie sesión con adaptador")

![Inicie sesión con adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "inicie sesión con adaptador")

## <a name="see-also"></a>Vea también

#### <a name="other-resources"></a>Otros recursos

[Métodos de autenticación adicionales](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))  
[Administración de riesgos con autenticación multifactor adicional para aplicaciones confidenciales](https://msdn.microsoft.com/en-us/library/dn280949\(v=msdn.10\))

