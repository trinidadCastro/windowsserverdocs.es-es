---
title: Configuración de la redirección de carpetas en el servidor de destino de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b76246e24f17538741c278d432fa1ec275e891eb
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180801"
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configuración de la redirección de carpetas en el servidor de destino de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Haga esta tarea si está habilitada la redirección de carpetas en el servidor de origen.

 En primer lugar, elimine la antigua configuración de directiva de grupo de redirección de carpetas. A continuación, use el panel de Windows Server Essentials para habilitar el redireccionamiento de carpetas en el servidor de destino.

### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para eliminar la antigua configuración de directiva de grupo de redirección de carpetas

1. En el servidor de destino, abra la herramienta administrativa **Administración de directivas de grupo**.

2. En **Administración de directivas de grupo**, expandaaa **Bosque:**<em>NombreDominioRed</em>, expandaaa **Dominios**, expandaaa *NombreDominioRed*y **Objetos de directiva de grupo**.

3. Haga clic con el botón secundario en **Redirección de carpetas de directiva de grupo de SBS** y después haga clic en **Eliminar**.

4. Haga clic con el botón secundario en **Plantillas de seguridad de directiva de grupo de SBS** y después haga clic en **Eliminar**.

5. Lea la advertencia y haga clic en **Sí**.

6. Cierre **Administración de directivas de grupo**.

### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar la redirección de carpetas en el servidor de destino

1. En el servidor de destino, abra el panel de Windows Server Essentials.

2. En la barra de navegación, haga clic en **Dispositivos**.

3. En el panel **Tareas de dispositivos**, haga clic en **Implementar directiva de grupo**.

4. En la página **Habilitar la directiva de grupo de redirección de carpetas**, seleccione las carpetas que desea redirigir y haga clic en **Siguiente**.

5. En la página **Habilitar la configuración de directivas de seguridad**, haga clic en **Finalizar**.

   Para aplicar el cambio a la redirección de carpetas, los usuarios de la red deben cerrar sesión en su equipo y después reiniciarla. Así se garantiza la transferencia de todas las carpetas redirigidas al servidor de destino.
