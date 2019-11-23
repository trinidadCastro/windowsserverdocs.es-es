---
title: Servicio de nombres Internet de Windows (WINS)
description: En este tema se proporciona información acerca de la retirada de WINS y el uso de DNS para los servicios de resolución de nombres en la red.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c313dfeb26319cd5f537df417724de7044648
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405251"
---
#  <a name="windows-internet-name-service-wins"></a>Servicio de nombres Internet de Windows (WINS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP.

Si aún no tiene WINS implementado en la red, no implemente WINS en su lugar, implemente el sistema de nombres de dominio \(DNS\). DNS también proporciona servicios de resolución y registro de nombres de equipo e incluye muchas ventajas adicionales sobre WINS, como la integración con Active Directory Domain Services.

Para obtener más información, consulte [sistema de nombres de dominio (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si ya ha implementado WINS en la red, se recomienda implementar DNS y, a continuación, retirar WINS.
