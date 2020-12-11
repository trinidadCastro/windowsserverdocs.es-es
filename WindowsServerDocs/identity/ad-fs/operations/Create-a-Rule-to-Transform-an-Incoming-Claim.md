---
description: 'Más información acerca de: creación de una regla para transformar una demanda entrante'
ms.assetid: ef83960f-d2cf-441f-b2b6-d97822ec7149
title: Crear una regla para transformar una notificación entrante
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7dbb6ca59de0d7a9c3e696084409323e743b27f1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040043"
---
# <a name="create-a-rule-to-transform-an-incoming-claim"></a>Crear una regla para transformar una notificación entrante


Mediante el uso de la plantilla de reglas de **transformación de notificaciones entrantes** en servicios de Federación de Active Directory (AD FS) \( AD FS \) , puede seleccionar una notificaciones entrantes, cambiar su tipo de notificaciones y cambiar su valor de notificaciones. Por ejemplo, puede usar esta plantilla de reglas para crear una regla que envíe una notificaciones de rol con el mismo valor de notificaciones de una demanda de grupo entrante. También puede usar esta regla para enviar una notificación de grupo con un valor de notificación de compradores cuando haya una notificación de grupo entrante con un valor de administradores, o bien puede enviar solo notificaciones de UPN de nombre principal de usuario \( \) que terminen con @fabrikam .

Puede usar el procedimiento siguiente para crear una regla de notificaciones con el complemento \- de administración de AD FS en.

La pertenencia al grupo **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para transformar una demanda entrante en una relación de confianza para usuario autenticado en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  \-Haga clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **transformar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla. En **tipo de notificaciones entrantes**, seleccione un tipo de demanda en la lista. En **tipo de notificaciones salientes**, seleccione un tipo de notificaciones en la lista y, a continuación, seleccione una de las siguientes opciones, que depende de los requisitos de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente**

    -   **Reemplazar las \- notificaciones de sufijo de correo electrónico entrante por una nueva regla de creación de sufijo de \- correo electrónico** 
 ![](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)

7.  Haga clic en el botón **Finalizar**.

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

> [!NOTE]
> Si está configurando el escenario de Access Control dinámico que usa AD FS \- notificaciones emitidas, cree primero una regla de transformación en la confianza del proveedor de notificaciones y, en **tipo de notificación entrante**, escriba el nombre de la notificación entrante, o bien, si se ha creado previamente una descripción de notificación, selecciónela en la lista. En segundo lugar, en **tipo de notificaciones salientes**, seleccione la dirección URL de notificaciones que desee y, a continuación, cree una regla de transformación en la relación de confianza para usuario autenticado para emitir la demanda del dispositivo.
>
> Para obtener más información acerca de los escenarios de Access Control dinámicos, consulte el [mapa de ruta de contenido de Access control dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o el [uso de notificaciones de AD DS con AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831504(v=ws.11)).

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para transformar una notificación entrante en una confianza de proveedor de notificaciones en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **transformar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla. En **tipo de notificaciones entrantes**, seleccione un tipo de demanda en la lista. En **tipo de notificaciones salientes**, seleccione un tipo de notificaciones en la lista y, a continuación, seleccione una de las siguientes opciones, que depende de los requisitos de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente**

    -   **Reemplazar las \- notificaciones de sufijo de correo electrónico entrante por una nueva regla de creación de sufijo de \- correo electrónico** 
 ![](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform4.PNG)

7.  Haga clic en el botón **Finalizar**.

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

> [!NOTE]
> Si está configurando el escenario de Access Control dinámico que usa AD FS \- notificaciones emitidas, cree primero una regla de transformación en la confianza del proveedor de notificaciones y, en **tipo de notificación entrante**, escriba el nombre de la notificación entrante, o bien, si se ha creado previamente una descripción de notificación, selecciónela en la lista. En segundo lugar, en **tipo de notificaciones salientes**, seleccione la dirección URL de notificaciones que desee y, a continuación, cree una regla de transformación en la relación de confianza para usuario autenticado para emitir la demanda del dispositivo.
>
> Para obtener más información acerca de los escenarios de Access Control dinámicos, consulte el [mapa de ruta de contenido de Access control dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o el [uso de notificaciones de AD DS con AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831504(v=ws.11)).

## <a name="to-create-a-rule-to-transform-an-incoming-claim-in-windows-server-2012-r2"></a>Para crear una regla para transformar una demanda entrante en Windows Server 2012 R2

1.  En Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS \\ relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas** de usuario de confianza y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, que depende de la confianza que está editando y en qué conjunto de reglas desea crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:

    -   **Reglas de transformación de aceptación**

    -   **Reglas de transformación de emisión**

    -   **Reglas de autorización de emisión**

    -   Reglas de autorización de **delegación** 
 ![ crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **transformar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)

6.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla. En **tipo de notificaciones entrantes**, seleccione un tipo de demanda en la lista. En **tipo de notificaciones salientes**, seleccione un tipo de notificaciones en la lista y, a continuación, seleccione una de las siguientes opciones, que depende de los requisitos de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente**

    -   **Reemplazar las \- notificaciones de sufijo de correo electrónico entrante por una nueva regla de creación de sufijo de \- correo electrónico** 
 ![](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform2.PNG)

> [!NOTE]
> Si está configurando el escenario de Access Control dinámico que usa AD FS \- notificaciones emitidas, cree primero una regla de transformación en la confianza del proveedor de notificaciones y, en **tipo de notificación entrante**, escriba el nombre de la notificación entrante, o bien, si se ha creado previamente una descripción de notificación, selecciónela en la lista. En segundo lugar, en **tipo de notificaciones salientes**, seleccione la dirección URL de notificaciones que desee y, a continuación, cree una regla de transformación en la relación de confianza para usuario autenticado para emitir la demanda del dispositivo.
>
> Para obtener más información acerca de los escenarios de Access Control dinámicos, consulte el [mapa de ruta de contenido de Access control dinámico](../../solution-guides/dynamic-access-control--scenario-overview.md) o el [uso de notificaciones de AD DS con AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831504(v=ws.11)).

7. Haga clic en **Finalizar**

8. En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

## <a name="additional-references"></a>Referencias adicionales
[Configuración de regla de notificación](Configure-Claim-Rules.md)

[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
