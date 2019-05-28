---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Crear una regla para enviar notificaciones mediante una regla personalizada
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ade2a8304288d102608c81a0c29155478e5a4b7b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189434"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Crear una regla para enviar notificaciones mediante una regla personalizada


Mediante el uso de la **enviar notificaciones mediante una regla personalizada** plantilla en los servicios de federación de Active Directory (AD FS), puede crear reglas de notificación personalizada para la situación en que una plantilla de regla estándar no satisface los requisitos de su organización. Reglas de notificaciones personalizadas se escriben en el lenguaje de reglas de notificación y, a continuación, se debe copiar en el **regla personalizada** cuadro de texto antes de que se pueden usar en un conjunto de reglas. Para obtener información sobre cómo construir la sintaxis para una regla avanzada, consulte [The Role of the Claim Rule Language](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
Puede usar el procedimiento siguiente para crear una regla de notificación mediante el complemento Administración de AD FS\-en.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla. En **regla personalizada**, escriba o pegue la sintaxis de lenguaje de reglas de notificación que desee para esta regla.  
![Crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Haga clic en **Finalizar**.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla. En **regla personalizada**, escriba o pegue la sintaxis de lenguaje de reglas de notificación que desee para esta regla.  
![Crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Haga clic en **Finalizar**.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar notificaciones mediante una notificación personalizada en Windows Server 2012 R2 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, haga clic en **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS\\relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o **autenticado**y, a continuación, haga clic en un determinado confiar en la lista donde desea crear esta regla.  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.  
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  En el **editar reglas de notificación** cuadro de diálogo, seleccione una las fichas siguientes, que depende de la relación de confianza que está editando y en qué regla establece desean crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar la regla Asistente para la que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante regla personalizada** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla. En **regla personalizada**, escriba o pegue la sintaxis de lenguaje de reglas de notificación que desee para esta regla.  
![Crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Haga clic en **Finalizar**.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configuración de regla de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
