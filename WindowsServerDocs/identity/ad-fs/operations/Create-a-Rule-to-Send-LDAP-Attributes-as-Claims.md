---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Crear una regla para enviar atributos LDAP como notificaciones
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Crear una regla para enviar atributos LDAP como notificaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Con los atributos de LDAP enviar como plantilla de regla de notificaciones en los servicios de federación de Active Directory \(AD FS\), puedes crear una regla que se seleccione atributos de un almacén de atributo de protocolo ligero de acceso a directorios \(LDAP\), como Active Directory, para enviar como notificaciones para el usuario de confianza. Por ejemplo, puedes usar esta plantilla de regla para crear atributos LDAP enviar como reclamaciones de regla que extrae los valores de atributo para usuarios autenticados desde el **displayName** y **telephoneNumber** atributos de Active Directory y, a continuación, enviar esos valores como dos notificaciones salientes diferentes.  
  
También puedes usar esta regla para enviar la pertenencia a grupos de todos los del usuario. Si quieres enviar solo pertenencias a grupos individuales, usa la pertenencia al grupo de enviar como una plantilla de regla de notificación. Puedes usar el siguiente procedimiento para crear una regla de notificación con la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar atributos LDAP como reclamaciones por un confiar terceros de confianza en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar atributos LDAP como notificaciones** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escribe el nombre para mostrar para esta regla, selecciona el **atributo tienda**y, a continuación, selecciona el atributo LDAP y se asigna al tipo de notificación saliente. 
![Crear la regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar atributos LDAP como reclamaciones por un proveedor de notificaciones de confianza en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar atributos LDAP como notificaciones** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escribe el nombre para mostrar para esta regla, selecciona el **atributo tienda**y, a continuación, selecciona el atributo LDAP y se asigna al tipo de notificación saliente. 
![Crear la regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Para crear una regla para enviar atributos LDAP como notificaciones de Windows Server 2012 R2  
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FSAD FS\\Trust relaciones**, haz clic en **reclamaciones proveedor confía** o **confiar confía en las partes**y, a continuación, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  En la **editar reglas de notificación** cuadro de diálogo, selecciona una de las siguientes pestañas, según la confianza de que vas a editar y establece qué regla quieras crear esta regla en y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar atributos LDAP como notificaciones** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escribe el nombre para mostrar para esta regla, en **tienda atributo** selecciona **Active Directory**y, en **atributos de asignación de LDAP a salientes los tipos de notificación** seleccionar el deseado **atributo LDAP** y correspondiente **el tipo de notificación saliente** tipos de las listas de drop\.  
  
    Tienes que seleccionar un nuevo atributo LDAP y un par de tipo de Reclamación saliente en una fila diferente para cada atributo de Active Directory que quieras para emitir una reclamación de como parte de esta regla.  
![Crear la regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
