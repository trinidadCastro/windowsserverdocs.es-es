---
title: Escenario de delegación de identidad con AD FS
description: Este escenario describe una aplicación que necesita para tener acceso a recursos de back-end que requieren la cadena de delegación de identidad para realizar comprobaciones de control de acceso.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819856"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Escenario de delegación de identidad con AD FS


[A partir de .NET Framework 4.5, Windows Identity Foundation (WIF) se ha totalmente integrado en .NET Framework. La versión de WIF mencionado en este tema, WIF 3.5, está en desuso y solo se debe usar al desarrollar con .NET Framework 3.5 SP1 o .NET Framework 4. Para obtener más información acerca de WIF en .NET Framework 4.5, también se denomina WIF 4.5, vea la documentación de Windows Identity Foundation en la Guía de desarrollo de .NET Framework 4.5.] 

Este escenario describe una aplicación que necesita para tener acceso a recursos de back-end que requieren la cadena de delegación de identidad para realizar comprobaciones de control de acceso. Una cadena de delegación de identidad sencilla suele estar compuesto por la información sobre el llamador inicial y la identidad del llamador inmediato.

Con el modelo de delegación de Kerberos en la plataforma Windows hoy en día, los recursos de back-end tienen acceso sólo a la identidad del llamador inmediato y no a los del llamador inicial. Este modelo se conoce comúnmente como el modelo de subsistema de confianza. WIF mantiene la identidad del llamador inicial, así como el llamador inmediato en la cadena de delegación mediante la propiedad de Actor.

El siguiente diagrama ilustra un escenario de delegación de identidad típico en el que un empleado de Fabrikam, tiene acceso a los recursos expuestos en una aplicación de Contoso.com.

![identidad](media/ad-fs-identity-delegation/id1.png)

Los usuarios ficticios que participa en este escenario son:

- Frank: Un empleado de Fabrikam que desea tener acceso a los recursos de Contoso.
- Daniel: Un desarrollador de la aplicación de Contoso que implementa los cambios necesarios en la aplicación.
- Adán: El Administrador de TI de Contoso.

Los componentes implicados en este escenario son:

- web1: Una aplicación Web con vínculos a recursos de back-end que requieren la identidad del llamador inicial de delegados. Esta aplicación se ha creado con ASP.NET.
- Un servicio Web que tiene acceso a SQL Server, lo que requiere la identidad delegada del llamador inicial, junto con la que el llamador inmediato. Este servicio se compila con WCF.
- sts1: Un STS que está en el rol de proveedor de notificaciones y emite las solicitudes que se esperan en la aplicación (web1). Ha establecido la confianza con Fabrikam.com y también con la aplicación.
- sts2: Un STS que está en el rol de proveedor de identidades para Fabrikam.com y proporciona un punto de conexión que el empleado de Fabrikam se utiliza para autenticar. Ha establecido en "contoso.com" de confianza para que los empleados de Fabrikam pueden acceder a recursos de Contoso.com.

>[!NOTE] 
>El término "Token de ActAs", que se utiliza a menudo en este escenario, hace referencia a un token emitido por un STS y contiene la identidad del usuario. La propiedad de Actor contiene la identidad del STS.

Como se muestra en el diagrama anterior, el flujo en este escenario es:


1. La aplicación Contoso está configurada para obtener un token de ActAs que contiene la identidad del empleado de Fabrikam e identidad del llamador inmediato en la propiedad de Actor. Daniel ha implementado estos cambios en la aplicación.
2. La aplicación Contoso está configurada para pasar el token de ActAs al servicio back-end. Daniel ha implementado estos cambios en la aplicación.
3. El servicio Web de Contoso está configurado para validar el token de ActAs llamando sts1. ADAM habilitó sts1 procesar las solicitudes de delegación.
4. Usuario de Fabrikam Frank accede a la aplicación de Contoso y se le concede acceso a los recursos de back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurar el proveedor de identidades (IP)

Hay tres opciones disponibles para el Administrador de Fabrikam.com, Frank:


1. Comprar e instalar un producto de STS como servicios de federación de Active Directory® (AD FS).
2. Suscribirse a un producto de STS en la nube como LiveID STS.
3. Crear a un STS personalizado mediante WIF.

Para este escenario de ejemplo, asumimos que Frank selecciona opción1 e instala AD FS como el IP-STS. También configura un punto de conexión, denominado \windowsauth, para autenticar a los usuarios. Haciendo referencia a la documentación del producto AD FS y hablar con Adam, el Administrador de TI de Contoso, Frank establece la confianza con el dominio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurar el proveedor de notificaciones

Las opciones disponibles para el Administrador de Contoso.com, Adam, son los mismos como se describió anteriormente para el proveedor de identidades. Para este escenario de ejemplo, asumimos que Adam selecciona la opción 1 e instala AD FS 2.0 como el RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Establecer confianza con la dirección IP y la aplicación

Haciendo referencia a la documentación de AD FS, Adam establece confianza entre Fabrikam.com y la aplicación.

## <a name="set-up-delegation"></a>Configurar la delegación

AD FS proporciona el procesamiento de la delegación. Haciendo referencia a la documentación de AD FS, Adam habilita el procesamiento de tokens de ActAs.

## <a name="application-specific-changes"></a>Cambios específicos de la aplicación

Los siguientes cambios se deben realizar para agregar compatibilidad para la delegación de identidad a una aplicación existente. Daniel usa WIF para realizar estos cambios.


- Almacenar en caché el token de arranque que web1 recibido de sts1.
- Use CreateChannelActingAs con el token emitido para crear un canal al servicio back-end Web.
- Llame al método del servicio back-end.

## <a name="cache-the-bootstrap-token"></a>Almacenar en caché el Token de arranque

El token de arranque es el token inicial emitido por el STS y la aplicación extrae las notificaciones de él. En este escenario de ejemplo, este token emitido por sts1 para el usuario Frank y la aplicación almacena en caché. Ejemplo de código siguiente muestra cómo recuperar un arranque token en una aplicación de ASP.NET:

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
WIF proporciona un método, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), que crea un canal del tipo especificado que aumenta las solicitudes de emisión de tokens con el token de seguridad especificado como un elemento ActAs. Puede pasar el token de arranque a este método y, a continuación, llame al método de servicio necesario en el canal devuelto. En este escenario de ejemplo, tiene la identidad de Frank la [Actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) propiedad establecida en la identidad del web1.

El fragmento de código siguiente muestra cómo llamar al servicio Web con [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) y, a continuación, llame a uno de los métodos de servicios, ComputeResponse, en el canal devuelto:

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
## <a name="web-service-specific-changes"></a>Cambios específicos del servicio de Web

Dado que el servicio Web está creado con WCF y habilitado para WIF, una vez que el enlace se configura con IssuedSecurityTokenParameters con la dirección del emisor correcta, se controla automáticamente la validación de las ActAs WIF. 

El servicio Web expone los métodos específicos necesarios para la aplicación. No hay ningún cambio de código específicos necesario en el servicio. Ejemplo de código siguiente muestra la configuración del servicio Web con IssuedSecurityTokenParameters:

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
[Desarrollo de AD FS](../../ad-fs/AD-FS-Development.md)  
