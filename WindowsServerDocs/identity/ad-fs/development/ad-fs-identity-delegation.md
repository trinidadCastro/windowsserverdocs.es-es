---
title: Escenario de delegación de identidad con AD FS
description: En este escenario se describe una aplicación que necesita acceder a los recursos de back-end que requieren que la cadena de delegación de identidad realice comprobaciones de control de acceso.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 292bec5f73e2746103ffc41cde729ddc59728e0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407877"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Escenario de delegación de identidad con AD FS


[A partir del .NET Framework 4,5, Windows Identity Foundation (WIF) se ha integrado totalmente en el .NET Framework. La versión de WIF que se trata en este tema, WIF 3,5, está en desuso y solo se debe usar al desarrollar con el .NET Framework 3,5 SP1 o el .NET Framework 4. Para obtener más información sobre WIF en el .NET Framework 4,5, también conocido como WIF 4,5, consulte la documentación de Windows Identity Foundation en la guía de desarrollo de .NET Framework 4,5.] 

En este escenario se describe una aplicación que necesita acceder a los recursos de back-end que requieren que la cadena de delegación de identidad realice comprobaciones de control de acceso. Normalmente, una cadena de delegación de identidad simple se compone de la información del autor de la llamada inicial y la identidad del llamador inmediato.

En la actualidad, con el modelo de delegación de Kerberos en la plataforma Windows, los recursos de back-end solo tienen acceso a la identidad del llamador inmediato y no a los del llamador inicial. Este modelo se conoce comúnmente como el modelo de subsistema de confianza. WIF mantiene la identidad del autor de la llamada inicial, así como el llamador inmediato en la cadena de delegación mediante la propiedad actor.

En el diagrama siguiente se muestra un escenario de delegación de identidad típico en el que un empleado de Fabrikam obtiene acceso a los recursos expuestos en una aplicación Contoso.com.

![identidad](media/ad-fs-identity-delegation/id1.png)

Los usuarios ficticios que participan en este escenario son:

- Francisco Un empleado de Fabrikam que desea tener acceso a los recursos de contoso.
- Daniel Desarrollador de aplicaciones de Contoso que implementa los cambios necesarios en la aplicación.
- Adam Administrador de TI de contoso.

Los componentes implicados en este escenario son:

- web1 Una aplicación web con vínculos a recursos de back-end que requieren la identidad delegada del llamador inicial. Esta aplicación se compila con ASP.NET.
- Un servicio Web que tiene acceso a un SQL Server, que requiere la identidad delegada del llamador inicial, junto con el del llamador inmediato. Este servicio se crea con WCF.
- sts1: Un STS que está en el rol del proveedor de notificaciones y emite notificaciones que la aplicación espera (web1). Ha establecido una relación de confianza con Fabrikam.com y también con la aplicación.
- sts2: Un STS que está en el rol de proveedor de identidades para Fabrikam.com y proporciona un punto de conexión que el empleado de Fabrikam usa para autenticarse. Ha establecido una relación de confianza con Contoso.com para que los empleados de Fabrikam tengan permiso para acceder a los recursos de Contoso.com.

>[!NOTE] 
>El término "token de ActAs", que se usa a menudo en este escenario, hace referencia a un token emitido por un STS y contiene la identidad del usuario. La propiedad de actor contiene la identidad del STS.

Como se muestra en el diagrama anterior, el flujo en este escenario es:


1. La aplicación Contoso está configurada para obtener un token ActAs que contiene la identidad del empleado de Fabrikam y la identidad del llamador inmediato en la propiedad actor. Daniel ha implementado estos cambios en la aplicación.
2. La aplicación Contoso está configurada para pasar el token ActAs al servicio back-end. Daniel ha implementado estos cambios en la aplicación.
3. El servicio Web de Contoso está configurado para validar el token de ActAs mediante una llamada a sts1. Adam ha habilitado sts1 para procesar solicitudes de delegación.
4. El usuario de Fabrikam Frank accede a la aplicación contoso y se le concede acceso a los recursos de back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurar el proveedor de identidades (IP)

Hay tres opciones disponibles para el administrador de Fabrikam.com, Frank:


1. Compre e instale un producto STS como Active Directory® servicios de Federación (AD FS).
2. Suscríbase a un producto STS en la nube como STS de LiveID.
3. Cree un STS personalizado mediante WIF.

En este escenario de ejemplo, suponemos que Frank selecciona Opción1 e instala AD FS como IP-STS. También configura un punto final, denominado \windowsauth, para autenticar a los usuarios. Al hacer referencia a la documentación del producto de AD FS y hablar con Adam, el administrador de TI de Contoso, Frank establece la confianza con el dominio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configuración del proveedor de notificaciones

Las opciones disponibles para el administrador de Contoso.com, Adam, son las mismas que las descritas anteriormente para el proveedor de identidades. En este escenario de ejemplo, suponemos que Adam selecciona la opción 1 e instala AD FS 2,0 como RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configuración de la confianza con la aplicación y la dirección IP

Al hacer referencia a la documentación de AD FS, Adam establece la confianza entre Fabrikam.com y la aplicación.

## <a name="set-up-delegation"></a>Configuración de la delegación

AD FS proporciona el procesamiento de la delegación. Al hacer referencia a la documentación AD FS, Adam permite el procesamiento de tokens ActAs.

## <a name="application-specific-changes"></a>Cambios específicos de la aplicación

Se deben realizar los siguientes cambios para agregar compatibilidad con la delegación de identidad a una aplicación existente. Daniel usa WIF para realizar estos cambios.


- Almacene en caché el token de arranque que web1 ha recibido de sts1.
- Use CreateChannelActingAs con el token emitido para crear un canal al servicio Web back-end.
- Llame al método del servicio back-end.

## <a name="cache-the-bootstrap-token"></a>Almacenar en caché el token de arranque

El token de arranque es el token inicial emitido por el STS y la aplicación extrae las notificaciones de este. En este escenario de ejemplo, el token lo emite sts1 para el usuario Frank y la aplicación lo almacena en caché. En el ejemplo de código siguiente se muestra cómo recuperar un token de arranque en una aplicación ASP.NET:

```
// Get the Bootstrap Token
SecurityToken bootstrapToken = null;

IClaimsPrincipal claimsPrincipal = Thread.CurrentPrincipal as IClaimsPrincipal;
if ( claimsPrincipal != null )
{
    IClaimsIdentity claimsIdentity = (IClaimsIdentity)claimsPrincipal.Identity;
    bootstrapToken = claimsIdentity.BootstrapToken;
}
```
WIF proporciona un método, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), que crea un canal del tipo especificado que aumenta las solicitudes de emisión de tokens con el token de seguridad especificado como un elemento ActAs. Puede pasar el token de arranque a este método y, a continuación, llamar al método de servicio necesario en el canal devuelto. En este escenario de ejemplo, la identidad de Frank tiene la propiedad [actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) establecida en web1's Identity.

En el fragmento de código siguiente se muestra cómo llamar al servicio Web con [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) y, a continuación, llamar a uno de los métodos del servicio, ComputeResponse, en el canal devuelto:

```
// Get the channel factory to the backend service from the application state
ChannelFactory<IService2Channel> factory = (ChannelFactory<IService2Channel>)Application[Global.CachedChannelFactory];

// Create and setup channel to talk to the backend service
IService2Channel channel;
lock (factory)
{
// Setup the ActAs to point to the caller's token so that we perform a 
// delegated call to the backend service
// on behalf of the original caller.
    channel = factory.CreateChannelActingAs<IService2Channel>(callerToken);
}

string retval = null;

// Call the backend service and handle the possible exceptions
try
{
    retval = channel.ComputeResponse(value);
    channel.Close();
} catch (Exception exception)
{
    StringBuilder sb = new StringBuilder();
    sb.AppendLine("An unexpected exception occurred.");
    sb.AppendLine(exception.StackTrace);
    channel.Abort();
    retval = sb.ToString();
}

```
## <a name="web-service-specific-changes"></a>Cambios específicos del servicio Web

Dado que el servicio Web se compila con WCF y está habilitado para WIF, una vez que el enlace se configura con IssuedSecurityTokenParameters con la dirección del emisor adecuada, WIF controla automáticamente la validación de ActAs. 

El servicio Web expone los métodos concretos que necesita la aplicación. No se necesitan cambios de código específicos en el servicio. En el ejemplo de código siguiente se muestra la configuración del servicio Web con IssuedSecurityTokenParameters:

```
// Configure the issued token parameters with the correct settings
IssuedSecurityTokenParameters itp = new IssuedSecurityTokenParameters( "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" );
itp.IssuerMetadataAddress = new EndpointAddress( "http://localhost:6000/STS/mex" );
itp.IssuerAddress = new EndpointAddress( "http://localhost:6000/STS" );

// Create the security binding element
SecurityBindingElement sbe = SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement( itp );
sbe.MessageSecurityVersion = MessageSecurityVersion.WSSecurity11WSTrust13WSSecureConversation13WSSecurityPolicy12BasicSecurityProfile10;

// Create the HTTP transport binding element
HttpTransportBindingElement httpBE = new HttpTransportBindingElement();

// Create the custom binding using the prepared binding elements
CustomBinding binding = new CustomBinding( sbe, httpBE );

using ( ServiceHost host = new ServiceHost( typeof( Service2 ), new Uri( "http://localhost:6002/Service2" ) ) )
{
    host.AddServiceEndpoint( typeof( IService2 ), binding, "" );
    host.Credentials.ServiceCertificate.SetCertificate( "CN=localhost", StoreLocation.LocalMachine, StoreName.My );

// Enable metadata generation via HTTP GET
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
    smb.HttpGetEnabled = true;
    host.Description.Behaviors.Add( smb );
    host.AddServiceEndpoint( typeof( IMetadataExchange ), MetadataExchangeBindings.CreateMexHttpBinding(), "mex" );

// Configure the service host to use WIF
    ServiceConfiguration configuration = new ServiceConfiguration();
    configuration.IssuerNameRegistry = new TrustedIssuerNameRegistry();

    FederatedServiceCredentials.ConfigureServiceHost( host, configuration );

    host.Open();

    Console.WriteLine( "Service2 started, press ENTER to stop ..." );
    Console.ReadLine();

    host.Close();
}
```

## <a name="next-steps"></a>Pasos siguientes
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
