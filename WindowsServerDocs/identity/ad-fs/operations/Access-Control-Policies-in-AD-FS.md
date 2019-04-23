---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Directivas de Control de acceso en AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861306"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Directivas de control de acceso en AD FS para Windows Server 2016

>Se aplica a: Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Plantillas de directiva de Control de acceso en AD FS  
Servicios de federación de Active Directory ahora admite el uso de plantillas de directiva de control de acceso.  Mediante el uso de plantillas de directiva de control de acceso, un administrador puede aplicar la configuración de directiva mediante la asignación de la plantilla de directiva a un grupo de usuarios de confianza (RP). Administrador también puede realizar actualizaciones en la plantilla de directiva y los cambios se aplicarán a los usuarios de confianza automáticamente si no necesita ninguna interacción del usuario.  
  
## <a name="what-are-access-control-policy-templates"></a>¿Qué plantillas de directiva de Control de acceso?  
La canalización de núcleo de AD FS para el procesamiento de directiva tiene tres fases: autenticación, autorización y notificación de emisión. Actualmente, los administradores de AD FS tienen que configurar una directiva para cada una de estas fases por separado.  Esto también implica la comprensión de las implicaciones de estas directivas y si estas directivas tienen una dependencia entre. Además, los administradores deben comprender la notificación regla lenguaje y autor reglas personalizadas para habilitar alguna directiva simple o común (p. ej. bloquear el acceso externo).  
  
Plantillas hacer qué directiva de control de acceso es reemplazar este modelo anterior, donde los administradores deben configurar con reglas de autorización de emisión de notificaciones de lenguaje.  Aplicar los cmdlets de PowerShell anteriores de las reglas de autorización de emisión, pero es mutuamente excluyente el nuevo modelo. Los administradores pueden elegir usar el nuevo modelo o el modelo anterior.  El nuevo modelo permite a los administradores controlar cuándo se debe conceder acceso, incluida la aplicación de autenticación multifactor.  
  
Plantillas de directiva de control de acceso usan un modelo de autorización.  Esto significa que de forma predeterminada, nadie tiene acceso y que se debe conceder acceso explícitamente.  Sin embargo, esto no es simplemente un todo o nada permitir.  Los administradores pueden agregar excepciones para la regla de permiso.  Por ejemplo, un administrador desea conceder acceso basado en una red específica, si selecciona esta opción y especifique el intervalo de direcciones IP.  Pero el administrador puede agregar y la excepción, por ejemplo, el administrador puede agregar una excepción desde una red específica y especificar ese intervalo de direcciones IP.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Acceso integrado control plantillas vs acceso personalizado control directiva plantillas de directiva  
AD FS incluye varias plantillas de directiva de control de acceso integrado.  Estos escenarios comunes que tienen el mismo conjunto de requisitos de la directiva, por ejemplo una directiva de acceso a un cliente para Office 365 de destino.  No se puede modificar estas plantillas.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Para proporcionar una mayor flexibilidad para satisfacer sus necesidades empresariales, los administradores pueden crear su propio acceso a las plantillas de directiva.  Se pueden modificar después de la creación y los cambios realizados en la plantilla de directiva personalizada se aplicarán a todas las solicitudes por segundo que se controlan mediante las plantillas de directiva.  Para agregar una plantilla de directiva personalizado simplemente haga clic en Agregar directiva de Control de acceso desde dentro de la administración de AD FS.  
  
Para crear una plantilla de directiva, un administrador debe especificar en qué condiciones se puede autorizar una solicitud de emisión de tokens o delegación. Opciones de acción y condición se muestran en la tabla siguiente.   Las condiciones en negrita se pueden configurar el Administrador con valores nuevos o diferentes. Administrador también puede especificar las excepciones si hay alguno. Cuando se cumple una condición, no se desencadenará una acción de permitir si se produce una excepción especificada y la solicitud entrante coincide con la condición especificada en la excepción.  
  
|Permitir a los usuarios|Con la excepción| 
| --- | --- | 
 |Desde **específico** red|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Desde **específico** grupos|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Desde dispositivos con **específico** niveles de confianza|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Con **específico** notificaciones en la solicitud|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Y requerir autenticación multifactor|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
  
Si el administrador selecciona varias condiciones, son de **AND** relación. Las acciones son mutuamente excluyentes y para una regla de directivas solo puede elegir una acción. Si el administrador selecciona varias excepciones, son de un **o** relación. A continuación se muestra un par de ejemplos de reglas de directiva:  
  
|**Directiva**|**Reglas de directiva**|
| --- | --- |  
|Acceso a extranet requiere MFA<br /><br />Se permiten todos los usuarios|**Regla #1**<br /><br />desde **extranet**<br /><br />y con MFA<br /><br />Permitir<br /><br />**Rule#2**<br /><br />desde **intranet**<br /><br />Permitir|  
|No se permiten el acceso externo, excepto que no sean de FTE<br /><br />Se permiten el acceso a la intranet para FTE en dispositivo unido al área de trabajo|**Regla #1**<br /><br />Desde **extranet**<br /><br />y desde **no FTE** grupo<br /><br />Permitir<br /><br />**Regla #2**<br /><br />desde **intranet**<br /><br />y desde **unido** dispositivo<br /><br />y desde **FTE** grupo<br /><br />Permitir|  
|Acceso a extranet requiere MFA, excepto "Administrador de servicios"<br /><br />Todos los usuarios tienen permiso para tener acceso a|**Regla #1**<br /><br />desde **extranet**<br /><br />y con MFA<br /><br />Permitir<br /><br />Excepto **grupo de administración de servicio**<br /><br />**Regla #2**<br /><br />Siempre<br /><br />Permitir|  
|dispositivo unido al lugar que no son de trabajo acceso desde extranet requiere MFA<br /><br />Permitir el tejido de AD para la intranet y extranet access|**Regla #1**<br /><br />desde **intranet**<br /><br />Y desde **AD Fabric** grupo<br /><br />Permitir<br /><br />**Regla #2**<br /><br />desde **extranet**<br /><br />y desde **no-unido** dispositivo<br /><br />y desde **AD Fabric** grupo<br /><br />y con MFA<br /><br />Permitir<br /><br />**Regla #3**<br /><br />desde **extranet**<br /><br />y desde **unido** dispositivo<br /><br />y desde **AD Fabric** grupo<br /><br />Permitir|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Plantilla de directiva sin parámetros de vs de plantilla de directiva con parámetros  
Las directivas de control de acceso pueden ser  
  
Una plantilla de directiva con parámetros es una plantilla de directiva que tiene parámetros. Un administrador necesita introducir el valor para esos parámetros al asignar esta plantilla al administrador RPs.An no puede realizar cambios en la plantilla de directiva con parámetros después de que se ha creado.  Un ejemplo de una directiva con parámetros es la directiva integrada, un grupo específico de permiso.  Cada vez que esta directiva se aplica a un punto de reunión, este parámetro debe especificarse.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Una plantilla de directiva sin parámetros es una plantilla de directiva que no tiene parámetros. Un administrador puede asignar esta plantilla RPs sin cualquier entrada necesaria y puede realizar cambios en una plantilla de directiva sin parámetros después de que se ha creado.  Un ejemplo de esto es la directiva integrada, permitir a todos los usuarios y solicitar MFA.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Cómo crear una directiva de control de acceso sin parámetros  
Para crear un acceso sin parámetros, la directiva de control use el procedimiento siguiente  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso sin parámetros  
  
1.  En administración de AD FS de la izquierda seleccione directivas de Control de acceso y a la derecha, haga clic en Agregar directiva de Control de acceso.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo:  Permitir a los usuarios con dispositivos autenticados.  
  
3.  En **permitir el acceso si se cumple alguna de las siguientes reglas**, haga clic en **agregar**.  
  
4.  En permitido, coloque una comprobación en el cuadro junto a **desde dispositivos con el nivel de confianza concreto**  
  
5.  En la parte inferior, seleccione el subrayado **específico**  
  
6.  En la ventana de copia de seguridad que aparece, seleccione **autenticado** en la lista desplegable.  Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Cómo crear una directiva de control de acceso con parámetros  
Para crear un control de acceso con parámetros directiva utilice el siguiente procedimiento  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso con parámetros  
  
1.  En administración de AD FS de la izquierda seleccione directivas de Control de acceso y a la derecha, haga clic en Agregar directiva de Control de acceso.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo:  Permitir a los usuarios con una notificación concreta.  
  
3.  En **permitir el acceso si se cumple alguna de las siguientes reglas**, haga clic en **agregar**.  
  
4.  En permitido, coloque una comprobación en el cuadro junto a **con notificaciones específicas de la solicitud**  
  
5.  En la parte inferior, seleccione el subrayado **específico**  
  
6.  En la ventana de copia de seguridad que aparece, seleccione **parámetro especificado cuando se asigna la directiva de control de acceso**.  Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Cómo crear una directiva de control de acceso personalizado con una excepción  
Para crear un control de acceso directiva con una excepción utilice el procedimiento siguiente.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Para crear una directiva de control de acceso personalizado con una excepción  
  
1.  En administración de AD FS de la izquierda seleccione directivas de Control de acceso y a la derecha, haga clic en Agregar directiva de Control de acceso.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo:  Permitir a los usuarios autentican de dispositivos, pero no administran.  
  
3.  En **permitir el acceso si se cumple alguna de las siguientes reglas**, haga clic en **agregar**.  
  
4.  En permitido, coloque una comprobación en el cuadro junto a **desde dispositivos con el nivel de confianza concreto**  
  
5.  En la parte inferior, seleccione el subrayado **específico**  
  
6.  En la ventana de copia de seguridad que aparece, seleccione **autenticado** en la lista desplegable.  Haga clic en **Aceptar**.  
  
7.  Excepto en, marque la casilla junto a **desde dispositivos con el nivel de confianza concreto**  
  
8.  En la parte inferior, excepto en, seleccione el subrayado **específico**  
  
9. En la ventana de copia de seguridad que aparece, seleccione **administrada** en la lista desplegable.  Haga clic en **Aceptar**.  
  
10. Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
    ![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Cómo crear una directiva de control de acceso personalizado con varias condiciones de permiso  
Para crear una directiva de control de acceso con autorización varias condiciones use el procedimiento siguiente  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso con parámetros  
  
1.  En administración de AD FS de la izquierda seleccione directivas de Control de acceso y a la derecha, haga clic en Agregar directiva de Control de acceso.  
  
2.  Escriba un nombre y una descripción.  Por ejemplo:  Permitir a los usuarios con una notificación concreta y de grupo específico.  
  
3.  En **permitir el acceso si se cumple alguna de las siguientes reglas**, haga clic en **agregar**.  
  
4.  En permitido, coloque una comprobación en el cuadro junto a **desde un grupo específico** y **con notificaciones específicas de la solicitud**  
  
5.  En la parte inferior, seleccione el subrayado **específico** para la primera condición, junto a grupos  
  
6.  En la ventana de copia de seguridad que aparece, seleccione **parámetro especificado cuando se asigna la directiva**.  Haga clic en **Aceptar**.  
  
7.  En la parte inferior, seleccione el subrayado **específico** para la segunda condición, junto a las notificaciones  
  
8.  En la ventana de copia de seguridad que aparece, seleccione **parámetro especificado cuando se asigna la directiva de control de acceso**.  Haga clic en **Aceptar**.  
  
9. Haga clic en **Aceptar**. Haga clic en **Aceptar**.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Cómo asignar una directiva de control de acceso a una nueva aplicación  
Asignar una directiva de control de acceso a una aplicación nueva es bastante sencillo y ahora se ha integrado en el Asistente para agregar un punto de reunión.  Puede seleccionar desde el Asistente para usuario autenticado entidad de confianza para la directiva de control de acceso que se va a asignar.  Esto es un requisito al crear una nueva entidad de confianza.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Cómo asignar una directiva de control de acceso a una aplicación existente  
Asignar una directiva de control de acceso a una aplicación existente simplemente seleccione la aplicación desde autenticado y haga clic derecho en **Editar directiva de Control de acceso**.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
Desde aquí puede seleccionar la directiva de control de acceso y se aplican a la aplicación.  
  
![directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 

