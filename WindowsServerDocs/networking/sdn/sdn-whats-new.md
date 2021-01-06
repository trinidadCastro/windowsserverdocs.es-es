---
title: Novedades de SDN para Windows Server
description: En este tema se proporciona información sobre las nuevas características de redes definidas por software para Windows Server 1709
manager: grcusanz
ms.topic: how-to
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: e4552447d4a7e3c22b83608747f1d975494c5ce8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949981"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novedades de SDN para Windows Server 2019

>Se aplica a: Windows Server (Canal semianual)


|                         **Característica**                          |                                                                                                                                                                                         **Descripción**                                                                                                                                                                                         | **Nueva/actualizada** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Redes cifradas](vnet-encryption/sdn-vnet-encryption.md) | El cifrado de red virtual permite el cifrado del tráfico de red virtual entre máquinas virtuales que se comunican entre sí dentro de subredes marcadas como "cifrado habilitado". También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física. |       Nuevo       |
|    [Auditoría de firewall](security/sdn-firewall-auditing.md)    |                                                                                            La auditoría de Firewall es una nueva funcionalidad para el Firewall de SDN en Windows Server 2019. Al habilitar el Firewall de SDN, se registra cualquier flujo procesado por las reglas de Firewall de SDN (ACL) que tienen habilitado el registro.                                                                                            |       Nuevo       |
| [Interconexión de red virtual](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      El emparejamiento de redes virtuales permite conectar dos redes virtuales sin problemas. Una vez emparejadas, para fines de conectividad, las redes virtuales aparecen como una sola.                                                                                                                      |       Nuevo       |
|           [Medición de salida](manage/sdn-egress.md)            |                  Esta nueva característica de Windows Server 2019 permite a SDN ofrecer medidores de uso para las transferencias de datos de salida. Con esta característica agregada, la controladora de red mantiene un permitidos por Virtual Network de todos los intervalos IP que se usan en SDN y tiene en cuenta cualquier paquete enlazado a un destino que no esté incluido en uno de estos intervalos para facturar las transferencias de datos de salida.                   |       Nuevo       |

---



