---
title: "Escenario de la delegación de identidad con AD FS"
description: "Este escenario describe una aplicación que necesite tener acceso a recursos de back-end que requieren la cadena de la delegación de identidad para realizar comprobaciones de control de acceso."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Escenario de la delegación de identidad con AD FS


[A partir de .NET Framework 4.5, Windows Identity Foundation (WIF) se ha completamente integrado en .NET Framework. La versión de WIF tratada en este tema, WIF 3.5, está en desuso y solo debe usarse cuando se desarrolla con .NET Framework 3.5 SP1 o .NET Framework 4. Para obtener más información sobre WIF en .NET Framework 4.5, también conocida como WIF 4.5, consulta la documentación de Windows Foundation de identidad en la Guía de desarrollo de .NET Framework 4.5.] 

Este escenario describe una aplicación que necesite tener acceso a recursos de back-end que requieren la cadena de la delegación de identidad para realizar comprobaciones de control de acceso. Normalmente consiste en una cadena de la delegación de identidad simple de la información sobre el llamador inicial y la identidad del llamador inmediato.

Con el modelo de la delegación Kerberos en la plataforma de Windows hoy en día, los recursos de back-end tienen acceso solo a la identidad del llamador inmediato y no a la del autor de la llamada inicial. Este modelo normalmente se conoce como el modelo de subsistema de confianza. WIF mantiene la identidad de la llamada inicial, así como el llamador inmediato en la cadena de delegación mediante la propiedad Actor.

El siguiente diagrama ilustra un escenario de delegación típico de identidad en el que un empleado de Fabrikam, tiene acceso a recursos que se exponen en una aplicación Contoso.com.

![Identidad](media/ad-fs-identity-delegation/id1.png)

Los usuarios ficticios participar en este escenario son:

- Francisco: Un empleado de Fabrikam que quiera tener acceso a recursos de Contoso.
- Daniel: Un Contoso desarrollador de la aplicación que implementa los cambios necesarios en la aplicación.
- ADAM: El Contoso Administrador de TI.

Son los componentes implicados en este escenario:

- Web1: una aplicación Web con vínculos a recursos de back-end que requieren que la identidad del autor de la llamada inicial delegada. Esta aplicación se cree con ASP.NET.
- Un servicio Web que tiene acceso a un servidor SQL Server, lo que requiere la identidad del autor de la llamada inicial, junto con el del llamador inmediato delegada. Este servicio se crea con WCF.
- STS1: un STS que está en la función de proveedor de notificaciones y emite las solicitudes que se espera que la aplicación (web1). Ha establecido confianza con Fabrikam.com y también con la aplicación.
- sts2: un STS que está en la función del proveedor de identidad para Fabrikam.com y proporciona un punto final que el empleado de Fabrikam se usa para autenticar. Ha establecido confianza con Contoso.com para que los empleados de Fabrikam pueden acceder a recursos en Contoso.com.

>[!NOTE] 
>El término "ActAs token", que se usa con frecuencia en este escenario, se refiere a un token que emite un STS y contiene la identidad del usuario. La propiedad Actor contiene la identidad del STS.

Como se muestra en el diagrama anterior, el flujo en este escenario es:


1. La aplicación Contoso está configurada para obtener un token de ActAs que contiene la identidad del empleado de Fabrikam e identidad del llamador inmediato en la propiedad Actor. Daniel ha implementado estos cambios a la aplicación.
2. La aplicación Contoso está configurada para pasar el token de ActAs para el servicio back-end. Daniel ha implementado estos cambios a la aplicación.
3. El servicio Contoso Web está configurado para validar el token ActAs llamando sts1. ADAM tiene habilitada la sts1 procesar las solicitudes de delegación.
4. Fabrikam usuario Frank accede a la aplicación de Contoso y obtiene acceso a los recursos de back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurar el proveedor de identidad (IP)

Existen tres opciones disponibles para el Administrador de Fabrikam.com, Frank:


1. Comprar e instalar un producto STS como servicios de federación de Active Directory® (AD FS).
2. Suscribirse a un producto de STS de nube como LiveID STS.
3. Crear a un STS personalizado con WIF.

Para este escenario de ejemplo, suponemos que Frank selecciona la opción 1 e instala AD FS como el STS de IP. También configura un punto final, denominado \windowsauth, para autenticar los usuarios. Al hacer referencia a la documentación del producto de AD FS y hablar con Adam, el Administrador de IT de Contoso, Frank establece la confianza con el dominio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurar el proveedor de notificaciones

Las opciones disponibles para el Administrador de Contoso.com, Adam, son los mismos tal como se describe anteriormente para el proveedor de identidad. Para este escenario de ejemplo, suponemos que Adam selecciona la opción 1 e instala AD FS 2.0 como el punto de reunión-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Establecer confianza con la dirección IP y la aplicación

Al hacer referencia a la documentación de AD FS, Adam establece la confianza entre Fabrikam.com y la aplicación.

## <a name="set-up-delegation"></a>Configurar la delegación

AD FS proporciona procesamiento de la delegación. Al hacer referencia a la documentación de AD FS, Adam permite que el procesamiento de tokens ActAs.

## <a name="application-specific-changes"></a>Cambios específicos de la aplicación

Los siguientes cambios deben realizarse para agregar compatibilidad para la delegación de identidad a una aplicación existente. Daniel usa WIF para realizar estos cambios.


- Almacenar en caché el token de inicio que web1 recibido de sts1.
- Usa CreateChannelActingAs con el token emitido para crear un canal al servicio Web back-end.
- Llamar al método del servicio back-end.

## <a name="cache-the-bootstrap-token"></a>Almacenar en caché el Token de inicio

El token de inicio es el token inicial emitido por el STS y la aplicación extrae reclamaciones de ella. En este escenario de ejemplo, este token emitido por sts1 para que el usuario Frank y la aplicación almacena en caché. El siguiente ejemplo de código muestra cómo recuperar una rutina token en una aplicación ASP.NET:

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
WIF proporciona un método, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), que crea un canal del tipo especificado que aumenta las solicitudes de emisión de token con el token de seguridad especificado como un elemento ActAs. Puedes pasar el token de inicio a este método y, a continuación, llama al método de servicio necesarias en el canal devuelto. En este escenario de ejemplo tiene identidad Francisco la [Actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) propiedad establecida en la identidad del web1.

El siguiente fragmento de código muestra cómo llamar al servicio Web con [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) y, a continuación, llama a uno de los métodos de servicios, ComputeResponse, en el canal devuelto:

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
## <a name="web-service-specific-changes"></a>Cambios de específicos del servicio Web

Dado que el servicio Web está creado con WCF y habilitado para WIF, una vez configurado el enlace con IssuedSecurityTokenParameters con la dirección correcta del emisor, la validación de la ActAs gestiona WIF automáticamente. 

El servicio Web expone los métodos necesarios para la aplicación. No hay ningún cambio de código específico que sea necesario en el servicio. El siguiente ejemplo de código muestra la configuración del servicio Web con IssuedSecurityTokenParameters:

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
[AD FS desarrollo](../../ad-fs/AD-FS-Development.md)  
