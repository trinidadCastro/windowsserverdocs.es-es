---
title: Configurar y administrar el cuórum en un clúster de conmutación por error
description: Información detallada sobre cómo administrar el quórum de clúster en un clúster de conmutación por error de Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21f99362205e0a6aa90ebd26cef8f3a779bdc1dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836496"
---
# <a name="configure-and-manage-quorum"></a>Configurar y administrar el cuórum

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona en segundo plano y los pasos para configurar y administrar el quórum en un clúster de conmutación por error de Windows Server.

## <a name="understanding-quorum"></a>Quórum de descripción

El cuórum para un clúster viene determinado por el número de elementos de votación que deben formar parte de la pertenencia al clúster activa para dicho clúster para que se inicie correctamente o continúe ejecutándose. Para obtener una explicación más detallada, consulte el [doc de quórum de clúster y grupo descripción](../storage/storage-spaces/understand-quorum.md).

## <a name="quorum-configuration-options"></a>Opciones de configuración de quórum

El modelo de quórum en Windows Server es flexible. Si necesita modificar la configuración de quórum para el clúster, puede usar el Asistente para configurar quórum de clúster o los cmdlets de PowerShell de Windows de clústeres de conmutación por error. Para conocer los pasos y consideraciones para configurar el cuórum, consulte [Configuración del cuórum de clúster](#configure-the-cluster-quorum) más adelante en este tema.

En la tabla siguiente encontrarás las tres opciones de configuración del cuórum que están disponibles en el Asistente para configurar cuórum de clúster.

<table>
<thead>
<tr class="header">
<th>Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Usar configuración típica</td>
<td>El clúster asigna automáticamente un voto a cada nodo y administra de forma dinámica los votos de nodos. Si funciona bien con el clúster y, además, hay disponible un almacenamiento compartido de clúster, el clúster seleccionará un testigo de disco. Esta opción es la recomendada en la mayoría de los casos, ya que el software del clúster elige automáticamente una configuración de cuórum y testigo que proporciona la mayor disponibilidad para el clúster.</td>
</tr>
<tr class="even">
<td>Agregar o cambiar el testigo de cuórum</td>
<td>Puede agregar, cambiar o quitar un recurso de testigo. Puede configurar un testigo de disco o recurso compartido de archivo. El clúster asigna automáticamente un voto a cada nodo y administra de forma dinámica los votos de nodos.</td>
</tr>
<tr class="odd">
<td>Configuración avanzada de cuórum y selección de testigo</td>
<td>Elige esta opción solo cuando tengas requisitos específicos para configurar el cuórum, ya sean de aplicación o de sitio. Puedes modificar el testigo de cuórum, agregar o quitar votos de los nodos y elegir si quieres que el clúster administre de forma dinámica los votos de nodos. De manera predeterminada, los votos se asignan a todos los nodos, y los votos de nodos se administran de forma dinámica.</td>
</tr>
</tbody>
</table>

Según la opción de configuración de cuórum que elijas y la configuración específica, el clúster se configurará en uno de los siguientes modos de cuórum:

<table>
<thead>
<tr class="header">
<th>Modo</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Mayoría de nodo (sin testigo)</td>
<td>Solo los nodos tienen votos. No se configura ningún testigo de cuórum. El cuórum de clúster es la mayoría de los nodos de votación en la pertenencia al clúster activa.</td>
</tr>
<tr class="even">
<td>Mayoría de nodo con testigo (disco o recurso compartido de archivos)</td>
<td>Los nodos tienen votos. Además, los testigos de cuórum tienen un voto. El cuórum de clúster es la mayoría de los nodos de votación en la pertenencia al clúster activa, más un voto de testigo.<br />
<br />
Los testigos de cuórum se pueden designar como testigos de disco o como testigos de recurso compartido de archivos.</td>
</tr>
<tr class="odd">
<td>Sin mayoría (solo testigo de disco)</td>
<td>Ningún nodo tiene votos. Solo los testigos de disco tienen un voto. El cuórum de clúster se determina por el estado del testigo de disco.<br />
<br />
El clúster tendrá un cuórum si hay un nodo disponible que se comunique con un disco específico del almacenamiento de clúster. En general, no se recomienda usar este modo y solo debería seleccionarse porque crea un punto de error único para el clúster.</td>
</tr>
</tbody>
</table>

Las siguientes subsecciones le proporcionará más información sobre opciones de configuración avanzada de cuórum.

### <a name="witness-configuration"></a>Configuración de testigos

Como norma general, al configurar un cuórum los elementos de votación del clúster deben tener un número impar. Por lo tanto, si el clúster contiene un número par de nodos de votación, tendrás que configurar un testigo de disco o un testigo de recurso compartido de archivos. El clúster admitirá un nodo más desactivado. Además, si agregas un voto de testigo, el clúster continuará ejecutándose si la mitad de los nodos de clúster se desactivan o desconectan de forma simultánea.

Si todos los nodos pueden ver el disco, usa un testigo de disco. Usa testigos de recurso compartido de archivos si necesitas la recuperación ante desastres multisitio con almacenamiento replicado. Se puede configurar un testigo de disco con almacenamiento replicado, pero solo si el proveedor de almacenamiento admite el acceso de lectura y escritura desde todos los sitios al almacenamiento replicado. <strong>*No se admite un testigo de disco con espacios de almacenamiento directo*</strong>.

En la tabla siguiente encontrarás información adicional y consideraciones sobre los tipos de testigos de cuórum.

<table>
<thead>
<tr class="header">
<th>Tipo de testigo</th>
<th>Descripción</th>
<th>Requisitos y recomendaciones</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Testigo de disco</td>
<td>- LUN dedicado que almacena una copia de la base de datos de clúster<br />
- Más útiles para los clústeres con almacenamiento compartido de (no replicado)</td>
<td>- Tamaño de LUN debe ser al menos 512 MB<br />
- Debe ser dedicado para el clúster y no asignado a un rol en clúster<br />
- Debe estar incluido en el clúster pruebas de validación de almacenamiento de almacenamiento y pase<br />
- No puede ser un disco de un volumen de compartido de clúster (CSV)<br />
- Disco básico con un único volumen<br />
- No es necesario tener una letra de unidad<br />
- Puede formatearse como NTFS o ReFS<br />
- Se puede configurar con RAID de hardware para tolerancia a errores<br />
- Se deben excluir de las copias de seguridad y análisis antivirus<br />
- No se admite un testigo de disco con espacios de almacenamiento directo</td>
</tr>
<tr class="even">
<td>Testigo de recurso compartido de archivos</td>
<td>- Recurso compartido de archivos SMB que está configurada en un servidor de archivos que ejecutan Windows Server<br />
- No almacena una copia de la base de datos de clúster<br />
- Mantiene la información de clúster solo en el archivo witness.log<br />
- Más útiles para clústeres multisitio con almacenamiento replicado</td>
<td>- Debe tener un mínimo de 5 MB de espacio libre<br />
- Debe dedicada para un solo clúster y no se usa para almacenar datos de usuario o la aplicación<br />
- Permisos de escritura deben haber habilitado para el objeto de equipo para el nombre del clúster<br />
<br />
A continuación, encontrarás consideraciones adicionales para un servidor de archivos que hospede el testigo de recurso compartido de archivos:<br />
<br />
- Un único servidor de archivos se puede configurar con testigos de recurso compartido de archivos para varios clústeres.<br />
- Debe ser el servidor de archivos en un sitio que es independiente de la carga de trabajo de clúster. Esto ofrece las mismas oportunidades de supervivencia para cualquier clúster si se pierden las comunicaciones de red de sitio a sitio. Si el servidor de archivos se encuentra en el mismo sitio, dicho sitio se convertirá en el sitio principal y será el único sitio que podrá conectarse al recurso compartido de archivos.<br />
- Si la máquina virtual no está hospedada en el mismo clúster que usa el testigo del recurso compartido de archivos, puede ejecutar el servidor de archivos en una máquina virtual.<br />
- Para lograr alta disponibilidad, el servidor de archivos puede configurarse en un clúster de conmutación por error independiente.</td>
</tr>

<tr class-"odd">
<td>Testigo en la nube</td>
<td>- Un archivo de testigo almacenado en Azure blob storage<br>
-Se recomienda cuando todos los servidores del clúster tienen una conexión a Internet segura.</td>
<td>Consulte <a href="https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness">implementar un testigo en la nube</a>.</td>
<td>
</td>
</tr>

</tbody>
</table>

### <a name="node-vote-assignment"></a>Asignación de votos de nodos

Como una opción de configuración avanzada de cuórum, puede elegir asignar o quitar votos de quórum en una base por nodos. De manera predeterminada, todos los nodos son votos asignados. Independientemente de la asignación de votos, todos los nodos continuarán funcionando en el clúster, recibirán actualizaciones de base de datos del clúster y pueden hospedar aplicaciones.

Es posible que quieras quitar votos de nodos en determinadas configuraciones de recuperación ante desastres. Por ejemplo, en un clúster multisitio podrías eliminar votos de los nodos en un sitio de copia de seguridad para que dichos nodos no afecten a los cálculos de cuórum. Esta configuración solo se recomienda para la conmutación por error manual en varios sitios. Para obtener más información, consulta [Consideraciones del quórum para las configuraciones de recuperación ante desastres](#quorum-considerations-for-disaster-recovery-configurations) más adelante en este tema.

Se puede comprobar el voto configurado de un nodo buscando el **NodeWeight** propiedad común del nodo de clúster mediante el uso de la [Get-ClusterNode](http://technet.microsoft.com/library/hh847268.aspx)cmdlet de Windows PowerShell. El valor 0 indica que el nodo no tiene configurado un voto de cuórum. El valor 1 indica que el voto de cuórum del nodo está asignado y que está administrado por el clúster. Para obtener más información sobre la administración de votos de nodos, consulta [Administración de quórum dinámico](#dynamic-quorum-management) más adelante en este tema.

Comprueba la asignación de votos de todos los nodos de clúster mediante la prueba de validación **Validar quórum de clúster**.

#### <a name="additional-considerations-for-node-vote-assignment"></a>Consideraciones adicionales para la asignación de votos de nodo

  - No uses la asignación de votos de nodos para obligar a usar un número impar de nodos de votación. En lugar de ello, configura un testigo de disco o un testigo de recurso compartido de archivos. Para obtener más información, consulte [configuración de testigos](#witness-configuration) más adelante en este tema.
  - Si se habilita la administración de cuórum dinámico, solo se pueden asignar o quitar de forma dinámica los nodos que estén configurados para tener votos de nodos asignados. Para obtener más información, consulta [Administración de quórum dinámico](#dynamic-quorum-management) más adelante en este tema.

### <a name="dynamic-quorum-management"></a>Administración dinámica de cuórum

En Windows Server 2012, como una opción de configuración avanzada de cuórum, puede elegir habilitar la administración de quórum dinámico por clúster. Para obtener más información acerca del quórum dinámica, consulte [esta explicación](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Con la administración de cuórum dinámico también se puede ejecutar un clúster en el último nodo de clúster superviviente. Al ajustar de forma dinámica el requisito de mayoría del cuórum, el clúster puede sostener apagados de nodo secuenciales en un nodo único.

El voto dinámico asignado por el clúster de un nodo puede comprobarse con el **DynamicWeight** propiedad común del nodo de clúster mediante el uso de la [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) cmdlet de Windows PowerShell. El valor 0 indica que el nodo no tiene un voto de cuórum. El valor 1 indica que el nodo tiene un voto de cuórum.

Comprueba la asignación de votos de todos los nodos de clúster mediante la prueba de validación **Validar quórum de clúster**.

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>Consideraciones adicionales para la administración de quórum dinámico

- La administración de cuórum dinámico no permite que el clúster mantenga un error simultáneo de una mayoría de miembros con derecho a voto. Para continuar ejecutándose, el clúster siempre debe tener una mayoría del cuórum cuando un nodo se apague o produzca errores.

- Si has quitado de manera explícita el voto de un nodo, el clúster tampoco puede agregar o quitar de forma dinámica dicho voto.
- Cuando se habilita espacios de almacenamiento directo, el clúster solo puede admitir errores en los dos nodos. Esto se explica más en el [grupo de sección de quórum](../storage/storage-spaces/understand-quorum.md#poolQuorum)

## <a name="general-recommendations-for-quorum-configuration"></a>Recomendaciones generales para la configuración de cuórum

El software del clúster configura automáticamente el cuórum para un nuevo clúster según el número de nodos configurados y la disponibilidad del almacenamiento compartido. Esta suele ser la configuración de cuórum más apropiada para dicho clúster. Pero es buena idea revisar la configuración de cuórum después de crear el clúster y antes de usar el clúster en producción. Para ver la configuración de quórum de clúster detallada, puede usar el Asistente para configuración, validar o [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) cmdlet de Windows PowerShell, para ejecutar el **validar configuración de quórum** probar. En el Administrador de clústeres de conmutación por error, la configuración de quórum básica se muestra en la información de resumen para el clúster seleccionado, o puede revisar la información sobre los recursos del quórum que se devuelve al ejecutar el [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Cmdlet de Windows PowerShell.

En cualquier momento, puedes ejecutar la prueba **Validar configuración de quórum** para comprobar que la configuración de quórum es óptima para el clúster. La salida de la prueba indica si es recomendable realizar cambios en la configuración de cuórum y cuáles son las opciones óptimas. Si se recomienda realizar un cambio, puedes usar el Asistente para configurar cuórum de clúster para aplicar la configuración recomendada.

Cuando el clúster esté en producción, no cambies la configuración de cuórum a menos que hayas determinado que el cambio es apropiado para el clúster. Puede que quieras cambiar la configuración de cuórum en los casos siguientes:

- Para agregar o expulsar nodos
- Para agregar o quitar almacenamiento
- En caso de errores de nodos o testigos de larga duración
- Para recuperar un clúster en un escenario de recuperación ante desastres multisitio

Para obtener más información sobre la validación de un clúster de conmutación por error, consulte [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="configure-the-cluster-quorum"></a>Configuración del cuórum de clúster

Puede configurar la configuración de quórum de clúster mediante el Administrador de clústeres de conmutación por error o los cmdlets de PowerShell de Windows de clústeres de conmutación por error.

>[!IMPORTANT]
>En general, la mejor opción es usar la configuración de cuórum recomendada por el Asistente para configurar cuórum de clúster. Solo se recomienda personalizar la configuración de cuórum si consideras que el cambio es adecuado para el clúster. Para obtener más información, consulta [Recomendaciones generales para la configuración de quórum](#general-recommendations-for-quorum-configuration) más adelante en este tema.

### <a name="configure-the-cluster-quorum-settings"></a>Configurar las opciones de cuórum de clúster

Para completar este procedimiento se requiere, como mínimo, pertenecer al grupo de **administradores** locales en todos los servidores en clúster, o equivalente. Además, la cuenta que uses debe ser una cuenta de usuario de dominio.

>[!NOTE]
>La configuración de cuórum de clúster se puede cambiar sin que sea necesario detener el clúster o desconectar los recursos de clúster.

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>Cambiar la configuración de quórum en un clúster de conmutación por error mediante el Administrador de clústeres de conmutación por error

1. En el Administrador de clústeres de conmutación por error, selecciona o especifica lo que quieras cambiar.
2. Con el clúster seleccionado, en **acciones**, seleccione **más acciones**y, a continuación, seleccione **configurar opciones de quórum de clúster**. Se abrirá el Asistente para configurar cuórum de clúster. Selecciona **Siguiente**.
3. En la página **Seleccionar opción de configuración de quórum** , selecciona una de las tres opciones de configuración y completa los pasos para dicha opción. Antes de modificar la configuración del cuórum puedes revisar las opciones. Para obtener más información acerca de las opciones, consulte [información general sobre el quórum en un clúster de conmutación por error](#overview-of-the-quorum-in-a-failover-cluster), anteriormente en este tema.

    - Para permitir que el clúster restaure automáticamente las opciones de quórum que sean óptimas para la configuración del clúster, seleccione **usar configuración típica** y, a continuación, complete el asistente.
    - Para agregar o cambiar el testigo de quórum, seleccione **agregar o cambiar el testigo de quórum**y, a continuación, complete los pasos siguientes. Para obtener más información y conocer consideraciones sobre la configuración de un testigo de quórum, consulta [Configuración de testigos](#witness-configuration) anteriormente en este tema.

      1. En la página **Seleccionar testigo de quórum** , selecciona una opción para configurar un testigo de disco o un testigo de recurso compartido de archivos. El asistente indica las opciones de selección de testigo recomendadas para el clúster.

          >[!NOTE]
          >También puedes seleccionar **No configurar un testigo de quórum** y completar el asistente después. No se recomienda tener un número par de nodos de votación en el clúster.

      2. Si seleccionas la opción para configurar un testigo de disco, en la página **Configurar testigo de almacenamiento** , selecciona el volumen de almacenamiento que quieras asignar como el testigo de disco y, después, completa el asistente.
      3. Si seleccionas la opción para configurar un testigo de recurso compartido de archivos, en la página **Configurar testigo del recurso compartido de archivos**, escribe o explora hasta un recurso compartido de archivos que quieras usar como el recurso de testigo y, después, completa el asistente.

    - Para configurar las opciones de administración de quórum y para agregar o cambiar el testigo de quórum, seleccione **avanzado de selección de configuración y el testigo de quórum**y, a continuación, complete los pasos siguientes. Para obtener información y consideraciones sobre la configuración avanzada de cuórum, consulte [Asignación de votos de nodos](#node-vote-assignment) y [Administración dinámica de cuórum](#dynamic-quorum-management) anteriormente en este tema.

      1. En la página **Seleccionar configuración de votación**, selecciona una opción para asignar votos a nodos. De manera predeterminada, todos los nodos tienen asignado un voto. Pero en algunos escenarios puedes asignar votos a un subconjunto específico de nodos.

          >[!NOTE]
          >También puedes seleccionar la opción **Ningún nodo**. Esta opción no suele recomendarse, ya que no permite que los nodos participen en la votación de cuórum y, además, requiere configurar un testigo de disco. Este testigo de disco se convierte en el único punto de error del clúster.

      2. En la página **Configurar administración de quórum** puedes habilitar o deshabilitar la opción **Permitir que el clúster administre dinámicamente la asignación de votos de nodos**. Al seleccionar esta opción, se aumenta la disponibilidad del clúster. Esta opción está seleccionada de manera predeterminada y no se recomienda deshabilitarla. Esta opción permite que el clúster continúe ejecutándose en escenarios de error que no son posibles cuando esta opción está deshabilitada.
      3. En la página **Seleccionar testigo de quórum** , selecciona una opción para configurar un testigo de disco o un testigo de recurso compartido de archivos. El asistente indica las opciones de selección de testigo recomendadas para el clúster.

          >[!NOTE]
          >También puedes seleccionar **No configurar un testigo de quórum**y completar el asistente después. No se recomienda tener un número par de nodos de votación en el clúster.

      4. Si seleccionas la opción para configurar un testigo de disco, en la página **Configurar testigo de almacenamiento** , selecciona el volumen de almacenamiento que quieras asignar como el testigo de disco y, después, completa el asistente.
      5. Si seleccionas la opción para configurar un testigo de recurso compartido de archivos, en la página **Configurar testigo del recurso compartido de archivos**, escribe o explora hasta un recurso compartido de archivos que quieras usar como el recurso de testigo y, después, completa el asistente.

4. Selecciona **Siguiente**. Confirme las selecciones en la página de confirmación que aparece y, a continuación, seleccione **siguiente**.

Una vez que se ejecuta el asistente y el **resumen** aparece la página, si desea ver un informe de las tareas que realizó el asistente, seleccione **Ver informe**. El informe más reciente permanecerá en el *systemroot ***\\clúster\\informes** carpeta con el nombre **QuorumConfiguration.mht**.

>[!NOTE]
>Después de configurar el quórum de clúster, ejecuta la prueba **Validar configuración de quórum** para comprobar la configuración de quórum actualizada.

### <a name="windows-powershell-equivalent-commands"></a>Comandos equivalentes de Windows PowerShell

Los ejemplos siguientes muestran cómo usar el [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) cmdlet como otros cmdlets de Windows PowerShell para configurar el quórum de clúster.

En el ejemplo siguiente se cambia la configuración de quórum en el clúster *CONTOSO-FC1* a una configuración de quórum de mayoría de nodo sencilla sin testigo de quórum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

En el ejemplo siguiente se cambia la configuración de cuórum en el clúster local a una mayoría de nodo con configuración de testigo. El recurso de disco llamado *Disco de clúster 2* se configura como un testigo de disco.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

En el ejemplo siguiente se cambia la configuración de cuórum en el clúster local a una mayoría de nodo con configuración de testigo. El recurso compartido de archivos denominado  *\\ \\CONTOSO FS\\fsw* está configurado como un testigo de recurso compartido de archivos.

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

>[!NOTE]
> * Si el servicio de clúster se detiene porque se pierde el cuórum, se mostrará el evento id. 1177 en el registro del sistema.
> * Es importante investigar por qué se ha perdido el cuórum de clúster.
> * Se recomienda poner un nodo o un testigo de cuórum en un estado correcto (unido al clúster) en lugar de iniciar el clúster sin un cuórum.

### <a name="force-start-cluster-nodes"></a>Forzar inicio de nodos de clúster

Después de determinar que no se puede recuperar el clúster si se ponen los nodos o el testigo de cuórum en un estado correcto, es necesario forzar el inicio del clúster. Al forzar el inicio del clúster, se invalidan las opciones de configuración de quórum de clúster y se inicia el clúster en el modo **ForceQuorum**.

Forzar el inicio de un clúster cuando no tenga cuórum puede ser especialmente útil en un clúster multisitio. Imagina un escenario de recuperación ante desastres con un clúster que contenga un sitio primario y un sitio de copia de seguridad en ubicaciones separadas (*SitioA* y *SitioB*). Si se produce un desastre únicamente en el *SitioA*, podría necesitarse una gran cantidad de tiempo para volver a poner en línea el sitio. Puede que quieras obligar a que el *SitioB* esté en línea, incluso aunque no tenga quórum.

Cuando se inicia un clúster con el modo **ForceQuorum** , y después de que vuelva a obtener votos de quórum suficientes, el clúster dejará automáticamente el estado forzado y su comportamiento volverá a ser normal. Por lo tanto, no será necesario volver a iniciar el clúster normalmente. Si el clúster pierde un nodo y también cuórum, volverá a estar sin conexión, ya que no estará en el estado forzado. Para ponerlo en línea cuando no tiene quórum requiere forzar el clúster se inicie sin quórum.

>[!IMPORTANT]
>* Después de forzar el inicio de un clúster, el administrador tendrá control total sobre el clúster.
>* El clúster usa la configuración del clúster en el nodo donde el clúster se encuentra en estado forzado y lo replicará al resto de nodos que estén disponibles.
>* Si obligas al nodo a que se inicie sin quórum, se ignorarán todas las opciones de configuración de quórum cuando el clúster esté en el modo **ForceQuorum**. Esto incluye asignaciones de votos de nodos específicas y opciones de administración de cuórum dinámico.

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>Evitar cuórum en nodos de clúster restantes

Después de obligar a que el clúster se inicie en un nodo, es necesario iniciar el resto de nodos del clúster con una opción para evitar el cuórum. Los nodos iniciados con una opción que evite el cuórum indican al servicio de clúster que se una a un clúster que se esté ejecutando, en lugar de formar una nueva instancia de clúster. Esto evita que los nodos restantes formen un clúster dividido que contenga dos instancias paralelas.

Esto es necesario para recuperar el clúster en algunos escenarios de recuperación ante desastres multisitio después de forzar el inicio del clúster en el sitio de copia de seguridad ( *SitioB*). Para unirse al clúster iniciado de manera forzosa en el *SitioB*, es necesario iniciar los nodos los nodos del sitio principal (*SitioA*) sin permitir el quórum.

>[!IMPORTANT]
>Después de iniciar de manera forzosa un clúster en un nodo, se recomienda iniciar siempre el resto de nodos sin permitir el cuórum.

Aquí le mostramos cómo recuperar el clúster con el Administrador de clústeres de conmutación por error:

1. En el Administrador de clústeres de conmutación por error, selecciona o especifica el clúster que quieras recuperar.
2. Con el clúster seleccionado, en **acciones**, seleccione **Forzar inicio de clúster**.

    El Administrador de clústeres de conmutación por error forzará el inicio del clúster en todos los nodos que estén accesibles. El clúster usa la configuración del clúster actual al iniciarse.

>[!NOTE]
>* Para forzar al clúster a iniciarse en un nodo específico que contiene una configuración de clúster que desea usar, debe usar los cmdlets de Windows PowerShell o las herramientas de línea de comandos equivalentes tal como se presenta después de este procedimiento. 
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

<table>
<thead>
<tr class="header">
<th>Elemento</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Número de votos de nodos por sitio</td>
<td>Debe ser igual</td>
</tr>
<tr class="even">
<td>Asignación de votos de nodos</td>
<td>No deben eliminarse los votos de nodos, ya que todos los nodos tienen la misma importancia</td>
</tr>
<tr class="odd">
<td>Administración dinámica de cuórum</td>
<td>Debe estar habilitado</td>
</tr>
<tr class="even">
<td>Configuración de testigos</td>
<td>Se recomienda el testigo de recurso compartido de archivos, configurado en un sitio separado de los sitios del clúster</td>
</tr>
<tr class="odd">
<td>Cargas de trabajo</td>
<td>Las cargas de trabajo se pueden configurar en cualquiera de los sitios</td>
</tr>
</tbody>
</table>

#### <a name="additional-considerations-for-automatic-failover"></a>Consideraciones adicionales para la conmutación automática por error

- Es necesario configurar el testigo de recurso compartido de archivos en un sitio separado para que todos los sitios tengan las mismas oportunidades de supervivencia. Para obtener más información, consulta [Configuración de testigos](#witness-configuration) anteriormente en este tema.

### <a name="manual-failover"></a>Conmutación por error manual

En esta configuración, el clúster está formado por un sitio principal, el *SitioA*, y un sitio de copia de seguridad (recuperación), el *SitioB*. Los roles agrupados en clúster están hospedados en el *SitioA*. Debido a la configuración de quórum de clúster, si se produce un error en todos los nodos del *SitioA*, el clúster dejará de funcionar. En este escenario, el administrador deberá conmutar por error de forma manual los servicios de clúster al *SitioB* y realizar pasos adicionales para recuperar el clúster.

En la tabla siguiente se resumen las consideraciones y recomendaciones para esta configuración.

<table>
<thead>
<tr class="header">
<th>Elemento</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Número de votos de nodos por sitio</td>
<td>Pueden ser distintos</td>
</tr>
<tr class="even">
<td>Asignación de votos de nodos</td>
<td>- Los votos de nodos no deben eliminarse de los nodos en el sitio primario, <em>SitioA</em><br />
- Los votos de nodos deben quitarse de los nodos en el sitio de copia de seguridad, <em>SitioB</em><br />
- Si se produce una interrupción de larga duración en <em>SitioA</em>, los votos deben asignarse a los nodos de <em>SitioB</em> para habilitar una mayoría del quórum en ese sitio como parte de la recuperación</td>
</tr>
<tr class="odd">
<td>Administración dinámica de cuórum</td>
<td>Debe estar habilitado</td>
</tr>
<tr class="even">
<td>Configuración de testigos</td>
<td>- Configurar un testigo si hay un número par de nodos en <em>SitioA</em><br />
- Si se necesita un testigo, configure un testigo del recurso compartido de archivos o un testigo de disco que es accesible sólo a los nodos en <em>SitioA</em> (también conocido como un testigo de disco asimétrico)</td>
</tr>
<tr class="odd">
<td>Cargas de trabajo</td>
<td>Usa los propietarios preferidos para que las cargas de trabajo sigan ejecutándose en los nodos de <em>SitioA</em></td>
</tr>
</tbody>
</table>

#### <a name="additional-considerations-for-manual-failover"></a>Consideraciones adicionales para la conmutación por error manual

- Solo los nodos del *SitioA* se configuran inicialmente con votos de quórum. Esto es necesario para garantizar que el estado de los nodos en el *SitioB* no afecte al quórum de clúster.
- Los pasos de recuperación pueden variar según si el *SitioA* puede admitir un error temporal o un error de larga duración.

## <a name="more-information"></a>Más información

* [Agrupación en clústeres de conmutación por error](failover-clustering.md)
* [Cmdlets de PowerShell de Windows de clústeres de conmutación por error](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Clúster de comprensión y Cuórum de grupo](../storage/storage-spaces/understand-quorum.md)