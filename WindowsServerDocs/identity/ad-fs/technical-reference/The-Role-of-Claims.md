---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: El papel de las notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 851a70bbed606530ca8292f65bc4f776eae77fae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407341"
---
# <a name="the-role-of-claims"></a>El papel de las notificaciones
En el modelo de identidad Claims @ no__t-0based, las notificaciones desempeñan un rol dinamizador en el proceso de Federación, son el componente clave por el que se determina el resultado de todas las solicitudes de autenticación y autorización de web @ no__t-1based. Con este modelo, las organizaciones pueden presentar identidades digitales y derechos (o *notificaciones*) en todo el ámbito empresarial sin ningún tipo de riesgo y de manera estándar.  
  
## <a name="what-are-claims"></a>¿Qué son las notificaciones?  
En su forma más sencilla, las notificaciones son simplemente *instrucciones* \(Para ejemplo, nombre, identidad, grupo @ no__t-2, que se usan principalmente para autorizar el acceso a las aplicaciones de notificaciones @ no__t-3based ubicadas en cualquier parte de Internet. Cada instrucción se corresponde con un *valor* que se almacena en la notificación.  
  
### <a name="how-claims-are-sourced"></a>Cómo se generan notificaciones  
El Servicio de federación en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 define qué notificaciones se intercambian entre los asociados federados. Para hacerlo, sin embargo, antes tiene que rellenar o generar la notificación con un valor recuperado o calculado. Cada valor de notificación representa un valor de un usuario, grupo o entidad, y se puede generar de dos formas:  
  
1.  Cuando el valor de que consta la notificación se recupera de un almacén de atributos, por ejemplo, cuando un valor de atributo del departamento de ventas se obtiene de las propiedades de una cuenta de usuario de Active Directory. Para obtener más información, consulte [The Role of Attribute Stores](The-Role-of-Attribute-Stores.md).  
  
2.  Cuando el valor de una notificación entrante se transforma en otro valor de acuerdo con la lógica expresada en una regla. Por ejemplo, cuando una notificación entrante con el valor de Admins. del dominio se transforma en un nuevo valor de Administradores antes de enviarse como notificación saliente. Para obtener más información, consulta [El papel de las reglas de notificaciones](The-Role-of-Claim-Rules.md).  
  
Las notificaciones pueden incluir valores como una dirección e @ no__t-0mail, el nombre principal de usuario \(UPN @ no__t-2, la pertenencia a grupos y otros atributos de cuenta.  
  
### <a name="how-claims-flow"></a>Flujo de las notificaciones  
Otras partes confían en los valores de las notificaciones para realizar tareas de autorización para las aplicaciones web @ no__t-0based que hospedan. A estas entidades se las conoce como *usuarios de confianza* en el complemento de administración de AD FS @ no__t-1in. El servicio de federación es responsable de negociar la confianza entre numerosos usuarios separados. Está diseñado para procesar y transmitir el intercambio de confianza de notificaciones de una organización que originalmente origina las notificaciones, también denominadas *proveedores de notificaciones* en el AD FS de administración de la no__t-1in, a un usuario de confianza. Luego, ese usuario de confianza emplea esas notificaciones para tomar decisiones de autorización.  
  
El flujo de notificaciones que tiene lugar en este proceso se conoce como la *canalización de notificaciones*. El flujo de notificaciones a través de la canalización de notificaciones se efectúa en tres pasos:  
  
1.  Las notificaciones que se reciben del proveedor de notificaciones se procesan mediante las reglas de transformación de aceptación en la confianza del proveedor de notificaciones. Estas reglas determinan qué notificaciones del proveedor de notificaciones se van a aceptar.  
  
2.  El resultado de las reglas de transformación de aceptación se usa como entrada en las reglas de autorización de emisión. Estas reglas determinan si el usuario va a poder acceder al usuario de confianza.  
  
3.  La salida de las reglas de transformación de aceptación se usa como entrada en las reglas de transformación. Estas reglas determinan qué notificaciones se van a enviar al usuario de confianza.  
  
Para obtener más información, consulta [El papel de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="how-claims-are-issued"></a>Cómo se emiten las notificaciones  
Al escribir reglas de notificaciones, el origen de las notificaciones entrantes de dichas reglas varía en función de si las reglas se están escribiendo en una confianza de proveedor de notificaciones o en una confianza para usuario autenticado. Si se escriben para una confianza de proveedor de notificaciones, las notificaciones entrantes son aquellas que se envían desde el proveedor de notificaciones de confianza al Servicio de federación. Si se escriben para una confianza para usuario autenticado, las notificaciones entrantes son aquellas que resultan de las reglas de notificaciones de la confianza de proveedor de notificaciones en cuestión. Para obtener más información acerca de las notificaciones entrantes y salientes, vea [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md) y [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-types"></a>¿Qué son los tipos de notificaciones?  
Un tipo de notificación ofrece contexto para el valor de la notificación Normalmente se expresa como un identificador uniforme de recursos \(URI @ no__t-1. AD FS puede admitir cualquier tipo de notificaciones y se configura con los tipos de notificaciones de la siguiente tabla de forma predeterminada.  
  
|Nombre|Descripción|URI|  
|--------|---------------|-------|  
|Dirección E @ no__t-0Mail|La dirección e @ no__t-0mail del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7emailaddress|  
|Nombre propio|Nombre propio del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7givenname|  
|Nombre|Nombre único del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7name|  
|UPN|El nombre principal de usuario \(UPN @ no__t-1 del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7upn|  
|Nombre común|Nombre común del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3CommonName|  
|AD FS 1. x E @ no__t-0Mail dirección|La dirección e @ no__t-0mail del usuario al interoperar con AD FS 1,1 o ADFS 1,0|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3EmailAddress|  
|Agrupar|Grupo al que pertenece el usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3Group|  
|UPN para AD FS 1.x|UPN del usuario al interoperar con AD FS 1.1 o AD FS 1.0|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2claims @ no__t-3UPN|  
|Rol|Rol que tiene el usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7role|  
|Apellido|Apellido del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7surname|  
|PPID|Identificador privado del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7privatepersonalidentifier|  
|Identificador de nombre|Identificador de nombre SAML del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7nameidentifier|  
|Método de autenticación|Método usado para autenticar al usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7authenticationmethod|  
|SID de grupo de solo denegación|El SID del grupo denegar @ no__t-0only del usuario|http: @no__t -0\/schemas.xmlsoap.org @ no__t-2ws @ no__t-32005 @ no__t-405 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlysid|  
|SID principal de solo denegación|El SID principal de denegación de @ no__t-0only del usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlyprimarysid|  
|SID de grupo principal de solo denegación|El SID del grupo primario deny @ no__t-0only del usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7denyonlyprimarygroupsid|  
|SID de grupo|SID de grupo del usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7groupsid|  
|SID de grupo principal|SID de grupo principal del usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7primarygroupsid|  
|SID principal|SID principal del usuario|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7primarysid|  
|Nombre de cuenta de Windows|El nombre de la cuenta de dominio del usuario con el formato \<domain @ no__t-1 @ no__t-2 @ no__t-3user @ no__t-4|http: @no__t -0\/schemas.microsoft.com @ no__t-2ws @ no__t-32008 @ no__t-406 @ no__t-5identity @ no__t-6claims @ no__t-7windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>¿Qué son las descripciones de notificaciones?  
Las descripciones de notificaciones representan una lista de tipos de notificaciones que AD FS admite y que se pueden publicar en metadatos de Federación. Los tipos de notificación mencionados en la tabla anterior se configuran como descripciones de notificaciones en el complemento de administración de AD FS @ no__t-0in.  
  
La colección de descripciones de notificaciones que se va a publicar en los metadatos de federación se almacena en la base de datos de configuración de AD FS. Estas descripciones están disponibles para su uso por parte de varios componentes del Servicio de federación.  
  
De igual modo, cada descripción incluye un URI, un nombre, un estado de publicación y una descripción del tipo de notificación en cuestión. Puede administrar la colección de descripciones de notificaciones mediante el nodo **descripciones de notificaciones** del complemento de administración de AD FS @ no__t-1in. Puede modificar el estado de publicación de una descripción de notificaciones mediante el ajuste @ no__t-0in. Las siguientes configuraciones están disponibles:  
  
-   **Publicar esta notificación en los metadatos de Federación como un tipo de notificación que este servicio de Federación puede aceptar** \(Publish como aceptado @ no__t-2: indica los tipos de notificación que este servicio de Federación va a aceptar de otros proveedores de notificaciones.  
  
-   **Publicar esta demanda en los metadatos de Federación como un tipo de demanda que este servicio de Federación puede enviar** \(Publish como se envía @ no__t-2: indica los tipos de notificaciones que ofrece este servicio de Federación. Se trata de los tipos de notificaciones que el Servicio de federación publica ante los demás como los tipos que quiere enviar. Los tipos de notificaciones reales que el proveedor de notificaciones envía son con frecuencia un subconjunto de esta lista.  
  
Para obtener más información sobre cómo establecer el estado de publicación de un tipo de demanda, consulte [adición de una descripción de notificaciones](https://technet.microsoft.com/library/dd807051.aspx) en la guía de implementación de AD FS.  
  
### <a name="when-generating-federation-metadata"></a>Cuándo generar metadatos de federación  
Los metadatos de federación incluyen todas las descripciones de notificaciones que se han marcado para publicarse.  
  
### <a name="when-claims-rules-are-processed"></a>Cuándo procesar reglas de notificaciones  
Si conservas información de configuración relativa a las descripciones de notificaciones, será más fácil configurar reglas sobre notificaciones. Para obtener más información sobre las reglas de notificaciones que se pueden usar en la organización del proveedor de notificaciones, consulta [El papel de las reglas de notificaciones](The-Role-of-Claim-Rules.md).  
  

