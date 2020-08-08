---
title: Agregar impresoras
description: Agregue una impresora para los usuarios de Multipoint Services.
ms.topic: article
ms.assetid: e1f6d3ca-8caf-4aa0-b0ea-93cdfd3f3f37
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 8fa721361e528aba4d72d9b96b8e71a0a3b12d3b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969322"
---
# <a name="add-printers"></a>Agregar impresoras
Use los procedimientos de este tema para que una impresora local esté disponible para todos los usuarios en un sistema Multipoint Services.

> [!NOTE]
> Si usa cuentas de dominio con Multipoint Services, los usuarios pueden usar cualquier impresora de red de sus estaciones.

1.  Conecte la impresora al servidor multipoint.

2.  Configurar la impresora como una impresora compartida:

    1.  Inicie sesión en el equipo de MultiPoint Server como administrador.

    2.  En la pantalla **Inicio**, abra el **Panel de control**.

    3.  En el panel de control, haga clic en **hardware**y, a continuación, haga clic en **dispositivos e impresoras**.

    4.  En **impresoras y faxes**, haga clic con el botón secundario en la impresora y, a continuación, haga clic en **propiedades de impresora**.

    5.  Haga clic en la pestaña **uso compartido** .

    6.  Haga clic en **compartir esta impresora**, especifique un nombre de recurso compartido para la impresora y, a continuación, haga clic en **Aceptar**.

Los usuarios que iniciaron sesión en cualquier estación que esté conectada al equipo de Multipoint Services podrán ver y usar la impresora.