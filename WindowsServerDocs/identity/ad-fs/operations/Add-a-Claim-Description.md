---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Agregar una descripción de notificación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1023ca7da02d2a1f6af42f68892dc4c5c8f1a2bf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444370"
---
# <a name="add-a-claim-description"></a>Agregar una descripción de notificación


En una organización del asociado de cuenta, los administradores crear notificaciones para representar la pertenencia de un usuario en un grupo o rol o para representar algunos datos sobre un usuario, por ejemplo, el número de identificación del empleado de un usuario.

En una organización del asociado de recurso, los administradores crear notificaciones correspondientes para representar grupos y usuarios que se pueden reconocer como los usuarios de recursos. Dado que de salida de notificaciones en el mapa de organización de asociado de cuenta para notificaciones entrantes en la organización del asociado de recurso, el asociado de recurso es capaz de aceptar las credenciales que proporciona el asociado de cuenta. 

Puede usar el siguiente procedimiento para agregar una notificación.

El requisito mínimo para realizar este procedimiento es pertenecer al grupo **Administradores** o un grupo equivalente en el equipo local.  Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Para agregar una descripción de notificación

1. En el administrador del servidor, haga clic en **herramientas**y, a continuación, seleccione **administración de AD FS**. 

2. Expanda **servicio** y haga clic derecho en **Agregar descripción de notificación**.
   ![Agregar descripción de notificación.](media/Add-a-Claim-Description/claimdesc1.png)

3. En la agregar un descripción de notificación de cuadro de diálogo **nombre para mostrar**, escriba un nombre único que identifica el grupo o rol de esta notificación.

4. Agregar un **nombre corto**.

5. En **de notificación de identificador**, escriba un URI que está asociado con el grupo o rol de la notificación que se va a usar.

6. En **descripción**, escriba el texto que mejor describa el propósito de esta notificación.

7. Según las necesidades de su organización, seleccione cualquiera de las casillas de verificación siguientes, según corresponda, al publicar esta notificación en los metadatos de federación:


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. Haga clic en **Aceptar**.

![Agregar descripción de notificación.](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
