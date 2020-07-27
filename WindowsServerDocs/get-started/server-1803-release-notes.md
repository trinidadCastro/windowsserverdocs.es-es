---
title: 'Notas de la versión: problemas importantes de Windows Server, versión 1803'
description: Más información acerca de problemas conocidos, limitaciones u otra información necesaria antes de instalar Windows Server, versión 1803
ms.prod: windows-server
ms.date: 05/07/2018
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7dc63afba827e2a58ba28d2c4398f1ba80d7e7b5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962497"
---
# <a name="release-notes-important-issues-in-windows-server-version-1803"></a>Notas de la versión: Problemas importantes en Windows Server, versión 1803

>Se aplica a: Canal semianual de Windows Server

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server junto con soluciones alternativas y formas de evitar los problemas. Para obtener información sobre las nuevas características de esta versión, consulta [Novedades de Windows Server, versión 1803](whats-new-in-windows-server-1803.md). Consulta [Acerca de los contenedores de Windows](/virtualization/windowscontainers/about/) si estás interesado en ejecutar un contenedor de Windows Server, versión 1803. 

A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server, versión 1803.  

Este artículo se actualiza continuamente. Si se identifican problemas conocidos, los documentaremos aquí. 


## <a name="software-defined-datacenter"></a>Centro de datos definido por software

Las características de centro de datos definidas por software como, por ejemplo, Espacios de almacenamiento directo, redes definidas por software y máquinas virtuales blindadas no están incluidas en Windows Server, versión 1803. Como se ha descrito en [Actualización del Canal semianual de Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), el Canal semianual de Windows Server se centra en contenedores y escenarios de aplicación que se benefician de una innovación más rápida. 

Si necesitas roles de infraestructura, usa una versión de Canal de mantenimiento a largo plazo: Windows Server 2016 (ya está disponible) o [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (que se lanzará más adelante en este año).

Estamos comprometidos a crear la mejor plataforma para la infraestructura hiperconvergida y seguimos desarrollando nuevas características y mejorando las existentes según tus comentarios. Gracias por ayudarnos a superar los [10 000 clústeres de espacios de almacenamiento directo](https://techcommunity.microsoft.com/t5/storage-at-microsoft/storage-spaces-direct-10-000-clusters-and-counting/ba-p/428185). Estamos deseando compartir más información próximamente este año.
