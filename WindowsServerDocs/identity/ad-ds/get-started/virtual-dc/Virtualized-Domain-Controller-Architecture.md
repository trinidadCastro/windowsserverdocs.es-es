---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: Arquitectura de controladores de dominio virtualizados
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b644103342e94a171699efeab238453bdb583eec
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067807"
---
# <a name="virtualized-domain-controller-architecture"></a>Arquitectura de controladores de dominio virtualizados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe la arquitectura de clonación de controles de dominio virtualizados y la restauración segura. Se mostrarán los procesos de clonación y restauración segura con diagramas de flujo y, después, podrás ver una explicación detallada de todos los pasos del proceso.

-   [Arquitectura de clonación de controles de dominio virtualizados](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)

-   [Arquitectura de restauración segura de controladores de dominio virtualizados](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)

## <a name="virtualized-domain-controller-cloning-architecture"></a><a name="BKMK_CloneArch"></a>Arquitectura de clonación de controles de dominio virtualizados

### <a name="overview"></a>Información general
La clonación de controles de dominio virtualizados depende de la plataforma del hipervisor para exponer un identificador llamado **identificador de generación de VM** a fin de detectar la creación de máquinas virtuales. AD DS almacena inicialmente el valor de este identificador en su base de datos (NTDS.DIT) durante la promoción de controladores de dominio. Cuando se arranca la máquina virtual, el valor actual del identificador de generación de VM de esta se compara con el valor de la base de datos. Si los valores son distintos, el controlador de dominio restaurará el identificador de invocación y descartará el grupo RID, lo que evita que USN pueda volver a crear entidades de seguridad duplicadas. El controlador de dominio buscará el archivo DCCloneConfig.xml en las ubicaciones invocadas en el paso 3 en [Procesamiento detallado de clonación](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). Si se encuentra el archivo DCCloneConfig.xml, entonces asumirá que se ha implementado como clon, por lo que iniciará la clonación para aprovisionarse como controlador de dominio adicional (para ello, vuelve a promocionarse mediante los contenidos de NTDS.DIT y de SYSVOL copiados desde el medio de origen).

En un entorno mixto, donde algunos hipervisores admiten VM-GenerationID y otros no, es posible que se implemente por error un medio clonado en un hipervisor que no admita VM-GenerationID. La presencia del archivo DCCloneConfig.xml indica el intento administrativo de clonar un controlador de dominio (DC). Por lo tanto, si se encuentra un archivo DCCloneConfig.xml durante el arranque, pero el host no proporciona ningún VM-GenerationID, el DC clonado se iniciará en el modo de restauración de servicios de directorio (DSRM) para que no afecte al resto del entorno. El medio clonado se puede mover posteriormente a un hipervisor que admita VM-GenerationID y, después, se puede volver a intentar la clonación.

Si el medio clonado se implementa en un hipervisor que admite VM-GenerationID, pero no se proporciona el archivo DCCloneConfig.xml, como el DC detecta un cambio de VM-GenerationID entre su DIT y el de la nueva VM, activará medidas de seguridad para evitar que se vuelva a usar USN y que se creen SID duplicados. Sin embargo, la clonación no se iniciará, por lo que el DC secundario continuará ejecutándose con la misma identidad que el DC de origen. Este DC secundario debe quitarse de la red lo antes posible para evitar incoherencias en el entorno. Para obtener más información sobre cómo recuperar este DC secundario y asegurarse de que las actualizaciones se replican de salida, consulte el artículo [2742970](https://support.microsoft.com/kb/2742970)de Microsoft Knowledge base.

### <a name="cloning-detailed-processing"></a><a name="BKMK_CloneProcessDetails"></a>Procesamiento detallado de clonación
En el diagrama siguiente puedes ver la arquitectura de una operación de clonación inicial y de una operación de reintento de clonación. Estos procesos se explican con más detalle posteriormente en este tema.

**Operación de clonación inicial**

![Arquitectura de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)

**Operación de reintento de clonación**

![Arquitectura de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)

En los pasos siguientes se explica el proceso con más detalle:

1.  Un controlador de dominio de máquina virtual existente se inicia en un hipervisor que admite el identificador de generación de VM.

    1.  Esta VM no tiene ningún valor de identificador de generación de VM existente establecido en el objeto de equipo de AD DS después de la promoción.

    2.  Incluso aunque esté vacío, si vuelve a crearse un equipo, este se clonará, ya que el nuevo identificador de generación de VM no coincidirá.

    3.  El identificador de generación de VM se establece después del siguiente reinicio en el DC y no se replica.

2.  Después, la máquina virtual lee el identificador de generación de VM proporcionado por el controlador VMGenerationCounter. Compara los dos identificadores de generación de VM.

    1.  Si los identificadores coinciden, quiere decir que no se trata de una máquina virtual nueva y, por lo tanto, no se realizará la clonación. Si existe un archivo DCCloneConfig.xml, el controlador de dominio cambiará el nombre del archivo con una marca de fecha y hora para evitar la clonación. El servidor continuará con el arranque normal. Así funcionan los reinicios de todos los controladores de dominio virtuales en Windows Server 2012.

    2.  Si los identificadores no coinciden, quiere decir que se trata de una máquina virtual nueva que contiene un NTDS.DIT de un controlador de dominio anterior (o que es una instantánea restaurada). Si existe el archivo DCCloneConfig.xml, el controlador de dominio realizará las operaciones de clonación. En caso contrario, continuará con las operaciones de restauración de instantánea. Consulta [Arquitectura de restauración segura de controladores de dominio virtualizados](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

    3.  Si el hipervisor no proporciona ningún identificador de generación de VM para compararlo, pero existe un archivo DCCloneConfig.xml, el invitado cambiará el nombre del archivo y, después, se iniciará en el modo DSRM para evitar que se cree un controlador de dominio duplicado en la red. Si no existe el archivo DCCloneConfig.xml, el invitado se iniciará en modo normal (con la posibilidad de que se cree un controlador de dominio duplicado en la red). Para obtener más información sobre cómo recuperar este controlador de dominio duplicado, consulta el artículo [2742970](https://support.microsoft.com/kb/2742970) de Microsoft KB.

3.  El servicio NTDS comprueba el nombre del valor DWORD del Registro VDCisCloning (en HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters).

    1.  Si no existe, este será un primer intento de clonar la máquina virtual. El invitado implementa las medidas de seguridad de duplicación de objetos de VDC, que consisten en la invalidación del grupo RID local y la configuración de un nuevo identificador de invocación de replicación para el controlador de dominio

    2.  Si ya se ha establecido en 0x1, este será un intento de clonación de “reintento” (es decir, cuando no ha podido completarse una operación de clonación anterior). No se toman las medidas de seguridad de duplicación de objetos de VDC, ya que deberían haberse ejecutado anteriormente y esto modificaría varias veces y de manera innecesaria el invitado.

4.  Se escribirá el nombre del valor DWORD del Registro IsClone (en Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters)

5.  El servicio NTDS cambia la marca de arranque de invitado para que se inicie en el modo de reparación de DS para reinicios posteriores.

6.  El servicio NTDS intenta leer el archivo DcCloneConfig.xml en una de las tres ubicaciones aceptadas (directorio de trabajo de DSA, %windir%\NTDS o medios extraíbles de lectura/escritura, ordenados por letra de unidad, en la raíz de la unidad).

    1.  Si el archivo no existe en ninguna ubicación válida, el invitado comprueba que la dirección IP no esté duplicada. Si la dirección IP no está duplicada, el servidor se iniciará en modo normal. Si existe una dirección IP duplicada, el equipo se iniciará en el modo DSRM para evitar que se cree un controlador de dominio duplicado en la red.

    2.  Si el archivo existe en una ubicación válida, el servicio NTDS validará su configuración. Si el archivo está en blanco (o alguna de las opciones está en blanco), NTDS configurará valores automáticos para dichas opciones.

    3.  Si existe el archivo DcCloneConfig.xml, pero este contiene entradas no válidas o no se puede leer, la clonación dará error y el invitado se iniciará en el modo de restauración de servicios de directorio (DSRM).

7.  El invitado deshabilita el registro automático de DNS para evitar que pueda modificarse de forma accidental el nombre de equipo y las direcciones IP de origen.

8.  El invitado detiene el servicio de Netlogon para evitar la publicación o respuesta de solicitudes de AD DS de red por parte de clientes.

9. NTDS comprueba que no haya servicios ni programas instalados que no formen parte de DefaultDCCloneAllowList.xml o de CustomDCCloneAllowList.xml

    1.  Si hay instalados servicios o programas que no se especifican en la lista de permitidos de exclusión predeterminada o en la lista de permitidos de exclusión personalizada, la clonación no se completará y el invitado se iniciará en el modo DSRM para evitar que se cree un controlador de dominio duplicado en la red.

    2.  Si no existen incompatibilidades, se continuará con la clonación.

10. Si se usa el direccionamiento IP automático porque la configuración de red del archivo DCCloneConfig.xml está en blanco, el invitado habilitará DHCP en los adaptadores de red para obtener la concesión de dirección IP, el enrutamiento de red e información de resolución de nombres.

11. El invitado busca y contacta con el controlador de dominio que ejecuta el rol FSMO del emulador de PDC. Este usa DNS y el protocolo DCLocator. Establece una conexión RPC e invoca el método IDL_DRSAddCloneDC para clonar el objeto de equipo del controlador de dominio.

    1.  Si el objeto de equipo de origen del invitado tiene el permiso extendido de encabezado de dominio “Permitir a un DC crear un clon de sí mismo”, se continuará con la clonación.

    2.  Si el objeto de equipo de origen del invitado no tiene dicho permiso extendido, la clonación se detendrá y el invitado se iniciará en el modo DSRM para evitar que se cree un controlador de dominio duplicado en la red.

12. El nombre de objeto de equipo de AD DS se establece para que coincida con el nombre especificado en el archivo DCCloneConfig.xml (si existe); en caso contrario, se genera automáticamente en el PDCE. NTDS crea la opción de NTDS correcta para el sitio lógico apropiado de Active Directory.

    1.  En el caso de una clonación de PDC, el invitado cambiará el nombre del equipo local y se reiniciará. Después del reinicio, pasa por el paso 1-10 de nuevo y, después, pasa al paso 13.

    2.  Si esto es una clonación de DC de réplica, no se reiniciará en esta fase.

13. El invitado proporciona la configuración de promoción al servicio de servidor de roles de DS, que iniciará la promoción.

14. El servicio de servidor de roles de DS detendrá todos los servicios relacionados con AD DS (NTDS, NTFRS/DFSR, KDC, DNS).

15. El invitado forzará la sincronización de hora de NT5DS (NTP de Windows) con otro controlador de dominio (en una jerarquía de Servicio de hora de Windows, se usará el PDCE). El invitado contactará con el PDCE. Se eliminarán todos los vales de Kerberos existentes.

16. El invitado configura los servicios DFSR o NTFRS para que se ejecuten automáticamente. El invitado elimina todos los archivos de base de datos DFSR y NTFRS existentes (predeterminado: c:\WINDOWS\ntfrs y c:\System Volume Information\DFSR \\ *<database_GUID>* ), con el fin de forzar la sincronización no autoritativa de SYSVOL cuando se inicia el servicio. El invitado no elimina el contenido de los archivos de SYSVOL para preinicializar el SYSVOL cuando se inicie posteriormente la sincronización.

17. Se cambiará el nombre del invitado. El servicio de servidor de roles de DS en el invitado comenzará la configuración de AD DS (promoción) con el archivo de base de datos NTDS.DIT existente como origen, en lugar de la base de datos de plantilla incluida en c:\windows\system32, como suele hacerse en una promoción.

18. El invitado contacta con el propietario del rol FSMO para obtener una nueva asignación de grupo RID.

19. El proceso de promoción crea un nuevo identificador de invocación y vuelve a crear el objeto de configuración de NTDS para el controlador de dominio clonado (independientemente de la clonación, esto forma parte de la promoción de dominios al usar una base de datos NTDS.DIT existente).

20. NTDS se replica en objetos no encontrados, más nuevos o que tienen una versión superior de un controlador de dominio asociado. El archivo NTDS.DIT ya contiene objetos del momento en que el controlador de dominio de origen estaba sin conexión, y estos se usarán como objetos posibles para reducir la entrada de tráfico de replicación. Se rellenan las particiones del catálogo global.

21. Se inicia el servicio DFSR o FRS y, como no existe ninguna base de datos, SYSVOL sincroniza de forma no autoritativa la entrada de un asociado de replicación. Este proceso reutiliza datos existentes en la carpeta SYSVOL para reducir el tráfico de replicación en la red.

22. El invitado vuelve a habilitar el registro del cliente DNS ahora que el equipo tiene un nombre único y está conectado a la red.

23. El invitado ejecuta los módulos de SYSPREP especificados en el elemento <SysprepInformation> del archivo DefaultDCCloneAllowList.xml para eliminar los datos de referencias al nombre de equipo y el SID anteriores.

24. Se completa la promoción de la clonación.

    1.  El invitado elimina la marca de arranque de DSRM para que el siguiente reinicio se realice en el modo normal.

    2.  El invitado cambia el nombre del archivo DCCloneConfig.xml (anexa una marca de fecha y hora) para que no vuelva a leerse en el siguiente arranque.

    3.  El invitado elimina el valor DWORD del Registro VdcIsCloning en HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.

    4.  El invitado establece en 0x1 el valor DWORD del Registro “VdcCloningDone” en HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters. Windows no usa este valor, sino que lo proporciona como un marcador para terceros.

25. El invitado actualiza el atributo msDS-GenerationID en su propio objeto de controlador de dominio clonado para que coincida con el identificador de generación de VM actual del invitado.

26. Se reinicia el invitado. Ahora es un controlador de dominio de publicación normal.

## <a name="virtualized-domain-controller-safe-restore-architecture"></a><a name="BKMK_SafeRestoreArch"></a>Arquitectura de restauración segura de controladores de dominio virtualizados

### <a name="overview"></a>Información general
AD DS depende de la plataforma del hipervisor para exponer un identificador llamado **identificador de generación de VM** a fin de detectar la restauración de instantáneas de máquinas virtuales. AD DS almacena inicialmente el valor de este identificador en su base de datos (NTDS.DIT) durante la promoción de controladores de dominio. Cuando un administrador restaura una máquina virtual a partir de una instantánea anterior, el valor actual del identificador de generación de VM de esta se compara con el valor de la base de datos. Si los valores son distintos, el controlador de dominio restaurará el identificador de invocación y descartará el grupo RID, lo que evita que USN pueda volver a crear entidades de seguridad duplicadas. Existen dos escenarios en los que se puede producir una restauración segura:

-   Cuando se inicia un controlador de dominio virtual después de restaurar una instantánea cuando esta estaba apagada

-   Cuando se restaura una instantánea en un controlador de dominio virtual en ejecución

    Si el controlador de dominio virtualizado en la instantánea se encuentra en estado suspendido, en lugar de apagado, tendrás que reiniciar el servicio AD DS para activar una nueva solicitud de grupo RID. Puedes reiniciar el servicio AD DS mediante el complemento Servicios o mediante Windows PowerShell (Restart-Service NTDS -force).

En las secciones siguientes se explica en detalle la restauración segura para cada escenario.

### <a name="safe-restore-detailed-processing"></a>Procesamiento detallado de restauración segura
En el diagrama de flujo siguiente puedes ver cómo se produce una restauración segura cuando se inicia un controlador de dominio virtual después de restaurar una instantánea cuando esta estaba apagada.

![Arquitectura de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)

1.  Cuando se inicia la máquina virtual después de una restauración de instantánea, tendrá un nuevo identificador de generación de VM proporcionado por el host del hipervisor debido a la restauración de la instantánea.

2.  El nuevo identificador de generación de la máquina virtual se compara con el identificador de generación de VM de la base de datos. Como los dos identificadores no coinciden, usará medidas de seguridad de virtualización (consulta el paso 3 de la sección anterior). Cuando se termine de aplicar la restauración, se actualizará el VM-GenerationID establecido en el objeto de equipo de AD DS para que coincida con el nuevo identificador proporcionado por el host del hipervisor.

3.  El invitado usa medidas de seguridad de virtualización para:

    1.  Invalidar el grupo RID local.

    2.  Establecer un nuevo identificador de invocación para la base de datos del controlador de dominio.

> [!NOTE]
> Esta parte de la restauración segura se superpone con el proceso de clonación. Aunque este proceso trata sobre la restauración segura de un controlador de dominio virtual después de arrancar una restauración de instantánea, también se realizan los mismos pasos durante el proceso de clonación.

En el diagrama siguiente se muestra cómo las medidas de seguridad de virtualización evitan la divergencia inducida por la reversión de USN cuando se restaura una instantánea en un controlador de dominio virtual en ejecución.

![Arquitectura de controlador de dominio virtualizado](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)

> [!NOTE]
> La ilustración anterior se ha simplificado para explicar los conceptos.

1.  En la hora T1, el administrador del hipervisor realiza un instantánea del DC1 virtual. En ese momento, DC1 tiene un valor USN ( **highestCommittedUsn** , en un caso real) de 100, InvocationId (representado como el identificador en el diagrama anterior) tiene un valor de A (en un caso real, esto sería el GUID). El valor de savedVMGID es el VM-GenerationID en el archivo DIT del DC (almacenado para el objeto de equipo del DC en un atributo llamado **msDS-GenerationId** ). El VMGID es el valor actual del VM-GenerationID obtenido por el controlador de la máquina virtual. Este valor es proporcionado por el hipervisor.

2.  Posteriormente, en T2, se agregan 100 usuarios a este DC (esto es tan solo un ejemplo de las actualizaciones que podrían realizarse en este DC entre T1 y T2; estas actualizaciones podrían ser una combinación de creación de usuarios, creación de grupos, actualizaciones de contraseña, actualizaciones de atributos, etc.). En este ejemplo, cada actualización consume un USN único (aunque, en la práctica, la creación de un usuario podría consumir más de un USN). Antes de aplicar estas actualizaciones, DC1 comprueba si el valor de VM-GenerationID de la base de datos (savedVMGID) coincide con el valor actual disponible en el controlador (VMGID). Coinciden, ya que no se ha realizado ninguna reversión, por lo que se aplican las actualizaciones y USN sube hasta 200, lo que indica que en la siguiente actualización se puede usar el USN 201. No se producen cambios en InvocationId, savedVMGID o VMGID. Estas actualizaciones se replican en DC2 en el siguiente ciclo de replicación. DC2 actualiza su límite máximo (y **UptoDatenessVector** ), que aquí se representa simplemente como DC1 (A) @USN = 200. Es decir, que DC2 conoce todas las actualizaciones de DC1 en el contexto de InvocationId A hasta USN 200.

3.  En T3, se aplica en DC1 la instantánea realizada en T1. DC1 se ha revertido, por lo que su USN se revierte a 100, lo que indica que podría usar USN a partir del 101 para asociarlos con próximas actualizaciones. Sin embargo, en este punto, el valor de VMGID sería distinto en hipervisores que admitan el VM-GenerationID.

4.  Como consecuencia, cuando DC1 realice una actualización, comprobará si el valor de VM-GenerationId que contiene su base de datos (savedVMGID) coincide con el valor del controlador de la máquina virtual (VMGID). En este caso no es el mismo, por lo que DC1 detecta esto como una reversión y activa las medidas de seguridad de virtualización; en otras palabras, restablece su InvocationId (Id. = B) y descarta el grupo RID (que no se muestra en el diagrama anterior). A continuación, guarda el nuevo valor de VMGID en su base de datos y confirma esas actualizaciones (USN 101-250) en el contexto del nuevo invocación de B. En el siguiente ciclo de replicación, DC2 no sabe nada de DC1 en el contexto del ID. de invocación B, por lo que solicita todo desde DC1 asociado con el ID. de invocación B. Como resultado, las actualizaciones realizadas en DC1 después de la aplicación de instantánea se convergirán de forma segura. Además, el conjunto de actualizaciones que se realizó en DC1 en T2 (que se perdieron en DC1 después de restaurar la instantánea) se volverían a replicar en DC1 en la siguiente replicación programada, ya que se replicaron a DC2 (como indica la línea de puntos que vuelve a DC1).

Después de que el invitado use las medidas de seguridad de virtualización, NTDS replica las diferencias del objeto de Active Directory de entrada de forma no autoritativa desde un controlador de dominio asociado. Como resultado, se actualiza el vector de actualización del servicio de directorio de destino. Después, el invitado sincroniza SYSVOL:

-   Si se usa FRS, el invitado detiene el servicio NTFRS y establece el valor del Registro BURFLAGS de D2. Después, inicia el servicio NTFRS que, de forma no autoritativa, realiza una replicación entrante y, siempre que sea posible, vuelve a usar los datos de SYSVOL no modificados.

-   Si se usa DFSR, el invitado detiene el servicio DFSR y elimina los archivos de base de datos de DFSR (ubicación predeterminada:%SystemRoot%\System Volume Information\DFSR \\ *<database GUID>* ). Después, inicia el servicio DFSR que, de forma no autoritativa, realiza una replicación entrante y, siempre que sea posible, vuelve a usar los datos de SYSVOL no modificados.

> [!NOTE]
> -   Si el hipervisor no proporciona ningún identificador de generación de VM para poder compararlo, no admitirá las medidas de seguridad de virtualización y el invitado funcionará como un controlador de dominio virtualizado que ejecuta Windows Server 2008 R2 o anterior. El invitado implementa la protección de cuarentena de reversión de USN si se intenta iniciar una replicación con unos USN no superiores a los últimos USN detectados por el DC asociado. Para obtener más información sobre la protección de cuarentena de la reversión de USN, consulta [USN y reversión de USN](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))

