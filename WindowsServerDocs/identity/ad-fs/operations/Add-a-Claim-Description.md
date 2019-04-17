---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: "Agregar una descripción de la notificación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0e388ef656d3b690da62b077cb9f9e678a771e64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-claim-description"></a>Agregar una descripción de la notificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En una organización de partner de la cuenta, los administradores crear notificaciones para representar la pertenencia de un usuario en un grupo o rol o para representar algunos datos acerca de un usuario, por ejemplo, el número de identificación del empleado de un usuario.

En una organización de partner de recursos, los administradores crear notificaciones correspondientes para representar los grupos y los usuarios que pueden ser reconocidos como los usuarios de recursos. Porque saliente notificaciones en el mapa de organización de partner de cuenta para notificaciones entrantes en la organización de partner de recurso, el asociado de recurso puede aceptar las credenciales que proporciona el asociado de cuenta. 

Puedes usar el siguiente procedimiento para agregar una reclamación.

Pertenencia a **administradores**, o equivalente, en el equipo local es lo mínimo necesario para completar este procedimiento.  Revisar detalles sobre el uso de las cuentas adecuadas y agrupar pertenencias a [Local y dominio predeterminada grupos](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Para agregar una descripción de la notificación

1. En el administrador del servidor, haz clic en **herramientas**y, a continuación, selecciona **AD FS administración**. 

2.  Expande **servicio** y en el clic derecho **agregar reclamación descripción**.
![Agregar la descripción de la notificación](media\Add-a-Claim-Description\claimdesc1.png)

3.  En la agregar un descripción reclamar cuadro de diálogo **nombre para mostrar**, escribe un nombre exclusivo que identifica el grupo o rol para esta notificación.

4.  Agregar un **nombre corto**.

5.  En **reclamar identificador**, escribe un URI que está asociado con el grupo o rol de la notificación de que vas a usar.

6.  En **descripción**, escribe el texto que mejor describa la finalidad de esta notificación.

7.  Según las necesidades de la organización, selecciona cualquiera de las siguientes casillas, según corresponda, para publicar esta notificación en los metadatos de federación:


    - Para publicar esta notificación para hacer asociados tenga en cuenta que este servidor puede aceptar esta notificación, haz clic en **publicar esta notificación en los metadatos de federación como un tipo de notificación que puede aceptar este servicio de federación**.
    - Para publicar esta notificación para hacer asociados tenga en cuenta que este servidor puede emitir esta notificación, haz clic en **publicar esta notificación en los metadatos de federación como un tipo de notificación que puede enviar este servicio de federación**.

8.  Haz clic en **Aceptar**.

![Agregar la descripción de la notificación](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>Consulta también  
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md) 
