---
title: 'Notas de la versión: problemas importantes de Windows Server, versión 1803'
description: Obtén información sobre los problemas conocidos, limitaciones u otra información que necesitas antes de instalar a Windows Server, versión 1803
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
ms.openlocfilehash: e9bd860769ec375a6d89ac452e3430b791fff3ad
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339323"
---
# Notas de la versión: Problemas importantes en Windows Server, versión 1803

>Se aplica a: Canal semianual de WindowsServer

Estas notas resumen los problemas más críticos en el sistema operativo de Windows Server, formas de evitar o solucionar los problemas conocidos. Para obtener información sobre las nuevas características de esta versión, vea [What ' s New in Windows Server versión 1803](whats-new-in-windows-server-1803.md). Echa un vistazo a [los contenedores de Windows sobre](https://docs.microsoft.com/virtualization/windowscontainers/about/) si estás interesado en la ejecución de un servidor de Windows, versión 1803, el contenedor. 

A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server, versión 1803.  

Se actualiza continuamente en este artículo. Si se identifican problemas conocidos, te enviaremos Documente aquí. 


## Centro de datos definido por software

Las características del centro de datos definido por software, como espacios de almacenamiento directo, definida por software de redes y blindadas máquinas virtuales no están incluidas en Windows Server, versión 1803. Como se describe en la [actualización del canal semianual de Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), el canal semianual de Windows Server se centra en contenedores y escenarios de aplicaciones que se benefician de una innovación más rápida. 

Si necesitas roles de infraestructura, usa una versión de canal de mantenimiento a largo plazo: Windows Server 2016 (ya está disponible) o [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (disponible más adelante este año).

Nos comprometemos a compilar la mejor plataforma para la infraestructura hiperconvergida y seguimos a desarrollar nuevas características y mejorar los existentes en función de tus comentarios. Gracias por ayudarnos a llegar a [más de 10.000 clústeres de espacios de almacenamiento directo](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum). Estamos deseando compartir más adelante este año.