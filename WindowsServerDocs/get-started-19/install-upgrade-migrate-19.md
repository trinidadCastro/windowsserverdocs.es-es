---
title: Instalar, actualizar o migrar a Windows Server 2019
description: Cómo realizar una instalación limpia, una actualización local o migrar a Windows Server 2019.
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
ms.openlocfilehash: 1fd955a640832eb161666f74b93d91bb2c3eff11
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810821"
---
# <a name="install--upgrade--migrate-to-windows-server-2019"></a>Instalar, actualizar o migrar a Windows Server 2019

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> El soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 termina en enero de 2020. [Obtén información sobre las opciones de actualización](http://aka.ms/upgradecenter).

¿Ha llegado la hora de pasar a una versión más reciente de Windows Server? En función de la versión que estés usando en este momento, tienes muchas opciones para hacerlo.

## <a name="clean-install"></a>Instalación limpia
Si quieres pasar de una versión anterior de Windows Server a Windows Server 2019 en el mismo hardware, debes realizar una **instalación limpia**, en la que basta con instalar el sistema operativo más reciente directamente sobre el más antiguo en el mismo hardware, lo que elimina el sistema operativo anterior. Esta es la forma más sencilla, pero antes tendrás que hacer una copia de seguridad de los datos y planear la reinstalación de las aplicaciones. Hay algunas cosas que debes tener en cuenta, como los requisitos del sistema, así que asegúrate de consultar los detalles referentes a [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) y [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Actualización local

Si quieres mantener el mismo hardware y todos los roles de servidor que has configurado sin reducir el servidor, conviene realizar una **actualización local**, mediante la que se pasa de un antiguo sistema operativo a uno más reciente, manteniendo la configuración, los roles y los datos del servidor intactos. Por ejemplo, si su servidor ejecuta Windows Server 2012 R2, puedes actualizarlo a Windows Server 2016 o Windows Server 2019. Sin embargo, no todos los sistemas operativos antiguos tienen una ruta de actualización a todas las versiones más recientes. Consulte el diagrama siguiente para ver las rutas de actualización disponibles:

![Diagrama de rutas de actualización locales de Windows Server](media/upgrade-paths.png)

Para obtener instrucciones detalladas sobre cómo realizar la actualización, visita el [Centro de actualizaciones de Windows Server](http://aka.ms/upgradecenter):

[![Captura de pantalla del Centro de actualizaciones de Windows Server](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Actualización gradual del sistema operativo del clúster

La actualización gradual del sistema operativo del clúster permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 y Windows Server 2016 sin detener la función Hyper-V o las cargas de trabajo del Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización gradual del sistema operativo del clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migración

La migración de Windows Server consiste en migrar un rol o característica a la vez desde un equipo de origen que ejecute Windows Server a otro equipo de destino que ejecute Windows Server, ya sea la misma versión o una más reciente. Para tales fines, la migración se define como mover un rol o una característica y sus datos a un equipo diferente, no a actualizar la característica en el mismo equipo. 

## <a name="license-conversion"></a>Conversión de licencia
En algunas versiones de los sistemas operativos, es posible convertir una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Esto se denomina **conversión de licencia**. Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter. Ten en cuenta que aunque puedes mover desde Server 2016 Standard a Server 2016 Datacenter, no es posible invertir el proceso e ir de Datacenter a Standard. En algunas versiones de Windows Server, es posible convertir también libremente entre las versiones de OEM, de licencia por volumen y comerciales con el mismo comando y la clave apropiada.


 
 
