---
title: Virtualización de controladores de dominio con Hyper-V
description: Consideraciones que se deben tener en cuenta al virtualizar controladores de Dominio de Active Directory de Windows Server en Hyper-V
author: MicrosoftGuyJFlo
ms.author: joflore
ms.date: 04/19/2018
ms.topic: article
ms.openlocfilehash: ad40b5e5049c8b4f29dab4ffac8246a73e5b2fcd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956992"
---
# <a name="virtualizing-domain-controllers-using-hyper-v"></a>Virtualización de controladores de dominio con Hyper-V

> Se aplica a: Windows Server 2016

Este tema se actualizará para que las instrucciones se apliquen a Windows Server 2016. Windows Server 2012 incluye muchas mejoras para los controladores de dominio virtualizados (DC), incluidas las medidas de seguridad para evitar la reversión de USN en los controladores de dominio virtuales y la capacidad de clonar controladores de dominio virtuales. Para obtener más información acerca de estas mejoras, consulte [Introducción a la virtualización de Active Directory Domain Services (AD DS) (nivel 100)](../../introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md).

Hyper-V consolida diferentes roles de servidor en un solo equipo físico. En esta guía se describen los controladores de dominio en ejecución como sistemas operativos invitados de 32 bits o 64 bits.

## <a name="planning-to-virtualize-domain-controllers"></a>Planear la virtualización de controladores de dominio

En esta sección se describen los requisitos de hardware para el servidor de Hyper-v, cómo evitar puntos únicos de error, cómo seleccionar el tipo de configuración adecuado para los servidores de Hyper-V y las máquinas virtuales, y las decisiones de seguridad y rendimiento.

## <a name="hyper-v-requirements"></a>Requisitos de Hyper-V

Para instalar y usar el rol de Hyper-V, debe tener lo siguiente:

   - **Un procesador x64**
      - Hyper-V está disponible en las versiones basadas en x64 de Windows Server 2008 o posterior.
   - **Virtualización asistida por hardware**
      - Esta característica está disponible en procesadores que incluyen una opción de virtualización; concretamente, Intel Virtualization Technology (Intel VT) o AMD Virtualization (AMD-V).
   - **Protección de ejecución de datos de hardware (DEP)**
      - La DEP por hardware debe estar disponible y habilitada. Concretamente, debes habilitar el bit XD de Intel (bit ejecutar deshabilitado) o el bit NX de AMD (bit no ejecutar).

## <a name="avoid-creating-single-points-of-failure"></a>Evitar la creación de puntos de concentración de errores

Cuando planee la implementación del controlador de dominio virtual, debe intentar evitar la creación de puntos de concentración de errores. Para evitar la inclusión de posibles puntos de concentración de errores, implemente la redundancia del sistema. Por ejemplo, considere las siguientes recomendaciones, teniendo en cuenta la posibilidad de un aumento de los costos de administración:

1. Ejecute al menos dos controladores de dominio virtualizados por dominio en distintos host de virtualización, lo que reduce el riesgo de perder todos los controladores de dominio en caso de que uno de los host de virtualización genere un error.
2. Al igual que se recomienda para otras tecnologías, diversifique el hardware donde se ejecutan los controladores de dominio; por ejemplo, use distintas CPU, placas base, adaptadores de red u otro tipo de hardware. La diversificación del hardware limita la posibilidad de daños derivados de un funcionamiento incorrecto específico de la configuración de un proveedor, un controlador, o un componente o tipo único de hardware.
3. Si es posible, deberían ejecutarse controladores de dominio en el hardware ubicado en distintas regiones de todo el mundo. Esto ayuda a reducir la repercusión de un desastre o error que afecta a un sitio donde se hospedan controladores de dominio.
4. Mantenga controladores de dominio físicos en cada uno de los dominios. De esta forma, se evita el riesgo de que un funcionamiento incorrecto de la plataforma de virtualización afecte a todos los sistemas host que usan dicha plataforma.

## <a name="security-considerations"></a>Consideraciones de seguridad

El equipo host en el cual se ejecutan los controladores de dominio virtuales debe administrarse con el mismo cuidado que un controlador de dominio grabable, aunque este equipo sea solo un equipo unido a dominio o a un grupo de trabajo. Ésta es una consideración de seguridad importante. Un host administrado incorrectamente es vulnerable a un ataque de elevación de privilegios, que se produce cuando un usuario malintencionado obtiene acceso y privilegios de sistema no autorizados ni asignados de forma legítima. Un usuario malintencionado puede usar este tipo de ataque para poner en riesgo todas las máquinas virtuales, dominios y bosques que aloja el equipo.

Asegúrese de tener en cuenta las siguientes consideraciones de seguridad al planificar la virtualización de controladores de dominio:

   - El administrador local de un equipo que aloja controladores de dominio virtuales y grabables debe tener credenciales equivalentes a las del administrador de dominio predeterminado de todos los dominios y bosques a los que pertenecen estos controladores de dominio.
   - La configuración recomendada para evitar problemas de seguridad y rendimiento es un host que ejecuta una instalación Server Core de Windows Server 2008 o posterior, sin aplicaciones que no sean Hyper-V. Esta configuración limita el número de aplicaciones y servicios que se instalan en el servidor, lo que tiene como resultado un mayor rendimiento y menos aplicaciones y servicios que podrían aprovecharse malintencionadamente para atacar el equipo o la red. El efecto de este tipo de configuración se conoce como una superficie de ataque reducida. En una sucursal u otras ubicaciones que no se puedan asegurar de forma satisfactoria, se recomienda un controlador de dominio de sólo lectura (RODC). Si existe otra red de administración independiente, se recomienda que el host se conecte sólo a la red de administración.
   - Puede usar BitLocker con los controladores de dominio, ya que Windows Server 2016 puede usar la característica TPM virtual para proporcionar también el material de clave de invitado para desbloquear el volumen del sistema.
   - El [tejido protegido y las máquinas virtuales blindadas](/it-server/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) pueden proporcionar controles adicionales para proteger los controladores de dominio.

Para obtener información acerca de los RODC, consulte [Guía de planeamiento e implementación del controlador de dominio de solo lectura](../../deploy/rodc/read-only-domain-controller-updates.md).

Para obtener más información acerca de la protección de controladores de dominio, consulte la [Guía de procedimientos recomendados para proteger las instalaciones de Active Directory](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

## <a name="security-boundaries-for-different-host-and-guest-configurations"></a>Límites de seguridad para diferentes configuraciones de host e invitado

El uso de máquinas virtuales permite tener varias configuraciones diferentes de controladores de dominio. Se debe prestar mucha atención a la forma en que las máquinas virtuales influyen sobre los límites y confianzas de la topología Active Directory. En la siguiente tabla se describen las posibles configuraciones de un controlador de dominio y host de Active Directory (servidor Hyper-V) y sus equipos invitados (máquinas virtuales que se ejecutan en el servidor Hyper-V).

|Máquina|Configuración 1|Configuration 2|
|-------|---------------|---------------|
|administrador de flujos de trabajo|Equipo miembro o grupo de trabajo|Equipo miembro o grupo de trabajo|
|Invitado|Controlador de dominio|Equipo miembro o grupo de trabajo|

![Diagrama de límites de seguridad](media/virtualized-domain-controller-architecture/Dd363553.f44706fd-317e-4f0b-9578-4243f4db225f(WS.10).gif)

   - El administrador del equipo host tiene el mismo acceso que el administrador de dominio a los invitados del controlador de dominio grabable y debe ser tratado como tal. En el caso de un RODC invitado, el administrador del equipo host tiene el mismo acceso que un administrador local en el RODC invitado.
   - Un controlador de dominio en una máquina virtual tiene derechos administrativos en el host si este está unido al mismo dominio. Existe la posibilidad de que un usuario malintencionado ponga en peligro todas las máquinas virtuales si el usuario malintencionado gana primero el acceso a la máquina virtual 1. Esto se conoce como un vector de ataque. Si existen controladores de dominio para varios dominios o bosques, estos deben tener administración centralizada, en la cual el administrador de un dominio es de confianza en todos los dominios.
   - La posibilidad de ataque desde la máquina virtual 1 existe aunque la máquina virtual 1 esté instalada como un RODC. Aunque el administrador de un RODC no tenga derechos de administrador de dominio explícitos, el RODC se puede usar para enviar directivas al equipo host. Estas directivas deberían incluir scripts de inicio. Si esta operación se realiza satisfactoriamente, el equipo host puede estar en peligro y se puede usar para poner en peligro a las demás máquinas virtuales del equipo host.

## <a name="security-of-vhd-files"></a>Seguridad de archivos VHD

El archivo VHD de un controlador de dominio virtual es equivalente al disco duro físico de un controlador de dominio físico. Como tal, se debe proteger con el mismo cuidado que se aplica a la seguridad del disco duro de un controlador de dominio físico. Asegúrese de que solo los administradores confiables y de confianza tienen permitido el acceso a los archivos VHD del controlador de dominio.

## <a name="rodcs"></a>RODC

Una ventaja de los RODC es la posibilidad de colocarlos en ubicaciones donde la seguridad física no puede garantizarse, como por ejemplo en sucursales. Puede usar Cifrado de unidad BitLocker de Windows para proteger los archivos VHD (no los sistemas de archivos que contiene) para que no se vean comprometidos en el host a través del robo del disco físico.

## <a name="performance"></a>Rendimiento

Con la nueva arquitectura microkernel de 64 bits, el rendimiento de Hyper-V aumenta de forma significativa en comparación con las plataformas de virtualización anteriores. Para obtener el mejor rendimiento del host, el host debe ser una instalación Server Core de Windows Server 2008 o posterior y no debe tener roles de servidor que no sean los de Hyper-V instalados.

El rendimiento de las máquinas virtuales depende específicamente de la carga de trabajo . Para garantizar un rendimiento satisfactorio de Active Directory, pruebe las topologías siguientes. Evalúe la carga de trabajo actual durante un período de tiempo con una herramienta como el monitor de confiabilidad y rendimiento (Perfmon. msc) o el [Kit de herramientas de Microsoft Assessment and Planning (MAP)](https://go.microsoft.com/fwlink/?linkid=137077). La herramienta MAP también puede ser útil si desea extraer un inventario de todos los servidores y roles de servidor existentes en la red.

Para obtener una idea general del rendimiento de los controladores de dominio virtualizados, se realizaron las siguientes pruebas de rendimiento con la [herramienta de pruebas de rendimiento de Active Directory (ADTest.exe)](https://go.microsoft.com/fwlink/?linkid=137088).

Las pruebas del protocolo ligero de acceso a directorios (LDAP) se ejecutaron en un controlador de dominio físico con ADTest.exe y, a continuación, en una máquina virtual alojada en un servidor idéntico al controlador de dominio físico. Se usó un solo procesador lógico para el equipo físico y un solo procesador virtual para la máquina virtual para alcanzar con facilidad el 100 por 100 de uso de la CPU. En la tabla siguiente, la letra y el número entre paréntesis después de cada prueba indican la prueba específica en ADTest.exe. Como se muestra en estos datos, el rendimiento del controlador de dominio virtualizado era de 88 a 98 por ciento del rendimiento del controlador de dominio físico.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Medición</th>
<th>Prueba</th>
<th>virtuales físicas</th>
<th>Las máquinas</th>
<th>Delta</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Búsquedas/seg.</p></td>
<td><p>Buscar nombre común en el alcance base (L1)</p></td>
<td><p>11508</p></td>
<td><p>10276</p></td>
<td><p>-10,71%</p></td>
</tr>
<tr class="even">
<td><p>Búsquedas/seg.</p></td>
<td><p>Buscar un conjunto de atributos en el alcance base (L2)</p></td>
<td><p>10123</p></td>
<td><p>9005</p></td>
<td><p>-11,04%</p></td>
</tr>
<tr class="odd">
<td><p>Búsquedas/seg.</p></td>
<td><p>Buscar todos los atributos en el alcance base (L3)</p></td>
<td><p>1284</p></td>
<td><p>1242</p></td>
<td><p>-3,27%</p></td>
</tr>
<tr class="even">
<td><p>Búsquedas/seg.</p></td>
<td><p>Buscar nombre común en el alcance de subárbol (L6)</p></td>
<td><p>8613</p></td>
<td><p>7904</p></td>
<td><p>-8,23%</p></td>
</tr>
<tr class="odd">
<td><p>Enlaces correctos/seg.</p></td>
<td><p>Realizar enlaces rápidos (B1)</p></td>
<td><p>1438</p></td>
<td><p>1374</p></td>
<td><p>-4,45%</p></td>
</tr>
<tr class="even">
<td><p>Enlaces correctos/seg.</p></td>
<td><p>Realizar enlaces simples (B2)</p></td>
<td><p>611</p></td>
<td><p>550</p></td>
<td><p>-9,98%</p></td>
</tr>
<tr class="odd">
<td><p>Enlaces correctos/seg.</p></td>
<td><p>Usar NTLM para realizar enlaces (B5)</p></td>
<td><p>1068</p></td>
<td><p>1056</p></td>
<td><p>-1,12%</p></td>
</tr>
<tr class="even">
<td><p>lógicas/s</p></td>
<td><p>Escribir varios atributos (W2)</p></td>
<td><p>6467</p></td>
<td><p>5885</p></td>
<td><p>-9,00%</p></td>
</tr>
</tbody>
</table>

Para garantizar un rendimiento satisfactorio, se instalaron componentes de integración (IC) para permitir al sistema operativo invitado usar "Enlightenments" o controladores sintéticos con reconocimiento del hipervisor. Durante el proceso de instalación, puede que sea necesario usar controladores emulados de electrónica integrada de unidades (IDE) o de adaptador de red. En entornos de producción, debe reemplazar estos controladores emulados con controladores sintéticos para aumentar el rendimiento.

Al supervisar el rendimiento de las máquinas virtuales con Monitor de confiabilidad y rendimiento de Windows (Perfmon.msc), dentro de la máquina virtual la información de la CPU no será del todo exacta como resultado de la forma en que la CPU virtual está programada en el procesador físico. Cuando desee obtener información de la CPU para una máquina virtual que se ejecuta en un servidor Hyper-V, use los contadores del Procesador lógico del hipervisor Hyper-V en la partición de host.

Para obtener más información sobre la optimización del rendimiento de AD DS e Hyper-V, consulte [directrices para la optimización del rendimiento de Windows Server 2016](../../../../administration/performance-tuning/index.md).

Asimismo, no planifique usar un VHD de disco de diferenciación en una máquina virtual configurada como controlador de dominio, puesto que el VHD de disco de diferenciación puede reducir el rendimiento. Para obtener más información acerca de los tipos de discos de Hyper-V, incluidos los discos de diferenciación, consulte [Asistente para nuevo disco duro virtual](https://go.microsoft.com/fwlink/?linkid=137279).

Para obtener información adicional sobre AD DS en entornos de hospedaje virtual, consulte [aspectos que deben tenerse en cuenta al hospedar Active Directory controladores de dominio en entornos de hospedaje virtual](https://go.microsoft.com/fwlink/?linkid=141292) en Microsoft Knowledge base.

## <a name="deployment-considerations-for-virtualized-domain-controllers"></a>Consideraciones de implementación para controladores de dominio virtualizados

Hay varias prácticas de máquinas virtuales comunes que se deben evitar al implementar controladores de dominio y consideraciones especiales para la sincronización y el almacenamiento de la hora.

## <a name="virtualization-deployment-practices-to-avoid"></a>Prácticas de implementación de virtualización que hay que evitar

Las plataformas de virtualización, tales como Hyper-V, ofrecen una serie de características útiles que facilitan la administración, el mantenimiento, la realización de copias de seguridad y la migración de equipos. Sin embargo, no se deben usar las siguientes prácticas y características comunes de implementación para los controladores de dominio virtuales:

- Para garantizar la durabilidad de Active Directory escrituras, no implemente los archivos de base de datos de un controlador de dominio virtual (la base de datos de Active Directory (NTDS. DIT), registros y SYSVOL) en discos IDE virtuales. En su lugar, cree un segundo VHD conectado a una controladora SCSI virtual y asegúrese de que la base de datos, los registros y SYSVOL se colocan en el disco SCSI de la máquina virtual durante la instalación del controlador de dominio.
- No implemente discos duros virtuales (VHD) de disco de diferenciación en una máquina virtual que se vaya a configurar como controlador de dominio. Esto hace que sea mucho más fácil poder volver a una versión anterior, además de disminuir el rendimiento. Para obtener más información sobre los tipos de VHD, consulte [Asistente para nuevo disco duro virtual](https://go.microsoft.com/fwlink/?linkid=137279).
- No implemente nuevos dominios y bosques de Active Directory en una copia de un sistema operativo Windows Server que no se haya preparado primero mediante la herramienta de preparación del sistema (Sysprep). Para obtener más información acerca de la ejecución de Sysprep, consulte la [Introducción a Sysprep (preparación del sistema)](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

   > [!WARNING]
   > No se admite la ejecución de Sysprep en un controlador de dominio.

- Para evitar una situación de posible reversión en el número de secuencias actualizadas (USN), no se deben usar copias de un archivo de VHD que represente un controlador de dominio ya implementado para implementar más controladores de dominio. Para obtener más información acerca de la reversión de USN, vea [USN y reversión de USN](#usn-and-usn-rollback).
   - Windows Server 2012 y versiones más recientes permiten a los administradores clonar imágenes de controlador de dominio si están preparadas correctamente cuando desean implementar controladores de dominio adicionales.
- No use la característica de exportación de Hyper-V para exportar máquinas virtuales que ejecutan un controlador de dominio.
  - Con Windows Server 2012 y versiones más recientes, una exportación e importación de un invitado virtual de controlador de dominio se administra como una restauración no autoritativa, ya que detecta un cambio del identificador de generación y no está configurado para la clonación.
  - Asegúrese de que ya no usa el invitado que exportó.
    - Puede usar la replicación de Hyper-V para mantener una segunda copia inactiva de un controlador de dominio. Si inicia la imagen replicada, también debe realizar la limpieza adecuada, por la misma razón que no usar el origen después de exportar una imagen de invitado de DC.

## <a name="physical-to-virtual-migration"></a>Migración de máquinas físicas a virtuales

System Center Virtual Machine Manager (VMM) 2008 permite una administración unificada de máquinas físicas y virtuales. Además, permite la capacidad de migrar una máquina física a una virtual. Este proceso se conoce como conversión de una máquina física a una virtual (conversión P2V). Durante el proceso de conversión FAV, la nueva máquina virtual y el controlador de dominio físico que se va a migrar no se deben ejecutar al mismo tiempo, para evitar una situación de reversión de USN, tal y como se describe en [USN y reversión de USN](#usn-and-usn-rollback).

Se debe realizar la conversión P2V en modo sin conexión para que los datos del directorio sean coherentes cuando el controlador de dominio se vuelva a activar. La opción de modo sin conexión se ofrece y recomienda en el Asistente para convertir el servidor físico. Para obtener una descripción de la diferencia entre el modo en línea y el modo sin conexión, consulte [FAV: conversión de equipos físicos a virtual machines en VMM](https://go.microsoft.com/fwlink/?linkid=155072). Durante la conversión P2V, la máquina virtual no debe estar conectada a la red. El adaptador de red de la máquina virtual debe estar habilitado solo después de que se complete y compruebe el proceso de conversión FAV. En este punto, la máquina de origen física estará desactivada. No conecte nuevamente la máquina de origen física a la red hasta que reformatee el disco duro.

> [!NOTE]
> Hay opciones más seguras para crear nuevos controladores de seguridad virtuales que no ejecutan los riesgos de la creación de una reversión de USN. Puede configurar un nuevo controlador de dominio virtual mediante promoción normal, promoción de instalación desde medios (IfM) y también mediante la clonación de controladores de dominio, si ya tiene al menos un controlador de dominio virtual.
Esto también ayuda a evitar problemas relacionados con el hardware o la plataforma. los invitados virtuales convertidos en FAV pueden encontrarse.

> [!WARNING]
> Para evitar problemas con la replicación de Active Directory, debe asegurarse de que solamente exista en la red concreta y en un momento específico una instancia (física o virtual) de un controlador de dominio determinado.
> Puede reducir la probabilidad de que el clon anterior sea un problema:
>
> - Cuando se esté ejecutando el nuevo controlador de dominio virtual, cambie la contraseña de la cuenta de equipo dos veces con: Netdom resetpwd/Server: <controlador de dominio>...
> - Exporte e importe el nuevo invitado virtual para forzarlo convirtiéndose en un nuevo identificador de generación y, por lo tanto, en un identificador de invocación de base de datos.

## <a name="using-p2v-migration-to-create-test-environments"></a>Usar la migración P2V para crear entornos de prueba

Se puede usar la migración P2V por mediante VMM para crear entornos de prueba. Se pueden migrar controladores de dominio de producción desde máquinas físicas hasta máquinas virtuales para crear un entorno de prueba sin desactivar de manera permanente los controladores de dominio de producción. Sin embargo, el entorno de prueba debe estar en una red diferente del entorno de producción si van a existir dos instancias del mismo controlador de dominio. Es necesario tener mucho cuidado al crear entornos de prueba con la migración P2V para evitar reversiones de USN que puedan afectar a los entornos de producción y prueba. A continuación se muestra un método que se puede usar para la creación de entornos de prueba con P2V.

Se migra un controlador de dominio en producción desde cada dominio a una máquina virtual de prueba que usa P2V, según las pautas establecidas en la sección Migración de máquinas físicas a virtuales. Las máquinas físicas de producción y las máquinas virtuales de prueba deben estar en redes diferentes cuando se vuelvan a conectar. Para evitar reversiones de USN en el entorno de prueba, todos los controladores de dominio que se van a migrar desde máquinas físicas a máquinas virtuales deben estar desconectados. (Puede hacerlo deteniendo el servicio NTDS o reiniciando el equipo en Modo de restauración de servicios de directorio (DSRM)). Una vez que los controladores de dominio están sin conexión, no se deben introducir nuevas actualizaciones en el entorno. Los equipos deben permanecer desconectados durante la migración de P2V; ninguno de los equipos debe volver a conectarse hasta que todos los equipos se hayan migrado completamente. Para obtener más información acerca de la reversión de USN, consulte USN y reversión de USN.

Los controladores de dominio de prueba posteriores deben promocionarse como réplicas en el entorno de prueba.

## <a name="time-service"></a>Servicio de hora

En el caso de las máquinas virtuales configuradas como controladores de dominio, se recomienda deshabilitar la sincronización de hora entre el sistema host y el sistema operativo invitado que actúa como controlador de dominio. Esto permite que el controlador de dominio invitado sincronice la hora desde la jerarquía de dominios.

Para deshabilitar el proveedor de sincronización de hora de Hyper-V, apague la máquina virtual y desactive la casilla sincronización de hora en Integration Services.

> [!NOTE]
> Esta guía se ha actualizado recientemente para reflejar la recomendación actual de sincronizar la hora del controlador de dominio invitado solo desde la jerarquía de dominios, en lugar de la recomendación anterior para deshabilitar parcialmente la sincronización de hora entre el sistema host y el controlador de dominio invitado.

## <a name="storage"></a>Storage

Para optimizar el rendimiento de la máquina virtual del controlador de dominio y garantizar la durabilidad de las escrituras de Active Directory, siga estas recomendaciones para almacenar los archivos de sistema operativo, Active Directory y VHD:

- **Almacenamiento de invitados**. Almacene el archivo de base de datos de Active Directory (Ntds.dit), los archivos de registro y los archivos SYSVOL en un disco virtual separado de los archivos del sistema operativo. Cree un segundo VHD conectado a una controladora SCSI virtual y almacene la base de datos, los registros y SYSVOL en el disco SCSI virtual de la máquina virtual. Los discos SCSI virtuales proporcionan un mayor rendimiento en comparación con el IDE virtual y admiten el acceso a la unidad forzada (FUA). FUA garantiza que el sistema operativo escribe y Lee los datos directamente desde el medio que omite cualquiera de los mecanismos de almacenamiento en caché.

  > [!NOTE]
  > Si tiene previsto usar BitLocker para el invitado de controlador de dominio virtual, debe asegurarse de que los volúmenes adicionales estén configurados para el "desbloqueo automático".
  > Puede encontrar más información sobre cómo configurar el desbloqueo automático en [enable-BitLockerAutoUnlock](/powershell/module/bitlocker/enable-bitlockerautounlock)

- **Almacenamiento host de archivos VHD**. Recomendaciones: recomendaciones de almacenamiento de host direcciones de almacenamiento de archivos VHD. Para lograr un rendimiento máximo, no almacene los archivos VHD en un disco que se use con frecuencia para otros servicios o aplicaciones, como el disco del sistema en el que está instalado el sistema operativo Windows del host. Almacene cada archivo VHD en una partición distinta del sistema operativo del host y en cualquier otro archivo VHD. La configuración ideal sería almacenar cada archivo VHD en una unidad física distinta.

  El sistema del disco físico del host también debe cumplir **al menos uno** de los siguientes criterios para cumplir los requisitos de integridad de los datos de la carga de trabajo virtualizada:

   - El sistema utiliza discos de clase de servidor (SCSI, Canal de fibra).
   - El sistema se asegura de que los discos estén conectados a un adaptador de bus host (HBA) con copia de seguridad en la batería.
   - El sistema utiliza un controlador de almacenamiento (por ejemplo, un sistema RAID) como dispositivo de almacenamiento.
   - El sistema garantiza que la alimentación del disco esté protegida por una fuente de alimentación ininterrumpida (SAI).
   - El sistema se asegura de que la característica de almacenamiento en caché de escritura del disco esté deshabilitada.

- **VHD fijo frente a discos de paso a través**. Existen muchas maneras de configurar el almacenamiento para máquinas virtuales. Cuando se usan archivos VHD, los VHD de tamaño fijo son más eficientes que los VHD dinámicos, porque la memoria para los VHD de tamaño fijo se asigna cuando se crean dichos discos. Los discos de paso a través, que las máquinas virtuales pueden usar para tener acceso a medios de almacenamiento físicos, consiguen incluso un mayor rendimiento. Este tipo de discos son esencialmente discos físicos o números de unidad lógica (LUN) conectados a una máquina virtual. Los discos de paso a través no son compatibles con la característica de instantánea. Por lo tanto, los discos de paso a través son la configuración de disco duro preferida debido a que el uso de instantáneas con controladores de dominio no es recomendable.

Para reducir la posibilidad de que se dañen los datos de Active Directory, use controladores SCSI virtuales:

   - Use unidades físicas SCSI (en contraposición a las unidades IDE/ATA) en los servidores Hyper-V que hospeden controladores de dominio virtuales. Si no se pueden usar unidades SCSI, asegúrese de que la memoria caché de escritura esté deshabilitada en las unidades ATA/IDE que hospedan controladores de dominio virtuales. Para obtener más información, consulte ID. de [evento 1539 – integridad](https://go.microsoft.com/fwlink/?linkid=162419)de la base de datos.
   - Para garantizar la durabilidad de las escrituras Active Directorydas, los Active Directory base de datos, los registros y SYSVOL deben colocarse en un disco SCSI virtual. Los discos SCSI virtuales admiten el acceso a la unidad forzada (FUA). FUA garantiza que el sistema operativo escribe y Lee los datos directamente desde el medio que omite cualquiera de los mecanismos de almacenamiento en caché.

## <a name="operational-considerations-for-virtualized-domain-controllers"></a>Consideraciones operativas de controladores de dominio virtualizados

Los controladores de dominio que se ejecutan en máquinas virtuales tienen restricciones operativas que no se aplican a los controladores de dominio que se ejecutan en equipos físicos. Cuando use un controlador de dominio virtualizado, hay algunas prácticas y características del software de virtualización que no debe usar:

   - No pause, detenga ni almacene el estado guardado de un controlador de dominio en una máquina virtual por períodos de tiempo superiores a la duración del marcador de exclusión del bosque y, a continuación, reanude a partir del estado pausado o almacenado. Si hace esto, puede afectar a la replicación. Para obtener información sobre cómo determinar la duración del desecho para el bosque, vea [determinar la duración del desecho del bosque](https://go.microsoft.com/fwlink/?linkid=137177).
   - No copie ni clone discos duros virtuales (VHD). Incluso con las medidas de seguridad aplicadas a la máquina virtual invitada, se pueden copiar los discos duros virtuales individuales y provocar la reversión de USN.
   - No tome ni use una instantánea de un controlador de dominio virtual. Técnicamente, es compatible con Windows Server 2012 y versiones más recientes, no es un sustituto de una buena estrategia de copia de seguridad. Hay algunas razones para tomar instantáneas de DC o restaurar las instantáneas.
   - No use un disco VHD de diferenciación en una máquina virtual que esté configurada como un controlador de dominio, ya que facilita demasiado la reversión a una versión anterior y, además, disminuye el rendimiento.
   - No use la característica Exportar en una máquina virtual que ejecute un controlador de dominio.
   - No restaure un controlador de dominio ni intente revertir el contenido de una base de datos de Active Directory por ningún otro medio que no sea usar una copia de seguridad compatible. Para obtener más información, vea [consideraciones sobre copias de seguridad y restauración para controladores de dominio virtualizados](#backup-and-restore-practices-to-avoid).

Todas estas recomendaciones se efectúan a fin de ayudarlo a evitar la posibilidad de una reversión del número de secuencia de actualización (USN). Para obtener más información acerca de la reversión de USN, vea USN y reversión de USN.

## <a name="backup-and-restore-considerations-for-virtualized-domain-controllers"></a>Consideraciones sobre la copia de seguridad y restauración de controladores de dominio virtualizados

Realizar copias de seguridad de los controladores de dominio es un requisito crítico en cualquier entorno. Las copias de seguridad protegen de posibles pérdidas de datos en caso de se produzca un error en el controlador de dominio o un error administrativo. Si se produjera, sería necesario revertir el estado del controlador de dominio a un punto en el tiempo anterior al error. El método admitido para la restauración de un controlador de dominio a un estado correcto es usar una aplicación de copia de seguridad compatible con Active Directory, como Copias de seguridad de Windows Server, para restaurar una copia de seguridad de estado del sistema que se originó desde la instalación actual del controlador de dominio. Para obtener más información sobre el uso de Copias de seguridad de Windows Server con Active Directory Domain Services (AD DS), consulte la [Guía paso a paso de AD DS de copia de seguridad y recuperación](https://go.microsoft.com/fwlink/?LinkId=138501).

Con la tecnología de máquina virtual, algunos requisitos de las operaciones de restauración de Active Directory tienen una especial relevancia. Por ejemplo, si se restaura un controlador de dominio usando una copia del archivo VHD (disco duro virtual), se omite el paso crítico de actualizar la versión de la base de datos de un controlador de dominio después de restaurarla. La replicación continuará con números de seguimiento inapropiados, lo que dará como resultado una base de datos incoherente entre réplicas del controlador de dominio. En la mayoría de casos, el sistema de replicación no detecta este problema y no se informa de ningún error, a pesar de las incoherencias entre controladores de dominio.

Existe una manera compatible de realizar copias de seguridad y restauración de un controlador de dominio virtualizado:

1. Ejecutar Copias de seguridad de Windows Server en el sistema operativo invitado.

Con Windows Server 2012 y más recientes hosts e invitados de Hyper-V, puede realizar copias de seguridad admitidas de controladores de dominio mediante instantáneas, exportación e importación de máquinas virtuales invitadas y también replicación de Hyper-V. Sin embargo, no es una buena opción para crear un historial de copias de seguridad adecuado, con la ligera excepción de la exportación de la máquina virtual invitada.

Con Windows Server 2016 Hyper-V existe compatibilidad con "instantáneas de producción" donde el servidor de Hyper-V desencadena una copia de seguridad basada en VSS del invitado y, cuando el invitado se realiza con la instantánea, el host captura los VHD y los almacena en la ubicación de copia de seguridad.

Aunque esto funciona con Windows Server 2012 y versiones más recientes, existe una incompatibilidad con BitLocker:

- Al realizar una captura de complementos de VSS, AD desea realizar una tarea posterior a la instantánea para marcar la base de datos como procedente de una copia de seguridad o, en el caso de preparar un origen IFM para RODC, quitar las credenciales de la base de datos.
- Cuando Hyper-V monta el volumen instantánea para esta tarea, no hay ninguna instalación que desbloquee el volumen para el acceso sin cifrar. Por lo tanto, el motor de base de datos de AD no puede obtener acceso a la base de datos y, finalmente, la instantánea no

> [!NOTE]
> El proyecto de máquina virtual blindada mencionado anteriormente tiene una copia de seguridad controlada por host de Hyper-V como no objetivo para la protección de datos máxima de la máquina virtual invitada.

## <a name="backup-and-restore-practices-to-avoid"></a>Procedimientos que se deben evitar al realizar copias de seguridad y restauraciones

Tal como se ha mencionado anteriormente, los controladores de dominio que se ejecutan en máquinas virtuales tienen restricciones que no se aplican a los controladores de dominio que se ejecutan en máquinas físicas. Cuando se realiza la copia de seguridad o la restauración de un controlador de dominio virtual, existen algunos procedimientos y ciertas características del software de virtualización que no se deberían usar:

   - No copie ni clone archivos VHD de controladores de dominio en lugar de realizar copias de seguridad periódicas. Si se copia o clona el archivo VHD, se vuelve obsoleto. Después, si el disco duro virtual se inicia en modo normal, se producirá una reversión de USN. Debería realizar operaciones de copia de seguridad adecuadas y que sean compatibles con los Servicios de dominio de Active Directory (AD DS), como las de la característica Copias de seguridad de Windows Server.
   - No use la característica Instantánea como una copia de seguridad para restaurar una máquina virtual que se configuró como controlador de dominio. Se producirán problemas con la replicación cuando revierta la máquina virtual a un estado anterior con Windows Server 2008 R2 y versiones anteriores. Para obtener más información, vea [USN y reversión de USN](#usn-and-usn-rollback). A pesar de que usar una instantánea para restaurar un controlador de dominio de solo lectura (RODC) no provocará problemas de replicación, no se recomienda usar este método de restauración.

## <a name="restoring-a-virtual-domain-controller"></a>Restaurar un controlador de dominio virtual

Para restaurar un controlador de dominio cuando se produce un error, debe realizar de forma periódica copias de seguridad del estado del sistema. El estado del sistema incluye archivos de datos y registro de Active Directory, el Registro, el volumen del sistema (carpeta SYSVOL) y diversos elementos del sistema operativo. Este requisito es el mismo para los controladores de dominio físicos y virtuales. Los procedimientos de restauración del estado del sistema que realizan las aplicaciones de copia de seguridad compatibles con Active Directory están diseñados para garantizar la coherencia de las bases de datos de Active Directory locales y replicadas después de un proceso de restauración, incluida la notificación a los asociados de replicación de los restablecimientos de los indicadores de invocación. Sin embargo, el uso de entornos de hospedaje virtuales y de aplicaciones de creación de imágenes de disco o de sistema operativo permite a los administradores omitir las comprobaciones y validaciones que normalmente se producen cuando se restaura el estado del sistema de un controlador de dominio.

Cuando sucede un error en una máquina virtual de controlador de dominio y no se produce una reversión del número de secuencia de actualización (USN), existen dos situaciones admitidas para la restauración de la máquina virtual:

   - Si existe una copia de seguridad válida de los datos del estado del sistema anterior al error, puede restaurar el estado del sistema mediante la opción de restauración de la utilidad de copia de seguridad que usó para crear la copia de seguridad. La copia de seguridad de los datos de estado del sistema se debe haber creado con una utilidad de copia de seguridad compatible con Active Directory dentro del intervalo de duración del marcador de exclusión que, de forma predeterminada, no es superior a 180 días. Debería realizar una copia de seguridad de los controladores de dominio al menos una vez cada mitad de la duración del marcador de exclusión. Para obtener instrucciones sobre cómo determinar la duración específica de los marcadores de exclusión para el bosque, vea [determinar la duración del desecho del bosque](https://go.microsoft.com/fwlink/?linkid=137177).
   - Si hay disponible una copia de trabajo del archivo VHD, pero no hay disponible ninguna copia de seguridad del estado del sistema, puede quitar la máquina virtual existente. Restaure la máquina virtual existente con una copia anterior del VHD, pero asegúrese de iniciarlo en Modo de restauración de servicios de directorio (DSRM) y configurar el Registro correctamente, tal como se describe en la siguiente sección. A continuación, reinicie el controlador de dominio en modo normal.

Use el proceso que se muestra en la siguiente ilustración para determinar la mejor forma de restaurar el controlador de dominio virtualizado.

![Diagrama de cómo restaurar el controlador de dominio virtualizado](media/virtualized-domain-controller-architecture/Dd363553.85c97481-7b95-4705-92a7-006e48bc29d0(WS.10).gif)

Para los RODC, el proceso de restauración y las decisiones son más fáciles.

![Diagrama cómo restaurar el controlador de dominio de solo lectura](media/virtualized-domain-controller-architecture/Dd363553.4c5c5eda-df95-4c6b-84e0-d84661434e5d(WS.10).gif)

## <a name="restoring-the-system-state-backup-of-a-virtual-domain-controller"></a>Restaurar la copia de seguridad del estado del sistema de un controlador de dominio virtual

Si existe una copia de seguridad válida del estado del sistema para la máquina virtual del controlador de dominio, puede restaurar de forma segura la copia de seguridad siguiendo el proceso de restauración que prescribe la herramienta de copia de seguridad que se usó para realizar una copia de seguridad del archivo VHD.

> [!IMPORTANT]
> Para restaurar correctamente el controlador de dominio, debe iniciarlo en DSRM. No debe permitir que el controlador de dominio se inicie en modo normal. Si se pierde la oportunidad de escribir DSRM durante el inicio del sistema, desactive la máquina virtual del controlador de dominio antes de que se pueda iniciar completamente en modo normal. Es importante iniciar el controlador de dominio en DSRM porque iniciar un controlador de dominio en modo normal incrementa sus USN, incluso aunque el controlador de dominio esté desconectado de la red. Para obtener más información acerca de la reversión de USN, vea USN y reversión de USN.

## <a name="to-restore-the-system-state-backup-of-a-virtual-domain-controller"></a>Para restaurar la copia de seguridad del estado del sistema de un controlador de dominio virtual

1. Inicie la máquina virtual del controlador de dominio y presione F5 para tener acceso a la pantalla del administrador de arranque de Windows. Si se le solicita que escriba credenciales de conexión, haga clic inmediatamente en el botón **Pausa** de la máquina virtual para que no continúe iniciándose. A continuación, escriba las credenciales de conexión y haga clic en el botón **Reproducir** de la máquina virtual. Haga clic dentro de la ventana de la máquina virtual y, a continuación, presione F5.

   Si no ve la pantalla del Administrador de arranque de Windows y el controlador de dominio empieza a iniciarse en el modo normal, apague la máquina virtual para impedir que se complete el inicio. Repita este paso todas las veces que sea necesario hasta que pueda obtener acceso a la pantalla del Administrador de arranque de Windows. No se puede tener acceso al DSRM desde el menú Recuperación de errores de Windows. Por lo tanto, si aparece el menú Recuperación de errores de Windows, apague la máquina virtual y vuelva a intentarlo.

2. En la pantalla del Administrador de arranque de Windows, presione F8 para obtener acceso a las opciones de arranque avanzadas.
3. En la pantalla **Opciones de arranque avanzadas**, seleccione **Modo de restauración de servicios de directorio** y, a continuación, presione ENTRAR. Esto inicia el controlador de dominio en DSRM.
4. Use el método de restauración adecuado para la herramienta que se usó para crear la copia de seguridad del estado del sistema. Si ha usado Copias de seguridad de Windows Server, consulte [realizar una restauración no autoritativa de AD DS](https://go.microsoft.com/fwlink/?linkid=132637).

## <a name="restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available"></a>Restaurar un controlador de dominio virtual cuando no hay disponible una copia de seguridad de los datos del estado del sistema adecuado

Si no dispone de una copia de seguridad de datos del estado del sistema que sea anterior al error de la máquina virtual, puede usar un archivo VHD previo para restaurar un controlador de dominio que se ejecute en una máquina virtual. Si es posible, realice una copia del archivo VHD de forma que, si se experimenta algún problema durante el proceso o no se realiza alguno de los pasos, puede volver a intentarlo con el archivo VHD copiado.

> [!IMPORTANT]
> - No debe considerar el uso del siguiente procedimiento para reemplazar la realización de copias de seguridad planeadas y programadas asiduamente.
> - **Microsoft no admite las restauraciones realizadas mediante el siguiente procedimiento, que debería usarse exclusivamente cuando no exista otra alternativa.**
> - No use este procedimiento si alguna máquina virtual ha iniciado en modo normal la copia del VHD que está a punto de restaurar.

## <a name="to-restore-a-previous-version-of-a-virtual-domain-controller-vhd-without-system-state-data-backup"></a>Para restaurar una versión anterior de un VHD de controlador de dominio virtual sin una copia de seguridad de datos del estado del sistema

1. Use el VHD anterior para iniciar el controlador de dominio virtual en DSRM, tal como se describe en la sección anterior. No permita que el controlador de dominio se inicie en modo normal. Si se pierde la pantalla del Administrador de arranque de Windows y el controlador de dominio empieza a iniciarse en el modo normal, apague la máquina virtual para impedir que se complete el inicio. Vea la sección anterior para obtener instrucciones detalladas para iniciar el DSRM.
2. Abra el Editor del Registro. Para abrir el Editor del Registro, haga clic en **Inicio**, en **Ejecutar**, escriba **regedit** y, a continuación, haga clic en Aceptar. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**. En el editor del registro, expanda la siguiente ruta de acceso: **HKEY \_ local \_ Machine \\ System \\ CurrentControlSet \\ Services \\ NTDS \\ Parameters**. Busque un valor llamado **DSA Previous Restore Count**. Si el valor está, anote el valor de configuración. Si el valor no está, el valor es igual al valor predeterminado, que es cero. No agregue un valor si no ve uno allí.
3. Haga clic con el botón secundario en la clave **Parameters**, haga clic en **Nuevo** y, a continuación, haga clic en **Valor de DWORD (32 bits)**.
4. Escriba el nuevo nombre **Base de datos restaurada desde una copia de seguridad** y, a continuación, presione ENTRAR.
5. Haga doble clic en el valor que acaba de crear para abrir el cuadro de diálogo **Editar valor de DWORD (32 bits)** y, a continuación, escriba **1** en el cuadro **Información del valor**. La opción **base de datos restaurada a partir de la entrada de copia de seguridad** está disponible en los controladores de dominio que ejecutan Windows 2000 Server con Service Pack 4 (SP4), windows Server 2003 con las actualizaciones que se incluyen en [detección y recuperación de una reversión de USN en Windows Server 2003, Windows Server 2008 y Windows Server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) en Microsoft Knowledge Base instalado y Windows Server 2008.
6. Reinicie el controlador de dominio en modo normal.
7. Cuando se reinicie el controlador de dominio, abra el Visor de eventos. Para abrir el Visor de eventos, haga clic en **Inicio**, en **Panel de control**, haga doble clic en **Herramientas administrativas** y, a continuación, haga doble clic en **Visor de eventos**.
8. Expanda **Registros de servicios y aplicaciones** y, a continuación, haga clic en el registro **Servicios de directorio**. Asegúrese de que aparezcan eventos en el panel de detalles.
9. Haga clic con el botón secundario en el registro **Servicios de directorio** y, a continuación, haga clic en **Buscar**. En **Buscar**, escriba **1109** y haga clic en **Buscar siguiente**.
10. Debería ver al menos una entrada con identificador de evento 1109. Si no ve esta entrada, continúe con el siguiente paso. En caso contrario, haga doble clic en la entrada y, a continuación, revise el texto que confirma que se realizó la actualización al identificador de invocación:

    ```
    Active Directory has been restored from backup media, or has been configured to host an application partition.
    The invocationID attribute for this directory server has been changed.
    The highest update sequence number at the time the backup was created is <time>

    InvocationID attribute (old value):<Previous InvocationID value>
    InvocationID attribute (new value):<New InvocationID value>
    Update sequence number:<USN>

    The InvocationID is changed when a directory server is restored from backup media or is configured to host a writeable application directory partition.
    ```

11. Cierre el Visor de eventos.
12. Use el Editor del Registro para comprobar que el valor en **DSA Previous Restore Count** es igual al del valor anterior más uno. Si este no es el valor correcto y no puede encontrar una entrada para el ID. de evento 1109 en Visor de eventos, compruebe que los Service Pack del controlador de dominio están actualizados. No se puede volver a intentar este procedimiento en el mismo VHD. Puede intentarlo de nuevo con una copia del VHD o con otro VHD distinto que no se haya iniciado en modo normal; para ello, vuelva a empezar en el paso 1.
13. Cierre el Editor del Registro.

## <a name="usn-and-usn-rollback"></a>Reversión de USN y USN

En esta sección se describen los problemas de replicación que pueden producirse como resultado de una restauración incorrecta de la base de datos de Active Directory con una versión anterior de una máquina virtual. Para obtener más información sobre el proceso de replicación de Active Directory, consulte Active Directory de los [conceptos de replicación](../replication/active-directory-replication-concepts.md) .

## <a name="usns"></a>Números de secuencias actualizadas (USN)

Servicios de dominio de Active Directory (AD DS) usa números de secuencias actualizadas (USN) para hacer un seguimiento de la replicación de datos entre controladores de dominio. Cada vez que se realiza un cambio en los datos del directorio, el USN se incrementa para indicar que esto ocurrió.

Para cada partición de directorio que almacena un controlador de dominio de destino, los USN se usan para realizar el seguimiento de la última actualización de origen que un controlador de dominio ha introducido en su base de datos, así como el estado de todos los demás controladores de dominio que almacenan una réplica de la partición de directorio. Cuando los controladores de dominio replican cambios entre sí, consultan los asociados de replicación de los cambios con USN mayores que el USN del último cambio que el controlador de dominio ha recibido de cada asociado.

Las dos tablas de metadatos de replicación siguientes contienen USN. Los controladores de dominio de origen y de destino usan estos valores para filtrar las actualizaciones que requiere el controlador de dominio de destino.

1. **Vector de actualización**: una tabla que el controlador de dominio de destino mantiene para realizar el seguimiento de las actualizaciones de origen recibidas de todos los controladores de dominio de origen. Cuando un controlador de dominio de destino solicita cambios para una partición del directorio, proporciona su vector de actualización al controlador de dominio de origen. A continuación, el controlador de dominio de origen usa este valor para filtrar las actualizaciones que envía al controlador de dominio de destino. El controlador de dominio de origen envía su vector de actualización al destino al finalizar un ciclo de replicación correcto para asegurarse de que el controlador de dominio de destino sabe que se ha sincronizado con las actualizaciones de origen de cada controlador de dominio y que las actualizaciones están en el mismo nivel que el origen.
2. **Marca de límite superior**: valor que mantiene el controlador de dominio de destino para realizar un seguimiento de los cambios más recientes que ha recibido de un controlador de dominio de origen específico para una partición específica. La marca de límite superior impide que el controlador de dominio de origen envíe los cambios que el controlador de dominio de destino ya ha recibido de él.

## <a name="directory-database-identity"></a>Identidad de la base de datos del directorio

Además de los USN, los controladores de dominio hacen un seguimiento de la base de datos del directorio de los asociados de replicación de origen. La identidad de la base de datos del directorio que se ejecuta en el servidor se mantiene separada de la identidad del propio objeto de servidor. La identidad de la base de datos de directorio en cada controlador de dominio se almacena en el atributo de **invocación** del objeto de configuración NTDS, que se encuentra en la siguiente ruta de acceso del Protocolo ligero de acceso a directorios (LDAP): CN = NTDS Settings, CN = ServerName, CN = servers, CN =*siteName*, CN = Sites, CN = Configuration, DC =*dominioraízbosque* La identidad del objeto de servidor se almacena en el atributo **objectGUID** del objeto de configuración NTDS. La identidad del objeto de servidor no cambia nunca. Sin embargo, la identidad de la base de datos de directorio cambia cuando se produce un procedimiento de restauración de estado del sistema en el servidor o cuando se agrega una partición de directorio de aplicaciones, y después se quita y se vuelve a agregar desde el servidor. (otro escenario: cuando una instancia de HyperV desencadena sus escritores VSS en una partición que contiene un VHD de un controlador de dominio virtual, el invitado a su vez desencadena sus propios escritores de VSS (el mismo mecanismo que usa la copia de seguridad o restauración anterior), lo que da lugar a otro medio por el que se restablece el ID. de invocación)

En consecuencia, **invocationID** relaciona de manera efectiva un conjunto de actualizaciones en un controlador de dominio con una versión específica de la base de datos del directorio. El vector de actualización y las tablas de marca de límite superior usan el ID. de **invocación** y el GUID de DC, respectivamente para que los controladores de dominio sepan de qué copia de la base de datos de Active Directory está llegando la información de replicación.

El atributo **invocationID** es un valor de identificador único global (GUID) que se ve cerca de la parte superior de los resultados que se obtienen al ejecutar el comando **repadmin /showrepl**. El siguiente texto es un ejemplo del resultado que se obtiene al ejecutar el comando:

   ```
   Repadmin: running command /showrepl against full DC local host
   Default-First-Site-Name\VDC1
   DSA Options: IS_GC
   DSA object GUID: 966651f3-a544-461f-9f2c-c30c91d17818
   DSA invocationID: b0d9208b-8eb6-4205-863d-d50801b325a9
   ```

Cuando AD DS se restaura correctamente en un controlador de dominio, se restablece el **invocación** de este. Como resultado de este cambio, experimentará un aumento en el tráfico de replicación, cuya duración es relativa al tamaño de la partición que se está replicando.

Por ejemplo, supongamos que VDC1 y DC2 son dos controladores de dominio del mismo dominio. En la siguiente ilustración se muestra cómo percibe DC2 a VDC1 cuando el valor invocationID se restablece en una restauración correcta.

![Diagrama en el que el valor de invocación se restablece correctamente](media/virtualized-domain-controller-architecture/Dd363553.ca71fc12-b484-47fb-991c-5a0b7f516366(WS.10).gif)

## <a name="usn-rollback"></a>Reversión de USN

La reversión de USN ocurre cuando se ignoran las actualizaciones normales de los USN y un controlador de dominio intenta usar un USN menor que el de la última actualización. Se detectará la reversión de USN y se detendrá la replicación antes de que se cree la divergencia en el bosque, en la mayoría de los casos.

Una reversión de USN puede tener varias causas, por ejemplo, cuando se usan los archivos de un disco duro virtual (VHD) antiguo o se realiza una conversión de física a virtual (conversión FaV) sin asegurarse de que la máquina física permanezca sin conexión de forma permanente después de la conversión. Para asegurar que no ocurra una reversión de USN, se deben tomar las siguientes precauciones:

   - Si no se ejecuta Windows Server 2012 o una versión más reciente, no tome ni use una instantánea de una máquina virtual de controlador de dominio.
   - No copiar el archivo VHD del controlador de dominio.
   - Si no se ejecuta Windows Server 2012 o una versión más reciente, no exporte la máquina virtual que ejecuta un controlador de dominio.
   - No restaurar un controlador de dominio ni intentar revertir el contenido de una base de datos de Active Directory de ninguna otra manera que no sea mediante una solución de copia de seguridad compatible, como Copias de seguridad de Windows Server.

En algunos casos, es posible que no se detecte la reversión de USN. En otros, podría ocasionar otros errores de replicación. Si esto ocurre, es necesario identificar la gravedad del problema y solucionarlo a tiempo. Para obtener información acerca de cómo quitar objetos persistentes que pueden producirse como resultado de la reversión de USN, vea los [objetos Active Directory obsoletos generan el ID. de evento 1988 en Windows Server 2003](https://go.microsoft.com/fwlink/?linkid=137185) en Microsoft Knowledge base.

## <a name="usn-rollback-detection"></a>Detectar una reversión de USN

En la mayoría de los casos, se logra detectar las reversiones de USN que se realizan sin restablecer **invocationID** causadas por procedimientos de restauración incorrectos. Windows Server 2008 proporciona protección contra replicaciones incorrectas después de que se realice una operación de restauración incorrecta de un controlador de dominio. Esta protección se activa cuando una operación de restauración incorrecta da como resultado USN más bajos que representan los cambios que los asociados de replicación ya recibieron.

En Windows Server 2008 y Windows Server 2003 SP1, cuando un controlador de dominio de destino solicita cambios mediante un USN usado previamente, el controlador de dominio de destino interpreta que la respuesta del asociado de replicación de origen significa que los metadatos de replicación no están actualizados. Esto indica que la base de datos de Active Directory del controlador de dominio de origen se ha revertido a un estado anterior. Por ejemplo, el archivo VHD de una máquina virtual se revirtió a una versión anterior. En este caso, el controlador de dominio de destino inicia las siguientes medidas de cuarentena en el controlador de dominio que se considera que se restauró incorrectamente:

   - AD DS pausa el servicio de Net Logon, lo que evita que las cuentas de usuario y de equipo cambien las contraseñas. Esto impide que se pierdan los cambios en caso de que ocurran después de una restauración incorrecta.
   - AD DS deshabilita la replicación de entrada y de salida de Active Directory.
   - AD DS genera el identificador de evento 2095 en el registro de eventos del Servicio de directorio para indicar la condición.

En la siguiente ilustración se muestra la secuencia de eventos que tiene lugar cuando se detecta una reversión de USN en VDC2, el controlador de dominio de destino que se ejecuta en una máquina virtual. En esta ilustración, la detección de la reversión de USN se produce en VDC2 cuando un asociado de replicación detecta que VDC2 ha enviado un valor de USN de actualización que el controlador de dominio de destino ha detectado anteriormente, lo que indica que la base de datos Vdc2 se ha revertido en el tiempo de forma incorrecta.

![Diagrama que muestra lo que sucede cuando se detecta una reversión de USN](media/virtualized-domain-controller-architecture/Dd363553.373b0504-43fc-40d0-9908-13fdeb7b3f14(WS.10).gif)

Si el registro de eventos del Servicio de directorio notifica el identificador de evento 2095, siga estos pasos inmediatamente.

## <a name="to-resolve-event-id2095"></a>Para resolver el identificador de evento 2095

1. Aísle la máquina virtual que grabó el error de la red.
2. Intente determinar si se originaron cambios en ese controlador de dominio y se propagaron a otros controladores de dominio. Si el evento fue resultado de iniciar una instantánea o una copia de una máquina virtual, intente determinar la hora en la que ocurrió la reversión de USN. Luego puede comprobar los asociados de replicación de ese controlador de dominio para determinar si la replicación ocurrió después de eso.

   Para ello puede usar la herramienta Repadmin. Para obtener información sobre cómo usar repadmin, consulte [supervisión y solución de problemas de la replicación de Active Directory mediante repadmin](https://go.microsoft.com/fwlink/?linkid=122830). Si no puede determinar esto, póngase en contacto con [soporte técnico de Microsoft](https://support.microsoft.com) para obtener ayuda.

3. Disminuya el nivel del controlador de dominio de manera forzosa. Esto implica limpiar los metadatos del controlador de dominio y asumir las funciones de maestro de operaciones (también conocidas como operaciones de maestro único flexible o FSMO). Para obtener más información, consulte la sección "recuperación de la reversión de USN" de [detección y recuperación de una reversión de USN en Windows server 2003, Windows server 2008 y Windows server 2008 R2](https://go.microsoft.com/fwlink/?linkid=137182) en Microsoft Knowledge base.
4. Quite todos los archivos VHD anteriores del controlador de dominio.

## <a name="undetected-usn-rollback"></a>Reversión de USN no detectada

Es posible que la reversión de USN no se detecte en una de estas dos situaciones:

1. El archivo VHD está asociado a máquinas virtuales diferentes que se ejecutan en varias ubicaciones al mismo tiempo.
2. El USN del controlador de dominio que se restauró aumentó más que el último USN que recibió el otro controlador de dominio.

En el primer caso, es posible que se repliquen otros controladores de dominio con cualquiera de las máquinas virtuales y que los cambios se realicen en cualquiera de ellas, sin replicarse a la otra. Esta divergencia del bosque es difícil de detectar y ocasiona respuestas impredecibles del directorio. Esto puede ocurrir después de la migración FaV si la máquina física y la virtual se ejecutan en la misma red. También podría suceder cuando se crean varios controladores de dominio desde el mismo controlador de dominio físico y, a continuación, se ejecutan en la misma red.

En el segundo caso, un intervalo de USN se aplica a dos conjuntos de cambios distintos. Esto puede ocurrir durante largos períodos sin que se detecte. Cuando se modifica un objeto que se creó durante ese período, se detecta un objeto persistente y se notifica como identificador de evento 1988 en el Visor de eventos. En la siguiente ilustración se muestra cómo es posible que no se detecte la reversión de USN en la situación descrita.

![Diagrama cómo podría no detectarse la reversión de USN](media/virtualized-domain-controller-architecture/Dd363553.63565fe0-d970-4b4e-b5f3-9c76bc77e2d4(WS.10).gif)

## <a name="read-only-domain-controllers"></a>Controladores de dominio de solo lectura

Los RODC son controladores de dominio que hospedan copias de solo lectura de las particiones en una base de datos de Active Directory. Los RODC evitan la mayoría de los problemas de la reversión de USN porque no replican ningún cambio a otros controladores de dominio. Sin embargo, si un RODC se replica desde un controlador de dominio grabable afectado por la reversión de USN, el RODC también se verá afectado.

No se recomienda restaurar un RODC con una instantánea. Para restaurar un RODC, use una aplicación de copia de seguridad compatible con Active Directory. Además, de la misma manera que con los controladores de dominio grabables, se debe tener cuidado para no permitir que un RODC permanezca desconectado por un tiempo mayor que la duración del marcador de exclusión. De lo contrario, podrían aparecer objetos persistentes en el RODC.

Para obtener más información acerca de los RODC, vea la [Guía de planeamiento e implementación del controlador de dominio de solo lectura](../../deploy/rodc/read-only-domain-controller-updates.md).
