---
title: Agregar impresoras
description: Agregue una impresora para los usuarios de Multipoint Services.
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: e1f6d3ca-8caf-4aa0-b0ea-93cdfd3f3f37
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 7be399a73ee37413e01ef714cfd02d4e8750ab79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856088"
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