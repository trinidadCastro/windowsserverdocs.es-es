---
title: Cifrado de Virtual Network
description: El cifrado de red virtual permite el cifrado del tráfico de red virtual entre máquinas virtuales que se comunican entre sí dentro de subredes marcadas como "cifrado habilitado".
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 72062d002e5530031a99e3b742507277ed5ec490
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990078"
---
# <a name="virtual-network-encryption"></a>Cifrado de Virtual Network

>Se aplica a: Windows Server

El cifrado de red virtual permite el cifrado del tráfico de red virtual entre máquinas virtuales que se comunican entre sí dentro de subredes marcadas como "cifrado habilitado". También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física.

El cifrado de red virtual requiere:
- Certificados de cifrado instalados en cada uno de los hosts de Hyper-V habilitados para SDN.
- Un objeto de credencial de la controladora de red que hace referencia a la huella digital de ese certificado.
- La configuración de cada una de las redes virtuales contiene subredes que requieren cifrado.

Una vez habilitado el cifrado en una subred, todo el tráfico de red dentro de esa subred se cifra automáticamente, además de cualquier cifrado de nivel de aplicación que también pueda tener lugar.  El tráfico que cruza entre subredes, incluso si está marcado como cifrado, se envía automáticamente sin cifrar. El tráfico que cruza el límite de la red virtual también se envía sin cifrar.

>[!TIP]
>Si debe restringir las aplicaciones para que solo se comuniquen en la subred cifrada, puede usar listas de Access Control (ACL) solo para permitir la comunicación dentro de la subred actual. Para obtener más información, consulte [uso de listas de Access Control (ACL) para administrar el flujo de tráfico de red del centro](../manage/use-acls-for-traffic-flow.md)de datos.

### <a name="next-steps"></a>Pasos siguientes

[Configuración del cifrado para una red virtual](./sdn-config-vnet-encryption.md)