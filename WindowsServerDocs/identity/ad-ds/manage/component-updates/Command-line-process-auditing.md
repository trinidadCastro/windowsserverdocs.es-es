---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Auditoría de proceso de línea de comandos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66ae6992775319cf614b0cb4c21f864150746687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859556"
---
# <a name="command-line-process-auditing"></a>Auditoría de proceso de línea de comandos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

**Autor**: Justin Turner, jefe ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
  
-   El evento de auditoría de creación 4688 Id. de proceso preexistente ahora incluirá información de auditoría para los procesos de línea de comandos.  
  
-   También registrará hash SHA1/2 del archivo ejecutable en el registro de eventos de Applocker  
  
    -   Aplicaciones y servicios\microsoft\windows\applocker  
  
-   Habilita a través de GPO, pero está deshabilitado de forma predeterminada  
  
    -   "Incluyen la línea de comandos de procesamiento de eventos de creación"  
  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura SEQ Figure \\ \* evento 16 árabe 4688**  
  
Revise el evento 4688 de Id. de actualizado en REF _Ref366427278 \h figura 16.  Antes de esto, actualice ninguno de los datos para **línea de comandos de proceso** se registra.  Debido a este inicio de sesión adicional ahora podemos ver que no sólo se inició el proceso wscript.exe, sino que también se usó para ejecutar un script VB.  
  
## <a name="configuration"></a>Configuración  
Para ver los efectos de esta actualización, deberá habilitar a dos configuraciones de directiva.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Debe tener auditar creación de procesos está habilitada para ver el evento 4688 de Id. de la auditoría.  
Para habilitar la directiva de creación del proceso de auditoría, edite la directiva de grupo siguientes:  
  
**Ubicación de la directiva:** Configuración del equipo > directivas > configuración de Windows > configuración de seguridad > configuración de auditoría avanzada > detallados de seguimiento  
  
**Nombre de la directiva:** Auditar creación de procesos  
  
**Compatible con:** Windows 7 y versiones posteriores  
  
**Descripción/Help:**  
  
Esta configuración de directiva de seguridad determina si el sistema operativo genera eventos de auditoría cuando se crea un proceso (se inicia) y el nombre del programa o el usuario que lo creó.  
  
Estos eventos de auditoría puede ayudarle a entender cómo se usa un equipo y realizar un seguimiento de actividad del usuario.  
  
Volumen de eventos: Bajo a medio, en función del uso del sistema  
  
**Valor predeterminado:** No configurado  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver las adiciones al evento 4688 de identificador, debe habilitar a la nueva configuración de directiva: Incluir la línea de comandos en los eventos de creación de procesos  
**Tabla SEQ Table \\ \* configuración de directiva de proceso de línea de comandos árabe de 19**  
  
|Configuración de directiva|Detalles|  
|------------------------|-----------|  
|**Path**|Creación del proceso Templates\System\Audit administrativas|  
|**Configuración de**|**Incluir la línea de comandos en los eventos de creación de procesos**|  
|**Configuración predeterminada**|No configurado (no habilitado)|  
|**Compatible con:**|?|  
|**Descripción**|Esta configuración de directiva determina qué información se registra en los eventos de auditoría de seguridad cuando se ha creado un nuevo proceso.<br /><br />Esta configuración solo se aplica cuando se habilita la directiva de creación del proceso de auditoría. Si habilita esta configuración de la información de línea de comandos para todos los procesos se registrarán en texto sin formato en el registro de eventos de seguridad como parte del evento 4688 de creación del proceso de auditoría de directiva, "un nuevo proceso se ha creado," en las estaciones de trabajo y servidores en el que este directiva se aplica la configuración.<br /><br />Si deshabilita o no configura esta directiva, no se incluirá información de línea de comandos del proceso en los eventos de creación del proceso de auditoría.<br /><br />Default: No configurado<br /><br />Nota: Cuando se habilita esta configuración de directiva, cualquier usuario con acceso para leer que los eventos de seguridad será capaz de leer correctamente los argumentos de línea de comandos para cualquier creó el proceso. Argumentos de línea de comandos pueden contener información confidencial o privada como contraseñas o datos de usuario.|  
  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Si usa la Configuración de directiva de auditoría avanzada, deberá confirmar que la configuración de directiva de auditoría básica no la sobrescribe.  Evento 4719 se registra cuando se sobrescribe la configuración.  
  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
En el procedimiento siguiente se muestra cómo evitar conflictos bloqueando la aplicación de cualquier configuración de directiva de auditoría básica.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Para asegurarse de que no se sobrescribe la Configuración de directiva de auditoría avanzada  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Abra la consola de administración de directivas de grupo  
  
2.  Haga clic en directiva predeterminada de dominio y, a continuación, haga clic en Editar.  
  
3.  Haga doble clic en configuración del equipo, haga doble clic en directivas y, a continuación, haga doble clic en configuración de Windows.  
  
4.  Haga doble clic en configuración de seguridad, haga doble clic en directivas locales y, a continuación, haga clic en Opciones de seguridad.  
  
5.  Haga doble clic en la auditoría: Forzar la configuración de subcategorías de directiva de auditoría (Windows Vista o posterior) para invalidar la configuración de categoría de directiva de auditoría y, a continuación, haga clic en definir esta configuración de directiva.  
  
6.  Haga clic en habilitado y, a continuación, haga clic en Aceptar.  
  
## <a name="additional-resources"></a>Recursos adicionales  
[Creación del proceso de auditoría](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Guía paso a paso de directiva de auditoría de seguridad avanzada](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: Preguntas más frecuentes](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Pruebe lo siguiente: Explore la auditoría de procesos de línea de comandos  
  
1.  Habilitar **creación del proceso de auditoría** eventos y asegúrese de que no se sobrescribe la configuración de directiva de auditoría avanzada  
  
2.  Crear un script que se generan algunos eventos de interés y se ejecutará el script.  Observe los eventos.  El script utilizado para generar el evento en la lección tendría este aspecto:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Habilitar la auditoría de procesos de línea de comandos  
  
4.  Ejecutar la misma secuencia de comandos como antes y observar los eventos  
  


