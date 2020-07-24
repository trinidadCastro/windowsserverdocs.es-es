---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Directivas de Control de acceso en AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9b85e6e10f8df3ec4d2c70e9aff687f264e055a8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962827"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Directivas de Access Control en Windows Server 2016 AD FS

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Access Control plantillas de directiva en AD FS  
Servicios de federación de Active Directory (AD FS) admite ahora el uso de plantillas de directiva de control de acceso.  Mediante el uso de plantillas de directiva de control de acceso, un administrador puede aplicar la configuración de la Directiva mediante la asignación de la plantilla de directiva a un grupo de usuarios de confianza (RPs). El administrador también puede realizar actualizaciones en la plantilla de directiva y los cambios se aplicarán automáticamente a los usuarios de confianza si no se necesita ninguna interacción del usuario.  
  
## <a name="what-are-access-control-policy-templates"></a>¿Qué son las plantillas de directiva de Access Control?  
La canalización de AD FS Core para el procesamiento de directivas tiene tres fases: autenticación, autorización y emisión de notificaciones. Actualmente, los administradores de AD FS tienen que configurar una directiva para cada una de estas fases por separado.  Esto también implica comprender las implicaciones de estas directivas y si estas directivas tienen entre dependencias. Además, los administradores deben conocer el lenguaje de reglas de notificaciones y crear reglas personalizadas para habilitar alguna directiva sencilla o común (por ejemplo, bloquear el acceso externo).  
  
¿Qué son las plantillas de directiva de control de acceso? reemplace este antiguo modelo en el que los administradores tienen que configurar reglas de autorización de emisión mediante el lenguaje de notificaciones.  Los antiguos cmdlets de PowerShell de reglas de autorización de emisión se siguen aplicando pero es mutuamente excluyente del nuevo modelo. Los administradores pueden elegir entre usar el nuevo modelo o el antiguo.  El nuevo modelo permite a los administradores controlar cuándo se debe conceder acceso, incluida la aplicación de multi-factor Authentication.  
  
Las plantillas de directiva de control de acceso usan un modelo de permiso.  Esto significa que, de forma predeterminada, nadie tiene acceso y el acceso se debe conceder explícitamente.  Sin embargo, esto no es solo una concesión de todo o nada.  Los administradores pueden agregar excepciones a la regla de permiso.  Por ejemplo, es posible que un administrador quiera conceder acceso basado en una red específica seleccionando esta opción y especificando el intervalo de direcciones IP.  Pero el administrador puede Agregar una excepción y, por ejemplo, el administrador puede Agregar una excepción de una red específica y especificar ese intervalo de direcciones IP.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Plantillas de directiva de control de acceso integradas y plantillas de directiva de control de acceso personalizadas  
AD FS incluye varias plantillas de directiva de control de acceso integradas.  Estos tienen como destino algunos escenarios comunes que tienen el mismo conjunto de requisitos de Directiva, por ejemplo, la Directiva de acceso de cliente para Office 365.  Estas plantillas no se pueden modificar.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Para proporcionar una mayor flexibilidad para satisfacer sus necesidades empresariales, los administradores pueden crear sus propias plantillas de directiva de acceso.  Estos se pueden modificar después de la creación y los cambios en la plantilla de directiva personalizada se aplicarán a todos los RPs que están controlados por esas plantillas de directiva.  Para agregar una plantilla de directiva personalizada, simplemente haga clic en agregar Directiva de Access Control desde la administración de AD FS.  
  
Para crear una plantilla de Directiva, un administrador debe especificar primero en qué condiciones se autorizará una solicitud para la emisión y/o delegación de tokens. Las opciones de condición y acción se muestran en la tabla siguiente.   El administrador puede seguir configurando las condiciones en negrita con valores diferentes o nuevos. El administrador también puede especificar excepciones si hay alguna. Cuando se cumple una condición, una acción de permiso no se desencadenará si se especifica una excepción y la solicitud entrante coincide con la condición especificada en la excepción.  
  
|Permitir a los usuarios|Except| 
| --- | --- | 
 |Desde una red **específica**|Desde una red **específica**<p>De grupos **específicos**<p>Desde dispositivos con niveles de confianza **específicos**<p>Con notificaciones **específicas** en la solicitud|  
|De grupos **específicos**|Desde una red **específica**<p>De grupos **específicos**<p>Desde dispositivos con niveles de confianza **específicos**<p>Con notificaciones **específicas** en la solicitud|  
|Desde dispositivos con niveles de confianza **específicos**|Desde una red **específica**<p>De grupos **específicos**<p>Desde dispositivos con niveles de confianza **específicos**<p>Con notificaciones **específicas** en la solicitud|  
|Con notificaciones **específicas** en la solicitud|Desde una red **específica**<p>De grupos **específicos**<p>Desde dispositivos con niveles de confianza **específicos**<p>Con notificaciones **específicas** en la solicitud|  
|Y requieren multi-factor Authentication|Desde una red **específica**<p>De grupos **específicos**<p>Desde dispositivos con niveles de confianza **específicos**<p>Con notificaciones **específicas** en la solicitud|  
  
Si un administrador selecciona varias condiciones, son de la relación **and** . Las acciones se excluyen mutuamente y, para una regla de Directiva, solo puede elegir una acción. Si el administrador selecciona varias excepciones, son de una relación de **o** . A continuación se muestran un par de ejemplos de reglas de directivas:  
  
|**Directiva**|**Reglas de directiva**|
| --- | --- |  
|El acceso a Extranet requiere MFA<p>Se permiten todos los usuarios|**#1 de reglas**<p>desde **extranet**<p>y con MFA<p>Permitir<p>**Regla n.º 2**<p>desde la **intranet**<p>Permitir|  
|No se permite el acceso externo excepto FTE<p>Se permite el acceso a la intranet para FTE en dispositivos Unidos al área de trabajo|**#1 de reglas**<p>Desde **extranet**<p>y de grupo **no FTE**<p>Permitir<p>**#2 de reglas**<p>desde la **intranet**<p>y desde un dispositivo **unido al área de trabajo**<p>y desde el grupo **FTE**<p>Permitir|  
|El acceso a Extranet requiere MFA excepto "administración de servicios"<p>A todos los usuarios se les permite el acceso|**#1 de reglas**<p>desde **extranet**<p>y con MFA<p>Permitir<p>Excepto **grupo de administradores de servicios**<p>**#2 de reglas**<p>Siempre<p>Permitir|  
|el acceso a un dispositivo no laborable Unido desde extranet requiere MFA<p>Permitir el acceso de AD fabric para la intranet y la extranet|**#1 de reglas**<p>desde la **intranet**<p>Y desde el grupo de **fabric de ad**<p>Permitir<p>**#2 de reglas**<p>desde **extranet**<p>y desde dispositivos **no Unidos al área de trabajo**<p>y desde el grupo de **fabric de ad**<p>y con MFA<p>Permitir<p>**#3 de reglas**<p>desde **extranet**<p>y desde un dispositivo **unido al área de trabajo**<p>y desde el grupo de **fabric de ad**<p>Permitir|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Plantilla de directiva con parámetros y plantilla de Directiva no parametrizada  
Las directivas de control de acceso se pueden  
  
Una plantilla de directiva con parámetros es una plantilla de directiva que tiene parámetros. Un administrador debe especificar el valor de esos parámetros al asignar esta plantilla a RPs.An administrador no puede realizar cambios en la plantilla de directiva con parámetros una vez creada.  Un ejemplo de una directiva parametrizada es la Directiva integrada, permitir un grupo específico.  Cada vez que se aplica esta directiva a un RP, es necesario especificar este parámetro.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Una plantilla de directiva sin parámetros es una plantilla de directiva que no tiene parámetros. Un administrador puede asignar esta plantilla a RPs sin ninguna entrada necesaria y puede realizar cambios en una plantilla de directiva sin parámetros una vez creada.  Un ejemplo de esto es la Directiva integrada, permitir a todos y requerir MFA.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Cómo crear una directiva de control de acceso sin parámetros  
Para crear una directiva de control de acceso sin parámetros, use el procedimiento siguiente:  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso sin parámetros  
  
1.  En administración de AD FS de la izquierda, seleccione directivas de Access Control y, a la derecha, haga clic en agregar Directiva de Access Control.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo: permitir usuarios con dispositivos autenticados.  
  
3.  En **permitir acceso si se cumple alguna de las siguientes reglas**, haga clic en **Agregar**.  
  
4.  En permitir, active la casilla situada junto a **desde dispositivos con un nivel de confianza específico** .  
  
5.  En la parte inferior, seleccione el subrayado **específico** .  
  
6.  En la ventana que aparece, seleccione **autenticado** en el menú desplegable.  Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Creación de una directiva de control de acceso con parámetros  
Para crear una directiva de control de acceso con parámetros, use el procedimiento siguiente:  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso con parámetros  
  
1.  En administración de AD FS de la izquierda, seleccione directivas de Access Control y, a la derecha, haga clic en agregar Directiva de Access Control.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo: permitir usuarios con una determinada demanda.  
  
3.  En **permitir acceso si se cumple alguna de las siguientes reglas**, haga clic en **Agregar**.  
  
4.  En permitir, active la casilla situada junto a **con notificaciones específicas en la solicitud** .  
  
5.  En la parte inferior, seleccione el subrayado **específico** .  
  
6.  En la ventana que aparece, seleccione **el parámetro especificado al asignar la Directiva de control de acceso**.  Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Cómo crear una directiva de control de acceso personalizada con una excepción  
Para crear una directiva de control de acceso con una excepción, use el procedimiento siguiente.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Para crear una directiva de control de acceso personalizada con una excepción  
  
1.  En administración de AD FS de la izquierda, seleccione directivas de Access Control y, a la derecha, haga clic en agregar Directiva de Access Control.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo: permitir usuarios con dispositivos autenticados pero no administrados.  
  
3.  En **permitir acceso si se cumple alguna de las siguientes reglas**, haga clic en **Agregar**.  
  
4.  En permitir, active la casilla situada junto a **desde dispositivos con un nivel de confianza específico** .  
  
5.  En la parte inferior, seleccione el subrayado **específico** .  
  
6.  En la ventana que aparece, seleccione **autenticado** en el menú desplegable.  Haga clic en **Aceptar**.  
  
7.  En excepto, active la casilla situada junto a **desde dispositivos con un nivel de confianza específico** .  
  
8.  En la parte inferior, en excepto, seleccione el subrayado **específico** .  
  
9. En la ventana que aparece, seleccione **administrado** en el menú desplegable.  Haga clic en **Aceptar**.  
  
10. Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Cómo crear una directiva de control de acceso personalizada con varias condiciones de permiso  
Para crear una directiva de control de acceso con varias condiciones de permiso, use el procedimiento siguiente:  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso con parámetros  
  
1.  En administración de AD FS de la izquierda, seleccione directivas de Access Control y, a la derecha, haga clic en agregar Directiva de Access Control.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo: permite a los usuarios con una determinada demanda y desde un grupo específico.  
  
3.  En **permitir acceso si se cumple alguna de las siguientes reglas**, haga clic en **Agregar**.  
  
4.  En permitir, active la casilla situada junto a **de un grupo específico** y **con notificaciones específicas en la solicitud** .  
  
5.  En la parte inferior, seleccione el subrayado **específico** para la primera condición, junto a grupos.  
  
6.  En la ventana que aparece, seleccione el **parámetro especificado al asignar la Directiva**.  Haga clic en **Aceptar**.  
  
7.  En la parte inferior, seleccione el subrayado **específico** de la segunda condición, junto a notificaciones.  
  
8.  En la ventana que aparece, seleccione **el parámetro especificado al asignar la Directiva de control de acceso**.  Haga clic en **Aceptar**.  
  
9. Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Asignación de una directiva de control de acceso a una nueva aplicación  
Asignar una directiva de control de acceso a una nueva aplicación es bastante sencillo y ahora se ha integrado en el Asistente para agregar un RP.  En el Asistente para la relación de confianza para usuario autenticado puede seleccionar la Directiva de control de acceso que desea asignar.  Este es un requisito a la hora de crear una nueva relación de confianza para usuario autenticado.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Asignación de una directiva de control de acceso a una aplicación existente  
Asignación de una directiva de control de acceso a una aplicación existente simplemente seleccione la aplicación de las relaciones de confianza para usuario autenticado y, a la derecha, haga clic en **editar Access Control Directiva**.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
Aquí puede seleccionar la Directiva de control de acceso y aplicarla a la aplicación.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Consulte también  
[Operaciones de AD FS](../ad-fs-operations.md) 
