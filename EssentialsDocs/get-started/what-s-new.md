---
title: Novedades de Windows Server 2016 Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 420d3b043959b8b1201aad7a5b3210fd9bd6a0da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817758"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Novedades de Windows Server 2016 Essentials

> Se aplica a: Windows Server 2016 Essentials

A continuación se enumeran las características nuevas y mejoradas de Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-services"></a>[Integración con servicios de Azure Site Recovery](azure-site-recovery-services-integration.md)

**Lo que hace** --&reg;cuando se produce un error en una máquina virtual protegida, o en el servidor host en el que se ejecuta la máquina virtual protegida, la conmutación por error con los servicios de Azure Site Recovery mantiene la continuidad del negocio hasta que la máquina virtual local o el servidor host se repara y está disponible. 

**Cómo funciona** : Azure Site Recovery servicios, que se ofrecen en Microsoft Azure, permite la replicación en tiempo real de las máquinas virtuales (VM) en un almacén de copia de seguridad de Azure. En caso de que el servidor o el sitio deje de funcionar debido a un error de hardware o de otro problema, puede realizar una conmutación por error con los servicios de Azure Site Recovery para que la imagen de máquina virtual almacenada en el almacén de copia de seguridad se aprovisione como una máquina virtual en ejecución en Azure. En combinación con una red virtual de Azure, los equipos cliente que se conectaron anteriormente al servidor local se conectarán de forma transparente al servidor que se ejecuta en Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-network"></a>[Integración con Azure Virtual Network](azure-virtual-network-integration.md)

**Lo que hace**: a medida que las organizaciones hacen su forma de computación en la nube, rara vez mueven todos sus recursos al mismo tiempo. En su lugar, mueven algunos recursos a la nube y mantienen algunos en el entorno local. De este modo, es fácil trasladar una organización a la nube en fases a lo largo del tiempo. La integración de Azure Virtual Network proporciona la infraestructura de red que hace que ese proceso sea sencillo y fácil de administrar.

**Cómo funciona** : la red virtual de Azure es un servicio que se ofrece en Microsoft Azure que permite a las organizaciones crear una red privada virtual de punto a punto (P2P) o de sitio a sitio (s2s) que hace que los recursos que se ejecutan en Azure (como las máquinas virtuales y el almacenamiento) parezcan estar en la red local para obtener acceso a recursos y aplicaciones sin problemas.



## <a name="support-for-larger-deployments"></a>[Compatibilidad con implementaciones de mayor tamaño](support-for-larger-deployments.md) 

Algunas pequeñas empresas más grandes necesitan más funcionalidad y capacidad para implementar Windows Server Essentials de manera eficaz. Windows Server 2016 Essentials proporciona una mayor capacidad de administración de dominios, usuarios y dispositivos al agregar compatibilidad con implementaciones de mayor tamaño con:                                                                                                                                                                                                 

 - varios dominios
 - varios controladores de dominio                                                                                                                                                                                                                                        
 - capacidad para especificar un controlador de dominio designado                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Vea también
--------

Introducción [a Windows Server essentials](get-started.md) &copy;&reg;