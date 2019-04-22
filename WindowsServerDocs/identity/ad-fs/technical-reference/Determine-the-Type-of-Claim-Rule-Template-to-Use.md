---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Establecer qué tipo de plantilla de regla de notificación usar
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 129cd83be4cd8302bd170ba87aad58c50f636006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815856"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Establecer qué tipo de plantilla de regla de notificación usar

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Una parte importante del diseño de un Active Directory Federation Services \(AD FS\) infraestructura es determinar el conjunto completo de reglas de notificación, y qué correspondiente de notificación de las plantillas de reglas que se debe usar para crearlas, para cada socio que participará en la federación con su organización. Crear reglas mediante el uso de plantillas de regla de notificación en el complemento Administración de AD FS\-en.  
  
Cada conjunto de reglas de notificación que configures solo se puede asociar con una confianza federada. Esto significa que no puedes crear un conjunto de reglas en una relación de confianza y usarlas para otras relaciones de confianza de tu Servicio de federación. En su lugar, puedes crear reglas fácilmente a partir de plantillas de reglas de notificación para ayudar a producir rápidamente un conjunto deseado de notificaciones acordadas entre cada socio federado y tu empresa.  
  
Para obtener más información sobre las reglas y las plantillas de reglas, consulta [The Role of Claim Rules](The-Role-of-Claim-Rules.md).  
  
Antes de empezar a determinar los tipos de plantillas de regla de notificación que debes usar, ten en cuenta las preguntas siguientes:  
  
-   ¿Qué notificaciones proporcionarán tus proveedores de notificaciones de confianza?  
  
-   ¿En qué notificaciones de cada proveedor de notificaciones confías?  
  
-   ¿Qué notificaciones requieren los usuarios de confianza que confían en este Servicio de federación?  
  
-   ¿Qué notificaciones estás dispuesto a divulgar a cada usuario de confianza?  
  
-   ¿Qué usuarios deben tener acceso a cada usuario de confianza?  
  
Responder a estas preguntas te ayudará a planificar un diseño de reglas de notificación sólido. También te ayudará a crear una estrategia de autorización y de control de acceso suavizada y a hacer que el equipo de implementación sea más eficaz durante la implementación.  
  
En la sección siguiente se da información sobre qué tipo de plantillas de regla seleccionar para tu entorno en función de las necesidades de tu empresa.  
  
## <a name="claim-rule-template-types"></a>Tipos de plantilla de regla de notificación  
La tabla siguiente describen todos los tipos de plantillas de regla de notificación que puede usar para crear reglas mediante el complemento Administración de AD FS\-en y las ventajas de usar un tipo de plantilla frente a otro.  
  
|Tipo de plantilla de notificación|Descripción|Ventajas|Desventajas|  
|----------------------|---------------|--------------|-----------------|  
|Pasar o filtrar una notificación entrante|Se usa para crear una regla que pasará a través todos los valores de notificación para un tipo de notificación determinado o notificaciones de filtros basadas en los valores de notificación para que solo ciertos valores de notificación para un tipo de notificación determinado pasen a través.<br /><br />Para obtener más información, consulte [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Puede utilizarse para seleccionar notificaciones determinadas para que se acepten o se emitan sin cambios|-No se puede cambiar el tipo y el valor de notificación|  
|Transformar una notificación entrante|Se usa para crear una regla que pueda seleccionar una notificación entrante y asignarla a un tipo de notificación diferente o asignar su valor de notificación a un nuevo valor de notificación.<br /><br />Para obtener más información, consulte [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Puede utilizarse para normalizar los valores o tipos de notificación<br />-Puede sustituir una e\-sufijo de correo electrónico de una notificación entrante|-Reemplazos de cadena más complejos requieren una regla personalizada|  
|Enviar atributos LDAP como notificaciones|Se usa para crear una regla que seleccionará los atributos de un almacén de atributos LDAP para enviarlos como notificaciones al usuario de confianza.<br /><br />Para obtener más información, consulte [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Puede obtener notificaciones de cualquier AD DS\/almacén de atributos de AD LDS<br />-Varias notificaciones se pueden emitir mediante una sola regla|-Rendimiento: lento, como resultado de búsqueda de la cuenta<br />-No se puede usar un filtro LDAP personalizado para realizar consultas|  
|Enviar la pertenencia a grupo como una notificación|Se usar para crear una regla que pueda enviar un tipo y un valor de notificación determinados cuando un usuario es miembro de un grupo de seguridad de Active Directory. Con esta regla solo se enviará una única notificación, según el grupo que selecciones.<br /><br />Para obtener más información, consulte [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Rendimiento rápido para emitir notificaciones de grupo: ninguna búsqueda de cuenta|-El usuario debe ser miembro de un grupo local de Active Directory|  
|Enviar notificaciones mediante una regla personalizada|Se usa para crear una regla personalizada que proporcionará más opciones avanzadas de una plantilla de regla estándar. Escribe reglas personalizadas con AD FS de lenguaje de reglas de notificación.<br /><br />Para obtener más información, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).|-Puede utilizarse para obtener notificaciones desde un almacén de atributos SQL<br />-Puede utilizarse para especificar un filtro LDAP personalizado<br />-Puede utilizarse para emitir un PPID<br />-Puede utilizarse con un almacén de atributos personalizados<br />-Puede utilizarse para agregar notificaciones solo al conjunto de notificaciones de entrada<br />-Puede utilizarse para enviar notificaciones basadas en más de una notificación entrante|-Más difícil de configurar \- puede resultar necesario algún tiempo de despegue para inicialmente Obtenga información sobre el lenguaje de reglas de notificación|  
|Permitir o denegar a los usuarios en función una notificación entrante|Se usa para crear una regla que permitirá o denegará a los usuarios el acceso al usuario de confianza, según el tipo y el valor de una notificación entrante.<br /><br />Para obtener más información, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifica el proceso de autorización|-Requiere que solo un tipo y un valor de notificación de especificarse<br />-No admite coincidencia de patrones para los valores de notificación|  
|Permitir a todos los usuarios|Se usa para crear una regla que permitirá a todos los usuarios tener acceso al usuario de confianza.<br /><br />Para obtener más información, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Fácil de configurar|-Menos seguro que el uso del permiso o denegar a los usuarios basados en una plantilla de notificación entrante|  
  

