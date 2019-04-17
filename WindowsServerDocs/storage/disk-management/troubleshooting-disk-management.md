---
title: "Solución de problemas de Administración de discos"
description: "En este artículo se describe cómo solucionar problemas de Administración de discos"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e4b361631799dccbc2b77fb5aa909052532a057d
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="troubleshooting-disk-management"></a>Solución de problemas de Administración de discos

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se tratan algunos problemas comunes que puedes encontrar al usar la Administración de discos.

<a id="BKMK_1"></a>

## <a name="partitions-on-basic-disks-added-to-the-system-do-not-appear-in-the-disk-management-volume-list-view"></a>Las particiones en discos básicos agregadas al sistema no aparecen en la vista Lista de volúmenes de Administración de discos.

**Causa:** los volúmenes de los discos básicos agregados al sistema no se montan automáticamente ni asignan letras de unidad de forma predeterminada.
Los discos dinámicos aparecen como **Externos** cuando se agregaron al sistema. Para usar los volúmenes, debes importar los discos **Externos** y luego ponerlos en el estado **Conectado**.
El sistema siempre monta automáticamente dispositivos de medios extraíbles (tales como unidades Zip o Jaz) y discos ópticos (tales como CD-ROM o DVD-RAM).

**Solución:** monta manualmente los volúmenes básicos asignando letras de unidad o creando puntos de montaje con Administración de discos o los comandos [DiskPart](http://go.microsoft.com/fwlink/?LinkId=64110) o [mountvol](http://go.microsoft.com/fwlink/?LinkId=64111).

<a id="BKMK_2"></a>

## <a name="a-basic-disks-status-is-not-initialized"></a>El estado de un disco básico es Sin inicializar.

**Causa:** el disco no incluye una firma válida. Después de instalar un nuevo disco, el sistema operativo debe escribir una firma de disco, el final del marcador de sector (también denominada palabra de firma) y un registro de arranque maestro o tabla de particiones GUID antes de crear particiones en el disco. Cuando se inicia por primera vez la Administración de discos después de instalar un nuevo disco, aparece un asistente que proporciona una lista de discos nuevos que detecta el sistema operativo. Si cancelas el asistente antes de escribir la firma del disco, el estado del disco permanece **Sin inicializar**.

**Solución:** inicializa el disco. El estado del disco cambia brevemente a **Inicializando** y luego al estado **Conectado**. Para obtener instrucciones sobre cómo inicializar un disco, consulta [Inicializar nuevos discos](initialize-new-disks.md). 

<a id="BKMK_3"></a>

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>El estado de un disco básico o dinámico es Ilegible.

**Causa:** el disco básico o dinámico no es accesible y es posible que experimentara errores de E/S, daños o errores de hardware. La copia del disco de la base de datos de configuración del disco del sistema podría estar dañada. Aparece un icono de error en los discos que muestran el estado **Ilegible**.

Los discos también podrían mostrar el estado **Ilegible** mientras giran o cuando Administración de discos vuelve a escanear todos los discos en el sistema. En algunos casos, se produce un error en un disco ilegible y no puede recuperarse. En el caso de los discos dinámicos, el estado **Ilegible** suele producir errores de E/S o daños en el disco, en lugar de producir daños en todo el disco.

**Solución:** vuelve a escanear los discos o reinicia el equipo para comprobar si el estado del disco cambia.

<a id="BKMK_4"></a>

## <a name="a-dynamic-disks-status-is-foreign"></a>El estado de un disco dinámico es Externo.

**Causa:** el estado **Externo** aparece cuando mueves un disco dinámico al equipo local desde otro equipo que ejecuta los sistemas operativos Windows 2000, Windows XP Professional, Windows XP 64-Bit Edition o Windows Server 2003. Aparece un icono de advertencia en los discos que muestran el estado **Externo**.

En algunos casos, un disco que se ha conectado anteriormente al sistema puede mostrar el estado **Externo**. Los datos de configuración de los discos dinámicos se almacenan en todos los discos dinámicos, por lo que la información sobre los discos que pertenecen al sistema se pierden cuando se produce un error en todos los discos dinámicos.

**Solución:** agrega el disco a la configuración del sistema de tu equipo para que puedas tener acceso a los datos del disco. Para agregar un disco a la configuración del sistema de tu equipo, importa el disco externo (haz clic con el botón derecho en el disco y luego haz clic en **Importar discos externos**). Cualquier volumen existente en el disco externo queda visible y puedes tener acceso a él al importar el disco. 

<a id="BKMK_5"></a>

## <a name="a-dynamic-disks-status-is-online-errors"></a>El estado de un disco dinámico es Conectado (errores).

**Causa:** el disco dinámico tiene errores de E/S en una región del disco. Aparece un icono de advertencia en el disco dinámico con errores.

**Solución:** si los errores de E/S son temporales, vuelve a activar el disco para devolverlo al estado **Conectado**.

<a id="BKMK_6"></a>

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>El estado de un disco dinámico es Desconectado o Falta.

**Causa:** un disco dinámico **Desconectado** puede estar dañado o no disponible de forma intermitente. Aparece un icono de error en el disco dinámico desconectado.

Si el estado del disco es **Desconectado** y el nombre del disco cambia a **Falta**, el disco estaba recientemente disponible en el sistema, pero ya no puede encontrarse ni identificarse. El disco que falta puede estar dañado, apagado o desconectado.

**Solución:** para conectar un disco que tiene el estado Desconectado y Falta:
1. Repara cualquier problema de cable, controlador o disco. 
2. Asegúrate de que el disco físico está activado y conectado al equipo. 
3. Luego usa el comando **Reactivar disco** para conectar el disco.

4. Si el estado del disco permanece **Desconectado** y el nombre del disco permanece en el estado **Falta** y determinas que el disco tiene un problema que no puede repararse, puedes quitar el disco del sistema haciendo clic con el botón derecho en el disco y luego haciendo clic en **Quitar disco**). Sin embargo, antes de quitar el disco, debes eliminar todos los volúmenes (o reflejos) en el disco. Puedes guardar los volúmenes reflejados en el disco quitando el reflejo en lugar de todo el volumen. Eliminar un volumen destruye los datos en el volumen, por lo que debes quitar un disco solo si estás completamente seguro de que el disco está dañado y no puede usarse de forma permanente.

**Para conectar un disco que está desconectado y sigue denominándose Disco \# (no falta), prueba uno o varios de los siguientes procedimientos:**

1. En Administración de discos, haz clic con el botón derecho en el disco y luego haz clic en **Reactivar disco** para conectar el disco. Si el estado del disco permanece **Desconectado**, comprueba los cables y el controlador del disco y asegúrate de que el disco físico sea el correcto. Corrige los problemas e intenta activar de nuevo el disco. Si la reactivación del disco se realiza correctamente, los volúmenes en el disco deben volver automáticamente al estado **Correcto**.
2. En Visor de eventos, comprueba los registros de eventos de los errores relacionados con el disco, como por ejemplo, "Sin copias de configuración buenas". Si los registros de eventos incluyen este error, ponte en contacto con los [Servicios de soporte técnico de Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Intenta mover el disco a otro equipo. Si puedes hacer que el disco cambie al estado **Conectado** en otro equipo, es muy probable que el problema se deba a la configuración del equipo en la que el disco no cambia al estado **Conectado**.

4. Intenta mover el disco a otro equipo que tenga discos dinámicos. Importa el disco en el equipo y luego mueve el disco al equipo en el que no cambiaría al estado **Conectado**. 

<a id="BKMK_7"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>El estado de un volumen básico o dinámico es Error.

**Causa:** el volumen básico o dinámico no se puede iniciar automáticamente, el disco está dañado o el sistema de archivos está dañado. A menos que se pueda reparar el sistema de archivos o el disco, el estado **Error** indica una pérdida de datos.

**Solución:** si se trata de un volumen básico con el estado **Error**:

-   Asegúrate de que el disco físico subyacente está activado y conectado al equipo. No es posible realizar ninguna otra acción de usuario en los volúmenes básicos.

    Si se trata de un volumen dinámico con el estado **Error**: 
-   Asegúrate de que los discos subyacentes están conectados. Si no es así, devuelve los discos al estado **Conectado**. Si se realiza correctamente, el volumen se reinicia automáticamente y vuelve al estado **Correcto**. Si el disco dinámico vuelve al estado **Conectado**, pero el volumen dinámico no vuelve al estado **Correcto**, puedes volver a activar el volumen manualmente. 
    
-   Si el volumen dinámico es un volumen reflejado o RAID-5 con datos antiguos, conectar el disco subyacente no reiniciará automáticamente el volumen. Si se desconectan los discos que incluyen los datos actuales, conéctalos en primer lugar (para permitir que los datos se sincronicen). De lo contrario, reinicia el volumen reflejado o RAID-5 manualmente, y luego ejecuta la herramienta de comprobación de errores o Chkdsk.exe.

<a id="BKMK_8"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>El estado de un volumen básico o dinámico es Desconocido.

**Causa:** el estado **Desconocido** aparece cuando el sector de arranque del volumen está dañado (posiblemente a causa de un virus) y ya no puedes tener acceso a los datos en el volumen. El estado **Desconocido** también aparece cuando instalas un nuevo disco pero no finalizas correctamente el asistente para crear una firma de disco.

**Solución** inicializa el disco. Para obtener instrucciones, consulta [Inicializar nuevos discos](initialize-new-disks.md). 

<a id="BKMK_9"></a>

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>El estado de un volumen dinámico es Datos incompletos.

**Causa:** moviste algunos discos, pero no todos, de un volumen de varios discos. Los datos de este volumen se destruirán a menos que muevas e importes los discos restantes que incluyen este volumen.

**Solución:**  
1. Mueve todos los discos que componen el volumen de varios discos al equipo.

2. Importa los discos. Para obtener instrucciones sobre cómo mover e importar discos, consulta [Mover discos a otro equipo](move-disks-to-another-computer.md).

Si ya no necesitas el volumen de varios discos, puedes importar el disco y crear nuevos volúmenes en él. Para ello: 

1. Haz clic con el botón derecho en el volumen con el estado **Error** o **Error de redundancia** y luego haz clic en **Eliminar volumen**. 

2. Haz clic con el botón derecho en el disco y luego haz clic en **Nuevo volumen**.

<a id="BKMK_10"></a>

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>El estado de un volumen dinámico es Correcto (en riesgo).

**Causa:** indica que el volumen dinámico es accesible en este momento, pero se han detectado errores de E/S en el disco dinámico subyacente. Si se detecta un error de E/S en cualquier parte de un disco dinámico, todos los volúmenes en el disco mostrarán el estado **Correcto (en riesgo)** y un icono de advertencia aparece en el volumen.

Si el estado del volumen es **Correcto (en riesgo)**, un estado del disco subyacente suele estar **Conectado (errores)**.

**Solución:**  
1. Devuelve el disco subyacente al estado **Conectado**. Una vez que el disco vuelve al estado **Conectado**, el volumen debe volver el estado **Correcto**. Si el estado **Correcto (en riesgo)** persiste, es posible que se produzca un error en el disco. 

2. Realiza una copia de seguridad de los datos y sustituye el disco lo antes posible. 

<a id="BKMK_11"></a>

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>No se pueden administrar volúmenes seccionados con Administración de discos o DiskPart.

**Causa:** algunos productos de administración de discos que no son de Microsoft reemplazan el Administrador de discos lógicos (LDM) de Microsoft para la administración avanzada de discos, lo que puede deshabilitarlo.

**Solución:** si estás usando un software de administración de discos que no es de Microsoft que ha deshabilitado LDM, debes ponerte en contacto con el proveedor de dicho software para recibir soporte técnico o ayuda para solucionar problemas con la configuración del disco.

<a id="BKMK_virtdisk"></a>

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Administración de discos no puede iniciar el servicio de disco virtual.

**Causa:** si un equipo remoto no es compatible con el servicio de disco virtual (VDS), o si no se puede establecer una conexión al equipo remoto porque está bloqueado por Firewall de Windows, puede aparecer este error.

**Solución:**  
1. Si el equipo remoto admite VDS, puedes configurar el Firewall de Windows Defender para permitir conexiones VDS. Si el equipo remoto no admite VDS, puedes usar la Conexión a Escritorio remoto para conectarte a él y luego ejecutar Administración de discos directamente en el equipo remoto.

2. Para administrar discos en equipos remotos que admiten VDS, debes configurar el Firewall de Windows Defender en el equipo local (en el que se ejecuta Administración de discos) y el equipo remoto.

3. En el equipo local, configura el Firewall de Windows Defender para habilitar la excepción de Administración remota del volumen.

<br />

> [!NOTE]
> La excepción de Administración remota del volumen incluye excepciones para Vds.exe, Vdsldr.exe y el puerto TCP 135.

<br />

 > [!NOTE]
 > No se admiten las conexiones remotas en grupos de trabajo. Tanto el equipo local como el equipo remoto deben ser miembros de un dominio.