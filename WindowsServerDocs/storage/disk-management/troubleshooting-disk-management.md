---
title: Solución de problemas de Administración de discos
description: En este artículo se describe cómo solucionar problemas de Administración de discos
ms.date: 12/22/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c234828706d999fe049626a2fd98db70e612766f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192741"
---
# <a name="troubleshooting-disk-management"></a>Solución de problemas de Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se tratan algunos problemas comunes que puedes encontrar al usar la Administración de discos.

> [!TIP]
> Si se produce un error o que algo no funciona al seguir estos procedimientos: no se alarme! Hay una gran cantidad de información en el [comunidad Microsoft](https://answers.microsoft.com/en-us/windows) sitio: intente buscar el [archivos, carpetas y almacenamiento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) sección y, si aún necesita ayuda, publique una pregunta en ellos y Microsoft u otros miembros de la Comunidad intentará ayuda. Si tiene comentarios sobre cómo mejorar estos temas, nos encantaría conocer su opinión. Simplemente responda el *es útil esta página?* símbolo del sistema y dejar comentarios allí o en el subproceso de comentarios públicos en la parte inferior de este tema.

## <a name="a-disks-status-is-not-initialized-or-the-disk-is-missing"></a>Estado de un disco no inicializado o el disco falta

![Administración de discos que se muestra un disco desconocido que se debe inicializar.](media\uninitialized-disk.PNG)

**Causa:** Si tiene un disco que no aparece en el Explorador de archivos y aparece en la administración de discos como *no inicializado*, es posible que el disco no tiene una firma de disco válida. Básicamente, esto significa que nunca se ha inicializado y con el formato del disco, o el formato de la unidad se ha dañado algún modo. 

También es posible que el disco está teniendo problemas de hardware o problemas al conectar, pero llegaremos a en algunos párrafos.

**Solución:**   si la unidad es completamente nueva y solo debe inicializarse, borrar todos los datos, la solución es fácil: vea [inicializar nuevos discos](initialize-new-disks.md). Sin embargo, hay muchas posibilidades de que ya ha intentado esto y funcionó. O quizá tenga un disco lleno de archivos importantes, y no desea borrar el disco por su inicialización.

Hay una serie de motivos, puede que falte o que no se pudo inicializar una a la razón común porque es un error en el disco un disco. Hay sólo lo mucho que pueda hacer para corregir un error de disco, pero estos son algunos pasos para probar si podemos obtener que funcione de nuevo. Si el disco funciona después de que uno de estos pasos, no preocuparse por los pasos siguientes, simplemente relajarse, celebrar y es posible actualizar las copias de seguridad.

1. Examine el disco en administración de discos. Si aparece *Offline* como se muestra aquí, try, haga clic en él y seleccionando **Online**.

    ![Disco aparece como sin conexión](media/offline-disk.png)
1. Si el disco aparece en administración de discos como *Online*, y tiene una partición primaria que se muestra como *correcto*, como se muestra aquí, que es una buena señal.

    ![Se muestra como en línea con un volumen en buen estado de disco](media/healthy-volume.png)
    - Si la partición tiene un sistema de archivos, pero sin letra de unidad (por ejemplo, E:), consulte [cambiar una letra de unidad](change-a-drive-letter.md) para agregar manualmente una letra de unidad.
    - Si no tiene un sistema de archivos (NTFS, ReFS, FAT32 o exFAT) y sabe que el disco está vacío, haga clic en la partición y seleccione **formato**. Al formatear un disco, borra todos los datos en él, por lo que no lo haga si está intentando recuperar archivos desde el disco: en su lugar, vaya al paso siguiente.
1. Si tiene discos, desconecte el disco externo, conéctelo en y, a continuación, seleccione **acción** > **volver a examinar los discos**. 
2. Apague su PC, desactivar el disco duro externo (si es un disco externo con un cable de alimentación) y, a continuación, vuelva a activar su PC y el disco.
    Para desactivar su PC de Windows 10, seleccione el botón Inicio, seleccione el botón de encendido y, a continuación, seleccione **apagar**.
1. Conecte el disco en un puerto USB diferente que se encuentra directamente en su PC (no en un concentrador).
    A veces, los discos USB no obtener suficiente capacidad de algunos puertos, o tiene otros problemas con determinados puertos. Esto es especialmente habitual con concentradores USB, pero a veces hay diferencias entre los puertos en un equipo, así que pruebe algunos puertos diferentes si las tiene.
1. Pruebe un cable diferente.
    Podría parecer una locura, pero producirá un error en un lote de cables, por lo que intente usar un cable diferente para conectar el disco. Si tiene un disco interno en un equipo de escritorio, probablemente necesitará apagar su PC antes de cambiar los cables, consulte el manual de su PC para obtener más información.
1. Compruebe el Administrador de dispositivos para los problemas.
    Presione y mantenga el botón Inicio, a continuación, seleccione el Administrador de dispositivos en el menú contextual (o secundario). Busque dispositivos con un signo de exclamación junto a él u otros problemas, haga doble clic en el dispositivo y, a continuación, leer su estado.

    Esta es una lista de [códigos de Error en el Administrador de dispositivos](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), pero un enfoque que a veces funciona es que hace clic en el dispositivo problemático, seleccione **desinstalar dispositivo**y, a continuación, **acción**  >  **Buscar cambios de hardware**.
    ![Administrador de dispositivos con un dispositivo USB desconocido](media\device-manager.PNG)
1. Conecte el disco en un equipo diferente.
    
    Si el disco no funciona en otro equipo, es una buena señal de que hay algo vaya mal sucede en el disco y no a su PC. Nada divertido, lo sabemos. Hay algunos pasos más que puede probar en [USB externo unidad error "Debe inicializar el disco antes de que el Administrador de discos lógicos pueden tener acceso a"](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware), pero podría ser el momento para buscar y solicitar ayuda en el [decomunidaddeMicrosoft](https://answers.microsoft.com/en-us/windows) de sitio, o póngase en contacto con el fabricante del disco.

    Si simplemente no se puede obtener su funcionamiento, también hay aplicaciones que pueden intentar recuperar datos desde un disco con errores, o si los archivos están muy importantes, puede pagar un laboratorio de recuperación de datos para intentar recuperarse de ellos. Si encuentra algo que funcione para usted, indíquelo en la siguiente sección de comentarios.

> [!IMPORTANT]
> Errores en los discos con bastante frecuencia, por lo que es importante realizar copias de seguridad con regularidad todos los archivos que le interesan. Si tiene un disco que no aparece en ocasiones o genera errores, tenga en cuenta esto un aviso para que vuelva a comprobar los métodos de copia de seguridad. Es correcto si está ligeramente por detrás de: hemos todos pasado por ahí. La mejor solución de copia de seguridad es que utiliza, por lo que le recomendamos que busque uno que funciona para usted y sígala.

> [!TIP]
Para obtener información sobre cómo usar aplicaciones integradas en Windows a los archivos de copia de seguridad en una unidad externa como una unidad USB, consulte [copia de seguridad y restaurar los archivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). También puede guardar archivos en OneDrive de Microsoft, que sincroniza los archivos de su PC en la nube. Si se produce un error en el disco duro, podrá obtener los archivos que se almacenan en OneDrive desde OneDrive.com. Para obtener más información, consulte [OneDrive en su PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>El estado de un disco básico o dinámico es ilegible

**Causa:**  el disco básico o dinámico no es accesible y es posible que haya experimentado un error de hardware, daños o errores de E/S. La copia del disco de la base de datos de configuración del disco del sistema podría estar dañada. Aparece un icono de error en los discos que muestran el estado **Ilegible**.

Los discos también podrían mostrar el estado **Ilegible** mientras giran o cuando Administración de discos vuelve a escanear todos los discos en el sistema. En algunos casos, se produce un error en un disco ilegible y no puede recuperarse. En el caso de los discos dinámicos, el estado **Ilegible** suele producir errores de E/S o daños en el disco, en lugar de producir daños en todo el disco.

**Solución:**  volver a examinar los discos o reinicie el equipo para ver si cambia el estado del disco. Pruebe también los pasos de solución de problemas descritos en [estado de un disco no inicializado o el disco falta completamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-dynamic-disks-status-is-foreign"></a>El estado de un disco dinámico es externo

**Causa:**  el **externa** estado se produce cuando se mueve un disco dinámico en el equipo local desde otro equipo PC. Aparece un icono de advertencia en los discos que muestran el estado **Externo**.

En algunos casos, un disco que se ha conectado anteriormente al sistema puede mostrar el estado **Externo**. Los datos de configuración de los discos dinámicos se almacenan en todos los discos dinámicos, por lo que la información sobre los discos que pertenecen al sistema se pierden cuando se produce un error en todos los discos dinámicos.

**Solución:**  agregar el disco a la configuración del sistema, por lo que puede tener acceso a datos en el disco. Para agregar un disco a la configuración del sistema de tu equipo, importa el disco externo (haz clic con el botón derecho en el disco y luego haz clic en **Importar discos externos**). Cualquier volumen existente en el disco externo queda visible y puedes tener acceso a él al importar el disco. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>Estado de un disco dinámico es en línea (errores)

**Causa:**  el disco dinámico tiene errores de E/S en una región del disco. Aparece un icono de advertencia en el disco dinámico con errores.

**Solución:**   si los errores de E/S son temporales, reactive el disco para llevarlo a **Online** estado.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>Estado de un disco dinámico es sin conexión o falta

**Causa:**  una **Offline** disco dinámico puede estar dañado o no está disponible temporalmente. Aparece un icono de error en el disco dinámico desconectado.

Si el estado del disco es **Desconectado** y el nombre del disco cambia a **Falta**, el disco estaba recientemente disponible en el sistema, pero ya no puede encontrarse ni identificarse. El disco que falta puede estar dañado, apagado o desconectado.

**Solución:** Conectar un disco que está sin conexión y que faltan en línea:

1. Repara cualquier problema de cable, controlador o disco. 
2. Asegúrate de que el disco físico está activado y conectado al equipo. 
3. Luego usa el comando **Reactivar disco** para conectar el disco.
4. Pruebe los pasos de solución de problemas descritos en [estado de un disco no inicializado o el disco falta completamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).
5. Si el estado del disco permanece **Desconectado** y el nombre del disco permanece en el estado **Falta** y determinas que el disco tiene un problema que no puede repararse, puedes quitar el disco del sistema haciendo clic con el botón derecho en el disco y luego haciendo clic en **Quitar disco**). Sin embargo, antes de quitar el disco, debes eliminar todos los volúmenes (o reflejos) en el disco. Puedes guardar los volúmenes reflejados en el disco quitando el reflejo en lugar de todo el volumen. Eliminar un volumen destruye los datos en el volumen, por lo que debes quitar un disco solo si estás completamente seguro de que el disco está dañado y no puede usarse de forma permanente.

**Para conectar un disco que está sin conexión y todavía se denomina disco \# (no falta) en línea, pruebe uno o varios de los siguientes procedimientos:**

1. En Administración de discos, haz clic con el botón derecho en el disco y luego haz clic en **Reactivar disco** para conectar el disco. Si el estado del disco permanece **Desconectado**, comprueba los cables y el controlador del disco y asegúrate de que el disco físico sea el correcto. Corrige los problemas e intenta activar de nuevo el disco. Si la reactivación del disco se realiza correctamente, los volúmenes en el disco deben volver automáticamente al estado **Correcto**.
2. En Visor de eventos, comprueba los registros de eventos de los errores relacionados con el disco, como por ejemplo, "Sin copias de configuración buenas". Si los registros de eventos incluyen este error, ponte en contacto con los [Servicios de soporte técnico de Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Intenta mover el disco a otro equipo. Si puedes hacer que el disco cambie al estado **Conectado** en otro equipo, es muy probable que el problema se deba a la configuración del equipo en la que el disco no cambia al estado **Conectado**.

4. Intenta mover el disco a otro equipo que tenga discos dinámicos. Importa el disco en el equipo y luego mueve el disco al equipo en el que no cambiaría al estado **Conectado**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>El estado de un volumen básico o dinámico es Error

**Causa:**   no se puede iniciar automáticamente el volumen básico o dinámico, el disco está dañado o el sistema de archivos está dañado. A menos que se pueda reparar el sistema de archivos o el disco, el estado **Error** indica una pérdida de datos.

**Solución:**

Si el volumen es un volumen básico con **Failed** estado:

- Asegúrate de que el disco físico subyacente está activado y conectado al equipo.
- Pruebe los pasos de solución de problemas descritos en [estado de un disco no inicializado o el disco falta completamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

Si se trata de un volumen dinámico con el estado **Error**:

-   Asegúrate de que los discos subyacentes están conectados. Si no es así, devuelve los discos al estado **Conectado**. Si se realiza correctamente, el volumen se reinicia automáticamente y vuelve al estado **Correcto**. Si el disco dinámico vuelve al estado **Conectado**, pero el volumen dinámico no vuelve al estado **Correcto**, puedes volver a activar el volumen manualmente.
-   Si el volumen dinámico es un volumen reflejado o RAID-5 con datos antiguos, conectar el disco subyacente no reiniciará automáticamente el volumen. Si se desconectan los discos que incluyen los datos actuales, conéctalos en primer lugar (para permitir que los datos se sincronicen). De lo contrario, reinicia el volumen reflejado o RAID-5 manualmente, y luego ejecuta la herramienta de comprobación de errores o Chkdsk.exe.
- Pruebe los pasos de solución de problemas descritos en [estado de un disco no inicializado o el disco falta completamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>Estado de un volumen básico o dinámico es desconocido

**Causa:**   el **desconocido** estado se produce cuando el sector de arranque para el volumen está dañado (posiblemente debido a un virus) y ya no puede tener acceso a datos en el volumen. El estado **Desconocido** también aparece cuando instalas un nuevo disco pero no finalizas correctamente el asistente para crear una firma de disco.

**Solución**  inicializar el disco. Para obtener instrucciones, consulta [Inicializar nuevos discos](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>Estado de un volumen dinámico es datos incompletos

**Causa:** Se movió parte, pero no todos los discos en un volumen de disco de múltiples. Los datos de este volumen se destruirán a menos que muevas e importes los discos restantes que incluyen este volumen.

**Solución:**

1. Mueve todos los discos que componen el volumen de varios discos al equipo.
2. Importa los discos. Para obtener instrucciones sobre cómo mover e importar discos, consulta [Mover discos a otro equipo](move-disks-to-another-computer.md).

Si ya no necesitas el volumen de varios discos, puedes importar el disco y crear nuevos volúmenes en él. Para ello:

1. Haz clic con el botón derecho en el volumen con el estado **Error** o **Error de redundancia** y luego haz clic en **Eliminar volumen**.
2. Haz clic con el botón derecho en el disco y luego haz clic en **Nuevo volumen**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>Estado de un volumen dinámico es correcto (en riesgo)

**Causa:**   indica que el volumen dinámico es accesible en este momento, pero se han detectado errores de E/S en el disco dinámico subyacente. Si se detecta un error de E/S en cualquier parte de un disco dinámico, todos los volúmenes en el disco mostrarán el estado **Correcto (en riesgo)** y un icono de advertencia aparece en el volumen.

Si el estado del volumen es **Correcto (en riesgo)** , un estado del disco subyacente suele estar **Conectado (errores)** .

**Solución:**  
1. Devuelve el disco subyacente al estado **Conectado**. Una vez que el disco vuelve al estado **Conectado**, el volumen debe volver el estado **Correcto**. Si el estado **Correcto (en riesgo)** persiste, es posible que se produzca un error en el disco. 

2. Realiza una copia de seguridad de los datos y sustituye el disco lo antes posible. 

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>No se puede administrar volúmenes seccionados mediante administración de discos o DiskPart

**Causa:**   algunos productos de administración de discos que no sean de Microsoft reemplazar el Administrador de discos lógicos (LDM) de Microsoft para la administración avanzada de discos, lo que puede deshabilitar LDM.

**Solución:**   si usa software de administración de discos que no son de Microsoft que deshabilitó LDM, debe ponerse en contacto con el proveedor en el software de administración de discos que no son de Microsoft para soporte técnico y asistencia para solucionar problemas con el disco configuración.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Administración de discos no puede iniciar el servicio de disco Virtual

**Causa:**   si un equipo remoto no es compatible con el servicio de disco Virtual (VDS) o si no se puede establecer una conexión al equipo remoto porque está bloqueado por Firewall de Windows, puede recibir este error.

**Solución:**

1. Si el equipo remoto admite VDS, puedes configurar el Firewall de Windows Defender para permitir conexiones VDS. Si el equipo remoto no admite VDS, puedes usar la Conexión a Escritorio remoto para conectarte a él y luego ejecutar Administración de discos directamente en el equipo remoto.
2. Para administrar discos en equipos remotos que admiten VDS, debes configurar el Firewall de Windows Defender en el equipo local (en el que se ejecuta Administración de discos) y el equipo remoto.
3. En el equipo local, configura el Firewall de Windows Defender para habilitar la excepción de Administración remota del volumen.


> [!NOTE]
> La excepción de Administración remota del volumen incluye excepciones para Vds.exe, Vdsldr.exe y el puerto TCP 135.


 > [!NOTE]
 > No se admiten las conexiones remotas en grupos de trabajo. Tanto el equipo local como el equipo remoto deben ser miembros de un dominio.