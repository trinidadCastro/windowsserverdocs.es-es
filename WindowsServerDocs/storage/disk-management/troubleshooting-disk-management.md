---
title: Solución de problemas de Administración de discos
description: En este artículo se describe cómo solucionar problemas de Administración de discos
ms.date: 12/20/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7eeb462d31391a228ec0e89afb09673ef14b51cf
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371253"
---
# <a name="troubleshooting-disk-management"></a>Solución de problemas de Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema se tratan algunos problemas comunes que puedes encontrar al usar Administración de discos y resolver problemas de los pasos de prueba.

> [!TIP]
> Si se produce un error o algo no funciona al seguir estos procedimientos, no te alarmes. Este tema es el primer recurso al que debes acudir; también existe una gran cantidad de información en la sección [Archivos, carpetas y almacenamiento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) del sitio de la [comunidad Microsoft](https://answers.microsoft.com/en-us/windows) acerca de la amplia variedad de configuraciones de hardware y software que puedes estar gestionando. Si necesitas más ayuda, publica una pregunta en este sitio, o bien [ponte en contacto con el soporte técnico de Microsoft](https://support.microsoft.com/contactus/) o con el fabricante del hardware.

## <a name="how-to-open-disk-management"></a>Cómo abrir Administración de discos

Antes de pasar a cuestiones más complejas, indicamos una manera sencilla de acceder a Administración de discos, por si todavía no has accedido:

1. Escribe **Administración de equipos** en el cuadro de búsqueda de la barra de tareas, selecciona y mantén pulsado (o haz clic con el botón derecho) **Administración de equipos** y, después, selecciona **Ejecutar como administrador** > **Sí**.
2. Cuando se abra Administración de equipos, ve a **Almacenamiento** > **Administración de discos**.

## <a name="disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps"></a>Discos que faltan o no se han inicializado, más pasos de solución de problemas generales

![Administración de discos en la que se muestra un disco desconocido que se debe inicializar.](media/uninitialized-disk.PNG)

**Causa:** Si tienes un disco que no aparece en el Explorador de archivos y se muestra en la Administración de discos como *No inicializado*, puede deberse a que el disco no tiene una firma de disco válida. Básicamente, esto significa que nunca se ha inicializado ni formateado, o que el formato de la unidad se ha dañado algún modo. 

También es posible que el disco tenga problemas de hardware o problemas al conectar, pero llegaremos a eso en algunos párrafos.

**Solución:**   si la unidad es completamente nueva y solo debe inicializarse, borrando los datos que contenga, la solución es fácil: consulta [Initialize New Disks](initialize-new-disks.md) (Inicializar nuevos discos). Sin embargo, hay muchas posibilidades de que ya hayas intentado esto y que no haya funcionado. O quizá tengas un disco lleno de archivos importantes y no quieres borrarlo mediante su inicialización.

Hay una serie de motivos por los que puede que falte o no se haya podido inicializar un disco o una tarjeta de memoria. Un motivo común es que el disco está fallando. Hay muchas cosas que se pueden hacer para corregir un error de disco, pero, a continuación, se indican algunos pasos para ver si conseguimos que funcione de nuevo. Si el disco funciona después de realizar uno de estos pasos, no te preocupes por los siguientes; simplemente relájate, celébralo y, a lo mejor, actualiza las copias de seguridad.

1. Examina el disco en la Administración de discos. Si aparece *Desconectado* como se muestra aquí, intenta hacer clic en él con el botón derecho y selecciona **Conectado**.

    ![El disco aparece como desconectado](media/offline-disk.png)
2. Si el disco aparece en la Administración de discos como *Conectado* y tiene una partición principal que aparece como *Correcto*, como se muestra aquí, es una buena señal.

    ![El disco se muestra como conectado con un volumen correcto](media/healthy-volume.png)
    - Si la partición tiene un sistema de archivos, pero ninguna letra de unidad (por ejemplo, E:), consulta [Cambio de una letra de unidad](change-a-drive-letter.md) para agregar una letra de unidad manualmente.
    - Si una partición no tiene un sistema de archivos (se muestra como RAW en lugar de NTFS, ReFS, FAT32 o exFAT) y sabes que el disco está vacío, selecciona y mantén pulsado (o haz clic con el botón derecho) la partición y selecciona **Formatear**. Al formatear un disco, se borran todos los datos que contiene, por lo que no lo hagas si estás intentando recuperar archivos del disco; en su lugar, ve al paso siguiente.
    - Si la partición aparece como *Sin asignar* y sabes que está vacía, selecciónala y mantenla pulsada (o haz clic en esta con el botón derecho) y, a continuación, selecciona **Nuevo volumen simple** y sigue las instrucciones para crear un volumen en el espacio libre. No realices este procedimiento si estás intentando recuperar archivos de esta partición; en su lugar, ve al paso siguiente.

    > [!NOTE]
    > Ignora las particiones que aparecen como **partición de sistema EFI** o **partición de recuperación**. Estas particiones están llenas de archivos muy importantes que el equipo necesita para funcionar correctamente. Es mejor dejarlos trabajar para que inicien el equipo y te ayuden en la recuperación de los problemas.
3. Si tienes un disco externo que no se muestra, desconéctalo y vuelve a conectarlo; a continuación, selecciona **Acción** > **Volver a examinar los discos**. 
4. Apaga tu PC, desactiva el disco duro externo (si es un disco externo con un cable de alimentación) y, a continuación, vuelve a activar tu PC y el disco.
    Para desactivar tu PC de Windows 10, selecciona el botón Inicio, después, el botón de encendido y, a continuación, selecciona **Apagar**.
5. Conecta el disco en otro puerto USB que se encuentre directamente en tu PC (no en un concentrador).
    A veces, los discos USB no obtienen suficiente potencia de algunos puertos o tienen otros problemas con determinados puertos. Esto es especialmente habitual con los concentradores USB, pero, a veces, hay diferencias entre los puertos de un equipo, por lo que puedes probar diferentes puertos, en caso de que los tenga.
6. Prueba otro cable.
    Podría parecer una locura, pero los cables fallan mucho, por lo que puedes intentar usar otro cable para conectar el disco. Si tienes un disco interno en un equipo de escritorio, probablemente necesitarás apagar tu PC antes de cambiar los cable; consulta el manual de tu PC para obtener más información.
7. Comprueba el Administrador de dispositivos para ver si hay problemas.
    Selecciona y mantén pulsado el botón Inicio (o haz clic en él con el botón derecho) y, a continuación, selecciona Administrador de dispositivos en el menú contextual. Busca dispositivos con un signo de exclamación al lado u otros problemas, haz doble clic en el dispositivo y, a continuación, lee su estado.

    Aquí encontrarás una lista de [códigos de error en el Administrador de dispositivos](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), pero un enfoque que a veces funciona consiste en seleccionar y mantener pulsado (o hacer clic con el botón derecho) el dispositivo problemático, seleccionar **Desinstalar dispositivo** y, a continuación, **Acción** > **Buscar cambios de hardware**.

    ![Administrador de dispositivos mostrando un dispositivo USB desconocido](media/device-manager.PNG)
8. Conecta el disco en otro equipo.
    
    Si el disco no funciona en otro equipo, es una buena señal de que hay algo que no funciona en el disco, y no en el equipo. Aunque sabemos que no es divertido. Hay algunos pasos más que puedes probar en [External USB drive error "You must initialize the disk before Logical Disk Manager can access it"](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware) (Error de unidad USB externa "Debes inicializar el disco para que el Administrador de discos lógicos pueda acceder a él"), pero podría ser el momento de buscar y solicitar ayuda en el sitio de la [comunidad Microsoft](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639), o bien de ponerte en contacto con el fabricante del disco o el [soporte técnico de Microsoft](https://support.microsoft.com/contactus/).

    Si no consigues que funcione, también hay aplicaciones que pueden intentar recuperar datos desde un disco con errores o, si los archivos están muy importantes, puedes pagar a un laboratorio de recuperación de datos para intentar recuperarlos. Si alguna de estas soluciones funciona, indícalo en la siguiente sección de comentarios.

> [!IMPORTANT]
> Los discos fallan con bastante frecuencia, por lo que es importante realizar copias de seguridad con regularidad de todos los archivos que te interesan. Si tienes un disco que en ocasiones no aparece o genera errores, considera esto como un recordatorio para que volver a comprobar tus métodos de copia de seguridad. No pasa nada si te has quedado atrás; todos lo hemos hecho. La mejor solución de copia de seguridad es una que uses, por lo que te recomendamos que busques una que te funcione y que prosigas con ella.
> 
> [!TIP]
> Para obtener información sobre cómo usar aplicaciones integradas en Windows para archivos de copia de seguridad en una unidad externa como una unidad USB, consulta [Copias de seguridad y restauración en Windows 10](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). También puedes guardar archivos en Microsoft OneDrive, que sincroniza los archivos de tu PC en la nube. Si se produce un error en el disco duro, podrás obtener los archivos que se almacenan en OneDrive desde OneDrive.com. Para obtener más información, consulta [OneDrive en tu PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>El estado de un disco básico o dinámico es Ilegible

**Causa:**  el disco básico o dinámico no es accesible y es posible que experimentara errores de E/S, daños o errores de hardware. La copia del disco de la base de datos de configuración del disco del sistema podría estar dañada. Aparece un icono de error en los discos que muestran el estado **Ilegible**.

Los discos también podrían mostrar el estado **Ilegible** mientras giran o cuando Administración de discos vuelve a escanear todos los discos en el sistema. En algunos casos, se produce un error en un disco ilegible y no puede recuperarse. En el caso de los discos dinámicos, el estado **Ilegible** suele producir errores de E/S o daños en parte del disco, en lugar de producir daños en todo el disco.

**Solución:**  vuelve a escanear los discos o reinicia el equipo para comprobar si el estado del disco cambia. Pruebe también los pasos de solución de problemas descritos en [El estado de un disco aparece como No inicializado o falta el disco completamente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="a-dynamic-disks-status-is-foreign"></a>El estado de un disco dinámico es Externo

**Causa:**  el estado **Externo** se produce cuando se mueve un disco dinámico al equipo local desde otro PC del equipo. Aparece un icono de advertencia en los discos que muestran el estado **Externo**.

En algunos casos, un disco que se ha conectado anteriormente al sistema puede mostrar el estado **Externo**. Los datos de configuración de los discos dinámicos se almacenan en todos los discos dinámicos, por lo que la información sobre los discos que pertenecen al sistema se pierde cuando se produce un error en todos los discos dinámicos.

**Solución:**  agrega el disco a la configuración del sistema de tu equipo para que puedas tener acceso a los datos del disco. Para agregar un disco a la configuración del sistema de tu equipo, importa el disco externo (selecciona y mantén pulsado el disco, o bien haz clic con el botón derecho en este, y luego haz clic en **Importar discos externos**). Cualquier volumen existente en el disco externo queda visible y puedes tener acceso a él al importar el disco. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>El estado de un disco dinámico es En línea (errores)

**Causa:**  el disco dinámico tiene errores de E/S en una región del disco. Aparece un icono de advertencia en el disco dinámico con errores.

**Solución:**   si los errores de E/S son temporales, vuelve a activar el disco para devolverlo al estado **Conectado**.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>El estado de un disco dinámico es Desconectado o Falta

**Causa:**  un disco dinámico **Desconectado** puede estar dañado o no disponible de forma intermitente. Aparece un icono de error en el disco dinámico desconectado.

Si el estado del disco es **Desconectado** y el nombre del disco cambia a **Falta**, el disco estaba recientemente disponible en el sistema, pero ya no puede encontrarse ni identificarse. El disco que falta puede estar dañado, apagado o desconectado.

**Solución:** para conectar un disco que tiene el estado Desconectado y Falta:

1. Repara cualquier problema de cable, controlador o disco. 
2. Asegúrate de que el disco físico está activado y conectado al equipo. 
3. Luego, usa el comando **Reactivar disco** para conectar el disco.
4. Prueba también los pasos de solución de problemas descritos en [El estado de un disco aparece como No inicializado o falta el disco completamente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).
5. Si el estado del disco sigue siendo **Desconectado** y el nombre del disco sigue siendo **Falta**, y determinas que el disco tiene un problema que no se puede reparar, puedes seleccionar y mantener pulsado el disco (o hacer clic con el botón derecho en este) y luego hacer clic en **Quitar disco** para quitar el disco del sistema. Sin embargo, antes de quitar el disco, debes eliminar todos los volúmenes (o reflejos) en el disco. Puedes guardar los volúmenes reflejados en el disco quitando el reflejo en lugar de todo el volumen. Al eliminar un volumen, se destruyen los datos de este, por lo que debes quitar un disco solo si estás completamente seguro de que el disco está dañado y no puede usarse de forma permanente.

**Para conectar un disco que está desconectado y sigue denominándose Disco \# (no falta), prueba uno o varios de los siguientes procedimientos:**

1. En Administración de discos, selecciona y mantén pulsado el disco (o haz clic con el botón derecho en este) y luego haz clic en **Reactivar disco** para conectar el disco. Si el estado del disco permanece **Desconectado**, comprueba los cables y la controladora de disco y asegúrate de que el disco físico sea el correcto. Corrige los problemas e intenta activar de nuevo el disco. Si la reactivación del disco se realiza correctamente, los volúmenes en el disco deben volver automáticamente al estado **Correcto**.
2. En Visor de eventos, comprueba los registros de eventos de los errores relacionados con el disco, como por ejemplo, "Sin copias de configuración buenas". Si los registros de eventos incluyen este error, ponte en contacto con los [Servicios de soporte técnico de productos de Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Intenta mover el disco a otro equipo. Si puedes hacer que el disco cambie al estado **Conectado** en otro equipo, es muy probable que el problema se deba a la configuración del equipo en la que el disco no cambia al estado **Conectado**.

4. Intenta mover el disco a otro equipo que tenga discos dinámicos. Importa el disco en el equipo y luego mueve el disco al equipo en el que no cambiaría al estado **Conectado**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>El estado de un volumen básico o dinámico es Error

**Causa:**   el volumen básico o dinámico no se puede iniciar automáticamente, el disco está dañado o el sistema de archivos está dañado. A menos que se pueda reparar el sistema de archivos o el disco, el estado **Error** indica una pérdida de datos.

**Solución:**

si se trata de un volumen básico con el estado **Error**:

- Asegúrate de que el disco físico subyacente está activado y conectado al equipo.
- Prueba también los pasos de solución de problemas descritos en [El estado de un disco aparece como No inicializado o falta el disco completamente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

Si se trata de un volumen dinámico con el estado **Error**:

-   Asegúrate de que los discos subyacentes están conectados. Si no es así, devuelve los discos al estado **Conectado**. Si se realiza correctamente, el volumen se reinicia automáticamente y vuelve al estado **Correcto**. Si el disco dinámico vuelve al estado **Conectado**, pero el volumen dinámico no vuelve al estado **Correcto**, puedes volver a activar el volumen manualmente.
-   Si el volumen dinámico es un volumen reflejado o RAID-5 con datos antiguos, conectar el disco subyacente no reiniciará automáticamente el volumen. Si se desconectan los discos que incluyen los datos actuales, conéctalos en primer lugar (para permitir que los datos se sincronicen). De lo contrario, reinicia el volumen reflejado o RAID-5 manualmente, y luego ejecuta la herramienta de comprobación de errores o Chkdsk.exe.
- Prueba también los pasos de solución de problemas descritos en [El estado de un disco aparece como No inicializado o falta el disco completamente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>El estado de un volumen básico o dinámico es Desconocido

**Causa:**   el estado **Desconocido** aparece cuando el sector de arranque del volumen está dañado (posiblemente a causa de un virus) y ya no puedes tener acceso a los datos en el volumen. El estado **Desconocido** también aparece cuando instalas un nuevo disco pero no finalizas correctamente el asistente para crear una firma de disco.

**Solución**  inicializa el disco. Para obtener instrucciones, consulta [Inicializar nuevos discos](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>El estado de un volumen dinámico es Datos incompletos

**Causa:** moviste algunos discos, pero no todos, de un volumen de varios discos. Los datos de este volumen se destruirán a menos que muevas e importes los discos restantes que incluyen este volumen.

**Solución:**

1. Mueve todos los discos que componen el volumen de varios discos al equipo.
2. Importa los discos. Para obtener instrucciones sobre cómo mover e importar discos, consulta [Mover discos a otro equipo](move-disks-to-another-computer.md).

Si ya no necesitas el volumen de varios discos, puedes importar el disco y crear nuevos volúmenes en él. Para ello:

1. Selecciona y mantén pulsado (o haz clic con el botón derecho) el volumen con el estado **Error** o **Error de redundancia** y luego haz clic en **Eliminar volumen**.
2. Selecciona y mantén pulsado (o haz clic con el botón derecho) el disco y, a continuación, haz clic en **Nuevo volumen**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>El estado de un volumen dinámico es Correcto (en riesgo)

**Causa:**   indica que el volumen dinámico es accesible en este momento, pero se han detectado errores de E/S en el disco dinámico subyacente. Si se detecta un error de E/S en cualquier parte de un disco dinámico, todos los volúmenes en el disco mostrarán el estado **Correcto (en riesgo)** y un icono de advertencia aparece en el volumen.

Si el estado del volumen es **Correcto (en riesgo)** , un estado del disco subyacente suele ser **Conectado (errores)** .

**Solución:**  
1. Devuelve el disco subyacente al estado **Conectado**. Una vez que el disco vuelve al estado **Conectado**, el volumen debe volver el estado **Correcto**. Si el estado **Correcto (en riesgo)** persiste, es posible que se produzca un error en el disco. 

2. Realiza una copia de seguridad de los datos y sustituye el disco lo antes posible.

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>No se pueden administrar volúmenes seccionados con Administración de discos o DiskPart

**Causa:**   algunos productos de administración de discos que no son de Microsoft reemplazan el Administrador de discos lógicos (LDM) de Microsoft para la administración avanzada de discos, lo que puede deshabilitarlo.

**Solución:**   si estás usando un software de administración de discos que no es de Microsoft que ha deshabilitado LDM, debes ponerte en contacto con el proveedor de dicho software para recibir soporte técnico o ayuda para solucionar problemas con la configuración del disco.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Administración de discos no puede iniciar el servicio de disco virtual

**Causa:**   si un equipo remoto no es compatible con el servicio de disco virtual (VDS), o si no se puede establecer una conexión al equipo remoto porque está bloqueado por Firewall de Windows, puede aparecer este error.

**Solución:**

1. Si el equipo remoto admite VDS, puedes configurar el Firewall de Windows Defender para permitir conexiones VDS. Si el equipo remoto no admite VDS, puedes usar la Conexión a Escritorio remoto para conectarte a él y luego ejecutar Administración de discos directamente en el equipo remoto.
2. Para administrar discos en equipos remotos que admiten VDS, debes configurar el Firewall de Windows Defender en el equipo local (en el que se ejecuta Administración de discos) y el equipo remoto.
3. En el equipo local, configura el Firewall de Windows Defender para habilitar la excepción de Administración remota del volumen.

> [!NOTE]
> La excepción de Administración remota del volumen incluye excepciones para Vds.exe, Vdsldr.exe y el puerto TCP 135.

> [!NOTE]
> No se admiten las conexiones remotas en grupos de trabajo. Tanto el equipo local como el equipo remoto deben ser miembros de un dominio.

Consulta también

- [Liberar espacio en la unidad en Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)