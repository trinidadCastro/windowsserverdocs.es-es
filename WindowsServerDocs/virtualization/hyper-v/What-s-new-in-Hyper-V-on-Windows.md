---
title: Novedades de Hyper-V en Windows Server 2016
description: Proporciona un resumen de las nuevas características de Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 8b7d9233b105f710d620b5142205fb2eadd0248a
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67141370"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>Novedades de Hyper-V en Windows Server

>Se aplica a: Windows Server 2019, Microsoft Hyper-V Server 2016, Windows Server 2016
  
Este artículo explica la funcionalidad nueva y modificada de Hyper-V en Windows Server 2019, Windows Server 2016 y Microsoft Hyper-V Server 2016. Para usar las nuevas características en las máquinas virtuales creadas con Windows Server 2012 R2 y mover o importar a un servidor que ejecuta Hyper-V en Windows Server 2019 o Windows Server 2016, deberá actualizar manualmente la versión de configuración de máquina virtual. Para obtener instrucciones, consulte [versión de actualización de virtual machine](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).  
  
Aquí es lo que se incluye en este artículo y si la funcionalidad es nueva o actualizada.  

## <a name="windows-server-version-1903"></a>Windows Server, versión 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>Agregar administrador de Hyper-V en instalaciones Server Core (actualizadas)

Como ya sabe, se recomienda usar la opción de instalación Server Core cuando se usa Windows Server, el canal semianual en producción. Sin embargo, Server Core de forma predeterminada se omite un número de herramientas de administración útiles. Puede agregar muchas de las más frecuente herramientas instalando la característica de compatibilidad de aplicaciones, pero todavía ha habido algunas herramientas que faltan.

Por lo tanto, según los comentarios recibidos, hemos agregado uno más herramientas para la característica de compatibilidad de aplicaciones en esta versión: Administrador de Hyper-V (virtmgmt.msc).

Para obtener más información, consulte [característica de compatibilidad de aplicaciones de Server Core](../../get-started-19/install-fod-19.md).

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>Seguridad: Mejoras de las máquinas virtuales blindadas (nuevas)

- **Mejoras de office de rama**

    Ahora puedes ejecutar máquinas virtuales blindadas en máquinas con conectividad intermitente para el Servicio de protección de host al aprovechar las nuevas características [HGS reserva](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) y [modo sin conexión](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode). HGS de reserva te permite configurar un segundo conjunto de direcciones URL para Hyper-V para probar si no puede conectar con el servidor HGS principal.

    El modo sin conexión te permite continuar iniciando las máquinas virtuales blindadas, incluso si no se puede conectar a HGS, siempre y cuando la máquina virtual se haya iniciado correctamente una vez y no haya cambiado la configuración de seguridad del host.

- **Mejoras de la solución de problemas**

    También hemos facilitado la [solución de problemas de las máquinas virtuales blindadas](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) habilitando el soporte con el modo de sesión mejorada de VMConnect y PowerShell Direct. Estas herramientas son especialmente útiles si perdiste la conectividad de red con la VM y necesitas actualizar su configuración para restaurar el acceso.

    No es necesario configurar estas características y están disponibles automáticamente cuando se coloca una VM blindada en un host de Hyper-V con Windows Server, versión 1803 o posterior.

- **Compatibilidad con Linux**

    Si ejecutas entornos de sistemas operativos mixtos, Windows Server 2019 admite ahora la ejecución de Ubuntu, Red Hat Enterprise Linux y SUSE Linux Enterprise Server dentro de las máquinas virtuales blindadas.

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>Compatible con el modo de espera conectado \(nuevo\)

Cuando se instala el rol Hyper-V en un equipo que usa el modelo de energía siempre activo o conectado (AOAC), el **conectado en espera** ahora está disponible el estado de energía.  
  
### <a name="discrete-device-assignment-new"></a>Asignación discreta de dispositivos \(nuevo\)

Esta característica le permite proporcionar un acceso directo y exclusivo de máquina virtual para algunos dispositivos de hardware PCIe. Usa un dispositivo de este modo, omite la pila de virtualización Hyper-V, lo que da como resultado un acceso más rápido. Para obtener más información sobre el hardware compatible, consulte "asignación discreta de dispositivos" en [requisitos del sistema para Hyper-V en Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Para obtener más información, incluida la forma de usar esta característica y las consideraciones, consulte la publicación "[asignación discreta de dispositivos: descripción y en segundo plano](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)" en el blog de virtualización.

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Compatibilidad con el cifrado de disco del sistema operativo en máquinas virtuales de generación 1 \(nuevo)

Ahora puede proteger el disco del sistema operativo con BitLocker drive encryption en máquinas virtuales de generación 1. Una nueva característica, el almacenamiento de claves, crea una unidad pequeña y dedicada para almacenar la clave de BitLocker de la unidad del sistema. Para ello, en lugar de usar un virtual confianza Platform Module (TPM), que solo está disponible en las máquinas virtuales de generación 2. Para descifrar el disco e iniciar la máquina virtual, el host de Hyper-V debe formar parte de un tejido protegido autorizado o tiene una clave privada de una de las protecciones de la máquina virtual. Almacenamiento de claves requiere una máquina virtual de la versión 8. Para obtener información sobre la versión de la máquina virtual, consulte [versión actualización máquina virtual de Hyper-V en Windows 10 o Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).  
  
### <a name="host-resource-protection-new"></a>Protección de recursos de host \(nuevo\)

Esta característica ayuda a impedir que una máquina virtual con más de su recurso compartido de recursos del sistema mediante la búsqueda de niveles excesivo de actividad. Esto puede ayudar a impedir que una actividad excesiva de la máquina virtual degradar el rendimiento del host o de otras máquinas virtuales. Cuando la supervisión detecta una máquina virtual con una actividad excesiva, la máquina virtual tiene menos recursos. Esta supervisión y la aplicación está desactivada de forma predeterminada. Usar Windows PowerShell para activar o desactivar. Para activarla, ejecute este comando:  
  
```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

Para obtener más información sobre este cmdlet, consulte [Set-VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor).

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>Agregar y quitar adaptadores de red y memoria frecuente \(nuevo\)

Ahora puede agregar o quitar un adaptador de red mientras se está ejecutando la máquina virtual, sin ningún tiempo de inactividad. Esto funciona para las máquinas virtuales de generación 2 que ejecutan sistemas operativos Windows o Linux.  
  
También puede ajustar la cantidad de memoria asignada a una máquina virtual mientras se está ejecutando, incluso si no ha habilitado la memoria dinámica. Esto funciona para la generación 1 y las máquinas virtuales de generación 2, que ejecutan Windows Server 2016 o Windows 10.  

### <a name="hyper-v-manager-improvements-updated"></a>Mejoras del Administrador de Hyper-V \(actualizado\) 
  
-   **Compatibilidad con credenciales alternativas** : ahora puede usar un conjunto diferente de credenciales de administrador de Hyper-V cuando se conecte a otro host remoto de Windows Server 2016 o Windows 10. También puede guardar estas credenciales para que resulte más fácil volver a iniciar sesión.  
  
-   **Administrar versiones anteriores** -con el Administrador de Hyper-V en Windows Server 2019, Windows Server 2016 y Windows 10, puede administrar los equipos que ejecutan Hyper-V en Windows Server 2012, Windows 8, Windows Server 2012 R2 y Windows 8.1.  
  
-   **Protocolo de administración actualizado** -Administrador de Hyper-V ahora se comunica con los hosts de Hyper-V remotos mediante el protocolo WS-MAN, que permite la autenticación CredSSP, Kerberos o NTLM. Cuando se usa CredSSP para conectarse a un host remoto de Hyper-V, puede hacer una migración en vivo sin habilitar la delegación restringida en Active Directory. La infraestructura basados en WS-MAN también facilita la habilitación de un host para la administración remota. WS-MAN se conecta a través del puerto 80, que está abierto de forma predeterminada.  
  
### <a name="integration-services-delivered-through-windows-update-updated"></a>Servicios de integración se entregan a través de Windows Update \(actualizado\) 

Las actualizaciones de servicios de integración para los invitados de Windows se distribuyen a través de Windows Update. Para los proveedores de servicios y los proveedores de hospedaje de nube privada, esto coloca el control de aplicación de actualizaciones en las manos de los inquilinos que son propietarios de las máquinas virtuales. Los inquilinos ahora pueden actualizar sus máquinas virtuales de Windows con todas las actualizaciones, incluidos los servicios de integración, con un único método. Para obtener más información acerca de los servicios de integración para invitados de Linux, consulte [Linux y FreeBSD máquinas virtuales Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
  
> [!IMPORTANT]  
> El archivo de imagen vmguest.iso ya no se necesita, por lo que no se incluye con Hyper-V en Windows Server 2016.  
  
### <a name="linux-secure-boot-new"></a>Arranque seguro de Linux \(nuevo\) 

Ahora es posible arrancar los sistemas operativos de Linux que se ejecutan en máquinas virtuales de generación 2 con la opción de arranque seguro habilitada. Ubuntu 14.04 y versiones posterior, SUSE Linux Enterprise Server 12 y versiones posterior, rojo Hat Enterprise Linux 7.0 y versiones posterior y CentOS 7.0 y versiones posteriores están habilitadas para el arranque seguro en hosts que ejecutan Windows Server 2016. Antes de arrancar la máquina virtual por primera vez, debe configurar la máquina virtual para usar la entidad emisora de certificados de Microsoft UEFI. Puede hacerlo desde el Administrador de Hyper-V, Virtual Machine Manager o una sesión de Windows Powershell con privilegios elevados. Para Windows PowerShell, ejecute este comando:  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
Para obtener más información acerca de las máquinas virtuales Linux en Hyper-V, consulte [Linux y FreeBSD máquinas virtuales Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Para obtener más información acerca del cmdlet, consulte [Set-VMFirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware).

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Más memoria y procesadores para hosts de Hyper-V y máquinas virtuales de generación 2 \(actualizado\)

A partir de la versión 8, puede usar máquinas virtuales de generación 2 significativamente más memoria y procesadores virtuales. Hosts también pueden configurarse con mucha más memoria y procesadores virtuales que admitían anteriormente. Estos cambios permiten nuevos escenarios como la ejecución de grandes bases de datos en memoria para el procesamiento de transacciones en línea (OLTP) y (DW) de almacenamiento de datos de comercio electrónico. Blog de Windows Server recientemente publicó los resultados de rendimiento de una máquina virtual con la versión 5.5 terabytes de memoria y 128 procesadores virtuales que se ejecuta la base de datos de 4 TB de memoria. Rendimiento era mayor del 95% del rendimiento de un servidor físico. Para obtener más información, consulte [rendimiento de máquina virtual a gran escala de Windows Server 2016 Hyper-V para el procesamiento de transacciones en memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Para obtener más información acerca de las versiones de la máquina virtual, consulte [versión actualización máquina virtual de Hyper-V en Windows 10 o Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obtener la lista completa de configuraciones máximas admitidas, consulte [planear la escalabilidad de Hyper-V en Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). 

### <a name="nested-virtualization-new"></a>Virtualización anidada \(nuevo\)

Esta característica permite usar una máquina virtual como host de Hyper-V y crear máquinas virtuales dentro de ese host virtualizado. Esto puede ser especialmente útil para entornos de desarrollo y prueba. Para usar la virtualización anidada, necesitará:  
  
-   Para ejecutar al menos 2019 de Windows Server, Windows Server 2016 o Windows 10 en el host físico de Hyper-V y el host virtualizado.  
  
-   Un procesador con Intel VT-x (virtualización anidada solo está disponible para los procesadores Intel en este momento).  
  
Para obtener información detallada e instrucciones, consulte [ejecutar Hyper-V en una máquina Virtual con la virtualización anidada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).  
  
### <a name="networking-features-new"></a>Las características de red \(nuevo\)

Nuevas características de red incluyen:  
  
-   **Remote dirigir el acceso a memoria (RDMA) y cambiar embedded teaming (SET)** . Puede configurar RDMA en enlazado a un conmutador virtual de Hyper-V, independientemente de si también se utiliza el conjunto de adaptadores de red. CONJUNTO proporciona un conmutador virtual con algunas de las mismas funcionalidades que la formación de equipos NIC. Para obtener más información, consulte [remoto acceso directo a memoria (RDMA) y Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).  
  
-   **Máquina virtual con varias colas (VMMQ)** . Mejora el rendimiento VMQ mediante la asignación de varias colas de hardware por máquina virtual.  La cola predeterminada se convierte en un conjunto de colas para una máquina virtual y se distribuye el tráfico entre las colas.  
  
-   **Calidad de servicio (QoS) para redes definidas por software**. Administra la clase predeterminada del tráfico a través del conmutador virtual en el ancho de banda de la clase predeterminada.  
  
Para obtener más información acerca de nuevas características de redes, consulte [cuáles son las novedades de redes](../../networking/What-s-New-in-Networking.md).  
  
### <a name="production-checkpoints-new"></a>Los puntos de control de producción \(nuevo\)

Los puntos de control de producción son imágenes de "punto-in-time" de una máquina virtual. Estos proporcionan una manera de aplicar un punto de control que cumple con las directivas de soporte técnico cuando una máquina virtual se ejecuta una carga de trabajo de producción. Los puntos de control de producción se basan en la tecnología de copia de seguridad dentro del invitado en lugar de un estado guardado. Para las máquinas virtuales de Windows, se usa el servicio de instantáneas de volumen (VSS). Las máquinas virtuales de Linux, se vacían los búferes del sistema de archivos para crear un punto de control que sea coherente con el sistema de archivos. Si prefiere usar puntos de control en función de los estados guardados, elija los puntos de control estándares en su lugar. Para obtener más información, consulte [elegir entre los puntos de control estándares o de producción en Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  
> [!IMPORTANT]  
> Nuevas máquinas virtuales usar puntos de control de producción como el valor predeterminado.  
  
### <a name="rolling-hyper-v-cluster-upgrade-new"></a>Actualización de clúster de Hyper-V gradual \(nuevo\)

Ahora puede agregar un nodo que ejecuta Windows Server 2019 o Windows Server 2016 en un clúster de Hyper-V con nodos que ejecutan Windows Server 2012 R2. Esto le permite actualizar el clúster sin tiempo de inactividad. El clúster se ejecuta en un nivel de la característica de Windows Server 2012 R2 hasta que actualice todos los nodos del clúster y actualice el nivel funcional del clúster con el cmdlet de Windows PowerShell [Update-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).  
  
> [!IMPORTANT]  
> Después de actualizar el nivel funcional del clúster, no se vuelve a Windows Server 2012 R2.  
  
Para un clúster de Hyper-V con un nivel funcional de Windows Server 2012 R2 con nodos que ejecutan Windows Server 2012 R2, Windows Server 2019 y Windows Server 2016, tenga en cuenta lo siguiente:  
  
-   Administrar el clúster, Hyper-V y máquinas virtuales desde un nodo que ejecuta Windows Server 2016 o Windows 10.  
  
-   Puede mover máquinas virtuales entre todos los nodos del clúster de Hyper-V.  
  
-   Para usar nuevas características de Hyper-V, todos los nodos deben ejecutar Windows Server 2016 o y debe actualizarse el nivel funcional del clúster.  
  
-   No se actualiza la versión de configuración de máquina virtual para máquinas virtuales existentes. Puede actualizar la versión de configuración solo después de actualizar el nivel funcional del clúster.  
  
-   Las máquinas virtuales que cree son compatibles con Windows Server 2012 R2, nivel de configuración de máquina virtual 5.  
  
Después de actualizar el nivel funcional del clúster:  
  
-   Puede habilitar nuevas características de Hyper-V.  
  
-   Para que las nuevas características de máquina virtual esté disponible, utilice el cmdlet Update-VmConfigurationVersion para actualizar manualmente el nivel de configuración de máquina virtual. Para obtener instrucciones, consulte [versión de actualización de virtual machine](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).    
-   No se puede agregar un nodo al clúster de Hyper-V que ejecuta Windows Server 2012 R2.  
  
> [!NOTE]  
> Hyper-V en Windows 10 no es compatible con clústeres de conmutación por error.  
  
Para obtener información detallada e instrucciones, consulte el [actualización gradual de clúster del sistema operativo](https://technet.microsoft.com/library/dn850430.aspx).  

### <a name="shared-virtual-hard-disks-updated"></a>Discos duros virtuales compartidos \(actualizado\)
Ahora puede redimensionar compartidos discos duros virtuales (vhdx archivos) utilizados para clústeres invitados, sin tiempo de inactividad. Discos duros virtuales compartidos puede ser crecidos o reducir mientras la máquina virtual está en línea. Clústeres invitados ahora también pueden proteger los discos duros virtuales compartidos mediante el uso de réplica de Hyper-V para la recuperación ante desastres.

Habilitar la replicación en la colección. Habilitar la replicación en una colección es **solo se expone a través de la interfaz WMI**. Consulte la documentación de [Msvm_CollectionReplicationService clase](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx) para obtener más detalles. **No se puede administrar la replicación de una colección a través de la interfaz de usuario o el cmdlet de PowerShell.** Las máquinas virtuales deben estar en los hosts que forman parte de un clúster de Hyper-V para tener acceso a las características que son específicas de una colección. Esto incluye el disco duro virtual compartido - compartidos discos duros virtuales en hosts independientes no son compatibles con Hyper-V Replica.

Siga las instrucciones para los VHD compartidos en [Virtual Hard Disk Sharing Overview](https://technet.microsoft.com/library/dn281956.aspx)y asegúrese de que los discos duros virtuales compartidos forman parte de un clúster invitado. 

Una colección con un disco duro virtual compartido pero no clúster asociado invitado no puede crear puntos de referencia para la colección (independientemente de si el disco duro virtual compartido se incluye en la creación de puntos de referencia o no). 

### <a name="virtual-machine-backupnew"></a>Copia de seguridad de máquina virtual\(nuevo\)

Si la copia de seguridad de una sola máquina virtual (independientemente de si el host está en clúster o no), no debe usar un grupo de máquinas virtuales.  Tampoco debe utilizar una colección de instantáneas. Grupos de máquinas virtuales y la recopilación de instantáneas están diseñados para usarse únicamente para la copia de seguridad de los clústeres invitados que usan vhdx compartido. En su lugar, debe realizar una instantánea mediante el [proveedor de WMI de Hyper-V v2](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx). Del mismo modo, no use la [proveedor WMI de clúster de conmutación por error](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx).

### <a name="shielded-virtual-machines-new"></a>Máquinas virtuales blindadas \(nuevo\)

Blindadas uso de máquinas virtuales con varias características para que sea más difícil para que los administradores de Hyper-V y el malware en el host inspeccionar, alterar o robar datos desde el estado de una máquina virtual blindada. Se cifran los datos y el estado, los administradores de Hyper-V no pueden ver los discos y la salida de vídeo y las máquinas virtuales pueden restringirse a ejecutar solo en hosts conocidos, correcto, determinado por un servidor de protección de Host. Para obtener más información, consulte [máquinas virtuales blindadas y tejido protegido](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).
  
> [!NOTE]  
> Las máquinas virtuales blindadas son compatibles con Hyper-V Replica. Para replicar una máquina virtual blindada, el host que desea replicar en debe estar autorizado para ejecutar esa máquina virtual blindada.  

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>Iniciar el orden de prioridad para las máquinas virtuales en clúster \(nuevo\)

Esta característica ofrece un mayor control sobre qué máquinas virtuales en clúster se inicia o reinicia de primero. Esto facilita la iniciar las máquinas virtuales que proporcionan servicios antes de máquinas virtuales que usan esos servicios. Definir conjuntos, colocar las máquinas virtuales en conjuntos y especificar las dependencias. Use los cmdlets de Windows PowerShell para administrar los conjuntos, como [New ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset), [Get ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset), y [agregar ClusterGroupSetDependency](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency).
.  
### <a name="storage-quality-of-service-qos-updated"></a>Calidad de servicio (QoS) de almacenamiento \(actualizado\)

Ahora puede crear directivas de Calidad de servicio de almacenamiento en un Servidor de archivos de escalabilidad horizontal y asignarlas a uno o varios discos virtuales en máquinas virtuales de Hyper-V. El rendimiento del almacenamiento se reajusta automáticamente para cumplir las directivas a medida que varía la carga de almacenamiento. Para obtener más información, consulte [calidad de servicio de almacenamiento](../../storage/storage-qos/storage-qos-overview.md).  
  
### <a name="virtual-machine-configuration-file-format-updated"></a>Formato de archivo de configuración de máquina virtual \(actualizado\)

Los archivos de configuración de máquina virtual usan un nuevo formato que permite leer y escribir datos de configuración más eficaces. El formato también hace daños en los datos menos probable que si se produce un error de almacenamiento. Archivos de datos de configuración de máquina virtual usan una extensión de nombre de archivo .vmcx y archivos de datos de estado en tiempo de ejecución utilizan una extensión de nombre de archivo .vmrs.  
  
> [!IMPORTANT]  
> La extensión .vmcx indica un archivo binario. No se admite la edición de archivos .vmcx o .vmrs.  
  
### <a name="virtual-machine-configuration-version-updated"></a>Versión de configuración de máquina virtual \(actualizado\)

La versión representa la compatibilidad de configuración de la máquina virtual, guarda el estado y los archivos de instantánea con la versión de Hyper-V. Las máquinas virtuales con la versión 5 son compatibles con Windows Server 2012 R2 y se pueden ejecutar en Windows Server 2012 R2 y Windows Server 2016. Las máquinas virtuales con las versiones que se introdujo en Windows Server 2016 y y Windows Server 2019 no se ejecuta en Hyper-V en Windows Server 2012 R2.   
  
Si mueve o importar una máquina virtual en un servidor que ejecuta Hyper-V en Windows Server 2016 o Windows Server 2019 desde Windows Server 2012 R2, no se actualiza automáticamente la configuración de la máquina virtual. Esto significa que puede devolver la máquina virtual a un servidor que ejecuta Windows Server 2012 R2. Sin embargo, esto también significa que no se puede usar las nuevas características de la máquina virtual hasta que actualice manualmente la versión de la configuración de máquina virtual.  
  
Para obtener instrucciones sobre la comprobación y actualizar la versión, consulte [versión de actualización de virtual machine](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). En este artículo también muestra la versión en el que se introdujeron algunas características.   
  
> [!IMPORTANT]  
> -   Después de actualizar la versión, no se puede mover la máquina virtual a un servidor que ejecuta Windows Server 2012 R2.  
> -   No se puede cambiar la configuración a una versión anterior.  
> -   El [actualización VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet está bloqueado en un clúster de Hyper-V cuando el nivel funcional del clúster es Windows Server 2012 R2.  

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Seguridad Virtualization-based para máquinas virtuales de generación 2 \(nuevo)

Seguridad basada en virtualización ofrece características como Device Guard y Credential Guard, que ofrece una mayor protección del sistema operativo contra ataques de malware. Seguridad basada en virtualización está disponible en máquinas de virtuales de generación 2 invitado a partir de la versión 8. Para obtener información sobre la versión de la máquina virtual, consulte [versión actualización máquina virtual de Hyper-V en Windows 10 o Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="windows-containers-new"></a>Contenedores de Windows \(nuevo\)

Contenedores de Windows permiten a varias aplicaciones aisladas ejecutar en un equipo. Son rápidas compilar y son altamente escalables y portátil. Existen dos tipos de contenedor en tiempo de ejecución, cada uno con distintos grados de aislamiento de la aplicación. Contenedores de Windows Server usar el aislamiento de proceso y espacio de nombres. Contenedores de Hyper-V usan una máquina virtual de peso ligero para cada contenedor.  
  
Las características principales incluyen:  
  
-   Compatibilidad con sitios web y aplicaciones mediante HTTPS  
  
-   Nano server puede hospedar tanto de contenedores de Hyper-V y Windows Server  
  
-   Capacidad de administrar datos mediante carpetas compartidas de contenedores  
  
-   Capacidad para restringir los recursos de contenedor  
  
Para obtener más información, incluidas las guías de inicio rápido, consulte el [documentación de contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/index).  
  
### <a name="windows-powershell-direct-new"></a>Windows PowerShell Direct \(nuevo\)

Esto le ofrece una manera de ejecutar comandos de Windows PowerShell en una máquina virtual desde el host. Windows PowerShell Direct se ejecuta entre el host y la máquina virtual. Esto significa no requiere los requisitos de firewall o de red y funciona independientemente de la configuración de administración remota.  
  
Windows PowerShell Direct es una alternativa a las herramientas existentes que usan los administradores de Hyper-V para conectarse a una máquina virtual en un host de Hyper-V:  
  
-   Herramientas de administración remota como PowerShell o Escritorio remoto.  
  
-   Conexión a máquina Virtual de Hyper-V (VMConnect)  
  
Estas herramientas funcionan bien, pero tienen ventajas y desventajas: VMConnect es confiable, pero puede ser difícil de automatizar. PowerShell remoto es eficaz, pero puede ser difícil de configurar y mantener. Pueden dejar más importancia a medida que crece la implementación de Hyper-V de estas ventajas e inconvenientes. Windows PowerShell Direct soluciona este problema al proporcionar una experiencia de scripting y automatización eficaz que es tan simple como utilizar VMConnect.
  
Para los requisitos e instrucciones, consulte [las máquinas virtuales de administración de Windows con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).  
