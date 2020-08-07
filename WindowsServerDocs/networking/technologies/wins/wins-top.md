---
title: Servicio de nombres Internet de Windows (WINS)
description: En este tema se proporciona información acerca de la retirada de WINS y el uso de DNS para los servicios de resolución de nombres en la red.
manager: brianlic
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 36dc2b0e8bbb6b65b0cc3568641017aa51122650
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955502"
---
#  <a name="windows-internet-name-service-wins"></a>Servicio de nombres Internet de Windows (WINS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El servicio de nombres Internet de Windows (WINS) es un servicio de resolución y registro de nombres de equipo heredado que asigna nombres NetBIOS de equipo a direcciones IP.

Si aún no tiene WINS implementado en la red, no implemente WINS en su lugar, implemente el DNS del sistema de nombres de dominio \( \) . DNS también proporciona servicios de resolución y registro de nombres de equipo e incluye muchas ventajas adicionales sobre WINS, como la integración con Active Directory Domain Services.

Para obtener más información, consulte [sistema de nombres de dominio (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si ya ha implementado WINS en la red, se recomienda implementar DNS y, a continuación, retirar WINS.
