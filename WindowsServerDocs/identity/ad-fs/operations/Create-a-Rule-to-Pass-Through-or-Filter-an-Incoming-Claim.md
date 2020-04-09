---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: Crear una regla para pasar a través o filtrar una demanda entrante
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fb885d8b822faf4bd5ee82ad70c59b99678a58e9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816838"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Crear una regla para pasar a través o filtrar una demanda entrante

Con la plantilla de reglas pasar a través o filtrar una notificación entrante en Servicios de federación de Active Directory (AD FS) \(AD FS\), puede pasar a través todas las notificaciones entrantes con un tipo de notificación seleccionado. También puede filtrar los valores de las notificaciones entrantes con un tipo de notificación seleccionado. Por ejemplo, puede usar esta plantilla de regla para crear una regla que enviará todas las notificaciones de grupo entrantes. También puede usar esta regla para enviar solo el nombre principal de usuario \(UPN\) notificaciones que terminan con @fabrikam.  
  
Puede usar el siguiente procedimiento para crear una regla de notificaciones con el complemento de administración de AD FS\-en.  
  
La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una demanda entrante en una relación de confianza para usuario autenticado en Windows Server 2016 

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **pasar o filtrar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en **tipo de notificaciones entrantes** Seleccione un tipo de notificaciones en la lista y, a continuación, seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través todos los valores de notificaciones**  
  
    -   **Pasar a través solo un valor de notificaciones específico**  
  
    -   **Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar solo los valores de notificaciones que empiezan con un valor específico**  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en una confianza de proveedor de notificaciones en Windows Server 2016 
  
1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **pasar o filtrar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en **tipo de notificaciones entrantes** Seleccione un tipo de notificaciones en la lista y, a continuación, seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través todos los valores de notificaciones**  
  
    -   **Pasar a través solo un valor de notificaciones específico**  
  
    -   **Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar solo los valores de notificaciones que empiezan con un valor específico**  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Para crear una regla para pasar a través o filtrar una demanda entrante en Windows Server 2012 R2

1.  En Administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **Administración de AD FS**.  
  
2.  En el árbol de consola, en **ad FSAD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas para usuario autenticado**y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.  
  
3.  \-haga clic en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, en función de la confianza que esté editando y el conjunto de reglas en el que desee crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **pasar o filtrar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en **tipo de notificaciones entrantes** Seleccione un tipo de notificaciones en la lista y, a continuación, seleccione una de las siguientes opciones, según las necesidades de su organización:  
  
    -   **Pasar a través todos los valores de notificaciones**  
  
    -   **Pasar a través solo un valor de notificaciones específico**  
  
    -   **Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico**  
  
    -   **Pasar solo los valores de notificaciones que empiezan con un valor específico**  
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Haga clic en el botón **Finalizar** .  
  
8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.  



  
## <a name="additional-references"></a>Referencias adicionales  
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
  
[Cuándo usar una regla de notificaciones de paso a través o filtrar](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
