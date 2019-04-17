---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: "Conceptos de servicios de federación de Active Directory clave de descripción"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>Descripción de los conceptos de clave de AD FS
Se recomienda que obtener información sobre los conceptos importantes para los servicios de federación de Active Directory y familiarizarse con su conjunto de características.  
  
> [!TIP]  
> Puedes encontrar vínculos adicionales de recursos de AD FS en la [AD FS contenido mapa](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) página en la Wiki de TechNet de Microsoft. Esta página es administrada por miembros de la Comunidad de AD FS y se supervisa regularmente por el equipo de AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminología de AD FS usado en esta guía  
  
|Término de AD FS|Definición|  
|--------------|--------------|  
|Organización de partner de cuenta|Una organización de partner de federación está representada por una confianza de proveedor de notificaciones en el servicio de federación. La organización de partner cuenta contiene los usuarios que tendrán acceso a aplicaciones basadas en Web\ en el partner de recursos.|  
|Servidor de federación de cuenta|El servidor de federación de la organización de partner de la cuenta. El servidor de federación de cuenta emite tokens de seguridad a los usuarios en función de autenticación de usuario. El servidor autentica al usuario, extrae los atributos relevantes y la información de pertenencia a grupo fuera del almacén de atributo, esta información a notificaciones, los paquetes y genera y firma un token de seguridad \ (que contiene la claims\) para devolver al usuario, ya sea para usarse en su propia organización o que se enviará a una organización asociada.|  
|Base de datos de configuración de AD FS|Una base de datos usada para almacenar todos los datos de configuración que representa una sola instancia de AD FS o servicios de federación. Estos datos de configuración pueden almacenarse en una base de datos de SQL Server o usar la característica de base de datos interna de Windows incluida en Windows Server 2016, Windows Server 2012 y 2012 R2 y Windows Server 2008 y 2008 R2. </br></br>Puedes crear la base de datos de configuración de AD FS para SQL Server mediante la herramienta de línea de Web\ Fsconfig.exe y para la base de datos interna de Windows mediante el Asistente para configuración del servidor de federación de AD FS.|  
|Proveedor de notificaciones|La organización que proporciona notificaciones a los usuarios. Consulta la organización de partner de cuenta.|  
|Reclamaciones proveedor confianza|En la administración de FS anuncios en snap\, reclamaciones proveedor confianzas son objetos de confianza normalmente creados en las organizaciones asociadas de recursos para representar la organización en la relación de confianza cuyas cuentas tendrán acceso a recursos de la organización de partner de recurso. Un objeto de confianza de proveedor de reclamaciones consta de una variedad de identificadores, nombres y las reglas que identifican a este asociado a los servicios de federación locales.|  
|Notificaciones locales proveedor confianza|Un objeto de confianza que representa AD LDS o third\ terceros LDAP\-based directorios en una granja de servidores de AD FS. Las notificaciones de una variable local confianza proveedor objeto consta de una variedad de identificadores, nombres y las reglas que identifican este directorio basado en LDAP\ a los servicios de federación locales.|  
|Metadatos de federación|El formato de datos para comunicar información de configuración entre un proveedor de notificaciones y un usuario de confianza para facilitar la configuración correcta de confianzas de proveedor de notificaciones y usuarios de confianza de terceros. El formato de datos se define en lenguaje de marcado de seguridad aserción \(SAML\) 2.0 y se extiende en WS\ federación.|  
|Servidor de federación|Un servidor de Windows que se ha configurado mediante el Asistente para configuración del servidor de federación de AD FS para que actúe en el rol de servidor de federación. Un servidor de federación emite tokens y sirve como parte de un servicio de federación.|  
|Proxy del servidor de federación|Un servidor de Windows que se ha configurado con el Asistente para configuración de Proxy de federación de servidor de AD FS para que actúe como el mantenimiento de un servidor proxy intermediario entre un cliente de Internet y un servicio de federación que se encuentra detrás de un firewall en una red corporativa.|  
|Servidor de federación principal|Un servidor de Windows que se haya configurado en el rol de servidor de federación mediante el Asistente para configuración del servidor de federación de AD FS y que tiene una copia de la base de datos de configuración de AD FS read\ y escritura. </br></br> El servidor de federación principal se crea al usar al Asistente para configuración del servidor de federación de AD FS y selecciona la opción de crear un nuevo servicio de federación y especificar el primer servidor de federación de que el equipo de la batería. Todos los otros servidores de federación de este conjunto deben replicar los cambios realizados en el servidor de federación principal para una copia solo read\ de la base de datos de configuración de AD FS que se almacena localmente. El término "servidor de federación principal" no se aplica cuando la base de datos de configuración de AD FS se almacena en una base de datos SQL como todos los servidores de federación igualmente pueden leer y escribir en una base de datos de configuración que se almacenan en un servidor SQL.|  
|Usuario de confianza|La organización que recibe y procesa reclamaciones. Consulta la organización de partner de recursos.|  
|Usa la confianza de terceros|En la administración de FS anuncios en snap\, confiar confianzas de terceros objetos de confianza normalmente se crean en:<br /><br />-Las organizaciones asociadas para representar la organización en la relación de confianza cuyas cuentas tendrán acceso a recursos de la organización de partner de recursos de la cuenta.<br />-Las organizaciones de partner resource para representar la confianza entre el servicio de federación y una sola aplicación web\.<br /><br />Un objeto de confianza de terceros confianza consta de una variedad de identificadores, nombres y las reglas que identifican este socio o web\ desde la aplicación a los servicios de federación locales.|  
|Servidor de federación de recursos|El servidor de federación de la organización de partner de recurso. Por lo general, el servidor de federación de recursos emite tokens de seguridad a los usuarios en función de un token de seguridad emitido por un servidor de federación de cuenta. El servidor recibe el token de seguridad, comprueba la firma, se aplica lógica de la regla de notificación a las notificaciones desempaquetadas para producir las notificaciones salientes deseadas, genera un nuevo token de seguridad \ (con la claims\ saliente) en función de información en el token de seguridad entrantes y firma el nuevo token para devolver al usuario y, finalmente, a la aplicación Web.|  
|Organización de partner de recursos|Un socio de federación está representado por una usuario de confianza confianza terceros en los servicios de federación de. El asociado de recurso emite tokens de seguridad basada en claims\ que contiene publicadas aplicaciones basadas en Web\ que pueden tener acceso los usuarios en el asociado de cuenta.|  
  
## <a name="overview-of-ad-fs"></a>Información general de AD FS  
AD FS es una solución de acceso de identidad que proporciona a los equipos cliente \ (interna o externa a tu red\) con acceso SSO transparente a protegido Internet\ aplicaciones o servicios, incluso cuando las cuentas de usuario y aplicaciones que se encuentran en redes completamente diferentes u organizaciones.  
  
Cuando una aplicación o servicio está en una red y una cuenta de usuario está en otra red, normalmente el usuario se pide secundario las credenciales cuando se intenta obtener acceso a la aplicación o servicio. Estas credenciales secundarias representan la identidad del usuario en el dominio donde se encuentra la aplicación o servicio. Por lo general son necesarias en el servidor Web que hospeda la aplicación o servicio para que puede tomar la decisión de autorización más adecuada.  
  
Con AD FS, las organizaciones pueden omitir las solicitudes de credenciales secundarias proporcionando confianza relaciones \(federation trusts\) que pueden usar estas organizaciones para proyectar los derechos digitales de identidades y acceso de un usuario a los asociados de confianza. En este entorno federada, cada organización sigue administrar sus propias identidades, pero cada organización segura también puede proyectar y Aceptar identidades de otras compañías.  
  
-   [El rol de tiendas de atributo](The-Role-of-Attribute-Stores.md)  
  
-   [La función de la base de datos de configuración de AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [La función de notificaciones](The-Role-of-Claims.md)  
  
-   [La función de reglas de notificación](The-Role-of-Claim-Rules.md)  
  
-   [El rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md)  
  
-   [La función de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md)  
  
-   [La función del lenguaje de regla de notificación](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinar el tipo de plantilla de regla de Reclamación de usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [¿Cómo se usan los URI en AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

