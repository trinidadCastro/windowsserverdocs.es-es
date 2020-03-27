---
title: Servicio de nombres Internet de Windows (WINS)
description: En este tema se proporciona información acerca de la retirada de WINS y el uso de DNS para los servicios de resolución de nombres en la red.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 54e3f69500ccb3dbf6b2dfe47f6dd035e0a20511
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315163"
---
#  <a name="windows-internet-name-service-wins"></a>Servicio de nombres Internet de Windows (WINS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP.

Si aún no tiene WINS implementado en la red, no implemente WINS en su lugar, implemente el sistema de nombres de dominio \(DNS\). DNS también proporciona servicios de resolución y registro de nombres de equipo e incluye muchas ventajas adicionales sobre WINS, como la integración con Active Directory Domain Services.

Para obtener más información, consulte [sistema de nombres de dominio (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si ya ha implementado WINS en la red, se recomienda implementar DNS y, a continuación, retirar WINS.
