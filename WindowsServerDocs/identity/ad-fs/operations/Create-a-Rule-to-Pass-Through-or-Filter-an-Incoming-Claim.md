---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: "Crear una regla para atravesar o filtrar una notificación entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>Crear una regla para atravesar o filtrar una notificación entrante

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Con el paso a través o una plantilla de notificación entrante de regla de filtro en los servicios de federación de Active Directory \(AD FS\), se puede pasar a través de todas las solicitudes entrantes con un tipo de notificación seleccionado. También puedes filtrar los valores de notificaciones entrantes con un tipo de notificación seleccionado. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que va a enviar todas las solicitudes entrantes de grupo. También puedes usar esta regla para enviar notificaciones de \(UPN\) de nombre de entidad de seguridad que terminan con usuario solo @fabrikam.  
  
Puedes usar el siguiente procedimiento para crear una regla de notificación con la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para atravesar o filtrar una notificación entrante en una confianza del fabricante de confiar en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **paso a través o una notificación entrante de filtro** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escribe el nombre para mostrar para esta regla, **tipo de notificación entrante** selecciona un tipo de notificación de la lista y, a continuación, selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Pasar solo un determinado valor de notificación**  
  
    -   **Pasar solo valores de las notificaciones que coinciden con un valor de sufijo de correo electrónico específicas**  
  
    -   **Pasar a través de los valores de notificación que comienzan con un valor específico**  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para atravesar o filtrar una notificación entrante en un proveedor de notificaciones de confianza en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **paso a través o una notificación entrante de filtro** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escribe el nombre para mostrar para esta regla, **tipo de notificación entrante** selecciona un tipo de notificación de la lista y, a continuación, selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Pasar solo un determinado valor de notificación**  
  
    -   **Pasar solo valores de las notificaciones que coinciden con un valor de sufijo de correo electrónico específicas**  
  
    -   **Pasar a través de los valores de notificación que comienzan con un valor específico**  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>Para crear una regla para atravesar o filtrar una notificación entrante en Windows Server 2012 R2

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FSAD FS\\Trust relaciones**, haz clic en **reclamaciones proveedor confía** o **confiar confía en las partes**y, a continuación, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo, selecciona una de las siguientes pestañas, según la confianza de que está editando y la regla que establece quieras crear esta regla en y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **paso a través o una notificación entrante de filtro** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  En la **configurar regla** página en **nombre de la regla de Reclamación** escribe el nombre para mostrar para esta regla, **tipo de notificación entrante** selecciona un tipo de notificación de la lista y, a continuación, selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Pasar solo un determinado valor de notificación**  
  
    -   **Pasar solo valores de las notificaciones que coinciden con un valor de sufijo de correo electrónico específicas**  
  
    -   **Pasar a través de los valores de notificación que comienzan con un valor específico**  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  



  
## <a name="additional-references"></a>Referencias adicionales  
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
  
[Cuándo usar uno de paso o la regla de filtro de notificación](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
