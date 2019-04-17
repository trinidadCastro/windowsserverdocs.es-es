---
title: "Conceptos de autenticación de Windows"
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
ms.openlocfilehash: 27e124c971926edd33f102fe6009a0c552c6d814
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-concepts"></a>Conceptos de autenticación de Windows

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema de introducción de referencia describe los conceptos en que se basa la autenticación de Windows.

La autenticación es un proceso para comprobar la identidad de un objeto o la persona. Cuando se autentica con un objeto, el objetivo es comprobar que el objeto es original. Al autenticar una persona, el objetivo es comprobar que la persona no es un impostor.

En un contexto de la red, la autenticación es el acto de demostrar la identidad para una aplicación de red o un recurso. Por lo general, la identidad es comprobada por una operación criptográfica que usa una clave solo el usuario sabe (al igual que con la criptografía de clave pública) o una clave compartida. El lado del servidor de intercambio de autenticación compara los datos firmados con una clave criptográfica conocida para validar el intento de autenticación.

Almacenar las claves criptográficas en una ubicación centralizada segura hace que el proceso de autenticación escalable y fácil de mantener. Active Directory es la recomendada y la tecnología de forma predeterminada para almacenar información de identidad, que incluyen el cifrado teclas que son las credenciales del usuario. Active Directory es necesario para implementaciones de Kerberos y NTLM de forma predeterminada.

Técnicas de autenticación abarcan desde un inicio de sesión simple para un sistema operativo o un inicio de sesión de un servicio o aplicación, que identifica a los usuarios en función de algo que solo sepa el usuario, como una contraseña, los mecanismos de seguridad más eficaces que usan algo que la has'such usuario tokens, certificados de clave pública, imágenes o biológicos atributos. En un entorno empresarial, los usuarios pueden acceder a varias aplicaciones en muchos tipos de servidores en una única ubicación o entre varias ubicaciones. Por estas razones, la autenticación debe admitir entornos para otras plataformas y otros sistemas operativos Windows.

## <a name="authentication-and-authorization-a-travel-analogy"></a>Autenticación y autorización: una analogía de viajes
Una analogía de viajes puede ayudar a explicar cómo funciona la autenticación. Algunas de las preparatorias tareas son suele ser necesarios comenzar el viaje. El viaje debe demostrar su identidad de true a su autoridad de host. Esta comprobación puede estar en el formulario de la prueba de nacionalidad, lugar de nacimiento, un asiento personal, fotografías o lo que es necesario por la ley del país de host. Se valida la identidad de viajero por la emisión de una cuenta de passport, que es análogo a una cuenta de sistema emite y administra por una organización--la entidad de seguridad. El passport y el destino previsto se basan en un conjunto de reglas y reglamentaciones emitidos por la autoridad.

**El viaje**

Cuando el viajero llega al borde internacional, pide credenciales un guardia de borde y el viajero presenta su cuenta de passport. El proceso es doble:

-   La protección autentica el pasaporte, compruebe que lo emitió una entidad de seguridad que confía en el gobierno local (relaciones de confianza, como mínimo, para emitir passports) y comprobar que no se ha modificado el pasaporte.

-   La protección autentica el viajero comprobando que la cara coincide con la cara de la persona que se muestra en la cuenta de passport y que otras credenciales requeridas están en buen estado.

Si la cuenta de passport demuestra que sea válido y demuestra el viaje a ser su propietario, autenticación es correcta y el viaje puedes permitir que el acceso en el borde.

Confianza transitiva entre las entidades de seguridad es la base de autenticación; el tipo de autenticación que tiene lugar en un borde internacional se basa en confianza. El gobierno local no conoce el viajero, pero sea de confianza que hace el gobierno host. Cuando el carnet de host passport, no sabía ya sea el viaje. La Agencia que emitió el certificado de nacimiento u otra documentación de confianza. La Agencia que emitió el certificado de nacimiento, a su vez, el médico quién firmó el certificado de confianza. El médico sido testigos de nacimiento de viajero y marca el certificado con una prueba directa de la identidad, en este caso con la superficie de la recién nacido. Confianza que se transfiere de este modo, a través de intermediarios de confianza es transitiva.

Transitiva es la base de seguridad de red en la arquitectura del cliente y servidor de Windows. Una relación de confianza fluye por un conjunto de dominios, como un árbol de dominio y establece una relación entre un dominio y todos los dominios de confianza de ese dominio. Por ejemplo, si el dominio A tiene una relación de confianza transitiva con el dominio B y si el dominio B confía en el dominio C, el dominio A confía en el dominio C.

Hay una diferencia entre la autenticación y autorización. Con la autenticación, el sistema demuestra que es quien dice que ser. Con la autorización, el sistema comprueba que tienes derechos para hacer lo que quieras hacer. Para aprovechar la analogía de borde con el paso siguiente, simplemente que la viajero es el propietario adecuado de un passport válido de autenticación no necesariamente autorizar el viajero escribir un país. Los residentes en un país determinado pueden especificar otro país presentando simplemente un passport solo en situaciones donde al país en el que se especifican concede el permiso de un número ilimitado de todos los ciudadanos de dicho país introducirla.

Del mismo modo, puede conceder unos determinados permisos de dominio para acceder a un recurso de los usuarios. Cualquier usuario que pertenezca a ese dominio tenga acceso al recurso, al igual que Canadá vamos a ciudadanos de EE. UU. escribe Canadá. Sin embargo, ciudadanos estadounidenses intentan escribir Brasil o India encuentra que no entran en dichos países simplemente al presentar un passport porque ambos de esos países requieren visitando ciudadanos de Estados Unidos para tener una visa válido. Por lo tanto, autenticación no garantiza el acceso a recursos o la autorización para usar recursos.

## <a name="credentials"></a>Credenciales
Una cuenta de passport y posiblemente asociados visados son las credenciales aceptadas para un viaje. Sin embargo, las credenciales no pueden permitir un viajero escribe o tener acceso a todos los recursos dentro de un país. Por ejemplo, se necesitan credenciales adicionales a una conferencia. En Windows, se pueden administrar credenciales para que sea posible para los titulares de cuentas acceder a recursos en la red sin tener que proporcionar sus credenciales repetidamente. Este tipo de acceso permite a los usuarios autenticarse una vez por el sistema para tener acceso a todas las aplicaciones y los orígenes de datos que están autorizados a usar sin escribir otro identificador de cuenta o contraseña. La plataforma de Windows aprovecha la capacidad de usar una identidad de usuario único (mantenida Active Directory) a través de la red mediante el almacenamiento en caché local las credenciales de usuario del autoridad de seguridad Local (LSA) del sistema operativo. Cuando un usuario inicia sesión en el dominio, paquetes de autenticación de Windows usan de forma transparente las credenciales para proporcionar inicio de sesión único al autenticar las credenciales a recursos de red. Para obtener más información acerca de las credenciales, consulta [procesos de credenciales de autenticación de Windows](credentials-processes-in-windows-authentication.md).

Una forma de la autenticación multifactor para el viaje podría ser el requisito para realizar y presentar varios documentos para autenticar su identidad, como una información de registro de passport y conferencia. Windows implementa este formulario o autenticación a través de tarjetas inteligentes, tarjetas inteligentes virtuales y tecnologías biométricas. 

## <a name="security-principals-and-accounts"></a>Cuentas y entidades de seguridad
En Windows, cualquier usuario, el servicio, el grupo o el equipo que se puede iniciar acción es una entidad de seguridad. Entidades de seguridad tienen cuentas, que pueden ser locales en un equipo o basado en dominio. Por ejemplo, los equipos unidos a un dominio de cliente de Windows pueden participar en un dominio de red mediante la comunicación con un controlador de dominio, incluso cuando no está conectado ningún usuario humano. Para iniciar las comunicaciones, el equipo debe tener una cuenta activa en el dominio. Antes de aceptar comunicaciones desde el equipo, la autoridad de seguridad local en el controlador de dominio autentica la identidad del equipo y, a continuación, define el contexto de seguridad del equipo al igual que lo haría en una entidad de seguridad humana. Este contexto de seguridad define la identidad y las funcionalidades de un usuario o un servicio en un equipo en particular o un usuario, servicio, grupo o equipo en una red. Por ejemplo, define los recursos, como un recurso compartido de archivos o impresoras, que se puede acceder y las acciones, como la lectura, escritura o modificación, que se puede realizar a un usuario, el servicio o el equipo en ese recurso. Para obtener más información, consulta [entidades de seguridad](https://technet.microsoft.com/itpro/windows/keep-secure/security-principals).

Una cuenta es un medio para identificar a un solicitante: el usuario humano o el servicio, solicitando acceso o recursos. El viaje que contiene el pasaporte auténtico posee una cuenta con el país del host. Los usuarios, grupos de usuarios, objetos y servicios todos pueden tener las cuentas individuales o compartir cuentas. Cuentas pueden ser miembros de grupos y se pueden asignar permisos y derechos específicos. Cuentas pueden verse restringidas para el equipo local, el grupo de trabajo, la red o asignadas a pertenencia a un dominio.

Las cuentas integradas y los grupos de seguridad de los cuales son miembros, se definen en cada versión de Windows. Mediante el uso de grupos de seguridad, puedes asignar los mismos permisos de seguridad para muchos de los usuarios que están autenticados correctamente, lo que simplifica la administración de acceso. Las reglas para emitir passports pueden requerir que se asigne el viaje a ciertos grupos, como empresa, turísticos o gobierno. Este proceso garantiza permisos de seguridad coherente en todos los miembros de un grupo. Mediante el uso de grupos de seguridad para asignar permisos significa que el control de los recursos de acceso permanece constante y fácil de administrar y auditar. Al agregar y quitar usuarios que necesiten acceso de los grupos de seguridad adecuada según sea necesario, puedes reducir al mínimo la frecuencia de los cambios en el control de acceso (ACL).

Independiente administra las cuentas de servicio y cuentas virtuales se introdujeron en Windows Server 2008 R2 y Windows 7 para proporcionar aplicaciones necesarias, como Microsoft Exchange Server y Internet Information Services (IIS), el aislamiento de sus propias cuentas de dominio, mientras que elimina la necesidad de un administrador manualmente administran el nombre de entidad de seguridad de servicio (SPN) y credenciales para estas cuentas. Cuentas de servicio administradas grupo se introdujeron en Windows Server 2012 y proporciona la misma funcionalidad dentro del dominio, pero también amplía esa funcionalidad a través de varios servidores. Cuando se conecta a un servicio hospedado en una granja de servidores, como Equilibrio de carga de red, los protocolos de autenticación que admite la autenticación mutua requieren que todas las instancias de los servicios de usan al mismo principal.

Para obtener más información acerca de las cuentas, consulta:

-   [Cuentas de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-accounts)

-   [Grupos de seguridad de Active Directory](https://technet.microsoft.com/itpro/windows/keep-secure/active-directory-security-groups)

-   [Cuentas locales](https://technet.microsoft.com/itpro/windows/keep-secure/local-accounts)

-   [Cuentas de Microsoft](https://technet.microsoft.com/itpro/windows/keep-secure/microsoft-accounts)

-   [Cuentas de servicio](https://technet.microsoft.com/itpro/windows/keep-secure/service-accounts)

-   [Identidades especiales](https://technet.microsoft.com/itpro/windows/keep-secure/special-identities)

## <a name="delegated-authentication"></a>Autenticación delegada
Para usar la analogía de viajes, países pueden emitir el mismo acceso a todos los miembros de una delegación gubernamental oficial, siempre que los delegados son conocidos. Esta delegación vamos a un miembro actúan en la autoridad de otro miembro. En Windows, la autenticación delegada se produce cuando un servicio de red acepta una solicitud de autenticación de un usuario y asume la identidad del usuario para iniciar una nueva conexión a un segundo servicio de red. Para admitir la autenticación delegada, es necesario establecer servidores front-end o de primer nivel, como los servidores web, que están responsables de controlar solicitudes de autenticación de cliente y servidores de back-end o capas, como bases de datos, que están responsables de almacenar información. Puede delegar el derecho a configurar la autenticación delegada a los usuarios de la organización para reducir la carga administrativa de los administradores.

Con el establecimiento de un equipo o un servicio como de confianza para delegación, permites que ese servicio o equipo completar la autenticación delegada, recibir un vale del usuario que realiza la solicitud y obtener acceso a información de ese usuario. Este modelo restringe el acceso de datos en los servidores de fondo solo para los usuarios o los servicios que presente credenciales con los tokens de control de acceso correcta. Además, permite auditar el acceso a esos recursos de back-end. Al exigir que tener acceso a todos los datos mediante las credenciales delegadas en el servidor para su uso en nombre del cliente, asegúrate de que el servidor no puede estar en riesgo y que puede tener acceso a información confidencial está almacenada en otros servidores. Autenticación delegada es útil para aplicaciones de varios niveles están diseñadas para usar funciones de inicio de sesión único en varios equipos.

### <a name="authentication-in-trust-relationships-between-domains"></a>Autenticación en las relaciones de confianza entre dominios
La mayoría de las organizaciones que tienen más de un dominio es necesitan para que los usuarios acceder a recursos compartidos que se encuentran en un dominio diferente, al igual que el viajero se permite viajar en distintas regiones en el país. Controlar este acceso requiere que los usuarios en un dominio también pueden autenticar y autorizar para usar recursos en otro dominio. Para proporcionar funcionalidades de autenticación y autorización entre clientes y servidores en distintos dominios, debe haber una relación de confianza entre los dos dominios. Relaciones de confianza son la tecnología subyacente que protegido comunicaciones de Active Directory, se producen y son un componente integral de seguridad de la arquitectura de red de Windows Server.

Cuando existe una relación de confianza entre dos dominios, los mecanismos de autenticación para cada dominio confiar en las autenticaciones procedente de otro dominio. Relaciones de confianza ayudan a proporcionar acceso controlado a recursos compartidos en un dominio de recursos, el dominio de confianza, comprobando que la autenticación entrante provienen de solicitudes de una entidad de confianza, el dominio de confianza. De este modo, confianzas actúan como puentes que solo permiten validan viajes de solicitudes de autenticación entre dominios.

Cómo una relación de confianza específicas pasa las solicitudes de autenticación depende de cómo esté configurado. Relaciones de confianza pueden ser unidireccional, proporcionando acceso desde el dominio de confianza a los recursos en el dominio de confianza o bidireccional, proporcionando acceso de cada dominio a los recursos en el otro dominio. Se confía también están bien no transitiva, en el que confianza caso existe solo entre los dominios de dos confianza socio, o transitiva, en cuyo caso confianza automáticamente se extiende a otros dominios que confíe en cualquiera de los socios.

Para obtener información sobre cómo funciona una relación de confianza, vea [cómo dominio y trabajo de confianzas de bosque](https://technet.microsoft.com/library/cc773178(v=ws.10).aspx).

### <a name="protocol-transition"></a>Transición de protocolos
Transición de protocolo ayuda a los diseñadores de aplicaciones al permitir que las aplicaciones admiten diferentes mecanismos de autenticación en el nivel de autenticación de usuario y cambiando el protocolo Kerberos para las características de seguridad, como la autenticación mutua y delegación restringida, en los niveles de la aplicación posteriores.

Para obtener más información acerca de la transición de protocolos, consulta [transición del protocolo Kerberos y delegación restringida](https://technet.microsoft.com/library/cc758097(v=ws.10).aspx).

### <a name="constrained-delegation"></a>Delegación restringida
Delegación restringida permite a los administradores especificar y aplicar los límites de confianza de la aplicación mediante la limitación del ámbito donde los servicios de la aplicación pueden actuar en nombre del usuario. Puedes especificar determinados servicios desde el que un equipo que es de confianza para la delegación puede solicitar recursos. La flexibilidad para limitar los derechos de autorización de servicios te ayuda a mejorar el diseño de seguridad de la aplicación reduciendo las oportunidades de comprometer los servicios de confianza.

Para obtener más información acerca de la delegación restringida, consulta [Introducción a la delegación Kerberos restringido](../kerberos/kerberos-constrained-delegation-overview.md).

## <a name="see-also"></a>Consulta también
[Información general técnica de inicio de sesión de Windows y autenticación](https://technet.microsoft.com/library/dn269029.aspx)


