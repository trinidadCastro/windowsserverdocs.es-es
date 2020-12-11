---
description: 'Más información sobre: creación de una regla para enviar una afirmación compatible con AD FS 1. x'
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: Creación de una regla para enviar una afirmación compatible con AD FS 1. x
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ee00149fca0a78c8faa4338b3aa5ba0060909703
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045663"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>Creación de una regla para enviar una afirmación compatible con AD FS 1. x

En situaciones en las que usa Servicios de federación de Active Directory (AD FS) \( AD FS \) para emitir notificaciones que recibirán los servidores de Federación que ejecutan AD FS 1,0 \( Windows Server 2003 R2 \) o AD FS 1,1 \( Windows server 2008 o Windows Server 2008 R2 \) , debe hacer lo siguiente:

-   Cree una regla que enviará un tipo de notificaciones de identificador de nombre con un formato de UPN, correo electrónico o nombre común.

-   Todas las demás notificaciones que se envían deben tener uno de los siguientes tipos de notificación:

    -   AD FS 1. Dirección de correo electrónico *x*

    -   AD FS 1. UPN *x*

    -   Nombre común

    -   Grupo

    -   Cualquier otro tipo de notificaciones que comience por https://schemas.xmlsoap.org/claims/ , como https://schemas.xmlsoap.org/claims/EmployeeID

En función de las necesidades de su organización, use uno de los procedimientos siguientes para crear un AD FS 1. notificación NameID compatible *x* :

-   Cree esta regla para emitir una demanda de ID. de nombre de AD FS 1. x mediante la **plantilla de reglas pasar a través o filtrar una notificaciones entrantes** .

-   Cree esta regla para emitir una claim de identificador de nombre de AD FS 1. x mediante la **plantilla de regla transformar una notificaciones entrantes**. Puede usar esta plantilla de reglas en situaciones en las que desee cambiar el tipo de notificaciones existente a un nuevo tipo de demanda que funcionará con AD FS 1. notificaciones *x* .

> [!NOTE]
> Para que esta regla funcione según lo previsto, asegúrese de que la relación de confianza para usuario autenticado o proveedor de notificaciones en la que va a crear esta regla se haya configurado para usar el **perfil AD FS 1,0 y 1,1**.

## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para emitir un AD FS 1. *x* notificaciones de identificador de nombre mediante la plantilla de reglas pasar a través o filtrar una notificaciones entrantes en una relación de confianza para usuario autenticado en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  \-Haga clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **pasar o filtrar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.

7.  En **tipo de notificaciones entrantes**, seleccione ID. de **nombre** en la lista.

8.  En **formato de ID. de nombre entrante**, seleccione una de las siguientes AD FS 1. *x* \- formatos de notificaciones compatibles de la lista:

    -   **UPN**

    -   **\-Correo electrónico**

    -   **Nombre común**

9. Seleccione una de las siguientes opciones, en función de las necesidades de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Pasar a través solo un valor de notificación determinado**

    -   **Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico**

    -   **Pasar solo los valores de notificaciones que empiezan con un valor específico** 
 ![ crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. Haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar** para guardar la regla.


## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para emitir un AD FS 1. notificación de ID. de nombre *x* uso de la plantilla de reglas pasar a través o filtrar una notificación entrante en una confianza de proveedor de notificaciones en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **pasar o filtrar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.

7.  En **tipo de notificaciones entrantes**, seleccione ID. de **nombre** en la lista.

8.  En **formato de ID. de nombre entrante**, seleccione una de las siguientes AD FS 1. *x* \- formatos de notificaciones compatibles de la lista:

    -   **UPN**

    -   **\-Correo electrónico**

    -   **Nombre común**

9. Seleccione una de las siguientes opciones, en función de las necesidades de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Pasar a través solo un valor de notificación determinado**

    -   **Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico**

    -   **Pasar solo los valores de notificaciones que empiezan con un valor específico** 
 ![ crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. Haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar** para guardar la regla.


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

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.

7.  En **tipo de notificaciones entrantes**, seleccione el tipo de notificaciones entrantes que desea transformar en la lista.

8.  En **tipo de notificaciones salientes**, seleccione ID. de **nombre** en la lista.

9. En **formato de ID. de nombre de salida**, seleccione una de las siguientes AD FS 1. *x* \- formatos de notificaciones compatibles de la lista:

    -   **UPN**

    -   **\-Correo electrónico**

    -   **Nombre común**

10. Seleccione una de las siguientes opciones, en función de las necesidades de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente**

    -   **Reemplazar las \- notificaciones de sufijo de correo electrónico entrante por una nueva regla de creación de sufijo de \- correo electrónico** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. Haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar** para guardar la regla.




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

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.

7.  En **tipo de notificaciones entrantes**, seleccione el tipo de notificaciones entrantes que desea transformar en la lista.

8.  En **tipo de notificaciones salientes**, seleccione ID. de **nombre** en la lista.

9. En **formato de ID. de nombre de salida**, seleccione una de las siguientes AD FS 1. *x* \- formatos de notificaciones compatibles de la lista:

    -   **UPN**

    -   **\-Correo electrónico**

    -   **Nombre común**

10. Seleccione una de las siguientes opciones, en función de las necesidades de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente**

    -   **Reemplazar las \- notificaciones de sufijo de correo electrónico entrante por una nueva regla de creación de sufijo de \- correo electrónico** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. Haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar** para guardar la regla.














## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>Para crear una regla para emitir un AD FS 1. *x* notificaciones de identificador de nombre mediante la plantilla de reglas pasar a través o filtrar una notificaciones entrantes en Windows Server 2012 R2

1.  En Administrador del servidor, haga clic en **herramientas** y, a continuación, haga clic en **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS \\ relaciones de confianza**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas** de usuario de confianza y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, en función de la confianza que esté editando y el conjunto de reglas en el que desee crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:

    -   **Reglas de transformación de aceptación**

    -   **Reglas de transformación de emisión**

    -   **Reglas de autorización de emisión**

    -   Reglas de autorización de **delegación** 
 ![ crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificaciones**, seleccione **pasar o filtrar una notificaciones entrantes** de la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.

7.  En **tipo de notificaciones entrantes**, seleccione ID. de **nombre** en la lista.

8.  En **formato de ID. de nombre entrante**, seleccione una de las siguientes AD FS 1. *x* \- formatos de notificaciones compatibles de la lista:

    -   **UPN**

    -   **\-Correo electrónico**

    -   **Nombre común**

9. Seleccione una de las siguientes opciones, en función de las necesidades de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Pasar a través solo un valor de notificación determinado**

    -   **Pasar solo los valores de notificaciones que coinciden con un valor de sufijo de correo electrónico específico**

    -   **Pasar solo los valores de notificaciones que empiezan con un valor específico** 
 ![ crear regla](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. Haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar** para guardar la regla.


## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>Para crear una regla para emitir un AD FS 1. *x* notificaciones de identificador de nombre mediante la plantilla de regla transformar una notificaciones entrantes en Windows Server 2012 R2

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

6.  En la página **configurar regla** , escriba un nombre de regla de notificaciones.

7.  En **tipo de notificaciones entrantes**, seleccione el tipo de notificaciones entrantes que desea transformar en la lista.

8.  En **tipo de notificaciones salientes**, seleccione ID. de **nombre** en la lista.

9. En **formato de ID. de nombre de salida**, seleccione una de las siguientes AD FS 1. *x* \- formatos de notificaciones compatibles de la lista:

    -   **UPN**

    -   **\-Correo electrónico**

    -   **Nombre común**

10. Seleccione una de las siguientes opciones, en función de las necesidades de su organización:

    -   **Pasar a través todos los valores de notificaciones**

    -   **Reemplazar un valor de notificación entrante por un valor de notificación saliente diferente**

    -   **Reemplazar las \- notificaciones de sufijo de correo electrónico entrante por una nueva regla de creación de sufijo de \- correo electrónico** 
 ![](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)

11. Haga clic en **Finalizar** y, a continuación, haga clic en **Aceptar** para guardar la regla.

## <a name="additional-references"></a>Referencias adicionales
[Configuración de regla de notificación](Configure-Claim-Rules.md)

[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
