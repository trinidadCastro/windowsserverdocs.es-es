---
title: Novedades de Windows Server 2016 Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: affff774-5fa6-4944-887a-9bfde05f6a3f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bc83686f76c49773203d63a88894841f65ffd1d9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433760"
---
# <a name="whats-new-in-windows-server-2016-essentials"></a>Novedades de Windows Server 2016 Essentials

> Se aplica a: Windows Server 2016 Essentials

A continuación es nuevas y mejoradas características de Windows Server 2016 Essentials.

## <a name="integration-with-azure-site-recovery-servicesazure-site-recovery-services-integrationmd"></a>[Integración con servicios de Azure Site Recovery](azure-site-recovery-services-integration.md)

**Lo que hace** : cuando una máquina virtual que está protegido se produce un error o el servidor host que ejecuta la máquina virtual protegida en produce un error, conmutación por error con Azure Site Recovery Services mantiene la continuidad del negocio hasta que la máquina virtual de localmente en o servidor host está reparada y disponible. 

**Cómo funciona** --Azure Site Recovery Services, que se ofrecen en Microsoft Azure, permite la replicación en tiempo real de las máquinas virtuales (VM) en un almacén de copia de seguridad en Azure. En caso de que el servidor o el sitio deja de funcionar debido a un hardware u otro error, puede conmutar por error con Azure Site Recovery Services para que la imagen de máquina virtual almacenada en el almacén de copia de seguridad se aprovisionará como una máquina virtual en ejecución en Azure. En combinación con una red Virtual de Azure, los equipos que había conectado al servidor local cliente transparente se conectarán al servidor que se ejecuta en Azure.     
                                                                                                                                                                                                                                                                                                               

## <a name="integration-with-azure-virtual-networkazure-virtual-network-integrationmd"></a>[Integración con red Virtual de Azure](azure-virtual-network-integration.md)

**Lo que hace**: las organizaciones lleguen cloud computing, que rara vez mover todos sus recursos al mismo tiempo. En su lugar, se mueven algunos recursos a la nube y mantener algunos en el entorno local. De este modo, es fácil mover una organización a la nube en fases con el tiempo. Integración de Azure virtual Network proporciona la infraestructura de red que hace que el proceso sin problemas y fáciles de administrar.

**Cómo funciona** --redes virtuales de Azure son un servicio que se ofrecen en Microsoft Azure que permite a las organizaciones crear un punto a punto (P2P) o de sitio a sitio (S2S) red privada virtual que permite que se ejecutan en Azure (como los recursos las máquinas virtuales y almacenamiento) se ven como si estuvieran en la red local para la aplicación sin problemas y acceso a los recursos.



## <a name="support-for-larger-deploymentssupport-for-larger-deploymentsmd"></a>[Soporte técnico para implementaciones más grandes](support-for-larger-deployments.md) 

Algunas pequeñas empresas más grandes necesitan más funcionalidad y capacidad para implementar Windows Server Essentials de manera eficaz. Windows Server 2016 Essentials proporciona una mayor capacidad de administración de dominios, usuarios y dispositivos mediante la adición de compatibilidad para implementaciones más grandes con:                                                                                                                                                                                                 

 - varios dominios
 - varios controladores de dominio                                                                                                                                                                                                                                        
 - capacidad de especificar un controlador de dominio designado                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Vea también
--------

[Introducción a Windows Server Essentials](get-started.md)
