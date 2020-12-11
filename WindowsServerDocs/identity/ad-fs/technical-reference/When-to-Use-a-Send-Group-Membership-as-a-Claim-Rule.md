---
description: 'Más información acerca de: Cuándo usar una pertenencia a un grupo de envío como una regla de notificaciones'
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Cuando usar la pertenencia a un grupo de envío como una regla de notificación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 912ebf3fe8c3db96de2d615d3bda6ea3c7f3f7a4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050433"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Cuando usar la pertenencia a un grupo de envío como una regla de notificación
Puede usar esta regla en Servicios de federación de Active Directory (AD FS) \( AD FS \) cuando quiera emitir un nuevo valor de notificaciones salientes solo para los usuarios que son miembros de un grupo de seguridad de Active Directory especificado. Cuando usa esta regla, emitirá una notificación única solo para el grupo que especifique y que coincida con la lógica de la regla, como se describe en la tabla siguiente.

|Opción de regla|Lógica de regla|
|---------------|--------------|
|Valor de notificación saliente|Si la pertenencia a grupos de un usuario es igual al *grupo especificado* y el tipo de notificaciones salientes es igual a *tipo de demanda especificado*, reemplace el valor del nombre del grupo existente por el *valor de notificaciones salientes especificado* y emita la demanda.|

Las secciones siguientes ofrecen una introducción básica a las reglas de notificación. También se proporcionan detalles sobre cuándo se debe usar la pertenencia al grupo de envío como una regla de notificación.

## <a name="about-claim-rules"></a>Acerca de las reglas de notificaciones
Una regla de notificaciones representa una instancia de la lógica de negocios que tomará una solicitud entrante, le aplicará una condición \( si x después \) y y generará una notificaciones salientes según los parámetros de la condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:

-   En el complemento de administración de AD FS \- , las reglas de notificaciones solo se pueden crear mediante plantillas de regla de notificaciones.

-   Las reglas de notificación procesan las notificaciones entrantes directamente desde un proveedor de notificaciones \( como Active Directory u otro servicio de Federación \) o desde la salida de las reglas de transformación de aceptación en una relación de confianza para proveedor de notificaciones.

-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.

-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.

Para obtener información más detallada sobre las reglas de notificaciones y los conjuntos de reglas de notificaciones, consulte [el rol de las reglas de notificaciones](The-Role-of-Claim-Rules.md). Para obtener más información sobre cómo se procesan las reglas, vea [el rol del motor de notificaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información sobre cómo se procesan los conjuntos de reglas de notificaciones, vea [el rol de la canalización de notificaciones](The-Role-of-the-Claims-Pipeline.md).

## <a name="outgoing-claim-value"></a>Valor de notificación saliente
Si usa la pertenencia al grupo de envío como una plantilla de regla de notificación, puede emitir una notificación que dependa de si un usuario es miembro de un grupo que especifique.

En otras palabras, esta plantilla de regla emite una petición solo cuando el usuario tiene el SID del ID. de seguridad de grupo \( \) que coincide con el grupo de Active Directory que el administrador especifica. Todos los usuarios que se autentiquen en Active Directory Domain Services \( AD DS tendrán \) notificaciones de SID de grupo entrantes para cada grupo al que pertenezcan. De forma predeterminada, las reglas de transformación de aceptación en la confianza del proveedor de notificaciones de Active Directory se pasan a través de estas notificaciones de SID de grupo. El uso de estos SID de grupo como base para emitir notificaciones es mucho más rápido que la búsqueda de grupos del usuario en AD DS.

Cuando se usa esta regla, solo se envía una notificación única, en función del grupo de Active Directory que seleccione. Por ejemplo, puede usar esta plantilla de regla para crear una regla que envíe una notificación de grupo con un valor de "Admin" si el usuario es miembro del grupo de seguridad Administradores de dominio.

## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurar esta regla en una confianza de proveedor de notificaciones
Los administradores deben usar este tipo de regla en las reglas de transformación de aceptación de una confianza de proveedor de notificaciones solo cuando los SID de grupo se reciben desde el proveedor de notificaciones, lo que es muy raro en la mayoría de proveedores de notificaciones, excepto Active Directory o AD DS.

## <a name="how-to-create-this-rule"></a>Cómo crear esta regla
Esta regla se crea mediante el lenguaje de reglas de notificaciones o mediante el uso de la plantilla enviar pertenencia a grupos LDAP como una regla de notificaciones en el complemento de administración \- de AD FS en. Esta plantilla de regla permite las siguientes opciones de configuración:

-   Especificar un nombre de regla de notificaciones

-   Seleccionar el grupo de un usuario mediante el selector de objetos

-   Seleccionar un tipo de notificación saliente.

-   Seleccione un formato de identificador de nombre saliente \( que esté disponible solo si se elige ID. de nombre en el campo tipo de notificaciones salientes.\)

-   Especificar un valor de notificación saliente.

Para obtener más información sobre cómo crear esta regla, consulte [crear una regla para enviar la pertenencia a un grupo como una demanda](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913569(v=ws.11)).

## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación
Si quiere emitir notificaciones basadas en un SID entrante que no sea un SID de grupo, use la plantilla de regla Transformar una notificación entrante. Si el administrador quiere recuperar los nombres de todos los grupos de los que el usuario es miembro, use en su lugar Enviar atributos LDAP como plantilla de reglas de notificaciones con el atributo **tokenGroups**.

### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Ejemplo: cómo emitir notificaciones de grupo basadas en la pertenencia a grupos del usuario
La siguiente regla emite notificaciones de grupo para un usuario según un SID de grupo entrante:

```
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);
```

## <a name="additional-references"></a>Referencias adicionales
[Crear una regla para enviar atributos LDAP como notificaciones](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807115(v=ws.11))

