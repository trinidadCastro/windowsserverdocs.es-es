---
title: Introducción a NTFS
description: Explicación de lo que es NTFS.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: b98877d0a94ff8033b65bf74d0118e2a5f1ea092
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402078"
---
# <a name="ntfs-overview"></a>Introducción a NTFS

>Se aplica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

NTFS (el sistema de archivos principal para las versiones recientes de Windows y Windows Server) proporciona un conjunto completo de características, como descriptores de seguridad, cifrado, cuotas de disco y metadatos enriquecidos, y se puede usar con volúmenes compartidos de clúster (CSV) para proporcionar de forma continua volúmenes disponibles a los que se puede tener acceso simultáneamente desde varios nodos de un clúster de conmutación por error.

Para obtener más información acerca de la funcionalidad nueva y modificada en NTFS en Windows Server 2012 R2, consulte [novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para obtener información adicional sobre las características, consulte la sección [información adicional](#additional-information) de este tema. Para obtener más información sobre el sistema de archivos resistente (ReFS) más reciente, consulte [información general del sistema de archivos resistente (Refs)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Aplicaciones prácticas

### <a name="increased-reliability"></a>Una mayor confiabilidad

NTFS utiliza su archivo de registro y la información de punto de control para restaurar la coherencia del sistema de archivos cuando el equipo se reinicia después de un error del sistema. Después de un error de sector incorrecto, NTFS reasigna dinámicamente el clúster que contiene el sector defectuoso, asigna un nuevo clúster para los datos, marca el clúster original como incorrecto y ya no usa el clúster anterior. Por ejemplo, después de un bloqueo del servidor, NTFS puede recuperar los datos mediante la reproducción de los archivos de registro.

NTFS supervisa y corrige continuamente los problemas de daños transitorios en segundo plano sin desconectar el volumen (esta característica se conoce como [NTFS de recuperación automática](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), presentada en Windows Server 2008). En el caso de problemas de daños mayores, la utilidad Chkdsk, en Windows Server 2012 y versiones posteriores, examina y analiza la unidad mientras el volumen está en línea, limitando el tiempo sin conexión al tiempo necesario para restaurar la coherencia de los datos en el volumen. Cuando NTFS se usa con volúmenes compartidos de clúster, no se requiere tiempo de inactividad. Para obtener más información, vea [Estado de NTFS y CHKDSK](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Mayor seguridad

- **Seguridad basada en la lista de Access Control (ACL) para archivos y carpetas**: NTFS le permite establecer permisos en un archivo o carpeta, especificar los grupos y usuarios cuyo acceso desea restringir o permitir y seleccionar tipo de acceso.

- **Compatibilidad con cifrado de unidad BitLocker**: cifrado de unidad BitLocker proporciona seguridad adicional para la información crítica del sistema y otros datos almacenados en volúmenes NTFS. A partir de Windows Server 2012 R2 y Windows 8.1, BitLocker ofrece compatibilidad con el cifrado de dispositivos en equipos basados en x86 y x64 con un Módulo de plataforma segura (TPM) compatible con conectado (anteriormente solo disponible en dispositivos Windows RT). El cifrado de dispositivo ayuda a proteger los datos en equipos basados en Windows y ayuda a impedir que los usuarios malintencionados accedan a los archivos del sistema en los que confían para detectar la contraseña del usuario o a una unidad de disco si lo quitan físicamente del equipo y lo instalan en un diferente. Para obtener más información, vea [novedades de BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Compatibilidad con volúmenes grandes**: NTFS puede admitir volúmenes de hasta 256 terabytes. Los tamaños de volúmenes admitidos se ven afectados por el tamaño del clúster y el número de clústeres. Con (2<sup>32</sup> – 1) clústeres (el número máximo de clústeres que admite NTFS), se admiten los siguientes tamaños de volumen y archivo.

  |Tamaño del clúster|Mayor volumen|Archivo más grande|
  |---|---|---|
  |4 KB (tamaño predeterminado)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamaño máximo)|256 TB|256 TB|

>[!IMPORTANT]
>Los servicios y las aplicaciones pueden imponer límites adicionales en los tamaños de archivos y volúmenes. Por ejemplo, el límite de tamaño del volumen es 64 TB si usa la característica versiones anteriores o una aplicación de copia de seguridad que hace uso de instantáneas de Servicio de instantáneas de volumen (VSS) (y no está usando un gabinete SAN o RAID). Sin embargo, es posible que tenga que usar tamaños de volumen más pequeños en función de la carga de trabajo y el rendimiento del almacenamiento.

### <a name="formatting-requirements-for-large-files"></a>Requisitos de formato para archivos de gran tamaño

Para permitir una extensión adecuada de archivos. vhdx grandes, hay nuevas recomendaciones para dar formato a los volúmenes. Al dar formato a los volúmenes que se usarán con desduplicación de datos o hospedará archivos muy grandes, como archivos. vhdx de más de 1 TB, use el cmdlet **Format-Volume** en Windows PowerShell con los parámetros siguientes.

|Parámetro|Descripción|
|---|---|
|-AllocationUnitSize 64 KB|Establece un tamaño de unidad de asignación NTFS de 64 KB.|
|-UseLargeFRS|Habilita la compatibilidad con segmentos de registro de archivos (FRS) de gran tamaño. Esto es necesario para aumentar el número de extensiones permitidas por archivo en el volumen. En el caso de los registros FRS grandes, el límite aumenta de aproximadamente 1,5 millones extensiones a aproximadamente 6 millones.|

Por ejemplo, el siguiente cmdlet da formato a la unidad D como un volumen NTFS, con FRS habilitado y un tamaño de unidad de asignación de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

También puede usar el comando **Format** . En una ventana del símbolo del sistema, escriba el siguiente comando, donde **/l** da formato a un volumen FRS grande y **/a: 64k** establece un tamaño de unidad de asignación de 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Nombre y ruta de acceso de archivo máximos

NTFS admite nombres de archivo largos y rutas de acceso de longitud extendida, con los siguientes valores máximos:

- **Compatibilidad con nombres de archivo largos, con compatibilidad con versiones anteriores**: NTFS permite nombres de archivo largos, almacenando un alias de 8,3 en disco (en Unicode) para proporcionar compatibilidad con sistemas de archivos que imponen un límite de 8,3 en los nombres de archivo y las extensiones. Si es necesario (por motivos de rendimiento), puede deshabilitar selectivamente el alias 8,3 en volúmenes NTFS individuales en Windows Server 2008 R2, Windows 8 y versiones más recientes del sistema operativo Windows.
  En los sistemas Windows Server 2008 R2 y versiones posteriores, los nombres cortos están deshabilitados de forma predeterminada cuando se da formato a un volumen mediante el sistema operativo. En cuanto a la compatibilidad de aplicaciones, los nombres cortos todavía están habilitados en el volumen del sistema.

- **Compatibilidad con rutas de acceso de longitud extendida**: muchas funciones de la API de Windows tienen versiones Unicode que permiten una ruta de acceso de longitud extendida de aproximadamente 32.767 caracteres, más allá\_del límite de la ruta de acceso de caracteres 260 definida por la configuración de ruta de acceso máxima. Para obtener información detallada sobre los requisitos de nombre de archivo y formato de ruta de acceso, así como instrucciones para implementar rutas de acceso de longitud extendida, vea asignar nombres a [archivos, rutas de acceso y espacios de nombres](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Almacenamiento en clúster**: cuando se usa en clústeres de conmutación por error, NTFS admite volúmenes disponibles continuamente a los que pueden tener acceso varios nodos de clúster simultáneamente cuando se usan junto con el sistema de archivos de volúmenes compartidos de clúster (CSV). Para obtener más información, consulte [usar volúmenes compartidos de clúster en un clúster de conmutación por error](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Asignación flexible de capacidad

Si el espacio de un volumen es limitado, NTFS proporciona las siguientes formas de trabajar con la capacidad de almacenamiento de un servidor:

- Use cuotas de disco para realizar el seguimiento y controlar el uso del espacio de disco en volúmenes NTFS para usuarios individuales.
- Use la compresión del sistema de archivos para maximizar la cantidad de datos que se pueden almacenar.
- Aumente el tamaño de un volumen NTFS agregando espacio sin asignar del mismo disco o de un disco diferente.
- Montaje de un volumen en cualquier carpeta vacía en un volumen NTFS local si se agotan las letras de unidad o es necesario crear espacio adicional accesible desde una carpeta existente.

## <a name="additional-information"></a>Información adicional

- [Recomendaciones de tamaño de clúster para ReFS y NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Introducción al sistema de archivos resistente (ReFS)](../refs/refs-overview.md)
- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Mantenimiento y CHKDSK de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [NTFS de recuperación automática](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introducido en Windows Server 2008)
- [NTFS transaccional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introducido en Windows Server 2008)
- [Almacenamiento en Windows Server](../storage.md)