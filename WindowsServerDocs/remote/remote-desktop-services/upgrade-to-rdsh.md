---
title: Actualización del host de sesión de Escritorio remoto a Windows Server 2016
description: En este artículo se describe cómo actualizar las implementaciones existentes de Servicios de Escritorio remoto a Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: e685c51a003a7121dab19c74d82796311ef0889a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857128"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>Actualización del host de sesión de Escritorio remoto a Windows Server 2016

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

> [!IMPORTANT]
> Todas las aplicaciones deben desinstalarse antes de la actualización y reinstalarse tras la actualización para evitar cualquier problema de compatibilidad de aplicaciones que pueda surgir debido a la actualización.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Actualizaciones admitidas del sistema operativo con el rol de RDS instalado
Las actualizaciones a Windows Server 2016 solo se admiten desde Windows Server 2012 R2 y Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>Actualizar una colección basada en sesiones de RDS
Con el fin de minimizar el tiempo de inactividad, es mejor seguir los pasos descritos a continuación al actualizar una colección basada en sesiones de RDS:

1. Identifica los servidores que se van a actualizar, por ejemplo, la mitad de los servidores de la colección.
2. Impide nuevas conexiones a estos servidores; para ello, establece **Permitir conexiones nuevas** en false.
3. Cierre todas las sesiones en estos servidores. 
4. Quita estos servidores de la colección.
5. Actualiza los servidores a Windows Server 2016.
6. Establece **Permiten conexiones nuevas** en "false" en los servidores restantes de la colección.
7. Vuelve a agregar los servidores actualizados a sus colecciones correspondientes.
8. Quita de la colección el conjunto de servidores restante que va a actualizarse.
9. Establece **Permiten conexiones nuevas** en "true" en los servidores actualizados de la colección.
10. Ahora puedes actualizar los servidores restantes de la implementación siguiendo los pasos del 3 al 9 anteriores.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>Actualizar un servidor host de sesión de Escritorio remoto independiente
Un servidor host de sesión de Escritorio remoto independiente se puede actualizar en cualquier momento.