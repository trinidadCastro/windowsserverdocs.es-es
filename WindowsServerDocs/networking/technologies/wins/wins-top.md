---
title: Servicio de nombres de Windows Internet (WINS)
description: En este tema proporciona información sobre la retirada de WINS y utiliza DNS para servicios de resolución de nombres de la red.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a3d132ada7b1ede83b046499058399a9da12190
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
#  <a name="windows-internet-name-service-wins"></a>Servicio de nombres de Windows Internet (WINS)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Servicio de nombres de Windows Internet (WINS) es un servicio nombre de equipo heredado registro y la resolución que asigna los nombres de equipo NetBIOS a direcciones IP.

Si todavía no tienes WINS implementados en la red, no implemente WINS - en su lugar, implementar el sistema de nombres de dominio \(DNS\). DNS también proporciona servicios de registro y la resolución de nombres de equipo e incluye muchas ventajas adicionales en WINS, como la integración con los servicios de dominio de Active Directory.

Para obtener más información, consulta [sistema de nombres de dominio (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si ya has implementado WINS en la red, se recomienda que implementar DNS y, a continuación, baja WINS.
