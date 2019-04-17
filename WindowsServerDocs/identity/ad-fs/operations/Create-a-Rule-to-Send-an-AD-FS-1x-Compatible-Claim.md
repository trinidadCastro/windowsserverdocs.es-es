---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: "Crear una regla para transformar una notificación entrante"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Crear una regla para enviar una notificación de AD FS 1.x Compatible

>Se aplica a: Windows Server 2016, Windows Server 2012 R2


En situaciones en las que estás usando los servicios de federación de Active Directory \(AD FS\) para emitir dice que será recibido por los servidores federación de AD FS 1.0 \ (Windows Server 2003 R2\) o AD FS 1.1 \ (Windows Server 2008 o Windows Server 2008 R2\), debes hacer lo siguiente:  
  
-   Crear una regla que te enviará un tipo de notificación de nombre identificador con un formato de UPN, correo electrónico o nombre común.  
  
-   Todas las demás reclamaciones que se envían deben tener uno de los siguientes tipos de notificación:  
  
    -   AD FS 1. *x* dirección de correo electrónico  
  
    -   AD FS 1. *x* UPN  
  
    -   Nombre común  
  
    -   Grupo  
  
    -   Cualquier otro tipo de notificación que comienza con https://schemas.xmlsoap.org/claims/ como https://schemas.xmlsoap.org/claims/EmployeeID  
  
Según las necesidades de la organización, usa uno de los siguientes procedimientos para crear un 1 de AD FS. *x* Reclamación de NameID compatible:  
  
-   Crear esta regla a problema una AD FS 1.x nombre identificador reclamación usando el **paso a través o una plantilla de notificación entrante de regla de filtro**  
  
-   Crear esta regla a problema una AD FS 1.x nombre identificador reclamación usando el **transformar una plantilla de notificación entrante de regla**. Puedes usar esta plantilla de regla en situaciones en que quieres cambiar el tipo de notificación existente a un nuevo tipo de notificación que funcione con AD FS 1. *x* reclamaciones.  
  
> [!NOTE]  
> Para que esta regla para que funcione según lo esperado, asegúrate de que el usuario de confianza de terceros de confianza o de confianza de proveedor de notificaciones que estás creando esta regla se ha configurado para usar la **perfil de AD FS 1.0 y 1.1**. 




## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para emitir una AD FS 1. *x* filtrar una plantilla de regla de notificación entrante en una confianza del fabricante de confiar en Windows Server 2016 o reclamar con el paso a través de Id. de nombre 

1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **confiar confía en las partes **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **Editar directiva de emisión de Reclamación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En la **Editar directiva de emisión de Reclamación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para la regla. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **paso a través o una notificación entrante de filtro** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona **nombre identificador** en la lista.  
  
8.  En **formato de identificador de nombre de entrante**, selecciona una de la siguiente AD FS 1. *x*\-compatible los formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\ correo**  
  
    -   **Nombre común**  
  
9. Selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Pasar solo un determinado valor de notificación**  
  
    -   **Pasar solo valores de las notificaciones que coinciden con un valor de sufijo de correo electrónico específicas**  
  
    -   **Pasar a través de los valores de notificación que comienzan con un valor específico**  
![Crear la regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para emitir una AD FS 1. *x* filtrar una plantilla de regla de notificación entrante en un proveedor de notificaciones de confianza en Windows Server 2016 o reclamar con el paso a través de Id. de nombre 
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**.  
  
2.  En el árbol de consola, en **AD FS**, haz clic en **reclamaciones proveedor confía **. 
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ y haga clic en la confianza seleccionada y, a continuación, haz clic en **editar reglas de notificación **.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En la **editar reglas de notificación** cuadro de diálogo **aceptación transformar reglas** haga clic en **Agregar regla** para iniciar el Asistente para la regla.
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En la **Seleccionar plantilla de regla** página, debajo **plantilla de regla de Reclamación**, selecciona **paso a través o una notificación entrante de filtro** en la lista y, a continuación, haz clic en **siguiente **.  
![Crear la regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona **nombre identificador** en la lista.  
  
8.  En **formato de identificador de nombre de entrante**, selecciona una de la siguiente AD FS 1. *x*\-compatible los formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\ correo**  
  
    -   **Nombre común**  
  
9. Selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Pasar solo un determinado valor de notificación**  
  
    -   **Pasar solo valores de las notificaciones que coinciden con un valor de sufijo de correo electrónico específicas**  
  
    -   **Pasar a través de los valores de notificación que comienzan con un valor específico**  
![Crear la regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  

  

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

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona el tipo de notificación entrante que desea transformar en la lista.  
  
8.  En **tipo de notificación saliente**, selecciona **nombre identificador** en la lista.  
  
9. En **formato de identificador de nombre saliente**, selecciona una de la siguiente AD FS 1. *x*\-compatible los formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\ correo**  
  
    -   **Nombre común**  
  
10. Selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**  
  
    -   **Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo**  
![Crear la regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  

  


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

6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona el tipo de notificación entrante que desea transformar en la lista.  
  
8.  En **tipo de notificación saliente**, selecciona **nombre identificador** en la lista.  
  
9. En **formato de identificador de nombre saliente**, selecciona una de la siguiente AD FS 1. *x*\-compatible los formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\ correo**  
  
    -   **Nombre común**  
  
10. Selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**  
  
    -   **Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo**  
![Crear la regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  













  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Para crear una regla para emitir una AD FS 1. *x* filtrar una plantilla de regla de notificación entrante en Windows Server 2012 R2 o reclamar con el paso a través de Id. de nombre
  
1.  En el administrador del servidor, haz clic en **herramientas**y, a continuación, haz clic en **AD FS administración **.  
  
2.  En el árbol de consola, en **AD FS\\Trust relaciones**, haz clic en **reclamaciones proveedor confía** o **confiar confía en las partes**y, a continuación, haz clic en una relación de confianza específicas en la lista que quieras para crear esta regla.  
  
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
  
6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona **nombre identificador** en la lista.  
  
8.  En **formato de identificador de nombre de entrante**, selecciona una de la siguiente AD FS 1. *x*\-compatible los formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\ correo**  
  
    -   **Nombre común**  
  
9. Selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Pasar solo un determinado valor de notificación**  
  
    -   **Pasar solo valores de las notificaciones que coinciden con un valor de sufijo de correo electrónico específicas**  
  
    -   **Pasar a través de los valores de notificación que comienzan con un valor específico**  
![Crear la regla](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para crear una regla para emitir una AD FS 1. *x* Reclamación de Id. de nombre con una plantilla de notificación entrante de regla de la transformación en Windows Server 2012 R2  
  
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
  
6.  En la **configurar regla**, escriba un nombre de regla de notificación.  
  
7.  En **tipo de notificación entrante**, selecciona el tipo de notificación entrante que desea transformar en la lista.  
  
8.  En **tipo de notificación saliente**, selecciona **nombre identificador** en la lista.  
  
9. En **formato de identificador de nombre saliente**, selecciona una de la siguiente AD FS 1. *x*\-compatible los formatos de la lista de notificación:  
  
    -   **UPN**  
  
    -   **E\ correo**  
  
    -   **Nombre común**  
  
10. Selecciona una de las siguientes opciones, según las necesidades de la organización:  
  
    -   **Pasar por todos los valores de las notificaciones**  
  
    -   **Reemplazar un valor de notificación entrante con un valor distinto de Reclamación saliente**  
  
    -   **Reemplazar reclamaciones de sufijo e\ correo entrante con un sufijo e\ correo nuevo**  
![Crear la regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. Haz clic en **finalizar**y, a continuación, haz clic en **Aceptar** guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Crear reglas de notificación para un usuario de confianza](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de solicitud de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[La función de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
