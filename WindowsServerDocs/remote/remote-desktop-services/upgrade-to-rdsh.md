---
title: Actualización del host de sesión de Escritorio remoto a Windows Server 2016
description: Obtenga información sobre cómo puede actualizar su host de sesión de Escritorio remoto a Windows Server 2016.
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 37aefa57473b9fe8d16fc399ad9bdc2e2e5d53d0
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965817"
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