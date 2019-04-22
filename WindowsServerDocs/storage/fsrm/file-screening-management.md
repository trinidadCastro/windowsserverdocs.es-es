---
title: Administración del filtrado de archivos
description: En este artículo se describe cómo crear filtros de archivos, generar notificaciones, definir plantillas de filtrado de archivos y crear excepciones al filtrado de archivos
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52a08ae7eaee81c00985d5334f9abeaa84e30879
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814166"
---
# <a name="file-screening-management"></a>Administración del filtrado de archivos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración del filtrado de archivo** del complemento MMC del Administrador de recursos del servidor de archivos, puedes realizar las siguientes tareas:

-   Crear filtros de archivos para controlar los tipos de archivos que los usuarios pueden guardar y generar notificaciones cuando los usuarios intentan guardar archivos no autorizados.
-   Definir plantillas de filtrado de archivos que pueden aplicarse a volúmenes o carpetas nuevos y que pueden usarse en toda la organización.
-   Crear excepciones al filtrado de archivos que amplían la flexibilidad de las reglas de filtrado de archivos.

Por ejemplo, puedes:

-   Asegúrate de que ningún archivo de música se almacena en las carpetas personales de un servidor, aunque podrías permitir el almacenamiento de tipos específicos de archivos multimedia que sean compatibles con la administración de derechos legales o que cumplan con las directivas de la empresa. En el mismo escenario, es posible que quieras conceder privilegios especiales al vicepresidente de la empresa para almacenar cualquier tipo de archivo en su carpeta personal.
-   Implementa un proceso de filtrado que te notifique por correo electrónico si un archivo ejecutable se ha almacenado en una carpeta compartida, incluida la información sobre el usuario que almacenó el archivo y la ubicación exacta del archivo, de modo que puedas tomar las medidas de precaución adecuadas.

Esta sección incluye los temas siguientes:

-   [Definir grupos de archivos para la selección](define-file-groups-for-screening.md)
-   [Crear un filtro de archivos](create-file-screen.md)
-   [Crear una excepción al filtro de archivo](create-file-screen-exception.md)
-   [Crear una plantilla de pantalla de archivo](create-file-screen-template.md)
-   [Editar propiedades de plantilla de filtro de archivos](edit-file-screen-template-properties.md)

> [!Note]
> Para establecer notificaciones por correo electrónico y ciertas funcionalidades de informes, debes configurar primero las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="see-also"></a>Vea también

-   [Opciones del Administrador de recursos del servidor de archivos de configuración](setting-file-server-resource-manager-options.md)


