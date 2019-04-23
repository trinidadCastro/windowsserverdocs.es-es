---
title: PASO 8 configurar INET1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 707591604a1d030b3abba9395081d2c2e4b7fb1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838816"
---
# <a name="step-8-configure-inet1"></a>PASO 8: Configurar INET1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para habilitar los equipos cliente para conectarse a servidores de acceso remoto a través de Internet, debe configurar una entrada DNS para 2 EDGE1 en INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Para crear la entrada DNS 2 EDGE1  
  
1.  En el **iniciar** , escriba**dnsmgmt.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, abra **zonas de búsqueda directa**, haga clic en **contoso.com**, a continuación, haga clic en **contoso.com**y, a continuación, haga clic en **Host nuevo (A o AAAA)**.  
  
3.  En **nombre**, tipo **2 EDGE1**. En **dirección IP**, tipo **131.107.0.20**. Haga clic en **Agregar host**, haga clic en **Aceptar**y, después, haga clic en **Listo**.  
  


