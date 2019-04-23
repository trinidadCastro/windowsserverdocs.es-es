---
title: Debe estar ejecutando el hipervisor de Windows
description: Proporciona instrucciones para resolver el problema notificado por esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: f34e10918c60fb602c3a88ef3434cda619b11d8d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884146"
---
# <a name="windows-hypervisor-must-be-running"></a>Debe estar ejecutando el hipervisor de Windows

>Se aplica a: Windows Server 2016
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Requisitos previos|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Hipervisor de Windows no se está ejecutando.*  
  
## <a name="impact"></a>Impacto  
  
*No se puede iniciar las máquinas virtuales hasta que se ejecuta el hipervisor de Windows.*  
  
## <a name="resolution"></a>Resolución  
  
*Compruebe el catálogo de Windows Server para ver si este servidor se califica para ejecutar Hyper-V. A continuación, asegúrese de que el BIOS está habilitado para la virtualización asistida por hardware y prevención de ejecución de datos aplicada por hardware. A continuación, compruebe el registro de eventos de hipervisor de Hyper-V.*  
  
Para comprobar el catálogo, vea [catálogo de Windows Server](https://go.microsoft.com/fwlink/?LinkId=111228) (https://go.microsoft.com/fwlink/?LinkId=111228).  
  
> [!CAUTION]  
> Cambiar ciertos parámetros en el BIOS del sistema de un equipo puede hacer que ese equipo para detener la carga del sistema operativo, o puede que los dispositivos de hardware, como unidades de disco duro no esté disponible. Consulte siempre el manual del usuario para el equipo determinar la manera adecuada para configurar el BIOS del sistema. Además, siempre es una buena idea realizar un seguimiento de los parámetros que modifican y original de valor para que pueda restaurarlos posteriormente si es necesario. Si experimenta problemas después de cambiar los parámetros en el BIOS del sistema, vuelva a intentar cargar la configuración predeterminada (una opción es suelen estar disponible en la utilidad de configuración del BIOS) o póngase en contacto con el fabricante del equipo para obtener ayuda.  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>Para comprobar la compatibilidad de virtualización en el BIOS o UEFI  
  
1.  Reinicie el equipo y tener acceso a la BIOS o UEFI a través de la herramienta de configuración. Acceso a esta herramienta está disponible normalmente cuando el equipo entra a través de un proceso de arranque. Inmediatamente después de activar la mayoría de los equipos, aparece un mensaje durante unos segundos que se enumeran la tecla o combinación de teclas para abrir la herramienta de configuración.  
  
2.  Busque la configuración de la virtualización y prevención de ejecución de datos (DEP) aplicada por hardware y comprobar que se encuentran en. Estos son ubicaciones comunes en el menú sobre estas opciones en la herramienta de configuración y ejemplos de lo podrían llamarse:  
  
    -   Compatibilidad con la virtualización:  
  
        -   Normalmente está disponible en la configuración para el procesador principal o el rendimiento. A veces resulta en la configuración de seguridad.  
  
        -   Busque los nombres de parámetro que incluyan "virtualización" o "tecnología de virtualización".  
  
    -   DEP forzada en hardware:  
  
        -   Normalmente está disponible en la configuración de seguridad o la memoria.  
  
        -   Busque los nombres de parámetro que incluyan "ejecución", "ejecutar" o "prevención".  
  
3.  Si es necesario, active la configuración siguiendo las instrucciones de la herramienta de configuración. Guarde los cambios y salir.  
  
4.  Si ha realizado algún cambio, apague el sistema y, a continuación, hacer una copia en Finalizar.  
  
    > [!IMPORTANT]  
    > Se recomienda que apague el sistema y vuelva a (a veces denominado un ciclo de energía) porque no se aplican los cambios en algunos equipos hasta que esto suceda.  
  
A continuación, compruebe el registro de eventos de hipervisor de Hyper-V. Si hay problemas, también podrá comprobar el registro del sistema.  
  
#### <a name="to-check-the-event-logs"></a>Para comprobar los registros de eventos  
  
1.  Abra el Visor de eventos. Haga clic en **iniciar**, haga clic en **herramientas administrativas**y, a continuación, haga clic en **Visor de eventos**.  
  
2.  Abra el registro de eventos de hipervisor de Hyper-V. En el panel de navegación, expanda **registros de aplicaciones y servicios** >> **Microsoft** >> **Windows**  >>   **Hipervisor de Hyper-V**y, a continuación, haga clic en **Operational**.  
  
3.  Si se está ejecutando el hipervisor de Windows, no es necesario realizar ninguna otra acción. Si no se está ejecutando el hipervisor de Windows, hacer esto:  
  
4.  Abra el registro del sistema. (En el panel de navegación, expanda **Windows registros** y, a continuación, seleccione **System**.)  
  
5.  Utilice un filtro para buscar eventos de hipervisor de Hyper-V:   
    1. En el **acciones** panel, haga clic en **filtrar registro actual**. Para **orígenes de eventos**, especifique "Hipervisor Hyper-V-".   
    2. Busque eventos que informan de problemas. Por ejemplo, 41 de Id. de evento indica un problema con la configuración del BIOS: "Error de inicio de Hyper-V; Cualquier VMX no está presente o no está habilitado en el BIOS."  
  
### <a name="see-also"></a>Vea también  
Para obtener más información sobre el uso de Hyper-V en Windows 10, incluida la forma de comprobar que su equipo puede ejecutar Hyper-V, consulte [requisitos de sistema de Windows 10 Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility). 


