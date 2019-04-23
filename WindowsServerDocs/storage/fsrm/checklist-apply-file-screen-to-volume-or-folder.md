---
title: 'Lista de comprobación: aplicar un filtro de archivos a un volumen o carpeta'
description: En este artículo se describe cómo aplicar un filtro de archivos a un volumen o carpeta
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4c6953d27361ab2e3210a1574a5988e434f701f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861926"
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>Lista de comprobación: aplicar un filtro de archivos a un volumen o carpeta

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para aplicar un filtro de archivos a un volumen o carpeta, usa la siguiente lista:
1. Establece la configuración del correo electrónico si tienes previsto enviar notificaciones de filtrado de archivos o informes de almacenamiento por correo electrónico siguiendo las instrucciones que se indican en [Configurar notificaciones por correo electrónico](configure-email-notifications.md).

2. Activa el registro de eventos de filtrado de archivos en la base de datos de auditoría si tienes previsto generar informes de Auditoría de filtrado de archivos.
[Configurar la auditoría de filtro de archivo](configure-file-screen-audit.md)

3. Evalúa los tipos de archivo almacenados que son candidatos para las reglas de filtrado. Puedes usar los informes del nodo **Administración de informes de almacenamiento** para ofrecer datos. (Por ejemplo, ejecute una archivos por grupo de archivos o un informe archivos grandes a petición para identificar los archivos que ocupan una gran cantidad de espacio en disco.) [Generar informes a petición](generate-reports-on-demand.md) 

4. Revisa los grupos de archivos preconfigurados o crea un nuevo grupo de archivos para aplicar una directiva específica de filtrado de la organización. [Definir grupos de archivos para la selección](define-file-groups-for-screening.md)  

5. Revisa las propiedades de plantillas de filtro de archivos disponibles. (En **administración del filtrado de archivo**, haga clic en el **plantillas de pantalla de archivo** nodo.) [Editar propiedades de plantilla de filtro de archivos](edit-file-screen-template-properties.md) 
<br />
 O bien:
 <br /> Crea una nueva plantilla de filtro de archivos para aplicar una directiva de almacenamiento de la organización.  [Crear una plantilla de pantalla de archivo](create-file-screen-template.md) 

6. Crea un filtro de archivos basado en la plantilla de un volumen o carpeta. 
 [Crear un filtro de archivos](create-file-screen.md)
 
7. Configura excepciones de filtro de archivos en las subcarpetas del volumen o carpeta. [Crear una excepción al filtro de archivo](create-file-screen-exception.md) 

8. Programa una tarea de informes que incluya un informe de Auditoría de filtrado de archivos para supervisar la actividad de filtrado periódicamente.
  [Programar un conjunto de informes](schedule-set-of-reports.md)


> [!NOTE]
> Para limitar el almacenamiento en una carpeta o volumen, consulte [lista de comprobación: Aplicar una cuota a un volumen o carpeta](checklist-apply-file-screen-to-volume-or-folder.md)