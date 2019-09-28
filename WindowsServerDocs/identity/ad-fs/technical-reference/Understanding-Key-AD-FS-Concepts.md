---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Descripción de los conceptos de Servicios de federación de Active Directory (AD FS) clave
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d212c2f820204bae8e1e55dc1ebc44200e266a18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407318"
---
# <a name="understanding-key-ad-fs-concepts"></a>Descripción de los conceptos de AD FS clave
Se recomienda que conozca los conceptos importantes de Servicios de federación de Active Directory (AD FS) y familiarícese con el conjunto de características.  
  
> [!TIP]  
> Puedes encontrar enlaces adicionales de recursos de AD FS en la página del [Mapa de contenido de AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) del Wiki de Microsoft TechNet. Esta página está administrada por miembros de la comunidad de AD FS y está supervisada con regularidad por el equipo de producto de AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminología de AD FS empleada en esta guía  
  
|Término de AD FS|Definición|  
|--------------|--------------|  
|Organización de asociado de cuenta|Organización de asociado de federación que se representa mediante una confianza de proveedor de notificaciones en el Servicio de federación. La organización del asociado de cuenta contiene los usuarios que van a acceder a las aplicaciones web @ no__t-0based en el asociado de recurso.|  
|Servidor de federación de cuenta|Servidor de federación de la organización de asociado de cuenta. El servidor de federación de cuenta se basa en la autenticación de usuario para emitir tokens de seguridad a los usuarios. El servidor autentica al usuario, extrae la información de pertenencia a grupos y atributos relevantes del almacén de atributos, empaqueta esta información en notificaciones y genera y firma un token de seguridad \(which contiene las notificaciones @ no__t-1 para devolver al usuario: puede usarse en su propia organización o enviarse a una organización asociada.|  
|Base de datos de configuración de AD FS|Base de datos que se usa para almacenar todos los datos de configuración que representan a una única instancia de AD FS o Servicio de federación. Estos datos de configuración pueden almacenarse en una base de datos SQL Server o mediante la característica Windows Internal Database incluida en Windows Server 2016, Windows Server 2012 y 2012 R2, y Windows Server 2008 y 2008 R2. </br></br>Puede crear la base de datos de configuración de AD FS para SQL Server mediante la herramienta Fsconfig. exe @ no__t-0line y para Windows Internal Database mediante el Asistente para la configuración del servidor de Federación de AD FS.|  
|Proveedor de notificaciones|Organización que proporciona notificaciones a sus usuarios. Consulta "Organización de asociado de cuenta".|  
|Confianza de proveedor de notificaciones|En el AD FS Management Snap @ no__t-0in, las confianzas de proveedor de notificaciones son objetos de confianza que se suelen crear en organizaciones de asociados de recurso para representar a la organización en la relación de confianza cuyas cuentas van a acceder a los recursos del asociado de recurso. Organización. Un objeto de confianza de proveedor de notificaciones consta de una serie de identificadores, nombres y reglas característicos de ese asociado en el Servicio de federación local.|  
|Confianza de proveedor de notificaciones local|Un objeto de confianza que representa AD LDS o tercer directorio @ no__t-0party LDAP @ no__t-1based en una granja de AD FS. Un objeto de confianza de proveedor de notificaciones local consta de una serie de identificadores, nombres y reglas que identifican este directorio LDAP @ no__t-0based en el Servicio de federación local.|  
|Metadatos de federación|Formato de datos en el que se transmite la información de configuración entre un proveedor de notificaciones y un usuario de confianza para facilitar la correcta configuración de las confianzas de proveedor de notificaciones y las confianzas para usuario autenticado. El formato de datos se define en Lenguaje de marcado de aserción de seguridad \(SAML @ no__t-1 2,0 y se amplía en WS @ no__t-2Federation.|  
|Servidor de federación|Un servidor de Windows que se haya configurado mediante el Asistente para configuración de servidor de Federación de AD FS para que actúe en el rol de servidor de Federación. Un servidor de federación emite tokens y presta servicio como parte del Servicio de federación.|  
|Servidor proxy de federación|Un servidor de Windows que se ha configurado mediante el Asistente para configuración de servidor proxy de Federación de AD FS para que actúe como un servicio de proxy intermediario entre un cliente de Internet y un Servicio de federación que se encuentra detrás de un firewall en una red corporativa.|  
|Servidor de federación principal|Un servidor de Windows que se ha configurado en el rol de servidor de Federación mediante el Asistente para configuración de servidor de Federación de AD FS y tiene una copia de Read @ no__t-0write de la base de datos de configuración de AD FS. </br></br> El servidor de Federación principal se crea cuando se usa el Asistente para la configuración del servidor de Federación de AD FS y se selecciona la opción para crear un Servicio de federación nuevo y hacer que ese equipo sea el primer servidor de Federación de la granja. Todos los demás servidores de Federación de esta granja deben replicar los cambios realizados en el servidor de Federación principal en una copia de Read @ no__t-0only de la base de datos de configuración de AD FS almacenada de forma local. El término “servidor de federación principal” no es válido si la base de datos de configuración de AD FS está almacenada en una base de datos SQL, ya que cualquier servidor de federación puede leer y escribir en iguales condiciones en una base de datos de configuración hospedada en un servidor SQL.|  
|Usuario de confianza|Organización que recibe y procesa notificaciones. Consulta "Organización de asociado de recurso".|  
|Confianza para usuario autenticado|En el complemento de administración de AD FS @ no__t-0in, las relaciones de confianza para usuario autenticado son objetos de confianza que se suelen crear en:<br /><br />-Las organizaciones de asociado de cuenta para representar la organización en la relación de confianza cuyas cuentas van a acceder a los recursos de la organización del asociado de recurso.<br />-Las organizaciones de asociados de recurso para representar la confianza entre el Servicio de federación y una única aplicación web @ no__t-0based.<br /><br />Un objeto de relación de confianza para usuario autenticado consta de una serie de identificadores, nombres y reglas que identifican a este asociado o Web @ no__t-0application en el Servicio de federación local.|  
|Servidor de federación de recursos|Servidor de federación de la organización de asociado de recurso. El servidor de federación de recursos suele emitir tokens a los usuarios según un token de seguridad que emite un servidor de federación de cuenta. El servidor recibe el token de seguridad, comprueba la firma, aplica la lógica de la regla de notificación a las notificaciones sin empaquetar para generar las notificaciones salientes deseadas, genera un nuevo token de seguridad \(with las notificaciones salientes @ no__t-1 según la información de la entrada token de seguridad y firma el token nuevo para volver al usuario y, en última instancia, a la aplicación Web.|  
|Organización de asociado de recurso|Asociado de federación que se representa mediante una confianza para usuario autenticado en el Servicio de federación. El asociado de recurso emite notificaciones de los tokens de seguridad @ no__t-0based que contienen aplicaciones web @ no__t-1based publicadas a las que pueden acceder los usuarios del asociado de cuenta.|  
  
## <a name="overview-of-ad-fs"></a>Introducción a AD FS  
AD FS es una solución de acceso de identidad que proporciona a los equipos cliente \(internal o externo a la red @ no__t-1 con acceso de SSO de conexión directa a aplicaciones o servicios protegidos de Internet @ no__t-2facing, incluso cuando las cuentas de usuario y las aplicaciones son se encuentra en redes u organizaciones totalmente distintas.  
  
Cuando una aplicación o servicio está en una red y una cuenta de usuario, en otra, lo habitual es que se pida al usuario que introduzca credenciales secundarias cuando trate de acceder a dicha aplicación o servicio. Estas credenciales secundarias representan la identidad del usuario en el dominio kerberos en el que la aplicación o servicio reside. El servidor web que hospeda esa aplicación o servicio suele requerirlas para poder tomar la decisión de autorización más adecuada.  
  
Con AD FS, las organizaciones pueden omitir las solicitudes de credenciales secundarias proporcionando relaciones de confianza \(federation confía en @ no__t-1 que estas organizaciones pueden utilizar para proyectar la identidad digital de un usuario y los derechos de acceso a los asociados de confianza. En este entorno federado, cada organización sigue administrando sus propias identidades y, al mismo tiempo, también pueden presentar y aceptar identidades de otras organizaciones sin riesgo alguno.  
  
-   [El rol de los almacenes de atributos](The-Role-of-Attribute-Stores.md)  
  
-   [El papel de la base de datos de configuración de AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [El papel de las notificaciones](The-Role-of-Claims.md)  
  
-   [El papel de las reglas de notificaciones](The-Role-of-Claim-Rules.md)  
  
-   [El papel del motor de notificaciones](The-Role-of-the-Claims-Engine.md)  
  
-   [El papel de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md)  
  
-   [El papel del lenguaje de reglas de notificaciones](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinar el tipo de plantilla de regla de notificaciones que se va a usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Uso de URI en AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

