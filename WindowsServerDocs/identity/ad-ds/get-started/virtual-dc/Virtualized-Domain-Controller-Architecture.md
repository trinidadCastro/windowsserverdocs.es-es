---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: Arquitectura de controlador de dominio virtualizada
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac8b190df065547d82aa431761eb5c00c94a2ad6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-architecture"></a>Arquitectura de controlador de dominio virtualizada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema cubre la arquitectura de controlador de dominio virtualizada clonación y restauración segura. Muestra los procesos de clonación y restauración segura con diagramas de flujo y, a continuación, se proporciona una explicación detallada de cada paso del proceso.  
  
-   [Controlador de dominio virtualizada clonación arquitectura](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [Arquitectura de restauración segura del controlador de dominio virtualizada](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>Controlador de dominio virtualizada clonación arquitectura  
  
### <a name="overview"></a>Introducción  
Clonación del controlador de dominio virtualizada se basa en la plataforma de hipervisor para exponer un identificador denominado **Id. de generación de la máquina virtual** para detectar la creación de una máquina virtual. AD DS inicialmente almacena el valor de este identificador en su base de datos (NTDS. DIT) durante la promoción del controlador de dominio. Cuando se inicia la máquina virtual, el valor actual del Id. de generación de la máquina virtual de la máquina virtual se compara con el valor de la base de datos. Si los dos valores son diferentes, el controlador de dominio restablece el Id. de invocación y descarta el conjunto de RID, impidiendo de USN volver a usar o la creación de entidades de seguridad duplicadas posible. El controlador de dominio, a continuación, busca un archivo DCCloneConfig.xml en las ubicaciones resaltados en el paso 3 en [clonación procesamiento detallada](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails). Si encuentra un archivo DCCloneConfig.xml, se deduce que se implementa como un clone, por lo que se inicia la clonación aprovisionar como un controlador de dominio mediante el uso de volver a promover la NTDS existente. Contenido DIT y SYSVOL copia del medio de origen.  
  
En un entorno donde algunos hipervisores admiten GenerationID de máquina virtual y otras no, es posible que un medio clone que accidentalmente se implementará en un hipervisor que no es compatible con la máquina virtual GenerationID. La presencia del archivo DCCloneConfig.xml indica administrativa intención de clonar un controlador de dominio. Por lo tanto, si se encuentra un archivo de DCCloneConfig.xml durante el arranque, pero un GenerationID de máquina virtual no se proporciona del host, el controlador de dominio duplicado se arranca en modo de restauración de servicios de directorio (DSRM) para evitar cualquier impacto en el resto del entorno. Los medios clone se pueden mover posteriormente a un hipervisor que admite la máquina virtual GenerationID y clonación, a continuación, se puede reintentar.  
  
Si los medios clone se implementan en un hipervisor que admite la máquina virtual GenerationID pero no se proporciona un archivo DCCloneConfig.xml, como el controlador de dominio detecta un cambio de máquina virtual GenerationID entre su DIT y el de la nueva máquina virtual, se activará medidas de seguridad para evitar que vuelva a utilizar USN y evitar SID duplicados. Sin embargo, clonación no se iniciará, por lo que el controlador de dominio secundario se seguirá ejecutando en la misma identidad que el origen de controlador de dominio. Este controlador de dominio secundario debe quitarse de la red en el tiempo lo antes posible para evitar cualquier incoherencias en el entorno. Para obtener más información sobre cómo recuperar este controlador de dominio secundario al mismo tiempo que las actualizaciones se replican salientes, consulta Microsoft KB [2742970](https://support.microsoft.com/kb/2742970).  
  
### <a name="BKMK_CloneProcessDetails"></a>Clonación procesamiento detallada  
El siguiente diagrama muestra la arquitectura para una operación de clonación inicial y para una operación de reintento clonación. Estos procesos se explican con más detalle más adelante en este tema.  
  
**Operación de clonación inicial**  
  
![Arquitectura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**Operación de reintento clonación**  
  
![Arquitectura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
Los siguientes pasos explican el proceso con más detalle:  
  
1.  Inicia un controlador de dominio existente de máquina virtual en un hipervisor que admita la Id. de generación de la máquina virtual.  
  
    1.  Esta máquina virtual no tiene establece ningún valor de identificador de generación de VM existente en su objeto de equipo de AD DS después de la promoción.  
  
    2.  Aunque es null, la creación de equipo siguiente significará se sigue clones, como un nuevo generación de VM-ID no coincidirán.  
  
    3.  El identificador de generación de máquina virtual se establece en el siguiente reinicio del controlador de dominio y no se replica.  
  
2.  La máquina virtual, a continuación, lee el identificador de generación de la máquina virtual proporcionado por el controlador VMGenerationCounter. Compara los dos identificadores de generación de la máquina virtual.  
  
    1.  Si coinciden con los identificadores, esto no es una nueva máquina virtual y clonación no seguirá adelante. Si existe un archivo DCCloneConfig.xml, el controlador de dominio cambia el nombre del archivo con una marca de hora y fecha para evitar la clonación. El servidor continúa arranque con normalidad. Este es el funcionamiento de cada reinicio del cualquier controlador de dominio virtuales en Windows Server 2012.  
  
    2.  Si no coinciden con los identificadores de dos, esto es una nueva máquina virtual que contiene un NTDS. DIT desde un controlador de dominio anterior (o es una instantánea restaurada). Si existe un archivo DCCloneConfig.xml, el controlador de dominio continúa con las operaciones de clonación. De lo contrario, continúa con operaciones de restauración de instantánea. Consulta [virtualizados arquitectura de restauración segura del controlador de dominio](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).  
  
    3.  Si el hipervisor no proporciona un identificador de generación de la máquina virtual para fines de comparación, pero hay un archivo DCCloneConfig.xml, el invitado cambia el nombre del archivo y, a continuación, arranca en DSRM para proteger la red de un controlador de dominio duplicado. Si no hay ningún archivo dccloneconfig.xml, arranca el invitado normalmente (con la posibilidad de un controlador de dominio duplicados en la red). Para obtener más información sobre cómo recuperar este controlador de dominio duplicados, consulta el artículo [2742970](https://support.microsoft.com/kb/2742970).  
  
3.  El servicio NTDS comprueba el valor del nombre del valor de DWORD VDCisCloning del registro (bajo HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters).  
  
    1.  Si no existe, este es un primer intento clonación para esta máquina virtual. El invitado implementa las medidas de seguridad de la duplicación de objeto v CC de invalidar el grupo de RID local y la configuración de un nuevo identificador de invocación de replicación del controlador de dominio  
  
    2.  Si ya está establecido en 0 x 1, este es un "Reintentar" clonación intento, donde no se pudo realizar en una operación de clonación anterior. No se toman las medidas de seguridad v CC duplicación de objeto que tenían que ya se ha ejecutado una vez antes e innecesariamente podrían modificar al invitado de varias veces.  
  
4.  Se escribe el nombre del valor del registro IsClone DWORD (bajo Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters)  
  
5.  El servicio NTDS cambia la marca de arranque de invitado se inicie en modo de reparación de DS para cualquier reiniciar el dispositivo.  
  
6.  El servicio NTDS intenta leer el DcCloneConfig.xml en una de las tres ubicaciones aceptadas (directorio de trabajo de DSA, % windir%\NTDS o medios extraíbles de lectura y escritura, por orden de la letra de unidad, en la raíz de la unidad).  
  
    1.  Si el archivo no existe en cualquier ubicación válida, el invitado comprueba la dirección IP de duplicación. Si no se duplica la dirección IP, el servidor inicia normalmente. Si hay una dirección IP duplicada, el equipo arranca en DSRM para proteger la red de un controlador de dominio duplicado.  
  
    2.  Si el archivo existe en una ubicación válida, el servicio NTDS valida su configuración. Si el archivo está en blanco (o cualquier configuración particular está en blanco) NTDS configura valores automáticos para esa configuración.  
  
    3.  Si la DcCloneConfig.xml existe pero contiene entradas no válidas o no es legible, se produce un error y el invitado de clonación arranca en modo de restauración de servicios de directorio (DSRM).  
  
7.  El invitado deshabilita todos los DNS registro automático para evitar que accidentalmente secuestro de nombre de equipo de origen y las direcciones IP.  
  
8.  El invitado detiene el servicio de Netlogon para evitar cualquier publicidad o responder a AD DS solicitudes de red de los clientes.  
  
9. NTDS valida que no hay ningún servicio ni programas instalados que no forman parte del DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml  
  
    1.  Si hay servicios o programas instalados que no están en la exclusión de forma predeterminada la lista de permitidos o la exclusión personalizada de lista de permitidos, clonación produce un error y el invitado arranca en DSRM para proteger la red de un controlador de dominio duplicado.  
  
    2.  Si no hay ningún incompatibilidades, clonación continúa.  
  
10. Si el direccionamiento IP automático se usarán debido a la configuración de red DCCloneConfig.xml en blanco, el invitado permite DHCP en los adaptadores de red para obtener una IP concesión de dirección, enrutamiento de red y la información de resolución de nombre.  
  
11. El invitado localiza y se pone en contacto con el controlador de dominio que se ejecuta el emulador PDC función FSMO. Utiliza DNS y el protocolo DCLocator. Establece una conexión de RPC y llama al método IDL_DRSAddCloneDC clonar el objeto de equipo del controlador de dominio.  
  
    1.  Si objeto de equipo de origen del invitado contiene permiso extendida cabeza en el dominio de "' Permitir que un controlador de dominio crear una copia de sí mismo", a continuación, clonación ganancias.  
  
    2.  Si el objeto de equipo de origen del invitado no guarda que arranca extendida permiso, se produce un error y el invitado de clonación dsrm para proteger la red de un controlador de dominio duplicado.  
  
12. Se establece el nombre del objeto de equipo de AD DS para que coincida con el nombre especificado en el DCCloneConfig.xml, si lo hay, o bien se genera automáticamente en el PDCE. NTDS crea el objeto de configuración NTDS correcto para el sitio de Active Directory lógico apropiado.  
  
    1.  Si se trata de un PDC clonación, a continuación, el invitado cambia el nombre del equipo local y se reinicia. Después del reinicio, pasa por el paso 1-10 nuevo, a continuación, entra en el paso 13.  
  
    2.  Si se trata de una réplica DC clonación, no hay ningún reinicio en esta etapa.  
  
13. El invitado proporciona la configuración de promoción para el servicio de rol servidor de DS, que comience la promoción.  
  
14. El servicio de rol servidor de DS deja todos los servicios relacionados con el DS AD (NTDS, NTFRS/DFSR, KDC, DNS).  
  
15. Hace que el invitado NT5DS sincronización de hora (Windows NTP) con otro controlador de dominio (en una jerarquía de servicio hora de Windows de forma predeterminada, esto significa usar la PDCE). El invitado pone en contacto con el PDCE. Vaciar todos los vales Kerberos existentes.  
  
16. El invitado configura los servicios DFSR o NTFRS para ejecutarse automáticamente. El invitado elimina todos los archivos de base de datos existentes DFSR y NTFRS (predeterminado: c:\windows\ntfrs y c:\system information\dfsr\\ volumen*< guid_base_de_datos >*), para forzar la sincronización no autorizado de SYSVOL cuando se inicie el servicio. El invitado no elimina el contenido del archivo de SYSVOL para inicializar previamente SYSVOL cuando la sincronización se inicia más adelante.  
  
17. Se cambia el invitado. El servicio de rol servidor de DS en el invitado comienza la configuración de AD DS (promoción), mediante la NTDS existente. Archivo de base de datos DIT como un origen, en lugar de la base de datos de plantilla que se incluye en c:\windows\system32 como una promoción normalmente lo hace.  
  
18. El invitado pone en contacto con el titular de la función eliminar maestro FSMO para obtener una asignación de bloque RID nuevo.  
  
19. El proceso de promoción crea un nuevo identificador de invocación y vuelve a crear el objeto de configuración NTDS del controlador de dominio clonados (independientemente de la clonación, esto forma parte de promoción de dominio cuando se usa una NTDS existente. Base de datos DIT).  
  
20. NTDS se replica en objetos que falta, más reciente, o que tienen una versión posterior de un controlador de dominio de partners. NTDS. DIT ya contiene objetos desde el momento en el controlador de dominio de origen se desconectó y los que se usan como sea posible para minimizar el tráfico de replicación entrante. Se rellenan las particiones de catálogo global.  
  
21. El servicio DFSR o FRS inicia y sincroniza entrante de un duplicador porque no hay ninguna base de datos, SYSVOL no autorizada. Este proceso utiliza volver a los datos existentes en la carpeta SYSVOL, con el fin de minimizar el tráfico de replicación de red.  
  
22. El invitado vuelve a habilita el registro del cliente DNS ahora que el equipo de forma exclusiva denominado y en red.  
  
23. El invitado ejecuta los módulos SYSPREP especificados por el DefaultDCCloneAllowList.xml <SysprepInformation> elemento con el fin de arrastre las referencias a la anterior nombre del equipo y el SID.  
  
24. Promoción de clonación está completa.  
  
    1.  El invitado quita la marca de arranque DSRM para que el siguiente reinicio será normal.  
  
    2.  El invitado cambia el nombre de la DCCloneConfig.xml con una marca de fecha y hora anexada, para que no se lee nuevamente en el próximo arranque.  
  
    3.  El invitado quita el nombre del valor del Registro DWORD VdcIsCloning en HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters.  
  
    4.  El invitado establece DWORD "VdcCloningDone" nombre de valor del registro bajo HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 0 x 1. Windows no usa este valor, pero en su lugar proporciona como un marcador de terceros.  
  
25. El atributo msDS GenerationID en su propio objeto de controlador de dominio clonados para que coincida con el Id. de la máquina virtual de generación de invitado actual actualiza el invitado  
  
26. Reinicia el invitado. Ahora es normal, controlador de dominio de publicidad.  
  
## <a name="BKMK_SafeRestoreArch"></a>Arquitectura de restauración segura del controlador de dominio virtualizada  
  
### <a name="overview"></a>Introducción  
AD DS se basa en la plataforma de hipervisor para exponer un identificador denominado **Id. de generación de la máquina virtual** para detectar la restauración de la instantánea de una máquina virtual. AD DS inicialmente almacena el valor de este identificador en su base de datos (NTDS. DIT) durante la promoción del controlador de dominio. Cuando un administrador restablece la máquina virtual desde una instantánea anterior, el valor actual del Id. de generación de la máquina virtual de la máquina virtual se compara con el valor de la base de datos. Si los dos valores son diferentes, el controlador de dominio restablece el Id. de invocación y descarta el conjunto de RID, impidiendo de USN volver a usar o la creación de entidades de seguridad duplicadas posible. Existen dos escenarios donde puede producirse restauración segura:  
  
-   Cuando se inicia un controlador de dominio virtual después de restaurar una instantánea mientras estaba cerrado  
  
-   Cuando se restaura una instantánea en un controlador de dominio virtual en ejecución  
  
    Si el controlador de dominio virtualizada de la instantánea está en un estado suspendido en lugar de apagado, debes reiniciar el servicio de AD DS para desencadenar una nueva solicitud de grupo RID. Puedes reiniciar el servicio de AD DS mediante el complemento Servicios o Windows PowerShell (servicio de reinicio NTDS-forzar).  
  
Las siguientes secciones explican restauración segura con todo detalle para cada escenario.  
  
### <a name="safe-restore-detailed-processing"></a>Procesamiento de restauración segura detallada  
El siguiente diagrama de flujo muestra restauración seguro de cómo se produce cuando se inicia un controlador de dominio virtual después de restaurar una instantánea mientras estaba cerrado.  
  
![Arquitectura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  Cuando se inicia la máquina virtual tras una restauración de la instantánea, tendrá nuevo Id. de generación de máquina virtual proporcionado por el host de hipervisor debido a la restauración de instantánea.  
  
2.  El nuevo ID de máquina virtual de generación de la máquina virtual se compara con el identificador de generación de la máquina virtual en la base de datos. Dado que no coinciden con los identificadores de dos, emplea medidas de seguridad de virtualización (consulta el paso 3 en la sección anterior). Una vez finalizada la restauración aplicar, se actualiza la máquina virtual-GenerationID establecer en su objeto de equipo de AD DS para que coincida con el nuevo Id. de proporcionar el host del hipervisor.  
  
3.  El invitado emplea medidas de seguridad de virtualización por:  
  
    1.  Invalidar el grupo de RID local.  
  
    2.  Para establecer un nuevo identificador de invocación de la base de datos del controlador de dominio.  
  
> [!NOTE]  
> Esta parte de la restauración segura coincide con el proceso de clonación. Aunque este proceso consiste en seguro restauración de un controlador de dominio virtual después de que se inicia después de una restauración de la instantánea, los mismos pasos producen durante el proceso de clonación.  
  
El siguiente diagrama muestra cómo evitar medidas de seguridad de virtualización que divergencia inducida reversión de USN cuando se restaura una instantánea en un controlador de dominio virtual en ejecución.  
  
![Arquitectura de DC virtualizada](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> La ilustración anterior se simplificó para explicar los conceptos.  
  
1.  Tiempo T1, el Administrador de hipervisor toma una instantánea de DC1 virtual. DC1 en este momento tiene un valor de USN (**highestCommittedUsn** en la práctica) de 100, valor de Id. de invocación (representado como ID en el diagrama anterior) de una (en la práctica sería un GUID). El valor de savedVMGID es la máquina virtual-GenerationID en el archivo DIT del controlador de dominio (almacenado en el objeto de equipo del controlador de dominio en un atributo denominado **msDS GenerationId**). El VMGID es el valor actual de la máquina virtual-GenerationId disponible en el controlador de máquina virtual. Este valor es proporcionado por el hipervisor.  
  
2.  En un momento posterior T2, se agregan los usuarios de 100 a este controlador de dominio (Ten en cuenta los usuarios como un ejemplo de actualizaciones que se han realizado en este controlador de dominio entre tiempo T1 y T2; estas actualizaciones en realidad pueden ser una combinación de creaciones de usuario, creaciones de grupo, actualizaciones de contraseña, las actualizaciones de atributos y así sucesivamente). En este ejemplo, cada actualización consume una USN único (aunque en la práctica, la creación de un usuario puede consumir USN más de una). Antes de confirmar que estas actualizaciones, DC1 comprueba si el valor de la máquina virtual GenerationID en su base de datos (savedVMGID) es el mismo que el valor actual disponible en el controlador (VMGID). Son iguales, como la reversión no ha ocurrido aún, por lo que las actualizaciones se confirman y USN mueve hasta 200, que indica que la próxima actualización puede utilizar USN 201. No hay ningún cambio en el Id. de invocación, savedVMGID o VMGID. Estas actualizaciones replicarán en DC2 en el siguiente ciclo de replicación. DC2 actualiza límite máximo (y **UptoDatenessVector**) representada aquí simplemente como DC1(A) @USN = 200. Es decir, DC2 tiene constancia de todas las actualizaciones de DC1 en el contexto de invocación de la A 200 USN.  
  
3.  Tiempo T3, se aplica la instantánea realizada en el momento T1 a DC1. DC1 se ha deshecho, por lo que su USN revierte a 100, que indica que podrías usar USN de 101 para asociar las actualizaciones subsiguientes. Sin embargo, en este punto, el valor de VMGID sería diferente en hipervisores que admiten GenerationID de máquina virtual.  
  
4.  Posteriormente, cuando DC1 realiza cualquier actualización, comprueba si el valor de la máquina virtual GenerationId que tiene en su base de datos (savedVMGID) es el mismo que el valor del controlador de máquina virtual (VMGID). En este caso, no es el mismo, por lo que DC1 deduce esto como indicativo de una operación de deshacer y desencadena medidas de seguridad de virtualización; en otras palabras, se restablece su Id. de invocación (ID = B) y se descarta el conjunto de RID (no se muestra en el diagrama anterior). A continuación, guarda el nuevo valor de VMGID en su base de datos y confirma esas actualizaciones (USN 101 - 250) en el contexto de la nueva B. InvocationId En el siguiente ciclo de replicación, DC2 sabe nada de DC1 en el contexto de invocación B, por lo solicita todo el contenido de DC1 asociado InvocationID B. Como resultado, las actualizaciones realizadas en DC1 posterior a la aplicación de la instantánea se convergen de forma segura. Además, el conjunto de actualizaciones que se realizaron en DC1 en T2 (que se perdieron en DC1 después de la restauración de la instantánea) replica en DC1 en la siguiente replicación programada porque tenían replique en DC2 (como se indica en la línea de puntos a DC1).  
  
Después de que el invitado emplea medidas de seguridad de virtualización, NTDS replica entrante de diferencias de objeto de Active Directory no autorizada de un controlador de dominio de partner. El vector de actualización del servicio de directorio de destino se actualiza según corresponda. A continuación, el invitado sincroniza SYSVOL:  
  
-   Si utiliza FRS, el invitado detiene el servicio NTFRS y establece el valor del registro de D2 BURFLAGS. A continuación, se inicia el servicio NTFRS, que no autorizada replica entrantes, volver a con existente sin modificar SYSVOL datos cuando sea posible.  
  
-   Si utiliza DFSR, el invitado se detiene el servicio DFSR y elimina los archivos de base de datos DFSR (ubicación predeterminada: %systemroot%\system volumen information\dfsr\\*<database GUID>*). A continuación, se inicia el servicio DFSR, que no autorizada replica entrantes, volver a con existente sin modificar SYSVOL datos cuando sea posible.  
  
> [!NOTE]  
> -   Si el hipervisor no proporciona un identificador de generación de la máquina virtual para fines de comparación, el hipervisor no es compatible con medidas de seguridad de virtualización y el invitado funcione como un controlador de dominio virtualizado que se ejecuta Windows Server 2008 R2 o versiones anteriores. El invitado implementa la protección de cuarentena de reversión de USN si hay un intento de inicio replicar con USN no avanzadas pasado el mayor USN último visto por el controlador de dominio asociado. Para obtener más información sobre la protección de cuarentena de reversión de USN, consulta [USN y reversión de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


