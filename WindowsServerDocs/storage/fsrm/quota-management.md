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
ms.openlocfilehash: 1f2d584e7d3a0239e38dcadcf6415683d91a4bec
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474203"
---
# <a name="quota-management"></a>Administración de cuotas

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración de cuotas** del complemento servidor de archivos Administrador de recursos Microsoft<sup>®</sup> Management Console (MMC), puede realizar las siguientes tareas:

-   Crear cuotas para limitar el espacio asignado a un volumen o carpeta y generar notificaciones cuando se esté a punto de alcanzar o superar el límite de dichas cuotas.
-   Generar cuotas automáticas aplicables a todas las carpetas existentes en un volumen o una carpeta y a todas las subcarpetas que se creen en lo sucesivo.
-   Definir plantillas de cuota que puedan aplicarse fácilmente a nuevos volúmenes o carpetas y que puedan utilizarse en toda una organización.

Por ejemplo, puede:

-   Coloque un límite de 200 megabytes (MB) en las carpetas del servidor personal de los usuarios, con una notificación de correo electrónico enviada a usted y al usuario cuando se superen los 180 MB de almacenamiento.
-   Establecer una cuota de advertencia de 500 MB en la carpeta compartida de un grupo. Cuando se alcance este límite de almacenamiento, todos los usuarios del grupo recibirán por correo electrónico una notificación en la que se les informará de que la cuota de almacenamiento se ha ampliado temporalmente a 520 MB para que puedan eliminar los archivos que no necesiten y poder cumplir la cuota predefinida de 500 MB.
-   Recibir una notificación cuando una carpeta temporal llegue a tener un uso de 2 gigabytes (GB) sin limitar la cuota de esa carpeta si es necesaria para ejecutar un servicio en el servidor.

Esta sección contiene los siguientes temas:

-   [Crear una cuota](create-quota.md)
-   [Crear una cuota automática](create-auto-apply-quota.md)
-   [Crear una plantilla de cuota](create-quota-template.md)
-   [Editar propiedades de plantillas de cuotas](edit-quota-template-properties.md)
-   [Editar propiedades de cuota automática](edit-auto-apply-quota-properties.md)

> [!Note]
> Para configurar las notificaciones de correo electrónico y las funciones de informes, primero debe configurar las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


