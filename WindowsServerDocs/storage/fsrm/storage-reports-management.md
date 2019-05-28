---
title: Administración de informes de almacenamiento
description: En este artículo se describe cómo generar, programar y supervisar informes de almacenamiento
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 62215aa802e2509be5305aef53069ae9643562f1
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476099"
---
# <a name="storage-reports-management"></a>Administración de informes de almacenamiento

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración de informes de almacenamiento** del Complemento de la consola de administración de Microsoft<sup>®</sup> (MMC) del Administrador de recursos del servidor de archivos, puedes realizar las siguientes tareas:

-   Programar informes de almacenamiento periódicos que te permiten identificar tendencias de uso del disco.
-   Supervisar intentos de guardar archivos no autorizados de todos los usuarios o un grupo seleccionado de usuarios.
-   Generar informes de almacenamiento al instante.

Por ejemplo, puedes:

-   Programar un informe para que se ejecute todos los domingos a medianoche y hacer que se genere una lista que incluya los archivos a los que se ha obtenido acceso más recientemente en los dos últimos días. Con esta información, puedes supervisar la actividad de almacenamiento durante el fin de semana y planificar el tiempo de inactividad del servidor, lo que tendrá un impacto menor en los usuarios que se conecten desde casa durante el fin de semana.
-   Ejecutar un informe en cualquier momento para identificar todos los archivos duplicados en un volumen de un servidor para que el espacio de disco pueda recuperarse rápidamente sin perder ningún dato.
-   Ejecutar un informe Archivos por grupo de archivos para identificar cómo los recursos de almacenamiento se segmentan en distintos grupos de archivos. 
-   Ejecutar un informe Archivos por propietario para analizar cómo los usuarios individuales usan los recursos de almacenamiento compartidos.

Esta sección incluye los temas siguientes:

-   [Programar un conjunto de informes](schedule-set-of-reports.md)
-   [Generar informes a petición](generate-reports-on-demand.md)

> [!Note]
> Para establecer notificaciones por correo electrónico y ciertas funcionalidades de informes, debes configurar primero las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="see-also"></a>Vea también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


