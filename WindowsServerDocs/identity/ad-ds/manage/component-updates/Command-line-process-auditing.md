---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: Auditoría de proceso de línea de comandos
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5ca29f1eef61bd11b2ceede4f335c029412e7331
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966407"
---
# <a name="command-line-process-auditing"></a>Auditoría de proceso de línea de comandos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

**Autor**: Diego Turner, Ingeniero de soporte técnico de nivel superior con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
  
-   El ID. de evento de auditoría de creación de procesos preexistente 4688 ahora incluirá información de auditoría para los procesos de línea de comandos.  
  
-   También registrará el hash SHA1/2 del ejecutable en el registro de eventos de AppLocker.  
  
    -   Aplicaciones y servicios Logs\Microsoft\Windows\AppLocker  
  
-   Habilita a través de GPO, pero está deshabilitado de forma predeterminada  
  
    -   "Incluir línea de comandos en eventos de creación de procesos"  
  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**Figura SEQ figura \\ \* árabe 16 evento 4688**  
  
Revise el ID. de evento 4688 actualizado en REF _Ref366427278 \h Figura 16.  Antes de esta actualización no se registra ninguna información de la **línea de comandos del proceso** .  Debido a este registro adicional, ahora podemos ver que no solo se ha iniciado el proceso de wscript.exe, sino que también se ha usado para ejecutar un script de VB.  
  
## <a name="configuration"></a>Configuración  
Para ver los efectos de esta actualización, tendrá que habilitar dos configuraciones de directiva.  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>Debe tener habilitada la auditoría de creación de procesos de auditoría para ver el ID. de evento 4688.  
Para habilitar la Directiva de creación de procesos de auditoría, edite la siguiente directiva de Grupo:  
  
**Ubicación de la Directiva:** Configuración del equipo directivas de > > configuración de Windows > configuración de seguridad > configuración avanzada de auditoría > seguimiento detallado  
  
**Nombre de la Directiva:** Auditar creación de procesos  
  
**Compatible con:** Windows 7 y versiones posteriores  
  
**Descripción/ayuda:**  
  
Esta configuración de directiva de seguridad determina si el sistema operativo genera eventos de auditoría cuando se crea (inicia) un proceso y el nombre del programa o usuario que lo creó.  
  
Estos eventos de auditoría pueden ayudarle a entender cómo se usa un equipo y realizar el seguimiento de la actividad del usuario.  
  
Volumen de eventos: bajo a medio, según del uso del sistema  
  
**Valor predeterminado:** No configurado  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>Para ver las adiciones al ID. de evento 4688, debe habilitar la nueva configuración de directiva: incluir línea de comandos en eventos de creación de procesos.  
**Tabla SEQ tabla de la configuración de la \\ \* Directiva de proceso de línea de comandos 19**  
  
|Configuración de Directiva|Detalles|  
|------------------------|-----------|  
|**Path**|Creación de procesos de Templates\System\Audit de administración|  
|**Configuración**|**Incluir línea de comandos en eventos de creación de procesos**|  
|**Configuración predeterminada**|No configurado (no habilitado)|  
|**Compatible con:**|?|  
|**Descripción**|Esta configuración de directiva determina la información que se registra en los eventos de auditoría de seguridad cuando se ha creado un nuevo proceso.<p>Esta configuración solo se aplica cuando está habilitada la Directiva de creación de procesos de auditoría. Si habilita esta Directiva, la información de la línea de comandos de cada proceso se registrará en texto sin formato en el registro de eventos de seguridad como parte del evento 4688 de creación de proceso de auditoría, "se ha creado un nuevo proceso", en las estaciones de trabajo y los servidores en los que se aplica esta configuración de directiva.<p>Si deshabilita o no establece esta configuración de Directiva, la información de línea de comandos del proceso no se incluirá en los eventos de creación de procesos de auditoría.<p>Valor predeterminado: no configurado<p>Nota: cuando esta configuración de directiva está habilitada, cualquier usuario con acceso para leer los eventos de seguridad podrá leer los argumentos de la línea de comandos para cualquier proceso creado correctamente. Los argumentos de la línea de comandos pueden contener información confidencial o privada como contraseñas o datos de usuario.|  
  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
Si usa la Configuración de directiva de auditoría avanzada, deberá confirmar que la configuración de directiva de auditoría básica no la sobrescribe.  El evento 4719 se registra cuando se sobrescribe la configuración.  
  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
En el procedimiento siguiente se muestra cómo evitar conflictos bloqueando la aplicación de cualquier configuración de directiva de auditoría básica.  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>Para asegurarse de que no se sobrescribe la Configuración de directiva de auditoría avanzada  
![auditoría de línea de comandos](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  Abra la consola de administración de directiva de grupo  
  
2.  Haga clic con el botón secundario en Directiva predeterminada de dominio y, a continuación, haga clic en Editar.  
  
3.  Haga doble clic en Configuración del equipo, haga doble clic en Directivasy, a continuación, haga doble clic en Configuración de Windows.  
  
4.  Haga doble clic en configuración de seguridad, haga doble clic en directivas locales y, a continuación, haga clic en opciones de seguridad.  
  
5.  Haga doble clic en Auditoría: forzar la configuración de subcategorías de la directiva de auditoría (Windows Vista o posterior) para invalidar la configuración de la categoría de directiva de auditoría y, a continuación, haga clic en Definir esta configuración de directiva.  
  
6.  Haga clic en Habilitaday, a continuación, haga clic en Aceptar.  
  
## <a name="additional-resources"></a>Recursos adicionales  
[Auditar creación de procesos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941613(v=ws.10))  
  
[Guía paso a paso de la directiva de auditoría de seguridad avanzada](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd408940(v=ws.10))  
  
[AppLocker: preguntas más frecuentes](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619725(v=ws.10))  
  
## <a name="try-this-explore-command-line-process-auditing"></a>Pruebe esto: explorar la auditoría de procesos de línea de comandos  
  
1.  Habilitar eventos de **creación de procesos de auditoría** y asegurarse de que no se sobrescribe la configuración de la Directiva de auditoría avanzada  
  
2.  Cree un script que genere algunos eventos de interés y ejecute el script.  Observe los eventos.  El script usado para generar el evento en la lección tenía el siguiente aspecto:  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  Habilitar la auditoría de procesos de línea de comandos  
  
4.  Ejecutar el mismo script que antes y observar los eventos  
  
