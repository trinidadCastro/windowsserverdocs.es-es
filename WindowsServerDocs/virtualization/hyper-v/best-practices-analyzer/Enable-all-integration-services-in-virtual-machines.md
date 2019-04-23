---
title: Habilitar todos los servicios de integración de las máquinas virtuales
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 307e2d407a0defa14a6b57bda95a2f3ab018406d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829436"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Habilitar todos los servicios de integración de las máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Uno o varios servicios de integración están deshabilitado o no funciona en una máquina virtual.*  
  
## <a name="impact"></a>Impacto  
  
*La característica de integración o servicio podrían no funcionar correctamente para las máquinas virtuales siguientes:*  
  
\<lista de nombres de máquina virtual >  
  
## <a name="resolution"></a>Resolución  
  
*Use la herramienta Servicios de complemento o sc config de línea de comandos para comprobar que el servicio está configurado para iniciarse automáticamente y no se ha detenido.*  
  
#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Para configurar la forma de un servicio se inicia con el complemento Servicios  
  
1.  Usar servicios de escritorio remoto o conexión a máquina Virtual para conectarse a la máquina virtual y el registro en el sistema operativo invitado.  
  
2.  Abra Servicios. (Haga clic en **iniciar**, haga clic en el **Iniciar búsqueda** , escriba **services.msc**, y, a continuación, presione ENTRAR.)  
  
3.  En el panel de detalles, haga clic con el botón secundario en el servicio que desee configurar y, a continuación, haga clic en **Propiedades**.  
  
4.  En el **General** ficha **inicio** escriba, haga clic en **automática**.  
  
#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Para configurar la forma de un servicio se inicia con SC Config  
  
1.  Abra Windows PowerShell. (En el escritorio, haga clic en **iniciar** y comience a escribir **Windows PowerShell**.)  
  
2.  Haga clic en **Windows PowerShell** y haga clic en **ejecutar como administrador**.  
  
3.  Reemplace < service-name > con el nombre del servicio, a continuación, escriba:  
  
    ```  
    sc config <service-name> start=auto  
    ```  
  


