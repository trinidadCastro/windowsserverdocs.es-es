---
title: Conceptos de autenticación de Windows
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 29d1db15-cae0-4e3d-9d8e-241ac206bb8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 0b26dca42d64338adeb8d818629e6a5f8b037f30
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475242"
---
# <a name="windows-authentication-concepts"></a>Conceptos de autenticación de Windows

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema de información general de referencia se describen los conceptos en los que se basa la autenticación de Windows.

La autenticación es un proceso para comprobar la identidad de un objeto o una persona. Cuando se autentica un objeto, el objetivo es comprobar que el objeto sea auténtico. Cuando se autentica a una persona, el objetivo es comprobar que la persona no es una impostor.

En un contexto de redes, la autenticación es el acto de probar la identidad a una aplicación o recurso de red. Normalmente, la identidad se demuestra mediante una operación criptográfica que utiliza una clave que solo el usuario conoce (como con la criptografía de clave pública) o una clave compartida. La parte del servidor del intercambio de autenticación compara los datos firmados con una clave criptográfica conocida para validar el intento de autenticación.

Almacenar las claves criptográficas en una ubicación central segura hace que el proceso de autenticación sea escalable y fácil de mantener. Active Directory es la tecnología predeterminada y recomendada para almacenar información de identidades, que incluyen las claves criptográficas que son las credenciales del usuario. Se requiere Active Directory para implementaciones de NTLM y Kerberos predeterminadas.

Las técnicas de autenticación abarcan desde un inicio de sesión simple a un sistema operativo o un inicio de sesión en un servicio o aplicación, que identifica a los usuarios en función de algo que solo el usuario conoce, como una contraseña, a mecanismos de seguridad más eficaces que usan algo que el usuario has'such como tokens, certificados de clave pública, imágenes o atributos biológicos. En un entorno empresarial, los usuario podrían acceder a varias aplicaciones en diversos tipos de servidores dentro de una sola ubicación o entre distintas ubicaciones. Por dichos motivos, la autenticación debe admitir entornos para otras plataformas y para otros sistemas operativos de Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticación y autorización: una analogía de viaje
Una analogía de viaje puede ayudar a explicar cómo funciona la autenticación. Normalmente es necesario realizar algunas tareas preparatorias para comenzar el recorrido. El viajero debe demostrar su verdadera identidad a sus entidades de hospedaje. Esta prueba puede ser una prueba de ciudadanía, un lugar de nacimiento, un justificante personal, fotografías o cualquier cosa que necesite la ley del país del host. La identidad del viajero se valida mediante la emisión de una cuenta de Passport, que es análoga a una cuenta del sistema emitida y administrada por una organización (la entidad de seguridad). El pasaporte y el destino previsto se basan en un conjunto de reglas y regulaciones emitidos por la autoridad gubernamental.

**El recorrido**

Cuando el viaje llega al borde internacional, una protección de borde solicita credenciales y el viajero presenta su pasaporte. El proceso tiene dos pliegues:

-   La protección autentica el pasaporte comprobando que fue emitido por una autoridad de seguridad en la que el gobierno local confía (confianzas, al menos, para emitir pasaportes) y comprobando que el pasaporte no se ha modificado.

-   La protección autentica el viajero comprobando que la superficie coincide con el aspecto de la persona que se ha ideado en el pasaporte y que las demás credenciales requeridas están en buen estado.

Si el pasaporte demuestra ser válido y el viajero demuestra ser su propietario, la autenticación se realiza correctamente y se puede permitir el acceso al viajero a través del borde.

La confianza transitiva entre las autoridades de seguridad es la base de la autenticación; el tipo de autenticación que tiene lugar en un borde internacional se basa en la confianza. El gobierno local no conoce el viajero, pero confía en que el gobierno del host. Cuando el gobierno del host emitió el pasaporte, no conocía el viaje. Confía en la agencia que emitió el certificado de nacimiento u otra documentación. La agencia que emitió el certificado de nacimiento, a su vez, confía en el medico que firmó el certificado. El doctor ha testigo el nacimiento del viajero y ha sellado el certificado con la prueba directa de la identidad, en este caso con la superficie del recién nacido. La confianza que se transfiere de esta manera a través de intermediarios de confianza es transitiva.

La confianza transitiva es la base de la seguridad de red en la arquitectura cliente/servidor de Windows. Una relación de confianza fluye a lo largo de un conjunto de dominios, como un árbol de dominios, y forma una relación entre un dominio y todos los dominios que confían en ese dominio. Por ejemplo, si el dominio A tiene una confianza transitiva con el dominio B y el dominio B confía en el dominio C, el dominio A confía en el dominio C.

Existe una diferencia entre la autenticación y la autorización. Con la autenticación de, el sistema demuestra que usted es quien dijo. Con la autorización, el sistema comprueba que tiene derechos para hacer lo que desea hacer. Para que el borde sea análogo al paso siguiente, simplemente la autenticación de que el viajero es el propietario adecuado de una cuenta de Passport válida no autoriza necesariamente al viajero a entrar en un país. Los residentes de un país determinado pueden entrar en otro país simplemente presentando una cuenta de Passport solo en situaciones en las que el país que se va a introducir conceda permiso ilimitado a todos los ciudadanos de ese país en particular.

Del mismo modo, puede conceder a todos los usuarios de determinados permisos de dominio para tener acceso a un recurso. Cualquier usuario que pertenezca a ese dominio tiene acceso al recurso, de la misma forma que Canadá permite a los ciudadanos de EE. UU. entrar en Canadá. Sin embargo, los ciudadanos de EE. UU. que intenten entrar en Brasil o en la India no podrán introducir esos países simplemente presentando una cuenta de Passport, ya que ambos países requieren que los ciudadanos de EE. UU. visiten una visa válida. Por lo tanto, la autenticación no garantiza el acceso a los recursos o la autorización para usar los recursos.

## <a name="credentials"></a>Credenciales
Una cuenta de Passport y, posiblemente, visas asociadas son las credenciales aceptadas para un viajero. Sin embargo, es posible que esas credenciales no permitan a un viajero entrar o tener acceso a todos los recursos de un país. Por ejemplo, se requieren credenciales adicionales para asistir a una conferencia. En Windows, las credenciales se pueden administrar para que los titulares de la cuenta puedan acceder a los recursos a través de la red sin tener que proporcionar sus credenciales de forma repetida. Este tipo de acceso permite a los usuarios autenticarse una vez por el sistema para tener acceso a todas las aplicaciones y orígenes de datos que están autorizados a usar sin especificar otro identificador de cuenta o contraseña. La plataforma Windows aprovecha la capacidad de usar una única identidad de usuario (mantenida por Active Directory) en la red mediante el almacenamiento en caché local de las credenciales de usuario en la autoridad de seguridad local (LSA) del sistema operativo. Cuando un usuario inicia sesión en el dominio, los paquetes de autenticación de Windows utilizan las credenciales de forma transparente para proporcionar el inicio de sesión único al autenticar las credenciales en los recursos de red. Para obtener más información sobre las credenciales, consulte [procesos de credenciales en la autenticación de Windows](credentials-processes-in-windows-authentication.md).

Una forma de autenticación multifactor para el viajero podría ser el requisito de transportar y presentar varios documentos para autenticar su identidad, como una cuenta de Passport e información de registro de conferencias. Windows implementa este formulario o autenticación a través de tarjetas inteligentes, tarjetas inteligentes virtuales y tecnologías biométricas.

## <a name="security-principals-and-accounts"></a>Entidades de seguridad y cuentas
En Windows, cualquier usuario, servicio, grupo o equipo que puede iniciar una acción es una entidad de seguridad. Las entidades de seguridad tienen cuentas, que pueden ser locales en un equipo o estar basadas en dominio. Por ejemplo, los equipos Unidos a un dominio de cliente de Windows pueden participar en un dominio de red mediante la comunicación con un controlador de dominio, incluso cuando ningún usuario humano haya iniciado sesión. Para iniciar las comunicaciones, el equipo debe tener una cuenta activa en el dominio. Antes de aceptar las comunicaciones del equipo, la autoridad de seguridad local del controlador de dominio autentica la identidad del equipo y, a continuación, define el contexto de seguridad del equipo tal como lo haría para una entidad de seguridad humana. Este contexto de seguridad define la identidad y las capacidades de un usuario o servicio en un equipo determinado o un usuario, un servicio, un grupo o un equipo de una red. Por ejemplo, define los recursos, como un recurso compartido de archivos o una impresora, a los que se puede tener acceso y las acciones, como lectura, escritura o modificación, que puede realizar un usuario, un servicio o un equipo de ese recurso. Para obtener más información, consulte [entidades de seguridad](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Una cuenta es un medio para identificar a un solicitante (el usuario o servicio humano) que solicita acceso o recursos. El viaje que incluye el pasaporte de la autenticidad posee una cuenta con el país del host. Los usuarios, grupos de usuarios, objetos y servicios pueden tener cuentas individuales o compartir cuentas. Las cuentas pueden ser miembros de grupos y se les puede asignar derechos y permisos específicos. Las cuentas se pueden restringir al equipo local, al grupo de trabajo, a la red o se les debe asignar la pertenencia a un dominio.

Las cuentas integradas y los grupos de seguridad, de los que son miembros, se definen en cada versión de Windows. Mediante el uso de grupos de seguridad, puede asignar los mismos permisos de seguridad a muchos usuarios que se autentican correctamente, lo que simplifica la administración del acceso. Las reglas para emitir pasaportes pueden requerir que el viajero se asigne a determinados grupos, como la empresa, el turista o el gobierno. Este proceso garantiza permisos de seguridad coherentes en todos los miembros de un grupo. El uso de grupos de seguridad para asignar permisos significa que el control de acceso de los recursos sigue siendo constante y fácil de administrar y auditar. Al agregar y quitar usuarios que requieran acceso a los grupos de seguridad adecuados según sea necesario, puede minimizar la frecuencia de los cambios en las listas de control de acceso (ACL).

Las cuentas de servicio administradas independientes y las cuentas virtuales se introdujeron en Windows Server 2008 R2 y Windows 7 para proporcionar las aplicaciones necesarias, como Microsoft Exchange Server y Internet Information Services (IIS), con el aislamiento de sus propias cuentas de dominio, a la vez que eliminan la necesidad de que un administrador administre manualmente el nombre principal de servicio (SPN) y las credenciales de estas cuentas. Las cuentas de servicio administradas de grupo se introdujeron en Windows Server 2012 y ofrecen la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad en varios servidores. Al establecer una conexión con un servicio hospedado en una granja de servidores (como el equilibrio de carga de red), los protocolos de autenticación que admiten la autenticación mutua necesitan que todas las instancias de los servicios usen la misma entidad de seguridad.

Para obtener más información acerca de las cuentas, consulte:

-   [Cuentas de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Grupos de seguridad de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Cuentas locales](https://technet.microsoft.com/itpro/windows/keep-bastion.local-accounts)

-   [Cuentas de Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Cuentas de servicio](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identidades especiales](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticación delegada
Para usar la analogía de viajes, los países pueden emitir el mismo acceso a todos los miembros de una delegación gubernamental oficial, siempre y cuando los delegados sean conocidos. Esta delegación permite que un miembro actúe en la autoridad de otro miembro. En Windows, la autenticación delegada se produce cuando un servicio de red acepta una solicitud de autenticación de un usuario y asume la identidad de ese usuario para iniciar una nueva conexión a un segundo servicio de red. Para admitir la autenticación delegada, debe establecer servidores front-end o de primer nivel, como servidores Web, responsables de administrar las solicitudes de autenticación de cliente y los servidores back-end o de n niveles, como las bases de datos de gran tamaño, que son responsables de almacenar información. Puede delegar el derecho de configuración de la autenticación delegada a los usuarios de su organización para reducir la carga administrativa de los administradores.

Mediante el establecimiento de un servicio o un equipo como de confianza para la delegación, se permite que el servicio o el equipo completen la autenticación delegada, reciban un vale para el usuario que realiza la solicitud y, a continuación, obtenga acceso a la información de dicho usuario. Este modelo restringe el acceso a los datos en los servidores back-end solo a los usuarios o servicios que presentan credenciales con los tokens de control de acceso correctos. Además, permite el acceso a la auditoría de los recursos de back-end. Al exigir que se tenga acceso a todos los datos mediante las credenciales delegadas en el servidor para su uso en nombre del cliente, asegúrese de que el servidor no se puede poner en peligro y de que puede tener acceso a información confidencial almacenada en otros servidores. La autenticación delegada es útil para las aplicaciones de varios niveles diseñadas para usar las capacidades de inicio de sesión único en varios equipos.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticación en relaciones de confianza entre dominios
La mayoría de las organizaciones que tienen más de un dominio tienen una necesidad legítima de que los usuarios tengan acceso a recursos compartidos que se encuentran en un dominio diferente, de la misma forma que se permite el viaje a diferentes regiones del país. El control de este acceso requiere que los usuarios de un dominio también puedan autenticarse y autorizarse para usar recursos de otro dominio. Para proporcionar capacidades de autenticación y autorización entre clientes y servidores de dominios diferentes, debe haber una relación de confianza entre los dos dominios. Las confianzas son la tecnología subyacente por la que se producen comunicaciones Active Directory protegidas y son un componente de seguridad integral de la arquitectura de red de Windows Server.

Cuando existe una confianza entre dos dominios, los mecanismos de autenticación para cada dominio confían en las autenticaciones procedentes del otro dominio. Las confianzas ayudan a proporcionar acceso controlado a los recursos compartidos de un dominio de recursos, el dominio que confía, comprobando que las solicitudes de autenticación entrantes proceden de una autoridad de confianza, el dominio de confianza. De esta manera, las confianzas actúan como puentes que permiten que solo las solicitudes de autenticación validadas viajen entre dominios.

La forma en que una confianza específica pasa solicitudes de autenticación depende de cómo esté configurada. Las relaciones de confianza pueden ser unidireccionales, ya que proporcionan acceso desde el dominio de confianza a los recursos del dominio que confía, o bien de forma bidireccional, al proporcionar acceso desde cada dominio a los recursos del otro dominio. Las confianzas también son no transitivas, en cuyo caso solo existe una confianza entre los dos dominios de asociados de confianza, o transitiva, en cuyo caso la confianza se extiende automáticamente a cualquier otro dominio en el que cualquiera de los asociados confíe.

Para obtener información sobre cómo funciona una confianza, vea [Cómo funcionan las confianzas de dominio y de bosque](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transición de protocolo
La transición de protocolos ayuda a los diseñadores de aplicaciones al permitir que las aplicaciones admitan distintos mecanismos de autenticación en el nivel de autenticación del usuario y al cambiar al protocolo Kerberos para las características de seguridad, como la autenticación mutua y la delegación restringida, en los niveles de aplicación posteriores.

Para obtener más información sobre la transición de protocolos, consulte [transición del protocolo Kerberos y delegación restringida](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delegación restringida
La delegación restringida proporciona a los administradores la capacidad de especificar y exigir límites de confianza de aplicaciones limitando el ámbito en el que los servicios de aplicación pueden actuar en nombre de un usuario. Puede especificar los servicios concretos desde los que un equipo en el que se confía para la delegación puede solicitar recursos. La flexibilidad de restringir los derechos de autorización para los servicios ayuda a mejorar el diseño de la seguridad de las aplicaciones, ya que reduce las oportunidades de poner en peligro los servicios que no son de confianza.

Para obtener más información acerca de la delegación restringida, consulte [información general sobre la delegación restringida de Kerberos](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="additional-references"></a>Referencias adicionales
[Información técnica sobre inicio de sesión y autenticación en Windows](https://technet.microsoft.com/library/dn269029.aspx)


