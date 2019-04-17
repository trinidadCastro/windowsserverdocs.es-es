---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: "Auditoría de proceso de línea de comandos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 61e7a5ca2b9c00c9976e6032bb10adb1974020b0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="command-line-process-auditing"></a>Auditoría de proceso de línea de comandos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

**Autor**: Justin Turner, Senior ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.  
  
## <a name="overview"></a>Introducción  
  
-   El evento de auditoría de creación identificador 4688 proceso preexistente ahora incluye información de auditoría para los procesos de línea de comandos.  
  
-   También registrará hash SHA1/2 del archivo ejecutable en el registro de eventos de Applocker  
  
    -   Aplicaciones y servicios\microsoft\windows\applocker  
  
-   Habilita a través de GPO, pero está deshabilitado de manera predeterminada  
  
    -   "Incluir la línea de comandos en procesar eventos de creación"  
  
![Auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura SEQ ilustración \\\ * evento 16 árabe 4688**  
  
Revisa el identificador 4688 del evento actualizada en REF _Ref366427278 \h figura 16.  Antes de esto, actualiza ninguno de los datos para **línea de comandos de proceso**, se registra.  Debido a este registro adicionales ahora podemos ver que no solo se inició el proceso de wscript.exe, sino que también se usó para ejecutar un script VB.  
  
## <a name="configuration"></a>Configuración  
Para ver los efectos de esta actualización, tendrás que habilitar las dos configuraciones de directiva.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Debes tener habilitado para ver el identificador 4688 del evento de auditoría de auditar creación de procesos.  
Para habilitar la directiva de auditoría proceso creación, edita la directiva de grupo siguiente:  
  
**Ubicación de las directivas:** configuración del equipo > directivas > configuración de Windows > configuración de seguridad > configuración de auditoría avanzada > seguimiento detallado  
  
**Nombre de la directiva:** auditar creación de procesos  
  
**Compatible con:** Windows 7 y superior  
  
**Descripción/ayuda:**  
  
Esta configuración de directiva de seguridad determina si el sistema operativo genera eventos de auditoría cuando se crea un proceso (se inicia) y el nombre del programa o usuario que la creó.  
  
Estos eventos de auditoría puede ayudarte a comprender cómo se utiliza un equipo y realizar un seguimiento de actividad del usuario.  
  
Volumen de eventos: bajo a medio, dependiendo del uso del sistema  
  
**Valor predeterminado:** no configurado  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver las adiciones a identificador 4688 del evento, debes habilitar la nueva configuración de directiva: incluir la línea de comandos en procesar eventos de creación  
**Tabla de tabla SEQ \\\ * configuración de directiva de proceso de línea de comandos de 19 árabe**  
  
|Configuración de directiva|Detalles|  
|------------------------|-----------|  
|**Ruta de acceso**|Creación de procesos Templates\System\Audit administrativas|  
|**Configuración**|**Incluir la línea de comandos en procesar eventos de creación**|  
|**Configuración predeterminada**|No configurado (no habilitado)|  
|**Compatible con:**|?|  
|**Descripción**|Esta configuración de directiva determina qué información se registra en los eventos de auditoría de seguridad cuando se ha creado un nuevo proceso.<br /><br />Esta opción solo se aplica cuando se habilita la directiva de auditar creación de procesos. Si habilita a esta configuración de directiva que se registrará la información de la línea de comandos para cada proceso en texto sin formato en el registro de eventos de seguridad como parte del evento 4688 auditar creación de procesos, "un nuevo proceso se ha creado," en los servidores y estaciones de trabajo en el que se aplica esta configuración de directiva.<br /><br />Si deshabilita o no configura esta directiva, la información del proceso línea de comandos no se incluirá en los eventos de auditar creación de procesos.<br /><br />Valor predeterminado: No configurado<br /><br />Nota: Cuando se habilita esta configuración de directiva, crean todos los usuarios con acceso para leer que los eventos de seguridad podrán leer correctamente de los argumentos de línea de comandos para cualquier proceso. Los argumentos de línea de comandos pueden contener información confidencial o privada, como contraseñas o datos de usuario.|  
  
![Auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Cuando usas las opciones de configuración de directiva de auditoría avanzada, debes confirmar que estas opciones no quedan sobrescritas por la configuración de directiva de auditoría básica.  Evento 4719 se registra cuando se sobrescribe la configuración.  
  
![Auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
El siguiente procedimiento muestra cómo evitar conflictos mediante el bloqueo de la aplicación de las opciones de configuración de directiva de auditoría básica.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Para garantizar que no se sobrescriban las opciones de configuración de directiva de auditoría avanzada  
![Auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Abre la consola de administración de directivas de grupo  
  
2.  Haz clic en la directiva de dominio predeterminada y, a continuación, haz clic en Editar.  
  
3.  Haz doble clic en configuración del equipo, haz doble clic en las directivas y, a continuación, haz doble clic en configuración de Windows.  
  
4.  Haz doble clic en la configuración de seguridad, haz doble clic en directivas locales y, a continuación, haz clic en Opciones de seguridad.  
  
5.  Haz doble clic en la auditoría: Forzar directiva subcategoría la configuración auditoría (Windows Vista o posterior) para invalidar la configuración de categoría de directiva de auditoría y, a continuación, haga clic en definir esta configuración de directiva.  
  
6.  Haga clic en habilitada y, a continuación, haz clic en Aceptar.  
  
## <a name="additional-resources"></a>Recursos adicionales  
[Auditar creación de procesos](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[Avanzada de directiva de auditoría de seguridad Step-by-Step guía](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker: Preguntas más frecuentes](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Lo prueba siguiente: Explorar el proceso de auditoría de línea de comandos  
  
1.  Habilitar **auditar creación de procesos** eventos y asegurarse de que no se sobrescribe la configuración de directiva de auditoría avanzada  
  
2.  Crear un script que generará algunos eventos de interés y ejecutar el script.  Observar los eventos.  El script que se usa para generar el evento en la lección tendría este aspecto:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Habilitar la auditoría de proceso de línea de comandos  
  
4.  La misma secuencia de comandos antes de ejecutar y observar los eventos  
  


