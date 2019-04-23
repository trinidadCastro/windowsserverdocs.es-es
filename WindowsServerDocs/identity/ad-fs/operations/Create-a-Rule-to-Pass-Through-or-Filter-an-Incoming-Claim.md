---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: Crear una regla para pasar a través o filtrar una notificación entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860276"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Crear una regla para pasar a través o filtrar una notificación entrante

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Mediante el paso a través o filtrar una plantilla de regla de notificación entrante en los servicios de federación de Active Directory \(AD FS\), puede pasar a través de todas las notificaciones entrantes con un tipo de notificación seleccionado. También puede filtrar los valores de las notificaciones entrantes con un tipo de notificación seleccionado. Por ejemplo, puede usar esta plantilla de regla para crear una regla que envíe todas las notificaciones de grupo entrantes. También puede usar esta regla para enviar solo nombre principal de usuario \(UPN\) notificaciones que terminan con @fabrikam.  
  
Puede usar el procedimiento siguiente para crear una regla de notificación con el complemento Administración de AD FS\-en.  
  
El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **pasar o filtrar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente** .  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **tipo de notificación entrante** seleccione un tipo de notificación en la lista y, a continuación, seleccione uno de los las opciones siguientes, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Pasar a través solo un determinado valor de notificación**  
  
    -   **Pasar a través de los valores de notificación que coincidan con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar a través de los valores de notificación que empiezan con un valor específico**  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **pasar o filtrar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente** .  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **tipo de notificación entrante** seleccione un tipo de notificación en la lista y, a continuación, seleccione uno de los las opciones siguientes, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Pasar a través solo un determinado valor de notificación**  
  
    -   **Pasar a través de los valores de notificación que coincidan con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar a través de los valores de notificación que empiezan con un valor específico**  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en Windows Server 2012 R2

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **FSAD de AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o **autenticado**y, a continuación, haga clic en un confianza específicos en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo, seleccione una de las fichas siguientes, dependiendo de la confianza que va a editar y establezca la regla a la desea crear esta regla en y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **pasar o filtrar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente** .  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  En el **configurar regla** página **nombre de la regla de notificación** escriba el nombre para mostrar para esta regla, en **tipo de notificación entrante** seleccione un tipo de notificación en la lista y, a continuación, seleccione uno de los las opciones siguientes, según las necesidades de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Pasar a través solo un determinado valor de notificación**  
  
    -   **Pasar a través de los valores de notificación que coincidan con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar a través de los valores de notificación que empiezan con un valor específico**  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  



  
## <a name="additional-references"></a>Referencias adicionales  
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
  
[Cuándo se debe usar un paso a través o filtrar la regla de notificación](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[El rol de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
