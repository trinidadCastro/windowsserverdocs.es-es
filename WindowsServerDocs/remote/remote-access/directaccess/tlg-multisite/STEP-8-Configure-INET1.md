---
title: PASO 8 configurar INET1
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 2e53eb8af7327932123fde8eb138611e5ae65242
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953681"
---
# <a name="step-8-configure-inet1"></a>PASO 8: Configurar INET1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para permitir que los equipos cliente se conecten a los servidores de acceso remoto a través de Internet, debe configurar una entrada DNS para 2-EDGE1 en INET1.

### <a name="to-create-the-2-edge1-dns-entry"></a>Para crear la entrada DNS 2-EDGE1

1.  En la pantalla **Inicio** , escriba**DNSMgmt. msc**y, a continuación, presione Entrar.

2.  En el árbol de consola, Abra **zonas de búsqueda directa**, haga clic en **contoso.com**, haga clic con el botón secundario en **contoso.com**y, a continuación, haga clic en **host nuevo (a o aaaa)**.

3.  En **nombre**, escriba **2-EDGE1**. En **dirección IP**, escriba **131.107.0.20**. Haga clic en **Agregar host**, haga clic en **Aceptar** y, después, haga clic en **Listo**.



