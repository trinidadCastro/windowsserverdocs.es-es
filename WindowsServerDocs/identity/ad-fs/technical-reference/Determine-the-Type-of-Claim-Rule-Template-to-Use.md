---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: "Determinar el tipo de plantilla de regla de Reclamación de usar"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 129cd83be4cd8302bd170ba87aad58c50f636006
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Determinar el tipo de plantilla de regla de Reclamación de usar

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Una parte importante del diseño de una infraestructura de servicios de federación de Active Directory \(AD FS\) consiste en determinar el conjunto completo de reglas de notificación, y qué correspondiente notificación plantillas de regla debes usar para crearlos, para cada partner que participará en federación con la organización. Crear reglas mediante el uso de plantillas de reglas de notificación en la administración de AD FS en snap\.  
  
Cada conjunto de reglas de notificación que se configuran solo puede asociarse con una confianza federada. Esto significa que no puedes crear un conjunto de reglas en una confianza y usarlos para otras confianzas en tu servicio de federación. En su lugar, puedes crear fácilmente las reglas de notificación de plantillas de regla para producir un conjunto de notificaciones que se acuerdan entre cada socio federado y la organización deseadas de más rápidamente.  
  
Para obtener más información sobre las reglas y las plantillas de regla, consulta [el rol de Reclamación reglas](The-Role-of-Claim-Rules.md).  
  
Antes de empezar a determinar los tipos de plantillas de notificación de regla que debes usar, ten en cuenta las siguientes preguntas:  
  
-   ¿Lo que dice se proporcionará por los proveedores de reclamaciones de confianza?  
  
-   ¿Qué notificaciones confía de cada proveedor de notificaciones?  
  
-   ¿Las notificaciones son necesarias para los usuarios de confianza que este servicio de federación de confianza?  
  
-   ¿Qué notificaciones están dispuesto a divulgar a cada usuario de confianza?  
  
-   ¿Los usuarios que deben tener acceso a cada usuario de confianza?  
  
Responder a estas preguntas se ayudarle a planear un sólido reclama diseño de la regla. También le ayudarán a crear una autorización suave y estrategia de control de acceso y aumentar la eficacia de tu equipo de distribución durante la implementación.  
  
En esta sección que aprenderás sobre el tipo de plantillas de regla para seleccionar para el entorno en función de tu empresa necesita.  
  
## <a name="claim-rule-template-types"></a>Los tipos de plantilla de regla de notificación  
La siguiente tabla describe todos los tipos de plantillas de regla de notificación que puedes usar para crear reglas mediante la administración de AD FS snap\ en, y las ventajas de usar una plantilla escribe sobre otra.  
  
|Tipo de plantilla de regla|Descripción|Ventajas|Desventajas de|  
|----------------------|---------------|--------------|-----------------|  
|Atravesar o filtrar una notificación entrante|Se usa para crear una regla que pasar por todos los valores de las notificaciones para un tipo de notificación seleccionado o filtrar según los valores de las notificaciones para que solo determinados valores de notificación para un tipo de notificación seleccionado pasará a través de notificaciones.<br /><br />Para obtener más información, consulta [cuándo usar un paso a través o una regla de Reclamación de filtro](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|: Puede usarse para seleccionar particular reclamaciones aceptación o emitido sin modificar|-No se puede cambiar el tipo y el valor de notificación|  
|Transformar una notificación entrante|Se usa para crear una regla que seleccione una notificación entrante y asigna a un tipo de notificación diferente o su valor de la notificación se asignan a un nuevo valor de la reclamación.<br /><br />Para obtener más información, consulta [cuándo usar una regla de Reclamación transformar](When-to-Use-a-Transform-Claim-Rule.md).|-Puede usarse para normalizar los valores o los tipos de notificación<br />-Reemplazar un sufijo e\ correo de una notificación entrante|-Reemplazos de cadena más complejos requieren una regla personalizada|  
|Enviar atributos LDAP como notificaciones|Se usa para crear una regla que se seleccione los atributos de un almacén de atributo LDAP enviar como notificaciones para el usuario de confianza.<br /><br />Para obtener más información, consulta [cuándo usar atributos LDAP enviar como reclamaciones regla](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Puede origen reclamaciones de cualquier almacén de atributo DS\ AD o AD LDS<br />-Se pueden emitir varias notificaciones con una sola regla|-Lento: como resultado de búsqueda de cuenta<br />: No se puede usar un filtro LDAP personalizado para realizar consultas|  
|Enviar la pertenencia al grupo como notificación|Se usa para crear una regla que puede enviar un valor y el tipo de notificación especificada cuando un usuario es miembro de un grupo de seguridad de Active Directory. Solo una reclamación solo se enviarán con esta regla, basada en el grupo que selecciones.<br /><br />Para obtener más información, consulta [cuándo usar una pertenencia al grupo de enviar como una regla de Reclamación](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Rendimiento rápido para emitir notificaciones de grupo: ninguna búsqueda de la cuenta|-El usuario debe ser miembro de un grupo local de Active Directory|  
|Enviar notificaciones usando una regla personalizada|Se usa para crear una regla personalizada que proporcionará opciones más avanzadas que una plantilla de regla estándar. Escriben reglas personalizadas con la de AD FS reclamar el idioma de la regla.<br /><br />Para obtener más información, consulta [cuándo usar una regla de notificación personalizada](When-to-Use-a-Custom-Claim-Rule.md).|-Puede usarse para tomar como origen reclamaciones de un almacén de atributo SQL<br />: Puede usarse para especificar un filtro personalizado de LDAP<br />: Puede usarse para emitir una p pid<br />-Se puede usar con un almacén de atributo personalizado<br />: Puede usarse para agregar notificaciones solo para el conjunto de solicitud de entrada<br />: Puede usarse para enviar notificaciones, en función de más de una notificación entrante|-Más difícil configurar \-algunos rampa el tiempo podría ser necesarias para obtener inicialmente del lenguaje de regla de notificación|  
|Permitir o denegar a los usuarios en función de una notificación entrante|Se usa para crear una regla de permitir o denegar el acceso a los usuarios a la parte de usuario de confianza, según el tipo y el valor de una notificación entrante.<br /><br />Para obtener más información, consulta [cuándo usar una regla de solicitud de autorización](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifica el proceso de autorización|-Requiere que el tipo de notificación solo una y otra reclamar valor especificarse<br />-No admite de coincidencia para valores de las notificaciones|  
|Permitir que todos los usuarios|Se usa para crear una regla que permitirá que todos los usuarios para acceder a la parte del usuario de confianza.<br /><br />Para obtener más información, consulta [cuándo usar una regla de solicitud de autorización](When-to-Use-an-Authorization-Claim-Rule.md).|-Fáciles de configurar|-Menos seguro que usar la permitir o denegar a los usuarios en función de una plantilla de notificación entrante|  
  

