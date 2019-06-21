---
title: PASO 8 configurar INET1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: d6967b975b3a950c90de465872832d623755a494
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281412"
---
# <a name="step-8-configure-inet1"></a>PASO 8: Configurar INET1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para habilitar los equipos cliente para conectarse a servidores de acceso remoto a través de Internet, debe configurar una entrada DNS para 2 EDGE1 en INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Para crear la entrada DNS 2 EDGE1  
  
1.  En el **iniciar** , escriba**dnsmgmt.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el árbol de consola, abra **zonas de búsqueda directa**, haga clic en **contoso.com**, a continuación, haga clic en **contoso.com**y, a continuación, haga clic en **Host nuevo (A o AAAA)** .  
  
3.  En **nombre**, tipo **2 EDGE1**. En **dirección IP**, tipo **131.107.0.20**. Haga clic en **Agregar host**, haga clic en **Aceptar**y, después, haga clic en **Listo**.  
  


