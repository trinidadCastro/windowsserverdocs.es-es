---
title: Aplicar una cuota a un volumen o carpeta
description: En este artículo se describe cómo aplicar una cuota de almacenamiento a un volumen o carpeta
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 44eea3c3802e261239db9bf8350777e1b83fb9c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394330"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Lista de comprobación: aplicar una cuota a un volumen o carpeta

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semianual)

1. Establece la configuración del correo electrónico si tienes previsto enviar notificaciones de umbral o informes de almacenamiento por correo electrónico. [Configurar notificaciones por correo electrónico](configure-email-notifications.md)

2. Evalúa los requisitos de almacenamiento del volumen o carpeta. Puedes usar los informes del nodo **Administración de informes de almacenamiento** para ofrecer datos. (Por ejemplo, ejecuta a petición un informe Archivos por propietario para identificar quién usa una gran cantidad de espacio del disco). [Generar informes a petición](generate-reports-on-demand.md)

3. Revisa las plantillas de cuota preconfiguradas disponibles. (En **Administración de cuotas**, haga clic en el nodo **plantillas de cuota** ). [Editar propiedades de plantilla de cuota](edit-quota-template-properties.md) 
<br />O bien, <br /> Crea una nueva plantilla de cuota para aplicar una directiva de almacenamiento de la organización. [Crear una plantilla de cuota](create-quota-template.md)

4. Crea una cuota basada en la plantilla del volumen o carpeta.  
 [Crear una cuota](create-quota.md) <br /> O bien, <br /> Crea una cuota automática para generar automáticamente cuotas para las subcarpetas del volumen o carpeta. [Crear una cuota automática](create-auto-apply-quota.md)

6. Programa una tarea de informes que incluya un informe de Uso de cuotas para supervisar cuotas periódicamente. [Programar un conjunto de informes](schedule-set-of-reports.md)

> [!Note]
> Si quieres filtrar archivos de un volumen o carpeta, consulta [Lista de comprobación: aplicar un filtro de archivos a un volumen o carpeta](checklist-apply-file-screen-to-volume-or-folder.md).











