---
title: Introducción a NTFS
description: Explicación de novedades de NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885376"
---
# <a name="ntfs-overview"></a>Introducción a NTFS

>Se aplica a: Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS: sistema de archivos principal para las versiones recientes de Windows y Windows Server, proporciona un conjunto completo de características que incluyen descriptores de seguridad, cifrado, cuotas de disco y metadatos enriquecidos y puede utilizarse con volúmenes compartidos de clúster (CSV) para proporcionar continuamente volúmenes disponibles que pueden tener acceso simultáneamente desde varios nodos de un clúster de conmutación por error.

Para obtener más información acerca de las funciones nuevas y modificadas de NTFS en Windows Server 2012 R2, vea [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para obtener información adicional de característica, consulte el [información adicional](#additional-information) sección de este tema. Para obtener más información sobre el sistema de archivos resistente (ReFS) más reciente, consulte [información general del sistema de archivos resistente (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Aplicaciones prácticas

### <a name="increased-reliability"></a>Mayor confiabilidad

NTFS usa su información de registro de archivo y el punto de control para restaurar la coherencia del sistema de archivos cuando se reinicia el equipo después de un error del sistema. Después de un error de sector defectuoso, NTFS reasigna dinámicamente el clúster que contiene el sector defectuoso, asigna un nuevo clúster para los datos, marca el clúster original como no válido y ya no usa el clúster anterior. Por ejemplo, después de un bloqueo del servidor, NTFS pueden recuperar datos mediante la reproducción de sus archivos de registro.

NTFS continuamente supervisa y corrige los problemas de daños transitorios en segundo plano sin tener que desconectar el volumen (esta característica se conoce como [Autorrecuperación NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introducidos en Windows Server 2008). Para problemas de daños en los más grandes, la utilidad Chkdsk, en Windows Server 2012 y versiones posteriores, se examina y analiza la unidad mientras el volumen está en línea, limitar el tiempo sin conexión en el tiempo necesario para restaurar la coherencia de datos en el volumen. Cuando se usa NTFS con volúmenes compartidos de clúster, no se requiere ningún tiempo de inactividad. Para obtener más información, consulte [NTFS Health and Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Aumentar la seguridad

- **Obtener acceso a la seguridad basada en ACL de la lista de Control de archivos y carpetas**— NTFS le permite establecer permisos en un archivo o carpeta, especifique los grupos y usuarios cuyo acceso desea restringir o permitir el y seleccione el tipo de acceso.

- **Compatibilidad con cifrado de unidad BitLocker**: cifrado de unidad BitLocker proporciona seguridad adicional para la información del sistema críticos y otros datos almacenados en volúmenes NTFS. A partir de Windows Server 2012 R2 y Windows 8.1, BitLocker ofrece compatibilidad para el cifrado de dispositivo en x86 y equipos basados en x64 64 con un módulo de plataforma segura (TPM) que admite el modo de espera conectado (anteriormente disponible solo en dispositivos Windows RT). Cifrado de dispositivo ayuda a proteger los datos en equipos basados en Windows, y ayuda a impedir que los usuarios malintencionados obtengan acceso a los archivos del sistema que necesitan para averiguar la contraseña del usuario, o tenga acceso a una unidad por físicamente quitarlo desde el equipo e instalarla en un uno diferente. Para obtener más información, consulte [cuáles son las novedades en BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Compatibilidad con volúmenes grandes**— NTFS puede admitir volúmenes tan grandes como 256 terabytes. Admite los tamaños se ven afectados por el tamaño del clúster y el número de clústeres del volumen. Con (2<sup>32</sup> – 1) se admiten clústeres (el número máximo de clústeres que admite NTFS), el siguiente volumen y tamaños de archivo.

  |Tamaño del clúster|Volumen más grande|Archivo de mayor tamaño|
  |---|---|---|
  |4 KB (tamaño predeterminado)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamaño máximo)|256 TB|256 TB|

>[!IMPORTANT]
>Aplicaciones y servicios pueden imponer límites adicionales sobre los tamaños de archivo y el volumen. Por ejemplo, el límite de tamaño del volumen es 64 TB si usa la característica versiones anteriores o una aplicación de copia de seguridad que hace uso de las instantáneas del servicio de instantáneas de volumen (VSS) (y no usa un alojamiento RAID o SAN). Sin embargo, es posible que deba usar tamaños más pequeños de volumen según la carga de trabajo y el rendimiento del almacenamiento.

### <a name="formatting-requirements-for-large-files"></a>Requisitos de formato para archivos de gran tamaño

Para permitir que la extensión adecuada de los archivos .vhdx de gran tamaño, existen nuevas recomendaciones para dar formato a volúmenes. Al dar formato a volúmenes que se usará con desduplicación de datos o va a hospedar archivos muy grandes, como los archivos .vhdx más de 1 TB, use el **Format-Volume** cmdlet de Windows PowerShell con los siguientes parámetros.

|Parámetro|Descripción|
|---|---|
|-AllocationUnitSize 64KB|Establece un tamaño de unidad de asignación de NTFS de 64 KB.|
|-UseLargeFRS|Permite la compatibilidad con segmentos de registro de archivos de gran tamaño (FRS). Esto es necesario para aumentar el número de extensiones permitidas por cada archivo en el volumen. Los registros de FRS grandes, el límite aumenta desde extensiones aproximadamente 1,5 millones a extensiones de aproximadamente 6 millones.|

Por ejemplo, los siguientes formatos de cmdlet unidad D como un volumen NTFS con FRS habilitado y un tamaño de unidad de asignación de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

También puede usar el **formato** comando. En un símbolo del sistema, escriba el siguiente comando, donde **/l** da formato a un gran volumen de FRS y **/A:64 k** establece un tamaño de unidad de asignación de 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Ruta de acceso y nombre de archivo máximo

NTFS admite nombres largos de archivo y rutas de acceso de longitud extendido, con los valores máximos siguientes:

- **Compatibilidad con nombres de archivo largos, con compatibilidad con versiones anteriores de**— NTFS permite nombres de archivo largos, almacenar un 8.3 alias en disco (Unicode) para ofrecer compatibilidad con sistemas de archivos que impone un límite 8.3 en los nombres de archivo y extensiones. Si es necesario (por motivos de rendimiento), puede deshabilitar de forma selectiva alias 8.3 en volúmenes NTFS individuales en Windows Server 2008 R2, Windows 8 y versiones más recientes del sistema operativo Windows.
  En Windows Server 2008 R2 y sistemas posteriores, los nombres cortos están deshabilitados de forma predeterminada cuando se formatea un volumen mediante el sistema operativo. Compatibilidad de aplicaciones, los nombres cortos todavía están habilitados en el volumen del sistema.

- **Compatibilidad con las rutas de acceso extendido-longitud**: las funciones de muchas API de Windows tienen versiones Unicode que permiten una ruta de acceso extendido-longitud de aproximadamente 32.767 caracteres, más allá del límite de 260 caracteres ruta definido por el número máximo de\_configuración de la ruta de acceso. Para nombre de archivo detallado y los requisitos de formato de ruta de acceso e instrucciones para implementar las rutas de acceso de longitud extendidos, vea [nomenclatura de archivos, rutas de acceso y espacios de nombres](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **En el clúster de almacenamiento**: cuando se utiliza en los clústeres de conmutación por error, NTFS admite volúmenes disponibles continuamente que pueden tener acceso a varios nodos de clúster simultáneamente cuando se usa junto con el sistema de archivos de volúmenes compartidos de clúster (CSV). Para obtener más información, consulte [Use Cluster Shared Volumes en un clúster de conmutación por error](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Asignación flexible de capacidad

Si se limita el espacio en un volumen, NTFS proporciona los siguientes métodos para trabajar con la capacidad de un servidor de almacenamiento:

- Utilice las cuotas de disco para realizar un seguimiento y controlar el uso de espacio en disco en volúmenes NTFS para usuarios individuales.
- Usar compresión de archivo del sistema para maximizar la cantidad de datos que se pueden almacenar.
- Aumentar el tamaño de un volumen NTFS mediante la adición de espacio sin asignar desde el mismo disco o desde un disco diferente.
- Montar un volumen en una carpeta vacía en un volumen NTFS local si necesita crear espacio adicional que sea accesible desde una carpeta existente o quedarse sin letras de unidad.

## <a name="additional-information"></a>Información adicional

|Tipo de contenido|Referencias|
|---|---|
|Evaluación|- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [Chkdsk y el estado NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [Recuperación automática de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introducida en Windows Server 2008)<br>- [NTFS transaccional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introducida en Windows Server 2008)|
|Recursos de la comunidad|- [Blog del equipo de almacenamiento de Windows](https://blogs.msdn.microsoft.com/san/)|
|Tecnologías relacionadas|- [Almacenamiento en Windows Server](../storage.md)<br>- [Clúster de uso de volúmenes compartidos en un clúster de conmutación por error](../../failover-clustering/failover-cluster-csvs.md)<br>-El [volúmenes compartidos de clúster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) y [diseño de almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) secciones de [diseñar la infraestructura de nube](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Espacios de almacenamiento](../storage-spaces/overview.md)<br>- [Introducción a resistente (ReFS) del sistema de archivos](../refs/refs-overview.md)
