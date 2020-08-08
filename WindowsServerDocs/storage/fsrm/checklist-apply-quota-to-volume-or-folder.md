---
title: Aplicar una cuota a un volumen o una carpeta
description: En este artículo se describe cómo aplicar una cuota de almacenamiento a un volumen o una carpeta.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6f35576c67b3b5837749d0f19da82b242cc6c1cd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957593"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Lista de comprobación: aplicar una cuota a un volumen o una carpeta

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semianual)

1. Configure los parámetros de correo electrónico si tiene pensado enviar por correo electrónico notificaciones de umbral o informes de almacenamiento. [Configurar notificaciones por correo electrónico](configure-email-notifications.md)

2. Evalúa los requisitos de almacenamiento en el volumen o la carpeta. Puede usar los informes del nodo **Administración de informes de almacenamiento** para proporcionar los datos. (Por ejemplo, ejecute un informe de Archivos por propietario a petición para identificar a los usuarios que usan grandes cantidades de espacio en disco). [Generar informes a petición](generate-reports-on-demand.md)

3. Revise las plantillas de cuota preconfiguradas disponibles. (En **Administración de cuotas**, haga clic en el nodo **plantillas de cuota** ). [Editar propiedades de plantilla de cuota](edit-quota-template-properties.md)
<br />-O bien- <br /> Cree una nueva plantilla de cuota para aplicar una directiva de almacenamiento en su organización. [Crear una plantilla de cuota](create-quota-template.md)

4. Cree una cuota basada en la plantilla de un volumen o de una carpeta.
 [Crear una cuota](create-quota.md) <br /> -O bien- <br /> Cree una cuota automática para generar automáticamente cuotas para las subcarpetas de un volumen o de una carpeta. [Crear una cuota automática](create-auto-apply-quota.md)

6. Programe una tarea de informes que contenga un informe de uso de cuotas para supervisar periódicamente el uso de las cuotas. [Programar un conjunto de informes](schedule-set-of-reports.md)

> [!Note]
> Si desea filtrar archivos en un volumen o una carpeta, consulte [lista de comprobación: aplicar un filtro de archivos a un volumen o una carpeta](checklist-apply-file-screen-to-volume-or-folder.md).











