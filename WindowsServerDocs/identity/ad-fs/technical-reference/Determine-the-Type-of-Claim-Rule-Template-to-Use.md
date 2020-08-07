---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: Establecer qué tipo de plantilla de regla de notificación usar
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: bbc73c48fa3c1dd4bb743d5c088fdd94d722d5a1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938026"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>Establecer qué tipo de plantilla de regla de notificación usar


Una parte importante del diseño de una \( infraestructura de AD FS de servicios de Federación de Active Directory (AD FS) \) es determinar el conjunto completo de reglas de notificaciones, y qué plantillas de reglas de notificaciones correspondientes debe usar para crearlas, para cada socio que participará en la Federación con su organización. Las reglas se crean mediante plantillas de reglas de notificaciones en el complemento \- de administración de AD FS en.

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
En la tabla siguiente se describen todos los tipos de plantillas de reglas de notificaciones que puede usar para crear reglas mediante el complemento \- de administración de AD FS en y las ventajas de usar un tipo de plantilla sobre otro.

|Tipo de plantilla de notificación|Descripción|Ventajas|Desventajas|
|----------------------|---------------|--------------|-----------------|
|Pasar o filtrar una notificación entrante|Se usa para crear una regla que pasará a través todos los valores de notificación para un tipo de notificación determinado o notificaciones de filtros basadas en los valores de notificación para que solo ciertos valores de notificación para un tipo de notificación determinado pasen a través.<p>Para obtener más información, consulte [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md).|-Se puede usar para seleccionar notificaciones determinadas para que se acepten o se emitan sin cambios|-No se puede cambiar el tipo y el valor de notificaciones|
|Transformar una notificación entrante|Se usa para crear una regla que pueda seleccionar una notificación entrante y asignarla a un tipo de notificación diferente o asignar su valor de notificación a un nuevo valor de notificación.<p>Para obtener más información, consulte [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).|-Se puede usar para normalizar los valores o tipos de notificaciones<br />-Puede reemplazar un \- sufijo de correo electrónico de una demanda entrante|-Las sustituciones de cadenas más complejas requieren una regla personalizada|
|Enviar atributos LDAP como notificaciones|Se usa para crear una regla que seleccionará los atributos de un almacén de atributos LDAP para enviarlos como notificaciones al usuario de confianza.<p>Para obtener más información, consulte [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md).|-Puede ser notificaciones de origen de cualquier \/ almacén de atributos AD LDS AD DS<br />-Se pueden emitir varias notificaciones con una sola regla|-Rendimiento: lento como resultado de la búsqueda de cuentas<br />-No se puede usar un filtro personalizado de LDAP para realizar consultas|
|Enviar la pertenencia a grupo como una notificación|Se usar para crear una regla que pueda enviar un tipo y un valor de notificación determinados cuando un usuario es miembro de un grupo de seguridad de Active Directory. Con esta regla solo se enviará una única notificación, según el grupo que selecciones.<p>Para obtener más información, consulte [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md).|-Rendimiento rápido para emitir notificaciones de Grupo: ninguna búsqueda de cuenta|-El usuario debe ser miembro de un grupo de Active Directory local|
|Enviar notificaciones mediante una regla personalizada|Se usa para crear una regla personalizada que proporcionará más opciones avanzadas de una plantilla de regla estándar. Las reglas personalizadas se escriben con el lenguaje de reglas de notificaciones de AD FS.<p>Para obtener más información, consulte [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).|-Se puede usar para el origen de notificaciones de un almacén de atributos de SQL<br />-Se puede usar para especificar un filtro LDAP personalizado.<br />-Se puede usar para emitir un PPID<br />-Se puede usar con un almacén de atributos personalizado<br />-Se puede usar para agregar notificaciones solo al conjunto de notificaciones de entrada.<br />-Se puede usar para enviar notificaciones basadas en más de una notificación entrante.|-Es posible que sea \- necesario configurar un tiempo de espera para obtener un conocimiento inicial del lenguaje de reglas de notificaciones.|
|Permitir o denegar a los usuarios en función una notificación entrante|Se usa para crear una regla que permitirá o denegará a los usuarios el acceso al usuario de confianza, según el tipo y el valor de una notificación entrante.<p>Para obtener más información, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Simplifica el proceso de autorización|: Requiere que solo se especifique un tipo de notificaciones y un valor de notificaciones.<br />-No admite la coincidencia de patrones para los valores de notificaciones|
|Permitir a todos los usuarios|Se usa para crear una regla que permitirá a todos los usuarios tener acceso al usuario de confianza.<p>Para obtener más información, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).|-Fácil de configurar|-Menos seguro que el uso de permitir o denegar a los usuarios en función de una plantilla de notificaciones entrante|


