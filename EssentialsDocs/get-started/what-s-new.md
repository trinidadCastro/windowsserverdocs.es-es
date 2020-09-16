---
title: Novedades de Windows Server 2016 Essentials
description: Información sobre las novedades de Windows Server 2016 Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: geschuma
ms.openlocfilehash: ecce1760c4768d37936343604d9344a6d489c8cc
ms.sourcegitcommit: efc49ff9062f943abe36c5b6206096315723f56d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90571334"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Novedades de Windows Server 2016 Essentials

> Se aplica a: Windows Server 2016 Essentials

A continuación se enumeran las características nuevas y mejoradas de Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-services"></a>[Integración con servicios de Azure Site Recovery](azure-site-recovery-services-integration.md)

**Qué hace**  -- &reg; Cuando se produce un error en una máquina virtual protegida, o en el servidor host en el que se ejecuta la máquina virtual protegida, la conmutación por error con los servicios de Azure Site Recovery mantiene la continuidad del negocio hasta que la máquina virtual o el servidor host local se reparen y estén disponibles.

**Cómo funciona** : Azure Site Recovery servicios, que se ofrecen en Microsoft Azure, permite la replicación en tiempo real de las máquinas virtuales (VM) en un almacén de copia de seguridad de Azure. En caso de que el servidor o el sitio deje de funcionar debido a un error de hardware o de otro problema, puede realizar una conmutación por error con los servicios de Azure Site Recovery para que la imagen de máquina virtual almacenada en el almacén de copia de seguridad se aprovisione como una máquina virtual en ejecución en Azure. En combinación con una red virtual de Azure, los equipos cliente que se conectaron anteriormente al servidor local se conectarán de forma transparente al servidor que se ejecuta en Azure.

## <a name="integration-with-azure-virtual-network"></a>[Integración con Azure Virtual Network](azure-virtual-network-integration.md)

**Lo que hace**: a medida que las organizaciones hacen su forma de computación en la nube, rara vez mueven todos sus recursos al mismo tiempo. En su lugar, mueven algunos recursos a la nube y mantienen algunos en el entorno local. De este modo, es fácil trasladar una organización a la nube en fases a lo largo del tiempo. La integración de Azure Virtual Network proporciona la infraestructura de red que hace que ese proceso sea sencillo y fácil de administrar.

**Cómo funciona** : la red virtual de Azure es un servicio que se ofrece en Microsoft Azure que permite a las organizaciones crear una red privada virtual de punto a punto (P2P) o de sitio a sitio (s2s) que hace que los recursos que se ejecutan en Azure (como las máquinas virtuales y el almacenamiento) parezcan estar en la red local para obtener acceso a recursos y aplicaciones sin problemas.

## <a name="support-for-larger-deployments"></a>[Compatibilidad con implementaciones de mayor tamaño](support-for-larger-deployments.md)

Algunas pequeñas empresas más grandes necesitan más funcionalidad y capacidad para implementar Windows Server Essentials de manera eficaz. Windows Server 2016 Essentials proporciona una mayor capacidad de administración de dominios, usuarios y dispositivos al agregar compatibilidad con implementaciones de mayor tamaño con:

- varios dominios
- varios controladores de dominio
- capacidad para especificar un controlador de dominio designado

Vea también

- [Introducción a Windows Server Essentials](get-started.md)
