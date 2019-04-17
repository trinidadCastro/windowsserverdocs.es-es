---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Directivas de Control de acceso de AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Directivas de Control de acceso de Windows Server 2016 AD FS

>Se aplica a: Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Plantillas de directiva de Control de acceso en AD FS  
Ahora, los servicios de federación de Active Directory admite el uso de plantillas de directiva de control de acceso.  Mediante el uso de plantillas de directiva de control de acceso, un administrador puede aplicar la configuración de directiva mediante la asignación de la plantilla de directiva a un grupo de usuarios de confianza (RPs). Administrador también puede realizar actualizaciones en la plantilla de directiva y los cambios se aplicarán a las partes de confianza automáticamente si no hay ninguna interacción del usuario es necesitada.  
  
## <a name="what-are-access-control-policy-templates"></a>Qué son las plantillas de directiva de Control de acceso  
La canalización de AD FS core para procesar la directiva tiene tres fases: autenticación, autorización y notificación de emisión. Actualmente, los administradores de AD FS deben configurar una directiva para cada una de estas fases por separado.  Esto implica también comprender las implicaciones de estas directivas y si estas directivas tienen dependencia entre. Además, los administradores tienen comprender la reclamación regla idioma y el autor reglas personalizadas para habilitar alguna directiva común o simple (ej. Bloquear el acceso externo).  
  
¿Qué directiva de control de acceso plantillas hacer es reemplazar este modelo anterior, donde los administradores deben configurar las reglas de autorización de emisión mediante notificaciones de idioma.  Los cmdlets de PowerShell antiguos emisión de reglas de autorización se siguen aplicando, pero es mutuamente incluyen el nuevo modelo. Los administradores pueden elegir usar el nuevo modelo o el modelo anterior.  El nuevo modelo permite a los administradores controlar cuándo se debe conceder acceso, incluido el cumplimiento de la autenticación multifactor.  
  
Plantillas de directiva de control de acceso usan un modelo de permiso.  Esto significa que de manera predeterminada, no tiene acceso y debe concederse explícitamente acceso.  Sin embargo, esto no es simplemente un todo o nada permitir.  Los administradores pueden agregar excepciones a la regla de permiso.  Por ejemplo, un administrador desea conceder acceso en función de una red determinada, si seleccionas esta opción y especificando el intervalo de direcciones IP.  Sin embargo, el administrador puede agregar y excepción, por ejemplo, el administrador puede agregar una excepción de una red determinada y especificar ese intervalo de direcciones IP.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Acceso integrado directiva plantillas vs acceso personalizados control directiva plantillas de control  
AD FS incluye varias plantillas de directivas de control de acceso integrados.  Estos destino algunos escenarios comunes que tienen el mismo conjunto de requisitos de directivas, por ejemplo una directiva de acceso a un cliente de Office 365.  No se puede modificar estas plantillas.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Para proporcionar una mayor flexibilidad para satisfacer sus necesidades empresariales, los administradores pueden crear su propios acceso plantillas de directiva.  Se pueden modificar después de la creación y cambios en la plantilla de directiva personalizada se aplicarán a todas las solicitudes por segundo que están controladas por esas plantillas de directiva.  Para agregar una plantilla de directiva personalizada simplemente haz clic en Agregar directiva de Control de acceso desde dentro de la administración de AD FS.  
  
Para crear una plantilla de directiva, un administrador debe especificar en qué condiciones se autorizarse una solicitud de emisión de token o delegación. Opciones de acción y la condición se muestran en la siguiente tabla.   Condiciones en negrita se puede configurar el Administrador con valores nuevos o diferentes. Administrador también puede especificar excepciones si hay alguna. Cuando una condición se cumple, no se desencadenará una acción de permitir si hay una excepción especificada y la solicitud entrante coincide con la condición especificada en la excepción.  
  
|Permitir que los usuarios|Excepto| 
| --- | --- | 
 |Desde **específico** red|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Desde **específico** grupos|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Desde dispositivos con **específico** niveles de confianza|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Con **específico** notificaciones en la solicitud|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
|Y requerir la autenticación multifactor|Desde **específico** red<br /><br />Desde **específico** grupos<br /><br />Desde dispositivos con **específico** niveles de confianza<br /><br />Con **específico** notificaciones en la solicitud|  
  
Si un administrador selecciona varias condiciones, son del **y** relación. Acciones son mutuamente excluyentes y para una regla de directivas puede elegir solo una acción. Si el administrador selecciona varias excepciones, son de un **o** relación. A continuación se muestran algunos ejemplos de reglas de directiva:  
  
|**Directiva**|**Reglas de directiva**|
| --- | --- |  
|Acceso a la extranet requiere MFA<br /><br />Todos los usuarios se permiten|**Regla #1**<br /><br />Desde **extranet**<br /><br />Y con MFA<br /><br />Permitir<br /><br />**Regla 2**<br /><br />Desde **intranet**<br /><br />Permitir|  
|No se permite el acceso externo excepto no etc<br /><br />Se permite el acceso de intranet para etc en dispositivo unido a un área de trabajo|**Regla #1**<br /><br />Desde **extranet**<br /><br />Y desde **no etc** grupo<br /><br />Permitir<br /><br />**Regla #2**<br /><br />Desde **intranet**<br /><br />Y desde **Unidos a un área de trabajo** dispositivo<br /><br />Y desde **etc** grupo<br /><br />Permitir|  
|Acceso a la extranet requiere MFA excepto "administración del servicio"<br /><br />Todos los usuarios pueden tener acceso|**Regla #1**<br /><br />Desde **extranet**<br /><br />Y con MFA<br /><br />Permitir<br /><br />Excepto **grupo de servicio de administración**<br /><br />**Regla #2**<br /><br />siempre<br /><br />Permitir|  
|No trabajo contexto dispositivo unido a acceder a desde extranet requiere MFA<br /><br />Permitir la estructura de AD de intranet y extranet acceso|**Regla #1**<br /><br />Desde **intranet**<br /><br />Y desde **AD Fabric** grupo<br /><br />Permitir<br /><br />**Regla #2**<br /><br />Desde **extranet**<br /><br />Y desde **no empresa unido** dispositivo<br /><br />Y desde **AD Fabric** grupo<br /><br />Y con MFA<br /><br />Permitir<br /><br />**Regla #3**<br /><br />Desde **extranet**<br /><br />Y desde **Unidos a un área de trabajo** dispositivo<br /><br />Y desde **AD Fabric** grupo<br /><br />Permitir|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Directiva parametrizados vs sin parámetros directiva plantilla  
Las directivas de control de acceso pueden ser  
  
Una plantilla de directiva parametrizados es una plantilla de directiva que tenga parámetros. Un administrador necesita el valor de los parámetros de entrada cuando asignar esta plantilla para RPs.An administrador no se realizan cambios en la plantilla de directiva parametrizados después de que se haya creado.  Un ejemplo de una directiva parametrizado es la directiva integrada, un grupo específico de permiso.  Siempre que esta directiva se aplica a un punto de reunión, debe especificarse este parámetro.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Una plantilla de directiva no parametrizados es una plantilla de directiva que no tiene parámetros. Un administrador puede asignar esta plantilla RPs sin ninguna entrada necesaria y realizar cambios en una plantilla de directiva no parametrizados después de que se haya creado.  Un ejemplo de esto es la directiva integrada, permitir que todos los usuarios y requieren MFA.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Cómo crear una directiva de control de acceso sin parámetros  
Para crear un acceso no parametrizados directiva de control usa el siguiente procedimiento  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso sin parámetros  
  
1.  De administración de AD FS a la izquierda selecciona las directivas de Control de acceso y a la derecha, haz clic en Agregar directiva de Control de acceso.  
  
2.  Escribe un nombre y una descripción.  Por ejemplo: permitir que los usuarios con dispositivos autenticados.  
  
3.  En **permitir el acceso si se cumplen algunas de las siguientes reglas**, haz clic en **agregar**.  
  
4.  En el permiso, coloque una comprobación en el cuadro junto a **desde dispositivos con nivel de confianza específicas**  
  
5.  En la parte inferior, selecciona el subrayado **específico**  
  
6.  En la ventana que pop-up, seleccione **autenticado** desde el menú desplegable.  Haz clic en **Aceptar**.  
  
    ![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Haz clic en **Aceptar**. Haz clic en **Aceptar**.  
  
    ![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Cómo crear una directiva de control de acceso parametrizados  
Para crear un control de acceso parametrizados directiva usa el siguiente procedimiento  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso parametrizados  
  
1.  De administración de AD FS a la izquierda selecciona las directivas de Control de acceso y a la derecha, haz clic en Agregar directiva de Control de acceso.  
  
2.  Escribe un nombre y una descripción.  Por ejemplo: permitir que los usuarios con una solicitud específica.  
  
3.  En **permitir el acceso si se cumplen algunas de las siguientes reglas**, haz clic en **agregar**.  
  
4.  En el permiso, coloque una comprobación en el cuadro junto a **con notificaciones específicas en la solicitud**  
  
5.  En la parte inferior, selecciona el subrayado **específico**  
  
6.  En la ventana que pop-up, seleccione **parámetro especifica cuándo se asigna la directiva de control de acceso**.  Haz clic en **Aceptar**.  
  
    ![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Haz clic en **Aceptar**. Haz clic en **Aceptar**.  
  
    ![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Cómo crear una directiva de control de acceso personalizados con una excepción  
Para crear un control de acceso a la directiva con una excepción usa el siguiente procedimiento.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Para crear una directiva de control de acceso personalizados con una excepción  
  
1.  De administración de AD FS a la izquierda selecciona las directivas de Control de acceso y a la derecha, haz clic en Agregar directiva de Control de acceso.  
  
2.  Escribe un nombre y una descripción.  Por ejemplo: permitir a los usuarios autenticados dispositivos pero no administrados.  
  
3.  En **permitir el acceso si se cumplen algunas de las siguientes reglas**, haz clic en **agregar**.  
  
4.  En el permiso, coloque una comprobación en el cuadro junto a **desde dispositivos con nivel de confianza específicas**  
  
5.  En la parte inferior, selecciona el subrayado **específico**  
  
6.  En la ventana que pop-up, seleccione **autenticado** desde el menú desplegable.  Haz clic en **Aceptar**.  
  
7.  Excepto en, active la casilla junto a **desde dispositivos con nivel de confianza específicas**  
  
8.  En la parte inferior, excepto en, selecciona el subrayado **específico**  
  
9. En la ventana que pop-up, seleccione **administrado** desde el menú desplegable.  Haz clic en **Aceptar**.  
  
10. Haz clic en **Aceptar**. Haz clic en **Aceptar**.  
  
    ![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Cómo crear una directiva de control de acceso personalizados con varias condiciones de autorización  
Para crear una directiva de control de acceso con permitir varias condiciones usan el siguiente procedimiento  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Para crear una directiva de control de acceso parametrizados  
  
1.  De administración de AD FS a la izquierda selecciona las directivas de Control de acceso y a la derecha, haz clic en Agregar directiva de Control de acceso.  
  
2.  Escribe un nombre y una descripción.  Por ejemplo: permitir que los usuarios con una solicitud específica y desde el grupo específico.  
  
3.  En **permitir el acceso si se cumplen algunas de las siguientes reglas**, haz clic en **agregar**.  
  
4.  En el permiso, coloque una comprobación en el cuadro junto a **de un determinado grupo** y **con notificaciones específicas en la solicitud**  
  
5.  En la parte inferior, selecciona el subrayado **específico** para la primera condición, junto a grupos  
  
6.  En la ventana que pop-up, seleccione **parámetro especificado cuando la directiva está asignada**.  Haz clic en **Aceptar**.  
  
7.  En la parte inferior, selecciona el subrayado **específico** para la segunda condición, al lado de notificaciones  
  
8.  En la ventana que pop-up, seleccione **parámetro especifica cuándo se asigna la directiva de control de acceso**.  Haz clic en **Aceptar**.  
  
9. Haz clic en **Aceptar**. Haz clic en **Aceptar**.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Cómo asignar una directiva de control de acceso a una nueva aplicación  
Asignar una directiva de control de acceso a una nueva aplicación es bastante sencillo y ahora se ha integrado en el Asistente para agregar un punto de reunión.  El Asistente de confianza de terceros confiar puedes seleccionar la directiva de control de acceso que se va a asignar.  Este es un requisito al crear una nueva confianza de terceros de confianza.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Cómo asignar una directiva de control de acceso a una aplicación existente  
Asignar una directiva de control de acceso a una aplicación existente simplemente selecciona la aplicación de confiar confianzas de terceros y en el clic derecho **Editar directiva de Control de acceso**.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
Desde aquí puedes seleccionar la directiva de control de acceso y aplicarlo a la aplicación.  
  
![Directivas de control de acceso](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Consulta también  
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md) 

