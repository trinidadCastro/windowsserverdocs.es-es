---
title: Crear un clúster de conmutación por error
description: Cómo crear un clúster de conmutación por error para Windows Server 2012 R2, Windows Server 2012, Windows Server 2016 y Windows Server 2019.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 496ae061d438a429ba13ee49d9da94127f308f37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369800"
---
# <a name="create-a-failover-cluster"></a>Crear un clúster de conmutación por error

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema se muestra cómo crear un clúster de conmutación por error a través del complemento Administrador de clústeres de conmutación por error o bien a través de Windows PowerShell. Este tema se centra en una implementación típica, en la que los objetos de equipo para el clúster y sus roles en clúster asociados se crean en los Servicios de dominio de Active Directory (AD DS). Si va a implementar un clúster de Espacios de almacenamiento directo, en su lugar, vea [implementar espacios de almacenamiento directo](../storage/storage-spaces/deploy-storage-spaces-direct.md).

También puede implementar un clúster desasociado Active Directory. Este método de implementación permite crear un clúster de conmutación por error sin permisos para crear objetos de equipo en AD DS y sin necesidad de solicitar que los objetos de equipo estén preconfigurados en AD DS. Esta opción solo está disponible a través de Windows PowerShell, y se recomienda únicamente para casos específicos. Para obtener más información, consulte [Implementar un clúster desconectado de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Lista de comprobación: crear un clúster de conmutación por error

| Estado | Tarea | Referencia |
| ---    | ---  | ---       |
| ☐    | Comprobar los requisitos previos | [Comprobar los requisitos previos](#verify-the-prerequisites) |
| ☐    | Instalar la característica de clúster de conmutación por error en todos los servidores que desees incluir como nodo de clúster | [Instalar la característica de clústeres de conmutación por error](#install-the-failover-clustering-feature) |
| ☐    | Ejecutar el Asistente para validación de clústeres a fin de validar la configuración | [Validar la configuración](#validate-the-configuration) |
| ☐ | Ejecutar el Asistente para crear clúster a fin de crear el clúster de conmutación por error | [Crear el clúster de conmutación por error](#create-the-failover-cluster) |
| ☐ | Crear roles en clúster para hospedar las cargas de trabajo de clúster | [Crear roles en clúster](#create-clustered-roles) |

## <a name="verify-the-prerequisites"></a>Comprobar los requisitos previos

Antes de comenzar, comprueba los siguientes requisitos previos:

- Asegúrate de que todos los servidores que deseas agregar como nodos de clúster ejecuten la misma versión de Windows Server.
- Revisa los requisitos de hardware para asegurarte de que tu configuración sea compatible. Para obtener más información, consulta [Requisitos de hardware de los clústeres de conmutación por error y opciones de almacenamiento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Si va a crear un clúster de Espacios de almacenamiento directo, consulte [requisitos de hardware de espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Para agregar almacenamiento en clúster durante la creación del clúster, asegúrese de que todos los servidores pueden acceder al almacenamiento. (También puedes agregar el almacenamiento en clúster después de crear el clúster).
- Asegúrate de que todos los servidores que deseas agregar como nodos de clúster estén unidos al mismo dominio de Active Directory.
- (Opcional) Crea una unidad organizativa (OU) y mueve a dicha OU las cuentas de equipo para los servidores que deseas agregar como nodos de clúster. Se recomienda colocar los clústeres de conmutación por error en su propia OU en AD DS. De este modo, podrás controlar mejor qué configuración de directiva de grupo o qué configuración de plantilla de seguridad afecta a los nodos de clúster. El hecho de aislar los clústeres en su propia OU también ayuda a evitar la eliminación accidental de los objetos de equipo de clúster.

Además, comprueba los siguientes requisitos de la cuenta:

- Asegúrate de que la cuenta que deseas usar para crear el clúster sea un usuario de dominio con derechos de administrador en todos los servidores que quieres agregar como nodos de clúster.
- Asegúrate de que se cumpla una de las siguientes condiciones:
    - El usuario que crea el clúster tiene el permiso **Crear objetos de equipo** en la OU o el contenedor donde residen los servidores que conformarán el clúster.
    - Si el usuario no tiene el permiso **Crear objetos de equipo**, solicita a un administrador de dominio que preconfigure un objeto de equipo de clúster para el clúster. Para obtener más información, consulta [Preconfigurar objetos de equipo de clúster en Servicios de dominio de Active Directory](prestage-cluster-adds.md).

> [!NOTE]
> Este requisito no se aplica si desea crear un clúster desconectado Active Directory en Windows Server 2012 R2. Para obtener más información, consulte [Implementar un clúster desconectado de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Instalar la característica de clúster de conmutación por error

Debes instalar la característica de clúster de conmutación por error en todos los servidores que desees incluir como nodo de clúster de conmutación por error.

### <a name="install-the-failover-clustering-feature"></a>Instalar la característica de clúster de conmutación por error

1. Inicie el Administrador del servidor.
2. En el menú **administrar** , seleccione **Agregar roles y características**.
3. En la página **antes de comenzar** , seleccione **siguiente**.
4. En la página **Seleccionar tipo de instalación** , seleccione Instalación basada en **características o en roles**y, a continuación, seleccione **siguiente**.
5. En la página **Seleccionar servidor de destino** , seleccione el servidor en el que desea instalar la característica y, a continuación, seleccione **siguiente**.
6. En la página **Seleccionar roles de servidor** , seleccione **siguiente**.
7. En la página **Seleccionar características**, activa la casilla **Clúster de conmutación por error**.
8. Para instalar las herramientas de administración del clúster de conmutación por error, seleccione **Agregar características**y, a continuación, seleccione **siguiente**.
9. En la página **confirmar selecciones de instalación** , seleccione **instalar**.
<br>La característica de clústeres de conmutación por error no requiere reiniciar el servidor.

10. Una vez completada la instalación, seleccione **cerrar**.
11. Repite este procedimiento en todos los servidores que desees incluir como nodo de clúster de conmutación por error.

> [!NOTE]
> Una vez instalada la característica de clústeres de conmutación por error, recomendamos aplicar las últimas actualizaciones de Windows Update. Además, para un clúster de conmutación por error basado en Windows Server 2012, revise las [revisiones y actualizaciones recomendadas para los clústeres de conmutación por error basados en Windows server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) soporte técnico de Microsoft artículo e instale las actualizaciones que se apliquen.

## <a name="validate-the-configuration"></a>Validar la configuración

Antes de crear el clúster de conmutación por error, recomendamos encarecidamente validar la configuración para asegurarse de que el hardware y sus valores sean compatibles con los clústeres de conmutación por error. Microsoft es compatible con una solución de clúster únicamente si el conjunto de la configuración supera todas las pruebas de validación y si todo el hardware está certificado para la versión de Windows Server en la que se ejecutan los nodos de clúster.

> [!NOTE]
> Debes tener por lo menos dos nodos para ejecutar todas las pruebas. Si solo tienes uno, no podrán ejecutarse muchas de las pruebas de almacenamiento más importantes.

### <a name="run-cluster-validation-tests"></a>Ejecutar pruebas de validación de clústeres

1. En un equipo que tenga las Herramientas de administración de clústeres de conmutación por error instaladas desde las Herramientas de administración remota del servidor, o en un servidor donde hayas instalado la característica de clústeres de conmutación por error, inicia el Administrador de clústeres de conmutación por error. Para hacer esto en un servidor, inicie Administrador del servidor y, a continuación, en el menú **herramientas** , seleccione **Administrador de clústeres de conmutación por error**.
2. En el panel **Administrador de clústeres de conmutación por error** , en **Administración**, seleccione **validar configuración**.
3. En la página **antes de comenzar** , seleccione **siguiente**.
4. En la página **seleccionar servidores o un clúster** , en el cuadro **escribir nombre** , escriba el nombre NetBIOS o el nombre de dominio completo de un servidor que vaya a agregar como nodo de clúster de conmutación por error y, a continuación, seleccione **Agregar**. Repite este paso para todos los servidores que quieras agregar. Para agregar varios servidores al mismo tiempo, separa los nombres con una coma o un punto y coma. Por ejemplo, escriba los nombres con el formato `server1.contoso.com, server2.contoso.com`. Cuando termine, seleccione **siguiente**.
5. En la página **Opciones de prueba** , seleccione **ejecutar todas las pruebas (recomendado)** y, a continuación, seleccione **siguiente**.
6. En la página **confirmación** , seleccione **siguiente**.

    La página Validación muestra el estado de las pruebas en ejecución.
7. En la página **Resumen**, realiza una de las acciones siguientes:
    
      - Si los resultados indican que las pruebas se completaron correctamente y la configuración es adecuada para la agrupación en clústeres y desea crear el clúster inmediatamente, asegúrese de que la casilla **crear el clúster ahora con los nodos validados** esté activada y, a continuación, seleccione **Finalizar**. Después, continúa con el paso 4 del procedimiento [Crear el clúster de conmutación por error](#create-the-failover-cluster).
      - Si los resultados indican que había advertencias o errores, seleccione **Ver informe** para ver los detalles y determinar qué problemas deben corregirse. Ten en cuenta que una advertencia sobre una prueba de validación en particular indica que ese aspecto del clúster de conmutación por error puede ser compatible, pero podría no cumplir los procedimientos recomendados.
        
        > [!NOTE]
        > Si recibes una advertencia sobre la prueba Validar reserva persistente de espacios de almacenamiento, consulta la entrada de blog [La advertencia sobre la validación de un clúster de conmutación por error de Windows indica que los discos no son compatibles con las reservas persistentes de espacios de almacenamiento](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) para obtener más información.

Para obtener más información sobre las pruebas de validación de hardware, consulta [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Crear el clúster de conmutación por error

Para completar este paso, asegúrate de que la cuenta de usuario con la que inicias sesión cumpla los requisitos indicados en la sección [Comprobar los requisitos previos](#verify-the-prerequisites) de este tema.

1. Inicie el Administrador del servidor.
2. En el menú **herramientas** , seleccione **Administrador de clústeres de conmutación por error**.
3. En el panel **Administrador de clústeres de conmutación por error** , en **Administración**, seleccione **crear clúster**.
    
    Se abre el Asistente para crear clúster.
4. En la página **antes de comenzar** , seleccione **siguiente**.
5. Si aparece la página **seleccionar servidores** , en el cuadro **escribir nombre** , escriba el nombre NetBIOS o el nombre de dominio completo de un servidor que vaya a agregar como nodo de clúster de conmutación por error y, a continuación, seleccione **Agregar**. Repite este paso para todos los servidores que quieras agregar. Para agregar varios servidores al mismo tiempo, separa los nombres con una coma o un punto y coma. Por ejemplo, escribe los nombres con el formato *server1.contoso.com; server2.contoso.com*. Cuando termine, seleccione **siguiente**.
    
    > [!NOTE]
    > Si decide crear el clúster inmediatamente después de ejecutar la validación en el [procedimiento de validación](#validate-the-configuration)de la configuración, no verá la página **seleccionar servidores** . Los nodos que se validaron se agregan automáticamente al Asistente para crear clúster, de modo que no tengas que introducirlos de nuevo.
6. Si antes omitiste la validación, aparecerá la página **Advertencia de validación** . Recomendamos encarecidamente ejecutar la validación de clústeres. Solo los clústeres que superan todas las pruebas de validación son compatibles con Microsoft. Para ejecutar las pruebas de validación, seleccione **sí**y, a continuación, seleccione **siguiente**. Complete el Asistente para validar una configuración tal y como se describe en [validación de la configuración](#validate-the-configuration).
7. En la página **Punto de acceso para administrar el clúster** , haz lo siguiente:
    
    1. En el cuadro **Nombre de clúster**, escribe el nombre que deseas utilizar para administrar el clúster. Antes de hacerlo, revisa la siguiente información:
        
          - Durante la creación del clúster, este nombre se registra como el objeto de equipo de clúster (también llamado *objeto de nombre de clúster* o *CNO*) en AD DS. Si especificas un nombre NetBIOS para el clúster, el CNO se crea en la misma ubicación en la que residen los objetos de equipo para los nodos de clúster. Puede ser el contenedor Equipos predeterminado o una OU.
          - Para especificar una ubicación diferente para el CNO, escribe el nombre distintivo de una OU en el cuadro **Nombre de clúster**. Por ejemplo: *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*.
          - Si un administrador de dominio ha preconfigurado el CNO en una OU diferente de donde residen los nodos de clúster, especifica el nombre distintivo que te proporcione el administrador de dominio.
    2. Si el servidor no tiene ningún adaptador de red configurado para usar DHCP, debes configurar una o varias direcciones IP estáticas para el clúster de conmutación por error. Selecciona la casilla situada junto a cada red que desees utilizar para la administración de clústeres. Seleccione el campo **Dirección** situado junto a una red seleccionada y, a continuación, escriba la dirección IP que desea asignar al clúster. Esta dirección o direcciones IP se asociarán al nombre del clúster en el Sistema de nombres de dominio (DNS).
    3. Cuando termine, seleccione **siguiente**.
8. En la página **Confirmación**, revisa la configuración. De manera predeterminada, la casilla **Agregar todo el almacenamiento apto al clúster** está seleccionada. Desactiva esta casilla si deseas realizar una de las siguientes acciones:
    
      - Deseas configurar el almacenamiento más adelante.
      - Tienes previsto crear espacios de almacenamiento en clúster mediante el Administrador de clústeres de conmutación por error o mediante los cmdlets de Windows PowerShell de clústeres de conmutación por error, y todavía no has creado espacios de almacenamiento en los Servicios de archivos y almacenamiento. Para obtener más información, consulte [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Seleccione **siguiente** para crear el clúster de conmutación por error.
10. En la página **Resumen**, confirma que el clúster de conmutación por error se ha creado correctamente. Si hay advertencias o errores, vea la salida de resumen o seleccione **Ver informe** para ver el informe completo. Seleccione **Finalizar**.
11. Para confirmar que se ha creado el clúster, comprueba que su nombre aparece en el **Administrador de clústeres de conmutación por error**, en el árbol de navegación. Puede expandir el nombre del clúster y seleccionar elementos en **nodos**, **almacenamiento** o **redes** para ver los recursos asociados.
    
    Ten en cuenta que el nombre del clúster podría tardar un poco en replicarse correctamente en DNS. Después de que el registro y la replicación de DNS se hayan realizado correctamente, si selecciona **todos los servidores** en Administrador del servidor, el nombre del clúster debe aparecer como servidor con el estado de **manejabilidad** de **en línea**.

Una vez creado el clúster, puedes realizar acciones como comprobar la configuración de quórum del clúster y, opcionalmente, crear volúmenes compartidos de clúster (CSV). Para obtener más información, consulte [Descripción del cuórum en espacios de almacenamiento directo](../storage/storage-spaces/understand-quorum.md) y [uso de volúmenes compartidos de clúster en un clúster de conmutación por error](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Crear roles en clúster

Después de crear el clúster de conmutación por error, puedes crear roles en clúster para hospedar cargas de trabajo de clúster.

>[!NOTE]
>Para los roles en clúster que necesitan un punto de acceso de cliente, se crea un objeto de equipo virtual (VCO) en AD DS. De manera predeterminada, todos los VCO para el clúster se crean en el mismo contenedor o en la misma OU que el CNO. Ten en cuenta que, después de crear un clúster, puedes mover el CNO a cualquier OU.

Aquí se muestra cómo crear un rol en clúster:

1. Utiliza el Administrador del servidor o Windows PowerShell para instalar el rol o la característica que sean necesarios para un rol en clúster en cada nodo de clúster de conmutación. Por ejemplo, si deseas crear un servidor de archivos en clúster, instala el rol de Servidor de archivos en todos los nodos de clúster.
    
    En la siguiente tabla se muestran los roles en clúster que puedes configurar en el Asistente para alta disponibilidad y la característica o el rol de servidor asociado que debes instalar como requisito previo.

   | Rol en clúster  | Requisito previo del rol o característica  |
   | ---------       | ---------                    |
   | Servidor de espacio de nombres     |   Espacios de nombres (parte del rol de servidor de archivos)       |
   | Servidor de espacios de nombres DFS     |  Rol de Servidor DHCP       |
   | Coordinador de transacciones distribuidas (DTC)     | Ninguno        |
   | Servidor de archivos     |  Rol de Servidor de archivos       |
   | Aplicación genérica     |  No disponible       |
   | Script genérico     |   No disponible      |
   | Servicio genérico     |   No disponible      |
   | Agente de réplicas de Hyper-V     |   Rol de Hyper-V      |
   | Servidor de destino iSCSI     |    Servidor de destino iSCSI (parte del rol de Servidor de archivos)     |
   | Servidor iSNS     |  Característica Servicio de servidor iSNS       |
   | Message Queue Server     |  Característica Servicios de Message Queue Server       |
   | Otro servidor     |  Ninguno       |
   | Máquina virtual     |  Rol de Hyper-V       |
   | Servidor WINS     |   Característica Servidor WINS      |

2. En Administrador de clústeres de conmutación por error, expanda el nombre del clúster, haga clic con el botón secundario en **roles**y, a continuación, seleccione **configurar rol**.
3. Sigue los pasos del Asistente para alta disponibilidad para crear el rol en clúster.
4. Para comprobar que se ha creado el rol en clúster, en el panel **Roles** , asegúrate de que el rol tenga el estado **En ejecución**. El panel Roles también indica el nodo propietario. Para probar la conmutación por error, haga clic con el botón secundario en el rol, seleccione **movimiento**y, a continuación, seleccione **seleccionar nodo**. En el cuadro de diálogo de **movimiento del rol en clúster** , seleccione el nodo de clúster deseado y, después, haga clic en **Aceptar**. En la columna **Nodo propietario** , comprueba que el nodo propietario ha cambiado.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Crear un clúster de conmutación por error a través de Windows PowerShell

Los siguientes cmdlets de Windows PowerShell realizan las mismas funciones que los procedimientos anteriores de este tema. Escribe cada cmdlet en una sola línea, aunque aparezcan con salto entre varias líneas debido a las limitaciones de formato.

> [!NOTE]
> Debe usar Windows PowerShell para crear un clúster desconectado Active Directory en Windows Server 2012 R2. Para obtener más información sobre la sintaxis, consulta [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

El siguiente ejemplo instala la característica Clústeres de conmutación por error.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

El siguiente ejemplo ejecuta todas las pruebas de validación de clústeres en los equipos llamados *Server1* y *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

> [!NOTE]
> El cmdlet **Test-Cluster** genera los resultados en un archivo de registro en el directorio de trabajo actual. Por ejemplo: C:\Users\<nombreDeUsuario > \AppData\Local\Temp.

El siguiente ejemplo crea un clúster de conmutación por error llamado *MyCluster* con los nodos *Server1* y *Server2*, asigna la dirección IP estática *192.168.1.12*y agrega todo el almacenamiento apto al clúster de conmutación por error.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

El siguiente ejemplo crea el mismo clúster de conmutación por error que el ejemplo anterior, pero no agrega almacenamiento apto al clúster de conmutación por error.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

El siguiente ejemplo crea un clúster llamado *MyCluster* en la OU *Cluster* del dominio *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Para obtener ejemplos de cómo agregar roles en clúster, consulta temas como [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) y [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## <a name="more-information"></a>Más información

  - [Clúster de conmutación por error](failover-clustering.md)
  - [Implementar un clúster de Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Servidor de archivos de escalabilidad horizontal para los datos de la aplicación](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Implementación de un clúster desconectado Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Uso de clústeres invitados para alta disponibilidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Actualización compatible con clústeres](cluster-aware-updating.md)
  - [Nuevo-clúster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Clúster de prueba](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
