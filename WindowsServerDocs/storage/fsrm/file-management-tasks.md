---
title: Tareas de administración de archivos
description: En este artículo se describe el proceso de automatización de las tareas de administración de archivos
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4e10aeec47498d6af72e767f519b11ebb4e72932
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961389"
---
# <a name="file-management-tasks"></a>Tareas de administración de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las tareas de administración de archivos automatizan el proceso de búsqueda de subconjuntos de archivos en un servidor y la aplicación de comandos sencillos. Estas tareas se pueden programar para que se realicen periódicamente a fin de reducir los costos repetitivos. Los archivos que se van a procesar mediante una tarea de administración de archivos se pueden definir con cualquiera de las siguientes propiedades:

-   Ubicación
-   Propiedades de clasificación
-   Hora de creación
-   Hora de modificación
-   Última hora de acceso

Las tareas de administración de archivos también se pueden configurar para notificar a los propietarios de archivos de cualquier directiva inminente que se aplique a sus archivos.

> [!Note]
> Las tareas de administración de archivos individuales se ejecutan en programaciones independientes.

<br />
Esta sección contiene los siguientes temas:

-   [Crear una tarea de expiración de archivos](create-file-expiration-task.md)
-   [Crear una tarea de administración de archivos personalizada](create-custom-file-management-task.md)

> [!Note]
> Para configurar las notificaciones de correo electrónico y algunas capacidades de informes, primero debe configurar las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


