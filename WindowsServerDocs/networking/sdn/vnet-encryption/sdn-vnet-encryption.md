---
title: Cifrado de red virtual
description: Este tema proporciona información acerca del cifrado de red Virtual para Software definido a redes en Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 425cc1cff8c7241aeec7764b60c89b4586fd323b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-network-encryption"></a>Cifrado de red virtual

>Se aplica a: Windows Server

Cifrado de red virtual proporciona la capacidad para el tráfico de red virtual se cifren entre las máquinas virtuales que se comunican entre sí dentro de subredes que están marcadas como "Cifrado habilitado".

Esta característica utiliza la seguridad de capa de transporte de datagramas (DTLS) en la subred virtual para cifrar los paquetes.  DTLS proporciona protección contra interceptaciones, alteraciones y falsificación cualquier persona con acceso a la red física.

Requries de cifrado de red virtual un certificado de cifrado para instalarse en cada uno de lo SDN habilitado hosts de Hyper-V, un objeto de credenciales en el controlador de red que hacen referencia a la huella digital de ese certificado y la configuración de cada una de las redes virtuales que contienen subredes requerir cifrado.

Una vez que el cifrado está habilitado en una subred, todo el tráfico de red dentro de subred se cifra automáticamente.  Esto hará que además de cualquier cifrado a nivel de aplicación que también puede tener lugar.  El tráfico que cruza entre subredes y aunque ambos las subredes se marcan como cifra automáticamente se envía sin cifrar.  Todo el tráfico que cruza los límites de red virtual también se envía sin cifrar.

Para obtener información sobre la configuración de cifrado de red Virtual, vea [configurar el cifrado para una red Virtual](sdn-config-vnet-encryption.md).

Si se debe restringir aplicaciones se comuniquen solo en la subred cifrada.  Puedes usar listas de Control de acceso (ACL) para permitir solamente la comunicación dentro de la subred actual.  

Para obtener información sobre cómo configurar listas de Control de acceso, consulta [usa acceso listas de Control (ACL) para hacer fluir el tráfico de red de Datacenter administrar](../manage/use-acls-for-traffic-flow.md).