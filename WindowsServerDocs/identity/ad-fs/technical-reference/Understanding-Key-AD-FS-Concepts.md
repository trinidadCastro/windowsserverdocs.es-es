---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Conceptos de servicios de federación de Active Directory clave de descripción
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878136"
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>Descripción de los conceptos de AD FS clave
Se recomienda que conozca los conceptos importantes para los servicios de federación de Active Directory y familiarizarse con su conjunto de características.  
  
> [!TIP]  
> Puedes encontrar enlaces adicionales de recursos de AD FS en la página del [Mapa de contenido de AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) del Wiki de Microsoft TechNet. Esta página está administrada por miembros de la comunidad de AD FS y está supervisada con regularidad por el equipo de producto de AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminología de AD FS empleada en esta guía  
  
|Término de AD FS|Definición|  
|--------------|--------------|  
|Organización de asociado de cuenta|Organización de asociado de federación que se representa mediante una confianza de proveedor de notificaciones en el Servicio de federación. La organización del asociado de cuenta contiene los usuarios que tendrán acceso a Web\-en función de las aplicaciones en el asociado de recurso.|  
|Servidor de federación de cuenta|Servidor de federación de la organización de asociado de cuenta. El servidor de federación de cuenta se basa en la autenticación de usuario para emitir tokens de seguridad a los usuarios. El servidor autentica al usuario, extrae los atributos relevantes y la información de pertenencia a grupo del almacén de atributos, empaqueta esta información en notificaciones y genera y firma un token de seguridad \(que contiene las notificaciones\)para devolver al usuario, ya sea que se usará en su propia organización o para enviarse a una organización asociada.|  
|Base de datos de configuración de AD FS|Base de datos que se usa para almacenar todos los datos de configuración que representan a una única instancia de AD FS o Servicio de federación. Estos datos de configuración pueden almacenarse en una base de datos de SQL Server o mediante la característica Windows Internal Database incluido con Windows Server 2016, Windows Server 2012 y 2012 R2 y Windows Server 2008 y 2008 R2. </br></br>Puede crear la base de datos de configuración de AD FS para SQL Server mediante el comando Fsconfig.exe\-herramienta de línea y de Windows Internal Database mediante el Asistente para configuración del servidor de federación de AD FS.|  
|Proveedor de notificaciones|Organización que proporciona notificaciones a sus usuarios. Consulta "Organización de asociado de cuenta".|  
|Confianza de proveedor de notificaciones|En el complemento Administración de AD FS\-, notificaciones de confianzas de proveedores de objetos de confianza normalmente se crean en organizaciones de asociado de recurso para representar la organización en la relación de confianza cuyas cuentas tendrán acceso a los recursos en el recurso organización del asociado. Un objeto de confianza de proveedor de notificaciones consta de una serie de identificadores, nombres y reglas característicos de ese asociado en el Servicio de federación local.|  
|Confianza de proveedor de notificaciones local|Un objeto de confianza que representa AD LDS o tercero\-LDAP de terceros\-en función de los directorios en una granja de AD FS. Una variable local de notificaciones de confianza de proveedor de objeto consta de una serie de identificadores, nombres y reglas que identifiquen esta LDAP\-en función de directorio para el servicio de federación local.|  
|Metadatos de federación|Formato de datos en el que se transmite la información de configuración entre un proveedor de notificaciones y un usuario de confianza para facilitar la correcta configuración de las confianzas de proveedor de notificaciones y las confianzas para usuario autenticado. El formato de datos se define en lenguaje de marcado de aserción de seguridad \(SAML\) 2.0 y se amplía en WS\-federación.|  
|Servidor de federación|Un servidor de Windows que se ha configurado mediante el Asistente para configuración del servidor de federación de AD FS para que actúe como servidor de federación. Un servidor de federación emite tokens y presta servicio como parte del Servicio de federación.|  
|Servidor proxy de federación|Un servidor de Windows que se ha configurado mediante el Asistente de configuración de AD FS Federation Server Proxy para que actúe como servicio de un proxy intermediario entre un cliente de Internet y un servicio de federación que se encuentra detrás de un firewall en una red corporativa.|  
|Servidor de federación principal|Un servidor de Windows que se ha configurado en el rol de servidor de federación mediante el Asistente para configuración del servidor de federación de AD FS y tiene una lectura\/escribir la copia de la base de datos de configuración de AD FS. </br></br> El servidor de federación principal se crea al usar al Asistente para configuración del servidor de federación de AD FS y seleccione la opción para crear un nuevo servicio de federación y eliges ese equipo en el primer servidor de federación en la granja de servidores. Todos los demás servidores de federación de la granja deben replicar los cambios realizados en el servidor de federación principal para una lectura\-solo copia de la base de datos de configuración de AD FS que se almacena localmente. El término “servidor de federación principal” no es válido si la base de datos de configuración de AD FS está almacenada en una base de datos SQL, ya que cualquier servidor de federación puede leer y escribir en iguales condiciones en una base de datos de configuración hospedada en un servidor SQL.|  
|Usuario de confianza|Organización que recibe y procesa notificaciones. Consulta "Organización de asociado de recurso".|  
|Confianza para usuario autenticado|En el complemento Administración de AD FS\-, autenticado es objetos de confianza que suelen creados en:<br /><br />-Organizaciones de asociados de cuenta para representar a la organización en la relación de confianza cuyas cuentas tendrá acceso a los recursos de la organización del asociado de recurso.<br />-Las organizaciones asociadas para representar la confianza entre el servicio de federación y un único sitio web\-aplicación basada en.<br /><br />Un objeto de confianza para usuario autenticado consta de una serie de identificadores, nombres y reglas que identifiquen asociado o web\-aplicación al servicio de federación local.|  
|Servidor de federación de recursos|Servidor de federación de la organización de asociado de recurso. El servidor de federación de recursos suele emitir tokens a los usuarios según un token de seguridad que emite un servidor de federación de cuenta. El servidor recibe el token de seguridad, comprueba la firma, se aplica la lógica de la regla de notificación para las notificaciones sin empaquetar para generar las notificaciones salientes deseadas, genera un nuevo token de seguridad \(con las notificaciones salientes\) según la información en el token de seguridad entrante y firma el nuevo token para devolver al usuario y, finalmente, a la aplicación Web.|  
|Organización de asociado de recurso|Asociado de federación que se representa mediante una confianza para usuario autenticado en el Servicio de federación. El asociado de recurso emite notificaciones\-en función de los tokens de seguridad que contenga Web publicado\-en función de las aplicaciones que pueden tener acceso los usuarios del asociado de cuenta.|  
  
## <a name="overview-of-ad-fs"></a>Introducción a AD FS  
AD FS es una solución de acceso de identidades que provee a los equipos cliente \(internos o externos a la red\) con acceso SSO sin problemas a protegida Internet\-orientado a servicios o aplicaciones, incluso cuando las cuentas de usuario y las aplicaciones se encuentran en redes u organizaciones completamente distintas.  
  
Cuando una aplicación o servicio está en una red y una cuenta de usuario, en otra, lo habitual es que se pida al usuario que introduzca credenciales secundarias cuando trate de acceder a dicha aplicación o servicio. Estas credenciales secundarias representan la identidad del usuario en el dominio kerberos en el que la aplicación o servicio reside. El servidor web que hospeda esa aplicación o servicio suele requerirlas para poder tomar la decisión de autorización más adecuada.  
  
Con AD FS, las organizaciones pueden omitir las solicitudes de credenciales secundarias al proporcionar relaciones de confianza \(confianzas de federación\) que estas organizaciones pueden utilizar para proyectar los derechos digitales de identidad y acceso de un usuario de confianza asociados. En este entorno federado, cada organización sigue administrando sus propias identidades y, al mismo tiempo, también pueden presentar y aceptar identidades de otras organizaciones sin riesgo alguno.  
  
-   [La función de los almacenes de atributos](The-Role-of-Attribute-Stores.md)  
  
-   [El rol de la base de datos de configuración de AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [El rol de notificaciones](The-Role-of-Claims.md)  
  
-   [La función de reglas de notificación](The-Role-of-Claim-Rules.md)  
  
-   [El rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md)  
  
-   [El rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md)  
  
-   [La función de lenguaje de reglas de notificación](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinar el tipo de plantilla de regla de notificación para usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Uso de URI en AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

