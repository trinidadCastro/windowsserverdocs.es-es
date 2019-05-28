---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: El papel de las notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 542d7a24e29b52dd3fa0d7ea6a9b2d27fb620d8d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188501"
---
# <a name="the-role-of-claims"></a>El papel de las notificaciones
En las notificaciones\-modelo de identidad basado en, las notificaciones desempeñan un papel fundamental en el proceso de federación, son el componente clave que el resultado de todas las aplicaciones Web\-se determinan en función de las solicitudes de autenticación y autorización. Con este modelo, las organizaciones pueden presentar identidades digitales y derechos (o *notificaciones*) en todo el ámbito empresarial sin ningún tipo de riesgo y de manera estándar.  
  
## <a name="what-are-claims"></a>¿Qué son las notificaciones?  
En su forma más simple, las notificaciones son simplemente *instrucciones* \(por ejemplo, nombre, identidad, grupo\), tomando sobre los usuarios, que se utilizan principalmente para autorizar el acceso a las reclamaciones\-aplicaciones basadas en ubicados en cualquier lugar en Internet. Cada instrucción se corresponde con un *valor* que se almacena en la notificación.  
  
### <a name="how-claims-are-sourced"></a>Cómo se generan notificaciones  
El servicio de federación de Active Directory Federation Services \(AD FS\) define las notificaciones que se intercambian entre los asociados federados. Para hacerlo, sin embargo, antes tiene que rellenar o generar la notificación con un valor recuperado o calculado. Cada valor de notificación representa un valor de un usuario, grupo o entidad, y se puede generar de dos formas:  
  
1.  Cuando el valor de que consta la notificación se recupera de un almacén de atributos, por ejemplo, cuando un valor de atributo del departamento de ventas se obtiene de las propiedades de una cuenta de usuario de Active Directory. Para obtener más información, consulte [The Role of Attribute Stores](The-Role-of-Attribute-Stores.md).  
  
2.  Cuando el valor de una notificación entrante se transforma en otro valor de acuerdo con la lógica expresada en una regla. Por ejemplo, cuando una notificación entrante con el valor de Admins. del dominio se transforma en un nuevo valor de Administradores antes de enviarse como notificación saliente. Para obtener más información, consulta [El papel de las reglas de notificaciones](The-Role-of-Claim-Rules.md).  
  
Las notificaciones pueden incluir valores como una e\-dirección, nombre Principal de usuario de correo \(UPN\), pertenencia a grupos y otros atributos de cuenta.  
  
### <a name="how-claims-flow"></a>Flujo de las notificaciones  
Otros usuarios se basan en los valores de las notificaciones para realizar tareas de autorización para Web\-en función de las aplicaciones que hospedan. Estas entidades se las conoce como *confianza* en el complemento Administración de AD FS\-en. El servicio de federación es responsable de negociar la confianza entre numerosos usuarios separados. Está diseñado para procesar y transmitir el intercambio de confianza de notificaciones desde una organización que genera inicialmente las notificaciones, también se denomina *proveedores de notificaciones* en el complemento Administración de AD FS\-en, a un usuario de confianza. Luego, ese usuario de confianza emplea esas notificaciones para tomar decisiones de autorización.  
  
El flujo de notificaciones que tiene lugar en este proceso se conoce como la *canalización de notificaciones*. El flujo de notificaciones a través de la canalización de notificaciones se efectúa en tres pasos:  
  
1.  Las notificaciones que se reciben del proveedor de notificaciones se procesan mediante las reglas de transformación de aceptación en la confianza del proveedor de notificaciones. Estas reglas determinan qué notificaciones del proveedor de notificaciones se van a aceptar.  
  
2.  El resultado de las reglas de transformación de aceptación se usa como entrada en las reglas de autorización de emisión. Estas reglas determinan si el usuario va a poder acceder al usuario de confianza.  
  
3.  La salida de las reglas de transformación de aceptación se usa como entrada en las reglas de transformación. Estas reglas determinan qué notificaciones se van a enviar al usuario de confianza.  
  
Para obtener más información, consulta [El papel de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="how-claims-are-issued"></a>Cómo se emiten las notificaciones  
Al escribir reglas de notificaciones, el origen de las notificaciones entrantes de dichas reglas varía en función de si las reglas se están escribiendo en una confianza de proveedor de notificaciones o en una confianza para usuario autenticado. Si se escriben para una confianza de proveedor de notificaciones, las notificaciones entrantes son aquellas que se envían desde el proveedor de notificaciones de confianza al Servicio de federación. Si se escriben para una confianza para usuario autenticado, las notificaciones entrantes son aquellas que resultan de las reglas de notificaciones de la confianza de proveedor de notificaciones en cuestión. Para obtener más información acerca de las notificaciones entrantes y salientes, vea [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md) y [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>¿Qué son los tipos de notificaciones?  
Un tipo de notificación ofrece contexto para el valor de la notificación Normalmente se expresa como un identificador uniforme de recursos \(URI\). AD FS puede admitir cualquier tipo de notificación y se configura de forma predeterminada con los tipos de notificación en la tabla siguiente.  
  
|Nombre|Descripción|URI|  
|--------|---------------|-------|  
|E\-dirección de correo|La e\-dirección del usuario de correo|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|Nombre propio|Nombre propio del usuario|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|Nombre|Nombre único del usuario|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|El nombre principal de usuario \(UPN\) del usuario|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|Nombre común|Nombre común del usuario|http:\/\/schemas.xmlsoap.org\/claims\/CommonName|  
|AD FS 1.x E\-dirección de correo|La e\-correo dirección del usuario al interoperar con AD FS 1.1 o AD FS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|Agrupar|Grupo al que pertenece el usuario|http:\/\/schemas.xmlsoap.org\/claims\/Group|  
|UPN para AD FS 1.x|UPN del usuario al interoperar con AD FS 1.1 o AD FS 1.0|http:\/\/schemas.xmlsoap.org\/claims\/UPN|  
|Rol|Rol que tiene el usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|Apellido|Apellido del usuario|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|Identificador privado del usuario|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|Identificador de nombre|Identificador de nombre SAML del usuario|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|Método de autenticación|Método usado para autenticar al usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identidad\/notificaciones\/authenticationmethod|  
|SID de grupo de solo denegación|Deny\-sólo el SID del usuario de grupo|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|SID principal de solo denegación|Deny\-sólo el SID principal del usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|SID de grupo principal de solo denegación|Deny\-solo SID de grupo principal del usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID de grupo|SID de grupo del usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|SID de grupo principal|SID de grupo principal del usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|SID principal|SID principal del usuario|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Nombre de cuenta de Windows|El nombre de cuenta de dominio del usuario en forma de \<dominio\>\\\<usuario\>|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>¿Qué son las descripciones de notificaciones?  
Descripciones de notificaciones representan una lista de tipos de notificaciones que admite AD FS y que se pueden publicar en metadatos de federación. Los tipos de notificación que se mencionan en la tabla anterior se configuran como descripciones de notificaciones en el complemento Administración de AD FS\-en.  
  
La colección de descripciones de notificaciones que se va a publicar en los metadatos de federación se almacena en la base de datos de configuración de AD FS. Estas descripciones están disponibles para su uso por parte de varios componentes del Servicio de federación.  
  
De igual modo, cada descripción incluye un URI, un nombre, un estado de publicación y una descripción del tipo de notificación en cuestión. Puede administrar la colección de descripciones de notificaciones mediante el **descripciones de notificaciones** nodo en el complemento Administración de AD FS\-en. Puede modificar el estado de publicación de una descripción de notificación mediante el complemento\-en. Las siguientes configuraciones están disponibles:  
  
-   **Publicar esta notificación en los metadatos de federación como un tipo de notificación que este servicio de federación puede aceptar** \(publicar como aceptado\): indica los tipos de notificación que se va a Aceptar de otros proveedores de notificaciones por esta federación Servicio.  
  
-   **Publicar esta notificación en los metadatos de federación como un tipo de notificación que este servicio de federación puede enviar** \(publicar como enviado\): indica los tipos de notificación que ofrecen este servicio de federación. Se trata de los tipos de notificaciones que el Servicio de federación publica ante los demás como los tipos que quiere enviar. Los tipos de notificaciones reales que el proveedor de notificaciones envía son con frecuencia un subconjunto de esta lista.  
  
Para obtener más información sobre cómo establecer el estado de publicación de un tipo de notificación, consulte [Add a Claim Description](https://technet.microsoft.com/library/dd807051.aspx) en la Guía de implementación de AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Cuándo generar metadatos de federación  
Los metadatos de federación incluyen todas las descripciones de notificaciones que se han marcado para publicarse.  
  
### <a name="when-claims-rules-are-processed"></a>Cuándo procesar reglas de notificaciones  
Si conservas información de configuración relativa a las descripciones de notificaciones, será más fácil configurar reglas sobre notificaciones. Para obtener más información sobre las reglas de notificaciones que se pueden usar en la organización del proveedor de notificaciones, consulta [El papel de las reglas de notificaciones](The-Role-of-Claim-Rules.md).  
  

