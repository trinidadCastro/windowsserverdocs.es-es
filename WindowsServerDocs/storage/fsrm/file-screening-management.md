---
title: Administración del filtrado de archivos
description: En este artículo se describe cómo crear filtros de archivos, generar notificaciones, definir plantillas de filtrado de archivos y crear excepciones de filtrado de archivos.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 43aed09aead02883f91c168e1cfaf6388aedfa85
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473982"
---
# <a name="file-screening-management"></a>Administración del filtrado de archivos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración del filtrado de archivos** del complemento MMC Administrador de recursos servidor de archivos, puede realizar las siguientes tareas:

-   Crear filtros de archivos para controlar los tipos de archivos que los usuarios pueden guardar y generar notificaciones cuando los usuarios intenten guardar archivos no autorizados.
-   Definir plantillas de filtrado de archivos que puedan aplicarse a nuevos volúmenes o carpetas y que pueden utilizarse en toda una organización.
-   Crear excepciones de filtrado de archivos que amplíen la flexibilidad de las reglas de filtrado de archivos.

Por ejemplo, puede:

-   Asegúrese de que no haya archivos de música almacenados en las carpetas personales de un servidor, pero podría permitir el almacenamiento de tipos específicos de archivos multimedia que admitan la administración de derechos legales o que cumplan las directivas de la empresa. En el mismo escenario, puede que desee conceder a un vicepresidente de la compañía privilegios especiales para almacenar cualquier tipo de archivos en su carpeta personal.
-   Implemente un proceso de filtrado que le envíe una notificación por correo electrónico cuando se almacene un archivo ejecutable en una carpeta compartida, incluida información sobre el usuario que almacenó el archivo y la ubicación exacta del archivo, para que pueda realizar los pasos de precaución necesarios.

Esta sección contiene los siguientes temas:

-   [Definir grupos de archivos para el filtrado](define-file-groups-for-screening.md)
-   [Crear un filtro de archivos](create-file-screen.md)
-   [Crear una excepción al filtro de archivos](create-file-screen-exception.md)
-   [Crear una plantilla de filtro de archivos](create-file-screen-template.md)
-   [Editar las propiedades de la plantilla de filtro de archivos](edit-file-screen-template-properties.md)

> [!Note]
> Para configurar las notificaciones de correo electrónico y algunas capacidades de informes, primero debe configurar las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


