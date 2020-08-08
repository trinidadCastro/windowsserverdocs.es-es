---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Descripción de los conceptos de Servicios de federación de Active Directory (AD FS) clave
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c75f2109f7ef67cb9c83ddd05f95030904413e23
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996576"
---
# <a name="understanding-key-ad-fs-concepts"></a>Understanding Key AD FS Concepts
Se recomienda que conozca los conceptos importantes de Servicios de federación de Active Directory (AD FS) y familiarícese con el conjunto de características.

> [!TIP]
> Puede encontrar vínculos de recursos de AD FS adicionales en la descripción de los [conceptos clave de AD FS]().

## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminología de AD FS empleada en esta guía

|Término de AD FS|Definición|
|--------------|--------------|
|Organización de asociado de cuenta|Organización de asociado de federación que se representa mediante una confianza de proveedor de notificaciones en el Servicio de federación. La organización del asociado de cuenta contiene los usuarios que tendrán acceso a \- las aplicaciones basadas en Web en el asociado de recurso.|
|Servidor de federación de cuenta|Servidor de federación de la organización de asociado de cuenta. El servidor de federación de cuenta se basa en la autenticación de usuario para emitir tokens de seguridad a los usuarios. El servidor autentica al usuario, extrae la información de pertenencia a grupos y atributos relevantes del almacén de atributos, empaqueta esta información en notificaciones y genera y firma un token de seguridad \( que contiene las notificaciones \) que se van a devolver al usuario, ya sea para usarse en su propia organización o para enviarse a una organización asociada.|
|Base de datos de configuración de AD FS|Base de datos que se usa para almacenar todos los datos de configuración que representan a una única instancia de AD FS o Servicio de federación. Estos datos de configuración pueden almacenarse en una base de datos SQL Server o mediante la característica Windows Internal Database incluida en Windows Server 2016, Windows Server 2012 y 2012 R2, y Windows Server 2008 y 2008 R2. </br></br>Puede crear la base de datos de configuración de AD FS para SQL Server mediante la herramienta de línea de comandos Fsconfig.exe \- y Windows Internal Database mediante el Asistente para la configuración del servidor de Federación de AD FS.|
|Proveedor de notificaciones|Organización que proporciona notificaciones a sus usuarios. Consulta "Organización de asociado de cuenta".|
|Confianza de proveedor de notificaciones|En el complemento de administración de AD FS \- , las confianzas de proveedor de notificaciones son objetos de confianza que se suelen crear en organizaciones de asociados de recurso para representar a la organización en la relación de confianza cuyas cuentas van a acceder a los recursos de la organización del asociado de recurso. Un objeto de confianza de proveedor de notificaciones consta de una serie de identificadores, nombres y reglas característicos de ese asociado en el Servicio de federación local.|
|Confianza de proveedor de notificaciones local|Objeto de confianza que representa AD LDS o \- \- directorios basados en LDAP de terceros en una granja de AD FS. Un objeto de confianza de proveedor de notificaciones local consta de una serie de identificadores, nombres y reglas que identifican este \- directorio basado en LDAP en el servicio de Federación local.|
|Metadatos de federación|Formato de datos en el que se transmite la información de configuración entre un proveedor de notificaciones y un usuario de confianza para facilitar la correcta configuración de las confianzas de proveedor de notificaciones y las confianzas para usuario autenticado. El formato de datos se define en Lenguaje de marcado de aserción de seguridad \( SAML \) 2,0 y se extiende en WS \- Federation.|
|Servidor de federación|Un servidor de Windows que se haya configurado mediante el Asistente para configuración de servidor de Federación de AD FS para que actúe en el rol de servidor de Federación. Un servidor de federación emite tokens y presta servicio como parte del Servicio de federación.|
|Servidor proxy de federación|Un servidor de Windows que se ha configurado mediante el Asistente para configuración de servidor proxy de Federación de AD FS para que actúe como un servicio de proxy intermediario entre un cliente de Internet y un Servicio de federación que se encuentra detrás de un firewall en una red corporativa.|
|Servidor de federación principal|Un servidor de Windows que se ha configurado en el rol de servidor de Federación mediante el Asistente para configuración de servidor de Federación de AD FS y tiene una \/ copia de lectura y escritura de la base de datos de configuración de AD FS. </br></br> El servidor de Federación principal se crea cuando se usa el Asistente para la configuración del servidor de Federación de AD FS y se selecciona la opción para crear un Servicio de federación nuevo y hacer que ese equipo sea el primer servidor de Federación de la granja. Todos los demás servidores de Federación de esta granja deben replicar los cambios realizados en el servidor de Federación principal en una \- copia de solo lectura de la base de datos de configuración de AD FS almacenada de forma local. El término "servidor de Federación principal" no se aplica cuando la base de datos de configuración de AD FS se almacena en una base de datos SQL, ya que todos los servidores de Federación pueden leer y escribir igualmente en una base de datos de configuración almacenada en un SQL Server.|
|Usuario de confianza|Organización que recibe y procesa notificaciones. Consulta "Organización de asociado de recurso".|
|Confianza para usuario autenticado|En el complemento de administración de AD FS \- en, las relaciones de confianza para usuario autenticado son objetos de confianza que se suelen crear en:<p>-Las organizaciones de asociado de cuenta para representar la organización en la relación de confianza cuyas cuentas van a acceder a los recursos de la organización del asociado de recurso.<br />-Las organizaciones de asociados de recurso para representar la confianza entre el Servicio de federación y una única \- aplicación basada en Web.<p>Un objeto de relación de confianza para usuario autenticado consta de una serie de identificadores, nombres y reglas que identifican a este asociado o \- aplicación web en el servicio de Federación local.|
|Servidor de federación de recursos|Servidor de federación de la organización de asociado de recurso. El servidor de federación de recursos suele emitir tokens a los usuarios según un token de seguridad que emite un servidor de federación de cuenta. El servidor recibe el token de seguridad, comprueba la firma, aplica la lógica de la regla de notificación a las notificaciones sin empaquetar para generar las notificaciones salientes deseadas, genera un nuevo token \( de seguridad con las notificaciones salientes en \) función de la información del token de seguridad entrante y firma el token nuevo para volver al usuario y, en última instancia, a la aplicación Web.|
|Organización de asociado de recurso|Asociado de federación que se representa mediante una confianza para usuario autenticado en el Servicio de federación. El asociado de recurso emite \- tokens de seguridad basados en notificaciones que contienen \- aplicaciones basadas en Web publicadas a las que pueden acceder los usuarios del asociado de cuenta.|

## <a name="overview-of-ad-fs"></a>Introducción a AD FS
AD FS es una solución de acceso de identidad que proporciona a los equipos cliente \( internos o externos de la red un \) acceso SSO sin problemas a \- aplicaciones o servicios protegidos con conexión a Internet, incluso cuando las cuentas de usuario y las aplicaciones se encuentran en redes u organizaciones totalmente diferentes.

Cuando una aplicación o servicio está en una red y una cuenta de usuario, en otra, lo habitual es que se pida al usuario que introduzca credenciales secundarias cuando trate de acceder a dicha aplicación o servicio. Estas credenciales secundarias representan la identidad del usuario en el dominio kerberos en el que la aplicación o servicio reside. El servidor web que hospeda esa aplicación o servicio suele requerirlas para poder tomar la decisión de autorización más adecuada.

Con AD FS, las organizaciones pueden omitir las solicitudes de credenciales secundarias proporcionando relaciones de confianza \( confianzas de Federación \) que estas organizaciones pueden utilizar para proyectar la identidad digital de un usuario y los derechos de acceso a los asociados de confianza. En este entorno federado, cada organización sigue administrando sus propias identidades y, al mismo tiempo, también pueden presentar y aceptar identidades de otras organizaciones sin riesgo alguno.

-   [El rol de los almacenes de atributos](The-Role-of-Attribute-Stores.md)

-   [El papel de la base de datos de configuración de AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)

-   [El papel de las notificaciones](The-Role-of-Claims.md)

-   [El papel de las reglas de notificaciones](The-Role-of-Claim-Rules.md)

-   [El papel del motor de notificaciones](The-Role-of-the-Claims-Engine.md)

-   [El papel de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md)

-   [El papel del lenguaje de reglas de notificaciones](The-Role-of-the-Claim-Rule-Language.md)

-   [Determinar el tipo de plantilla de regla de notificaciones que se va a usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)

-   [Uso de URI en AD FS](How-URIs-Are-Used-in-AD-FS.md)