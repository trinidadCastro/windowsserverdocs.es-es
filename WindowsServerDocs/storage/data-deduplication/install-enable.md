---
ms.assetid: 07d6b251-c492-4d9f-bcc4-031023695b24
title: Instalación y habilitación de Desduplicación de datos
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
description: Cómo instalar Desduplicación de datos en Windows Server, determinar si una carga de trabajo es un buen candidato para la desduplicación y habilitar la desduplicación en volúmenes.
ms.openlocfilehash: 36c9894fd8916643340134698f36af3bd50c34d8
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322427"
---
# <a name="install-and-enable-data-deduplication"></a>Instalar y habilitar Desduplicación de datos
> Se aplica a Windows Server (canal semianual), Windows Server 2016

En este tema se explica cómo instalar la característica [Desduplicación de datos](overview.md), cómo evaluar las cargas de trabajo para la desduplicación y cómo habilitar Desduplicación de datos en volúmenes específicos.

> [!Note]  
> Si tienes previsto ejecutar Desduplicación de datos en un clúster de conmutación por error, todos los nodos del clúster deben tener instalado el rol de servidor Desduplicación de datos.

## <a id="install-dedup"></a>Instalación de desduplicación de datos
> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334) contiene un conjunto de correcciones de errores para Desduplicación de datos en el que se incluyen correcciones importantes de confiabilidad; te recomendamos encarecidamente que lo instales al usar Desduplicación de datos con Windows Server 2016.

### <a id="install-dedup-via-server-manager"></a>Instalación de desduplicación de datos mediante Administrador del servidor
1. En el Asistente para agregar roles y características, seleccione **Roles de servidor** y, después, seleccione **Desduplicación de datos**.  
![instalar desduplicación de datos a través de Administrador del servidor: seleccione desduplicación de datos en roles de servidor](media/install-dedup-via-server-manager-1.png)
2. Haga clic en **Siguiente** hasta que se active el botón **Instalar** y, a continuación, haga clic en **Instalar**.  
![instalar desduplicación de datos a través de Administrador del servidor: haga clic en instalar](media/install-dedup-via-server-manager-2.png)

### <a id="install-dedup-via-powershell"></a>Instalación de desduplicación de datos mediante PowerShell
Para instalar Desduplicación de datos, ejecute el siguiente comando de PowerShell como administrador:  
`Install-WindowsFeature -Name FS-Data-Deduplication`

Para instalar Desduplicación de datos en una instalación de Nano Server:

1. Cree una instalación de Nano Server con el almacenamiento instalado tal como se describe en [Getting Started with Nano Server](../../get-started/getting-started-with-nano-server.md) (Introducción a Nano Server).
2. Desde un servidor que ejecuta Windows Server 2016 en un modo distinto al de Nano Server, o desde un equipo con Windows con [Herramientas de administración remota del servidor](https://www.microsoft.com/download/details.aspx?id=45520) (RSAT) instaladas, instale Desduplicación de datos con una referencia explícita a la instancia de Nano Server (reemplace "MyNanoServer" por el nombre real de la instancia de Nano Server):  
    ```PowerShell
    Install-WindowsFeature -ComputerName <MyNanoServer> -Name FS-Data-Deduplication
    ```  
    <br />
    <strong>--O bien--</strong>
    <br />
    Conecte de manera remota con la instancia de Nano Server con comunicación remota de PowerShell e instale Desduplicación de datos mediante DISM:  
    
    ```PowerShell
    Enter-PSSession -ComputerName MyNanoServer 
    dism /online /enable-feature /featurename:dedup-core /all
    ```

## <a id="enable-dedup"></a>Habilitar desduplicación de datos
### <a id="enable-dedup-candidate-workloads"></a>Determinar qué cargas de trabajo son candidatas para desduplicación de datos
Desduplicación de datos puede reducir de manera eficaz los costos de consumo de datos de la aplicación de servidor, ya que reduce la cantidad de espacio en disco consumido por los datos redundantes. Antes de habilitar la desduplicación, es importante que comprenda las características de su carga de trabajo para asegurarse de que obtiene el máximo rendimiento de su almacenamiento. Hay dos clases de cargas de trabajo a tener en cuenta:

* *Cargas de trabajo recomendadas* que han demostrado tener tanto conjuntos de datos que se benefician de la desduplicación como patrones de consumo de recursos que son compatibles con el modelo de procesamiento posterior de Desduplicación de datos. Le recomendamos que siempre [habilite Desduplicación de datos](install-enable.md#enable-dedup-lights-on) en estas cargas de trabajo:
    * Servidores de archivos de uso general (GPFS) que sirven a recursos compartidos, como recursos compartidos de equipo, carpetas particulares de usuario, carpetas de trabajo y recursos compartidos de desarrollo de software.
    * Servidores de infraestructura de escritorio virtualizado (VDI).
    * Aplicaciones de copia de seguridad virtualizadas, como [Microsoft Data Protection Manager (DPM)](https://technet.microsoft.com/library/hh758173.aspx).
* Cargas de trabajo que podrían beneficiarse de desduplicación, pero no siempre son buenas candidatas para ella. Por ejemplo, las cargas de trabajo siguientes podrían funcionar bien con la desduplicación, pero debe evaluar primero las ventajas de la desduplicación:
    * Hosts de Hyper-V de uso general
    * Servidores SQL Server
    * Servidores de línea de negocio (LOB)

### <a id="enable-dedup-evaluating-sometimes-workloads"></a>Evaluar las cargas de trabajo para la desduplicación de datos
> [!Important]  
> Si está ejecutando una carga de trabajo recomendada, puede omitir esta sección e ir a la sección [Habilitación de Desduplicación de datos](install-enable.md#enable-dedup-lights-on) para su carga de trabajo.

Para determinar si una carga de trabajo funciona bien con la desduplicación, responda a las preguntas siguientes. Si no está seguro acerca de una carga de trabajo, considere realizar una implementación piloto de Desduplicación de datos en un conjunto de datos de prueba para la carga de trabajo para ver cómo se desempeña.

1. **¿El conjunto de los conjuntos de sus cargas de trabajo tiene suficiente duplicación para beneficiarse de la habilitación de la desduplicación?**  
    Antes de habilitar Desduplicación de datos para una carga de trabajo, investigue cuánta duplicación de conjunto de datos de la carga de trabajo tiene mediante la herramienta de evaluación del ahorro de desduplicación de datos o DDPEval. Después de instalar Desduplicación de datos, puede encontrar esta herramienta en `C:\Windows\System32\DDPEval.exe`. DDPEval puede evaluar el potencial de optimización en los volúmenes conectados directamente (incluidas unidades locales o volúmenes compartidos de clúster) y recursos compartidos de red asignados o sin asignar.  
    &nbsp;   
    La ejecución de DDPEval.exe devolverá un resultado similar al siguiente:  
    &nbsp;  
    `Data Deduplication Savings Evaluation Tool`  
    `Copyright 2011-2012 Microsoft Corporation.  All Rights Reserved.`    
    &nbsp;   
    `Evaluated folder: E:\Test`     
    `Processed files: 34`  
    `Processed files size: 12.03MB`  
    `Optimized files size: 4.02MB`  
    `Space savings: 8.01MB`  
    `Space savings percent: 66`  
    `Optimized files size (no compression): 11.47MB`  
    `Space savings (no compression): 571.53KB`  
    `Space savings percent (no compression): 4`  
    `Files with duplication: 2`  
    `Files excluded by policy: 20`  
    `Files excluded by error: 0`  

2. **¿Qué aspecto tienen los patrones de e/s de la carga de trabajo en su conjunto de elementos? ¿Qué rendimiento tengo para mi carga de trabajo?**  
     Desduplicación de datos realiza la optimización de los archivos como un trabajo periódico, en lugar de cuando el archivo se escribe un el disco. Por lo tanto, es importante examinar los patrones de lectura previstos de la carga de trabajo para el volumen desduplicado. Como Desduplicación de datos mueve el contenido del archivo al almacenamiento de fragmentos e intenta organizar el almacenamiento de fragmentos por archivo siempre que sea posible, las operaciones de lectura se desempeñarán mejor cuando se aplican a intervalos secuenciales de un archivo.  

    Las cargas de trabajo de tipo base de datos suelen tener patrones de lectura más aleatorios que los patrones de lectura secuenciales, porque las bases de datos no suelen garantizar que el diseño de la base de datos sea óptimo para todas las posibles consultas que se pueden ejecutar. Como las secciones del almacén de fragmentos pueden existir en todo el volumen, el acceso a los intervalos de datos en el almacén de fragmentos para las consultas de base de datos puede introducir latencia adicional. Las cargas de trabajo de alto rendimiento son especialmente sensibles a esta latencia adicional, pero otras cargas de trabajo de tipo base de datos podrían no serlo.

    > [!Note]  
    > Estos problemas se aplican principalmente a las cargas de trabajo de almacenamiento en volúmenes formados por medios de almacenamiento rotacionales tradicionales (también conocidos como unidades de disco duro o HDD). La infraestructura de almacenamiento de memoria flash (también conocido como unidades de disco de estado sólido o SSD) se ve menos afectada por los patrones aleatorios de E/S porque una de las propiedades de los medios de memoria flash es el mismo tiempo de acceso a todas las ubicaciones de los medios. Por lo tanto, la desduplicación no presentará la misma cantidad de latencia para las lecturas en conjuntos de datos de la carga de trabajo almacenados en los medios de memoria flash, como ocurre en los medios de almacenamiento rotacionales tradicionales.

3. **¿Cuáles son los requisitos de recursos de mi carga de trabajo en el servidor?**  
    Dado que Desduplicación de datos usa un modelo de procesamiento posterior, Desduplicación de datos necesita periódicamente disponer de suficientes recursos del sistema para completar su [optimización y otros trabajos](understand.md#job-info). Esto significa que las cargas de trabajo que tienen un determinado tiempo de inactividad, como por la noche o los fines de semana, son idóneas para la desduplicación, mientras que las cargas de trabajo que se ejecutan ininterrumpidamente podrían no serlo. Aun así, las cargas de trabajo sin tiempo de inactividad podrían ser buenas candidatas para la desduplicación si la carga de trabajo no tiene requisitos elevados de recursos en el servidor.

### <a id="enable-dedup-lights-on"></a>Habilitar desduplicación de datos
Antes de habilitar Desduplicación de datos, debe elegir el [tipo de uso](understand.md#usage-type) que más se parezca a la carga de trabajo. Hay tres tipos de uso incluidos con Desduplicación de datos.

* [Predeterminado](understand.md#usage-type-default): optimizado específicamente para servidores de archivos de uso general.
* [Hyper-V](understand.md#usage-type-hyperv): optimizado específicamente para servidores VDI.
* [Copia de seguridad](understand.md#usage-type-backup): optimizado específicamente para aplicaciones de copia de seguridad virtualizadas, como [Microsoft DPM](https://technet.microsoft.com/library/hh758173.aspx).

#### <a id="enable-dedup-via-server-manager"></a>Habilitar la desduplicación de datos mediante Administrador del servidor
1. Seleccione **Servicios de archivos y almacenamiento** en el Administrador del servidor.  
![haga clic en servicios de archivos y almacenamiento](media/enable-dedup-via-server-manager-1.PNG)
2. Seleccione **Volúmenes** en **Servicios de archivos y almacenamiento**.  
![haga clic en volúmenes](media/enable-dedup-via-server-manager-2.png)
3. Haga clic con el botón derecho en el volumen deseado y seleccione **Configurar desduplicación de datos**.  
![haga clic en configurar desduplicación de datos](media/enable-dedup-via-server-manager-3.png)
4. Seleccione el **Tipo de uso** deseado en el cuadro de lista desplegable y seleccione **Aceptar**.  
![seleccionar el tipo de uso deseado en la lista desplegable](media/enable-dedup-via-server-manager-4.png)
5. Si está ejecutando una carga de trabajo recomendada, ha terminado. Para otras cargas de trabajo, consulte [Otras consideraciones](#enable-dedup-sometimes-considerations).

> [!Note]  
> Obtenga más información sobre la exclusión de extensiones de archivo o carpetas y la selección de la programación de desduplicación, incluido por qué podría interesarle hacerlo, en la página sobre la [configuración de desduplicación de datos](advanced-settings.md).

#### <a id="enable-dedup-via-powershell"></a>Habilitación de la desduplicación de datos mediante PowerShell
1. Con un contexto de administrador, ejecute el siguiente comando de PowerShell:  
    ```PowerShell
    Enable-DedupVolume -Volume <Volume-Path> -UsageType <Selected-Usage-Type>
    ```

2. Si está ejecutando una carga de trabajo recomendada, ha terminado. Para otras cargas de trabajo, consulte [Otras consideraciones](#enable-dedup-sometimes-considerations).

> [!Note]  
> Los cmdlets de PowerShell de Desduplicación de datos, incluido [`Enable-DedupVolume`](https://technet.microsoft.com/library/hh848441.aspx), pueden ejecutarse de forma remota si se anexa el parámetro `-CimSession` con una sesión CIM. Esto es especialmente útil para ejecutar los cmdlets de PowerShell de Desduplicación de datos de forma remota en una instancia de Nano Server. Para crear una nueva sesión de CIM, ejecute [`New-CimSession`](https://technet.microsoft.com/library/jj590760.aspx).

#### <a id="enable-dedup-sometimes-considerations"></a>Otras consideraciones
> [!Important]  
> Si está ejecutando una carga de trabajo recomendada, puede omitir esta sección.

* Los tipos de uso de Desduplicación de datos ofrecen valores predeterminados razonables para cargas de trabajo recomendadas, pero también proporcionan un buen punto de partida para todas las cargas de trabajo. Para otras cargas de trabajo que no sean las recomendadas, es posible modificar [Configuración avanzada de Desduplicación de datos](advanced-settings.md) para mejorar el rendimiento de desduplicación.
* Si la carga de trabajo tiene requisitos elevados de recursos en el servidor, los trabajos de Desduplicación de datos [deberían programarse para ejecutarse durante los tiempos de inactividad para esa carga de trabajo](advanced-settings.md#modifying-job-schedules-change-schedule). Esto es especialmente importante cuando se ejecuta la desduplicación en un host hiperconvergido, ya que la ejecución de Desduplicación de datos durante las horas de trabajo previstas puede agotar las máquinas virtuales.
* Si la carga de trabajo no tiene requisitos elevados de recursos, o si es más importante que se completen los trabajos de optimización que atender a las solicitudes de carga de trabajo, [se pueden ajustar la memoria, la CPU y la prioridad de los trabajos de Desduplicación de datos](advanced-settings.md#modifying-job-schedules).

## <a id="faq"></a>Preguntas más frecuentes (p + f)
**Deseo ejecutar la desduplicación de datos en el conjunto de datos para la carga de trabajo X. ¿Es compatible?**  
Aparte de las cargas de trabajo [que se sabe que no interoperan con Desduplicación de datos](interop.md), admitimos completamente la integridad de datos de Desduplicación de datos con cualquier carga de trabajo. Las cargas de trabajo recomendadas son compatibles también con Microsoft por rendimiento. El rendimiento de otras cargas de trabajo depende en gran medida de lo que hacen en el servidor. Debe determinar qué efectos sobre el rendimiento tiene Desduplicación de datos en la carga de trabajo, y si son aceptables para esta carga de trabajo.

**¿Cuáles son los requisitos de tamaño del volumen para los volúmenes desduplicados?**  
En Windows Server 2012 y Windows Server 2012 R2, los volúmenes tenían que ajustarse cuidadosamente para asegurarse de que Desduplicación de datos pudiera hacer frente a la renovación en el volumen. Normalmente, esto significaba que el tamaño máximo medio de un volumen desduplicado para una elevada carga de trabajo de renovación era de 1 a 2 TB, y el tamaño máximo absoluto recomendado era de 10 TB. En Windows Server 2016, estas limitaciones se han quitado. Para obtener más información, consulte [What's new in Data Deduplication](whats-new.md#large-volume-support) (Novedades de Desduplicación de datos).

**¿Es necesario modificar la programación u otros valores de desduplicación de datos para las cargas de trabajo recomendadas?**  
No, los [tipos de uso](understand.md#usage-type) proporcionados se crearon para ofrecer valores predeterminados razonables para cargas de trabajo recomendadas.

**¿Cuáles son los requisitos de memoria para la desduplicación de datos?**  
Como mínimo, Desduplicación de datos debe tener 300 MB + 50 MB por cada TB de datos lógicos. Por ejemplo, si está optimizando un volumen de 10 TB, necesitará un mínimo de 800 MB de memoria asignada para la desduplicación (`300 MB + 50 MB * 10 = 300 MB + 500 MB = 800 MB`). Mientras que Desduplicación de datos puede optimizar un volumen con esta baja cantidad de memoria, tener tales recursos restringidos ralentizará trabajos de Desduplicación de datos.

Lo ideal es que Desduplicación de datos tenga 1 GB de memoria por cada 1 TB de datos lógicos. Por ejemplo, si se está optimizando un volumen de 10 TB, lo ideal es necesitar 10 GB de memoria asignada para Desduplicación de datos (`1 GB * 10`). Esta relación asegurará el máximo rendimiento para los trabajos de Desduplicación de datos.

**¿Cuáles son los requisitos de almacenamiento para la desduplicación de datos?**  
En Windows Server 2016, Desduplicación de datos puede admitir tamaños de volúmenes de hasta 64 TB. Para más información, consulte [Novedades de desduplicación de datos](whats-new.md#large-volume-support).
