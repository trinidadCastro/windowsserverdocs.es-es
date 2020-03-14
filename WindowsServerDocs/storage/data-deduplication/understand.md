---
ms.assetid: acc0803b-fa05-4fc3-b94d-2916abf4fdbd
title: Información acerca de Desduplicación de datos
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: e26d285f1802e78b2cbf84e38407ff134e7b7035
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322397"
---
# <a name="understanding-data-deduplication"></a>Información acerca de Desduplicación de datos

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

En este documento se describe cómo funciona la [Desduplicación de datos](overview.md).

## <a name="how-does-dedup-work"></a>¿Cómo funciona desduplicación de datos?

Desduplicación de datos en Windows Server se creó con los siguientes dos principios:

1. **La optimización no debe entrar en el modo de escritura en el disco**  
    Desduplicación de datos optimiza los datos mediante un modelo de procesamiento posterior. Todos los datos se escriben sin optimizar en el disco y se optimizan más adelante con Desduplicación de datos.

2. **La optimización no debe cambiar la semántica de acceso**  
    Los usuarios y las aplicaciones que acceden a datos en un volumen optimizado desconocen por completo que los archivos a los que acceden se han desduplicado.

Cuando se ha habilitado para un volumen, Desduplicación de datos se ejecuta en segundo plano para:

- Identificar patrones repetidos en los archivos del volumen.
- Mover sin problemas las partes (o fragmentos) con punteros especiales denominados [puntos de reanálisis](#dedup-term-reparse-point) que apuntan a una copia única del fragmento.

Esto sucede en los cuatro pasos siguientes:

1. Examinar el sistema de archivos en busca de archivos que cumplan la directiva de optimización.  
![Examinar el sistema de archivos.](media/understanding-dedup-how-dedup-works-1.gif)  
2. Dividir archivos en fragmentos de tamaño variable.  
![Dividir archivos en fragmentos.](media/understanding-dedup-how-dedup-works-2.gif)
3. Identificar fragmentos únicos.  
![Identificar fragmentos únicos.](media/understanding-dedup-how-dedup-works-3.gif)
4. Colocar los fragmentos en el almacén de fragmentos y, opcionalmente, comprimir.  
![Mover al almacén de fragmentos.](media/understanding-dedup-how-dedup-works-4.gif)
5. Reemplazar el flujo de archivos original de los archivos ahora optimizados con un punto de reanálisis al almacén de fragmentos.  
![Reemplazar el flujo de archivos con el punto de reanálisis.](media/understanding-dedup-how-dedup-works-5.gif)

Cuando se leen archivos optimizados, el sistema de archivos envía los archivos con un reanálisis al filtro del sistema de archivos de Desduplicación de datos (Dedup.sys). El filtro redirige la operación de lectura a los fragmentos adecuados que constituyen el flujo de ese archivo del almacén de fragmentos. Las modificaciones de los intervalos de un archivo desduplicado se escriben sin optimizar en el disco y se optimizan mediante el [trabajo de optimización](understand.md#job-info) la próxima vez que se ejecute.

## <a id="usage-type"></a>Tipos de uso
Los siguientes Tipos de uso proporcionan una configuración razonable de Desduplicación de datos para cargas de trabajo comunes:  

| Tipo de uso | Cargas de trabajo ideales | Diferencias |
|------------|-----------------|------------------|
| <a id="usage-type-default"></a>Predeterminada | Servidor de archivos de uso general:<ul><li>Recursos compartidos del equipo</li><li>Carpetas de trabajo</li><li>Redirección de carpetas</li><li>Recursos compartidos de desarrollo de software</li></ul> | <ul><li>Optimización en segundo plano</li><li>Directiva de optimización predeterminada:<ul><li>Antigüedad mínima de archivo = 3 días</li><li>Optimizar archivos en uso = No</li><li>Optimizar archivos parciales = No</li></ul></li></ul> |
| <a id="usage-type-hyperv"></a>Hyper-V | Servidores de Infraestructura de escritorio virtual (VDI) | <ul><li>Optimización en segundo plano</li><li>Directiva de optimización predeterminada:<ul><li>Antigüedad mínima de archivo = 3 días</li><li>Optimizar archivos en uso = Sí</li><li>Optimizar archivos parciales = Yes</li></ul></li><li>Ajustes técnicos para la interoperabilidad de Hyper-V</li></ul> |
| <a id="usage-type-backup"></a>Copiar | Aplicaciones de copia de seguridad virtualizadas, como [Microsoft Data Protection Manager (DPM)](https://technet.microsoft.com/library/hh758173.aspx) | <ul><li>Optimización de prioridad</li><li>Directiva de optimización predeterminada:<ul><li>Antigüedad mínima de archivo = 0 días</li><li>Optimizar archivos en uso = Sí</li><li>Optimizar archivos parciales = No</li></ul></li><li>Ajustes técnicos para la interoperabilidad con soluciones de DPM o similares a DPM.</li></ul> |

## <a id="job-info"></a>Tareas
Desduplicación de datos usa una estrategia de procesamiento posterior para optimizar y mantener la eficacia del espacio de un volumen.

| Nombre del trabajo | Descripciones del trabajo | Programación predeterminada |
|----------|------------------|------------------|
| <a id="job-info-optimization"></a>Optimización | El trabajo **Optimización** se desduplica mediante la fragmentación de los datos de un volumen por la configuración de directiva de volumen, para lo que comprime esos fragmentos (opcionalmente) y almacena fragmentos de forma única en el almacén de fragmentos. El proceso de optimización que usa Desduplicación de datos se describe con detalle en [¿Cómo funciona Desduplicación de datos?](understand.md#how-does-dedup-work) | Una vez cada hora |
| <a id="job-info-gc"></a>Recolección de elementos no utilizados | El trabajo **Recolección de elementos no utilizados** recupera espacio en disco. Para ello, quita los fragmentos innecesarios a los que ya no hacen referencia los archivos que se han modificado o eliminado recientemente. | Todos los sábados a las 2:35 a. m. |
| <a id="job-info-scrubbing"></a>Limpieza de integridad | El trabajo **Limpieza de integridad** identifica los daños en el almacén de fragmentos debido a errores en discos o sectores incorrectos. Cuando sea posible, Desduplicación de datos puede utilizar automáticamente las características de volumen (por ejemplo, reflejo o paridad en un volumen de Espacios de almacenamiento) para reconstruir los datos dañados. Además, Desduplicación de datos mantiene copias de seguridad de fragmentos populares a los que se hace referencia más de 100 veces en un área denominada zona activa. | Todos los sábados a las 3:35 a. m. |
| <a id="job-info-unoptimization"></a>Desoptimización | El trabajo **Desoptimización**, que es un trabajo especial que solo se puede ejecutar manualmente, deshace la optimización realizada por la desduplicación y deshabilita Desduplicación de datos de ese volumen. | [Solo a petición](run.md#disabling-dedup) |

## <a id="dedup-term"></a>Terminología de desduplicación de datos
| Término | Definición |
|------|------------|
| <a id="dedup-term-chunk"></a>Paquete | Un fragmento es una sección de un archivo que se ha seleccionado por el algoritmo de fragmentación de Desduplicación de datos por ser probable que ocurra en otros archivos similares. |
| <a id="dedup-term-chunk-store"></a>Almacén de fragmentos | El almacén de fragmentos es una serie organizada de archivos contenedores en la carpeta Información de volumen de sistema que utiliza Desduplicación de datos para almacenar fragmentos de forma única. |
| <a id="dedup-term-dedup"></a>Duplica | Abreviatura de Desduplicación de datos usada con frecuencia en PowerShell y las API y los componentes de Windows Server, así como en la comunidad de Windows Server. |
| <a id="dedup-term-file-metadata"></a>Metadatos de archivo | Cada archivo contiene metadatos que describen propiedades interesantes sobre el archivo que no están relacionadas con el contenido principal del mismo. Por ejemplo, la fecha de creación, la fecha de la última lectura, el autor, etc. |
| <a id="dedup-term-file-stream"></a>Secuencia de archivos | La secuencia de archivos es el contenido principal del archivo. Esta es la parte del archivo que optimiza Desduplicación de datos. |
| <a id="dedup-term-file-system"></a>Sistema de archivos | El sistema de archivos es la estructura de datos en disco y de software que el sistema operativo usa para almacenar archivos en medios de almacenamiento. Desduplicación de datos es compatible en volúmenes con formato NTFS. |
| <a id="dedup-term-file-system-filter"></a>Filtro del sistema de archivos | Un filtro de sistema de archivos es un complemento que modifica el comportamiento predeterminado del sistema de archivos. Para conservar la semántica de acceso, Desduplicación de datos usa un filtro de sistema de archivos (Dedup.sys) para redirigir las lecturas a contenido optimizado completamente transparente para el usuario o aplicación que realiza la solicitud de lectura. |
| <a id="dedup-term-optimization"></a>Optimización | Un archivo se considera optimizado (o desduplicado) por Desduplicación de datos si se ha fragmentado y sus fragmentos únicos se han almacenado en el almacén de fragmentos. |
| <a id="dedup-term-in-policy"></a>Directiva de optimización | La directiva de optimización especifica los archivos que deben tenerse en cuenta para Desduplicación de datos. Por ejemplo, los archivos se pueden considerar fuera de directiva si son completamente nuevos, están abiertos, se encuentran en una ruta de acceso determinada del volumen o son de un tipo de archivo determinado. |
| <a id="dedup-term-reparse-point"></a>Punto de reanálisis | Un [punto de reanálisis](https://msdn.microsoft.com/library/windows/desktop/aa365503.aspx) es una etiqueta especial que notifica al sistema de archivos que pase la E/S a un filtro de sistema de archivos específico. Cuando se ha optimizado el flujo de archivos de un archivo, Desduplicación de datos reemplaza el flujo de archivos por un punto de reanálisis, lo que permite a Desduplicación de datos conservar la semántica de acceso para ese archivo. |
| <a id="dedup-term-volume"></a>Cantidad | Un volumen es una construcción de Windows para una unidad de almacenamiento lógico que puede abarcar varios dispositivos de almacenamiento físico a través de uno o varios servidores. Desduplicación está habilitada en un volumen en función de su configuración individual. |
| <a id="dedup-term-workload"></a>Duplica | Una carga de trabajo es una aplicación que se ejecuta en Windows Server. Las cargas de trabajo de ejemplo incluyen el servidor de archivos de uso general, Hyper-V y SQL Server. |

> [!Warning]  
> A menos que lo indique el personal de soporte técnico de Microsoft autorizado, no intente modificar manualmente el almacén de fragmentos. Si lo hace, podrían producirse daños en los datos o la pérdida de estos.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
**¿En qué se diferencia desduplicación de datos de otros productos de optimización?**  
Hay varias diferencias importantes entre Desduplicación de datos y otros productos de optimización de almacenamiento comunes:

* *¿En qué se diferencia desduplicación de datos del almacén de instancia única?*  
    Almacén de instancia única, o SIS, es la tecnología anterior a Desduplicación de datos que se presentó por primera vez en Windows Storage Server 2008 R2. Para optimizar un volumen, Almacén de instancia única identificaba los archivos que eran idénticos y los reemplazaba por vínculos lógicos a una única copia de un archivo almacenado en el almacén común de SIS. A diferencia del Almacén de instancia única, Desduplicación de datos puede ahorrar espacio de los archivos que no son idénticos pero comparten muchos patrones comunes, y de los archivos que contienen en sí mismos muchos patrones repetidos. Almacén de instancia única cayó en desuso en Windows Server 2012 R2 y se quitó de Windows Server 2016 en favor de Desduplicación de datos.

* *¿En qué se diferencia desduplicación de datos de la compresión NTFS?*  
    La compresión NTFS es una característica de NTFS que se puede habilitar opcionalmente en el nivel de volumen. Con la compresión NTFS, cada archivo se optimiza individualmente a través de la compresión en tiempo de escritura. A diferencia de la compresión NTFS, Desduplicación de datos puede ahorrar espacio en todos los archivos de un volumen. Esto es mejor que la compresión NTFS, ya que los archivos pueden tener <u>ambas</u> duplicaciones internas (lo cual se resuelve mediante la compresión NTFS) y presentar similitudes con otros archivos en el volumen (lo cual no se resuelve mediante la compresión NTFS). Además, Desduplicación de datos funciona con un modelo de procesamiento posterior, lo que significa que los archivos nuevos o modificados se escribirán en el disco sin optimizar y se optimizarán más adelante con Desduplicación de datos.

* *¿En qué se diferencia desduplicación de datos de formatos de archivo de almacenamiento como zip, rar, 7z, CAB, etc.?*  
    Los formatos de archivo de almacenamiento, como zip, rar, 7z, cab, etc., realizan la compresión en un conjunto de archivos especificado. Al igual que Desduplicación de datos, se optimizan tanto los patrones duplicados en los archivos como entre los archivos, pero es necesario elegir qué archivos deben incluirse en el almacenamiento. La semántica de acceso también cambia. Para acceder a un archivo específico dentro del almacenamiento, debe abrir este, seleccionar un archivo específico y descomprimir ese archivo para su uso. Desduplicación de datos funciona de forma transparente para los usuarios y administradores y no requiere ninguna intervención manual. Además, Desduplicación de datos conserva la semántica de acceso, ya que los archivos optimizados no cambian después de la optimización.

**¿Se puede cambiar la configuración de desduplicación de datos para el tipo de uso seleccionado?**  
Sí. Aunque Desduplicación de datos proporciona valores predeterminados razonables para **cargas de trabajo recomendadas**, quizá prefiera ajustar la configuración de Desduplicación de datos para sacar el máximo provecho de su almacenamiento. Además, otras cargas de trabajo [requerirán algún ajuste para asegurarse de que Desduplicación de datos no interfiera con la carga de trabajo](install-enable.md#enable-dedup-sometimes-considerations).

**¿Puedo ejecutar manualmente un trabajo de desduplicación de datos?**  
Sí, [todos los trabajos de Desduplicación de datos se pueden ejecutar manualmente](run.md#running-dedup-jobs-manually). Quizá prefiera esta opción si los trabajos programados no se ejecutaron debido a una cantidad insuficiente de recursos del sistema o a un error. Además, el trabajo Desoptimización solo puede realizarse manualmente.

**¿Se pueden supervisar los resultados históricos de los trabajos de desduplicación de datos?**  
Sí, [todos los trabajos de Desduplicación de datos dejan entradas en el registro de eventos de Windows](run.md#monitoring-dedup).

**¿Se pueden cambiar las programaciones predeterminadas para los trabajos de desduplicación de datos en el sistema?**  
Sí, [todas las programaciones son configurables](advanced-settings.md#modifying-job-schedules). La modificación de las programaciones predeterminadas de Desduplicación de datos es especialmente deseable para asegurarse de que los trabajos de Desduplicación de datos tengan tiempo de finalizar y no compitan por los recursos con la carga de trabajo.
