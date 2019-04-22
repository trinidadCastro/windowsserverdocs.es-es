---
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Crear una regla para transformar una notificación entrante
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e982b7608f7602268657ceae74f641bbaaaec939
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816686"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Crear una regla para transformar una notificación entrante

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Mediante el uso de la **transformar una notificación entrante** plantilla de regla en Active Directory Federation Services \(AD FS\), puede seleccionar una notificación entrante, su tipo de notificación y su valor de notificación. Por ejemplo, puede usar esta plantilla de regla para crear una regla que envía una notificación de rol con el mismo valor de notificación de una notificación de grupo entrante. También puede usar esta regla para enviar la notificación de un grupo con un valor de notificación del comprador cuando hay una notificación de grupo entrante con un valor de administradores, o puede enviar solo nombre principal de usuario \(UPN\) notificaciones que terminan con @fabrikam.  
  
Puede usar el procedimiento siguiente para crear una regla de notificación con el complemento Administración de AD FS\-en.  
  
Pertenencia a **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en una confianza de Windows Server 2016 

1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **autenticado**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **Editar directiva de emisión de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  En el **Editar directiva de emisión de notificación** cuadro de diálogo **reglas de transformación de emisión** haga clic en **Agregar regla** para iniciar el Asistente para reglas. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **transformar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla. En **tipo de notificación entrante**, seleccione un tipo de notificación en la lista. En **tipo de notificación saliente**, seleccione un tipo de notificación en la lista y, a continuación, seleccione una de las opciones siguientes, que depende de los requisitos de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**  
  
    -   **Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico**  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)   

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.
  
> [!NOTE]  
> Si está configurando el escenario de Control de acceso dinámico que usa AD FS\-emite notificaciones, crear una regla de transformación en la confianza de proveedor de notificaciones y en **tipo de notificación entrante**, escriba el nombre de la notificación entrante, o bien, si un Descripción de notificación creada anteriormente, selecciónelo en la lista. Segundo, en **tipo de notificación saliente**, seleccione la dirección URL de notificación que desee y, a continuación, cree una regla de transformación en la relación de confianza para emitir la notificación de dispositivo.  
>   
> Para obtener más información acerca de los escenarios de Control de acceso dinámico, consulte [Guía de contenido de Control de acceso dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o [mediante notificaciones de AD DS con AD FS](https://technet.microsoft.com/library/hh831504.aspx). 

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en una confianza del proveedor de notificaciones en Windows Server 2016 
  
1.  En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**.  
  
2.  En el árbol de consola, bajo **AD FS**, haga clic en **confianzas de proveedor de notificaciones**. 
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Derecha\-haga clic en la relación de confianza seleccionada y, a continuación, haga clic en **editar reglas de notificación**.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  En el **editar reglas de notificación** cuadro de diálogo **reglas de transformación de aceptación** haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![Crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **transformar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla. En **tipo de notificación entrante**, seleccione un tipo de notificación en la lista. En **tipo de notificación saliente**, seleccione un tipo de notificación en la lista y, a continuación, seleccione una de las opciones siguientes, que depende de los requisitos de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**  
  
    -   **Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico**  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)       

7.  Haga clic en el **finalizar** botón.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

> [!NOTE]  
> Si está configurando el escenario de Control de acceso dinámico que usa AD FS\-emite notificaciones, crear una regla de transformación en la confianza de proveedor de notificaciones y en **tipo de notificación entrante**, escriba el nombre de la notificación entrante, o bien, si un Descripción de notificación creada anteriormente, selecciónelo en la lista. Segundo, en **tipo de notificación saliente**, seleccione la dirección URL de notificación que desee y, a continuación, cree una regla de transformación en la relación de confianza para emitir la notificación de dispositivo.  
>   
> Para obtener más información acerca de los escenarios de Control de acceso dinámico, consulte [Guía de contenido de Control de acceso dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o [mediante notificaciones de AD DS con AD FS](https://technet.microsoft.com/library/hh831504.aspx).   
  
## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Para crear una regla para transformar una notificación entrante en Windows Server 2012 R2 
  
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
  
5.  En el **Seleccionar plantilla de regla** página, en **plantilla de regla de notificación**, seleccione **transformar una notificación entrante** en la lista y, a continuación, haga clic en **siguiente**.  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   

6.  En el **configurar regla** página, en **nombre de la regla de notificación**, escriba el nombre para mostrar para esta regla. En **tipo de notificación entrante**, seleccione un tipo de notificación en la lista. En **tipo de notificación saliente**, seleccione un tipo de notificación en la lista y, a continuación, seleccione una de las opciones siguientes, que depende de los requisitos de su organización:  
  
    -   **Pasar a través de todos los valores de notificación**  
  
    -   **Reemplazar un valor de notificación entrante con un valor de notificación saliente diferente**  
  
    -   **Reemplace entrante e\-con una nueva e las notificaciones de sufijo de correo\-sufijo de correo electrónico**  
![Crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)  

> [!NOTE]  
> Si está configurando el escenario de Control de acceso dinámico que usa AD FS\-emite notificaciones, crear una regla de transformación en la confianza de proveedor de notificaciones y en **tipo de notificación entrante**, escriba el nombre de la notificación entrante, o bien, si un Descripción de notificación creada anteriormente, selecciónelo en la lista. Segundo, en **tipo de notificación saliente**, seleccione la dirección URL de notificación que desee y, a continuación, cree una regla de transformación en la relación de confianza para emitir la notificación de dispositivo.  
>   
> Para obtener más información acerca de los escenarios de Control de acceso dinámico, consulte [Guía de contenido de Control de acceso dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o [mediante notificaciones de AD DS con AD FS](https://technet.microsoft.com/library/hh831504.aspx).  
  
7.  Haga clic en **Finalizar**.  
  
8.  En el **editar reglas de notificación** cuadro de diálogo, haga clic en **Aceptar** para guardar la regla.  

## <a name="additional-references"></a>Referencias adicionales 
[Configurar reglas de notificación](Configure-Claim-Rules.md)  
 
[Lista de comprobación: Creación de reglas de notificación para una relación de confianza para usuario autenticado](https://technet.microsoft.com/library/ee913578.aspx)  

[Lista de comprobación: Crear reglas de notificación para un proveedor de notificaciones de confianza](https://technet.microsoft.com/library/ee913564.aspx)  
  
[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[El rol de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[La función de reglas de notificación](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
