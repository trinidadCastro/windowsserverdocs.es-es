---
title: Conceptos de autenticación de Windows
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee3fa19efa8c5098008749a467e649edcb5bb716
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876506"
---
# <a name="windows-authentication-concepts"></a>Conceptos de autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de referencia describe los conceptos en que se basa la autenticación de Windows.

La autenticación es un proceso para comprobar la identidad de un objeto o una persona. Cuando se autentica un objeto, el objetivo es comprobar que el objeto sea auténtico. Cuando se autentica una persona, el objetivo es comprobar que la persona que no es un impostor.

En un contexto de redes, la autenticación es el acto de probar la identidad a una aplicación o recurso de red. Normalmente, la identidad se demuestra mediante una operación de cifrado que usa una clave solo el usuario conoce (como ocurre con criptografía de clave pública) o una clave compartida. La parte del servidor del intercambio de autenticación compara los datos firmados con una clave criptográfica conocida para validar el intento de autenticación.

Almacenar las claves criptográficas en una ubicación central segura hace que el proceso de autenticación sea escalable y fácil de mantener. Active Directory es la recomendada y la tecnología predeterminada para almacenar información de identidad, que incluyen el cifrado de claves que son las credenciales del usuario. Se requiere Active Directory para implementaciones de NTLM y Kerberos predeterminadas.

Técnicas de autenticación oscilar entre un inicio de sesión simple y un sistema operativo o un inicio de sesión a un servicio o aplicación, que identifica a los usuarios en función de algo que solo el usuario conoce, como una contraseña, a los mecanismos de seguridad más eficaces que emplean algo que el has'such de usuario como los tokens, certificados de clave pública, imágenes o atributos biológicos. En un entorno empresarial, los usuario podrían acceder a varias aplicaciones en diversos tipos de servidores dentro de una sola ubicación o entre distintas ubicaciones. Por dichos motivos, la autenticación debe admitir entornos para otras plataformas y para otros sistemas operativos de Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticación y autorización: Una analogía de viaje
Una analogía de viaje puede ayudar a explicar cómo funciona la autenticación. Algunas tareas preparatorias son normalmente necesarias comenzar el viaje. El viaje debe demostrar su identidad true a sus entidades de host. Esta prueba puede tener el formato de la prueba de la ciudadanía, lugar de nacimiento, un asiento personal, fotografías o todo lo que se requieren las leyes del país de host. Identidad de viajero se valida mediante la emisión de una cuenta de passport, que es análogo a una cuenta de sistema emitido y administrados por una organización, la entidad de seguridad. La cuenta de passport y el destino previsto se basan en un conjunto de reglas y normas emitidos por la autoridad gubernamental.

**El viaje**

Cuando llega el viaje en la frontera internacional, solicita las credenciales de una restricción de borde y el viajero presenta su cuenta de passport. El proceso tiene dos vertientes:

-   La protección autentica la cuenta de passport mediante la comprobación de que se ha emitido por una entidad de seguridad que el gobierno local confíe (confianzas de, al menos, para emitir pasaportes) y comprueba que no se ha modificado la cuenta de passport.

-   La protección autentica el viajero comprobando que la cara coincide con la cara de la persona en la imagen en la cuenta de passport y que otras credenciales necesarias están en buen estado.

Si la cuenta de passport demuestra para ser válido y el viajero demuestra para ser su propietario, la autenticación es correcta y el viajero puede tener acceso entre el borde.

Confianza transitiva entre entidades de seguridad es el fundamento de autenticación; el tipo de autenticación que tiene lugar en las fronteras internacionales se basa en la confianza. El gobierno local no conoce el viaje, pero confía en que el gobierno de host no. Cuando el host digitalizada passport, no sabía bien el viajero. Confianza de la entidad que emitió el certificado de nacimiento u otra documentación. La entidad que emitió el certificado de nacimiento, a su vez, el médico que firmó el certificado de confianza. El médico había presenciado nacimiento de viajero y marca el certificado con la prueba directa de la identidad, en este caso con la superficie de la recién nacido. Relación de confianza que se transfiere de esta manera, a través de intermediarios de confianza es transitiva.

Confianza transitiva es la base de seguridad de red en la arquitectura cliente/servidor de Windows. Una relación de confianza fluye a través de un conjunto de dominios, como un árbol de dominios y establece una relación entre un dominio y todos los dominios que confían en ese dominio. Por ejemplo, si el dominio A tiene una confianza transitiva con el dominio B, y si el dominio B confía en el dominio C, el dominio A confía en el dominio C.

Hay una diferencia entre la autenticación y autorización. Con la autenticación, el sistema demuestra que usted es quien dice que ser. Con la autorización, el sistema comprueba que tiene derechos para hacer lo que desee. Para aprovechar la analogía de borde para el paso siguiente, simplemente autenticar que el viaje es el propietario adecuado de una cuenta de passport válido no necesariamente autorizar el viaje para introducir un país o región. Los residentes de un país determinado se pueden especificar otro país presentando simplemente una cuenta de passport solo en situaciones donde el país especificado que concede permiso ilimitado para todos los ciudadanos de dicho país para ENTRAR.

De forma similar, puede conceder a todos los usuarios desde un determinados permisos de dominio para tener acceso a un recurso. Cualquier usuario que pertenezca a ese dominio tiene acceso al recurso, tal como Canadá permite a los ciudadanos de Estados Unidos introducir Canadá. Sin embargo, los ciudadanos de Estados Unidos intenta entrar en Brasil o India encontrar que no entran en dichos países simplemente presentando una cuenta de passport porque ambos de estos países necesitan visitar los ciudadanos de Estados Unidos para que tenga un visa válido. Por lo tanto, la autenticación no garantiza el acceso a recursos o la autorización para usar los recursos.

## <a name="credentials"></a>Credenciales
Una cuenta de passport y visados posiblemente asociados son las credenciales aceptadas para un viaje. Sin embargo, esas credenciales no es posible que permiten un viajero introducir o tener acceso a todos los recursos dentro de un país. Por ejemplo, se necesitan credenciales adicionales para asistir a una conferencia. En Windows, las credenciales se pueden administrar y le permite a los propietarios de cuenta para tener acceso a recursos a través de la red sin necesidad de forma repetida proporcionar sus credenciales. Este tipo de acceso permite a los usuarios autenticarse una vez por el sistema para tener acceso a todas las aplicaciones y los orígenes de datos que están autorizados para usar sin escribir otro identificador de cuenta o contraseña. La plataforma de Windows aprovecha la capacidad de usar una identidad de usuario único (mantenida Active Directory) a través de la red almacenando localmente en caché las credenciales de usuario del autoridad de seguridad Local (LSA) del sistema operativo. Cuando un usuario inicia sesión en el dominio, los paquetes de autenticación de Windows usan forma transparente las credenciales para proporcionar inicio de sesión único al autenticar las credenciales a los recursos de red. Para obtener más información acerca de las credenciales, vea [procesos de las credenciales de autenticación de Windows](credentials-processes-in-windows-authentication.md).

Una forma de autenticación multifactor para el viajero podría ser el requisito para ejecutar y presentar varios documentos para autenticar su identidad, como una información de registro de passport y conferencia. Windows implementa este formulario o la autenticación mediante tarjetas inteligentes, tarjetas inteligentes virtuales y las tecnologías de biométricas. 

## <a name="security-principals-and-accounts"></a>Las cuentas y entidades de seguridad
En Windows, cualquier usuario, servicio, grupo o equipo que puede iniciar la acción es una entidad de seguridad. Las entidades de seguridad tienen cuentas, que pueden ser local en un equipo o basado en dominio. Por ejemplo, los equipos unidos a un dominio de cliente de Windows pueden participar en un dominio de red mediante la comunicación con un controlador de dominio, incluso cuando ningún usuario haya iniciado sesión. Para iniciar las comunicaciones, el equipo debe tener una cuenta activa en el dominio. Antes de aceptar comunicaciones desde el equipo, la autoridad de seguridad local en el controlador de dominio autentica la identidad del equipo y, a continuación, define el contexto de seguridad del equipo tal como lo haría para una entidad de seguridad humana. Este contexto de seguridad define la identidad y las capacidades de un usuario o un servicio en un determinado equipo o un usuario, servicio, grupo o equipo de la red. Por ejemplo, define los recursos, como un recurso compartido de archivos o impresoras, que pueden tener acceso y las acciones, como lectura, escritura o modificación, que puede realizarse por un usuario, servicio o equipo en ese recurso. Para obtener más información, consulte [entidades de seguridad](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Una cuenta es un medio para identificar a un solicitante, el usuario humano o servicio--solicitando acceso o los recursos. El viaje en el titular de la cuenta de passport auténtico posee una cuenta con el país del host. Los usuarios, grupos de usuarios, objetos y servicios, todos pueden tener cuentas individuales o compartir las cuentas. Las cuentas pueden ser miembro de grupos y se pueden asignar permisos y derechos específicos. Las cuentas pueden restringirse al equipo local, el grupo de trabajo, la red o asignadas la pertenencia a un dominio.

Las cuentas integradas y los grupos de seguridad de los cuales son miembros, se definen en cada versión de Windows. Mediante el uso de grupos de seguridad, puede asignar los mismos permisos de seguridad para muchos usuarios que se han autenticado correctamente, lo que simplifica la administración de acceso. Reglas para la emisión de pasaportes pueden exigir que el viaje asignarse a ciertos grupos, como empresa, turísticos o gobierno. Este proceso garantiza los permisos de seguridad coherente en todos los miembros de un grupo. Mediante el uso de grupos de seguridad para asignar permisos significa que el control de acceso de recursos permanece constante y fácil de administrar y auditar. Agregando y quitando los usuarios que requieren acceso de los grupos de seguridad apropiados según sea necesario, puede minimizar la frecuencia de los cambios en el control de acceso (ACL).

Independiente de cuentas de servicio administradas y cuentas virtuales se introdujeron en Windows Server 2008 R2 y Windows 7 para proporcionar a las aplicaciones necesarias, como Microsoft Exchange Server y de Internet Information Services (IIS), el aislamiento de su propio dominio cuentas, mientras se elimina la necesidad de un administrador administre manualmente el nombre principal de servicio (SPN) y las credenciales de estas cuentas. Las cuentas de servicio administradas de grupo se introdujeron en Windows Server 2012 y proporciona la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad por medio de varios servidores. Al establecer una conexión con un servicio hospedado en una granja de servidores (como el equilibrio de carga de red), los protocolos de autenticación que admiten la autenticación mutua necesitan que todas las instancias de los servicios usen la misma entidad de seguridad.

Para obtener más información acerca de las cuentas, consulte:

-   [Cuentas de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Grupos de seguridad de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Cuentas locales](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Cuentas de Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Cuentas de servicio](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identidades especiales](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticación delegada
Para usar la analogía de viaje, países podrían emitir el mismo acceso a todos los miembros de una delegación organismo oficial, siempre que los delegados son bien conocidos. Esta delegación permite a un miembro actuar en la entidad de otro miembro. En Windows, la autenticación delegada se produce cuando un servicio de red acepta una solicitud de autenticación de un usuario y asume la identidad del usuario para iniciar una nueva conexión a un segundo servicio de red. Para admitir la autenticación delegada, debe establecer los servidores front-end o de primer nivel, como servidores web, que son responsables de controlar las solicitudes de autenticación de cliente y servidores back-end o n niveles, como grandes bases de datos, que son responsables de almacenar información. Puede delegar el derecho para configurar la autenticación delegada a los usuarios de su organización a reducir la carga administrativa en los administradores.

Mediante el establecimiento de un servicio o un equipo como de confianza para delegación, permite que ese servicio o equipo completar la autenticación delegada, recibe un vale para el usuario que realiza la solicitud y, a continuación, obtener acceso a información para ese usuario. Este modelo restringe el acceso de datos en servidores back-end solo a los usuarios o los servicios que presentar credenciales con los tokens de control de acceso correctas. Además, permite para la auditoría de acceso de esos recursos de back-end. Al exigir que se tiene acceso a todos los datos por medio de credenciales que se delegan en el servidor para usarlo en nombre del cliente, asegúrese de que el servidor no puede estar en peligro y que puede obtener acceso a información confidencial se almacena en otros servidores. Autenticación delegada es útil para aplicaciones de varios niveles que están diseñadas para usar las capacidades de inicio de sesión único en varios equipos.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticación en las relaciones de confianza entre dominios
Mayoría de las organizaciones que tienen más de un dominio tiene un legítimo necesario para que los usuarios tener acceso a recursos compartidos que se encuentran en un dominio diferente, tal como el viajero se permite el viaje a diferentes regiones en el país. Controlar este acceso requiere que los usuarios de un dominio también pueden ser autenticados y autorizados para usar recursos en otro dominio. Para proporcionar capacidades de autenticación y autorización entre clientes y servidores en dominios diferentes, debe haber una relación de confianza entre los dos dominios. Las confianzas son la tecnología subyacente por el que se producen comunicaciones seguras de Active Directory y son un componente de seguridad integral de la arquitectura de red de Windows Server.

Cuando existe una confianza entre dos dominios, los mecanismos de autenticación para cada dominio confiar en las autenticaciones procedentes de otro dominio. Relaciones de confianza ayudan a proporcionar acceso controlado a los recursos compartidos en un dominio de recursos, el dominio que confía, comprobando que la autenticación entrante las solicitudes procedan de una autoridad de confianza, el dominio de confianza. De este modo, las confianzas actúan como puentes que permiten sólo validan viaje de solicitudes de autenticación entre dominios.

Cómo una relación de confianza específico pasa las solicitudes de autenticación depende de cómo esté configurada. Las relaciones de confianza pueden ser unidireccional, proporcionando acceso desde el dominio de confianza a los recursos del dominio que confía o bidireccional, proporcionando acceso de cada dominio a los recursos en el otro dominio. Confía también son ambos no transitiva, en el que existe confianza case solo entre los dominios de asociados de confianza de dos, o transitivo, en cuyo caso confianza extiende automáticamente a cualquier otros dominios que confía en cualquiera de los asociados.

Para obtener información acerca del funcionamiento de una relación de confianza, consulte [funcionamiento de las confianzas de bosque y dominio](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transición de protocolos
Transición de protocolo ayuda a los diseñadores de aplicaciones al permitir que las aplicaciones admiten distintos mecanismos de autenticación en el nivel de autenticación de usuario y al cambiar el protocolo Kerberos para las características de seguridad, como la autenticación mutua y delegación restringida, en los niveles de aplicación subsiguientes.

Para obtener más información acerca de la transición de protocolos, consulte [Kerberos Protocol Transition and Constrained Delegation](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delegación restringida
La delegación restringida proporciona a los administradores la capacidad de especificar y exigir límites de confianza de aplicaciones limitando el ámbito en que los servicios de aplicación pueden actuar en nombre de un usuario. Puede especificar determinados servicios desde la que un equipo que sea de confianza para la delegación puede solicitar los recursos. La flexibilidad para limitar los derechos de autorización para los servicios ayuda a mejorar el diseño de seguridad de la aplicación al reducir las oportunidades de compromiso por servicios de confianza.

Para obtener más información acerca de la delegación restringida, consulte [introducción de la delegación restringida de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Vea también
[Información técnica sobre inicio de sesión de Windows y autenticación](https://technet.microsoft.com/library/dn269029.aspx)


