---
title: Actualizar el Host de sesión de escritorio remoto a Windows Server 2016
description: En este artículo se describe cómo actualizar las implementaciones existentes de servicios de escritorio remoto a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 0cf5af29d610ba64d045e10241fd39b01d3f7024
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856066"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>Actualizar el Host de sesión de escritorio remoto a Windows Server 2016

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

> [!IMPORTANT]
> Todas las aplicaciones deben desinstalarse antes de la actualización y reinstalarse tras la actualización para evitar cualquier problema de compatibilidad de aplicaciones que puede aumentar debido a la actualización.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Admite actualizaciones del sistema operativo con instalado el rol RDS
Se admiten actualizaciones a Windows Server 2016 solo desde Windows Server 2012 R2 y Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>Actualizar una colección basada en sesiones RDS
Con el fin de mantener el tiempo de inactividad al mínimo, es mejor seguir los pasos descritos a continuación al actualizar una colección basada en sesiones RDS:

1. Identificar los servidores que se puede actualizar, por ejemplo, mitad de los servidores de la colección.
2. Impedir nuevas conexiones a estos servidores estableciendo **permiten nuevas conexiones** en false.
3. Cierre todas las sesiones en estos servidores. 
4. Quitar estos servidores de la colección.
5. Actualizar los servidores a Windows Server 2016.
6. Establecer **permiten nuevas conexiones** en "false" en los servidores restantes de la colección.
7. Agregue los servidores actualizados a sus correspondientes colecciones.
8. Quitar el conjunto de servidores para actualizarse desde la colección restantes.
9. Establecer **permiten nuevas conexiones** en "true" en los servidores actualizados en la colección.
10. Ahora puede actualizar los demás servidores de la implementación siguiendo los pasos del 3 al 9 anteriores.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>Actualizar un servidor de Host de sesión de escritorio remoto independientes
Un servidor de Host de sesión de escritorio remoto independientes se puede actualizar en cualquier momento.