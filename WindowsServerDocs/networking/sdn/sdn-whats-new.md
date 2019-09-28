---
title: Novedades de SDN para Windows Server
description: En este tema se proporciona información sobre las nuevas características de redes definidas por software para Windows Server 1709
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 09fcf8e3faf8d8b7f943255599dd3b273314db23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405995"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Novedades de SDN para Windows Server 2019

>Se aplica a: Windows Server (Canal semianual)


|                         **Característica**                          |                                                                                                                                                                                         **Descripción**                                                                                                                                                                                         | **Nuevo/actualizado** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Redes cifradas](vnet-encryption/sdn-vnet-encryption.md) | El cifrado de red virtual permite el cifrado del tráfico de red virtual entre máquinas virtuales que se comunican entre sí dentro de subredes marcadas como "cifrado habilitado". También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física. |       Nuevo       |
|    [Auditoría de Firewall](security/sdn-firewall-auditing.md)    |                                                                                            La auditoría de Firewall es una nueva funcionalidad para el Firewall de SDN en Windows Server 2019. Al habilitar el Firewall de SDN, se registra cualquier flujo procesado por las reglas de Firewall de SDN (ACL) que tienen habilitado el registro.                                                                                            |       Nuevo       |
| [Interconexión de red virtual](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      El emparejamiento de redes virtuales permite conectar dos redes virtuales sin problemas. Una vez emparejadas, para fines de conectividad, las redes virtuales aparecen como una sola.                                                                                                                      |       Nuevo       |
|           [Medición de salida](manage/sdn-egress.md)            |                  Esta nueva característica de Windows Server 2019 permite a SDN ofrecer medidores de uso para las transferencias de datos de salida. Con esta característica agregada, la controladora de red mantiene una lista de permitidos por Virtual Network de todos los intervalos de direcciones IP que se usan en SDN y tiene en cuenta que cualquier paquete enlazado a un destino que no esté incluido en uno de estos intervalos para facturar las transferencias de datos de salida.                   |       Nuevo       |

---



