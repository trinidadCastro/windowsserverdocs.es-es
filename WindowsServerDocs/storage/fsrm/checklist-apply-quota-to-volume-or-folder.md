---
title: Aplicar una cuota a un volumen o carpeta
description: En este artículo se describe cómo aplicar una cuota de almacenamiento a un volumen o carpeta
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 62910af666fb16db5c2e7a30b49eedfa8c12cacb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860856"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Lista de comprobación: Aplicar una cuota a un volumen o carpeta

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

1. Establece la configuración del correo electrónico si tienes previsto enviar notificaciones de umbral o informes de almacenamiento por correo electrónico. [Configurar notificaciones por correo electrónico](configure-email-notifications.md)

2. Evalúa los requisitos de almacenamiento del volumen o carpeta. Puedes usar los informes del nodo **Administración de informes de almacenamiento** para ofrecer datos. (Por ejemplo, ejecute una archivos por informe de propietario a petición para identificar los usuarios que usan grandes cantidades de espacio en disco.) [Generar informes a petición](generate-reports-on-demand.md)

3. Revisa las plantillas de cuota preconfiguradas disponibles. (En **administración de cuotas**, haga clic en el **plantillas de cuota** nodo.) [Editar propiedades de la plantilla de cuota](edit-quota-template-properties.md) 
<br />O bien: <br /> Crea una nueva plantilla de cuota para aplicar una directiva de almacenamiento de la organización. [Crear una plantilla de cuota](create-quota-template.md)

4. Crea una cuota basada en la plantilla del volumen o carpeta.  
 [Crear una cuota](create-quota.md) <br /> O bien: <br /> Crea una cuota automática para generar automáticamente cuotas para las subcarpetas del volumen o carpeta. [Crear un automáticamente una cuota de aplicación](create-auto-apply-quota.md)

6. Programa una tarea de informes que incluya un informe de Uso de cuotas para supervisar cuotas periódicamente. [Programar un conjunto de informes](schedule-set-of-reports.md)

> [!Note]
> Si desea filtrar los archivos en una carpeta o volumen, consulte [lista de comprobación: Aplicar un filtro de archivos a un volumen o carpeta](checklist-apply-file-screen-to-volume-or-folder.md).











