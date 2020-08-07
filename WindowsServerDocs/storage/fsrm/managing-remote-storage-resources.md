---
title: Managing Remote Storage Resources
description: En este artículo se describe cómo administrar recursos de almacenamiento en un equipo remoto.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8498d55cbdeab609bb3526c9ef884e330148d714
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950639"
---
# <a name="managing-remote-storage-resources"></a>Managing Remote Storage Resources

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Existen dos opciones para administrar los recursos de almacenamiento en un equipo remoto:

-   Conéctese al equipo remoto desde el servidor de archivos Administrador de recursos complemento Microsoft<sup>®</sup> Management Console (MMC) (que puede usar para administrar los recursos remotos).
-   Usar las herramientas de línea de comandos instaladas con el Administrador de recursos del servidor de archivos.

Ambas opciones permiten trabajar con cuotas, archivos de pantalla, administrar clasificaciones, programar tareas de administración de archivos y generar informes con esos recursos remotos.

> [!Note]
> El Administrador de recursos del servidor de archivos puede administrar recursos en el equipo local o en un equipo remoto, pero no en los dos al mismo tiempo.

Por ejemplo, se puede:

-   Conéctese a otro equipo del dominio con el complemento servidor de archivos Administrador de recursos MMC y revise el uso de almacenamiento en un volumen o carpeta ubicado en el equipo remoto.
-   Crear plantillas de cuota o de filtro de archivos en un servidor local y después usar las herramientas de línea de comandos para importar las plantillas en un servidor de archivos ubicado en una sucursal.

Esta sección contiene los siguientes temas:

-   [Conectarse a un equipo remoto](connect-to-remote-computer.md)
-   [Herramientas de línea de comandos](command-line-tools.md)
