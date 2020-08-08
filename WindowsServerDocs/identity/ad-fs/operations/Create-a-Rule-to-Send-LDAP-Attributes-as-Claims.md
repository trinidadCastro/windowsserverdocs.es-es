---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: Crear una regla para enviar atributos LDAP como notificaciones
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 938e82f5318bc374fd3f89bf5354c2fc447a9723
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967212"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>Crear una regla para enviar atributos LDAP como notificaciones


Si usa la plantilla de regla enviar atributos LDAP como notificaciones en Servicios de federación de Active Directory (AD FS) \( AD FS \) , puede crear una regla que seleccionará los atributos de un almacén de atributos LDAP de Protocolo ligero de acceso a directorios \( , como \) Active Directory, para enviarlos como notificaciones al usuario de confianza. Por ejemplo, puede usar esta plantilla de reglas para crear una regla de envío de atributos LDAP como notificaciones que extraerá los valores de atributo para los usuarios autenticados de los atributos **displayName** y **telephoneNumber** Active Directory y, a continuación, enviará esos valores como dos notificaciones salientes diferentes.

También puede usar esta regla para enviar todas las pertenencias a grupos del usuario. Si desea enviar solo pertenencias a grupos individuales, use la plantilla de regla de envío de pertenencia a grupos como una notificación. Puede usar el procedimiento siguiente para crear una regla de notificaciones con el complemento \- de administración de AD FS en.

La pertenencia al grupo **Administradores** o equivalente en el equipo local es el requisito mínimo necesario para completar este procedimiento.  Revise los detalles sobre el uso de las cuentas y pertenencias a grupos adecuadas en [grupos predeterminados locales y de dominio](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>Para crear una regla para enviar atributos LDAP como notificaciones para una relación de confianza para usuario autenticado en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en relaciones de confianza para usuario **autenticado**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)

3.  \-Haga clic con el botón derecho en la confianza seleccionada y luego haga clic en **Editar Directiva de emisión de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)

4.  En el cuadro de diálogo **Editar Directiva de emisión de notificaciones** , en **reglas de transformación de emisión** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **Enviar atributos LDAP como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, seleccione el **almacén de atributos**y, a continuación, seleccione el atributo LDAP y asígnelo al tipo de notificaciones salientes.
![crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)

7.  Haga clic en el botón **Finalizar**.

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>Para crear una regla para enviar atributos LDAP como notificaciones para una confianza de proveedor de notificaciones en Windows Server 2016

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **AD FS**, haga clic en **confianzas de proveedor de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , en **reglas de transformación de aceptación** , haga clic en **Agregar regla** para iniciar el Asistente para reglas.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **Enviar atributos LDAP como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, seleccione el **almacén de atributos**y, a continuación, seleccione el atributo LDAP y asígnelo al tipo de notificaciones salientes.
![crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)

7.  Haga clic en el botón **Finalizar**.

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.



## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>Para crear una regla para enviar atributos LDAP como notificaciones para Windows Server 2012 R2

1.  En el Administrador del servidor, haga clic en **Herramientas** y, luego, seleccione **Administración de AD FS**.

2.  En el árbol de consola, en **relaciones de \\ confianza de ad FSAD FS**, haga clic en **confianzas de proveedor de notificaciones** o en **confianzas para usuario autenticado**y, a continuación, haga clic en una confianza específica en la lista en la que desea crear esta regla.

3.  Haga clic con el botón secundario \- en la confianza seleccionada y, a continuación, haga clic en **editar reglas de notificaciones**.
![crear regla](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)

4.  En el cuadro de diálogo **editar reglas de notificaciones** , seleccione una de las siguientes pestañas, en función de la confianza que esté editando y el conjunto de reglas en el que desee crear esta regla y, a continuación, haga clic en **Agregar regla** para iniciar el Asistente para reglas que está asociado con ese conjunto de reglas:

    -   **Reglas de transformación de aceptación**

    -   **Reglas de transformación de emisión**

    -   **Reglas de autorización de emisión**

    -   Reglas de autorización de **delegación** 
 ![ crear regla](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  En la **página Seleccionar plantilla de regla** , en **plantilla de regla de notificación**, seleccione **Enviar atributos LDAP como notificaciones** en la lista y, a continuación, haga clic en **siguiente**.
![crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)

6.  En la página **configurar regla** , en nombre de la **regla de notificaciones** , escriba el nombre para mostrar de esta regla, en **almacén de atributos** , seleccione **Active Directory**y, en **asignación de atributos LDAP a tipos de notificaciones salientes** , seleccione el **atributo LDAP** deseado y los tipos de **tipo de notificaciones salientes** correspondientes en las listas desplegables \- .

    Tiene que seleccionar un nuevo atributo LDAP y un par de tipos de notificaciones salientes en una fila diferente para cada Active Directory atributo para el que desea emitir una demanda como parte de esta regla.
![crear regla](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)
7.  Haga clic en el botón **Finalizar**.

8.  En el cuadro de diálogo **editar reglas de notificaciones** , haga clic en **Aceptar** para guardar la regla.

## <a name="additional-references"></a>Referencias adicionales
[Configuración de regla de notificación](Configure-Claim-Rules.md)

[Lista de comprobación: crear reglas de notificación para una relación de confianza para usuario autenticado](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913578(v=ws.11))

[Lista de comprobación: Lista de comprobación: crear reglas de notificación para confianza de proveedores de notificaciones](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913564(v=ws.11))

[Cuándo usar una regla de notificación de autorización](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)

[El papel de las notificaciones](../../ad-fs/technical-reference/The-Role-of-Claims.md)

[El papel de las reglas de notificaciones](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)
