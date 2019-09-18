---
title: 'Notas de la versión: problemas importantes de Windows Server, versión 1803'
description: Más información acerca de problemas conocidos, limitaciones u otra información necesaria antes de instalar Windows Server, versión 1803
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 05/07/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 506236cbb3893fe200b39aa1d58ed44cdbd9704a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868377"
---
# <a name="release-notes-important-issues-in-windows-server-version-1803"></a>Notas de la versión: Problemas importantes en Windows Server, versión 1803

>Se aplica a: Canal semianual de Windows Server

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server junto con soluciones alternativas y formas de evitar los problemas. Para obtener información sobre las nuevas características de esta versión, consulta [Novedades de Windows Server, versión 1803](whats-new-in-windows-server-1803.md). Consulta [Acerca de los contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/) si estás interesado en ejecutar un contenedor de Windows Server, versión 1803. 

A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server, versión 1803.  

Este artículo se actualiza continuamente. Si se identifican problemas conocidos, los documentaremos aquí. 


## <a name="software-defined-datacenter"></a>Centro de datos definido por software

Las características de centro de datos definidas por software como, por ejemplo, Espacios de almacenamiento directo, redes definidas por software y máquinas virtuales blindadas no están incluidas en Windows Server, versión 1803. Como se ha descrito en [Actualización del Canal semianual de Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), el Canal semianual de Windows Server se centra en contenedores y escenarios de aplicación que se benefician de una innovación más rápida. 

Si necesitas roles de infraestructura, usa una versión de Canal de mantenimiento a largo plazo: Windows Server 2016 (ya está disponible) o [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (que se lanzará más adelante en este año).

Estamos comprometidos a crear la mejor plataforma para la infraestructura hiperconvergida y seguimos desarrollando nuevas características y mejorando las existentes según tus comentarios. Gracias por ayudarnos a superar los [10 000 clústeres de espacios de almacenamiento directo](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum). Estamos deseando compartir más información próximamente este año.