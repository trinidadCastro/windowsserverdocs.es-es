---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: "La función de notificaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98765deaba67ffdc0ee18b6d8ef573e531d739cc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claims"></a>La función de notificaciones
En el modelo de identidad basada en claims\ reclamaciones desempeñan un papel esencial en el proceso de federación, son el componente clave por el que se determina el resultado de todas las solicitudes de autenticación y autorización basadas en Web\. Este modelo permite a las organizaciones proyectar de forma segura los derechos de uso y la identidad digital o *reclamaciones*, entre los límites de empresa en una forma estandarizada de y de seguridad.  
  
## <a name="what-are-claims"></a>¿Qué notificaciones?  
En su forma más simple, las notificaciones son simplemente *instrucciones* \ (por ejemplo, nombre, identidad, group\), acerca de los usuarios, que se usan principalmente para autorizar el acceso a aplicaciones claims\ ubicados en cualquier lugar en Internet. Cada instrucción corresponde a un *valor* que se almacena en la notificación.  
  
### <a name="how-claims-are-sourced"></a>¿Cómo se originan reclamaciones  
El servicio de federación de los servicios de federación de Active Directory \(AD FS\) define qué notificaciones se intercambian socios federados. Sin embargo, antes de hacer esto en primer lugar debes rellenar o la reclamación con un valor recuperado o un valor calculado de origen. Cada valor de la notificación representa un valor de un usuario, grupo o entidad y se originados en uno de dos maneras:  
  
1.  Cuando el valor que conforma la reclamación se recupera desde un almacén de atributo, por ejemplo, se recupera un valor de atributo del departamento de ventas de las propiedades de una cuenta de usuario de Active Directory. Para obtener más información, consulta [las funciones de atributo tiendas](The-Role-of-Attribute-Stores.md).  
  
2.  Cuando el valor de una notificación entrante se transforma en otro valor en función de la lógica expresada en una regla. Por ejemplo, cuando una notificación entrante con el valor de administradores de dominio se transforma en un nuevo valor de administradores antes de que se envía como una notificación saliente. Para obtener más información, consulta [el rol de Reclamación reglas](The-Role-of-Claim-Rules.md).  
  
Reclamaciones pueden incluir valores como una dirección de correo de e\, \(UPN\) nombre Principal de usuario, pertenencia al grupo y otros atributos de la cuenta.  
  
### <a name="how-claims-flow"></a>El fluyan de notificaciones  
Otras partes dependen de los valores de las notificaciones para realizar tareas de autorización para aplicaciones basadas en Web\ que alojan. Estas partes se conocen como *usuarios de confianza* en la administración de AD FS en snap\. El servicio de federación es responsable de establecer la confianza entre muchas partes diferentes. Se ha diseñado para procesar y el flujo de una reclamación de una organización que inicialmente fuentes de las notificaciones, también contempladas como el intercambio de confianza *dice proveedores* en la administración de AD FS snap\ en, un usuario de confianza. Un usuario de confianza, a continuación, usa estas notificaciones para tomar decisiones de autorización.  
  
El flujo de una reclamación con este proceso se conoce como la *canalización reclamaciones*. Hay tres pasos en el flujo de una reclamación a través de la canalización de notificaciones:  
  
1.  Las notificaciones que se reciben del proveedor de reclamaciones procesa las reglas de transformación de aceptación de la confianza del proveedor de notificaciones. Estas reglas determinan qué notificaciones se aceptan desde el proveedor de notificaciones.  
  
2.  El resultado de las reglas de transformación de aceptación se utiliza como entrada para las reglas de autorización de emisión. Estas reglas de determinan si el usuario está autorizado para acceder a la parte del usuario de confianza.  
  
3.  El resultado de las reglas de transformación de aceptación se usa como entrada para las reglas de transformación de emisión. Estas reglas determinan las notificaciones que se enviarán a la parte del usuario de confianza.  
  
Para obtener más información, consulta [el rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>Cómo se emiten notificaciones  
Cuando escribas reglas de notificación, el origen de las notificaciones entrantes para las reglas de notificación varía en función de si estás escribiendo las reglas en una relación de confianza del proveedor de notificaciones o una confianza de terceros de confianza. Cuando escribas reglas de notificación para una confianza de proveedor de notificaciones, las notificaciones entrantes son las notificaciones enviadas desde el proveedor de notificaciones de confianza para el servicio de federación. Cuando escribas reglas para una confianza de terceros de confianza, las notificaciones entrantes son las notificaciones que se envían por las reglas de notificación de la confianza del proveedor de reclamaciones aplicable. Para obtener más información acerca de las notificaciones entrantes y salientes, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md) y [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>¿Cuáles son los tipos de notificación?  
Un tipo de notificación proporciona contexto para el valor de notificación. Por lo general se expresa como una \(URI\) identificador uniforme de recursos. AD FS puede admitir cualquier tipo de notificación, y que esté configurado con los tipos de notificación en la siguiente tabla de manera predeterminada.  
  
|Nombre|Descripción|URI|  
|--------|---------------|-------|  
|Dirección de correo E\|La dirección de correo de e\ del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/EmailAddress|  
|Nombre|El nombre de usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenName|  
|Nombre|El nombre único del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/Name|  
|UPN|La entidad de seguridad de usuario nombre \(UPN\) del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/UPN|  
|Nombre común|El nombre común del usuario|http:///\/schemas.xmlsoap.org\/claims\/commonName|  
|Dirección de correo electrónico AD FS 1.x E\|La dirección de correo de e\ del usuario al interoperar con AD FS 1.1 o ADFS 1.0|http:///\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Grupo|Un grupo en el que el usuario es un miembro de|http:///\/schemas.xmlsoap.org\/claims\/Group|  
|AD FS 1.x UPN|El UPN de usuario al interoperar con AD FS 1.1 o ADFS 1.0|http:///\/schemas.xmlsoap.org\/claims\/UPN|  
|Función|Una función que el usuario tiene|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/Role|  
|Apellidos|El apellido del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/Surname|  
|P PID|El identificador privado del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificador del nombre|El identificador del nombre SAML del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/NameIdentifier|  
|Método de autenticación|El método que se usa para autenticar al usuario|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/AuthenticationMethod|  
|Denegar el SID del grupo solo|El grupo solo deny\ SID del usuario|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|Denegar a solo SID principal|El SID principal solo deny\ del usuario|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|Denegar solo principal SID del grupo|El grupo principal solo deny\ SID del usuario|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID del grupo|El SID del grupo del usuario|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/GroupSID|  
|SID del grupo principal|El SID del grupo principal del usuario|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID principal|El SID principal del usuario|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nombre de la cuenta de Windows|El nombre de cuenta de dominio del usuario en forma de<domain>\\<user>|http:///\/schemas.Microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>¿Cuáles son las descripciones de notificación?  
Descripciones de Reclamación representan una lista de tipos de notificaciones que AD FS admite y puede publicarse en los metadatos de federación. Los tipos de notificación mencionados en la tabla anterior se configuran como descripciones de notificaciones en la administración de AD FS en snap\.  
  
La colección de descripciones de notificación que se publicará en los metadatos de federación se almacena en la base de datos de configuración de AD FS. Estos reclama descripciones se usan varios componentes del servicio de federación.  
  
Descripción de cada notificación incluye un URI del tipo de notificación, el nombre, el estado de publicación y la descripción. Puede administrar la colección de descripción de la reclamación mediante la **reclamar descripciones** nodo en la administración de AD FS en snap\. Puedes modificar el estado de una descripción de la reclamación mediante el snap\ de publicación. Las siguientes opciones están disponibles:  
  
-   **Publicar esta notificación en los metadatos de federación como un tipo de notificación que puede aceptar este servicio de federación** \(Publish as Accepted\): indica los tipos de notificación que se aceptarán por este servicio de federación de otros proveedores de reclamaciones.  
  
-   **Publicar esta notificación en los metadatos de federación como un tipo de notificación que puede enviar este servicio de federación** \(Publish as Sent\): indica los tipos de notificación proporcionadas por los servicios de federación de este. Estos son los tipos de notificación de que los servicios de federación de publica a otros usuarios los está dispuesta a enviar. Los tipos de notificación real enviados por el proveedor de notificaciones con frecuencia son un subconjunto de esta lista.  
  
Para obtener más información acerca de cómo establecer el estado de publicación de un tipo de notificación, consulta [agregar una descripción reclamar](https://technet.microsoft.com/library/dd807051.aspx) en la Guía de implementación de AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Al generar metadatos de federación  
Metadatos de federación incluyen todas las descripciones de notificación que están marcadas para la publicación.  
  
### <a name="when-claims-rules-are-processed"></a>Cuando se procesan las reglas de notificaciones  
Cuando guardas información de configuración acerca de las descripciones de reclamaciones, resulta más sencillo para configurar las reglas acerca de las notificaciones. Para obtener más información sobre las reglas de notificación que se puede usar en la organización de proveedor de notificaciones, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md).  
  

