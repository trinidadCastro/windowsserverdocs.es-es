---
title: Servicio de nombres Internet de Windows (WINS)
description: En este tema proporciona información sobre la retirada de WINS y el uso de DNS para servicios de resolución de nombres en la red.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbc1871d29021aa3c99f14368a4711dac63f4cee
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843636"
---
#  <a name="windows-internet-name-service-wins"></a>Servicio de nombres Internet de Windows (WINS)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El servicio WINS es un servicio heredado de registro y resolución de nombres de ordenadores que asigna nombres NetBIOS de ordenador a direcciones IP.

Si no dispone de WINS implementados en la red, no implementan WINS - en su lugar, implemente el sistema de nombres de dominio \(DNS\). DNS incluye muchos beneficios adicionales a través de WINS, como la integración con Active Directory Domain Services y también proporciona servicios de registro y la resolución de nombres de equipo.

Para obtener más información, consulte [Domain Name System (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Si ya ha implementado WINS en la red, se recomienda que implementar DNS y, a continuación, dar de baja WINS.
