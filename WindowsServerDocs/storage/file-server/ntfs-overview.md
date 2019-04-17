---
title: Introducción a NTFS
description: Una explicación de NTFS es.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122108"
---
# Introducción a NTFS

>Se aplica a: Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS, el sistema de archivos principal para las versiones recientes de Windows y Windows Server, proporciona un conjunto completo de características que incluyen descriptores de seguridad, cifrado, cuotas de disco y metadatos enriquecidos y puede usarse con volúmenes compartidos de clúster (CSV) para proporcionar continuamente volúmenes disponibles que pueden tener acceso simultáneamente desde varios nodos de un clúster de conmutación por error.

Para obtener más información sobre las funcionalidades nuevas y modificadas en el sistema NTFS en Windows Server 2012 R2, consulte [What ' s New in NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para obtener información de la característica adicional, consulta la sección [información adicional](#additional-information) de este tema. Para obtener más información sobre el sistema de archivos resistente (ReFS) más recientes, consulta la [información general del sistema de archivos resistente (ReFS)](../refs/refs-overview.md).

## Aplicaciones prácticas

### Aumentar la confiabilidad

NTFS usa su información de punto de control y archivos de registro para restaurar la coherencia del sistema de archivos cuando se reinicia el equipo después de un error del sistema. Después de un error de sector defectuoso, NTFS reasigna dinámicamente el clúster que contiene los sectores defectuosos, asigna un nuevo clúster para los datos, marca el clúster original como defectuosos y ya no usa el clúster antiguo. Por ejemplo, después de un bloqueo de servidor, NTFS puede recuperar datos mediante la reproducción de sus archivos de registro.

NTFS continuamente supervisa y corrige los problemas de daños transitorio en segundo plano sin poner el volumen sin conexión (esta característica se conoce como [NTFS de recuperación automática](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), que se introdujo en Windows Server 2008). Para los problemas de daños más grandes, la utilidad Chkdsk, en Windows Server 2012 y versiones posteriores, analiza y analiza la unidad mientras el volumen está conectado, limitar el tiempo sin conexión en el tiempo necesario para restaurar la coherencia de datos en el volumen. Cuando se usa NTFS con volúmenes compartidos de clúster, no es necesario ningún tiempo de inactividad. Para obtener más información, consulta el [estado de NTFS y Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### Una mayor seguridad

- **Seguridad basada en la lista de Control de acceso ACL de archivos y carpetas**: NTFS te permite establecer permisos en un archivo o carpeta, especificar los grupos y usuarios cuyo acceso que quieras restringir o permitir y selecciona el tipo de acceso.

- **Compatibilidad con el cifrado de unidad BitLocker**: cifrado de unidad BitLocker proporciona seguridad adicional para obtener información de críticos del sistema y otros datos almacenados en volúmenes NTFS. A partir de Windows Server 2012 R2 y Windows 8.1, BitLocker proporciona compatibilidad para el cifrado de dispositivo en x86 y equipos basados en x64 con un módulo de plataforma segura (TPM) que admite conectado stand-by (anteriormente disponible únicamente en dispositivos Windows RT). Cifrado de dispositivos ayuda a proteger datos en equipos basados en Windows, y ayuda a impedir que los usuarios malintencionados obtengan acceso a los archivos del sistema que necesitan para detectar la contraseña del usuario, u obtengan acceso a una unidad físicamente quitarlo desde el equipo y lo instalen en una otra. Para obtener más información, consulte [Novedades de BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Compatibilidad con volúmenes grandes**: NTFS pueden admitir volúmenes tan grandes como 256 terabytes. Admite tamaños se ven afectados por el tamaño de clúster y el número de clústeres de volumen. Con (2<sup>32</sup> -1) se admiten clústeres (el número máximo de clústeres que admita NTFS), el siguiente volumen y tamaños de archivo.

  |Tamaño de clúster|Volumen más grande|Archivo más grande|
  |---|---|---|
  |4 KB (tamaño predeterminado)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamaño máximo)|256TB|256TB|

>[!IMPORTANT]
>Las aplicaciones y servicios impone límites adicionales en tamaños de archivo y el volumen. Por ejemplo, el límite de tamaño de volumen es 64 TB si estás usando la característica de versiones anteriores o una aplicación de copia de seguridad que hace uso de instantáneas de servicio de instantáneas de volumen (VSS) (y no estás usando un alojamiento de SAN o RAID). Sin embargo, es posible que debes usar tamaños más pequeños de volumen según la carga de trabajo y el rendimiento de almacenamiento.

### Requisitos de formato de archivos de gran tamaño

Para permitir la extensión adecuada de los archivos de grande .vhdx, hay nuevas recomendaciones para dar formato a los volúmenes. Al dar formato a los volúmenes que se usará con desduplicación de datos o se alojarán en archivos muy grandes, como archivos .vhdx mayores que 1 TB, use el cmdlet de **Formato de volumen** en Windows PowerShell con los siguientes parámetros.

|Parámetro|Descripción|
|---|---|
|-AllocationUnitSize 64KB|Establece un tamaño de unidad de asignación de 64 KB NTFS.|
|-UseLargeFRS|Permite la admisión de segmentos de registro de archivos de gran tamaño (FRS). Esto es necesario para aumentar el número de extensiones permitidas por cada archivo en el volumen. Para los registros de FRS grandes, el límite se aumenta de extensiones de aproximadamente 1,5 millones a aproximadamente 6 millones extensiones.|

Por ejemplo, los siguientes formatos de cmdlet unidad D como un volumen NTFS con FRS habilitado y un tamaño de unidad de asignación de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

También puedes usar el comando de **formato** . En un símbolo del sistema, escribe el comando siguiente, **donde/l** da formato a un gran volumen de FRS y **/A:64 k** establece un tamaño de unidad de asignación de 64 KB:

```PowerShell
format /L /A:64k
```

### Ruta de acceso y nombre de archivo máximo

NTFS es compatible con los nombres de archivo largos y rutas de acceso de longitud extendido, con los valores máximos siguientes:

- **Soporte técnico para los nombres de archivo largos, con compatibilidad con versiones anteriores**: NTFS permite que los nombres de archivo largos, almacenar una 8.3 alias en el disco (en Unicode) para proporcionar compatibilidad con sistemas de archivos que impone un límite 8.3 en los nombres de archivo y extensiones. Si es necesario (por motivos de rendimiento), puede deshabilitar selectivamente aliasing 8.3 en volúmenes NTFS individuales en Windows Server 2008 R2, Windows 8 y versiones más recientes del sistema operativo Windows.
  En Windows Server 2008 R2 y sistemas posteriores, los nombres cortos se deshabilitan de manera predeterminada cuando un volumen está formateado con el sistema operativo. Compatibilidad de aplicaciones, los nombres cortos todavía están habilitados en el volumen del sistema.

- **Compatibilidad con las rutas de acceso de longitud extendido**: funciones muchas API de Windows tienen versiones de Unicode que permiten una ruta de acceso de longitud extendido de aproximadamente 32.767 caracteres, más allá del límite de ruta de acceso de 260 caracteres definido por la configuración de MAX\_PATH. Para el nombre de archivo detallado y requisitos de formato de ruta de acceso e instrucciones para implementar las rutas de acceso de longitud extendido, consulta [los archivos de nombres, Paths and Namespaces](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Almacenamiento de Clustered**: cuando se usa en clústeres de conmutación por error, NTFS admite volúmenes disponibles continuamente que se puedan acceder por varios nodos de clúster simultáneamente cuando se usa junto con el sistema de archivos de volúmenes compartidos de clúster (CSV). Para obtener más información, consulta [Volúmenes compartidos de clúster usa en un clúster de conmutación por error](../../failover-clustering/failover-cluster-csvs.md).

### Asignación flexible de capacidad

Si el espacio en un volumen está limitado, NTFS proporciona las siguientes maneras para trabajar con la capacidad de almacenamiento de un servidor:

- Usa las cuotas de disco para realizar un seguimiento y controlar el uso del espacio en disco en volúmenes NTFS para los usuarios individuales.
- Usar la compresión de sistema de archivos para maximizar la cantidad de datos que se pueden almacenar.
- Aumentar el tamaño de un volumen NTFS mediante la adición de un espacio sin asignar desde el mismo disco o desde un disco diferente.
- Montar un volumen en cualquier carpeta vacía en un volumen NTFS local si quedarse sin letras de unidad o necesitas crear espacio adicional que sea accesible desde una carpeta existente.

## Información adicional

|Tipo de contenido|Referencias|
|---|---|
|Evaluación|- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [Chkdsk y estado NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [Recuperación automática de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introducida en Windows Server 2008)<br>- [NTFS transaccional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introducida en Windows Server 2008)|
|Recursos de la comunidad|- [Blog del equipo de almacenamiento de Windows](https://blogs.msdn.microsoft.com/san/)|
|Tecnologías relacionadas|- [Almacenamiento en Windows Server](../storage.md)<br>- [Clúster de uso compartido de volúmenes en un clúster de conmutación por error](../../failover-clustering/failover-cluster-csvs.md)<br>-Las secciones de [Diseño del almacenamiento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) y los [Volúmenes compartidos de clúster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) de [Diseño de la infraestructura de nube](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Espacios de almacenamiento](../storage-spaces/overview.md)<br>- [Información general resistente (ReFS) del sistema de archivos](../refs/refs-overview.md)
