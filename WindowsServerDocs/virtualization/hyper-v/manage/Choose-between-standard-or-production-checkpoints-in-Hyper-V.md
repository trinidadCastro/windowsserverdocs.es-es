---
title: Elección entre los puntos de control estándar o de producción en Hyper-V
description: Proporciona instrucciones para configurar una máquina virtual para usar puntos de control estándar o de producción.
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: 00a3c8d94fc18d180faa8927b33b90dc98854da0
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746480"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>Elección entre los puntos de control estándar o de producción en Hyper-V

>Se aplica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019


A partir de Windows Server 2016 y Windows 10, puede elegir entre los puntos de control estándar y de producción para cada máquina virtual. Los puntos de control de producción son el valor predeterminado para las nuevas máquinas virtuales.

- Los puntos de control de producción son imágenes "a un momento dado" de una máquina virtual, que se pueden restaurar más adelante de forma que se admitan por completo para todas las cargas de trabajo de producción. Esto se consigue mediante el uso de tecnología de copia de seguridad dentro del invitado para crear el punto de control, en lugar de usar tecnología de estado guardado.

- Los puntos de control estándar capturan el estado, los datos y la configuración de hardware de una máquina virtual en ejecución y están pensados para su uso en escenarios de desarrollo y pruebas. Los puntos de control estándar pueden ser útiles si necesita volver a crear una condición o estado específico de una máquina virtual en ejecución para que pueda solucionar un problema.

  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>Cambiar los puntos de control a los puntos de control de producción o estándar

1.  En el **Administrador de Hyper-V**, haga clic con el botón secundario en la máquina virtual y haga clic en **configuración**.

2.  En la sección **Administración** , seleccione **puntos de control**.

3.  Seleccione los puntos de control de producción o los puntos de control estándares.

    Si elige puntos de control de producción, también puede especificar si el host debe tomar un punto de control estándar si no se puede tomar un punto de control de producción. Si desactiva esta casilla y no se puede tomar un punto de control de producción, no se toma ningún punto de control.

4.  Si desea almacenar los archivos de configuración de punto de comprobación en un lugar diferente, cámbielo en la sección **Ubicación del archivo de punto de comprobación** .

5.  Haga clic en **aplicar** para guardar los cambios. Si ya ha terminado, haga clic en **Aceptar** para cerrar el cuadro de diálogo.

> [!NOTE]
> Solo se admiten **puntos de control de producción** en invitados que ejecutan Active Directory Domain Services rol (controlador de dominio) o rol de Active Directory Lightweight Directory Services.

## <a name="additional-references"></a>Referencias adicionales

-   [Puntos de control de producción](../What-s-new-in-Hyper-V-on-Windows.md#production-checkpoints-new)

-   [Habilitar o deshabilitar puntos de control](Enable-or-disable-checkpoints-in-Hyper-V.md)



