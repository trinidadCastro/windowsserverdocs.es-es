---
title: Tareas de administración de archivos
description: En este artículo se describe el proceso de automatizar tareas de administración de archivos
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d8d798611a00e29337a5d45979947a51f03bcdee
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475894"
---
# <a name="file-management-tasks"></a>Tareas de administración de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las tareas de administración de archivos automatizan el proceso de encontrar subconjuntos de archivos en un servidor y aplicar comandos sencillos. Estas tareas se pueden programar para que tengan lugar periódicamente con el fin de reducir los costes repetitivos. Se pueden definir los archivos que se procesarán mediante una tarea de administración de archivos a través de cualquiera de las siguientes propiedades:

-   Location
-   Propiedades de clasificación
-   Hora de creación
-   Hora de modificación
-   Acceso más reciente

Las tareas de administración de archivos también pueden configurarse para notificar a los propietarios de archivos sobre cualquier directiva inminente que se aplicará a sus archivos.

> [!Note]
> Las tareas de administración de archivos individuales se ejecutan en programaciones independientes.

<br />
Esta sección incluye los temas siguientes:

-   [Crear una tarea de expiración de archivos](create-file-expiration-task.md)
-   [Crear una tarea de administración de archivos personalizada](create-custom-file-management-task.md)

> [!Note]
> Para establecer notificaciones por correo electrónico y ciertas funcionalidades de informes, debes configurar primero las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="see-also"></a>Vea también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


