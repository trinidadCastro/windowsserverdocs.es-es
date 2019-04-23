---
title: Servicio de mantenimiento de Windows Server
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 5afb64dcf0c59697ed55d7cf51ef1bc36e7e0e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863816"
---
# <a name="health-service-in-windows-server"></a>Servicio de mantenimiento de Windows Server

> Se aplica a Windows Server 2016

El servicio de mantenimiento es una característica nueva en Windows Server 2016 que mejora la supervisión diaria y la experiencia operativa para los clústeres que ejecutan espacios de almacenamiento directo.

## <a name="prerequisites"></a>Requisitos previos  

Servicio de mantenimiento está habilitado de forma predeterminada con Espacios de almacenamiento directo. No se requiere ninguna acción adicional para instalarlo o iniciarlo. Para obtener más información acerca de espacios de almacenamiento directo, vea [espacios de almacenamiento directo en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Informes

Consulte [informa del servicio de mantenimiento](health-service-reports.md).

## <a name="faults"></a>Errores

Consulte [errores del servicio de mantenimiento](health-service-faults.md).

## <a name="actions"></a>Acciones

Consulte [acciones de servicio de mantenimiento](health-service-actions.md).

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

Debe reemplazar el disco físico retirado cuando sea posible. A menudo, consta de un intercambio directo: es decir, apagar el alojamiento de almacenamiento o nodo no es necesario. Consulte el error para conocer información útil sobre la ubicación y la pieza.  

#### <a name="verification"></a>Comprobación

Cuando se inserta el disco de reemplazo, se comprobará en el documento de componentes compatibles (consulte la sección siguiente).

#### <a name="pooling"></a>Agrupación  

Si se permite, el disco de reemplazo se sustituye automáticamente en el grupo de su predecesor para su uso. En este punto, el sistema vuelve a su estado inicial perfecto y el error desaparece.  

## <a name="supported-components-document"></a>Documento de componentes compatibles  

El servicio de mantenimiento proporciona un mecanismo de cumplimiento para restringir los componentes usados por espacios de almacenamiento directo a los que están en un documento de componentes admite proporcionada por el administrador o el proveedor de la solución. Esta lista se puede utilizar para evitar el uso erróneo de hardware no compatible por cualquier usuario, que puede ayudar con el cumplimiento de los contratos de soporte técnico o garantía. Esta funcionalidad está actualmente limitada a los dispositivos de disco físico, incluidas unidades SSD, unidades de disco duro, y unidades de NVMe. Puede restringir el documento de componentes compatibles en el modelo, fabricante (opcional) y versión de firmware (opcional).

### <a name="usage"></a>Uso  

El documento de componentes admite usa una sintaxis basada en XML. Se recomienda usar el editor de texto que prefiera, como la versión gratuita [Visual Studio Code](http://code.visualstudio.com/) o el Bloc de notas, para crear un documento XML que puede guardar y reutilizar.

#### <a name="sections"></a>Secciones

El documento tiene dos secciones independientes: `Disks` y `Cache`.

Si el `Disks` sección es siempre muestran solo las unidades (como `Disk`) tienen permitido unirse a grupos. Las unidades no enumeradas no podrán unirse a los grupos, lo que imposibilita eficazmente su uso en producción. Si esta sección se deja vacía, se permitirá cualquier unidad unirse a los grupos.

Si el `Cache` sección es siempre muestran solo las unidades (como `CacheDisk`) se usan para almacenar en caché. Si esta sección se deja vacía, espacios de almacenamiento directo intenta [estimación en función de tipo de medio y el tipo de bus](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). Unidades que aparecen aquí también deben aparecer en `Disks`.

>[!IMPORTANT]
> El documento de componentes compatibles no se aplica con carácter retroactivo a las unidades ya agrupadas y en uso.  

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

Para obtener una lista de varias unidades, basta con agregar más `<Disk>` o `<CacheDisk>` etiquetas.

Para insertar este código XML al implementar espacios de almacenamiento directo, use el `-XML` parámetro:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

Para establecer o modificar el documento de componentes admite una vez que se ha implementado espacios de almacenamiento directo:

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

## <a name="see-also"></a>Vea también

- [Informes de servicio de mantenimiento](health-service-reports.md)
- [Errores del servicio de mantenimiento](health-service-faults.md)
- [Acciones de servicio de mantenimiento](health-service-actions.md)
- [Configuración del servicio de mantenimiento](health-service-settings.md)
- [Espacios de almacenamiento directo en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
