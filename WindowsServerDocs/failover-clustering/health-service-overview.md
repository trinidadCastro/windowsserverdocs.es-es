---
title: Servicio de estado en Windows Server 2016
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 834fcfb749e89e4768dce3f229564feea550a432
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-in-windows-server-2016"></a>Servicio de estado en Windows Server 2016
> Se aplica a Windows Server 2016

El servicio de estado es una nueva característica de Windows Server 2016 que mejora la supervisión diarias y de la experiencia operativa clústeres ejecutan directa de los espacios de almacenamiento.

## <a name="prerequisites"></a>Requisitos previos  

El servicio de estado está habilitado de forma predeterminada con directa de los espacios de almacenamiento. Se requiere ninguna acción adicional para configurarlo o iniciarlo. Para obtener más información sobre directa de los espacios de almacenamiento, consulta [directa de los espacios de almacenamiento en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Informes

Consulta [informes de estado del servicio](health-service-reports.md).

## <a name="faults"></a>Errores de página

Consulta [defectos de servicio de salud](health-service-faults.md).

## <a name="actions"></a>Acciones

Consulta [acciones de servicio de salud](health-service-actions.md).

## <a name="automation"></a>Automatización de la  

Esta sección describen los flujos de trabajo que estén automatizadas por el servicio de estado del ciclo de vida del disco.  

### <a name="disk-lifecycle"></a>Ciclo de vida de disco   

El servicio de estado automatiza la mayoría de las etapas del ciclo de vida de disco físico. Supongamos que es el estado inicial de la implementación en perfecto estado - es decir, todos los discos físicos funcionan correctamente.  

#### <a name="retirement"></a>Retirada  

Discos físicos se retiran automáticamente cuando ya no pueden usarse, y se genera un error correspondiente. Hay varios casos:  

-   Error de medios: el disco físico está definitivamente no se pudo o dividido y debe reemplazarse.  

-   Perdido la comunicación: el disco físico ha perdido la conectividad durante más de 15 minutos consecutivos.  

-   No responde: el disco físico ha demostrado la latencia de más 5.0 segundos tres o más veces dentro de una hora.  

>[!NOTE]
> Si se pierde a varios discos físicos la conectividad a la vez, o a un nodo completo o alojamiento de almacenamiento, el servicio de estado *no* retirar estos discos, dado que no es probable que sea el problema de raíz.  

Si el disco retirado se funciona como la memoria caché de muchos otros discos físicos, estos se automáticamente asignarse a otro disco caché si está disponible. No se requiere ninguna acción especial.  

#### <a name="restoring-resiliency"></a>Restaurar el estado de resistencia  

Una vez que se ha retirado un disco físico, el servicio de estado comienza inmediatamente a copiar sus datos en los discos físicos restantes, para restaurar el estado de resistencia completa. Una vez completado esto, los datos es completamente seguros y a errores tolerancia a errores nuevamente.  

>[!NOTE]
> Esta restauración inmediata requiere la capacidad disponible suficiente entre los discos físicos restantes.  

#### <a name="blinking-the-indicator-light"></a>Parpadear la luz indicadora  

Si es posible, el servicio de estado comenzarán a parpadear la luz de indicador en el disco físico retirado o la ranura. Esto seguirá indefinidamente, hasta que se ha reemplazado el disco retirado.  

>[!NOTE]
> En algunos casos, el disco posible error de forma que impide que incluso su luz del indicador de funcionamiento - por ejemplo, una pérdida de energía total.  

#### <a name="physical-replacement"></a>Reemplazo física  

Debes reemplazar el disco físico retirado cuando sea posible. A menudo, se trata de un intercambio - es decir no es necesario apagar el nodo o almacenamiento local. Consulta el error de ubicación útil y la información del elemento.  

#### <a name="verification"></a>Verificación

Cuando se inserta el disco de reemplazo, se comprobará contra el documento de componentes compatibles (consulta la sección siguiente).

#### <a name="pooling"></a>Agrupación  

Si se permite, el disco de reemplazo se sustituye automáticamente en el grupo de sus predecesoras, para especificar el uso. En este punto, el sistema se devuelve a su estado inicial de la salud perfecta y, a continuación, el error desaparecerá.  

## <a name="supported-components-document"></a>Documento de componentes compatibles  

El servicio de estado proporciona un mecanismo de cumplimiento para restringir los componentes usados por espacios de almacenamiento directa a los que estén en un documento de componentes compatibles, proporcionada por el administrador o el proveedor de soluciones. Esto puede usarse para evitar el uso errónea de hardware no admitido por usted u otras personas, que puede ser útil con el cumplimiento de los contratos de garantía o soporte técnico. Esta funcionalidad actualmente está limitada a los dispositivos de disco físico, incluidos los SSD, unidades de disco duro, y NVMe unidades. Restringir el documento de componentes compatibles en la versión de modelo, fabricante (opcional) y el firmware (opcional).

### <a name="usage"></a>Uso de  

El documento de componentes compatibles, se usa una sintaxis de XML y de inspiración. Te recomendamos que uses el editor de texto, como código de Visual Studio (disponible de forma gratuita [aquí](http://code.visualstudio.com/)) o en el Bloc de notas, para crear un documento XML que puedes guardar y volver a usar.

#### <a name="sections"></a>Secciones

El documento tiene dos secciones independientes: **discos** y **caché**.

Si la **discos** sección se proporciona, pueden unirse a grupos de únicamente las unidades que aparece. Las unidades en la lista no pueden unirse a grupos, que impide eficazmente su uso en producción. Si esta sección está vacía, cualquier unidad se podrá unirse a grupos.

Si la **caché** sección se proporciona, únicamente las unidades que aparece se usará para almacenar en caché. Si esta sección está vacía, directa de los espacios de almacenamiento intentará adivinar en función de tipo de medios y el tipo de bus. Por ejemplo, si la implementación usa unidades de estado sólido (SSD) y unidades de disco duro (HDD), el primero se elige automáticamente para almacenar en caché; Sin embargo, si la implementación usa el flash de todo, necesitarás especificar los dispositivos de resistencia más altos que le gustaría usar para almacenar en caché aquí.

>[!IMPORTANT]
> No se aplica el documento de componentes compatibles retroactiva ya agrupadas de unidades y en uso.  

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
        <BinaryPath>\\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
  </Disks>

  <Cache>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Cache>

</Components>

```

Para mostrar varias unidades, simplemente agrega adicionales ** &lt;disco&gt; ** etiquetas en cualquiera de las secciones.

Para insertar este código XML al implementar directa de los espacios de almacenamiento, usa el **- XML** marca:

```PowerShell
Enable-ClusterS2D -XML <MyXML>
```

Para definir o modificar el documento de componentes compatibles cuando directa de los espacios de almacenamiento se haya implementado (es decir, una vez que ya se está ejecutando el servicio de estado), usa el siguiente cmdlet de PowerShell:

```PowerShell
$MyXML = Get-Content <\\path\to\file.xml> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>El modelo, fabricante y las propiedades de la versión de firmware deben coincidir exactamente con los valores que Obtén usando el **Get-disco físico** cmdlet. Esto puede diferir de la expectativa "sentido común", según la implementación de tu proveedor. Por ejemplo, en lugar de "Contoso", el fabricante puede ser "CONTOSO Ltd.", o puede estar en blanco mientras que el modelo es "Contoso-XZY9000".  

Puedes verificar mediante el siguiente cmdlet de PowerShell:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Configuración

Consulta [configuración del servicio de salud](health-service-settings.md).

## <a name="see-also"></a>Consulta también

- [Informes de estado de servicio](health-service-reports.md)
- [Errores de servicio de estado](health-service-faults.md)
- [Acciones de servicio de estado](health-service-actions.md)
- [Configuración del servicio de estado](health-service-settings.md)
- [Espacios de almacenamiento dirigen en Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
