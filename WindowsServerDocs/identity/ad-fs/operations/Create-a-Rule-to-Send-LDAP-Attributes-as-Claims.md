---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Crear una regla para enviar atributos LDAP como notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00ea4f9f868b9c82c2a0859be971db26394251a3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189349"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Crear una regla para enviar atributos LDAP como notificaciones


Mediante el envío de atributos LDAP como plantilla de reglas de notificaciones en servicios de federación de Active Directory \(AD FS\), puede crear una regla que seleccionará los atributos de un Lightweight Directory Access Protocol \(LDAP\)almacén de atributos, como Active Directory, que se envían como notificaciones al usuario autenticado. Por ejemplo, puede usar esta plantilla de regla para crear un enviar atributos LDAP como notificaciones de regla que extraerá los valores de atributo para los usuarios autenticados de los **displayName** y **telephoneNumber** activo Directorio de los atributos y, a continuación, enviar esos valores como dos notificaciones salientes diferentes.  
  
También puede utilizar esta regla para enviar todas las pertenencias a grupos del usuario. Si desea enviar solo pertenencias a grupos individuales, use la plantilla de regla de envío de pertenencia a grupos como una notificación. Puede usar el procedimiento siguiente para crear una regla de notificación con el complemento Administración de AD FS\-en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar atributos LDAP como notificaciones para una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar atributos LDAP como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, seleccione el **atributo Store**y, a continuación, seleccione el atributo LDAP y asígnelo a la tipo de notificación saliente. 
![Crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar atributos LDAP como notificaciones para una confianza de proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar atributos LDAP como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, seleccione el **atributo Store**y, a continuación, seleccione el atributo LDAP y asígnelo a la tipo de notificación saliente. 
![Crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Para crear una regla para enviar atributos LDAP como notificaciones de Windows Server 2012 R2  
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **FSAD de AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o **autenticado**y, a continuación, haga clic en un confianza específicos en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  En el **editar reglas de notificación** cuadro de diálogo, seleccione una de las fichas siguientes, dependiendo de la relación de confianza que va a editar y establezca la regla a la desea crear esta regla en y, a continuación, haga clic en **Agregar regla** para iniciar la regla Asistente para la que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar atributos LDAP como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **almacén de atributos** seleccione **Active Directory**y en **tipos de notificación de asignación de atributos LDAP a salientes** seleccione deseado **atributo LDAP** y correspondiente **tipo de notificación saliente** tipos en la lista\-listas desplegables.  
  
    Debe seleccionar un nuevo atributo LDAP y el par de tipo de notificación saliente de una fila diferente para cada atributo de Active Directory que desea emitir una notificación para como parte de esta regla.  
![Crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
