---
title: Habilitar o deshabilitar la protección de disco
description: Aprenda a usar la protección de disco con Multipoint Services
ms.topic: article
ms.assetid: 00aba4c4-0244-4b39-8c85-c46fd96e1d6a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/07/2020
ms.openlocfilehash: a0e0d1e85cd8212963da802fc7104f12da24f498
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950101"
---
# <a name="enable-or-disable-disk-protection"></a>Habilitar o deshabilitar la protección de disco
La característica Protección de disco permite restablecer el sistema MultiPoint Services a un estado específico cada vez que se reinicia el sistema. Con Protección de disco, los usuarios pueden realizar cambios temporales en el sistema MultiPoint Services, que se descartan al reiniciarse el servidor. Entre los ejemplos de cambios que se descartarán cuando se reinicie el servidor se incluyen personalizar el perfil de un usuario, guardar archivos, cambiar la configuración o instalar aplicaciones.

## <a name="enable-disk-protection"></a>Habilitar protección de disco

1.  En Multipoint Manager, haga clic en la pestaña **Inicio** y, a continuación, en * nombre del equipo ***tareas**, haga clic en **Habilitar protección de disco**.

2.  Revise la información y, a continuación, haga clic en **Aceptar**.

Después de reiniciarse el sistema, los cambios realizados en el mismo, incluida la instalación de aplicaciones nuevas, se descartarán tras cada reinicio posterior.

## <a name="disable-disk-protection"></a>Deshabilitar la protección de disco

1.  En Multipoint Manager, haga clic en la pestaña **Inicio** y, a continuación, en * nombre del equipo ***tareas**, haga clic en **deshabilitar protección de disco**.

2.  Revise la información y, a continuación, haga clic en **Aceptar**.

Después de reiniciarse el sistema, los cambios realizados en el mismo, incluida la instalación de aplicaciones nuevas, serán permanentes y no se descartarán la próxima vez que se reinicie el sistema.

