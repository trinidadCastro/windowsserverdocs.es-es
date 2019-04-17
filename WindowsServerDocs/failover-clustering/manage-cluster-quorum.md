---
title: Configurar y administrar el cuórum en un clúster de conmutación por error
description: Obtener información detallada sobre cómo administrar el cuórum de clúster en un clúster de conmutación por error de Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21f99362205e0a6aa90ebd26cef8f3a779bdc1dc
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024001"
---
# Configurar y administrar el cuórum

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona en segundo plano y los pasos para configurar y administrar el cuórum en un clúster de conmutación por error de Windows Server.

## Cuórum conocimiento

El cuórum para un clúster viene determinado por el número de elementos voto que debe formar parte de pertenencia al clúster active para ese clúster para iniciar correctamente o seguir ejecutándose. Para obtener una explicación más detallada, vea la [Descripción de documento de cuórum de clúster y grupo](../storage/storage-spaces/understand-quorum.md).

## Opciones de configuración de cuórum

El modelo de cuórum en Windows Server es flexible. Si es necesario modificar la configuración de cuórum de clúster, puedes usar el Asistente para configurar Cuórum de clúster o los cmdlets de PowerShell de Windows de clústeres de conmutación por error. Para que pasos y consideraciones para configurar el cuórum, consulta [Configurar el cuórum de clúster](#configure-the-cluster-quorum) más adelante en este tema.

La siguiente tabla enumera las opciones de configuración de tres cuórum que están disponibles en el Asistente para configurar Cuórum de clúster.

<table>
<thead>
<tr class="header">
<th>Opción</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Configuración típica de uso</td>
<td>El clúster asigna automáticamente un voto a cada nodo y administra dinámicamente los votos nodo. Si es apropiado para el clúster y hay almacenamiento compartido del clúster, el clúster selecciona a un testigo de disco. Se recomienda esta opción en la mayoría de los casos, porque el software de clúster elige automáticamente una configuración de cuórum y testigo proporciona la mayor disponibilidad para el clúster.</td>
</tr>
<tr class="even">
<td>Agregar o cambiar al testigo de cuórum</td>
<td>Puedes agregar, cambiar o eliminar un testigo de recurso. Puedes configurar a un testigo de recurso compartido o el disco. El clúster asigna automáticamente un voto a cada nodo y administra dinámicamente los votos nodo.</td>
</tr>
<tr class="odd">
<td>Selección de testigo y la configuración de cuórum avanzada</td>
<td>Debe seleccionar esta opción solo cuando tiene requisitos específicos de la aplicación o específicos para configurar el cuórum. Puedes modificar al testigo de cuórum, agregar o quitar votos de nodo y elegir si el clúster administra dinámicamente votos de nodo. De manera predeterminada, votos se asignan a todos los nodos y los votos nodo se administran de forma dinámica.</td>
</tr>
</tbody>
</table>

Dependiendo de la opción de configuración de cuórum que elijas y su configuración específica, el clúster se configurarán en uno de los modos de cuórum siguientes:

<table>
<thead>
<tr class="header">
<th>Modo</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Mayoría de nodo (ningún testigo)</td>
<td>Solo los nodos tienen votos. No se ha configurado ningún testigo de cuórum. El cuórum de clúster es la mayoría de los nodos de voto de la pertenencia al clúster active.</td>
</tr>
<tr class="even">
<td>Mayoría de nodo con testigo (disco o archivo compartido)</td>
<td>Los nodos tienen votos. Además, un testigo tiene un voto. El cuórum de clúster es la mayoría de los nodos voto en la pertenencia al clúster active además un voto de testigo.<br />
<br />
Puede ser un testigo de un testigo de disco designado o un testigo de recurso compartido de archivo designado.</td>
</tr>
<tr class="odd">
<td>Sin mayoría (solo testigo de disco)</td>
<td>No hay nodos tienen votos. Solo un testigo de disco tiene un voto. El cuórum de clúster viene determinado por el estado del testigo de disco.<br />
<br />
El clúster tiene cuórum si un nodo es y la comunicación con un disco específico en el almacenamiento de clúster. Por lo general, no se recomienda este modo, y no deben seleccionarse porque se creará un único punto de error para el clúster.</td>
</tr>
</tbody>
</table>

Las siguientes secciones te proporcionará más información sobre las opciones de configuración de cuórum avanzadas.

### Configuración de testigos

Como regla general al configurar un cuórum, los elementos voto del clúster deben ser un número impar. Por lo tanto, si el clúster contiene un número par de voto nodos, debes configurar un testigo de disco o un testigo del recurso compartido de archivos. El clúster podrán mantener un nodo adicional hacia abajo. Además, agregar un voto de testigo permite el clúster para seguir funcionando si mitad de los nodos de clúster al mismo tiempo bloquearse o está desconectada.

Por lo general, se recomienda un testigo de disco si todos los nodos de ver el disco. Se recomienda un testigo del recurso compartido de archivos al que debes tener en cuenta la recuperación ante desastres multisitio con almacenamiento replicado. Configurar a un testigo de disco con almacenamiento replicado es posible solo si el proveedor de almacenamiento admite el acceso de lectura y escritura de todos los sitios en el almacenamiento replicado. <strong>*Un testigo de disco no es compatible con espacios de almacenamiento directo*</strong>.

La siguiente tabla proporciona información adicional y consideraciones acerca de los tipos de testigo de cuórum.

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
<td>- LUN dedicada que almacena una copia de la base de datos de clúster<br />
- Muy útil para los clústeres con almacenamiento compartido de (no replicado)</td>
<td>- Tamaño de LUN debe ser al menos 512 MB<br />
- Debes dedicada al uso del clúster y no se asigna a un rol en el clúster<br />
- Deben incluirse en clústeres pruebas de validación de almacenamiento de almacenamiento y pasa<br />
- No puede ser un disco que es un volumen de compartidos de clúster (CSV)<br />
- Disco básico con un único volumen<br />
- No es necesario tener una letra de unidad<br />
- Puede estar formateada con NTFS o ReFS<br />
- Pueden configurarse con RAID de hardware para la tolerancia a errores<br />
- Se deben excluir de las copias de seguridad y el análisis antivirus<br />
- Un testigo de disco no es compatible con espacios de almacenamiento directo</td>
</tr>
<tr class="even">
<td>Testigo del recurso compartido de archivos</td>
<td>- Recurso compartido de archivos SMB que está configurada en un servidor de archivos que ejecutan Windows Server<br />
- No almacena una copia de la base de datos de clúster<br />
- Mantiene la información de clúster únicamente en un archivo witness.log<br />
- Muy útil para los multisitio clústeres con almacenamiento replicado</td>
<td>- Debe tener un mínimo de 5 MB de espacio libre<br />
- Debes dedicada al clúster único y no se usa para almacenar datos de usuario o la aplicación<br />
- Deben habilitado para que el objeto de equipo para el nombre del clúster permisos de escritura<br />
<br />
Las siguientes son consideraciones adicionales para un servidor de archivos que hospeda el testigo del recurso compartido de archivos:<br />
<br />
- Un servidor de archivos solo se puede configurar con testigos de recurso compartido de archivo para varios clústeres.<br />
- El servidor de archivos debe estar en un sitio que es independiente de la carga de trabajo de clúster. Esto permite igualdad de oportunidades para los sitios de clúster sobrevivir a si se pierde la comunicación de red de sitio a sitio. Si el servidor de archivos está en el mismo sitio, ese sitio se convierte en el sitio primario y es el único sitio que puede alcanzar el recurso compartido de archivos.<br />
- El servidor de archivos puede ejecutar en una máquina virtual si la máquina virtual no se hospeda en el mismo clúster que usa el testigo del recurso compartido de archivos.<br />
- Alta disponibilidad, se puede configurar el servidor de archivos en un clúster de conmutación por error independiente.</td>
</tr>

<tr class-"odd">
<td>Testigo en la nube</td>
<td>- Un archivo de testigo almacenado en el almacenamiento de blobs de Azure<br>
-Recomendada cuando todos los servidores del clúster tienen una conexión confiable de Internet.</td>
<td>Consulta <a href="https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness">implementar un testigo en la nube</a>.</td>
<td>
</td>
</tr>

</tbody>
</table>

### Asignación del nodo de voto

Como una opción de configuración de cuórum avanzados, puedes asignar o quitar votos cuórum de forma por nodo. De manera predeterminada, todos los nodos se asignan votos. Independientemente de la asignación de voto, todos los nodos seguirán funcionando en el clúster, recibir actualizaciones de la base de datos de clúster y pueden alojar las aplicaciones.

Es posible que quieras quitar votos de nodos en determinadas configuraciones de recuperación ante desastres. Por ejemplo, en un clúster multisitio, podrías quitar votos desde los nodos de un sitio de copia de seguridad para que los nodos no afectan a los cálculos de cuórum. Se recomienda esta configuración solo para conmutación por error manual entre sitios. Para obtener más información, consulta [las consideraciones de Cuórum para las configuraciones de recuperación ante desastres](#quorum-considerations-for-disaster-recovery-configurations) más adelante en este tema.

Puede comprobar el voto de un nodo configurado consultando la propiedad común de **NodeWeight** del nodo de clúster mediante el cmdlet de Windows PowerShell de [Get-ClusterNode](http://technet.microsoft.com/library/hh847268.aspx). Un valor de 0 indica que el nodo no tiene un voto de cuórum configurado. Un valor de 1 indica que se le asigna el voto de cuórum del nodo y está administrado por el clúster. Para obtener más información sobre la administración de nodo votos, consulta [administración dinámica cuórum](#dynamic-quorum-management) más adelante en este tema.

La asignación de voto para todos los nodos del clúster puede comprobarse mediante el uso de la prueba de validación de **Validar Cuórum de clúster** .

#### Consideraciones adicionales para la asignación del nodo de voto

  - Asignación del nodo de voto no se recomienda para aplicar un número impar de nodos voto. En su lugar, debes configurar un testigo de disco o un testigo de recurso compartido. Para obtener más información, consulta la [configuración de testigos](#witness-configuration) más adelante en este tema.
  - Si se habilita la administración de cuórum dinámico, solo los nodos que están configurados para tener votos nodo asignados pueden tener sus votos asignado o quitado de forma dinámica. Para obtener más información, consulta [administración dinámica cuórum](#dynamic-quorum-management) más adelante en este tema.

### Administración de cuórum dinámico

En Windows Server 2012, como una opción de configuración de cuórum avanzados, puedes elegir habilitar la administración de cuórum dinámica por clúster. Para obtener más información sobre cómo dinámica funciona de cuórum, consulta [esta explicación](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Con la administración de cuórum dinámico, también es posible para un clúster que se ejecute en el último nodo del clúster que queda. Ajustando dinámicamente el requisito de la mayoría de cuórum, el clúster puede admitir apagados secuencial nodo a un nodo único.

Puede comprobar el voto dinámico asignado por el clúster de un nodo con la propiedad común **DynamicWeight** del nodo de clúster mediante el cmdlet de Windows PowerShell de [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) . Un valor de 0 indica que el nodo no tiene un voto de cuórum. Un valor de 1 indica que el nodo tiene un voto de cuórum.

La asignación de voto para todos los nodos del clúster puede comprobarse mediante el uso de la prueba de validación de **Validar Cuórum de clúster** .

#### Consideraciones adicionales para la administración de cuórum dinámico

- Administración de cuórum dinámico no permite que el clúster para mantener un error de la mayoría de los miembros voto de simultáneos. Para continuar con la ejecución, el clúster siempre debe tener la mayoría de cuórum en el momento de un error o el apagado de nodo.

- Si se ha quitado explícitamente el voto de un nodo, el clúster dinámicamente no se puede agregar o quitar ese voto.
- Cuando se habilita espacios de almacenamiento directo, el clúster puede admitir solo dos errores en nodos. Esto se explica más en la [sección de cuórum de grupo](../storage/storage-spaces/understand-quorum.md#poolQuorum)

## Recomendaciones generales para la configuración de cuórum

El software del clúster configura automáticamente el cuórum en un clúster nuevo, según el número de nodos configurados y la disponibilidad de almacenamiento compartido. Por lo general, suele ser la configuración de cuórum más apropiada para dicho clúster. Sin embargo, es una buena idea para revisar la configuración de cuórum después de crea el clúster, antes de colocar el clúster en producción. Para ver la configuración de cuórum de clúster detallada, se puede usar el validar un asistente de configuración o el cmdlet [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) Windows PowerShell, para ejecutar la prueba de **Validar la configuración de Cuórum** . En el Administrador de clústeres de conmutación por error, la configuración de cuórum básica se muestra en la información de resumen para el clúster seleccionado, o puedes revisar la información acerca de los recursos de cuórum que se devuelve cuando se ejecuta [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Windows PowerShell cmdlet.

En cualquier momento, puedes ejecutar la prueba de **Validar la configuración de Cuórum** para validar que la configuración de cuórum es óptima para el clúster. El resultado de la prueba indica si se recomienda un cambio en la configuración de cuórum y las opciones de configuración que son las óptimas. Si se recomienda un cambio, puedes usar al Asistente para configurar Cuórum de clúster para aplicar la configuración recomendada.

Una vez que el clúster esté en producción, no cambie la configuración de cuórum, a menos que hayas determinado que el cambio es apropiado para el clúster. Es posible que quieras considera la posibilidad de cambiar la configuración de cuórum en las siguientes situaciones:

- Agregar o expulsar nodos
- Agregar o quitar el almacenamiento
- Un error de nodo o un testigo a largo plazo
- Recuperar un clúster en un escenario de recuperación ante desastres multisitio

Para obtener más información sobre la validación de un clúster de conmutación por error, consulta [Validar Hardware para un clúster de conmutación por error](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Configurar el cuórum de clúster

Puedes configurar la configuración de cuórum de clúster mediante el Administrador de clústeres de conmutación por error o los cmdlets de PowerShell de Windows de clústeres de conmutación por error.

>[!IMPORTANT]
>Es preferible utilizar la configuración de cuórum que se recomienda por el Asistente para configurar Cuórum de clúster. Se recomienda personalizar la configuración de cuórum solo si ha determinado que el cambio es apropiado para el clúster. Para obtener más información, consulta [las recomendaciones generales para la configuración de cuórum](#general-recommendations-for-quorum-configuration) en este tema.

### Configurar la configuración de cuórum de clúster

Pertenencia a grupos en el grupo de **administradores** local en cada servidor en clúster, o equivalente, es los permisos mínimos necesarios para completar este procedimiento. Además, la cuenta que usas debe ser una cuenta de usuario de dominio.

>[!NOTE]
>Puedes cambiar la configuración de cuórum de clúster sin detener el clúster o toma de recursos del clúster sin conexión.

### Cambiar la configuración de cuórum en un clúster de conmutación por error mediante el Administrador de clústeres de conmutación por error

1. En el Administrador de clústeres de conmutación por error, selecciona o especifique el clúster que quieres cambiar.
2. Con el clúster seleccionado, debajo de **acciones**, seleccione **Más acciones**y, a continuación, selecciona **Configurar opciones de configuración de Cuórum de clúster**. Aparece el Asistente para configurar Cuórum de clúster. Selecciona **Siguiente**.
3. En la página **Seleccionar la opción de configuración de Cuórum** , selecciona una de las tres opciones de configuración y realizar los pasos para esa opción. Antes de configurar las opciones de configuración de cuórum, puedes revisar las opciones. Para obtener más información acerca de las opciones, consulta [información general sobre el cuórum en un clúster de conmutación por error](#overview-of-the-quorum-in-a-failover-cluster), anteriormente en este tema.

    - Para permitir que el clúster restablecer automáticamente la configuración de cuórum que es las óptima para la configuración actual del clúster, selecciona la **Configuración típica de uso** y, a continuación, completa al asistente.
    - Para agregar o cambiar al testigo de cuórum, selecciona **Agregar o cambiar el testigo de cuórum**y, a continuación, realiza los siguientes pasos. Para obtener información y consideraciones acerca de cómo configurar a un testigo de cuórum, consulta la [configuración de testigos](#witness-configuration) anteriormente en este tema.

      1. En la página **Seleccionar testigo de Cuórum** , selecciona una opción para configurar un testigo de disco o un testigo del recurso compartido de archivos. El asistente indica las opciones de selección de testigo que se recomiendan para el clúster.

          >[!NOTE]
          >Puedes también seleccionar **no configure un testigo** y, a continuación, completa al asistente. Si tienes un número par de voto nodos en el clúster, esto puede no ser una configuración recomendada.

      2. Si seleccionas la opción para configurar a un testigo de disco, en la página **Configurar un testigo de almacenamiento** , seleccione el volumen de almacenamiento que quieres asignar como el testigo de disco y luego complete el asistente.
      3. Si seleccionas la opción para configurar a un testigo de recurso compartido de archivo, en la página **Configurar testigo de recurso compartido** , escribe o busca en un recurso compartido de archivo que se usará como el recurso testigo y, a continuación, completa al asistente.

    - Para configurar las opciones de administración de cuórum así como para agregar o cambiar al testigo de cuórum, seleccione **la configuración de cuórum avanzadas y selección de testigo**y, a continuación, realiza los siguientes pasos. Para obtener información y consideraciones acerca de las opciones de configuración de cuórum avanzada, consulta [nodo votar asignación](#node-vote-assignment) y [administración de cuórum dinámico](#dynamic-quorum-management) anteriormente en este tema.

      1. En la página **Seleccionar configuración voto** , selecciona una opción para asignar votos a nodos. De manera predeterminada, todos los nodos se asignan un voto. Sin embargo, para determinados escenarios, puedes asignar votos únicamente a un subconjunto de los nodos.

          >[!NOTE]
          >También puedes seleccionar **No nodos**. Esto por lo general, no se recomienda, porque no permite nodos participe en el voto de cuórum, y requiere configurar a un testigo de disco. Este testigo de disco se convierte en el único punto de error para el clúster.

      2. En la página **Configurar administración de Cuórum** , puedes habilitar o deshabilitar la opción **Permitir clúster para administrar dinámicamente la asignación de nodo vota** . Esta opción por lo general, aumenta la disponibilidad del clúster. De manera predeterminada, la opción está habilitada y se recomienda encarecidamente no deshabilitar esta opción. Esta opción permite que el clúster siga ejecutándose en escenarios de error que no son posibles cuando esta opción está deshabilitada.
      3. En la página **Seleccionar testigo de Cuórum** , selecciona una opción para configurar un testigo de disco o un testigo del recurso compartido de archivos. El asistente indica las opciones de selección de testigo que se recomiendan para el clúster.

          >[!NOTE]
          >Puedes también seleccionar **no configure un testigo**y, a continuación, completa al asistente. Si tienes un número par de voto nodos en el clúster, esto puede no ser una configuración recomendada.

      4. Si seleccionas la opción para configurar a un testigo de disco, en la página **Configurar un testigo de almacenamiento** , seleccione el volumen de almacenamiento que quieres asignar como el testigo de disco y luego complete el asistente.
      5. Si seleccionas la opción para configurar a un testigo de recurso compartido de archivo, en la página **Configurar testigo de recurso compartido** , escribe o busca en un recurso compartido de archivo que se usará como el recurso testigo y, a continuación, completa al asistente.

4. Selecciona **Siguiente**. Confirma las opciones seleccionadas en la página de confirmación que aparece y, a continuación, seleccione **siguiente**.

Después de que se ejecuta el asistente y aparece la página de **Resumen** , si quieres ver un informe de las tareas que realizadas por el asistente, selecciona **Ver el informe**. El informe más reciente permanecerá en la *raízDelSistema *** \\Cluster\\Reports** carpeta con el nombre **QuorumConfiguration.mht**.

>[!NOTE]
>Después de configurar el cuórum de clúster, te recomendamos que ejecute la prueba de **Validar la configuración de Cuórum** para comprobar la configuración de cuórum actualizados.

### Comandos equivalentes de Windows PowerShell

Los siguientes ejemplos muestran cómo usar el cmdlet [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) y otros cmdlets de Windows PowerShell para configurar el cuórum de clúster.

En el ejemplo siguiente se cambia la configuración de cuórum de clúster *CONTOSO FC1* a una configuración de la mayoría de nodo simple con ningún testigo de cuórum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

En el ejemplo siguiente se cambia la configuración de cuórum en el clúster local para la mayoría de nodo con la configuración de testigos. El recurso de disco denominado *2 de disco de clúster* está configurado como un testigo de disco.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

En el ejemplo siguiente se cambia la configuración de cuórum en el clúster local para la mayoría de nodo con la configuración de testigos. El recurso compartido de archivos denominado *\\\CONTOSO-FS\\fsw* está configurado como un testigo del recurso compartido de archivos.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

En el ejemplo siguiente se quita el voto de cuórum de nodo *ContosoFCNode1* en el clúster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

En el siguiente ejemplo se agrega el voto de cuórum a nodos *ContosoFCNode1* en el clúster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

En el ejemplo siguiente, se permite la propiedad **DynamicQuorum** del clúster *CONTOSO FC1* (si se había deshabilitado):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## Recuperar un clúster iniciando sin cuórum

No se iniciará un clúster que no tiene suficiente votos cuórum. Como primer paso, siempre debe confirmar la configuración de cuórum de clúster e investigar por qué el clúster ya no tiene cuórum. Esto puede suceder si tienes nodos que dejó de responder, o si el sitio primario no es accesible en un clúster multisitio. Después de identificar la causa del error de clúster, puedes usar los pasos de recuperación se describe en esta sección.

>[!NOTE]
> * Si se detiene el servicio de clúster porque cuórum se pierde, 1177 de Id. de evento aparece en el registro del sistema.
> * Siempre es necesario investigar por qué el cuórum de clúster en estado perdido.
> * Siempre es preferible para conectar a un testigo de cuórum o de nodo a un estado correcto (unirse al clúster) en lugar de a partir del clúster sin cuórum.

### Nodos de clúster de inicio de fuerza

Después de determinar que no se puede recuperar el clúster al llevar a los nodos o un testigo de cuórum a un estado correcto, forzar el clúster para iniciar sea necesario. Forzar el clúster para iniciar invalida las opciones de configuración de cuórum de clúster e inicia el clúster en modo de **ForceQuorum** .

Obligar a un clúster para iniciar cuando no es necesario cuórum puede ser especialmente útil en un clúster multisitio. Considera la posibilidad de un escenario de recuperación ante desastres con un clúster que contiene por separado encuentra sitios principales y de reserva, *AccessUn* y *SitioB*. Si hay un desastre original en *AccessUn*, puede tardar una gran cantidad de tiempo para el sitio que volver a conectarse. Es probable que desee forzar *SitioB* a entrar en línea, aunque no es necesario cuórum.

Cuando se inicia un clúster en modo de **ForceQuorum** , y después recupera el cuórum suficiente vota, el clúster automáticamente sale del estado forzado y se comporta con normalidad. Por lo tanto, no es necesario iniciar el clúster nuevo con normalidad. Si el clúster pierde un nodo y pierde cuórum, es sin conexión nuevo porque ya no está en el estado forzado. Para poner en línea cuando no es necesario cuórum requiere forzar el clúster para iniciar sin cuórum.

>[!IMPORTANT]
>* Una vez un clúster se fuerza a trabajar, el administrador tiene pleno control del clúster.
>* El clúster usa la configuración del clúster en el nodo donde el clúster es fuerza a trabajar y replica a todos los otros nodos que están disponibles.
>* Si se fuerza el clúster para iniciar sin cuórum, se ignoran todas las opciones de configuración de cuórum mientras el clúster permanece en modo de **ForceQuorum** . Esto incluye las asignaciones de voto de nodo específico y configuración de administración de cuórum dinámica.

### Impedir que el cuórum en otros nodos de clúster

Una vez que tengas fuerza iniciales del clúster en un nodo, es necesario iniciar cualquier otros nodos en el clúster con una configuración para evitar cuórum. Un nodo a trabajar con una configuración que impide cuórum indica al servicio de clúster para unirse a un clúster de ejecución existente en lugar de formar una nueva instancia de clúster. Esto impide que el resto de los nodos que forman un clúster en dos paneles que contiene dos instancias de competencia.

Esto suele ser necesario cuando necesites recuperar el clúster en algunos escenarios de recuperación ante desastres multisitio una vez que tengas fuerza a trabajar el clúster en el sitio de copia de seguridad, *SitioB*. Para unir la fuerza inicia clúster en *SitioB*, los nodos del sitio principal, *AccessUn*, que se puede iniciar con el cuórum impidió.

>[!IMPORTANT]
>Una vez un clúster se fuerza a trabajar en un nodo, te recomendamos que siempre se inicie el resto de los nodos con el cuórum impidió.

Aquí te mostramos cómo recuperar el clúster con el Administrador de clústeres de conmutación por error:

1. En el Administrador de clústeres de conmutación por error, selecciona o especifique el clúster que quieras recuperar.
2. Con el clúster seleccionado, en **acciones**, selecciona **El inicio de clúster de fuerza**.

    Fuerza el Administrador de clústeres de conmutación por error, inicia el clúster en todos los nodos que son accesibles. El clúster utiliza la configuración actual del clúster cuando se inicia.

>[!NOTE]
>* Para forzar el clúster para iniciar en un nodo específico que contiene una configuración de clúster que quieres usar, debes usar los cmdlets de Windows PowerShell o herramientas de línea de comandos equivalentes como presenta después de este procedimiento. 
> * Si usas el Administrador de clústeres de conmutación por error para conectarse a un clúster de fuerza a trabajar, y se utiliza la acción de **Iniciar el servicio de clúster** para iniciar un nodo, se inicia automáticamente el nodo con la configuración que impide que cuórum.

#### Comandos equivalentes de Windows PowerShell (inicio-Clusternode)

El siguiente ejemplo muestra cómo usar el cmdlet **Start-ClusterNode** para forzar el inicio del clúster en el nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

Como alternativa, puede escribir el siguiente comando localmente en el nodo:

```PowerShell
Net Start ClusSvc /FQ
```

El siguiente ejemplo muestra cómo usar el cmdlet **Start-ClusterNode** para iniciar el servicio de clúster con el cuórum impidió en el nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

Como alternativa, puede escribir el siguiente comando localmente en el nodo:

```PowerShell
Net Start ClusSvc /PQ
```

## Consideraciones de cuórum para configuraciones de recuperación ante desastres

En esta sección se resume las características y configuraciones de cuórum para dos configuraciones de clúster multisitio en las implementaciones de recuperación ante desastres. Las instrucciones de configuración de cuórum difieren en función de si es necesario conmutación automática por error o conmutación por error manual para cargas de trabajo entre los sitios. Por lo general, la configuración depende de los acuerdos de nivel de servicio (SLA) que se encuentran en lugar de la organización para proporcionar y admitir cargas de trabajo en clúster en el caso de un desastre en un sitio o un error.

### Conmutación automática por error

En esta configuración, el clúster consta de dos o más sitios que pueden hospedar roles en clúster. Si se produce un error en cualquier sitio, se espera que los roles en clúster de conmutación por error automáticamente a los sitios restantes. Por lo tanto, el cuórum de clúster debe estar configurado para que cualquier sitio puede admitir un error de sitio completa.

La siguiente tabla resume las consideraciones y recomendaciones para esta configuración.

<table>
<thead>
<tr class="header">
<th>Elemento</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Número de nodo votos por sitio</td>
<td>Debe ser igual</td>
</tr>
<tr class="even">
<td>Asignación del nodo de voto</td>
<td>Nodo votos no deben eliminarse porque todos los nodos tienen la misma importancia</td>
</tr>
<tr class="odd">
<td>Administración de cuórum dinámico</td>
<td>Debe estar habilitado</td>
</tr>
<tr class="even">
<td>Configuración de testigos</td>
<td>Se recomienda testigo del recurso compartido de archivos, configurada en un sitio que es independiente de los sitios de clúster</td>
</tr>
<tr class="odd">
<td>Cargas de trabajo</td>
<td>Las cargas de trabajo pueden configurarse en cualquiera de los sitios</td>
</tr>
</tbody>
</table>

#### Consideraciones adicionales para la conmutación automática por error

- Configurar al testigo del recurso compartido de archivos en un sitio independiente es necesario proporcionar una oportunidad de igual para sobrevivir a cada sitio. Para obtener más información, consulta la [configuración de testigos](#witness-configuration) anteriormente en este tema.

### Conmutación por error manual

En esta configuración, el clúster consta de un sitio primario, *AccessUn*y un sitio *b*de copia de seguridad (recuperación). Roles en clúster se hospedan en *AccessUn*. Debido a la configuración de cuórum de clúster, si se produce un error en todos los nodos *AccessUn*, el clúster deja de funcionar. En este escenario, el administrador debe conmutar por los servicios de clúster *valor* de en manualmente y realizar pasos adicionales para recuperar el clúster.

La siguiente tabla resume las consideraciones y recomendaciones para esta configuración.

<table>
<thead>
<tr class="header">
<th>Elemento</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Número de nodo votos por sitio</td>
<td>Puede ser diferente</td>
</tr>
<tr class="even">
<td>Asignación del nodo de voto</td>
<td>- Nodo vota no debe quitarse de nodos en el sitio primario, <em>AccessUn</em><br />
- Nodo vota debe quitarse de nodos en el sitio de copia de seguridad, <em>SitioB</em><br />
- Si se produce una interrupción a largo plazo al <em>AccessUn</em>, votos deben estar asignados a nodos en <em>SitioB</em> para habilitar la mayoría de cuórum en el sitio como parte de recuperación</td>
</tr>
<tr class="odd">
<td>Administración de cuórum dinámico</td>
<td>Debe estar habilitado</td>
</tr>
<tr class="even">
<td>Configuración de testigos</td>
<td>- Configure un testigo si hay un número par de nodos en <em>AccessUn</em><br />
- Si es necesario un testigo, configurar un testigo del recurso compartido de archivos o un testigo de disco que es accesible solo en nodos <em>AccessUn</em> (también conocido como un testigo de disco asimétrico)</td>
</tr>
<tr class="odd">
<td>Cargas de trabajo</td>
<td>Usar los propietarios preferidos para mantener las cargas de trabajo que se ejecutan en los nodos en <em>AccessUn</em></td>
</tr>
</tbody>
</table>

#### Consideraciones adicionales para la conmutación por error manual

- Solo los nodos en *AccessUn* inicialmente están configurados con votos cuórum. Esto es necesario para asegurarse de que el estado de los nodos en *SitioB* no afecta el cuórum de clúster.
- Pasos de recuperación pueden variar en función de si *AccessUn* mantiene un error temporal o a largo plazo.

## Más información

* [Clústeres de conmutación por error](failover-clustering.md)
* [Cmdlets de Windows PowerShell de clústeres de conmutación por error](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Conocimiento clúster y grupo Cuórum](../storage/storage-spaces/understand-quorum.md)