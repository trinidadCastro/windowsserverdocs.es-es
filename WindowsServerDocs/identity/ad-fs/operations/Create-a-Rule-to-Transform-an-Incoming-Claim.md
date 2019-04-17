---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: "Crear una regla para transformar una notificación entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Crear una regla para transformar una notificación entrante

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Mediante el uso de la **transformar una notificación entrante** plantilla de reglas en los servicios de federación de Active Directory \(AD FS\), puedes seleccionar una notificación entrante, cambiar su tipo de notificación y cambiar su valor de la notificación. Por ejemplo, puedes usar esta plantilla de regla para crear una regla que envía una solicitud de función con el mismo valor de la notificación de una notificación entrante de grupo. También puedes usar esta regla para enviar la notificación de un grupo con un valor de notificaciones de compradores cuando hay una notificación de grupo entrante con un valor de administradores de TI, o puedes enviar solo nombre principal de usuario \(UPN\) dice que finalizan con @fabrikam.  
  
Puedes usar el siguiente procedimiento para crear una regla de notificación con la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en una confianza del fabricante de confiar en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **transformar una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla. En **tipo de notificación entrante**, selecciona una reclamación escriba la lista. En **tipo de notificación saliente**, selecciona un tipo de notificación de la lista y, a continuación, selecciona una de las siguientes opciones, que depende de los requisitos de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**  
  
    -   **Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo**  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.
  
> [!NOTE]  
> Si estás configurando el escenario de Control de acceso dinámico que usa AD FS\ emitido reclamaciones, crear una regla de transformación de la confianza del proveedor de notificaciones y en **tipo de notificación entrante**, escribe el nombre de la notificación entrante o, si se ha creado previamente una descripción de la notificación, selecciona en la lista. Segundo, en **tipo de notificación saliente**, selecciona la dirección URL de notificación que quieras y, a continuación, crear una regla de la transformación en la confianza de terceros confianza para emitir la reclamación de dispositivo.  
>   
> Para obtener más información acerca de los escenarios de Control de acceso dinámico, consulta [plan de contenido de Control de acceso dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o [reclamaciones de usar AD DS con AD FS ](https://technet.microsoft.com/library/hh831504.aspx). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en un proveedor de notificaciones de confianza en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **transformar una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla. En **tipo de notificación entrante**, selecciona una reclamación escriba la lista. En **tipo de notificación saliente**, selecciona un tipo de notificación de la lista y, a continuación, selecciona una de las siguientes opciones, que depende de los requisitos de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**  
  
    -   **Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo**  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Haz clic en el **finalizar** botón.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

> [!NOTE]  
> Si estás configurando el escenario de Control de acceso dinámico que usa AD FS\ emitido reclamaciones, crear una regla de transformación de la confianza del proveedor de notificaciones y en **tipo de notificación entrante**, escribe el nombre de la notificación entrante o, si se ha creado previamente una descripción de la notificación, selecciona en la lista. Segundo, en **tipo de notificación saliente**, selecciona la dirección URL de notificación que quieras y, a continuación, crear una regla de la transformación en la confianza de terceros confianza para emitir la reclamación de dispositivo.  
>   
> Para obtener más información acerca de los escenarios de Control de acceso dinámico, consulta [plan de contenido de Control de acceso dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o [reclamaciones de usar AD DS con AD FS ](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Para crear una regla para transformar una notificación entrante en Windows Server 2012 R2 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **AD FS administración **.  
  
2.  En el árbol de consola, en **AD FS\\Trust relaciones**, haz clic en **reclamaciones proveedor confía** o **confiar confía en las partes**y, a continuación, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  En la **editar reglas de notificación** cuadro de diálogo, selecciona una las fichas siguientes, que depende de la confianza de que está editando y en la regla que establece quieran crear esta regla y, a continuación, haz clic en **Agregar regla** para iniciar el Asistente de regla que está asociado con ese conjunto de reglas:  
  
    -   **Reglas de transformación de aceptación**  
  
    -   **Reglas de transformación de emisión**  
  
    -   **Reglas de autorización de emisión**  
  
    -   **Reglas de autorización de delegación**  
![Crear la regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **transformar una notificación entrante** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla. En **tipo de notificación entrante**, selecciona una reclamación escriba la lista. En **tipo de notificación saliente**, selecciona un tipo de notificación de la lista y, a continuación, selecciona una de las siguientes opciones, que depende de los requisitos de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**  
  
    -   **Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo**  
![Crear la regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Si estás configurando el escenario de Control de acceso dinámico que usa AD FS\ emitido reclamaciones, crear una regla de transformación de la confianza del proveedor de notificaciones y en **tipo de notificación entrante**, escribe el nombre de la notificación entrante o, si se ha creado previamente una descripción de la notificación, selecciona en la lista. Segundo, en **tipo de notificación saliente**, selecciona la dirección URL de notificación que quieras y, a continuación, crear una regla de la transformación en la confianza de terceros confianza para emitir la reclamación de dispositivo.  
>   
> Para obtener más información acerca de los escenarios de Control de acceso dinámico, consulta [plan de contenido de Control de acceso dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o [reclamaciones de usar AD DS con AD FS ](https://technet.microsoft.com/library/hh831504.aspx).  
  
7.  Haz clic en **finalizar**.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
