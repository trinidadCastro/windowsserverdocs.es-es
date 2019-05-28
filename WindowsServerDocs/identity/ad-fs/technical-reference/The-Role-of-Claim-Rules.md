---
ms.assetid: 65e474b5-3076-4ba3-809d-a09160f7c2bd
title: El papel de las reglas de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 06f2f0d1fb48c6b9dea89762a30fdf77643d0e53
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188574"
---
# <a name="the-role-of-claim-rules"></a>El papel de las reglas de notificaciones
La función general del servicio de federación de Active Directory Federation Services \(AD FS\) es emitir un token que contenga un conjunto de notificaciones. La decisión con respecto a qué notificaciones de AD FS acepta y luego emite se rige por reglas de notificación.  
  
## <a name="what-are-claim-rules"></a>¿Qué son las reglas de notificación?  
Una regla de notificación representa una instancia de la lógica de negocios que va a realizar una o más notificaciones entrantes, les aplicará condiciones \(if x, entonces y\) y generar notificaciones salientes según los parámetros de condición. Para obtener más información acerca de las notificaciones entrantes y salientes, consulte [The Role of Claims](The-Role-of-Claims.md).  
  
Usará reglas de notificación cuando necesite implementar lógica de negocios que controle el flujo de notificaciones a través de la canalización de notificaciones. Mientras que la canalización de notificaciones es más un concepto lógico del extremo\-a\-terminar el proceso de flujo de notificaciones, las reglas son un elemento administrativo real que se puede usar para personalizar el flujo de notificaciones a través del proceso de emisión de notificaciones de notificación.  
  
Para obtener más información acerca de la canalización de notificaciones, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
Las reglas de notificación ofrecen las siguientes ventajas:  
  
-   Proporcionar un mecanismo para que los administradores apliquen ejecución\-lógica de negocios de tiempo para confiar en las notificaciones de proveedores de notificaciones  
  
-   Proporcionan un mecanismo para que los administradores definan las notificaciones que se publicarán para los usuarios autenticados.  
  
-   Proporcionar notificaciones detalladas y eficaces\-en función de las capacidades de autorización a los administradores que desean permitir o denegar el acceso a usuarios específicos  
  
### <a name="how-claim-rules-are-processed"></a>Cómo se procesan las reglas de notificación  
Las reglas de notificación se procesan a través de la canalización de notificaciones con el *motor de notificaciones*. El motor de notificaciones es un componente lógico del Servicio de federación que examina el conjunto de notificaciones entrantes que presenta un usuario y, después, según la lógica de cada regla, generará un conjunto de resultados de notificaciones.  
  
Juntos, el motor de reglas de notificaciones y el conjunto de reglas de notificación asociado a una relación de confianza federada concreta determinan si las notificaciones entrantes deben pasarse tal y como están, si se deben filtrar para cumplir criterios de una condición específica o si se deben transformar en un conjunto de notificaciones completamente nuevo antes de que el Servicio de federación las emita.  
  
Para obtener más información sobre este proceso, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
## <a name="what-are-claim-rule-templates"></a>¿Qué son las plantillas de reglas de notificación?  
AD FS incluye un conjunto predefinido de regla de notificación de las plantillas que están diseñadas para ayudarle a fácilmente seleccionar y crean las reglas de notificación más adecuadas para sus necesidades de negocio concretas. Las plantillas de reglas de notificaciones solo se usan durante el proceso de creación de reglas de notificación.  
  
En el complemento Administración de AD FS\-, las reglas solo pueden crearse mediante las plantillas de regla de notificación. Después de usar el complemento\-en para seleccionar una plantilla de regla de notificación, los datos necesarios para la lógica de la regla de entrada y guárdelo en la base de datos de configuración, será \(desde ese punto en adelante\) hace referencia en la interfaz de usuario como una regla de notificación.  
  
### <a name="how-claim-rule-templates-work"></a>Cómo funcionan las plantillas de reglas de notificación  
A primera vista, parecen ser simplemente formularios de entrada proporcionados por el complemento Plantillas de regla de notificación\-para recopilar datos y procesar lógica específica en las notificaciones entrantes. Sin embargo, en un nivel mucho más detallado, las plantillas de reglas de notificación almacenan el marco del lenguaje de reglas de notificación necesario que constituye la lógica de base necesaria para que pueda crear rápidamente una regla sin necesidad de conocer el lenguaje en detalle.  
  
Cada plantilla que se proporciona en la interfaz de usuario \(UI\) representa una sintaxis de lenguaje de la regla de notificación rellenada previamente, basándose en las tareas administrativas requeridas con más frecuencia. Sin embargo, existe una plantilla de reglas que es la excepción. Esta plantilla se conoce como la plantilla de reglas personalizada. Con esta plantilla, no se rellena previamente ninguna sintaxis. En su lugar, debe crear directamente la sintaxis del lenguaje de reglas de notificación en el cuerpo del formulario de la plantilla de reglas de notificación mediante la sintaxis del lenguaje de reglas de notificación.  
  
Para obtener más información sobre cómo usar la sintaxis de lenguaje de reglas de notificación, consulte [The Role of el lenguaje de reglas de notificación](The-Role-of-the-Claim-Rule-Language.md) en la Guía de implementación de AD FS.  
  
> [!TIP]  
> Puede ver el lenguaje de reglas de notificación asociado a una regla en cualquier momento si hace clic en el botón **Ver lenguaje de la regla** de las propiedades de una regla de notificación.  
  
### <a name="how-to-create-a-claim-rule"></a>Cómo crear una regla de notificación  
Las reglas de notificación se crean por separado para cada relación de confianza federada dentro del Servicio de federación y no se comparten entre varias relaciones de confianza. Puede crear una regla desde una plantilla de regla de notificación, empezar desde cero mediante la creación de la regla con el lenguaje de reglas de notificación o usar Windows PowerShell para personalizar una regla.  
  
Todas estas opciones coexisten para ofrecerle la flexibilidad de elegir el método adecuado para un escenario determinado. Para obtener más información sobre cómo crear una regla de notificación, consulte [configurar reglas de notificación](https://technet.microsoft.com/library/ee913571.aspx) en la Guía de AD FSDeployment.  
  
#### <a name="using-claim-rule-templates"></a>Uso de plantillas de regla de notificación  
Las plantillas de reglas de notificaciones solo se usan durante el proceso de creación de reglas de notificación. Puede usar cualquiera de las plantillas siguientes para crear una regla de notificación:  
  
-   Pasar o filtrar una notificación entrante  
  
-   Transformar una notificación entrante  
  
-   Enviar atributos LDAP como notificaciones  
  
-   Enviar la pertenencia a grupo como una notificación  
  
-   Enviar notificaciones mediante una regla personalizada  
  
-   Permitir o denegar a los usuarios en función una notificación entrante  
  
-   Permitir a todos los usuarios  
  
Para obtener más información que describe cada una de estas plantillas de regla de notificación, consulte [determinar el tipo de notificación de plantilla de regla para uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  
#### <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación  
Para las reglas de negocio que escapan del ámbito de las plantillas de reglas de notificación estándares, puede usar una plantilla de reglas personalizada para expresar una serie de condiciones lógicas complejas con el lenguaje de reglas de notificación. Para obtener más información sobre el uso de una regla personalizada, vea [cuándo se debe usar una regla de notificación personalizada](When-to-Use-a-Custom-Claim-Rule.md).  
  
#### <a name="using-windowspowershell"></a>Uso de Windows PowerShell  
También puede utilizar el objeto de cmdlet ADFSClaimRuleSet con Windows PowerShell para crear o administrar las reglas de AD FS. Para obtener más información acerca de cómo usar Windows PowerShell con este cmdlet, consulte [administración de AD FS con Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=179634).  
  
## <a name="what-is-a-claim-rule-set"></a>¿Qué es un conjunto de reglas de notificación?  
Como se muestra en la ilustración siguiente, un conjunto de reglas de notificación es una agrupación de una o varias reglas para una relación de confianza federada determinada que define la forma en que el motor de reglas de notificación procesará las notificaciones. Cuando se recibe una notificación entrante en el Servicio de federación, el motor de reglas de notificación aplica la lógica que especifica el conjunto de reglas de notificación adecuado. Es la suma final de la lógica de cada regla del conjunto la que determina cómo se emiten las notificaciones para una relación de confianza determinada en su totalidad.  
  
![Roles de AD FS](media/adfs2_claimruleset.gif)  
  
El motor de notificaciones procesa las reglas de notificación en orden cronológico, dentro de un conjunto determinado de reglas. Este orden es importante, ya que el resultado de una regla puede usarse como entrada de la siguiente regla del conjunto.  
  
## <a name="what-are-claim-rule-set-types"></a>¿Qué son los tipos de conjuntos de reglas de notificación?  
Un tipo de conjunto de reglas de notificación es un segmento lógico de una relación de confianza federada que identifica de manera categórica si el conjunto de reglas de notificación asociado a la relación de confianza se usará para la emisión, autorización o aceptación de notificaciones. Cada relación de confianza federada puede tener uno o varios tipos de reglas de notificación asociados, según el tipo de relación de confianza que se use.  
  
En la tabla siguiente se describen los distintos tipos de conjuntos de reglas de notificación y se explica su relación con una relación de confianza del proveedor de notificaciones o con una relación de confianza para usuario autenticado.  
  
|Tipo de conjunto de reglas de notificación|Descripción|Se usa en|  
|-----------------------|---------------|-----------|  
|Conjunto de reglas de transformación de aceptación|Un conjunto de reglas de notificación que usa en una relación de proveedor de notificaciones concreta para especificar las notificaciones entrantes que se aceptarán desde la organización del proveedor de notificaciones y las notificaciones salientes que se enviarán a la relación de confianza para usuario autenticado.<br /><br />Las notificaciones entrantes que se usarán como origen de este conjunto de reglas serán las notificaciones de salida del conjunto de reglas de transformación de emisión, como se especifica en la organización del proveedor de notificaciones.<br /><br />De forma predeterminada, el nodo de confianza del proveedor de notificaciones contiene una relación de confianza de proveedor de notificaciones denominada **Active Directory**, que se usa para representar el almacén de atributos de origen para el conjunto de reglas de transformación de aceptación. Este objeto de confianza se usa para representar la conexión desde el Servicio de federación a una base de datos de Active Directory en la red. Esta relación de confianza predeterminada es la que procesa las notificaciones para los usuarios que Active Directory ha autenticado y no se puede eliminar.|Relaciones de confianza de proveedor de notificaciones|  
|Conjunto de reglas de transformación de emisión|Un conjunto de reglas de notificación que se usan en una relación de confianza para usuario autenticado a fin de especificar las notificaciones que se emitirán al usuario autenticado.<br /><br />Las notificaciones entrantes que se usarán como origen para este conjunto de reglas serán inicialmente las notificaciones de salida de las reglas de transformación de aceptación.|Relaciones de confianza para usuario autenticado|  
|Conjunto de reglas de autorización de emisión|Un conjunto de reglas de notificación que se usa en una relación de confianza para usuario autenticado a fin de especificar los usuarios a los que se les permitirá recibir un token para el usuario autenticado.<br /><br />Estas reglas determinan si un usuario puede recibir notificaciones para un usuario autenticado y, por lo tanto, obtener acceso al usuario autenticado.<br /><br />A menos que especifique una regla de autorización de emisión, se denegará el acceso a todos los usuarios de forma predeterminada.|Relaciones de confianza para usuario autenticado|  
|Conjunto de reglas de autorización de delegación|Un conjunto de reglas de notificación que se usa en una relación de confianza para usuario autenticado a fin de especificar los usuarios que tendrán permiso para actuar como delegados de otros usuarios para el usuario autenticado.<br /><br />Estas reglas determinan si se permite que el solicitante pueda suplantar a un usuario mientras se sigue identificando al solicitante en el token que se envía al usuario autenticado.<br /><br />A menos que especifique una regla de autorización de emisión, ningún usuario puede actuar como delegado de manera predeterminada.|Relaciones de confianza para usuario autenticado|  
|Conjunto de reglas de autorización de suplantación|Un conjunto de reglas de notificación que se configura mediante Windows PowerShell para determinar si un usuario puede suplantar a otro para el usuario autenticado.<br /><br />Estas reglas determinan si se permite que el solicitante pueda suplantar a un usuario sin identificar al solicitante en el token que se envía al usuario autenticado.<br /><br />La suplantación de otro usuario de esta manera es una capacidad muy eficaz, ya que el usuario autenticado no sabrá que se está suplantando al usuario.|Confianza para usuario autenticado|  
  
Para obtener más información sobre cómo seleccionar las reglas de notificación adecuado para usarla en su organización, consulte [determinar el tipo de notificación de plantilla de regla para uso](Determine-the-Type-of-Claim-Rule-Template-to-Use.md).  
  

