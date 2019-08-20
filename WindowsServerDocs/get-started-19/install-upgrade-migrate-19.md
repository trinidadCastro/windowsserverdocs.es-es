---
title: Instalación, actualización o migración a Windows Server 2019
description: Cómo realizar una instalación limpia, una actualización local o migrar a Windows Server 2019
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4e99cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 7e90738a157f620124bfca3d5f1f4c12789d3bf2
ms.sourcegitcommit: b17ccf7f81e58e8f4dd844be8acf784debbb20ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69023915"
---
# <a name="install-upgrade-or-migrate-to-windows-server"></a>Instalación, actualización o migración a Windows Server 2019

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> El soporte técnico ampliado para Windows Server 2008 R2 y Windows Server 2008 termina en enero de 2020. [Obtén información sobre las opciones de actualización](http://aka.ms/upgradecenter). Para descargar Windows Server 2019, consulta [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

¿Ha llegado la hora de pasar a una versión más reciente de Windows Server? En función de la versión que estés usando en este momento, tienes muchas opciones para hacerlo.

## <a name="clean-install"></a>Instalación limpia

La manera más sencilla de instalar Windows Server es realizar una instalación limpia, donde se instala en un servidor en blanco o se sobrescribe un sistema operativo existente. Esta es la forma más sencilla, pero antes tendrás que hacer una copia de seguridad de los datos y planear la reinstalación de las aplicaciones. Hay algunas cosas que debes tener en cuenta, como los requisitos del sistema, así que asegúrate de consultar los detalles referentes a [Windows Server 2019](https://go.microsoft.com/fwlink/?linkid=2006124), [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) y [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

## <a name="in-place-upgrade"></a>Actualización en contexto

Si quieres mantener el mismo hardware y todos los roles de servidor que has configurado sin reducir el servidor, conviene realizar una **actualización local**, mediante la que se pasa de un antiguo sistema operativo a uno más reciente, manteniendo la configuración, los roles y los datos del servidor intactos. Por ejemplo, si su servidor ejecuta Windows Server 2012 R2, puedes actualizarlo a Windows Server 2016 o Windows Server 2019. Sin embargo, no todos los sistemas operativos antiguos tienen una ruta de actualización a todas las versiones más recientes. 

Para obtener instrucciones detalladas sobre cómo realizar la actualización, visita el [Centro de actualizaciones de Windows Server](http://aka.ms/upgradecenter):

[![Captura de pantalla del Centro de actualizaciones de Windows Server](media/upgrade-center.png)](http://aka.ms/upgradecenter)

## <a name="cluster-os-rolling-upgrade"></a>Actualización gradual del sistema operativo del clúster

La actualización gradual del sistema operativo del clúster permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 y Windows Server 2016 sin detener la función Hyper-V o las cargas de trabajo del Servidor de archivos de escalabilidad horizontal. Esta característica permite evitar tiempos de inactividad que podrían afectar a los contratos de nivel de servicio. Esta nueva característica se describe con más detalle en [Actualización gradual del sistema operativo del clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="migration"></a>Migración

La migración de Windows Server consiste en migrar un rol o característica a la vez desde un equipo de origen que ejecute Windows Server a otro equipo de destino que ejecute Windows Server, ya sea la misma versión o una más reciente. Para tales fines, la migración se define como mover un rol o una característica y sus datos a un equipo diferente, no a actualizar la característica en el mismo equipo. 

## <a name="license-conversion"></a>Conversión de licencia

En algunas versiones de los sistemas operativos, es posible convertir una edición concreta de la versión a otra edición de la misma versión en un solo paso, con un sencillo comando y la clave de licencia correspondiente. Esto se denomina **conversión de licencia**. Por ejemplo, si el servidor ejecuta Windows Server 2016 Standard, puedes realizar la conversión a Windows Server 2016 Datacenter. Ten en cuenta que aunque puedes mover desde Server 2016 Standard a Server 2016 Datacenter, no es posible invertir el proceso e ir de Datacenter a Standard. En algunas versiones de Windows Server, es posible convertir también libremente entre las versiones de OEM, de licencia por volumen y comerciales con el mismo comando y la clave apropiada.