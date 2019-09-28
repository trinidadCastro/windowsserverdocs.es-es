---
title: Agregar impresoras
description: Agregue una impresora para los usuarios de Multipoint Services.
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1f6d3ca-8caf-4aa0-b0ea-93cdfd3f3f37
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: e5da3d4919eba9f36e4b28f7c2d2fb3e3de02804
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405138"
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