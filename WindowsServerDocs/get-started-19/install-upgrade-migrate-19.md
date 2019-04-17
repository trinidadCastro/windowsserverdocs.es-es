---
title: Instalar | Actualizar | Migrar a Windows Server 2019
description: Cómo una instalación limpia, actualización en contexto o migrar a Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 58c363fc0a1e336519bc6ec4276651345cc2b5eb
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121394"
---
# Instalar | Actualizar | Migrar a Windows Server 2019

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> El soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 termina en enero de 2020. [Más información sobre las opciones de actualización](http://aka.ms/upgradecenter).

¿Ha llegado la hora de pasar a una versión más reciente de Windows Server? En función de la versión que estés utilizando en este momento, tienes muchas opciones para hacerlo.

## Instalación limpia
Si quieres mover desde una versión anterior de Windows Server para Windows Server 2019 en el mismo hardware, debes efectuar una **instalación limpia**, donde que basta con instalar el sistema operativo más reciente directamente sobre el más antiguo en el mismo hardware, lo que elimina la sistema operativo anterior. Esta es la forma más sencilla, pero antes tendrás que hacer una copia de seguridad de los datos y planear la reinstalación de las aplicaciones. Hay algunas cosas que debes tener en cuenta, por ejemplo, los requisitos del sistema, así que asegúrate de consultar los detalles para [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418)y [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124).

## Actualización en contexto
Si quieres mantener el mismo hardware y todos los roles de servidor que hayas configurado sin eliminar el servidor, querrás realizar una **actualización en contexto**, por lo que se pasa de un sistema operativo anterior a uno más reciente, y las opciones de configuración, los roles de servidor y los datos intactos. Por ejemplo, si el servidor ejecuta Windows Server 2012 R2, puede actualizarlo a Windows Server 2016 o Windows Server 2019. Sin embargo, no todos los sistemas operativos antiguos tiene una ruta de actualización a todas las versiones más recientes. Consulta el siguiente diagrama para las rutas de actualización disponibles:

![Diagrama de rutas de actualización en contexto de Windows Server](media/upgrade-paths.png)

Para obtener instrucciones paso a paso sobre cómo actualizar, visita el [Centro de actualización de Windows Server](http://aka.ms/upgradecenter):

<a href="http://aka.ms/upgradecenter"><img src="media/upgrade-center.png" alt="Screenshot of Windows Upgrade Center" title="Centro de actualización de Windows Server"></a>

## Actualización del sistema operativo sucesiva de clúster
Actualización gradual de clúster del sistema operativo permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 y Windows Server 2016 sin detener el Hyper-V o las cargas de trabajo del servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización gradual del sistema operativo del clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Migración

Migración de Windows Server es cuando se mueve un rol o característica cada vez desde un equipo de origen que ejecute Windows Server a otro equipo de destino que se está ejecutando Windows Server, ya sea la misma o una versión más reciente. Para estos fines, la migración se define como mover un rol o una característica y sus datos a un equipo diferente, no a actualizar la característica en el mismo equipo. 

## Conversión de licencia
En algunas versiones de los sistemas operativos, es posible convertir una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Esto se denomina **conversión de licencia**. Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter. En algunas versiones de Windows Server, es posible convertir también libremente entre las versiones de OEM, de licencia por volumen y comerciales con el mismo comando y la clave apropiada.


 
 
