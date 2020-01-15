---
title: Servicio de mantenimiento en Windows Server
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 158681e2038e3d8015933771d06d3bfb24d31586
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948476"
---
# <a name="health-service-in-windows-server"></a>Servicio de mantenimiento en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016

El Servicio de mantenimiento es una nueva característica de Windows Server 2016 que mejora la supervisión cotidiana y la experiencia operativa de los clústeres que ejecutan Espacios de almacenamiento directo.

## <a name="prerequisites"></a>Requisitos previos  

Servicio de mantenimiento está habilitado de forma predeterminada con Espacios de almacenamiento directo. No se requiere ninguna acción adicional para instalarlo o iniciarlo. Para obtener más información acerca de Espacios de almacenamiento directo, consulte [espacios de almacenamiento directo en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Informes

Vea [informes de servicio de mantenimiento](health-service-reports.md).

## <a name="faults"></a>Errores

Vea [errores de servicio de mantenimiento](health-service-faults.md).

## <a name="actions"></a>Acciones

Consulte [servicio de mantenimiento acciones](health-service-actions.md).

## <a name="automation"></a>Automatización  

En esta sección se describen los flujos de trabajo que se automatizan mediante el servicio de mantenimiento en el ciclo de vida del disco.  

### <a name="disk-lifecycle"></a>Ciclo de vida del disco   

El servicio de mantenimiento automatiza la mayoría de las etapas del ciclo de vida del disco físico. Supongamos que el estado inicial de la implementación es un estado perfecto: es decir, todos los discos físicos funcionan correctamente.  

#### <a name="retirement"></a>Retirada  

Los discos físicos se retirarán automáticamente cuando ya no se usen, y se genera un error correspondiente. Hay varios casos:  

-   Error del medio: el disco físico definitivamente tiene un error o está dañado y debe reemplazarse.  

-   Pérdida de comunicación: el disco físico ha perdido la conectividad durante más de 15 minutos consecutivos.  

-   No responde: el disco físico ha mostrado una latencia de más 5 segundos tres o más veces en una hora.  

>[!NOTE]
> Si se pierde la conectividad en muchos discos físicos a la vez, o a un nodo completo o contenedor de almacenamiento, el servicio de mantenimiento *no* retirará estos discos, ya que no es probable que sean el problema raíz.  

Si el disco retirado sirve de memoria caché para otros discos físicos, estos se reasignarán automáticamente a otro disco de la memoria caché, si hubiera alguno disponible. No se requiere ninguna acción del usuario especial.  

#### <a name="restoring-resiliency"></a>Restauración de la resistencia  

Cuando se ha retirado un disco físico, el servicio de mantenimiento comienza inmediatamente a copiar sus datos en los discos físicos restantes para restaurar la resistencia completa. Cuando se haya completado, los datos están completamente seguros y tolerantes a errores de nuevo.  

>[!NOTE]
> Esta restauración inmediata requiere suficiente capacidad disponible entre los discos físicos restantes.  

#### <a name="blinking-the-indicator-light"></a>Parpadeo de la luz del indicador  

Si es posible, el servicio de mantenimiento comenzará a hacer parpadear la luz del indicador en el disco físico retirado o en la ranura. Esto continuará indefinidamente, hasta que se reemplace el disco retirado.  

>[!NOTE]
> En algunos casos, el disco puede haber dado un error de forma que impida incluso el funcionamiento de la luz del indicador; por ejemplo, una pérdida total de alimentación.  

#### <a name="physical-replacement"></a>Reemplazo físico  

Debe reemplazar el disco físico retirado cuando sea posible. La mayoría de las veces, se compone de un intercambio activo, es decir, no es necesario apagar el nodo o el contenedor de almacenamiento. Consulte el error para conocer información útil sobre la ubicación y la pieza.  

#### <a name="verification"></a>Comprobación

Cuando se inserte el disco de reemplazo, se comprobará con el documento componentes admitidos (consulte la sección siguiente).

#### <a name="pooling"></a>Agrupación  

Si se permite, el disco de reemplazo se sustituye automáticamente en el grupo de su predecesor para su uso. En este punto, el sistema vuelve a su estado inicial perfecto y el error desaparece.  

## <a name="supported-components-document"></a>Documento de componentes admitidos  

El Servicio de mantenimiento proporciona un mecanismo de cumplimiento para restringir los componentes que usa Espacios de almacenamiento directo a los de un documento de componentes admitidos proporcionado por el administrador o el proveedor de la solución. Esta lista se puede utilizar para evitar el uso erróneo de hardware no compatible por cualquier usuario, que puede ayudar con el cumplimiento de los contratos de soporte técnico o garantía. Esta funcionalidad está limitada actualmente a los dispositivos de disco físico, incluidas las unidades SSD, HDD y NVMe. El documento de componentes admitidos puede restringir el modelo, el fabricante (opcional) y la versión de firmware (opcional).

### <a name="usage"></a>Uso  

El documento de componentes compatibles utiliza una sintaxis inspirada en XML. Se recomienda usar su editor de texto favorito, como el [Visual Studio Code](https://code.visualstudio.com/) gratuito o el Bloc de notas, para crear un documento XML que puede guardar y volver a usar.

#### <a name="sections"></a>Secciones

El documento tiene dos secciones independientes: `Disks` y `Cache`.

Si se proporciona la sección `Disks`, solo se permite unir grupos a las unidades enumeradas (como `Disk`). Las unidades que no figuran en la lista no se pueden unir a grupos, lo que impide su uso en producción. Si esta sección se deja vacía, se permitirá a cualquier unidad unirse a grupos.

Si se proporciona la sección `Cache`, solo se usan para el almacenamiento en caché las unidades enumeradas (como `CacheDisk`). Si esta sección se deja vacía, Espacios de almacenamiento directo intenta [adivinar según el tipo de medio y el tipo de bus](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). Las unidades que se muestran aquí también deben aparecer en `Disks`.

>[!IMPORTANT]
> El documento componentes admitidos no se aplica de forma retroactiva a las unidades ya agrupadas y en uso.  

#### <a name="example"></a>Ejemplo

```XML
<Components>

  <Disks>
    <Disk>
      <Manufacturer>Contoso</Manufacturer>
      <Model>XYZ9000</Model>
      <AllowedFirmware>
        <Version>2.0</Version>
        <Version>2.1</Version>
        <Version>2.2</Version>
      </AllowedFirmware>
      <TargetFirmware>
        <Version>2.1</Version>
        <BinaryPath>C:\ClusterStorage\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Disks>

  <Cache>
    <CacheDisk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </CacheDisk>
  </Cache>

</Components>

```

Para enumerar varias unidades, simplemente agregue `<Disk>` o `<CacheDisk>` etiquetas adicionales.

Para insertar este XML al implementar Espacios de almacenamiento directo, use el parámetro `-XML`:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

Para establecer o modificar el documento de componentes admitidos una vez que se ha implementado Espacios de almacenamiento directo:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>Las propiedades de modelo, fabricante y la versión de firmware deben coincidir exactamente con los valores que se obtienen con el cmdlet **Get-PhysicalDisk**. Puede diferir de la expectativa de "sentido común", dependiendo de la implementación del proveedor. Por ejemplo, en lugar de "Contoso", el fabricante puede ser "CONTOSO LTD" o puede estar en blanco mientras que el modelo es "Contoso-XZY9000".  

Puede comprobarlo mediante el siguiente cmdlet de PowerShell:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Configuración

Consulte [configuración de servicio de mantenimiento](health-service-settings.md).

## <a name="see-also"></a>Consulta también

- [Servicio de mantenimiento informes](health-service-reports.md)
- [Errores de Servicio de mantenimiento](health-service-faults.md)
- [Servicio de mantenimiento acciones](health-service-actions.md)
- [Configuración de Servicio de mantenimiento](health-service-settings.md)
- [Espacios de almacenamiento directo en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
