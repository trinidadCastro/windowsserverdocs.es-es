---
title: Habilitar todos los servicios de integración en máquinas virtuales
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 1984c3d1d6261756bf83f899985b457681537046
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364889"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Habilitar todos los servicios de integración en máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o más servicios de integración están deshabilitados o no funcionan en una máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*Es posible que el servicio o la característica de integración no funcionen correctamente para las siguientes máquinas virtuales:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use el complemento servicios o la herramienta de línea de comandos SC config para comprobar que el servicio está configurado para iniciarse automáticamente y no se ha detenido.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Para configurar el modo en que se inicia un servicio con el complemento Servicios  
  
1.  Use Servicios de Escritorio remoto o conexión a máquina virtual para conectarse a la máquina virtual e iniciar sesión en el sistema operativo invitado.  
  
2.  Abra Servicios. (Haga clic en **Inicio**, haga clic en el cuadro **Iniciar búsqueda** , escriba **Services. msc**y, a continuación, presione Entrar).  
  
3.  En el panel de detalles, haga clic con el botón secundario en el servicio que desee configurar y, a continuación, haga clic en **Propiedades**.  
  
4.  En la pestaña **General** , en tipo de **Inicio** , haga clic en **automático**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Para configurar el modo en que se inicia un servicio con SC config  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **Inicio** y comience a escribir **Windows PowerShell**).  
  
2.  Haga clic con el botón derecho en **Windows PowerShell** y haga clic en **Ejecutar como administrador**.  
  
3.  Reemplace < nombre de servicio > por el nombre del servicio y, a continuación, escriba:  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


