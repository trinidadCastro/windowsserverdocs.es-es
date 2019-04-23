---
title: Novedades de SDN para Windows Server
description: Este tema proporciona información sobre las nuevas características de redes definidas por Software para Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: ee9ec68bbbb0be2befd432fbcc692ce177e8fd2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857956"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novedades de SDN para Windows Server de 2019

>Se aplica a: Windows Server (Canal semianual)


| **Característica** | **Descripción** | **Nuevos y actualizados** | 
| --- | --- | --- |
|[Redes cifradas](vnet-encryption/sdn-vnet-encryption.md) |Cifrado de red virtual permite el cifrado de tráfico de red virtual entre las máquinas virtuales que se comunican entre sí dentro de las subredes marcadas como 'El cifrado habilitado'. También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física. |Nuevo |
|[Auditoría del Firewall](security/sdn-firewall-auditing.md) |Auditoría de servidor de seguridad es una nueva funcionalidad para el firewall SDN en Windows Server 2019. Cuando habilita el firewall de SDN, obtiene registra cualquier flujo procesado por las reglas de firewall SDN (ACL) que tienen habilitado el registro. |Nuevo |
|[Emparejamiento de redes virtuales](vnet-peering/sdn-vnet-peering.md) |Emparejamiento de redes virtuales permite conectar dos redes virtuales sin problemas. Una vez emparejadas, efectos de conectividad, las redes virtuales aparecen como una sola.  |Nuevo |
|[Salida de medición](manage/sdn-egress.md) |Esta nueva característica de Windows Server 2019 permite SDN ofrecer los medidores de uso para las transferencias de datos de salida. Con esta característica agregada, mantiene de controladora de red, una lista de permitidos por la red Virtual de todos los intervalos IP usa en SDN y considere la posibilidad de cualquier paquete destinado a un destino que no está incluido en uno de estos intervalos factura las transferencias de datos de salida. |Nuevo |
---



