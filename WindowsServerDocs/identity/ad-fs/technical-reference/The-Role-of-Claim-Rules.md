---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: El papel de las reglas de notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a1e324724f6e56782633357630c26417e0b4c4f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860188"
---
# <a name="the-role-of-claim-rules"></a>El papel de las reglas de notificaciones
La función global del Servicio de federación en Servicios de federación de Active Directory (AD FS) \(AD FS\) es emitir un token que contenga un conjunto de notificaciones. La decisión con respecto a las notificaciones que AD FS acepta y luego emite se rige por las reglas de notificación.

## <a name="what-are-claim-rules"></a>¿Qué son las reglas de notificación?
Una regla de notificación representa una instancia de la lógica de negocios que tomará una o varias notificaciones entrantes, les aplicará condiciones \(si x, entonces y\) y generará una o varias notificaciones salientes según los parámetros de la condición. Para obtener más información sobre las notificaciones entrantes y salientes, vea [el rol de las notificaciones](The-Role-of-Claims.md).

Usará reglas de notificación cuando necesite implementar lógica de negocios que controle el flujo de notificaciones a través de la canalización de notificaciones. Aunque la canalización de notificaciones es más un concepto lógico del extremo\-a\-proceso final para el flujo de notificaciones, las reglas de notificación son un elemento administrativo real que puede usar para personalizar el flujo de notificaciones a través del proceso de emisión de notificaciones.

Para obtener más información sobre la canalización de notificaciones, consulte [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md).

Las reglas de notificación ofrecen las siguientes ventajas:

-   Proporcionar un mecanismo para que los administradores apliquen lógica de negocios de ejecución\-tiempo para las notificaciones de confianza de los proveedores de notificaciones

-   Proporcionan un mecanismo para que los administradores definan las notificaciones que se publicarán para los usuarios autenticados.

-   Proporcionan capacidades de autorización basadas en\-de notificaciones enriquecidas y detalladas a los administradores que desean permitir o denegar el acceso a usuarios específicos.

### <a name="how-claim-rules-are-processed"></a>Cómo se procesan las reglas de notificación
Las reglas de notificación se procesan a través de la canalización de notificaciones con el *motor de notificaciones*. El motor de notificaciones es un componente lógico del Servicio de federación que examina el conjunto de notificaciones entrantes que presenta un usuario y, después, según la lógica de cada regla, generará un conjunto de resultados de notificaciones.

Juntos, el motor de reglas de notificaciones y el conjunto de reglas de notificación asociadas a una confianza federada determinada determinan si las notificaciones entrantes deben pasarse tal como están, filtradas para cumplir los criterios de una condición específica o transformarse en un conjunto completamente nuevo de notificaciones antes de que las emita el Servicio de federación como notificaciones salientes.

Para obtener más información sobre este proceso, vea [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md).

## <a name="what-are-claim-rule-templates"></a>¿Qué son las plantillas de reglas de notificación?
AD FS incluye un conjunto predefinido de plantillas de reglas de notificaciones que están diseñadas para ayudarle a seleccionar y crear fácilmente las reglas de notificaciones más adecuadas para sus necesidades empresariales concretas. Las plantillas de reglas de notificaciones solo se usan durante el proceso de creación de reglas de notificación.

En el\-de administración de AD FS, en, las reglas solo se pueden crear mediante plantillas de reglas de notificaciones. Después de usar el ajuste\-en para seleccionar una plantilla de regla de notificaciones, escriba los datos necesarios para la lógica de la regla y guárdelos en la base de datos de configuración, se \(desde ese punto hacia delante\) a la que se hace referencia en la interfaz de usuario como una regla de notificaciones.

### <a name="how-claim-rule-templates-work"></a>Cómo funcionan las plantillas de reglas de notificación
A primera vista, las plantillas de reglas de notificación parecen ser simplemente formularios de entrada proporcionados por el complemento\-en para recopilar datos y procesar lógica específica de las notificaciones entrantes. Sin embargo, en un nivel mucho más detallado, las plantillas de reglas de notificación almacenan el marco del lenguaje de reglas de notificación necesario que constituye la lógica de base necesaria para que pueda crear rápidamente una regla sin necesidad de conocer el lenguaje en detalle.

Cada plantilla que se proporciona en la interfaz de usuario \(interfaz de usuario\) representa una sintaxis del lenguaje de reglas de notificaciones rellenada previamente, basándose en las tareas administrativas que se requieren con más frecuencia. Sin embargo, existe una plantilla de reglas que es la excepción. Esta plantilla se conoce como la plantilla de reglas personalizada. Con esta plantilla, no se rellena previamente ninguna sintaxis. En su lugar, debe crear directamente la sintaxis del lenguaje de reglas de notificación en el cuerpo del formulario de la plantilla de reglas de notificación mediante la sintaxis del lenguaje de reglas de notificación.

Para obtener más información sobre cómo usar la sintaxis del lenguaje de reglas de notificaciones, vea [el rol del lenguaje de reglas de notificaciones](The-Role-of-the-Claim-Rule-Language.md) en la guía de implementación de AD FS.

> [!TIP]
> Puede ver el lenguaje de reglas de notificación asociado a una regla en cualquier momento si hace clic en el botón **Ver lenguaje de la regla** de las propiedades de una regla de notificación.

### <a name="how-to-create-a-claim-rule"></a>Cómo crear una regla de notificación
Las reglas de notificación se crean por separado para cada relación de confianza federada dentro del Servicio de federación y no se comparten entre varias relaciones de confianza. Puede crear una regla desde una plantilla de regla de notificación, empezar desde cero mediante la creación de la regla con el lenguaje de reglas de notificación o usar Windows PowerShell para personalizar una regla.

Todas estas opciones coexisten para ofrecerle la flexibilidad de elegir el método adecuado para un escenario determinado. Para obtener más información sobre cómo crear una regla de notificaciones, consulte [configuración de reglas de notificaciones](https://technet.microsoft.com/library/ee913571.aspx) en la guía de ad FSDeployment.

#### <a name="using-claim-rule-templates"></a>Uso de plantillas de regla de notificación
Las plantillas de reglas de notificaciones solo se usan durante el proceso de creación de reglas de notificación. Puede usar cualquiera de las plantillas siguientes para crear una regla de notificación:

-   Pasar por una notificación entrante o filtrarla

-   Transformar una notificación entrante

-   Enviar atributos LDAP como notificaciones

-   enviar asociación al grupo como notificación

-   Enviar notificaciones mediante regla personalizada

-   Permitir o denegar a los usuarios en función una notificación entrante

-   Permitir a todos los usuarios

Para obtener más información que describe cada una de estas plantillas de reglas de notificaciones, consulte [determinar el tipo de plantilla de regla de notificaciones que se va a usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).

#### <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación
Para las reglas de negocio que escapan del ámbito de las plantillas de reglas de notificación estándares, puede usar una plantilla de reglas personalizada para expresar una serie de condiciones lógicas complejas con el lenguaje de reglas de notificación. Para obtener más información sobre el uso de una regla personalizada, consulte [Cuándo usar una regla de notificaciones personalizadas](When-to-Use-a-Custom-Claim-Rule.md).

#### <a name="using-windowspowershell"></a>Uso de Windows PowerShell
También puede usar el objeto de cmdlet ADFSClaimRuleSet con Windows PowerShell para crear o administrar reglas en AD FS. Para obtener más información sobre cómo puede usar Windows PowerShell con este cmdlet, consulte [Administración de AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).

## <a name="what-is-a-claim-rule-set"></a>¿Qué es un conjunto de reglas de notificación?
Como se muestra en la ilustración siguiente, un conjunto de reglas de notificación es una agrupación de una o varias reglas para una relación de confianza federada determinada que define la forma en que el motor de reglas de notificación procesará las notificaciones. Cuando se recibe una notificación entrante en el Servicio de federación, el motor de reglas de notificación aplica la lógica que especifica el conjunto de reglas de notificación adecuado. Es la suma final de la lógica de cada regla del conjunto la que determina cómo se emiten las notificaciones para una relación de confianza determinada en su totalidad.

![Roles AD FS](media/adfs2_claimruleset.gif)

El motor de notificaciones procesa las reglas de notificación en orden cronológico, dentro de un conjunto determinado de reglas. Este orden es importante, ya que el resultado de una regla puede usarse como entrada de la siguiente regla del conjunto.

## <a name="what-are-claim-rule-set-types"></a>¿Qué son los tipos de conjuntos de reglas de notificación?
Un tipo de conjunto de reglas de notificación es un segmento lógico de una relación de confianza federada que identifica de manera categórica si el conjunto de reglas de notificación asociado a la relación de confianza se usará para la emisión, autorización o aceptación de notificaciones. Cada relación de confianza federada puede tener uno o varios tipos de reglas de notificación asociados, según el tipo de relación de confianza que se use.

En la tabla siguiente se describen los distintos tipos de conjuntos de reglas de notificación y se explica su relación con una relación de confianza del proveedor de notificaciones o con una relación de confianza para usuario autenticado.

|Tipo de conjunto de reglas de notificación|Descripción|Se usa en|
|-----------------------|---------------|-----------|
|Conjunto de reglas de transformación de aceptación|Un conjunto de reglas de notificación que usa en una relación de proveedor de notificaciones concreta para especificar las notificaciones entrantes que se aceptarán desde la organización del proveedor de notificaciones y las notificaciones salientes que se enviarán a la relación de confianza para usuario autenticado.<p>Las notificaciones entrantes que se usarán como origen de este conjunto de reglas serán las notificaciones de salida del conjunto de reglas de transformación de emisión, como se especifica en la organización del proveedor de notificaciones.<p>De forma predeterminada, el nodo de confianza del proveedor de notificaciones contiene una relación de confianza de proveedor de notificaciones denominada **Active Directory**, que se usa para representar el almacén de atributos de origen para el conjunto de reglas de transformación de aceptación. Este objeto de confianza se usa para representar la conexión desde el Servicio de federación a una base de datos de Active Directory en la red. Esta relación de confianza predeterminada es la que procesa las notificaciones para los usuarios que Active Directory ha autenticado y no se puede eliminar.|Relaciones de confianza de proveedor de notificaciones|
|Conjunto de reglas de transformación de emisión|Un conjunto de reglas de notificación que se usan en una relación de confianza para usuario autenticado a fin de especificar las notificaciones que se emitirán al usuario autenticado.<p>Las notificaciones entrantes que se usarán como origen para este conjunto de reglas serán inicialmente las notificaciones de salida de las reglas de transformación de aceptación.|Relaciones de confianza para usuario autenticado|
|Conjunto de reglas de autorización de emisión|Un conjunto de reglas de notificación que se usa en una relación de confianza para usuario autenticado a fin de especificar los usuarios a los que se les permitirá recibir un token para el usuario autenticado.<p>Estas reglas determinan si un usuario puede recibir notificaciones para un usuario autenticado y, por lo tanto, obtener acceso al usuario autenticado.<p>A menos que especifique una regla de autorización de emisión, se denegará el acceso a todos los usuarios de forma predeterminada.|Relaciones de confianza para usuario autenticado|
|Conjunto de reglas de autorización de delegación|Un conjunto de reglas de notificación que se usa en una relación de confianza para usuario autenticado a fin de especificar los usuarios que tendrán permiso para actuar como delegados de otros usuarios para el usuario autenticado.<p>Estas reglas determinan si se permite que el solicitante pueda suplantar a un usuario mientras se sigue identificando al solicitante en el token que se envía al usuario autenticado.<p>A menos que especifique una regla de autorización de delegación, ningún usuario puede actuar como delegado de forma predeterminada.|Relaciones de confianza para usuario autenticado|
|Conjunto de reglas de autorización de suplantación|Un conjunto de reglas de notificación que se configura mediante Windows PowerShell para determinar si un usuario puede suplantar a otro para el usuario autenticado.<p>Estas reglas determinan si se permite que el solicitante pueda suplantar a un usuario sin identificar al solicitante en el token que se envía al usuario autenticado.<p>La suplantación de otro usuario de esta manera es una capacidad muy eficaz, ya que el usuario autenticado no sabrá que se está suplantando al usuario.|Relación de usuario de confianza|

Para obtener más información acerca de cómo seleccionar las reglas de notificaciones adecuadas que se usarán en su organización, consulte [determinar el tipo de plantilla de regla de notificaciones que se va a usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).


