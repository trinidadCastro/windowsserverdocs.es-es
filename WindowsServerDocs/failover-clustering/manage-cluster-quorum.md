---
title: Configurar y administrar el cuórum en un clúster de conmutación por error
description: Información detallada sobre cómo administrar el cuórum de clúster en un clúster de conmutación por error de Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03e155cb9d30bc32da407f0d9ae915308f31494a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361016"
---
# <a name="configure-and-manage-quorum"></a>Configurar y administrar el cuórum

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema se proporciona información general y los pasos para configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server.

## <a name="understanding-quorum"></a>Descripción del cuórum

El cuórum para un clúster viene determinado por el número de elementos de votación que deben formar parte de la pertenencia al clúster activa para dicho clúster para que se inicie correctamente o continúe ejecutándose. Para obtener una explicación más detallada, consulte el [documento Descripción del clúster y el cuórum del grupo](../storage/storage-spaces/understand-quorum.md).

## <a name="quorum-configuration-options"></a>Opciones de configuración de cuórum

El modelo de cuórum en Windows Server es flexible. Si necesita modificar la configuración de quórum para el clúster, puede usar el Asistente para configurar Cuórum de clúster o los cmdlets de Windows PowerShell de clústeres de conmutación por error. Para conocer los pasos y consideraciones para configurar el cuórum, consulte [Configuración del cuórum de clúster](#configure-the-cluster-quorum) más adelante en este tema.

En la tabla siguiente encontrarás las tres opciones de configuración del cuórum que están disponibles en el Asistente para configurar cuórum de clúster.

| Opción  |Descripción  |
| --------- | ---------|
| Usar configuración típica     |  El clúster asigna automáticamente un voto a cada nodo y administra de forma dinámica los votos de nodos. Si funciona bien con el clúster y, además, hay disponible un almacenamiento compartido de clúster, el clúster seleccionará un testigo de disco. Esta opción es la recomendada en la mayoría de los casos, ya que el software del clúster elige automáticamente una configuración de cuórum y testigo que proporciona la mayor disponibilidad para el clúster.       |
| Agregar o cambiar el testigo de cuórum     |   Puede agregar, cambiar o quitar un recurso de testigo. Puede configurar un testigo de disco o recurso compartido de archivo. El clúster asigna automáticamente un voto a cada nodo y administra de forma dinámica los votos de nodos.      |
| Configuración avanzada de cuórum y selección de testigo     | Elige esta opción solo cuando tengas requisitos específicos para configurar el cuórum, ya sean de aplicación o de sitio. Puedes modificar el testigo de cuórum, agregar o quitar votos de los nodos y elegir si quieres que el clúster administre de forma dinámica los votos de nodos. De manera predeterminada, los votos se asignan a todos los nodos, y los votos de nodos se administran de forma dinámica.        |

Según la opción de configuración de cuórum que elijas y la configuración específica, el clúster se configurará en uno de los siguientes modos de cuórum:

| Modo  | Descripción  |
| --------- | ---------|
| Mayoría de nodo (sin testigo)     |   Solo los nodos tienen votos. No se configura ningún testigo de cuórum. El cuórum de clúster es la mayoría de los nodos de votación en la pertenencia al clúster activa.      |
| Mayoría de nodo con testigo (disco o recurso compartido de archivos)     |   Los nodos tienen votos. Además, los testigos de cuórum tienen un voto. El cuórum de clúster es la mayoría de los nodos de votación en la pertenencia al clúster activa, más un voto de testigo. Los testigos de cuórum se pueden designar como testigos de disco o como testigos de recurso compartido de archivos. 
| Sin mayoría (solo testigo de disco)     | Ningún nodo tiene votos. Solo los testigos de disco tienen un voto. <br>El cuórum de clúster se determina por el estado del testigo de disco. En general, no se recomienda usar este modo y solo debería seleccionarse porque crea un punto de error único para el clúster.       |

En las siguientes subsecciones se proporciona más información sobre la configuración avanzada de cuórum.

### <a name="witness-configuration"></a>Configuración de testigos

Como norma general, al configurar un cuórum los elementos de votación del clúster deben tener un número impar. Por lo tanto, si el clúster contiene un número par de nodos de votación, tendrás que configurar un testigo de disco o un testigo de recurso compartido de archivos. El clúster admitirá un nodo más desactivado. Además, si agregas un voto de testigo, el clúster continuará ejecutándose si la mitad de los nodos de clúster se desactivan o desconectan de forma simultánea.

Si todos los nodos pueden ver el disco, usa un testigo de disco. Usa testigos de recurso compartido de archivos si necesitas la recuperación ante desastres multisitio con almacenamiento replicado. Se puede configurar un testigo de disco con almacenamiento replicado, pero solo si el proveedor de almacenamiento admite el acceso de lectura y escritura desde todos los sitios al almacenamiento replicado. <strong>*No se admite un testigo de disco con espacios de almacenamiento directo*</strong>.

En la tabla siguiente encontrarás información adicional y consideraciones sobre los tipos de testigos de cuórum.

| Tipo de testigo  | Descripción  | Requisitos y recomendaciones  |
| ---------    |---------        |---------                        |
| Testigo de disco     |  <ul><li> LUN dedicado que almacena una copia de la base de datos del clúster</li><li> Recomendado para clústeres con almacenamiento compartido (no replicado)</li>       |  <ul><li>El tamaño del LUN debe ser como mínimo de 512 MB</li><li> Debe ser exclusivo para el clúster y no debe asignarse a un rol en clúster</li><li> Debe incluirse en almacenamiento en clúster y completar sin errores las pruebas de validación de almacenamiento</li><li> No puede ser un disco que sea un volumen compartido de clúster (CSV)</li><li> Disco básico con un solo volumen</li><li> No necesita tener asignada una letra de unidad</li><li> Puede formatearse como NTFS o como ReFS</li><li> De manera opcional, se puede configurar con RAID de hardware para tolerancia a errores</li><li> Debe excluirse de las copias de seguridad y de los análisis antivirus</li><li> No se admite un testigo de disco con Espacios de almacenamiento directo</li>|
| Testigo de recurso compartido de archivos     | <ul><li>El recurso compartido de archivos SMB que se configura en un servidor de archivos que ejecute Windows Server</li><li> No almacena una copia de la base de datos del clúster</li><li> Mantiene toda la información del clúster en el archivo witness.log</li><li> Recomendado para clústeres multisitio con almacenamiento replicado </li>       |  <ul><li>Debe disponer de un mínimo de 5 MB de espacio</li><li> Debe ser dedicado para un solo clúster y no usarse para almacenar datos de usuarios o de aplicaciones</li><li> Debe tener habilitados los permisos de escritura para el objeto de equipo del nombre del clúster</li></ul><br>A continuación, encontrarás consideraciones adicionales para un servidor de archivos que hospede el testigo de recurso compartido de archivos:<ul><li>Un solo servidor de archivos se puede configurar con testigos del recurso compartido de archivos para varios clústeres.</li><li> El servidor de archivos debe encontrarse en un sitio que esté separado de la carga de trabajo del clúster. Esto ofrece las mismas oportunidades de supervivencia para cualquier clúster si se pierden las comunicaciones de red de sitio a sitio. Si el servidor de archivos se encuentra en el mismo sitio, dicho sitio se convertirá en el sitio principal y será el único sitio que podrá conectarse al recurso compartido de archivos.</li><li> El servidor de archivos se puede ejecutar en una máquina virtual si esta no está hospedada en el mismo clúster que usa el testigo de recurso compartido de archivos.</li><li> Para obtener una alta disponibilidad, el servidor de archivos se puede configurar en un clúster de conmutación por error separado. </li>      |
| Testigo en la nube     |  <ul><li>Un archivo testigo almacenado en el almacenamiento de blobs de Azure</li><li> Recomendado cuando todos los servidores del clúster tienen una conexión a Internet confiable.</li>      |  Vea [implementación de un testigo en la nube](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).       |

### <a name="node-vote-assignment"></a>Asignación de votos de nodos

Como opción de configuración avanzada de cuórum, puede elegir asignar o quitar votos de cuórum por nodo. De manera predeterminada, todos los nodos son votos asignados. Independientemente de la asignación de votos, todos los nodos continuarán funcionando en el clúster, recibirán actualizaciones de base de datos del clúster y pueden hospedar aplicaciones.

Es posible que quieras quitar votos de nodos en determinadas configuraciones de recuperación ante desastres. Por ejemplo, en un clúster multisitio podrías eliminar votos de los nodos en un sitio de copia de seguridad para que dichos nodos no afecten a los cálculos de cuórum. Esta configuración solo se recomienda para la conmutación por error manual en varios sitios. Para obtener más información, consulta [Consideraciones del quórum para las configuraciones de recuperación ante desastres](#quorum-considerations-for-disaster-recovery-configurations) más adelante en este tema.

El voto configurado de un nodo se puede comprobar buscando la propiedad común **NodeWeight** del nodo de clúster mediante el cmdlet [Get-ClusterNode de](https://technet.microsoft.com/library/hh847268.aspx)Windows PowerShell. El valor 0 indica que el nodo no tiene configurado un voto de cuórum. El valor 1 indica que el voto de cuórum del nodo está asignado y que está administrado por el clúster. Para obtener más información sobre la administración de votos de nodos, consulta [Administración de quórum dinámico](#dynamic-quorum-management) más adelante en este tema.

Comprueba la asignación de votos de todos los nodos de clúster mediante la prueba de validación **Validar quórum de clúster**.

#### <a name="additional-considerations-for-node-vote-assignment"></a>Consideraciones adicionales para la asignación de votos de nodo

  - No uses la asignación de votos de nodos para obligar a usar un número impar de nodos de votación. En lugar de ello, configura un testigo de disco o un testigo de recurso compartido de archivos. Para obtener más información, vea [configuración de testigos](#witness-configuration) más adelante en este tema.
  - Si se habilita la administración de cuórum dinámico, solo se pueden asignar o quitar de forma dinámica los nodos que estén configurados para tener votos de nodos asignados. Para obtener más información, consulta [Administración de quórum dinámico](#dynamic-quorum-management) más adelante en este tema.

### <a name="dynamic-quorum-management"></a>Administración dinámica de cuórum

En Windows Server 2012, como opción de configuración avanzada de cuórum, puede elegir habilitar la administración dinámica de cuórum por clúster. Para obtener más información sobre cómo funciona el cuórum dinámico, vea [esta explicación](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Con la administración de cuórum dinámico también se puede ejecutar un clúster en el último nodo de clúster superviviente. Al ajustar de forma dinámica el requisito de mayoría del cuórum, el clúster puede sostener apagados de nodo secuenciales en un nodo único.

El voto dinámico asignado a un clúster de un nodo se puede comprobar con la propiedad común **DynamicWeight** del nodo de clúster mediante el cmdlet de Windows PowerShell [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) . El valor 0 indica que el nodo no tiene un voto de cuórum. El valor 1 indica que el nodo tiene un voto de cuórum.

Comprueba la asignación de votos de todos los nodos de clúster mediante la prueba de validación **Validar quórum de clúster**.

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>Consideraciones adicionales para la administración dinámica de cuórum

- La administración de cuórum dinámico no permite que el clúster mantenga un error simultáneo de una mayoría de miembros con derecho a voto. Para continuar ejecutándose, el clúster siempre debe tener una mayoría del cuórum cuando un nodo se apague o produzca errores.

- Si has quitado de manera explícita el voto de un nodo, el clúster tampoco puede agregar o quitar de forma dinámica dicho voto.
- Cuando Espacios de almacenamiento directo está habilitado, el clúster solo puede admitir dos errores de nodo. Esto se explica con más detalle en la [sección Grupo de cuórum](../storage/storage-spaces/understand-quorum.md)

## <a name="general-recommendations-for-quorum-configuration"></a>Recomendaciones generales para la configuración de cuórum

El software del clúster configura automáticamente el cuórum para un nuevo clúster según el número de nodos configurados y la disponibilidad del almacenamiento compartido. Esta suele ser la configuración de cuórum más apropiada para dicho clúster. Pero es buena idea revisar la configuración de cuórum después de crear el clúster y antes de usar el clúster en producción. Para ver la configuración detallada del cuórum de clúster, puede usar el Asistente para validar una configuración o el cmdlet [Test-Cluster de](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) Windows PowerShell para ejecutar la prueba **validar configuración de cuórum** . En Administrador de clústeres de conmutación por error, la configuración de cuórum básica se muestra en la información de resumen del clúster seleccionado, o puede revisar la información sobre los recursos de cuórum que devuelve al ejecutar el cmdlet de Windows PowerShell [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) .

En cualquier momento, puedes ejecutar la prueba **Validar configuración de quórum** para comprobar que la configuración de quórum es óptima para el clúster. La salida de la prueba indica si es recomendable realizar cambios en la configuración de cuórum y cuáles son las opciones óptimas. Si se recomienda realizar un cambio, puedes usar el Asistente para configurar cuórum de clúster para aplicar la configuración recomendada.

Cuando el clúster esté en producción, no cambies la configuración de cuórum a menos que hayas determinado que el cambio es apropiado para el clúster. Puede que quieras cambiar la configuración de cuórum en los casos siguientes:

- Para agregar o expulsar nodos
- Para agregar o quitar almacenamiento
- En caso de errores de nodos o testigos de larga duración
- Para recuperar un clúster en un escenario de recuperación ante desastres multisitio

Para obtener más información sobre la validación de un clúster de conmutación por error, consulte [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="configure-the-cluster-quorum"></a>Configuración del cuórum de clúster

Puede configurar los valores del cuórum de clúster con Administrador de clústeres de conmutación por error o los cmdlets de Windows PowerShell de clústeres de conmutación por error.

> [!IMPORTANT]
> En general, la mejor opción es usar la configuración de cuórum recomendada por el Asistente para configurar cuórum de clúster. Solo se recomienda personalizar la configuración de cuórum si consideras que el cambio es adecuado para el clúster. Para obtener más información, consulta [Recomendaciones generales para la configuración de quórum](#general-recommendations-for-quorum-configuration) más adelante en este tema.

### <a name="configure-the-cluster-quorum-settings"></a>Configurar las opciones de cuórum de clúster

Para completar este procedimiento se requiere, como mínimo, pertenecer al grupo de **administradores** locales en todos los servidores en clúster, o equivalente. Además, la cuenta que uses debe ser una cuenta de usuario de dominio.

> [!NOTE]
> La configuración de cuórum de clúster se puede cambiar sin que sea necesario detener el clúster o desconectar los recursos de clúster.

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>Cambiar la configuración de cuórum en un clúster de conmutación por error mediante Administrador de clústeres de conmutación por error

1. En el Administrador de clústeres de conmutación por error, selecciona o especifica lo que quieras cambiar.
2. Con el clúster seleccionado, en **acciones**, seleccione **más acciones**y, a continuación, seleccione **configurar cuórum de clúster**. Se abrirá el Asistente para configurar cuórum de clúster. Selecciona **Siguiente**.
3. En la página **Seleccionar opción de configuración de quórum** , selecciona una de las tres opciones de configuración y completa los pasos para dicha opción. Antes de modificar la configuración del cuórum puedes revisar las opciones. Para obtener más información sobre las opciones, vea [Descripción del cuórum](#understanding-quorum), anteriormente en este tema.

    - Para permitir que el clúster restablezca automáticamente la configuración de cuórum que sea óptima para la configuración de clúster actual, seleccione **usar la configuración típica** y, a continuación, complete el asistente.
    - Para agregar o cambiar el testigo de quórum, seleccione **Agregar o cambiar el testigo de cuórum**y, a continuación, complete los pasos siguientes. Para obtener más información y conocer consideraciones sobre la configuración de un testigo de quórum, consulta [Configuración de testigos](#witness-configuration) anteriormente en este tema.

      1. En la página **Seleccionar testigo de quórum** , selecciona una opción para configurar un testigo de disco o un testigo de recurso compartido de archivos. El asistente indica las opciones de selección de testigo recomendadas para el clúster.

          > [!NOTE]
          > También puedes seleccionar **No configurar un testigo de quórum** y completar el asistente después. No se recomienda tener un número par de nodos de votación en el clúster.

      2. Si seleccionas la opción para configurar un testigo de disco, en la página **Configurar testigo de almacenamiento** , selecciona el volumen de almacenamiento que quieras asignar como el testigo de disco y, después, completa el asistente.
      3. Si seleccionas la opción para configurar un testigo de recurso compartido de archivos, en la página **Configurar testigo del recurso compartido de archivos**, escribe o explora hasta un recurso compartido de archivos que quieras usar como el recurso de testigo y, después, completa el asistente.

    - Para configurar las opciones de administración de cuórum y para agregar o cambiar el testigo de cuórum, seleccione **Configuración avanzada de cuórum y selección de testigo**y, después, complete los pasos siguientes. Para obtener información y consideraciones sobre la configuración avanzada de cuórum, consulte [Asignación de votos de nodos](#node-vote-assignment) y [Administración dinámica de cuórum](#dynamic-quorum-management) anteriormente en este tema.

      1. En la página **Seleccionar configuración de votación**, selecciona una opción para asignar votos a nodos. De manera predeterminada, todos los nodos tienen asignado un voto. Pero en algunos escenarios puedes asignar votos a un subconjunto específico de nodos.

          > [!NOTE]
          > También puedes seleccionar la opción **Ningún nodo**. Esta opción no suele recomendarse, ya que no permite que los nodos participen en la votación de cuórum y, además, requiere configurar un testigo de disco. Este testigo de disco se convierte en el único punto de error del clúster.

      2. En la página **Configurar administración de quórum** puedes habilitar o deshabilitar la opción **Permitir que el clúster administre dinámicamente la asignación de votos de nodos**. Al seleccionar esta opción, se aumenta la disponibilidad del clúster. Esta opción está seleccionada de manera predeterminada y no se recomienda deshabilitarla. Esta opción permite que el clúster continúe ejecutándose en escenarios de error que no son posibles cuando esta opción está deshabilitada.
      3. En la página **Seleccionar testigo de quórum** , selecciona una opción para configurar un testigo de disco o un testigo de recurso compartido de archivos. El asistente indica las opciones de selección de testigo recomendadas para el clúster.

          > [!NOTE]
          > También puedes seleccionar **No configurar un testigo de quórum**y completar el asistente después. No se recomienda tener un número par de nodos de votación en el clúster.

      4. Si seleccionas la opción para configurar un testigo de disco, en la página **Configurar testigo de almacenamiento** , selecciona el volumen de almacenamiento que quieras asignar como el testigo de disco y, después, completa el asistente.
      5. Si seleccionas la opción para configurar un testigo de recurso compartido de archivos, en la página **Configurar testigo del recurso compartido de archivos**, escribe o explora hasta un recurso compartido de archivos que quieras usar como el recurso de testigo y, después, completa el asistente.

4. Selecciona **Siguiente**. Confirme las selecciones en la página de confirmación que aparece y, a continuación, seleccione **siguiente**.

Una vez que se ejecute el asistente y aparezca la página **Resumen** , si desea ver un informe de las tareas realizadas por el asistente, seleccione **Ver informe**. El informe más reciente permanecerá en la carpeta <em>systemroot</em> **\\Cluster\\Reports** con el nombre **QuorumConfiguration. mht**.

> [!NOTE]
> Después de configurar el quórum de clúster, ejecuta la prueba **Validar configuración de quórum** para comprobar la configuración de quórum actualizada.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

En los siguientes ejemplos se muestra cómo usar el cmdlet [set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) y otros cmdlets de Windows PowerShell para configurar el cuórum de clúster.

En el ejemplo siguiente se cambia la configuración de quórum en el clúster *CONTOSO-FC1* a una configuración de quórum de mayoría de nodo sencilla sin testigo de quórum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

En el ejemplo siguiente se cambia la configuración de cuórum en el clúster local a una mayoría de nodo con configuración de testigo. El recurso de disco llamado *Disco de clúster 2* se configura como un testigo de disco.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

En el ejemplo siguiente se cambia la configuración de cuórum en el clúster local a una mayoría de nodo con configuración de testigo. El recurso de recurso compartido de archivos denominado *\\\\contoso-FS\\FSW* está configurado como testigo de recurso compartido de archivos.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

En los ejemplos siguientes se elimina el voto de quórum del nodo *ContosoFCNode1* en el clúster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

En el ejemplo siguiente se agrega el voto de quórum al nodo *ContosoFCNode1* en el clúster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

En el ejemplo siguiente se habilita la propiedad **DynamicQuorum** del clúster *CONTOSO-FC1* (si se deshabilitó anteriormente):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## <a name="recover-a-cluster-by-starting-without-quorum"></a>Recuperar un clúster mediante el inicio sin cuórum

Un clúster no se iniciará si no tiene suficientes votos de cuórum. El primer paso es confirmar la configuración de cuórum de clúster e tratar de averiguar por qué el clúster ya no tiene un cuórum. Esto puede ocurrir si tienes nodos que han dejado de responder, o bien si el sitio principal no es accesible en un clúster multisitio. Después de identificar la causa raíz del error del clúster, sigue los pasos de recuperación que se describen en esta sección.

> [!NOTE]
> * Si el servicio de clúster se detiene porque se pierde el cuórum, se mostrará el evento id. 1177 en el registro del sistema.
> * Es importante investigar por qué se ha perdido el cuórum de clúster.
> * Se recomienda poner un nodo o un testigo de cuórum en un estado correcto (unido al clúster) en lugar de iniciar el clúster sin un cuórum.

### <a name="force-start-cluster-nodes"></a>Forzar inicio de nodos de clúster

Después de determinar que no se puede recuperar el clúster si se ponen los nodos o el testigo de cuórum en un estado correcto, es necesario forzar el inicio del clúster. Al forzar el inicio del clúster, se invalidan las opciones de configuración de quórum de clúster y se inicia el clúster en el modo **ForceQuorum**.

Forzar el inicio de un clúster cuando no tenga cuórum puede ser especialmente útil en un clúster multisitio. Imagina un escenario de recuperación ante desastres con un clúster que contenga un sitio primario y un sitio de copia de seguridad en ubicaciones separadas (*SitioA* y *SitioB*). Si se produce un desastre únicamente en el *SitioA*, podría necesitarse una gran cantidad de tiempo para volver a poner en línea el sitio. Puede que quieras obligar a que el *SitioB* esté en línea, incluso aunque no tenga quórum.

Cuando se inicia un clúster con el modo **ForceQuorum** , y después de que vuelva a obtener votos de quórum suficientes, el clúster dejará automáticamente el estado forzado y su comportamiento volverá a ser normal. Por lo tanto, no será necesario volver a iniciar el clúster normalmente. Si el clúster pierde un nodo y también cuórum, volverá a estar sin conexión, ya que no estará en el estado forzado. Para volver a ponerlo en línea cuando no tiene quórum, es necesario forzar el inicio del clúster sin cuórum.

> [!IMPORTANT]
> * Después de forzar el inicio de un clúster, el administrador tendrá control total sobre el clúster.
> * El clúster usa la configuración del clúster en el nodo donde el clúster se encuentra en estado forzado y lo replicará al resto de nodos que estén disponibles.
> * Si obligas al nodo a que se inicie sin quórum, se ignorarán todas las opciones de configuración de quórum cuando el clúster esté en el modo **ForceQuorum**. Esto incluye asignaciones de votos de nodos específicas y opciones de administración de cuórum dinámico.

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>Evitar cuórum en nodos de clúster restantes

Después de obligar a que el clúster se inicie en un nodo, es necesario iniciar el resto de nodos del clúster con una opción para evitar el cuórum. Los nodos iniciados con una opción que evite el cuórum indican al servicio de clúster que se una a un clúster que se esté ejecutando, en lugar de formar una nueva instancia de clúster. Esto evita que los nodos restantes formen un clúster dividido que contenga dos instancias paralelas.

Esto es necesario para recuperar el clúster en algunos escenarios de recuperación ante desastres multisitio después de forzar el inicio del clúster en el sitio de copia de seguridad ( *SitioB*). Para unirse al clúster iniciado de manera forzosa en el *SitioB*, es necesario iniciar los nodos los nodos del sitio principal (*SitioA*) sin permitir el quórum.

> [!IMPORTANT]
> Después de iniciar de manera forzosa un clúster en un nodo, se recomienda iniciar siempre el resto de nodos sin permitir el cuórum.

Aquí se muestra cómo recuperar el clúster con Administrador de clústeres de conmutación por error:

1. En el Administrador de clústeres de conmutación por error, selecciona o especifica el clúster que quieras recuperar.
2. Con el clúster seleccionado, en **acciones**, seleccione **Forzar inicio de clúster**.

    El Administrador de clústeres de conmutación por error forzará el inicio del clúster en todos los nodos que estén accesibles. El clúster usa la configuración del clúster actual al iniciarse.

> [!NOTE]
> * Para forzar que el clúster se inicie en un nodo específico que contenga una configuración de clúster que desee usar, debe usar los cmdlets de Windows PowerShell o las herramientas de línea de comandos equivalentes que se muestran después de este procedimiento. 
> * Si usas el Administrador de clústeres de conmutación por error para conectarte a un clúster que se haya iniciado de manera forzosa y, además, usas la acción **Iniciar el Servicio de clúster** para iniciar un nodo, el nodo se iniciará automáticamente con la opción que impide el quórum.

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Comandos equivalentes de Windows PowerShell (Start-Clusternode)

En el ejemplo siguiente se muestra cómo usar el cmdlet **Start-ClusterNode** para forzar al clúster a iniciarse en el nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

Como alternativa, puedes usar el comando siguiente de forma local en el nodo:

```PowerShell
Net Start ClusSvc /FQ
```

En el ejemplo siguiente se muestra cómo usar el cmdlet **Start-ClusterNode** para iniciar el servicio de clúster con el cuórum impedido en el nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

Como alternativa, puedes usar el comando siguiente de forma local en el nodo:

```PowerShell
Net Start ClusSvc /PQ
```

## <a name="quorum-considerations-for-disaster-recovery-configurations"></a>Consideraciones del cuórum para las configuraciones de recuperación ante desastres

En esta sección encontrarás un resumen de las características y las configuraciones de cuórum para dos configuraciones de clúster multisitio en implementaciones de recuperación ante desastres. Las instrucciones de configuración de cuórum son distintas según si necesitas la conmutación automática por error o la conmutación por error manual para cargas de trabajo entre sitios. La configuración suele depender de los contratos de nivel de servicio de su organización para suministrar y dar soporte a cargas de trabajo en clúster en caso de un error o desastre en el sitio.

### <a name="automatic-failover"></a>Conmutación automática por error

En esta configuración, el clúster está formado por dos o más sitios que pueden hospedar roles agrupados en clúster. Si se produce un error en uno de los sitios, los roles agrupados en clúster conmutarán por error automáticamente en el resto de sitios. Por lo tanto, el cuórum de clúster debe configurarse de manera que cualquier sitio pueda sostener un error de sitio completo.

En la tabla siguiente se resumen las consideraciones y recomendaciones para esta configuración.

| Elemento  | Descripción  |
| ---------| ---------|
| Número de votos de nodos por sitio     | Debe ser igual       |
| Asignación de votos de nodos     |  No deben eliminarse los votos de nodos, ya que todos los nodos tienen la misma importancia       |
| Administración dinámica de cuórum     |   Debe estar habilitado      |
| Configuración de testigos     |  Se recomienda el testigo de recurso compartido de archivos, configurado en un sitio separado de los sitios del clúster       |
| Cargas de trabajo     |  Las cargas de trabajo se pueden configurar en cualquiera de los sitios       |

#### <a name="additional-considerations-for-automatic-failover"></a>Consideraciones adicionales para la conmutación automática por error

- Es necesario configurar el testigo de recurso compartido de archivos en un sitio separado para que todos los sitios tengan las mismas oportunidades de supervivencia. Para obtener más información, consulta [Configuración de testigos](#witness-configuration) anteriormente en este tema.

### <a name="manual-failover"></a>Conmutación por error manual

En esta configuración, el clúster está formado por un sitio principal, el *SitioA*, y un sitio de copia de seguridad (recuperación), el *SitioB*. Los roles agrupados en clúster están hospedados en el *SitioA*. Debido a la configuración de quórum de clúster, si se produce un error en todos los nodos del *SitioA*, el clúster dejará de funcionar. En este escenario, el administrador deberá conmutar por error de forma manual los servicios de clúster al *SitioB* y realizar pasos adicionales para recuperar el clúster.

En la tabla siguiente se resumen las consideraciones y recomendaciones para esta configuración.

| Elemento   |Descripción  |
| ---------| ---------|
| Número de votos de nodos por sitio     |  <ul><li> Los votos de nodos no deben eliminarse de los nodos en el sitio principal, **SitioA**</li><li>Los votos de nodos deben eliminarse de los nodos en el sitio de copia de seguridad, **SitioB**</li><li>Si se produce una interrupción de larga duración en el **SitioA**, los votos deben asignarse a los nodos del **SitioB** para habilitar una mayoría del quórum en dicho sitio como parte de la recuperación</li>       |
| Administración dinámica de cuórum     |  Debe estar habilitado       |
| Configuración de testigos     |  <ul><li>Configura un testigo si hay un número par de nodos en el **SitioA**</li><li>Si se necesita un testigo, configura un testigo de recurso compartido de archivos o un testigo de disco que solo sea accesible para nodos en el **SitioA** (a veces se conoce como un testigo de disco asimétrico)</li>       |
| Cargas de trabajo     |  Usa los propietarios preferidos para que las cargas de trabajo sigan ejecutándose en los nodos de **SitioA**       |

#### <a name="additional-considerations-for-manual-failover"></a>Consideraciones adicionales para la conmutación por error manual

- Solo los nodos del *SitioA* se configuran inicialmente con votos de quórum. Esto es necesario para garantizar que el estado de los nodos en el *SitioB* no afecte al quórum de clúster.
- Los pasos de recuperación pueden variar según si el *SitioA* puede admitir un error temporal o un error de larga duración.

## <a name="more-information"></a>Más información

* [Clúster de conmutación por error](failover-clustering.md)
* [Cmdlets de Windows PowerShell de clústeres de conmutación por error](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Descripción del Cuórum de clústeres y grupos](../storage/storage-spaces/understand-quorum.md)