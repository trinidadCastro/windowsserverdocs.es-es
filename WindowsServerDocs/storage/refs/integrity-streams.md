---
title: Flujos de integridad de ReFS
author: gawatu
ms.author: jgerend
manager: dmoss
ms.date: 10/16/2018
ms.topic: article
ms.assetid: 1f1215cd-404f-42f2-b55f-3888294d8a1f
ms.openlocfilehash: 15c4b7942be949af33e70d2a5f299af426040e7b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950509"
---
# <a name="refs-integrity-streams"></a>Flujos de integridad de ReFS
>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual), Windows 10

Las secuencias de integridad son una característica opcional en ReFS que valida y mantiene la integridad de datos usando sumas de comprobación. Aunque ReFS siempre usa las sumas de comprobación para los metadatos, ReFS no genera ni valida de forma predeterminada las sumas de comprobación para los datos de archivo. Secuencias de integridad es una característica opcional que permite a los usuarios emplear sumas de comprobación para los datos de archivos. Cuando se habilitan los flujos de integridad, ReFS puede determinar claramente si los datos son válidos o están dañados. Además, los espacios de almacenamiento y ReFS pueden arreglar datos y metadatos dañados automáticamente y de manera conjunta.

## <a name="how-it-works"></a>Funcionamiento

Las secuencias de integridad pueden habilitarse para archivos individuales, directorios o un volumen completo y la configuración de las secuencias de integridad puede cambiarse en cualquier momento. Además, la configuración de la secuencia de integridad de archivos y directorios se hereda de sus directorios principales.

Una vez que las secuencias de integridad se han habilitado, ReFS creará y mantendrá una suma de comprobación para los archivos especificados en los metadatos del archivo. Esta suma comprobación permite a ReFS validar la integridad de los datos antes de acceder a él. Antes de devolver cualquier dato que tenga secuencias integridad habilitadas, ReFS calculará, en primer lugar, la suma de comprobación:

![Calcular la suma de comprobación para los datos de archivo](media/compute-checksum.gif)

A continuación, esta suma de comprobación se compara con la que se incluye en los metadatos de archivo. Si las sumas de comprobación coinciden, los datos se marcan como válidos y se devuelven al usuario. Si las sumas de comprobación no coinciden, los datos están dañados. La resistencia del volumen determina cómo responde ReFS a los daños:

- Si ReFS está montado en un espacio simple y no resistente o en una unidad vacía, ReFS devolverá un error al usuario sin devolver los datos dañados.
- Si ReFS está montado en un espejo resistente o en un espacio de paridad, ReFS intentará arreglar todos los daños.
    - Si el intento se realiza con éxito, ReFS aplicará una escritura correctiva para restaurar la integridad de los datos y devolverá los datos válidos a la aplicación. La aplicación no es consciente de la existencia de ningún daño.
    - Si el intento no se realiza con éxito, ReFS devolverá un error.

ReFS registrará todos los daños en el registro de eventos del sistema y aquí se reflejarán si los daños se han solucionados.

![Integridad de datos de restauración de escritura correctiva](media/corrective-write.gif)

## <a name="performance"></a>Rendimiento

Aunque las secuencias de integridad proporcionan mayor integridad de datos para el sistema, esto también incurre en un coste del rendimiento. Hay un par de diferentes razones para ello:
- Si las secuencias de integridad están habilitadas, todas las operaciones de escritura se convierten en operaciones de asignar a la escritura. Aunque esto evita cualquier cuello de botella de lectura-modificación-escritura, ya que no es necesario leer o modificar los datos existentes, los datos de archivo se fragmentan con frecuencia, lo que retrasa las lecturas.
- Dependiendo de la carga de trabajo y el almacenamiento subyacente del sistema, el coste computacional de la computación y la validación de la suma de comprobación puede causar que la latencia de la E/S aumente.

Dado que los flujos de integridad conllevan un costo de rendimiento, se recomienda que se deshabiliten las secuencias de integridad en los sistemas sensibles al rendimiento.

## <a name="integrity-scrubber"></a>Limpieza de integridad

Tal y como se ha descrito anteriormente, ReFS validará automáticamente la integridad de los datos antes de tener acceso a los datos. ReFS también usa un regulador de fondo, que permite a ReFS validar los datos a los que se accede con poca frecuencia. Este borrador examina periódicamente el volumen, identifica los daños latentes y desencadena de forma proactiva una reparación de los datos dañados.

  >[!NOTE]
  >La limpieza de integridad de datos solo puede validar los datos de archivo de los archivos en los que están habilitadas las secuencias de integridad.

De forma predeterminada, el limpiador se ejecuta cada cuatro semanas, aunque este intervalo se puede configurar dentro de Programador de tareas en Microsoft\Windows\Data Integrity Scan.

## <a name="examples"></a>Ejemplos
Para supervisar y cambiar la configuración de la integridad de datos de archivo, ReFS usa los cmdlets de **Get-FileIntegrity** y **Set-FileIntegrity** cmdlets.

### <a name="get-fileintegrity"></a>Get-FileIntegrity
Para ver si se habilitan las secuencias de integridad para los datos de archivos, usa el cmdlet **Get FileIntegrity**.

```PowerShell
PS C:\> Get-FileIntegrity -FileName 'C:\Docs\TextDocument.txt'
```

También puedes usar el cmdlet **Get-Item** para obtener la configuración de las secuencias de integridad para todos los archivos de un directorio específico.

```PowerShell
PS C:\> Get-Item -Path 'C:\Docs\*' | Get-FileIntegrity
```

### <a name="set-fileintegrity"></a>Set-FileIntegrity
Para habilitar/deshabilitar las secuencias de integridad para los datos de archivos, usa el cmdlet **Set-FileIntegrity**.

```PowerShell
PS C:\> Set-FileIntegrity -FileName 'H:\Docs\TextDocument.txt' -Enable $True
```

También puedes usar el cmdlet **Get-Item** para establecer configuración de las secuencias de integridad para todos los archivos de una carpeta específica.

```PowerShell
PS C:\> Get-Item -Path 'H\Docs\*' | Set-FileIntegrity -Enable $True
```

El cdmlet **Set-FileIntegrity** puede usarse directamente en volúmenes y directorios.

```PowerShell
PS C:\> Set-FileIntegrity H:\ -Enable $True
PS C:\> Set-FileIntegrity H:\Docs -Enable $True
```

## <a name="additional-references"></a>Referencias adicionales

-   [Información general de ReFS](refs-overview.md)
-   [Clonación de bloques de ReFS](block-cloning.md)
-   [Introducción a Espacios de almacenamiento directo](../storage-spaces/storage-spaces-direct-overview.md)
