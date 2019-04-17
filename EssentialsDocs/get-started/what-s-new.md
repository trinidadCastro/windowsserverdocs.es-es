---
title: Novedades en Windows Server 2016 Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 7fed7e71f7ac163437fe5d32da7c867f93fbcf00
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
#<a name="whats-new-in-windows-server-2016-essentials"></a>Novedades en Windows Server 2016 Essentials

> Se aplica a: Windows Server 2016 Essentials

A continuación es nuevas y mejoradas características en Windows Server 2016 Essentials.

##[<a name="integration-with-azure-site-recovery-services"></a>Integración con los servicios de recuperación de sitios de Azure](azure-site-recovery-services-integration.md)

**Lo que hace** : cuando una máquina virtual que está protegida se produce un error, o el servidor host que se ejecuta la máquina virtual protegida en se produce un error de producir un error con los servicios de recuperación de sitios de Azure mantiene hasta que la máquina virtual de local en la continuidad del negocio o servidor host está reparado y disponible. 

**Cómo funciona** --servicios de recuperación del sitio de Azure, que se ofrecen en Microsoft Azure, permite la replicación en tiempo real de las máquinas virtuales (VM) a un depósito de copia de seguridad en Azure. En caso de que el servidor o el sitio deja de funcionar por un hardware u otro error, puede conmutación por error con los servicios de recuperación de sitios de Azure para que la imagen de máquina virtual almacenada en el almacén de copia de seguridad se aprovisionará como una VM en ejecución en Azure. En combinación con una red Virtual de Azure, equipos que conectado anteriormente en el servidor local de cliente de forma transparente se conectan al servidor que ejecuta en Azure.     
                                                                                                                                                                                                                                                                                                               

## [<a name="integration-with-azure-virtual-network"></a>Integración con redes virtuales de Azure](azure-virtual-network-integration.md)

**Lo que hace**, como las organizaciones hacer su forma de informática, que rara vez mover todos sus recursos al mismo tiempo en la nube. En su lugar, se mueve algunos recursos en la nube y mantener algunos en locales. De este modo, es fácil mover una organización a la nube en fases con el tiempo. Integración de red virtual Azure proporciona la infraestructura de red que hace que el proceso sencilla y fácil de administrar.

**Cómo funciona** : redes virtuales de Azure son un servicio que se ofrecen en Microsoft Azure que permite a las organizaciones crear un punto a punto (P2P) o el sitio a sitio (S2S) red privada virtual que hace que los recursos que se ejecutan en Azure (como máquinas virtuales y almacenamiento) aspecto como si fueran en la red local acceso a la aplicación y los recursos sin problemas.



##[<a name="support-for-larger-deployments"></a>Soporte técnico para realizar implementaciones grandes](support-for-larger-deployments.md) 

Algunas pequeñas empresas más grandes necesitan más funcionalidad y capacidad para implementar Windows Server Essentials de manera eficaz. Windows Server 2016 Essentials ofrece una mayor capacidad de administración de dominios, los usuarios y dispositivos agregando compatibilidad para implementaciones más grandes con:                                                                                                                                                                                                 

 - varios dominios
 - varios controladores de dominio                                                                                                                                                                                                                                        
 - capacidad para especificar un controlador de dominio designado                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

<a name="see-also"></a>Consulta también
--------

[Introducción a Windows Server Essentials](get-started.md)
