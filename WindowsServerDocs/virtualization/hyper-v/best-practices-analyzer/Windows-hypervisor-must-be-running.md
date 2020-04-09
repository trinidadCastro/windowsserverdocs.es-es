---
title: El hipervisor de Windows debe estar en ejecución
description: Proporciona instrucciones para resolver el problema que informa esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: kbdazure
ms.date: 10/03/2016
ms.openlocfilehash: b24700e0ed617177af888013e36f971870d0ac59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860958"
---
# <a name="windows-hypervisor-must-be-running"></a>El hipervisor de Windows debe estar en ejecución

>Se aplica a: Windows Server 2016
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Requisitos previos|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
  
*El hipervisor de Windows no se está ejecutando.*  
  
## <a name="impact"></a>Impacto  
  
*No se pueden iniciar máquinas virtuales hasta que se ejecute el hipervisor de Windows.*  
  
## <a name="resolution"></a>Resolución  
  
*Compruebe el catálogo de Windows Server para ver si el servidor está calificado para ejecutar Hyper-V. A continuación, asegúrese de que el BIOS está habilitado para la virtualización asistida por hardware y la prevención de ejecución de datos aplicada por hardware. A continuación, compruebe el registro de eventos del hipervisor de Hyper-V.*  
  
Para comprobar el catálogo, consulte [Catálogo de Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228).  
  
> [!CAUTION]  
> Cambiar determinados parámetros en el BIOS del sistema de un equipo puede hacer que ese equipo deje de cargar el sistema operativo, o bien puede hacer que los dispositivos de hardware, como las unidades de disco duro, no estén disponibles. Consulte siempre el manual del usuario para el equipo para determinar la manera adecuada de configurar el BIOS del sistema. Además, siempre es una buena idea realizar un seguimiento de los parámetros que se modifican y de su valor original, de modo que se puedan restaurar posteriormente si es necesario. Si experimenta problemas después de cambiar los parámetros en el BIOS del sistema, intente cargar la configuración predeterminada (una opción suele estar disponible en la utilidad de configuración del BIOS) o póngase en contacto con el fabricante del equipo para obtener ayuda.  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Para comprobar la compatibilidad con la virtualización en el BIOS o UEFI  
  
1.  Reinicie el equipo y acceda al BIOS o UEFI a través de la herramienta de configuración de. Normalmente, el acceso a esta herramienta está disponible cuando el equipo pasa por un proceso de arranque. Inmediatamente después de activar la mayoría de los equipos, aparece un mensaje durante unos segundos que enumera la tecla o combinación de teclas que se va a presionar para abrir la herramienta de configuración.  
  
2.  Busque la configuración de virtualización y prevención de ejecución de datos (DEP) forzada en hardware y compruebe que están activadas. A continuación se muestran las ubicaciones de menú comunes para esta configuración en la herramienta de configuración y se muestran ejemplos de su nombre:  
  
    -   Compatibilidad con la virtualización:  
  
        -   Normalmente está disponible en la configuración del procesador o rendimiento principal. A veces está en la configuración de seguridad.  
  
        -   Busque nombres de parámetros que incluyan "virtualización" o "tecnología de virtualización".  
  
    -   DEP forzada por hardware:  
  
        -   Normalmente está disponible en la configuración de seguridad o de memoria.  
  
        -   Busque los nombres de parámetro que incluyen "ejecución", "ejecución" o "prevención".  
  
3.  Si es necesario, active la configuración siguiendo las instrucciones de la herramienta de configuración de. Guarde los cambios y salga.  
  
4.  Si realizó algún cambio, apague el dispositivo y vuelva a encenderlo para finalizar.  
  
    > [!IMPORTANT]  
    > Se recomienda desactivar el apagado y volver a encenderlo (a veces denominado ciclo de energía), ya que los cambios no se aplican en algunos equipos hasta que esto suceda.  
  
A continuación, compruebe el registro de eventos del hipervisor de Hyper-V. Si hay problemas, también comprobará el registro del sistema.  
  
#### <a name="to-check-the-event-logs"></a>Para comprobar los registros de eventos  
  
1.  Abra el Visor de eventos. Haga clic en **Inicio**, en **herramientas administrativas**y, a continuación, en **visor de eventos**.  
  
2.  Abra el registro de eventos del hipervisor de Hyper-V. En el panel de navegación, expanda **registros de aplicaciones y servicios** >> **Microsoft** >> **Windows** >> **Hyper-V-hipervisor**y, a continuación, haga clic en **operativo**.  
  
3.  Si el hipervisor de Windows se está ejecutando, no es necesario realizar ninguna otra acción. Si el hipervisor de Windows no se está ejecutando, haga lo siguiente:  
  
4.  Abra el registro del sistema. (En el panel de navegación, expanda **registros de Windows** y, a continuación, seleccione **sistema**).  
  
5.  Use un filtro para buscar eventos de hipervisor de Hyper-V:   
    1. En el panel **acciones** , haga clic en **filtrar registro actual**. En el caso de los **orígenes de eventos**, especifique "Hyper-V-hipervisor".   
    2. Busque los eventos que notifican problemas. Por ejemplo, el ID. de evento 41 indica un problema con la configuración del BIOS: "error de inicio de Hyper-V. VMX no está presente o no está habilitado en el BIOS ".  
  
### <a name="see-also"></a>Consulta también  
Para obtener más información sobre el uso de Hyper-V en Windows 10, incluido cómo comprobar que el equipo puede ejecutar Hyper-V, consulte [requisitos del sistema de Hyper-v de Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility). 


