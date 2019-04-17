---
title: "Administración de cuotas"
description: "En este artículo se describe cómo crear y administrar cuotas"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 60436f12e07b8a3f16312829d53a2885c98f30ed
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="quota-management"></a>Administración de cuotas

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración de cuotas** del Complemento de la consola de administración de Microsoft<sup>®</sup> (MMC) del Administrador de recursos del servidor de archivos, puedes realizar las siguientes tareas:

-   Crear cuotas para limitar el espacio permitido para un volumen o carpeta y generar notificaciones cuando alcancen o superen los límites de cuota.
-   Generar cuotas automáticas que se aplican a todas las subcarpetas existentes en un volumen o carpeta y a todas las subcarpetas que se creen en el futuro.
-   Definir plantillas de cuotas que pueden aplicarse fácilmente a volúmenes o carpetas nuevos y que pueden usarse en toda la organización.

Por ejemplo, puedes:

-   Establecer un límite de 200megabytes (MB) en las carpetas personales del servidor de los usuarios; se os enviará una notificación por correo electrónico a ti y al usuario cuando se hayan superado los 180MB de almacenamiento.
-   Establecer una cuota de 500MB flexible en la carpeta compartida de un grupo. Cuando se alcance este límite de almacenamiento, todos los usuarios del grupo recibirán por correo electrónico una notificación en la que se les informará que la cuota de almacenamiento se ha ampliado temporalmente a 520MB para que puedan eliminar los archivos que no necesiten y poder cumplir la directiva de cuota predefinida de 500MB.
-   Recibir una notificación cuando una carpeta temporal alcance los 2gigabytes (GB) de uso, pero no limitar la cuota de la carpeta, porque es necesaria para un servicio que se ejecuta en el servidor.

Esta sección incluye los siguientes temas:

-   [Crear una cuota](create-quota.md)
-   [Crear una cuota automática](create-auto-apply-quota.md)
-   [Crear una plantilla de cuota](create-quota-template.md)
-   [Editar propiedades de plantillas de cuotas](edit-quota-template-properties.md)
-   [Editar propiedades de cuota automática](edit-auto-apply-quota-properties.md)

> [!Note]
> Para establecer notificaciones por correo electrónico y funcionalidades de informes, debes configurar primero las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="see-also"></a>Consulta también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


