---
title: 'Lista de comprobación: aplicar un filtro de archivos a un volumen o una carpeta'
description: En este artículo se describe cómo aplicar un filtro de archivos a un volumen o una carpeta.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e7318d2e67cb5dd39aaa4fb95ad6cbca7eff9512
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957603"
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>Lista de comprobación: aplicar un filtro de archivos a un volumen o una carpeta

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para aplicar un filtro de archivos a un volumen o una carpeta, use la siguiente lista:
1. Configure las opciones de correo electrónico si tiene previsto enviar notificaciones de filtrado de archivos o informes de almacenamiento por correo electrónico siguiendo las instrucciones de [configurar notificaciones de correo electrónico](configure-email-notifications.md).

2. Habilite el registro de eventos de filtrado de archivos en la base de datos de auditoría si tiene pensado generar informes de auditoría de filtrado de archivos.
[Configurar Auditoría de filtrado de archivos](configure-file-screen-audit.md)

3. Evalúe los tipos de archivos almacenados que pueden ser candidatos para las reglas de filtrado. Puede usar los informes del nodo **Administración de informes de almacenamiento** para proporcionar datos. (Por ejemplo, ejecute un informe de Archivos por grupo de archivos o un informe de Archivos grandes a petición para identificar los archivos que ocupan grandes cantidades de espacio en disco). [Generar informes a petición](generate-reports-on-demand.md)

4. Revise los grupos de archivos preconfigurados o cree un nuevo grupo de archivos para aplicar una directiva de filtrado concreta en su organización. [Definir grupos de archivos para el filtrado](define-file-groups-for-screening.md)

5. Revise las propiedades de las plantillas de filtro de archivos disponibles. (En **Administración del filtrado de archivos**, haga clic en el nodo **plantillas de filtro de archivos** ). [Editar propiedades](edit-file-screen-template-properties.md) de la plantilla de filtro de archivos
<br />
 -O bien-
 <br /> Cree una nueva plantilla de filtro de archivos para aplicar una directiva de almacenamiento en su organización.  [Crear una plantilla de filtro de archivos](create-file-screen-template.md)

6. Cree un filtro de archivos basado en la plantilla de un volumen o de una carpeta.
 [Crear un filtro de archivos](create-file-screen.md)

7. Configure excepciones de filtro de archivos en subcarpetas del volumen o de la carpeta. [Crear una excepción al filtro de archivos](create-file-screen-exception.md)

8. Programe una tarea de informes que contenga un informe de auditoría de filtrado de archivos para supervisar periódicamente la actividad de filtrado.
  [Programar un conjunto de informes](schedule-set-of-reports.md)


> [!NOTE]
> Para limitar el almacenamiento en un volumen o una carpeta, consulte [lista de comprobación: aplicar una cuota a un volumen o una carpeta.](checklist-apply-file-screen-to-volume-or-folder.md)