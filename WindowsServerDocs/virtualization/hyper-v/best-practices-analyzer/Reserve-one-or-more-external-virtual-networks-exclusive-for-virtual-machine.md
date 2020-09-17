---
title: Reservar una o más redes virtuales externas para uso exclusivo de máquinas virtuales
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
ms.date: 8/16/2016
ms.openlocfilehash: 8039f5ef94f1ca762a994607d5a1faf8eac0d98e
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746510"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>Reservar una o más redes virtuales externas para uso exclusivo de máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Todas las redes virtuales externas están configuradas para su uso por parte del sistema operativo de administración y de las máquinas virtuales.*

## <a name="impact"></a>Impacto

*El rendimiento de red puede verse afectado en el sistema operativo de administración.*

## <a name="resolution"></a>Solución

*Use el administrador de conmutadores virtuales para dejar de compartir una red virtual externa con el sistema operativo de administración.*

#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>Para dejar de compartir la red virtual externa con el sistema operativo de administración

1.  Abra el administrador de Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.

2.  En el menú **Acciones**, haz clic en **Administrador de conmutadores virtuales**.

3.  En **conmutadores virtuales**, haga clic en el nombre del conmutador virtual externo.

4.  En el área **tipo de conexión** , en el nombre del adaptador de red físico, desactive la casilla permitir que **el sistema operativo de administración comparta este adaptador de red** .

5.  Haga clic en **Aceptar**.



