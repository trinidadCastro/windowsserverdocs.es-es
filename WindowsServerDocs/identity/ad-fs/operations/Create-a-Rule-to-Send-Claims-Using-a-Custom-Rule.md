---
description: 'Más información sobre: creación de una regla para enviar notificaciones mediante una regla personalizada'
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: Crear una regla para enviar notificaciones mediante una regla personalizada
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fe34270ad7535995d85d85eb091b67b79ab78c83
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048163"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>Crear una regla para enviar notificaciones mediante una regla personalizada


Mediante el uso de la plantilla **enviar notificaciones mediante una regla personalizada** en Servicios de federación de Active Directory (AD FS) (AD FS), puede crear reglas de notificaciones personalizadas para la situación en la que una plantilla de regla estándar no satisface los requisitos de su organización. Las reglas de notificaciones personalizadas se escriben en el lenguaje de reglas de notificaciones y se deben copiar en el cuadro de texto **regla personalizada** antes de que se puedan usar en un conjunto de reglas. Para obtener información sobre cómo construir la sintaxis de una regla avanzada, vea [el rol del lenguaje de reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md).

Puede usar el procedimiento siguiente para crear una regla de notificaciones mediante el complemento de administración \- de AD FS en.

La pertenencia al grupo **administradores**, o equivalente, en el equipo local es el requisito mínimo para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una demanda entrante en una relación de confianza para usuario autenticado en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  \-Haga clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)

6.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla. En **regla personalizada**, escriba o pegue la sintaxis del lenguaje de reglas de notificaciones que desea para esta regla.
![crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)

7.  Haga clic en **Finalizar**

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para pasar a través o filtrar una notificación entrante en una confianza de proveedor de notificaciones en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)

6.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla. En **regla personalizada**, escriba o pegue la sintaxis del lenguaje de reglas de notificaciones que desea para esta regla.
![crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)

7.  Haga clic en **Finalizar**

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.



















## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>Para crear una regla para enviar notificaciones mediante una notificación personalizada en Windows Server 2012 R2

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

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **enviar notificaciones mediante una regla personalizada** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)

6.  En la página **configurar regla** , en **nombre de la regla de notificaciones**, escriba el nombre para mostrar de esta regla. En **regla personalizada**, escriba o pegue la sintaxis del lenguaje de reglas de notificaciones que desea para esta regla.
![crear regla](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)

7.  Haga clic en **Finalizar**

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

## <a name="additional-references"></a>Referencias adicionales
[Configuración de regla de notificación](Configure-Claim-Rules.md)

[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
