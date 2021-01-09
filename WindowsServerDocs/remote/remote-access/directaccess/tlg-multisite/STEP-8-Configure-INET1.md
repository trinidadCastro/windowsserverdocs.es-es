---
title: PASO 8 configurar INET1
description: Aprenda a configurar una entrada DNS para 2-EDGE1 en INET1.
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 08/07/2020
ms.openlocfilehash: 0a17e726d200cc344c83f2d4aafed2f5493b294a
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040375"
---
# <a name="step-8-configure-inet1"></a>PASO 8: Configurar INET1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para permitir que los equipos cliente se conecten a los servidores de acceso remoto a través de Internet, debe configurar una entrada DNS para 2-EDGE1 en INET1.

### <a name="to-create-the-2-edge1-dns-entry"></a>Para crear la entrada DNS 2-EDGE1

1.  En la pantalla **Inicio** , escriba **DNSMgmt. msc** y, a continuación, presione Entrar.

2.  En el árbol de consola, Abra **zonas de búsqueda directa**, haga clic en **contoso.com**, haga clic con el botón secundario en **contoso.com** y, a continuación, haga clic en **host nuevo (a o aaaa)**.

3.  En **nombre**, escriba **2-EDGE1**. En **dirección IP**, escriba **131.107.0.20**. Haga clic en **Agregar host**, haga clic en **Aceptar** y, después, haga clic en **Listo**.



