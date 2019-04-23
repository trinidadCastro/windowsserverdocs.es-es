---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: Cuando usar la pertenencia a un grupo de envío como una regla de notificación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dffd886ffd0bedd429918f72408b2d13d9fa1bdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859176"
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Cuando usar la pertenencia a un grupo de envío como una regla de notificación
Puede usar esta regla en Active Directory Federation Services \(AD FS\) cuando desee emitir un nuevo valor de notificación saliente para sólo aquellos usuarios que son miembros de un grupo de seguridad de Active Directory especificado. Cuando usa esta regla, emitirá una notificación única solo para el grupo que especifique y que coincida con la lógica de la regla, como se describe en la tabla siguiente.  
  
|Opción de regla|Lógica de regla|  
|---------------|--------------|  
|Valor de notificación saliente|Si la pertenencia a grupos de un usuario es igual a la del *grupo especificado* y el tipo de notificación saliente es igual a la del *tipo de notificación especificado*, reemplace el valor del nombre del grupo existente por el *valor de notificación saliente especificado* y emita la notificación.|  
  
Las secciones siguientes ofrecen una introducción básica a las reglas de notificación. También se proporcionan detalles sobre cuándo se debe usar la pertenencia al grupo de envío como una regla de notificación.  
  
## <a name="about-claim-rules"></a>Sobre las reglas de notificación  
Una regla de notificación representa una instancia de la lógica de negocios que se tomará una notificación entrante, se aplicará una condición \(if x, entonces y\) y generará una notificación saliente basándose en los parámetros de condición. La lista siguiente destaca las sugerencias importantes que deberías conocer sobre las reglas de notificaciones antes de seguir leyendo este tema:  
  
-   En el complemento Administración de AD FS\-, notificación solo se pueden crear reglas mediante las plantillas de regla de notificación  
  
-   El proceso de reglas de notificación entrante de notificaciones directamente desde un proveedor de notificaciones \(como Active Directory u otro servicio de federación\) o desde la salida de la aceptación de las reglas en una confianza de proveedor de notificaciones de transformación.  
  
-   El motor de emisión de notificaciones procesa las reglas de notificación en orden cronológico dentro de un conjunto determinado de reglas. Al establecer una precedencia en las reglas, puedes perfeccionar o filtrar aún más las notificaciones que generan las reglas anteriores dentro de un conjunto determinado de reglas.  
  
-   Siempre tendrás que especificar un tipo de notificación entrante para las plantillas de regla de notificaciones. Sin embargo, puedes procesar varios valores de notificación con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre las reglas de notificación y conjuntos de reglas de notificación, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Para obtener más información cómo se procesan los conjuntos de reglas de notificación, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valor de notificación saliente  
Si usa la pertenencia al grupo de envío como una plantilla de regla de notificación, puede emitir una notificación que dependa de si un usuario es miembro de un grupo que especifique.  
  
En otras palabras, esta plantilla de regla emite una notificación solo cuando el usuario tiene el identificador de seguridad del grupo \(SID\) que coincide con el grupo de Active Directory que el administrador especifica. Todos los usuarios que se autentican en Active Directory Domain Services \(AD DS\) tendrán notificaciones SID para cada grupo que pertenecen a grupo entrante. De forma predeterminada, las reglas de transformación de aceptación en la confianza del proveedor de notificaciones de Active Directory se pasan a través de estas notificaciones de SID de grupo. El uso de estos SID de grupo como base para emitir notificaciones es mucho más rápido que la búsqueda de grupos del usuario en AD DS.  
  
Cuando se usa esta regla, solo se envía una notificación única, en función del grupo de Active Directory que seleccione. Por ejemplo, puede usar esta plantilla de regla para crear una regla que envíe una notificación de grupo con un valor de "Admin" si el usuario es miembro del grupo de seguridad Administradores de dominio.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurar esta regla en una confianza de proveedor de notificaciones  
Los administradores deben usar este tipo de regla en las reglas de transformación de aceptación de una confianza de proveedor de notificaciones solo cuando los SID de grupo se reciben desde el proveedor de notificaciones, lo que es muy raro en la mayoría de proveedores de notificaciones, excepto Active Directory o AD DS.  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Crear esta regla mediante el lenguaje de reglas de notificación o mediante el uso de la pertenencia al grupo de envío del LDAP como una plantilla de regla de notificación en la administración de AD FS ajustar\-en. Esta plantilla de regla permite las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación.  
  
-   Seleccionar el grupo de un usuario mediante el selector de objetos.  
  
-   Seleccionar un tipo de notificación saliente.  
  
-   Seleccione un formato de Id. de nombre saliente \(que está disponible solo cuando se elige el identificador de nombre del campo de tipo de notificación saliente\)  
  
-   Especificar un valor de notificación saliente.  
  
Para obtener más información sobre cómo crear esta regla, vea [crear una regla para enviar pertenencia a grupo como notificación](https://technet.microsoft.com/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Uso del lenguaje de las reglas de notificación  
Si quiere emitir notificaciones basadas en un SID entrante que no sea un SID de grupo, use la plantilla de regla Transformar una notificación entrante. Si el administrador quiere recuperar los nombres de todos los grupos de los que el usuario es miembro, use en su lugar Enviar atributos LDAP como plantilla de reglas de notificaciones con el atributo **tokenGroups**.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Por ejemplo: Cómo emitir notificaciones de grupo basadas en la pertenencia a grupos  
La siguiente regla emite notificaciones de grupo para un usuario según un SID de grupo entrante:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
[Crear una regla para enviar atributos LDAP como notificaciones](https://technet.microsoft.com/library/dd807115.aspx)  
  

