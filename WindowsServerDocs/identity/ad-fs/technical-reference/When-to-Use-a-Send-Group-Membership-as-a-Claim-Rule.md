---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: "Cuándo usar una pertenencia a grupos de enviar como una regla de notificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10e027b4de463aad2b05eae40a622aebf8f12a3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>Cuándo usar una pertenencia a grupos de enviar como una regla de notificación
Puedes usar esta regla en los servicios de federación de Active Directory \(AD FS\) cuando quieras emitir un nuevo valor de la solicitud saliente para solo aquellos usuarios que son miembros de un grupo de seguridad Activ directorio especificado. Cuando usas esta regla, se emite una solicitud única de solo el grupo que especifiques y que coincida con la lógica de la regla, tal como se describe en la siguiente tabla.  
  
|Opción de regla|Lógica de la regla|  
|---------------|--------------|  
|Valor de notificación saliente|Si es igual a la pertenencia a grupos de un usuario la *grupo especificado* y tipo de notificación saliente es igual a *especifica el tipo de notificación*, a continuación, reemplaza el valor de nombre de grupo existente con la *saliente especificado el valor de notificación* y emitir la reclamación.|  
  
Las siguientes secciones proporcionan una introducción básica reglas de notificación. También ofrecen detalles sobre cuándo usar la pertenencia al grupo de enviar como una regla de notificación.  
  
## <a name="about-claim-rules"></a>Acerca de las reglas de notificación  
Una regla de Reclamación representa una instancia de la lógica de negocios que tomar una notificación entrante, aplicar una condición a ella \ (si está x después y\) y generar una notificación saliente basándose en los parámetros de la condición. La siguiente lista describe sugerencias que debes conocer reclamar reglas antes de leer más en este tema:  
  
-   En la administración de FS anuncios en snap\, reglas de notificación solo se pueden crear mediante plantillas de notificación de regla  
  
-   Proceso de reglas de notificación entrante dice directamente desde un proveedor de notificaciones \ (como Active Directory u otro Service\ federación) o desde la salida de la aceptación, transforman las reglas en una relación de confianza del proveedor de notificaciones.  
  
-   Reglas de notificación se procesan el motor de emisión de notificaciones en orden cronológico dentro de un conjunto de reglas determinado. Estableciendo la prioridad de las reglas de aún más puede restringir o filtrar notificaciones que se generan mediante reglas anteriores dentro de un conjunto de reglas determinado.  
  
-   Plantillas de notificación de regla siempre requieren que especifiques un tipo de notificación entrante. Sin embargo, puede procesar varios valores de las notificaciones con el mismo tipo de notificación con una sola regla.  
  
Para obtener más información sobre la reclamación y reclamación conjuntos de reglas, consulta [el rol reclamar las reglas de](The-Role-of-Claim-Rules.md). Para obtener más información acerca de cómo se procesan las reglas, consulta [el rol del motor reclamaciones](The-Role-of-the-Claims-Engine.md). Para obtener más información, cómo se procesan los conjuntos de reglas de notificación, consulta [el rol de la canalización de reclamaciones](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="outgoing-claim-value"></a>Valor de notificación saliente  
Usar la pertenencia al grupo de enviar como una plantilla de regla de notificación, puede emitir una reclamación que depende de si un usuario es miembro de un grupo que especifiques.  
  
En otras palabras, el identificador de estos problemas de plantilla de regla una notificación solamente cuando el usuario tiene la seguridad de grupo \(SID\) que coincide con el grupo de Active Directory que especifica el administrador. Todos los usuarios autentican en los servicios de dominio de Active Directory \(AD DS\) tendrá grupo entrante que SID las reclamaciones por cada grupo que pertenecen. De manera predeterminada, las reglas de transformación de aceptación de Active Directory reclamaciones de proveedor de confianza se pasan a través de estos SID reclamaciones de grupo. Usando estos grupo SID como base para emitir notificaciones es mucho más rápido que buscar los grupos del usuario en AD DS.  
  
Cuando usas esta regla, solo una reclamación solo se envían basado en el grupo de Active Directory que selecciones. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que enviará una notificación de grupo con un valor de "Administrador" si el usuario es miembro del grupo de seguridad Administradores de dominio.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configuración de esta regla en una relación de confianza del proveedor de notificaciones  
Los administradores deben usar este tipo de regla en las reglas de transformación de aceptación de una confianza de proveedor de notificaciones solo cuando se reciben los SID del grupo en el proveedor de notificaciones, que es muy poco habitual para los proveedores de reclamaciones excepto Active Directory o AD DS.  
  
## <a name="how-to-create-this-rule"></a>Cómo crear esta regla  
Crear esta regla ya sea con el idioma de la regla de reclamación o mediante el uso de la pertenencia al grupo enviar del LDAP como una notificación de plantilla en la administración de AD FS snap\ en la regla. Esta plantilla de regla proporciona las siguientes opciones de configuración:  
  
-   Especificar un nombre de regla de notificación  
  
-   Selecciona el grupo de un usuario mediante el selector de objetos  
  
-   Selecciona un tipo de notificación saliente  
  
-   Selecciona un formato de nombre de identificador saliente \ (que está disponible solo cuando se elige Id. de nombre de la salida field\ tipo de notificación)  
  
-   Especificar un valor de la solicitud saliente  
  
Para obtener más información sobre cómo crear esta regla, consulta [crear una regla para enviar pertenencia a grupos como notificación](https://technet.microsoft.com/en-us/library/ee913569.aspx).  
  
## <a name="using-the-claim-rule-language"></a>Con el lenguaje de regla de notificación  
Si desea emitir reclamaciones basadas en un SID entrante que no sea un SID de grupo, la transformación se usa una plantilla de regla de notificación entrante. Si el administrador quiere recuperar los nombres de todos los grupos que el usuario es miembro de, usa los atributos de LDAP enviar como plantilla de regla de notificaciones en su lugar con la **tokenGroups** atributo.  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>Ejemplo: Procedimiento para emitir en función de la pertenencia a grupos de notificaciones de grupo  
La siguiente regla problemas de notificaciones de grupo para un usuario en función de un grupo entrante SID:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
[Crear una regla para enviar atributos LDAP como notificaciones](https://technet.microsoft.com/library/dd807115.aspx)  
  

