---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: "La función de reglas de notificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e47dbecdeee620d3a237ad8d8c41a550d3ef069c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-claim-rules"></a>La función de reglas de notificación
La función general de los servicios de federación de los servicios de federación de Active Directory \(AD FS\) es emitir un token que contiene un conjunto de notificaciones. La decisión sobre qué notificaciones AD FS acepta y, a continuación, emite se rige por las reglas de notificación.  
  
## <a name="what-are-claim-rules"></a>¿Cuáles son las reglas de notificación?  
Una regla de Reclamación representa una instancia de la lógica de negocios que va a realizar una o más notificaciones entrantes, aplicar condiciones a ellos \ (si está x después y\) y generar notificaciones salientes basados en los parámetros de la condición. Para obtener más información acerca de las notificaciones entrantes y salientes, consulta [el rol de reclamaciones](The-Role-of-Claims.md).  
  
Usar reglas de notificación cuando se necesita implementar la lógica de negocios que va a controlar el flujo de una reclamación a través de la canalización de reclamaciones. Mientras la canalización de notificaciones es más lógico concepto de notificaciones el proceso de end\-to\-end para hacer fluir reglas de notificación son un elemento administrativo real que puedes usar para personalizar el flujo de una reclamación a través del proceso de emisión de reclamaciones.  
  
Para obtener más información acerca de la canalización de notificaciones, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md).  
  
Reglas de notificación proporcionan las siguientes ventajas:  
  
-   Proporcionar un mecanismo para los administradores a aplicar la lógica de negocios run\ tiempo para que confía reclamaciones de proveedores de notificaciones  
  
-   Proporcionar un mecanismo para que los administradores definir qué notificaciones se publican a los usuarios de confianza  
  
-   Proporcionar funcionalidades detallada y sofisticada claims\ de autorización a los administradores que deseen para permitir o denegar el acceso a usuarios específicos  
  
### <a name="how-claim-rules-are-processed"></a>Cómo se procesan las reglas de notificación  
Reglas de notificación se procesan a través de la canalización de notificaciones con la *motor reclamaciones*. El motor de notificaciones es un componente lógico de los servicios de federación que examina el conjunto de notificaciones entrantes presentado por un usuario y, a continuación, en función de la lógica en cada regla, se produce un conjunto de resultados de una reclamación.  
  
Juntos, las notificaciones de regla motor y el conjunto de reglas asociadas a una determinada confianza federada determinan si notificaciones entrantes deben pasan son, filtrados para cumplir con los criterios de la condición específica o transforma en un nuevo conjunto de notificaciones antes de que se emiten como notificaciones salientes por el servicio de federación de notificación.  
  
Para obtener más información sobre este proceso, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>¿Cuáles son las plantillas de reglas de notificación?  
AD FS incluye un conjunto predefinido de regla de notificación de plantillas que están diseñadas para que resulte fácil seleccionar y crean las reglas de notificación más adecuadas para la necesidad de negocio. Reclama regla plantillas solo se usan durante el proceso de creación de reglas de notificación.  
  
En la administración de FS anuncios en snap\, reglas sólo pueden crearse mediante plantillas de regla de notificación. Una vez que usas en snap\ para seleccionar una plantilla de regla de notificación, los datos necesarios para la lógica de la regla de entrada y guardarlos en la base de datos de configuración, estará \ (desde ese forward\ punto) contempladas en la interfaz de usuario como una regla de notificación.  
  
### <a name="how-claim-rule-templates-work"></a>Cómo solicitar trabajo de plantillas de regla  
A primera vista, plantillas de regla de Reclamación parecen ser formularios de entrada solo proporcionados por snap\ para recopilar datos y procesos lógica específica de notificaciones entrantes. Sin embargo, en un nivel mucho más detallado, reclamar el almacén de plantillas de reglas marco de lenguaje de regla que conforman la lógica de base necesarios crear rápidamente una regla sin tener que saber el idioma muy de notificación de la necesaria.  
  
Cada plantilla que se proporciona en la interfaz de usuario \(UI\) representa una sintaxis de lenguaje de la regla de Reclamación previamente, en función de las tareas administrativas requeridas con más frecuencia. Hay una plantilla de regla sin embargo, es decir, la excepción. Esta plantilla se conoce como la plantilla de regla personalizada. Con esta plantilla, sintaxis no se rellenaron previamente. En su lugar, debe modificar directamente la sintaxis del lenguaje de regla de notificación en el cuerpo de la forma de plantilla de regla de reclamación mediante la sintaxis de lenguaje de regla de notificación.  
  
Para obtener más información sobre cómo usar la sintaxis de lenguaje de regla de notificación, consulta [el rol del lenguaje de regla reclamar](The-Role-of-the-Claim-Rule-Language.md) en la Guía de implementación de AD FS.  
  
> [!TIP]  
> Puedes ver el idioma de la regla de Reclamación asociado con una regla en cualquier momento haciendo clic en el **Ver regla idioma** botón en las propiedades de una regla de notificación.  
  
### <a name="how-to-create-a-claim-rule"></a>Cómo crear una regla de notificación  
Reglas de notificación se crean por separado para cada relación de confianza federada dentro de los servicios de federación de y no se comparten entre varias relaciones de confianza. Puede crear una regla de una plantilla de regla de reclamación, empezar desde cero creando la regla usando el lenguaje de regla de reclamación o usar Windows PowerShell para personalizar una regla.  
  
Todas estas opciones coexistan para proporcionarte la flexibilidad de elegir el método adecuado para un escenario dado. Para obtener más información sobre cómo crear una regla de notificación, consulta [configurar reglas reclamar](https://technet.microsoft.com/library/ee913571.aspx) en la Guía de FSDeployment anuncios.  
  
#### <a name="using-claim-rule-templates"></a>Uso de plantillas de notificación de regla  
Reclama regla plantillas solo se usan durante el proceso de creación de reglas de notificación. Puedes usar cualquiera de las siguientes plantillas para crear una regla de notificación:  
  
-   Atravesar o filtrar una notificación entrante  
  
-   Transformar una notificación entrante  
  
-   Enviar atributos LDAP como notificaciones  
  
-   Enviar la pertenencia al grupo como notificación  
  
-   Enviar notificaciones usando una regla personalizada  
  
-   Permitir o denegar a los usuarios en función de una notificación entrante  
  
-   Permitir que todos los usuarios  
  
Para obtener más información que describe cada una de estas plantillas de reglas de notificación, consulta [determinar el tipo de notificación regla plantilla uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
Reglas de negocio que están fuera del ámbito de plantillas de regla de reclamación estándar, puedes usar una plantilla de regla personalizada para expresar una serie de condiciones de una lógica compleja con el lenguaje de regla de notificación. Para obtener más información sobre cómo usar una regla personalizada, consulta [cuándo usar una regla personalizada de Reclamación](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windows-powershell"></a>Uso de Windows PowerShell  
También puedes usar el objeto de cmdlet ADFSClaimRuleSet con Windows PowerShell para crear o administrar reglas en AD FS. Para obtener más información acerca de cómo puedes usar Windows PowerShell con este cmdlet, consulta [AD FS administración con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>¿Qué es un conjunto de reglas de notificación?  
Como se muestra en la siguiente ilustración, un conjunto de reglas de notificación es una agrupación de una o varias reglas para una determinada confianza federada que vas a definir cómo se procesará reclamaciones por el motor de reglas de reclamaciones. Cuando se recibe una notificación entrante en el servicio de federación el motor de reglas de Reclamación aplica la lógica de especificado por el conjunto de reglas de notificación adecuado. Es la suma final de la lógica de cada regla en el conjunto de que se determina cómo se emitirán notificaciones para una determinado confianza en su totalidad.  
  
![Roles de AD FS](media/adfs2_claimruleset.gif)  
  
Reglas de notificación se procesan el motor de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Este orden es importante, porque el resultado de una regla que puede usarse como la entrada a la siguiente regla del conjunto.  
  
## <a name="what-are-claim-rule-set-types"></a>¿Cuáles son las reglas de notificación configura tipos?  
Una regla de Reclamación establecida tipo es un segmento lógico de una confianza federada que clasificada por categorías identifica si el conjunto de reglas de Reclamación asociado con la relación de confianza se usará para la emisión de reclamaciones, aceptación o autorización. Cada confianza federada puede tener uno o más de Reclamación de regla establece tipos asociados, según el tipo de confianza que se usa.  
  
La siguiente tabla describe los distintos tipos de conjuntos de reglas de notificación y explica a su relación con un usuario de confianza de terceros de confianza o de confianza de proveedor de notificaciones.  
  
|Tipo de conjunto de reglas de notificación|Descripción|Usado en|  
|-----------------------|---------------|-----------|  
|Conjunto de reglas de transformación de aceptación|Un conjunto de reglas de notificación que usas en un determinado notificaciones de confianza de proveedor para especificar las notificaciones entrantes que se aceptarán de la organización de proveedor de notificaciones y las notificaciones salientes que se enviarán a la confianza de terceros de confianza.<br /><br />Las notificaciones entrantes que se usará para tomar como origen de este conjunto de reglas, serán las notificaciones salida por la regla de transformación de emisión establecida como se especifica en la organización de proveedor de notificaciones.<br /><br />De manera predeterminada, el nodo de confianza del proveedor de reclamaciones contiene una confianza de proveedor de Reclamación denominada **Active Directory** que se usa para representar el almacén de atributo de origen para el conjunto de reglas de transformación de aceptación. Este objeto de confianza se usa para representar la conexión de los servicios de federación de a una base de datos de Active Directory de la red. Esta confianza predeterminado es lo que procesa notificaciones para los usuarios que han sido autenticados Active Directory y no se puede eliminar.|Relaciones de confianza de proveedor de notificaciones|  
|Conjunto de reglas de transformación de emisión|Un conjunto de reglas de notificación que se usan en una confianza de terceros de confianza para especificar las notificaciones que se emitirá al usuario de confianza.<br /><br />Las notificaciones entrantes que se usará para tomar como origen de este conjunto de reglas, será inicialmente las notificaciones que obtiene las reglas de transformación de aceptación.|Confiar confianzas de terceros|  
|Conjunto de reglas de autorización de emisión|Un conjunto de reglas de notificación que usas en una confianza de terceros de confianza para especificar los usuarios que pueden recibir un token para el usuario de confianza.<br /><br />Estas reglas determinar si un usuario puede recibir notificaciones para un usuario de confianza y, por lo tanto, acceso a la parte del usuario de confianza.<br /><br />A menos que especifiques una regla de autorización de emisión, todos los usuarios se denegará el acceso de manera predeterminada.|Confiar confianzas de terceros|  
|Conjunto de reglas de autorización de delegación|Un conjunto de reglas de notificación que usas en una confianza de terceros de confianza para especificar los usuarios que pueden actuar como delegados de otros usuarios para que el usuario de confianza.<br /><br />Estas reglas determinan si se permite que el solicitante suplantar a un usuario mientras se sigue identifican a solicitante en el token que se envía al usuario de confianza.<br /><br />A menos que especifiques una regla de autorización de emisión, ningún usuario puede actuar como delegados de manera predeterminada.|Confiar confianzas de terceros|  
|Conjunto de reglas de autorización de representación|Un conjunto de reglas que se configuran mediante Windows PowerShell para determinar si un usuario pueden totalmente de Reclamación suplantar a otro usuario para el usuario de confianza.<br /><br />Estas reglas determinan si se permite que el solicitante suplantar a un usuario sin identificar al solicitante en el token que se envía al usuario de confianza.<br /><br />Suplantan la identidad de otro usuario de esta forma es una funcionalidad muy eficaz, porque el usuario de confianza no sabrá que el usuario se suplanta.|Usa la confianza de terceros|  
  
Para obtener más información sobre cómo seleccionar las reglas de notificación adecuado para usar en tu organización, consulta [determinar el tipo de notificación regla plantilla uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

