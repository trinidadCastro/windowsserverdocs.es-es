---
title: Administración de cuotas
description: En este artículo se describe cómo crear y administrar cuotas
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5a655e28020d08bb1c10fa862c007f914a8cf566
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403076"
---
# <a name="quota-management"></a>Administración de cuotas

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración de cuotas** del Complemento de la consola de administración de Microsoft<sup>®</sup> (MMC) del Administrador de recursos del servidor de archivos, puedes realizar las siguientes tareas:

-   Crear cuotas para limitar el espacio permitido para un volumen o carpeta y generar notificaciones cuando alcancen o superen los límites de cuota.
-   Generar cuotas automáticas que se aplican a todas las subcarpetas existentes en un volumen o carpeta y a todas las subcarpetas que se creen en el futuro.
-   Definir plantillas de cuotas que pueden aplicarse fácilmente a volúmenes o carpetas nuevos y que pueden usarse en toda la organización.

Por ejemplo, puedes:

-   Coloque un límite de 200 megabytes (MB) en las carpetas del servidor personal de los usuarios, con una notificación de correo electrónico enviada a usted y al usuario cuando se superen los 180 MB de almacenamiento.
-   Establezca una cuota de 500 MB flexible en la carpeta compartida de un grupo. Cuando se alcanza este límite de almacenamiento, todos los usuarios del Grupo reciben una notificación por correo electrónico de que la cuota de almacenamiento se ha ampliado temporalmente a 520 MB para que puedan eliminar archivos innecesarios y cumplir con la Directiva de cuota preestablecida 500 MB.
-   Recibir una notificación cuando una carpeta temporal alcance los 2 gigabytes (GB) de uso, pero no limitar la cuota de la carpeta, porque es necesaria para un servicio que se ejecuta en el servidor.

Esta sección incluye los temas siguientes:

-   [Crear una cuota](create-quota.md)
-   [Crear una cuota automática](create-auto-apply-quota.md)
-   [Crear una plantilla de cuota](create-quota-template.md)
-   [Editar propiedades de plantillas de cuotas](edit-quota-template-properties.md)
-   [Editar propiedades de cuota automática](edit-auto-apply-quota-properties.md)

> [!Note]
> Para establecer notificaciones por correo electrónico y funcionalidades de informes, debes configurar primero las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="see-also"></a>Vea también

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


