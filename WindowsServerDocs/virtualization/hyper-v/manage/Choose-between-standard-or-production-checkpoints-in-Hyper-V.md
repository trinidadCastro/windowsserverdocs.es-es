---
title: Elija entre los puntos de control estándares o de producción en Hyper-V
description: Proporciona instrucciones para configurar una máquina virtual para usar puntos de control estándares o de producción
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3591e17c9485fc8f9e365f6322c4f48e783db8ce
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442226"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>Elija entre los puntos de control estándares o de producción en Hyper-V

>Se aplica a: Windows 10, Windows Server 2016 y Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

  
A partir de Windows Server 2016 y Windows 10, puede elegir entre los puntos de control estándar y de producción para cada máquina virtual. Los puntos de control de producción son el valor predeterminado para nuevas máquinas virtuales.
  
- Los puntos de control de producción son "en tiempo de" imágenes de una máquina virtual, que se pueden restaurar más adelante de forma que es completamente compatible con todas las cargas de trabajo de producción. Esto se consigue mediante el uso de tecnología de copia de seguridad dentro del invitado para crear el punto de control, en lugar de usar tecnología de estado guardado.  
  
- Puntos de control estándares capturan la configuración de estado, datos y hardware de una máquina virtual en ejecución y están pensadas para su uso en escenarios de desarrollo y pruebas. Los puntos de control estándares pueden ser útiles si tiene que volver a crear un estado específico o una condición de una máquina virtual en ejecución, por lo que puede solucionar un problema.  
 
  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>Cambiar los puntos de control de producción ni en los puntos de control estándares  
  
1.  En **Administrador de Hyper-V**, haga clic en la máquina virtual y haga clic en **configuración**.  
  
2.  En el **administración** sección, seleccione **puntos de control**.  
  
3.  Seleccione los puntos de control de producción o los puntos de control estándares.  
  
    Si elige los puntos de control de producción, también puede especificar si el host debe tener un punto de control estándar si no se pueden tomar un punto de control de producción. Si desactiva esta casilla de verificación y no se pueden tomar un punto de control de producción, a continuación, no se realiza ningún punto de comprobación.  
  
4.  Si desea almacenar los archivos de configuración de punto de control en un lugar diferente, cambiar en el **ubicación del archivo de punto de comprobación** sección.  
  
5.  Haga clic en **aplicar** para guardar los cambios. Si ha terminado, haga clic en **Aceptar** para cerrar el cuadro de diálogo.  
  
> [!NOTE]
> Solo **puntos de control de producción** se admiten en los invitados que ejecutan el rol de servicios de dominio de Active Directory (controlador de dominio) o el rol de Active Directory Lightweight Directory Services.

## <a name="see-also"></a>Vea también  
  
-   [Puntos de control de producción](../What-s-new-in-Hyper-V-on-Windows.md#BKMK_check)  
  
-   [Habilitar o deshabilitar puntos de control](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


