---
title: Administración de cuotas
description: En este artículo se describe cómo crear y administrar cuotas
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: febcd6ab0744a7fddd024e1f0afdb93711e8939a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829546"
---
# <a name="quota-management"></a>Administración de cuotas

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración de cuotas** del Complemento de la consola de administración de Microsoft<sup>®</sup> (MMC) del Administrador de recursos del servidor de archivos, puedes realizar las siguientes tareas:

-   Crear cuotas para limitar el espacio permitido para un volumen o carpeta y generar notificaciones cuando alcancen o superen los límites de cuota.
-   Generar cuotas automáticas que se aplican a todas las subcarpetas existentes en un volumen o carpeta y a todas las subcarpetas que se creen en el futuro.
-   Definir plantillas de cuotas que pueden aplicarse fácilmente a volúmenes o carpetas nuevos y que pueden usarse en toda la organización.

Por ejemplo, puedes:

-   Pone un límite de 200 megabytes (MB) en las carpetas del servidor personal de los usuarios, con una notificación por correo electrónico que se enviará a usted y el usuario cuando se ha superado los 180 MB de almacenamiento.
-   Establecer una cuota flexible de 500 MB en la carpeta compartida de un grupo. Cuando se alcanza este límite de almacenamiento, todos los usuarios en el grupo se notificación por correo electrónico que la cuota de almacenamiento se ha ampliado temporalmente a 520 MB para que puedan eliminar archivos innecesarios y cumplir con la directiva de cuota de 500 MB preestablecido.
-   Recibir una notificación cuando una carpeta temporal alcance los 2 gigabytes (GB) de uso, pero no limitar la cuota de la carpeta, porque es necesaria para un servicio que se ejecuta en el servidor.

Esta sección incluye los temas siguientes:

-   [Crear una cuota](create-quota.md)
-   [Crear un automáticamente una cuota de aplicación](create-auto-apply-quota.md)
-   [Crear una plantilla de cuota](create-quota-template.md)
-   [Editar propiedades de la plantilla de cuota](edit-quota-template-properties.md)
-   [Automática de editar propiedades de cuota](edit-auto-apply-quota-properties.md)

> [!Note]
> Para establecer notificaciones por correo electrónico y funcionalidades de informes, debes configurar primero las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="see-also"></a>Vea también

-   [Opciones del Administrador de recursos del servidor de archivos de configuración](setting-file-server-resource-manager-options.md)


