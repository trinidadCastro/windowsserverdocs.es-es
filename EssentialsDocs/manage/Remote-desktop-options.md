---
title: Opciones de escritorio remoto
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51076946-ea9b-4ac7-9a6e-d6023816b97d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: f4603adb7da1df0853b4c816254241d21b969fc5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625921"
---
# <a name="remote-desktop-options"></a>Opciones de escritorio remoto

## <a name="connection-speed"></a>Velocidad de conexión
 La velocidad de la conexión a un equipo de red mediante el Acceso web remoto determina las opciones de escritorio que están disponibles en el equipo host. La siguiente tabla indica qué opciones de escritorio están disponibles para la velocidad con la que se conecta al equipo remoto mediante el Acceso web remoto.

| Opción de escritorio | Módem lento (28,8 Kbps) | Módem rápido (56 Kbps) (valor predeterminado) | Banda ancha (128 Kbps - 1,5 Mbps) | Red de área local (1,5 Mbps o superior) |
|--|--|--|--|--|
| Suavizado de fuentes | No | No | No | Sí |
| Composición del escritorio | No | No | Sí | Sí |
| Mostrar contenido de ventana al arrastrar | No | No | Sí | Sí |
| Animaciones de menús y ventanas | No | No | Sí | Sí |
| Temas | No | Sí | Sí | Sí |
| Almacenar mapas de bits en caché | Sí | Sí | Sí | Sí |

## <a name="screen-size"></a>Tamaño de la pantalla
 Esta opción determina el tamaño de la ventana que se abre en el equipo local cuando se conecte a un equipo remoto a través del sitio web de acceso remoto. El tamaño de ventana se expresa en píxeles.

> [!NOTE]
>  Cuando se conecta al servidor, se abre el panel. El tamaño predeterminado del panel es de 1024 x 741 y puede cambiarse el tamaño.

-   Pantalla completa (varios monitores)

-   1280 x 720

-   1024 x 768

-   800 x 600

-   640 x 480

## <a name="enable-the-remote-computer-to-print-to-my-local-printer"></a>Habilitar el equipo remoto para imprimir en la impresora local
 De forma predeterminada está habilitado. Esta opción permite imprimir en la impresora que está conectada al equipo local desde el equipo remoto.

## <a name="play-sounds-from-the-remote-computer"></a>Reproducir sonidos desde el equipo remoto
 De forma predeterminada está habilitado. Estas opciones permiten reproducir sonidos, como los sonidos del sistema, en el equipo local desde el equipo remoto.

## <a name="enable-copy-and-paste-between-the-remote-computer-and-the-local-computer"></a>Habilitar copiar y pegar entre el equipo remoto y el equipo local
 Esta opción no está habilitada de forma predeterminada. Esta opción permite copiar y pegar los archivos entre el equipo remoto y el equipo local de la misma manera en la que copiaría y pegaría archivos desde una ubicación a otra en el equipo local.

## <a name="enable-the-remote-computer-to-access-drives-on-my-local-computer"></a>Habilitar el equipo remoto para que tenga acceso a las unidades del equipo local
 Esta opción no está habilitada de forma predeterminada. Esta opción permite tener acceso a los archivos y carpetas en las unidades de disco duro que están conectadas al equipo local desde el equipo remoto.

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar Acceso web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Usar Acceso web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)
