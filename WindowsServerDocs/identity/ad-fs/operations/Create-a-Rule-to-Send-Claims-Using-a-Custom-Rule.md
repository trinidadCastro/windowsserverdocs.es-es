---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Crear una regla para enviar notificaciones usando una regla personalizada
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f0eef89e651585d48ba87d14bc782efa49087669
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Crear una regla para enviar notificaciones usando una regla personalizada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Mediante el uso de la **enviar notificaciones usando una regla personalizada** plantilla en los servicios de federación de Active Directory (AD FS), puedes crear reglas de notificación personalizada para la situación en la que una plantilla de regla estándar no satisface los requisitos de la organización. Las reglas de notificación personalizada se escriben en el idioma de la regla de notificación y, a continuación, se debe copiar en el **regla personalizada** cuadro de texto antes de que pueden usarse en un conjunto de reglas. Para obtener información acerca de cómo construir la sintaxis para una regla avanzada, consulta [el rol del lenguaje de regla reclamación](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).  
  
Puedes usar el siguiente procedimiento para crear una regla de notificación mediante el uso de la administración de AD FS en snap\.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para atravesar o filtrar una notificación entrante en una confianza del fabricante de confiar en Windows Server 2016 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla. En **regla personalizada**, escribe o pega la sintaxis de lenguaje de regla de reclamación que desees para esta regla.  
![Crear la regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Haz clic en **finalizar**.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para atravesar o filtrar una notificación entrante en un proveedor de notificaciones de confianza en Windows Server 2016 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla. En **regla personalizada**, escribe o pega la sintaxis de lenguaje de regla de reclamación que desees para esta regla.  
![Crear la regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  Haz clic en **finalizar**.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar notificaciones mediante una notificación personalizada en Windows Server 2012 R2 
  
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
  
5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **enviar notificaciones usando una regla personalizada** en la lista y, a continuación, haz clic en **siguiente**.  
![Crear la regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  En la **configurar regla** página, debajo **nombre de la regla de Reclamación**, escribe el nombre para mostrar para esta regla. En **regla personalizada**, escribe o pega la sintaxis de lenguaje de regla de reclamación que desees para esta regla.  
![Crear la regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  Haz clic en **finalizar**.  
  
8.  En la **editar reglas de notificación** cuadro de diálogo, haz clic en **Aceptar** guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
