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
ms.openlocfilehash: 450e5a4e800a8631adacb9826db63e747ecce4e1
ms.sourcegitcommit: 568b924d32421256f64abfee171304f1daf320d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "85070530"
---
# <a name="ntfs-overview"></a>Introducción a NTFS

>Se aplica a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS (sistema de archivos principal para las versiones recientes de Windows y Windows Server) proporciona un conjunto completo de características, incluidos descriptores de seguridad, cifrado, cuotas de disco y metadatos enriquecidos, y que se pueden usar con Volúmenes compartidos de clúster (CSV) para proporcionar volúmenes disponibles continuamente a los que se puede tener acceso simultáneamente desde varios nodos de un clúster de conmutación por error.

Para obtener más información sobre la funcionalidad nueva y modificada en NTFS en Windows Server 2012 R2, consulta [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para obtener información adicional sobre las características, consulte la sección [Información adicional](#additional-information) de este tema. Para obtener más información sobre el Sistema de archivos resistente (ReFS) más reciente, consulta [Información general del Sistema de archivos resistente (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Aplicaciones prácticas

### <a name="increased-reliability"></a>Una mayor confiabilidad

NTFS usa su archivo de registro y la información de punto de control para restaurar la coherencia del sistema de archivos cuando el equipo se reinicia después de un error del sistema. Después de un error de sector defectuoso, NTFS reasigna dinámicamente el clúster que contiene el sector defectuoso, asigna un nuevo clúster para los datos, marca el clúster original como defectuoso y ya no usa el clúster anterior. Por ejemplo, después de un bloqueo del servidor, NTFS puede recuperar los datos mediante la reproducción de los archivos del registro.

NTFS supervisa y corrige continuamente los problemas de daños transitorios en segundo plano sin desconectar el volumen (esta característica se conoce como [NTFS de recuperación automática](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), presentada en Windows Server 2008). En el caso de problemas de daños mayores, la utilidad Chkdsk, en Windows Server 2012 y versiones posteriores, examina y analiza la unidad mientras el volumen está en línea, limitando el tiempo sin conexión al necesario para restaurar la coherencia de los datos en el volumen. Cuando NTFS se usa con Volúmenes compartidos de clúster, no se requiere tiempo de inactividad. Para más información, consulta [Mantenimiento de NTFS y Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Mayor seguridad

- **Seguridad basada en la lista de control de acceso (ACL) para archivos y carpetas**: NTFS permite establecer permisos en un archivo o carpeta, especificar los grupos y usuarios cuyo acceso quieres restringir o permitir, y seleccionar el tipo de acceso.

- **Compatibilidad con Cifrado de unidad BitLocker**: El Cifrado de unidad BitLocker proporciona seguridad adicional para la información crítica del sistema y otros datos almacenados en volúmenes NTFS. A partir de Windows Server 2012 R2 y Windows 8.1, BitLocker admite el cifrado de dispositivos en equipos basados tanto en x86 como en x64 con un Módulo de plataforma segura (TPM) compatible con el modo de espera conectado (anteriormente disponible solo en dispositivos con Windows RT). El cifrado de dispositivos ayuda a proteger los datos en equipos basados en Windows, y ayuda a bloquear los usuarios malintencionados para que no accedan a los archivos del sistema que necesitan para detectar la contraseña del usuario ni accedan a una unidad de disco si la quitan físicamente del equipo y la instalan en otro. Para obtener más información, consulte el tema sobre [Novedades de BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Compatibilidad con volúmenes grandes**: NTFS puede admitir volúmenes de hasta 256 terabytes. Los tamaños de volumen admitidos se ven afectados por el tamaño del clúster y el número de clústeres. Con (2<sup>32</sup> – 1) clústeres (el número máximo de clústeres que admite NTFS), se admiten los siguientes tamaños de volumen y archivo.

  |Tamaño de clúster|Volumen más grande|Archivo más grande|
  |---|---|---|
  |4 KB (tamaño predeterminado)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamaño máximo)|256 TB|256 TB|

>[!IMPORTANT]
>Los servicios y las aplicaciones pueden imponer límites adicionales a los tamaños de archivo y volumen. Por ejemplo, el límite de tamaño de un volumen es de 64 TB si usas la característica Versiones anteriores o una aplicación de copia de seguridad que usa instantáneas del Servicio de instantáneas de volumen (VSS) (y no usas un gabinete SAN o RAID). Sin embargo, es posible que tengas que usar tamaños de volumen más pequeños en función de la carga de trabajo y el rendimiento del almacenamiento.

### <a name="formatting-requirements-for-large-files"></a>Requisitos de formato para archivos de gran tamaño

Para permitir una extensión adecuada de archivos .vhdx grandes, hay nuevas recomendaciones para formatear los volúmenes. Al formatear los volúmenes que se usarán con Desduplicación de datos o que hospedarán archivos muy grandes, como archivos .vhdx de más de 1 TB, usa el cmdlet **Format-Volume** en Windows PowerShell con los parámetros siguientes.

|Parámetro|Descripción|
|---|---|
|-AllocationUnitSize 64KB|Establece un tamaño de unidad de asignación de NTFS de 64 KB:|
|-UseLargeFRS|Habilita la compatibilidad con segmentos de registro de archivos (FRS) de gran tamaño. Esto es necesario para aumentar el número de extensiones permitidas por archivo en el volumen. En el caso de los registros FRS grandes, el límite aumenta de aproximadamente 1,5 millones de extensiones a aproximadamente 6 millones.|

Por ejemplo, el siguiente cmdlet formatea la unidad D como volumen NTFS, con FRS habilitado y un tamaño de unidad de asignación de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

También puedes usar el comando **format**. En una ventana del símbolo del sistema, escribe el siguiente comando, donde **/L** formatea un volumen FRS grande, y **/A:64k** establece un tamaño de unidad de asignación de 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Nombre y ruta de acceso máximos de un archivo

NTFS admite nombres de archivo largos y rutas de acceso de longitud ampliada, con los siguientes valores máximos:

- **Compatibilidad con nombres de archivo largos, con compatibilidad con versiones anteriores**: NTFS permite nombres de archivo largos, y almacena un alias 8.3 en disco (en Unicode) para proporcionar compatibilidad con sistemas de archivos que imponen un límite de 8.3 en los nombres de archivo y las extensiones. Si es necesario (por motivos de rendimiento), puedes deshabilitar selectivamente el alias 8.3 en volúmenes NTFS individuales en Windows Server 2008 R2, Windows 8 y versiones más recientes del sistema operativo Windows.
  En los sistemas Windows Server 2008 R2 y versiones posteriores, los nombres cortos están deshabilitados de forma predeterminada cuando un volumen se formatea con el sistema operativo. En cuanto a la compatibilidad de aplicaciones, los nombres cortos todavía están habilitados en el volumen del sistema.

- **Compatibilidad con rutas de acceso de longitud extendida**: Muchas funciones de la API de Windows tienen versiones Unicode que permiten una ruta de longitud extendida de aproximadamente 32 767 caracteres, más allá del límite de la ruta de acceso de 260 caracteres definido por la opción MAX\_PATH. Para obtener información detallada sobre los requisitos de formato de nombre de archivo y de ruta de acceso, así como instrucciones para implementar rutas de acceso de longitud extendida, consulta [Asignación de nombres a archivos, rutas de acceso y espacios de nombres](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Almacenamiento en clúster**: Cuando NTFS se usa en clústeres de conmutación por error, admite volúmenes disponibles continuamente a los que pueden acceder varios nodos de clúster simultáneamente cuando se usan junto con el sistema de archivos de Volúmenes compartidos de clúster (CSV). Para obtener más información, consulta [Uso de Volúmenes compartidos de clúster en un clúster de conmutación por error](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Asignación flexible de capacidad

Si el espacio de un volumen está limitado, NTFS proporciona las siguientes formas de trabajar con la capacidad de almacenamiento de un servidor:

- Usar cuotas de disco para realizar el seguimiento y controlar el uso del espacio de disco en volúmenes NTFS para usuarios individuales.
- Usar la compresión del sistema de archivos para maximizar la cantidad de datos que se pueden almacenar.
- Aumentar el tamaño de un volumen NTFS agregando espacio sin asignar del mismo disco o de otro disco.
- Montar un volumen en cualquier carpeta vacía en un volumen NTFS local si se agotan las letras de unidad o tienes que crear espacio adicional al que se pueda acceder desde una carpeta existente.

## <a name="additional-information"></a>Información adicional

- [Recomendaciones de tamaño de clúster para ReFS y NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Información general del Sistema de archivos resistente (ReFS)](../refs/refs-overview.md)
- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Novedades de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Mantenimiento de NTFS y Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [NTFS de recuperación automática](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (que se presentó en Windows Server 2008)
- [NTFS transaccional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (que se presentó en Windows Server 2008)
- [Almacenamiento en Windows Server](../storage.yml)