---
title: Administración de informes de almacenamiento
description: En este artículo se describe cómo generar, programar y supervisar informes de almacenamiento
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5bdcada1b445298c8743bdb39491726b594d0a66
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475482"
---
# <a name="storage-reports-management"></a>Administración de informes de almacenamiento

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En el nodo **Administración de informes de almacenamiento** del complemento servidor de archivos Administrador de recursos Microsoft<sup>®</sup> Management Console (MMC), puede realizar las siguientes tareas:

-   Programar informes de almacenamiento periódicos que permitan identificar las tendencias de uso de disco.
-   Realizar un seguimiento de los intentos de guardar archivos no autorizados para todos los usuarios o para un grupo de usuarios seleccionado.
-   Generar informes de almacenamiento inmediatamente.

Por ejemplo, puede:

-   Programar un informe para que se ejecute todos los domingos a medianoche y hacer que se genere una lista que incluya los archivos a los que se ha obtenido acceso más recientemente en los dos últimos días. Con esta información, puede supervisar la actividad de almacenamiento del fin de semana y planear el tiempo de inactividad del servidor que tendrá menos impacto en los usuarios que se conectan desde casa durante el fin de semana.
-   Ejecutar un informe en cualquier momento para identificar todos los archivos duplicados en un volumen de un servidor de modo que el espacio en disco pueda recuperarse rápidamente sin perder ningún dato.
-   Ejecutar un informe de Archivos por grupo de archivos para identificar cómo se segmentan los recursos de almacenamiento en distintos grupos de archivos
-   Ejecute un informe de Archivos por propietario para analizar la forma en que los usuarios individuales usan recursos de almacenamiento compartido.

Esta sección contiene los siguientes temas:

-   [Programar un conjunto de informes](schedule-set-of-reports.md)
-   [Generar informes a petición](generate-reports-on-demand.md)

> [!Note]
> Para configurar las notificaciones de correo electrónico y algunas capacidades de informes, primero debe configurar las opciones generales del Administrador de recursos del servidor de archivos.

## <a name="additional-references"></a>Referencias adicionales

-   [Configurar las opciones del Administrador de recursos del servidor de archivos](setting-file-server-resource-manager-options.md)


