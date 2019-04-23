---
title: Cifrado de red virtual
description: Cifrado de red virtual permite el cifrado de tráfico de red virtual entre las máquinas virtuales que se comunican entre sí dentro de las subredes marcadas como 'El cifrado habilitado'.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: f2f50ae3146854e2ef6081b0c400a474b53dcf66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851086"
---
# <a name="virtual-network-encryption"></a>Cifrado de red virtual

>Se aplica a: Windows Server

Cifrado de red virtual permite el cifrado de tráfico de red virtual entre las máquinas virtuales que se comunican entre sí dentro de las subredes marcadas como 'El cifrado habilitado'. También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física.

Se requiere el cifrado de red virtual:
- Certificados de cifrado en cada uno de los hosts de Hyper-V habilitado SDN.
- Un objeto de credencial en la controladora de red que hacen referencia a la huella digital del certificado.
- La configuración en cada una de las redes virtuales contiene subredes que requieren cifrado.

Una vez que habilite el cifrado en una subred, todo el tráfico de red dentro de esa subred se cifra automáticamente, además de cualquier cifrado de nivel de aplicación que también puede tener lugar.  El tráfico que cruza entre subredes, incluso si marcada como cifrada, se envía sin cifrar automáticamente. Todo el tráfico que cruza el límite de red virtual también se envía sin cifrar.

>[!TIP]
>Si debe restringir las aplicaciones se comuniquen solo en la subred cifrada, puede usar listas de Control de acceso (ACL) sólo para permitir la comunicación dentro de la subred actual. Para obtener más información, consulte [usar Access Control Lists (ACL) para el flujo de tráfico de red de centro de datos de administrar](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

### <a name="next-steps"></a>Pasos siguientes

[Configurar el cifrado de una red virtual](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

