---
title: Guía de usuario de Server Performance Advisor
description: Guía de usuario de Server Performance Advisor
ms.assetid: be99a5a9-b9c0-436b-912c-e5c5839a533d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: 97fab1a36db2e903b3a909bee9e2f1748f7841fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834146"
---
# <a name="server-performance-advisor-users-guide"></a>Guía de usuario de Server Performance Advisor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8

Esta guía de usuario de Microsoft Server Performance Advisor (SPA) proporciona instrucciones sobre cómo usar la SPA para identificar los cuellos de botella de rendimiento en los sistemas implementados en varios roles de servidor.

SPA puede ayudarle con lo siguiente:

* Administrar el rendimiento del servidor y solucionar problemas de rendimiento del servidor.

* Proporcionar recomendaciones sobre la configuración común y problemas de rendimiento e informes de datos.

* Proporcionar mejores recomendaciones de práctica en función de los datos recopilados.

> [!NOTE]
> La consola de SPA no realiza cambios en los servidores.

Para obtener más información sobre el desarrollo de módulos de SPA de Advisor, consulte [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

## <a name="server-performance-advisor-overview"></a>Información general de Server Performance Advisor


En esta sección se explica el historial de SPA, describe su audiencia de destino y los escenarios y presenta una introducción a la arquitectura de la herramienta.

### <a name="history-of-spa"></a>Historial de SPA

La versión original de Microsoft Server Performance Advisor (SPA) se publicó en 2005, 2006. Fundamentalmente Windows Server 2003, incluido el núcleo del sistema operativo y los roles de servidor, como Internet Information Services (IIS) y active directory. El objetivo de la herramienta es que le ayudarán a evaluar el rendimiento del servidor y solucionar problemas de rendimiento del servidor.

SPA proporciona recomendaciones e informes de datos a los administradores del sistema acerca de problemas de rendimiento y la configuración común. SPA recopila datos de rendimiento procedentes de diversos orígenes en los servidores, por ejemplo, los contadores de rendimiento, las claves del registro, WMI las consultas, los archivos de configuración y Event Tracing for Windows (ETW). En función de los datos de rendimiento de servidor que recopila, SPA puede proporcionar información detallada sobre la situación de rendimiento de servidor actual y problema recomendaciones sobre lo que se puede mejorar.

Ha habido más de un millón de descargas de la herramienta SPA original y ayudó a muchos administradores de sistemas administrar el rendimiento de sus servidores. Ha sido una herramienta de solución de problemas de rendimiento mucho éxito.

Desde el lanzamiento de la SPA original, hay varios cambios importantes en el mundo de rendimiento del servidor. En concreto, la versión de Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008, con las nuevas versiones de los roles de servidor correspondiente, como Internet Information Services (IIS) 8.5. Las versiones más recientes de Server Performance Advisor extienden la capacidad de la SPA original para los sistemas operativos de servidor recientes y los roles de servidor.

Aunque SPA 3.1 comparte los mismos objetivos de diseño y un paradigma de análisis de rendimiento como la herramienta original, se ha reescrito con la tecnología más reciente. También tiene las siguientes mejoras clave:

* Puede realizar análisis en los servidores que ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008.

* Admite la funcionalidad de análisis remoto, que permite la SPA 3.1 recopilar y analizar datos desde una consola central, sin necesidad de instalar el código en los servidores que se va a analizar.

* Utiliza Microsoft SQL Server para el análisis de datos y almacenamiento, lo que permite procesar y almacenar grandes cantidades de datos.

* Separa la herramienta de los módulos de Advisor. Lógica de análisis de rendimiento para cada rol de servidor se libera en forma de un módulo del Asesor de actualizaciones, que puede publicado o actualizado por separado de la herramienta.

* Proporciona los módulos del Asistente para la que se desarrollan en secuencias de comandos SQL con una arquitectura abierta. Las partes que no son de Microsoft pueden desarrollar módulos de advisor o ampliar los módulos del Asesor de actualizaciones existente de Microsoft para cubrir necesidades especiales.

* Admite las nuevas características como los informes de comparación en paralelo y tendencia y gráficos históricos para ayudarle a encontrar anomalías.

* Proporciona características como modifican, importación y exportación umbrales para ayudarle a bien optimización los informes y notificaciones y compartan esos ajustes con otros usuarios de la SPA.

* Admite varios proyectos, que pueden utilizarse para agrupar los servidores de destino.

* Proporciona periódicas recopilación de datos de la consola de SPA.

* Permite la generación de informes y consultas personalizadas mediante el uso de Microsoft SQL Server (para usuarios avanzados).

* Cmdlets de Windows PowerShell personalizados están disponibles para su uso con la función SPA

* Compatible con versiones anteriores para importar y ver informes desde una base de datos de SPA 3.0

### <a name="target-audience"></a>Destinatarios

La herramienta SPA está diseñada principalmente para los administradores del sistema que administran menos de 100 servidores en diversas funciones de servidor. También se puede usar por ingenieros de soporte técnico para recopilar datos de rendimiento y a solucionar problemas de rendimiento para los clientes.

SPA ofrece recomendaciones para ayudarle a solucionar problemas de rendimiento. Sin embargo, se basa en suposiciones que pueden no ser aplicables a su entorno de servidor específico. Las recomendaciones que se ofrecen a través de SPA se deben considerar las sugerencias. Esperamos que los usuarios SPA tengan una buena comprensión de su configuración del sistema, casos de uso y el impacto de ajuste del sistema en el comportamiento general del sistema. Los administradores del sistema deben decidir si se deben aplicar las recomendaciones de SPA en su entorno.

### <a href="" id="what-s-new-in-spa-3-1-"></a>¿Qué novedades de SPA 3.1?

Se agregaron las siguientes características y mejoras de SPA 3.1:

* Active directory Advisor Pack ayuda a analizar el rendimiento general de la función de servidor servidor s active directory Domain Services.

* Soporte técnico a los desarrolladores agregar alerta de notificaciones de eventos ETW pierde en los módulos de advisor y hacer que la alerta sea visible cuando se ha generado un informe y los eventos se han perdido debido a alta frecuencia de registro en una lenta o muy contenidos disco

### <a name="target-scenarios"></a>Escenarios de destino

Los siguientes son los escenarios de destino para SPA:

* **Entorno de servidor**

    SPA está diseñado para configurar y mantener fácilmente. Se aplica a entornos de servidor que usan servidores de 1 a 100. SPA no escala bien, si está intentando administración del rendimiento de más de 100 servidores. Para entornos más grandes o más sofisticados, considere la posibilidad de usar System Center Operations Manager.

* **Solucionar problemas de rendimiento**

    Puede usar SPA como una herramienta de solución de problemas de rendimiento. Proporciona la capacidad para recopilar datos de rendimiento de alto nivel, lleva a cabo una exhaustiva posteriores al procesamiento en los datos para ofrecer una mejor comprensión de su comportamiento general del sistema y que marca las anomalías. Cuando se sospecha que un problema de rendimiento por un cliente, puede usar la SPA para recopilar y analizar los datos de rendimiento del servidor.

    SPA genera un informe que se puede ver. Informes SPA proporcionan una lista de notificación que destaca los posibles problemas y una sección de datos que incluye el índice de rendimiento de diversos, configuraciones y opciones para el servidor. Puede usar este informe para identificar el problema de rendimiento específicas y usar las recomendaciones para encontrar soluciones para el problema. Los informes se pueden comparar con otros informes que se generaron en un momento diferente o un servidor diferente. Mediante esta comparación en paralelo, puede determinar las diferencias entre la línea base normal frente a un comportamiento anómalo.

    **Tenga en cuenta** SPA no está diseñado para ser una depuración o la herramienta de disponibilidad. Además, desde la perspectiva de los servidores, se puede considerar una herramienta de solo lectura y no modifica las configuraciones de servidores.

     

* **Supervisión del rendimiento de índice**

    Puede usar la SPA para supervisar el índice de rendimiento de servidores. Puede elegir ejecutar SPA habitualmente en servidores para recopilar datos de rendimiento y, a continuación, ejecutar un gráfico de tendencias o un gráfico histórico para detectar las anomalías. Puede ver el informe para un análisis determinado, obtener más información sobre el problema de rendimiento y, a continuación, use las recomendaciones u otros datos de informe para resolver el problema.

### <a name="spa-architecture"></a>Arquitectura SPA

La lógica de recopilación de datos SPA se basa en un protocolo en Windows, denominados registros de rendimiento y alertas (PLA). PLA permite a los programas recopilar datos de rendimiento de los servidores locales o remotos, como los contadores de rendimiento, las consultas WMI, seguimientos ETW, claves del registro y archivos de configuración. Cuando SPA ejecuta un análisis de rendimiento de un servidor de destino, crea un conjunto de recopiladores de datos PLA basándose en el módulo de Advisor SPA específico que seleccionó. El módulo del Asesor de actualizaciones contiene el origen de datos que se recopilen y el recurso compartido de archivos donde están los registros que se almacenará. Recopilación de datos SPA almacena solo una cuenta de usuario único; la misma cuenta de usuario utilizada por PLA también se utiliza para escribir los registros.

SPA usa una base de datos de SQL Server para almacenar los registros de rendimiento que se recopilan desde los servidores de destino. SPA importa todos los registros de rendimiento desde el recurso compartido de archivos en la base de datos y, a continuación, usa la lógica de análisis de datos dentro de cada paquete de advisor para procesar los datos y generar los informes. Un paquete de advisor analiza los datos de rendimiento que se recopilan desde los servidores de destino y genera los informes SPA. Para obtener más información sobre la creación de un paquete de advisor, consulte [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Las interfaces de usuario de consola SPA e interacciones se crean como parte de la SPAConsole.exe. Puede usar la consola para crear una base de datos, agregar o quitar paquetes de advisor, administrar servidores de destino, ejecutar análisis de rendimiento y ver informes de rendimiento.

Puede ejecutar la consola SPA en los siguientes sistemas operativos:

* Windows 8.1

* Windows 8

* Windows 7

* Windows Server 2012 R2

* Windows Server 2012

* Windows Server 2008 R2

* Windows Server 2008

En una aplicación empresarial típica, hay tres niveles: la capa de presentación, la capa de lógica empresarial y la capa de almacenamiento. SPA se ha diseñado como un dos productos de nivel de la consola y la base de datos. La consola actúa como una capa de presentación con cierta lógica relacionada con los procesos que incluyen y las sirve de base de datos como la capa de almacenamiento y la capa de lógica empresarial. La consola de captura la entrada de usuario, y controla los pasos para la recopilación de datos, procesamiento de datos y generación de informes. SPA no dependen de servicios del sistema de Windows.

El diagrama siguiente muestra la arquitectura de alto nivel del sistema SPA. El proceso es:

1.  Desde la consola SPA, ejecutar análisis de rendimiento en los servidores especificados.

2.  Cuando se recopilan los datos de rendimiento, PLA en los servidores de destino escribe los registros en el recurso compartido de archivo especificado por el conjunto de recopiladores de datos.

3.  Una vez completada la recopilación de datos en un equipo de destino, la consola SPA importa los registros de la base de datos de SQL Server.

4.  La consola invoca la lógica de procesamiento de datos del módulo específico del Asesor de actualizaciones.

5.  El módulo de advisor procesa los datos y llama a las API de SPA para insertar registros en las tablas de informe del sistema definidas por el marco SPA.

6.  Puede usar el Visor de informes dentro de la consola para ver los informes que generan. Para ayudarle a solucionar problemas de rendimiento, SPA proporciona tres tipos de informes: solo los informes, los informes en paralelo y tendencia o gráficos históricos. Para obtener más información sobre los informes, vea [ver informes](#bkmk-viewingreports).

![arquitectura de SPA](../media/server-performance-advisor/spa-user-manual-architecture.png)

## <a name="getting-started-with-spa"></a>Introducción a SPA


### <a name="requirements"></a>Requisitos

La consola SPA se puede instalar en Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008. No se admite la ejecución de SPA en versiones anteriores del sistema operativo Windows Server. Se ejecuta SPA en x86 o x64, pero no admite las arquitecturas IA64 o ARM.

SPA se basa en Windows Presentation Foundation (WPF) 2.0, que forma parte de Microsoft .NET Framework 4, por lo que se requiere .NET Framework 4.

También deberá instalar SQL Server 2008 R2 Express en el mismo equipo donde está instalado la SPA. SQL Server 2008 R2 Express es un motor de base de datos gratuita publicada por Microsoft. Puede descargar Microsoft SQL Server 2008 R2 Express desde Microsoft Download Center. Aunque se recomienda SQL Server 2008 R2 Express, las versiones más recientes de SQL Server también pueden ser compatibles con la SPA.

**Tenga en cuenta** SPA no incluye SQL Server o .NET Framework como parte del paquete de instalación de SPA. Después de instalar Microsoft SQL Server 2008 R2 y .NET Framework 4.0, se recomienda que ejecute Windows Update antes de instalar la SPA.

 

Dado que los usuarios pueden crear y administrar bases de datos con la SPA, la cuenta de usuario que se usa para ejecutar SPA debe tener los mismos privilegios de administrador que SQL Server.

### <a href="" id="bkmk-setupspa"></a>Configuración de SPA

SPA se empaqueta como un archivo .cab que incluye todos los archivos binarios para el marco SPA, los cmdlets de Windows PowerShell que se usan en escenarios avanzados, y empaqueta el Asistente para la siguiente: Sistema operativo de núcleo, Hyper-V, active directory e IIS. Después de extraer el archivo .cab en una carpeta, se requiere ninguna instalación adicional. Sin embargo, para ejecutar la SPA, deberá habilitar la recopilación de datos de los servidores de destino como sigue:

* Para ejecutar la recopilación de datos PLA, la cuenta de usuario que usa para ejecutar la consola SPA debe formar parte del grupo de seguridad de administradores en el servidor de destino. Si el servidor de destino y la consola se encuentran en el mismo dominio, la cuenta de usuario de dominio debe formar parte del grupo de seguridad de administradores en el servidor de destino. Si el servidor de destino y la consola no están en el mismo dominio, cree una cuenta de usuario administrativo en el servidor de destino con el mismo nombre de usuario y la contraseña como la cuenta de usuario que usa para ejecutar la consola SPA.

* Cree una carpeta compartida para los resultados en el servidor.

* Asegúrese de que la cuenta de usuario que utilice para ejecutar la consola SPA ha leído y permisos de escritura en la carpeta compartida. PLA usa esta cuenta para escribir registros en la carpeta.
La consola SPA usa la misma cuenta para leer los registros e impórtelas en la base de datos.

    **Tenga en cuenta** implementa un búfer circular para almacenar el seguimiento de ETW y los mueve a la carpeta compartida que sea posible. Si el servidor está ocupado o la operación de escritura es lenta, ETW quita los seguimientos cuando el búfer esté lleno. Es importante que se encuentra la carpeta compartida en un servidor con acceso de E/S rápido. Se recomienda que cada servidor de destino tiene una carpeta compartida para minimizar la pérdida de datos causada por la E/S de archivos lenta.

     

* PLA tener acceso a los servidores de destino, establecer el Firewall de Windows para permitir el acceso de las alertas y registros de rendimiento remotos en servidores de destino. PLA usa el puerto TCP 139.

* Asegúrese de que el **los registros de rendimiento y alertas** servicio se está ejecutando.

* Si el servidor de consola y de destino se encuentra en subredes diferentes, también deberá establecer el campo de dirección IP remoto en las reglas de firewall de entrada en el **ámbito** configuración en el **los registros de rendimiento y alertas** página, como se muestra aquí.

    ![Pla propiedades](../media/server-performance-advisor/spa-user-manual-pla-firewall.png)

* Activar la detección de red en la consola y en cada uno de los servidores de destino.

* Si el servidor de destino no está unido a un dominio, habilite a la configuración del registro siguiente: **HKLM\\SOFTWARE\\Microsoft\\Windows\\Currentversion\\Policies\\system\\LocalAccountTokenFilterPolicy**.

**Tenga en cuenta** de forma predeterminada, SPA escribe los registros de diagnóstico en la carpeta donde se encuentra SpaConsole.exe. Si SPA se instala en la carpeta archivos de programa, SPA solo es ser capaz de escribir el registro cuando SpaConsole.exe es ejecutar como administrador.

Si desea ejecutar análisis de datos en el equipo de la consola, deberá ejecutar SPA como administrador. PLA pasa por una ruta de acceso de código diferente cuando se ejecuta en un equipo local, lo que requiere privilegios de administrador.

Si desea ejecutar el paquete de Advisor de SPA de IIS en los servidores, deberá habilitar la consulta de WMI y el seguimiento ETW para el servidor IIS. Puede hacerlo al habilitar **seguimiento** bajo el **estado y diagnóstico** servicio de rol y **herramientas ni Scripts de administración de IIS** en **las herramientas de administración**  de la **servidor Web (IIS)** rol de servidor.

 

### <a name="creating-your-first-project"></a>Crear su primer proyecto

Después de que todo está configurado, puede crear su primer proyecto SPA. Como se describe en la sección anterior, SPA almacena todo lo que está relacionado con análisis de datos de rendimiento dentro de una base de datos. Cada proyecto SPA se corresponde con una sola base de datos. El **la primera vez que Use el Asistente para** le guiará por los pasos siguientes.

**Para crear un proyecto SPA**

1.  Iniciar SpaConsole.exe. La consola entra en modo desconectado, donde SPA no está conectado a cualquier base de datos y la ventana principal está en blanco.

2.  Para crear un nuevo proyecto, haga clic en **archivo**y, a continuación, haga clic en **nuevo proyecto**. Esto inicia la primera vez que usar el asistente. La primera página muestra los pasos que seguir al usar al asistente:

    * Crear una base de datos

    * Paquetes de aprovisionamiento del Asesor de actualizaciones

    * Agregar servidores a la lista de servidores de destino

3.  Haz clic en **Siguiente**. El **crear base de datos de proyecto** página le pide que proporcione el nombre de la instancia de Microsoft SQL Server donde desea crear la base de datos. Por ejemplo, si está en el mismo equipo que la consola, puede usar **localhost\\&lt;el nombre del servidor SQL&gt;**.

    **Tenga en cuenta** el nombre de instancia predeterminado para una instalación de SQL Server 2008 R2 Express es SQLExpress. Para una instancia de SQL Server 2008 R2 Express que está instalado en el equipo local, la base de datos normalmente haría predeterminado a **localhost\\SQLExpress**. Sin embargo, es posible que se han modificado durante la instalación de SQL Server, por lo que deberá asegurarse de que use el nombre de instancia de SQL Server correcto.

     

4.  Proporcione el nombre de la base de datos. Solo letras, dígitos y caracteres de subrayado (\_) se permiten como caracteres válidos para un nombre de base de datos. Una sugerencia para el nombre de la base de datos SPA razonable sería **SPA**. Si escribe un nombre no válido, aparece un icono rojo de error. La información sobre herramientas asociado muestra el motivo del error de validación.

    **Tenga en cuenta** es importante recordar el nombre de la base de datos y el nombre de instancia del servidor, porque estos son los identificadores únicos para el proyecto. Deberá proporcionar esta información si desea cambiar a esta base de datos.

     

5.  Después de proporcionar el nombre de instancia de servidor y el nombre de la base de datos, la primera vez que usar el asistente genera la ubicación del archivo de base de datos.

6.  En el **crear base de datos de proyecto** página, haga clic en **siguiente**. La primera vez, usar el asistente crea una base de datos y genera todos los esquemas de base de datos relacionados con la SPA, funciones y procedimientos almacenados en la base de datos. Este paso puede tardar varios segundos según la velocidad del hardware y de red.

    **Tenga en cuenta** si se produce un error en este paso, aparece un mensaje de error. Algunos de los problemas comunes son: Consola no se puede conectar a la instancia de SQL Server, no tiene privilegios suficientes para crear la base de datos, o el nombre de la base de datos ya existe.

     

7.  Cuando el paso anterior se realiza correctamente, verá el **aprovisionar paquete advisor** página. Muestra todos los módulos del Asesor de actualizaciones que están disponibles en el equipo. SPA analiza automáticamente la carpeta denominada **APs** bajo el directorio raíz SPA. Muestra el nombre completo, la versión y el autor de cada módulo del Asesor de actualizaciones.

    **Tenga en cuenta** para obtener más información acerca de cómo se utilizan el nombre completo y la versión de SPA, consulte [Administrar módulos de advisor](#bkmk-manageadvisorpacks)

     

8.  Elija qué paquetes de Asesor de actualizaciones que desea aprovisionar en la base de datos de proyecto y, a continuación, haga clic en **siguiente**. O puede hacer clic **Skip** para ir al paso siguiente sin necesidad de aprovisionar los módulos del Asesor de actualizaciones.

    **Tenga en cuenta** puede aprovisionar módulos advisor siempre que se está usando la herramienta. Para obtener más información, consulte [Administrar módulos advisor](#bkmk-manageadvisorpacks).

     

9.  En el **agregar servidores** página, para cada servidor para agregarse a la lista de servidores de destino, hay dos campos obligatorios para rellenar: **Nombre del servidor** y **ubicación del recurso compartido de archivo**.

    **Tenga en cuenta** también hay un **comentario** campo, que se utiliza principalmente para clasificar o encontrar el servidor. En casos donde haya varios servidores, puede importar un archivo de valores (.csv) separados por comas que contiene el nombre del servidor, la carpeta de resultados y el campo de comentario opcional. El **comentario** campo se utiliza para describir el servidor y el término que se puede usar para filtrar los servidores de recopilación de datos. Si se están inicializando los servidores a través del archivo .csv, un error de análisis dentro del archivo no carga los servidores.

     

10. Varias configuraciones deben establecerse para habilitar la recopilación de datos PLA, como se describe en [configurar SPA](#bkmk-setupspa). El **Agregar servidor** página proporciona una funcionalidad de configuración de prueba para ayudarle a solucionar problemas de configuración. Seleccione la casilla de verificación asociada con el equipo y, a continuación, haga clic en **probar la conectividad**. SPA intenta generar un recopilador de datos establecido en servidores de destino y lo intenta de nuevo los resultados a la base de datos de importación. Si todo es correcto, el **estado** muestra **pasar**. Si se produce un error, aparece una información sobre herramientas que describe el motivo del error.

11. Cada uno de los servidores se agrega automáticamente a la base de datos incluso si no pasa la prueba de configuración. Para quitar los servidores de la lista, seleccione el nombre del servidor y haga clic en **quitar**.

12. Cuando todo lo que se complete, haga clic en **finalizar** para cerrar la primera vez que usar el asistente. Si cierra la primera vez usar el asistente antes de finalizarlo, conservan todos los pasos anteriores y ninguno de ellos revertir automáticamente. Deberá realizar manualmente los cambios futuros.

## <a name="running-analysis"></a>Ejecutando análisis


Después de configurar la base de datos, puede ejecutar análisis de rendimiento en los servidores.

Cada vez que inicia la consola SPA, se abre automáticamente el último proyecto usado por el usuario actual. La ventana principal contiene una lista de servidores. Cada servidor tiene cuatro propiedades: Nombre del servidor, resultado del análisis, estado actual y comentario.

* **Nombre del servidor** el nombre del servidor, que es el identificador de servidor. No hay nombres duplicados se permiten.

* **Resultado del análisis** de forma predeterminada, muestra el resultado del análisis de rendimiento más recientes de ejecución en el servidor. Si no ha habido cualquier análisis de rendimiento que se ejecute en el servidor, se muestra **ningún informe**. Si se produce una advertencia generada por el informe, se muestra **advertencia** y la marca de tiempo cuando se generó el informe más reciente. Si no se encontró ningún problema durante el análisis más reciente en el servidor, se muestra **Aceptar** y la marca de tiempo.

    **Tenga en cuenta** si recientemente ha cambiado una configuración del sistema, le recomendamos que ejecute el análisis de nuevo para evaluar el impacto general de los cambios y obtener un informe actualizado del estado del sistema. SPA no realiza un seguimiento de cambios de configuración para el sistema sometido a prueba.

     

* **Estado actual** muestra el estado de rendimiento de las tareas de análisis están ejecutando actualmente en el servidor. Puede cancelar una tarea en ejecución, haga clic en el **cancelar** icono, que se designa mediante una X roja.

* **comentario** describe el servidor de destino actual. Por ejemplo, puede describir el servidor mediante el rol de servidor (por ejemplo, SQL Server) o una ubicación (por ejemplo, Kent). SPA usa el **nombre del servidor** y **comentario** para ayudar a buscar y encontrar el servidor adecuado. Puede escribir en el cuadro de texto de búsqueda. Si el **nombre del servidor** o **comentario** columnas contiene la cadena exacta que escribió en el cuadro de búsqueda, el servidor se muestra en la lista de servidores.

Los controles siguientes también están disponibles en la consola:

* **Repita** una casilla de verificación que describe la posibilidad de que se repita regularmente una colección, en función de un intervalo de tiempo. La mayoría de las instalaciones de servidores, desearía tener una colección de SPA se repita cada hora para que tenga suficiente historial para el análisis. Si desea ejecutar una sola vez la colección, no debe seleccionar la **repita** casilla de verificación.

* **Quitar periodicidad** un botón que permite cancelar una en curso repetir el trabajo de la colección. Cancela la colección de repetición, pero no a la colección actual (si existe) está en curso. Esta opción permite restablecer un nuevo intervalo de recopilación de repetición o ejecutar manualmente la colección.

Antes de comenzar el análisis de rendimiento, seleccione la duración de la recopilación de datos. Aunque recopilar más datos ayuda a proporcionar una imagen más precisa de la situación de rendimiento del servidor, también genera un mayor número de registros y podría tener más impacto potencial en el servidor. Elegir la duración de la colección de datos adecuada según sus necesidades concretas. Cada paquete de advisor define una duración mínima válida. La duración de la recopilación de datos que elija debe ser mayor que la duración mínima de los módulos seleccionados de advisor.

Para ejecutar el análisis de rendimiento en servidores de destino, seleccione los servidores que desea ejecutar el análisis de rendimiento frente a y haga clic en ** ejecutar análisis de ** un cuadro de diálogo aparece y le pide que seleccione los módulos de advisor para ejecutarse en los servidores. Los módulos seleccionados advisor ejecutan simultáneamente. Los informes que se generan se basan en los datos de rendimiento recopilados durante el mismo período de tiempo.

**Tenga en cuenta** si selecciona un servidor que tenga un análisis de rendimiento periódico en ejecución, el **Quitar periodicidad** botón le permite cancelar la recopilación periódica de datos. SPA no permite varias sesiones de recopilación de datos al mismo tiempo en el mismo equipo.

 

## <a href="" id="bkmk-viewingreports"></a>Visualización de informes


En la SPA, hay tres tipos de informe de análisis de rendimiento: Único informe, informes en paralelo y tendencia y gráficos históricos.

Después del análisis de rendimiento de ejecución, se genera un informe para cada uno de los módulos de advisor ejecutar en el equipo de destino. En la lista de servidor en la ventana principal, puede expandir **resultado del análisis de** para ver todos los módulos del Asesor de actualizaciones que se han ejecutado en el servidor especificado. Puede hacer clic en un nombre de informe para ver un informe único.

Hay tres iconos junto al nombre del módulo del Asesor de actualizaciones que muestran el estado de ejecución en el servidor de análisis más reciente:

* El **más reciente** icono muestra el informe generado por el análisis de rendimiento más reciente en este servidor para el módulo del Asesor de actualizaciones.

* El **buscar** icono muestra la lista del rendimiento de informes de análisis, lo que permite elegir el informe correcto. El **paquete Advisor** y **servidor de destino** campos se rellenan previamente con la actual advisor pack y destino información del servidor. El intervalo de tiempo predeterminado se establece en una semana, y se establece la fecha de finalización en hoy en día. Si hace clic en el **búsqueda** botón en la esquina superior derecha, puede obtener una lista de todos los informes de análisis de rendimiento para el módulo de servidor y advisor seleccionado dentro del intervalo de tiempo.

* El **ver gráficos** icono abre la tendencia y la vista de gráfico histórico.

La siguiente ilustración muestra el **más reciente**, **buscar**, y **ver gráficos** iconos después de cada paquete del Asesor de actualizaciones:

![iconos de informe](../media/server-performance-advisor/spa-user-manual-report-icons.png)

### <a name="searching-for-and-within-reports"></a>Buscar y dentro de los informes

Búsqueda de informes se realiza mediante **el Explorador de informes**. Esto le permite buscar por intervalo de fechas, nombre del servidor y paquete del Asistente para informes. Se trata de la manera recomendada para buscar un informe de interés que no sea el último informe de ejecución. El último informe de ejecución está disponible a través de **Ver informe** para ese servidor.

Al ver un informe específico, puede fácilmente navegue al informe anterior y por hora o examinar un informe relacionado, como un punto de acceso diferentes ejecutando al mismo tiempo. Estas opciones están disponibles en **acciones**.

También es posible buscar dentro de un informe. Un número de los informes tienen un **buscar** cuadro de búsqueda de cadena disponible para la búsqueda de cadena de texto rápido dentro del informe. Para quitar el cuadro de texto, puede descartarla. Para activar un cuadro de búsqueda (en windows que tienen el texto de búsqueda), puede usar el Control + F contextual. El **buscar** cuadro permite al usuario especificar una búsqueda de mayúsculas y minúsculas según corresponda con el **Coincidir mayúsculas y minúsculas** opción.

La siguiente ilustración muestra el **buscar** cuadro de búsqueda con la cadena **Power** en el **informe** ficha.

![el cuadro de búsqueda de búsqueda](../media/server-performance-advisor/spa-user-manual-find-search-box.png)

### <a name="single-report"></a>Informe único

Un informe solo muestra los resultados del análisis de rendimiento de una sola ejecución de un paquete de advisor en un único equipo. El informe muestra el nombre del módulo de advisor, el nombre de la duración de recopilación de datos, el tiempo que se genera el informe y el servidor de destino.

Un único informe contiene una sección de notificación y las secciones de datos.

### <a name="notification-section"></a>Sección de notificaciones

La sección de notificaciones consta de un conjunto de reglas de análisis de rendimiento. Cada notificación contiene algún origen de datos, algunos valores de umbral y alguna lógica de negocios. Al ejecutar análisis de rendimiento, la lógica de negocios evalúa los orígenes de datos con los umbrales para determinar si la regla supera o no. Si no es así, aparece una advertencia para informarle sobre un posible problema de rendimiento. También proporciona recomendaciones para ayudarle a solucionar el problema. La sección notificación siempre es la primera pestaña en la vista de informe único.

La sección de notificaciones se divide en dos partes: **Advertencia** y **otras notificaciones**.

Si el origen de datos para una regla cumple determinadas condiciones según la configuración de la lógica y el umbral, aparece una advertencia en el **advertencia** área. Una advertencia incluye las siguientes partes:

* Un icono de advertencia indica la existencia de un posible problema.

* El nombre de la regla. Por ejemplo, **red recibir paquetes caídas** es un vínculo que señala a la página de detalles de la regla, como se describe en [Administrar módulos advisor](#bkmk-manageadvisorpacks).

* Una descripción sencilla sobre el problema potencial.

* Una recomendación para una posible solución para el problema de rendimiento posibles.

Pueden tener distintos servidores drásticamente diferentes configuraciones y los patrones de uso, y no es posible establecer los umbrales y las reglas que son aplicables a todos los servidores en todas las condiciones. SPA proporciona la capacidad de modificar los umbrales. También puede deshabilitar una regla si la regla no es aplicable a su escenario. De forma predeterminada, todas las reglas están habilitadas. Una regla deshabilitada no aparece en el área de notificación. Para obtener más información, consulte [Administrar módulos advisor](#bkmk-manageadvisorpacks).

El **otras notificaciones** área contiene todas las otras reglas, donde no se genera ninguna advertencia o la regla no es aplicable. Contiene elementos similares que se encuentra en la **advertencia** área. La principal diferencia es que si no se genera ninguna advertencia o la regla no es aplicable, normalmente no se proporciona una recomendación.

### <a name="data-sections"></a>Secciones de datos

Secciones de datos contienen los datos de rendimiento que genera el módulo de advisor en función de los datos sin procesar recopilados de los servidores de destino. Secciones de datos incluyen un conjunto de secciones de nivel superior y varios niveles de subsecciones. Las secciones de nivel superior se presentan como pestañas. Todas las subsecciones en las secciones de nivel superior se presentan en áreas expansibles. Puede contraer o expandir cada una de las secciones para ayudar a centrarse en el área de interés, tal como se muestra en la ilustración siguiente.

![secciones de datos](../media/server-performance-advisor/spa-user-manual-data-sections.png)

El módulo de Asesor principal SPA de sistema operativo y el módulo de IIS SPA Advisor contienen un **información general del sistema** sección. Esta sección incluye la información sobre el uso de recursos y la configuración de nivel superior. Otras secciones de nivel superior representan áreas de datos de rendimiento. SPA presenta datos de informe de las maneras siguientes:

* **Valor único** un par clave/valor. La clave es una cadena que representa el significado del valor. El valor puede ser una cadena, un valor numérico o un valor booleano. A menudo, esto se usa para mostrar información estática, como la arquitectura de configuración, por ejemplo, la CPU, el tamaño total de memoria y la versión del BIOS que no cambian con el tiempo.

* **valor de lista** esto a veces es un par clave/valor, pero el valor de la lista puede contener varios campos. Por ejemplo, el atributo de la CPU se puede mostrar en una tabla con varias columnas y varias filas. Cada fila representa una CPU, y cada columna representa un atributo de la CPU.

* **Estadísticas** puede considerarse un tipo especial de valor único. Solo puede contener datos numéricos. Durante el tiempo de recopilación de datos, muchos de los puntos de datos numéricos fluctúan en lugar de permanecer constante. Por ejemplo, el uso de CPU cambia cada vez que el PLA recopila el contador de rendimiento. Con un único valor, no puede reflejar con precisión la situación de rendimiento. En lugar de mostrar un solo valor, valor promedio, máximo, mínimo y el 90% se usan para estos puntos de datos numéricos dinámico. El valor del 90% representa la actividad en o por encima del percentil 90 de todos los eventos para ese contador en ese intervalo de la colección especificada.

* **La lista superior** contiene normalmente los principales consumidores de un recurso específico o las primeras entidades que experimentaron determinados eventos. Por ejemplo, **Top 10 procesa en términos de uso promedio de CPU** incluye los procesos de diez principales con mayor uso de CPU promedio durante el tiempo de recopilación de datos. Dado que el uso de CPU es también un punto de datos numéricos dinámicos, otras estadísticas, como máximo, mínimo y valor del 90% también se incluyen en la lista para proporcionar al usuario una imagen más completa del consumo de CPU.

Como se mencionó en las secciones anteriores, SPA se basa en PLA para recopilar el seguimiento ETW, consultas WMI, los contadores de rendimiento, las claves del registro y archivos de configuración para generar el informe. Es importante comprender el origen de datos detrás de cada punto de datos en el informe. SPA proporciona dicha información a través de la información sobre herramientas. Puede mantener el puntero sobre las columnas de clave o filas para ver la información sobre herramientas del origen de datos. Por ejemplo, **WMI:Win32\_DisDrive: título** significa que es el origen de datos de una consulta de WMI, el nombre de clase WMI es Win32\_unidad de disco y la propiedad es **título**.

### <a href="" id="side-by-side-report-"></a>Informe de Side-by-side

Los informes solo proporcionan notificaciones y una sección de datos para ayudar a los problemas de rendimiento de usuario buscar posibles, pero a menudo es difícil de identificar un posible problema de rendimiento mirando directamente en un único informe. Un único informe puede contener demasiados puntos de datos, que hace que resulte difícil encontrar los posibles problemas.

Para solucionar este problema, SPA proporciona la capacidad de comparar dos informes. Puede comparar un informe con un problema potencial para un informe de instantánea para ayudar a encontrar las diferencias.

Side-by-side informes pueden iniciarse desde un visor de informes de único. Los usuarios puedan seleccionar **acciones**y, a continuación, haga clic en **comparar informes** para seleccionar los informes. Solo es significativa para comparar informes desde el mismo módulo de advisor. Puede comparar el informe con un informe anterior en el tiempo, el siguiente informe en el tiempo o un informe arbitrario que se ha seleccionado a través de las capacidades de búsqueda. Por ejemplo, para aislar un comportamiento anormal, puede comparar un informe de servidor de base de referencia a un informe que se generó en un momento diferente en el mismo equipo o a un informe que se generó en un equipo diferente que tenga un rol de servidor similar y cargar.

Un informe en paralelo se parece mucho al igual que el informe. Contiene una sección de notificación y secciones de datos. Contiene el mismo número de secciones de datos y las notificaciones que el Visor de informes única. La única diferencia es que los informes se muestran en la forma en paralelo. Cada sección contiene los datos del informe de origen (informe de 1) y el informe de destino (informe de 2). El informe side-by-side muestra el nombre del módulo de advisor, el nombre del servidor de destino (informe de 1 a la izquierda) y el informe 2 a la derecha, el tiempo que se generó el informe y la duración de la recopilación de datos de cada informe.

Si descarta el **buscar** cuadro de diálogo, puede reactivarla escribiendo Control + f el. Este cuadro de diálogo busca y resalta las cadenas de texto dentro de la sección actual.

En la sección de notificación, si cualquiera de los resultados de los dos informes que se comparan es una advertencia, se muestra en el **advertencia** área. En caso contrario, los resultados se muestran en el **otras notificaciones** área. La clave para un informe en paralelo identificar las diferencias entre los informes, no hay información detallada acerca de una regla se muestra. Los usuarios hacer clic en el nombre de regla para que aparezca el formulario de detalle de la regla para obtener más información acerca de la regla.

En las secciones de datos, los datos se presentan de forma en paralelo con los datos de informe de 1 a la izquierda y los datos de informe 2 a la derecha. SPA muestra valores únicos en la misma tabla, pero en lugar de etiquetado las columnas **valor**, se denominan **Report 1** y **Report 2** respectivamente. El informe de side-by-side muestra todos los otros tipos de datos en tablas en paralelo.

El Visor de informes en paralelo también proporciona información sobre herramientas acerca del origen de datos.

### <a name="historical-and-trend-charts"></a>Gráficos históricos y de tendencia

Solo tiene sentido para mostrar tendencias y gráficos históricos para un servidor específico y un módulo específico del Asesor de actualizaciones. Debe elegir el intervalo de tiempo (que se obtuvo el valor predeterminado la última semana) y, a continuación, haga clic en **Aceptar** para que aparezca la tendencia y Visor gráfico histórico.

La tendencia y un gráfico histórico Visor tiene tres pestañas: gráfico histórico, gráfico de tendencias de 24 horas y gráfico de tendencias de 7 días.

### <a name="historical-chart"></a>Gráfico histórico

El gráfico histórico muestra una serie de valores de un punto de datos numéricos mediante el período de tiempo determinado. Por ejemplo, el **latencia media de solicitudes** para IIS en un único servidor durante los últimos 15 días. Cada punto de datos en un gráfico histórico representa el valor de un origen de datos específico realizado en una sesión de análisis de rendimiento.

Hay un par de formas de usar un gráfico histórico:

1.  Para ayudar a encontrar anomalías en un momento determinado para un punto de datos de ejemplo, 2:00 a.m. cada día, el **latencia media de solicitudes** de IIS salta de aproximadamente 200 ms a 500 ms.

2.  Para ayudar a correlacionar varios puntos de datos. Por ejemplo, para mostrar **latencia media de solicitudes** y **recuento medio de solicitud** juntos durante los últimos 15 días. El informe podría mostrar la latencia de solicitud y el aumento del recuento de solicitud al mismo tiempo en detectar, lo que podría indicar que el aumento de latencia de solicitud está provocado por un aumento en el número de solicitudes.

En un gráfico histórico, los usuarios pueden hacer lo siguiente:

* Mostrar varias series de datos en el área del gráfico. Cada serie de datos se muestra como un gráfico de líneas en el Visor de informes. Cada gráfico de líneas se escala automáticamente para ajustarse en el Visor de informes.

* Agregar o quitar una serie de datos de la lista de series de datos en la parte inferior del Visor gráfico histórico.

* Mostrar u ocultar una serie de datos en la lista de series de datos. Los usuarios hacer clic en una serie de datos específicos de la lista para resaltar el gráfico de líneas correspondiente en el área del gráfico.

* Para acercar un determinado período de tiempo seleccionando el período de tiempo dentro del área del gráfico. Para ello, haga clic en el botón que se encuentra en la esquina inferior izquierda del gráfico.

* Investigue un único informe haciendo doble clic en un punto de datos determinado.

* Copie los datos y ponerla a disposición de otros programas, como Microsoft Excel. Esto le permite utilizar gráficos funciones, si procede de Microsoft Excel.

### <a name="trend-charts"></a>Gráficos de tendencias

Muchos problemas de rendimiento repetitivas están provocados por las tareas periódicas que se ejecutan en él o contra los servidores de destino. Por ejemplo, un sitio Web orientada a servicios de entretenimiento puede obtener resultados más durante el fin de semana, o una tarea de copia de seguridad de disco de programación puede ofrecer el rendimiento de un servidor de cada día a las 2:00.

Un gráfico de tendencias está diseñado para ayudarle a encontrar estos problemas de rendimiento. Problemas de rendimiento pueden ocurrir repetidamente en diversos patrones. Los patrones más comunes son patrones diarias y semanal, donde se producen problemas de rendimiento durante la misma hora de un día o el mismo día de la semana. Por lo tanto, la SPA proporciona un gráfico de tendencias de 24 horas y un gráfico de tendencias de 7 días.

El gráfico de análisis de tendencia proporciona un nivel más profundo de investigación en un conjunto de datos y busca de tendencias en función de la hora del día. El eje x se establece en un período de 24 horas, empezando en 0:00 (medianoche) y el final a las 23:59. SPA no muestra las tendencias de varias series de datos al mismo tiempo. Puede hacer clic en **elegir series de datos** para seleccionar una serie de datos para ver.

Para procesar los datos, SPA busca todas las instantáneas realizadas entre 0:00 y 0:59 por cada hora. SPA determina el mínimo, máximo, promedio y valores de sigma para el conjunto de las instantáneas realizadas durante esa hora y los representa gráficamente como gráficos de vela japonesa. SPA repite el proceso para las instantáneas realizadas entre la 1:00 a 1:59, a continuación, 2:00 a 2:59 y así sucesivamente. Si no hay instantáneas existen para la hora en cuestión, SPA deja esa hora en blanco en el gráfico y continúa con la hora siguiente.

Un gráfico de tendencias de 7 días es muy similar del gráfico de tendencias de 24 horas. La única diferencia es que agrupa una serie de datos según el día de la semana en lugar de la hora del día.

La serie de datos que seleccionó en gráficos históricos y de tendencia se almacena como una preferencia de usuario. La próxima vez que la tendencia y un gráfico histórico Visor se abre para el mismo módulo de advisor, el mismo conjunto de series de datos aparecen como el valor predeterminado.

## <a name="managing-reports"></a>Administración de informes


### <a name="deleting-reports"></a>Eliminar informes

Los informes se pueden quitar para minimizar el número de informes que deben administrarse mediante la SPA. Según la frecuencia de informes y el número de servidores, se recomienda eliminar informes innecesarios. Mientras SPA no tiene un límite en los informes que pueden administrar, la base de datos subyacente puede tener un límite de tamaño.

**Tenga en cuenta** informes eliminados no es posible recuperarlo.

 

### <a name="exporting-and-importing-reports"></a>Exportación e importación de informes

Los informes se pueden exportar a un archivo XML al transporte en otra consola SPA o al correo electrónico a otro usuario. Exportar el informe no elimina el informe. Para exportar el informe que está viendo, de **Visor de informes**, haga clic en **acciones**y, a continuación, haga clic en **exportar**. Para exportar varios informes de **el Explorador de informes**, haga clic en **selección habilitar varios**, seleccione varios informes en el cuadro de selección y, a continuación, haga clic en **exportar**. Exporta los informes en formato XML en el directorio de destino seleccionado.

Un informe exportado se puede ver en la SPA. los informes importados no se agregan a la base de datos de la SPA. Están pensados principalmente para actuar como una aplicación de Visor XML para el informe exportado. El servidor para el informe importado no debe tener los mismos paquetes de advisor instalados como la consola original de SPA de informe exportado.

## <a href="" id="bkmk-manageadvisorpacks"></a>Módulos de administración de advisor


SPA incluye los módulos de Asesor de actualizaciones para el sistema operativo principal, Hyper-V, active directory e IIS. SPA proporciona una arquitectura abierta para desarrollar módulos de advisor con SQL, por lo que también es posible que no sean de Microsoft a los desarrolladores crear versiones de módulos de advisor. Hay cuatro opciones para administrar un paquete de advisor: aprovisionar, personalizar, restablecer o quitar.

### <a name="provision-new-advisor-packs"></a>Aprovisionar nuevos módulos de advisor

Nuevos módulos de advisor pueden liberarse por Microsoft o por los desarrolladores que no sean de Microsoft. Un módulo del Asesor de actualizaciones incluye un provisionMetaData.xml y un conjunto de scripts SQL que describen la lógica.

**Para aprovisionar un nuevo módulo de advisor**

1.  Copie todo el contenido del módulo del Asesor de actualizaciones en el *SpaRoot %*\\directorio APs.

2.  En la ventana principal, haga clic en **configuración**y, a continuación, haga clic en **Configurar módulos Advisor**. El **Configurar módulos Advisor** abre el cuadro de diálogo.

    **Tenga en cuenta** este cuadro de diálogo es similar a la **aprovisionar paquete advisor** página en la primera vez que Use el Asistente para. Muestra una lista de módulos del Asesor de actualizaciones que están disponibles para administrar. Cada módulo del Asesor de actualizaciones de la lista tiene propiedades como el nombre, instala la versión, la versión y autor. Nombre es el nombre completo del módulo del Asesor de actualizaciones de, y la versión instalada es la versión de este módulo de advisor ya se ha aprovisionado en el proyecto. Si el paquete de advisor no se aprovisiona en la base de datos actual, se muestra el cuadro de texto de la versión instalada **no instalado**. El campo de versión indica la versión de este módulo de advisor, que se archiva bajo la carpeta de módulos del Asesor de actualizaciones.

     

3.  Seleccione el módulo del Asesor de actualizaciones de la lista. Si no se ha aprovisionado el módulo del Asesor de actualizaciones o si hay una versión más reciente en la carpeta de módulos del Asesor de actualizaciones a la de la base de datos, el **aprovisionar** botón está habilitado. Haga clic en el **aprovisionar** botón.

4.  Cuando se complete, el aprovisionamiento del **versión instalada** campo para el módulo de advisor seleccionado contiene la nueva información de versión.

### <a name="customize-advisor-packs"></a>Personalizar los módulos de advisor

SPA define una arquitectura abierta que permite a los usuarios modificar los módulos de advisor. Los usuarios pueden modificar los archivos del módulo del Asistente para cambiar los umbrales, uso compartido de los umbrales y habilitar o deshabilitar reglas.

Para obtener más información acerca de cómo cambiar y crear paquetes de advisor, consulte [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="changing-thresholds"></a>Cambiar los umbrales

Los umbrales se usan en la SPA para determinar si se cumple la condición del desencadenador de una regla. Los umbrales reales escenarios de cliente para el real pueden variar enormemente debido a la carga de trabajo, el entorno de hardware y negocio necesita. Los umbrales predeterminados podrían no ser adecuados para el caso de usuario actual, por lo que SPA proporciona la capacidad de modificar el umbral existente.

Puede modificar los valores de umbral, haga clic en el nombre de la regla en un informe único o en paralelo. O bien para administrar todas las reglas de un módulo determinado del Asesor de actualizaciones, pueden modificar los valores de umbral de la **configuración** menú.

**Para cambiar un valor de umbral**

1.  En el **configuración** menú, haga clic en **Configurar módulos Advisor**, haga clic en el nombre del módulo del Asesor de actualizaciones que se modificará y, a continuación, haga clic en **configurar**.

    **Tenga en cuenta** se le presentará una lista de todas las reglas que se incluyen en el paquete de advisor. La casilla situada a la izquierda del nombre de módulo del Asesor de actualizaciones indica si la regla está habilitada. Si se deshabilita una regla, se oculta de todos los informes.

     

2.  Haga clic en la regla específica que desea modificar. El **detalles de la regla** formulario para la regla seleccionada se abre.

El formulario de detalles de la regla contiene información detallada sobre una regla específica. Incluye el nombre, descripción, estado, resultados posibles y los umbrales. SPA admite dos tipos de resultados de la regla, **advertencia** y **Aceptar**. Para cada tipo, no hay texto de las recomendaciones y una recomendación.

Algunas de las reglas no tiene umbrales definidos. Por ejemplo, el **Keep Alive de HTTP** regla comprueba si un valor booleano para IIS. Por lo que la lista de los umbrales puede estar vacía. En caso contrario, se enumeran todos los umbrales usados por la regla actual. Una descripción detallada sobre cómo se usa un umbral en la regla se incluye como parte de la descripción.

Si el **detalles de la regla** formulario se inicia desde el **configuración** menú, la lista de umbral tiene tres columnas: nombre de la configuración original y cambiar la configuración. Si se inicia desde un informe único o en paralelo, los valores de umbral que se utilizan en el informe se incluirán también. Los usuarios pueden modificar los valores de umbral actual si cambia el valor en **cambiar configuración** columna y, a continuación, haga clic en **guardar** para guardar los cambios en la base de datos.

Todos los cambios realizados en los umbrales solo está se aplica a los informes que se generan después de los cambios. Los informes existentes no son se ven afectados por estos cambios.

### <a name="sharing-thresholds"></a>Los umbrales de uso compartido

Si administra sus servidores en situaciones similares, puede usar el mismo conjunto de umbrales. Puede exportar e importar los umbrales de un módulo específico del Asesor de actualizaciones mediante la **configuración** menú. Puede seleccionar el módulo específico del Asesor de actualizaciones y, a continuación, haga clic en **configurar**. El archivo exportado umbral está en un formato XML.

Al importar un umbral, SPA valida el formato de archivo XML y comprueba que el archivo coincide con el módulo del Asesor de actualizaciones seleccionadas. Si esto se realiza correctamente, SPA importa todos los valores del archivo de umbral en la base de datos del proyecto actual. Es similar al escenario anterior cambiar umbrales, todos los cambios de valor de umbral solo surten efecto en los informes que se generan en el futuro. Los informes existentes no resultan afectados.

### <a name="enable-or-disable-rules"></a>Habilitar o deshabilitar reglas

Una regla puede habilitarse o deshabilitarse desde el **detalles de la regla** formulario. Debe hacer clic en **guardar** para conservar los cambios realizados. Si se deshabilita una regla, no es mostrarse en cualquiera de los informes. Pero la lógica empresarial subyacente se desencadena al generar el informe, por lo que si decide volver a habilitar la regla, se muestra en los informes de nuevo.

### <a name="reset-advisor-packs-to-original-state"></a>restablecer los módulos de advisor a estado original

Puede decidir modificar un módulo aprovisionado del Asesor de actualizaciones en la base de datos. Además de modificar los umbrales, también puede cambiar las secuencias de comandos SQL. SPA no es compatible con los metadatos del informe, cambiar o agregar o quitar reglas para un módulo aprovisionado del Asesor de actualizaciones. La modificación de esas áreas podría provocar un comportamiento inesperado.

Para obtener más información acerca de cómo cambiar un módulo aprovisionado del Asesor de actualizaciones, consulte el [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Si desea revertir los cambios realizados en un módulo de advisor aprovisionado, puede elegir restablecer el módulo del Asesor de actualizaciones. Esto sobrescribe todos los scripts SQL que están relacionados con el módulo del Asesor de actualizaciones y restablecer todos los valores de los umbrales predeterminados. Esto evita que todos los informes existentes.

Restableciendo el módulo del Asesor de actualizaciones puede realizarse mediante la **Configurar módulos Advisor** formulario. Debe seleccionar el módulo de advisor se puede restablecer y, a continuación, haga clic en **restablecer**.

### <a name="remove-advisor-packs"></a>quitar módulos de advisor

Cuando ya no se necesita un módulo del Asesor de actualizaciones, los usuarios pueden quitar de la base de datos. eliminación del paquete del Asistente para todo lo relacionado con el módulo de advisor quita la base de datos, incluidas las reglas y umbrales, todos los scripts SQL y todos los informes. Ninguna de las acciones se pueden revertir.

eliminación del paquete del Asesor de actualizaciones puede realizarse mediante **Configurar módulos Advisor** formulario. Debe seleccionar el módulo del Asesor de actualizaciones que desea eliminar y, a continuación, haga clic en **Deprovision**.

### <a name="update-existing-advisor-packs"></a>Actualización de paquetes existentes de advisor

Actualización de módulos del Asesor de actualizaciones existentes es muy similar a restablecer el módulo del Asesor de actualizaciones a su estado original. Para actualizar el módulo del Asesor de actualizaciones a una versión más reciente, copie el nuevo módulo de advisor en la carpeta del módulo del Asesor de actualizaciones. Un paquete de advisor se considera una actualización de un módulo del Asesor de actualizaciones existente solo cuando no hay ningún cambio de metadatos de informe. El informe y las reglas de los identificadores deben ser idénticos. En caso contrario, se deben tratar como dos módulos diferentes del Asesor de actualizaciones.

Si hay cambios de lógica del negocio y ningún cambio de metadatos de informe para un paquete de advisor, se debe tener un nuevo número de versión, por ejemplo de 1.0 a 2.0. Si se produce el cambio de metadatos de informe, el módulo del Asesor de actualizaciones debe tener un nombre completo distinto. Por ejemplo, podría cambiar Microsoft.ServerPerformanceAdvisor.IIS.V1 a Microsoft.ServerPerformanceAdvisor.IIS.V2.

Si existe una versión más reciente del módulo del Asesor de actualizaciones, la lista en el **Configurar módulos de Advisor** formulario se rellena automáticamente el **versión** columna con la versión más reciente del módulo del Asesor de actualizaciones. Puede seleccionar el módulo del Asesor de actualizaciones y, a continuación, haga clic en el **restablecer**. Las actualizaciones del módulo del Asesor de actualizaciones con la nueva lógica de negocios y los umbrales. Se conservan todos los informes de este módulo de advisor.

## <a name="managing-servers"></a>Administración de servidores


SPA proporciona capacidades básicas para la administración de servidores de destino. Puede agregar nuevos servidores a la lista de servidores de destino, quite los servidores de la lista o modificar comentarios para los servidores.

**Para agregar o quitar servidores o modificar la información del servidor**

1.  Haga clic en el **configuración** menú, haga clic en **configurar servidores**.

2.  En la lista de servidores que existen actualmente en la base de datos del proyecto, la última fila está vacía. Haga clic en la fila y rellene los campos. El **nombre del servidor** y **ubicación del recurso compartido de archivo** carpeta campos son obligatorios, y el nombre del servidor debe ser único.

3.  Escriba otra información del servidor en el **comentario** columna para cada servidor.

    **Tenga en cuenta** este campo usa un formato de texto sin formato, por lo que puede usar como un campo de descripción. O bien, use este campo para etiquetar los servidores, por lo que se pueden encontrar fácilmente en la ventana principal o a los servidores del grupo, por ejemplo, mediante la función de servidor o ubicación.

     

4.  Si desea usar SPA con un gran número de servidores, SPA es compatible con un formato de valores separados por comas (.csv) para la importación. El archivo debe contener al menos dos campos: **Servidor** y **ubicación del recurso compartido de archivos**. El tercer campo, **comentario** es opcional, pero se recomienda para organizar los servidores. También puede exportar la lista de servidores a un archivo .csv para determinar el formato adecuado o realizar copias de seguridad de la configuración del servidor.

### <a name="searching-and-filtering"></a>Buscar y filtrar

Si administra más de unos pocos servidores, SPA proporciona compatibilidad básica para encontrar rápidamente los servidores en la ventana principal. Puede hacer clic en la columna de encabezado para ordenar según el nombre del servidor, los resultados del análisis, estado actual de la tarea o comentarios. También puede usar la funcionalidad de búsqueda. En la esquina superior derecha de la ventana principal, puede escribir una cadena de búsqueda. El **servidor de destino** lista en la ventana principal usa la cadena para filtrar los servidores y para mostrar solo los servidores con los campos de nombre o comentario que contiene la cadena de búsqueda.

La siguiente ilustración muestra cómo la cadena **delL** coincide con los servidores con la cadena **delL** o servidores con **comentario** campo que contenga **delL**.

![ejemplo de filtrado y búsqueda](../media/server-performance-advisor/spa-user-manual-find-search-example.png)

## <a name="advanced-functionality"></a>Funcionalidad avanzada


### <a name="working-with-spa-windows-powershell-cmdlets"></a>Trabajar con los cmdlets de PowerShell de Windows de SPA

La consola SPA tiene soporte técnico a través de la interfaz de usuario para la recopilación periódica de datos. Si esa funcionalidad no es suficiente para su entorno, hay cmdlets de Windows PowerShell que un administrador avanzado puede usar para personalizar la recopilación de datos. Estos cmdlets de Windows PowerShell permiten a los administradores de sistema ejecutar el análisis de rendimiento en servidores de destino y use SPA de asistencia al cliente remoto automáticamente. Por ejemplo, los administradores del sistema pueden escribir secuencias de comandos para invocar los cmdlets SPA en determinados intervalos de tiempo que se tomarán muestras periódicamente la condición de rendimiento de los servidores de destino.

Se recomienda cerrar la aplicación SPAConsole.exe antes de usar estos cmdlets.

Antes de ejecutar los cmdlets de Windows PowerShell, deberá registrar los cmdlets en el equipo de la consola.

**Para registrar los cmdlets de Windows PowerShell**

1.  Desde un símbolo de Windows PowerShell con privilegios elevados, escriba **registerSpaCmdlets.cmd**. El **registrar los cmdlets SPA correctamente** aparece el mensaje.

2.  Ejecute **SPA PowerShell.cmd**. Si se pasa la ruta de acceso a un archivo de script de Windows PowerShell, ejecute los scripts automáticamente. En caso contrario, se abre un símbolo de Windows PowerShell, que está listo para ejecutar los cmdlets de PowerShell de Windows de SPA.

En la tabla siguiente se describe los cmdlets de PowerShell de Windows de SPA:

| Nombre del cmdlet | Parámetros | Descripción |
| ------ | ------- | ------ |
| Start-SpaAnalysis | **-ServerName** nombre del servidor de destino.<br>**-AdvisorPackName** nombre completo del módulo de advisor para poner en cola en el servidor. Cuando más de un módulo está programado para ejecutarse al mismo tiempo, el valor del parámetro debe tener formato AP1name, AP2name.<br>**-Duración** duración para la recopilación de datos.<br>**-Credential** las credenciales de usuario para la cuenta que ejecuta la recolección de datos en el servidor de destino.<br>**-SqlInstanceName** nombre de la instancia de SQL Server.<br>**-SqlDatabaseName** nombre de la base de datos del proyecto SPA. | Inicia una sesión de recolección de datos SPA en el servidor especificado. |
| Stop-SpaAnalysis | **-SqlInstanceName** nombre de la instancia de SQL Server.<br>**-SqlDatabaseName** nombre de la base de datos del proyecto SPA.<br>**-ServerName** nombre del servidor de destino. | Intenta detener una sesión SPA en ejecución. Si una sesión se ha completado, devuelve sin hacer nada. |
| Get-SpaServer | **-SqlInstanceName** nombre de la instancia de SQL Server.<br>**-SqlDatabaseName** nombre de la base de datos del proyecto SPA. | Obtiene la lista de servidores en la base de datos. Devuelve una lista de objetos, incluidas estas propiedades: Nombre, estado, recurso compartido de archivos y comentario. |
| Get-SpaAdvisorPacks | **-SqlInstanceName** nombre de la instancia de SQL Server<br>**-SqlDatabaseName** nombre de la base de datos del proyecto SPA | Obtiene la lista del módulo del Asesor de actualizaciones en la base de datos. Devuelve una lista de objetos, incluidas estas propiedades: Nombre, DisplayName, autor y versión. |

Windows PowerShell proporciona la capacidad para pasar las credenciales a través de los archivos cifrados para habilitar escenarios de automatización. Para obtener más información sobre el uso de archivos cifrados para pasar credenciales a un cmdlet, consulte [crear Scripts de Windows PowerShell que acepte credenciales](https://technet.microsoft.com/magazine/ff714574.aspx).

### <a name="automating-spa-report-collection-by-using-windows-powershell"></a>Automatización de la colección de informes SPA mediante Windows PowerShell

Los procedimientos siguientes representan un ejemplo de cómo automatizar un conjunto de informe SPA mediante los cmdlets de PowerShell de Windows de SPA.

Las credenciales de usuario pueden cifrarse y almacenar en caché a través de Windows PowerShell. Estas credenciales se utilizan para iniciar sesión en servidores remotos. Aunque no son específicas para SPA, el ejemplo siguiente muestra esta técnica:

``` syntax
$fileName = 'D:\temp\operator.txt'
$userName = 'domainname\operator'
 
# save credential to file
$(Get-Credential).Password | convertFrom-SecureString | Set-Content $fileName
 
# load credential from file
$credential = New-Object System.Management.Automation.PsCredential $userName, $(Get-Content $fileName | convertTo-SecureString)
 
# run command
.\start-SpaAnalysis  ServerName: Server1  Credential: $credential  AdvisorPackName:Microsoft.ServerPerformanceAdvisor.CoreOS.V1 10  Duration:10  SqlInstanceName: .\SQLExpress  SqlDatabaseName:SPA8294
```

Antes de poder ejecutar este archivo, debe ejecutar **set-executionpolicy remoteSigned**

Puede ejecutar desde un archivo por lotes mediante el comando siguiente:

``` syntax
PowerShell -command "& '.\RunSpa.ps1' "
```

### <a name="use-t-sql-to-generate-reports"></a>Usar Transact-SQL para generar informes

Datos de informe SPA se pueden extraer con SQL para crear informes personalizados no se proporcionan dentro de la SPAConsole.exe.

Por ejemplo, el siguiente comando de Transact-SQL proporciona una lista de las 10 de servidores por CPU para el período de tiempo que abarca los últimos tres días:

``` syntax
select TOP 10
   MachineName,
   AverageCpu
FROM (
   select
      __MachineName MachineName,
      AVG(AverageValue) AverageCpu
   FROM [SPA].[Microsoft.ServerPerformanceAdvisor.CoreOS.V1].vwCpuPerformance
   WHERE __Creationtime > dateadd(DAY, -3, GETUTcdate())
      AND Name = N'% Processor time' AND CpuId = N'_Total'
   GROUP BY __MachineName
) t
OrdER BY t.AverageCpu DESC 
```

### <a name="working-with-multiple-projects"></a>Trabajar con varios proyectos

En algunos casos, es posible que desea crear particiones de las bases de datos de SQL Server utilizados por la SPA. Esto puede ayudar a reducir el tamaño de datos de la base de datos de SQL Server o le ayudarán a los servidores de una partición lógica. En estos casos, la consola SPA es compatible con varios proyectos. Puede crear una nueva base de datos del proyecto haciendo **archivo**y, a continuación, haga clic en **nuevo proyecto**. La primera vez que Use el Asistente para aparece para ayudarle a crear la nueva base de datos del proyecto.

Después de que se crean los proyectos, haga clic en **archivo**y, a continuación, haga clic en **Abrir proyecto** para cambiar entre proyectos. La consola SPA no permite cambiar las bases de datos cuando se ejecutan el análisis de rendimiento sobresaliente dentro del proyecto abierto actualmente. Esto sirve para proteger la integridad de los datos de rendimiento.

Cuando elige crear una nueva base de datos de proyecto o abrir una base de datos de otro proyecto, se cierra el proyecto actual. Todos los informes abiertos se cierran cuando se cierra el proyecto actual.

**Tenga en cuenta** SQL Server 2008 R2 Express tiene un límite de la base de datos de 10 GB. Mediante el uso de varios proyectos, puede usar uno o más bases de datos de SQL Server y sobrepasar el límite de 10 GB SQL Server 2008 R2 Express.

 

### <a name="logging-and-debugging"></a>Registro y depuración

SPA proporciona funcionalidad de registro básica. Solo permite los registros se escriban en un archivo de registro, que se encuentra en la misma carpeta que SPAConsole.exe. La cuenta de usuario que ejecuta la consola SPA debe tener permiso de escritura a la carpeta donde se instaló la SPA para asegurarse de que los registros pueden escribirse en el archivo de registro.

SPA contiene los siguientes niveles de registro válido:

* **Informativo** vuelca los registros de todas las acciones que se ocupa de la consola SPA y está diseñado principalmente para fines de depuración.

* **Advertencia** registra todos los errores y excepciones que se producen dentro de la consola SPA. Algunos de los errores son simplemente los errores de validación que pueden controlarse mediante la consola SPA.

* **Crítica** registra solo los errores y excepciones que no pueden controlarse mediante la consola SPA. Estos errores, hacer que la consola SPA se bloquee. Los registros proporcionan información de contexto para estos errores.

De forma predeterminada, el nivel de registro es advertencia, lo que significa que SPA solo registra errores y excepciones que se producen en la SPA. El nivel de registro puede cambiarse modificando el **SpaConsole.exe.config** en la misma carpeta de archivos como SpaConsole.exe se encuentra. Todos los registros se escriben en el archivo log.txt en la misma carpeta.

SPA también proporciona alguna funcionalidad básica para la depuración. Para activar la depuración para SPA, los usuarios necesitan modificar manualmente la base de datos del proyecto SPA. La configuración se almacena en una tabla de configuraciones. Usuario debe ejecutar el siguiente script SQL para cambiar el proyecto SPA para modo de depuración:

``` syntax
UPdate [Configurations] SET Value = N'true' WHERE Name = N'Debugmode'.
```

En modo de depuración, el marco SPA conserva todos los datos temporales que se generan durante el análisis de rendimiento para que pueda solucionar los problemas de los módulos de advisor. Este script está diseñado principalmente para usarse por desarrolladores de módulo del Asesor de actualizaciones.

Para obtener más información sobre las capacidades de depuración de SPA, consulte el [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="managing-the-database"></a>Administración de la base de datos

### <a name="backup-and-restore-the-database"></a>Copia de seguridad y restaurar la base de datos

La consola SPA no proporciona funcionalidad para realizar copias de seguridad y restaurar la base de datos de la SPA. Debe usar las herramientas estándar de Microsoft SQL Server y las secuencias de comandos para realizar copias de seguridad y restaurar bases de datos.

### <a name="clean-up-the-database"></a>limpiar la base de datos

Las bases de datos del proyecto SPA pueden aumentar de tamaño cuando se ejecuta un análisis más rendimiento. De forma predeterminada, SPA mantiene todos los informes. Es posible que desea quitar informes antiguos para ayudar a mejorar el rendimiento. Puede eliminar los informes a través de la **el Explorador de informes** interfaz.

## <a name="privacy-and-security"></a>Privacidad y seguridad


SPA solo admite la recopilación de datos de servidores de destino en la consola SPA. No está diseñado para enviar información a Microsoft o a los desarrolladores que no sean de Microsoft. Para obtener más información acerca de la privacidad SPA, consulte los términos de licencia del Software de Microsoft para Server Performance Advisor.

Todos los datos recopilados por la SPA se almacena en las bases de datos del proyecto. Dada la naturaleza de algunos de los seguimientos ETW, SPA puede recopilar información confidencial que podría tener una importancia empresarial alta. Debe tener en cuenta el posible riesgo asociado con el uso compartido de acceso a las bases de datos del proyecto SPA. Archivos de registro temporal se guardan en las carpetas compartidas que se especifican en cada equipo de destino. Aunque se trata de SPA eliminar esos registros temporales, los archivos cuando finalice la importación de datos, no hay ninguna garantía de que siempre se eliminarán los archivos de registro. Asegúrese de que las carpetas compartidas se encuentran en ubicaciones seguras.

Módulos de SPA de advisor contiene secuencias de comandos SQL para analizar y analizar los registros de rendimiento para generar informes de rendimiento. SPA intenta limitar el privilegio esas secuencias de comandos que se ejecutan en. Sin embargo, hay una posibilidad de que las secuencias de comandos pueden recopilar información confidencial a través de SPA de servidores de destino, o get o modificar la información confidencial que se almacena en la misma base de datos del proyecto SPA. Deberá asegurarse de que todos los módulos del Asesor de actualizaciones que se aprovisionan en la base de datos del proyecto SPA proceden de orígenes de confianza.

## <a name="errors-and-troubleshooting"></a>Errores y solución de problemas


### <a name="spaconsoleexe-does-not-start-or-write-log-file"></a>No se inicie SPAConsole.exe o escribir el archivo de registro

Cuando intenta ejecutar SPAConsole.exe por primera vez, si no está instalada .NET Framework, la aplicación no inicie ni escribir en un archivo de registro. Asegúrese de que un compatible.NET que Framework está instalado y el trabajo correctamente antes de empezar la SPA.

### <a name="locating-log-information"></a>Buscar información de registro

SPA almacena información sobre los errores en el archivo log.txt bajo la carpeta de la SPA. información de pila de llamadas y mensajes de error detallados se escriben en esta carpeta. Si experimenta errores que se necesitan más información para interpretar, puede abrir el archivo log.txt para ver los detalles del error.

### <a name="database-size-limitations-for-sql-server-express"></a>Limitaciones de tamaño de base de datos de SQL Server Express

SQL Server Express tiene un límite de tamaño de 10 GB para una base de datos de usuario. Esta base de datos de tamaño tiene la capacidad para almacenar los informes de 20.000-30.000. Para reducir la posibilidad de alcanzar este límite de la base de datos, se recomienda eliminar informes periódicamente o con otra versión de SQL Server que no tiene la limitación de base de datos de usuario de 10 GB.

### <a name="sql-server-express-log-size-and-disk-capacity"></a>SQL Server Express disco y el tamaño de la capacidad del registro

Si usa SQL Server Express, la base de datos de usuario está limitado a 10 GB, pero el correspondiente archivo de registro puede superar los 70 GB. Por estas razones, se recomienda 100 GB o más de espacio libre en disco para SQL Server Express. Este espacio en disco debe ser suficiente para almacenar aproximadamente 20 000 a 30.000 informes. Este archivo de registro se denomina SPADB\_log.ldf y se encuentra en **% Program Files %\\Microsoft SQL Server\\MSSQL10. SQLEXPRESS\\MSSQL\\datos**.

### <a name="failure-to-connect-to-target-server"></a>Error al conectar al servidor de destino

Al ejecutar análisis de rendimiento en servidores de destino, la cuenta de usuario que ejecuta la consola SPA debe tener ciertos privilegios para poner en cola el recopilador de datos que se establezca en PLA en los servidores de destino. También debe cambiarse para permitir comunicaciones PLA pasar a través de configuración de Firewall de Windows. Para obtener instrucciones de configuración detallados, consulte [configurar SPA](#bkmk-setupspa).

### <a name="failure-to-create-pla-data-collection-set-on-the-target-server"></a>Error al crear el conjunto de recopilación de datos PLA en el servidor de destino

Si se produce una no se puede crear el conjunto de recopilación de datos PLA en mensajes del servidor de destino, asegúrese de hacer lo siguiente:

* Asegúrese de que el **los registros de rendimiento y alertas** servicio se está ejecutando

* La configuración de seguridad **acceso de red: No permitir el almacenamiento de contraseñas y credenciales para la autenticación de red** está deshabilitado. La configuración de seguridad debe deshabilitarse porque SPA debe usar las credenciales de usuario para crear el conjunto de recopilación de datos en el servidor de destino.

### <a href="" id="running-spa-against-the-console-"></a>Ejecución de SPA en la consola

PLA pasa por un canal diferente si el servidor de destino es igual a la consola SPA. Incluso si la cuenta de usuario se está ejecutando la consola SPA con privilegios de administrador, se produce un error en PLA. Si la consola SPA está instalada en un servidor de destino, los usuarios necesitan iniciar SPA como administrador para asegurarse de que puede ejecutar la tarea de análisis de rendimiento en la consola.

### <a name="running-multiple-consoles-at-the-same-time"></a>Ejecución de varias consolas al mismo tiempo

SPA no es compatible con varias consolas de ejecución en la misma base de datos del proyecto SPA al mismo tiempo. También SPA no proporciona el mecanismo de bloqueo y la sincronización para evitar que suceda. Si dos consolas SPA se ejecutan al mismo tiempo, la consola se comportará de forma incoherente dependiendo de la secuencia de tiempo que se ejecutan estas consolas SPA. Para evitar esto, todas las sesiones de análisis de rendimiento en cola deben quitarse de la lista antes de que sea procesado por la consola que inicia el análisis.

SPA protege la integridad de cada informe que se generó correctamente por la SPA. al mismo tiempo, SPA no garantiza que se hayan completado todas las tareas de análisis en cola. Si ve los cambios de estado incoherente para las sesiones de análisis de rendimiento o errores que el sistema de notificación no pueden encontrar los registros de rendimiento generados por el conjunto de recopiladores de datos, suele deberse a varias instancias de la consola SPA que se ejecuta en el mismo Base de datos de proyecto SPA.

Ejecución de cmdlets de PowerShell de Windows de SPA también podría verse afectada por una consola SPA que se ejecuta en la misma base de datos de la SPA. Se recomienda que cierre la consola SPA antes de ejecutar los cmdlets de PowerShell de Windows de SPA.

### <a name="spaconsoleexe-recurring-collection-is-disrupted"></a>Se interrumpe la recopilación periódica de SPAConsole.exe

Cuando se ejecuta el SPAConsole.exe y se utiliza una recopilación periódica de datos (por ejemplo, una colección cada hora), el servidor que ejecuta el SPAConsole.exe no debe estar en modo de ahorro de energía tal que puede suspender. SPA no comprueba la directiva de ahorro de energía. Esta actividad suspendida puede afectar a la recopilación de datos periódicos regulares.

### <a name="lost-etw-events"></a>Eventos ETW perdidos

Para crear el impacto mínimo en el rendimiento en los servidores de destino, PLA está diseñado para ejecutarse con prioridad baja, mientras que está recopilando información sobre el rendimiento. Si el servidor de destino está ocupado, PLA puede quitar algunas de las tareas de recopilación de datos para dar prioridad a las tareas de alta prioridad que se ejecutan en servidores de destino. Debe considerar la configuración de la carpeta compartida donde los eventos se escriben en un disco que entran en conflicto t con la s E/S de carga de trabajo o una unidad más rápida, como un SSD. Cuando se quitan los eventos, se notifican en la vista de informe, ya que eventos perdidos pueden afectar la confiabilidad de las métricas de rendimiento generado.

También podrían provocar algunos problemas de permisos del registro o las consultas WMI que se omitan. Sin embargo, esto es mucho menos probable que la pérdida de eventos ETW. Como resultado, los resultados de conjunto de recopiladores de datos a veces no contienen todos los valores que se solicitan. Deberá asegurarse de que la situación se controla mediante los scripts de Transact-SQL para todos los módulos del Asesor de actualizaciones. Si los datos no existe en el resultado de la colección de datos, se marca como no hay datos en los informes.

Dado que es común para PLA pérdida de eventos ETW, los puntos de datos que se generan en función de un seguimiento ETW pueden no ser coherentes con los puntos de datos que se generan basado en, por ejemplo, los contadores de rendimiento. Por ejemplo, es posible que vea que el uso total de CPU por IIS es el 80% (que proviene de los contadores de rendimiento) y que la parte superior direcciones URL solo usan 10% de todo el tiempo de CPU (que es un punto de datos que procede de la traza ETW). Normalmente, el origen de datos para un punto de datos puede verse a través de la información sobre herramientas del punto de datos. Debe tener en cuenta los efectos de este tipo de pérdida de datos.

Para evitar la pérdida de estos eventos, se debe cerrar la carpeta de resultados para el servidor de destino.

Si los resultados del recopilador de datos contienen datos incompletos aparte de la pérdida de seguimiento ETW y el desarrollador de advisor pack había agregado compatibilidad para la notificación de pérdida de eventos ETW, se muestra una barra de información sobre el informe solo para notificar al usuario sobre el potencialmente causados por la pérdida de datos incoherentes en el informe. información de pérdida de datos detallados se puede encontrar en el archivo log.txt.

## <a name="glossary"></a>Glosario


Estos son algunos de los términos usados con SPA:

* **Paquete de Advisor** una colección de metadatos y los scripts de Transact-SQL que procesan los registros de rendimiento que se recopilan desde el servidor de destino. El módulo del Asesor de actualizaciones, a continuación, genera informes de los datos de registro de rendimiento. Los metadatos en el módulo de advisor definen los datos que se recopilan desde el servidor de destino para las medidas de rendimiento. Los metadatos también definen el conjunto de reglas, los umbrales y el formato de informe. A menudo, un paquete de advisor está escrito específicamente para un rol de servidor único, Internet, por ejemplo, servicios de Information (IIS).

* **Consola SPA** SpaConsole.exe, que es la parte central de SPA. SPA no es necesario ejecutar en el servidor de destino que se está probando. La consola SPA contiene todas las interfaces de usuario para SPA, desde la configuración del proyecto para ejecutar el análisis y visualización de informes. Por diseño, SPA es una aplicación de dos niveles. La consola SPA contiene la capa de interfaz de usuario y la parte de la capa de lógica de negocios. La consola SPA programa y procesa las solicitudes de análisis de rendimiento.

* **Marco de SPA** proporciona todos los interfaces de usuario, el procesamiento del registro de rendimiento, configuración, control de errores y los procedimientos de administración y las API de base de datos.

* **Proyecto SPA** informa de una base de datos que contiene toda la información sobre los servidores de destino, los módulos de advisor y análisis de rendimiento que se generan en los servidores de destino para los módulos de advisor. Puede comparar y ver los gráficos de historial y la tendencia dentro del mismo proyecto SPA. Puede crear más de un proyecto. Los proyectos SPA son independientes entre sí y no hay ningún dato que se comparten entre varios proyectos.

* **Servidor de destino** el equipo físico o máquina virtual que ejecuta Windows Server con determinados roles de servidor, como IIS.

* **Sesión de análisis de datos** un análisis del rendimiento en un servidor de destino específico. Una sesión de análisis de datos puede incluir varios módulos de advisor. Los conjuntos de recopiladores de datos de esos módulos de advisor se combinan en un conjunto de recopiladores de datos único. Todos los registros de rendimiento para una sesión de análisis de datos solo se recopilan durante el mismo período de tiempo. Analizar los informes generados por los módulos del Asesor de actualizaciones que se ejecutan en la misma sesión de análisis de datos puede ayudar a los usuarios a comprender la situación general del rendimiento e identificar las causas raíz de problemas de rendimiento.

* **Seguimiento de eventos para Windows** un sistema de seguimiento de alto rendimiento, baja sobrecarga y escalable que se proporciona en Windows. Proporciona la generación de perfiles y funciones, que pueden utilizarse para solucionar una variedad de escenarios de depuración. SPA usa eventos ETW como un origen de datos de generación de informes de rendimiento. Para obtener información general sobre ETW, vea [Improve Debugging and Performance Tuning with ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

* **Windows Management Instrumentation (WMI)** la infraestructura de datos de administración y operaciones en Windows. Puede escribir scripts de WMI o las aplicaciones para automatizar tareas administrativas en equipos remotos. WMI también proporciona datos de administración de productos y otras partes del sistema operativo. SPA usa información de clase WMI y los puntos de datos como orígenes para generar informes de rendimiento.

* **Contadores de rendimiento** utilizado para proporcionar información sobre el sistema operativo o una aplicación, servicio o controlador de rendimiento. Los datos del contador de rendimiento pueden ayudar a determinar cuellos de botella del sistema y optimizar el rendimiento del sistema y aplicación. El sistema operativo, red y dispositivos proporcionan datos del contador que puede consumir una aplicación para proporcionar a los usuarios una vista gráfica de buen rendimiento del sistema. SPA usa información del contador de rendimiento y los puntos de datos como orígenes para generar informes de rendimiento.

* **Los registros de rendimiento y alertas (PLA)** rendimiento recopila registros y realiza un seguimiento y genera alertas de rendimiento cuando se cumplen determinados desencadenadores. PLA puede usarse para recopilar los contadores de rendimiento, seguimiento de eventos para Windows (ETW), las consultas WMI, claves del registro y configuración de los archivos. PLA también admite la recopilación de datos remotos a través de llamadas a procedimiento remoto (RPC). El usuario define un conjunto de recopiladores de datos, que incluye información acerca de los datos que se recopilen, frecuencia de recopilación de datos, duración de recopilación de datos, filtros y una ubicación para guardar los archivos de resultados. SPA utiliza PLA para recopilar todos los datos de rendimiento de los servidores de destino.

* **Único informe** informe un SPA que se genera en función de la sesión de análisis de datos para el paquete de un asesor en un servidor de destino único. Puede contener las notificaciones y las diversas secciones de datos.

* **Informe de Side-by-side** informe un SPA que compara dos informes únicos para el mismo módulo de advisor. Los dos informes pueden generarse desde servidores de destino diferente o desde las ejecuciones de análisis de rendimiento independientes en el mismo servidor de destino. El informe side-by-side crea la capacidad de comparar dos informes para ayudar a los usuarios a identificar comportamientos anómalos o configuración en uno de los informes. Un informe en paralelo contiene las notificaciones y las diversas secciones de datos. En cada sección, datos de ambos informes están enumerado side-by-side.

* **Gráfico de tendencias** informe un SPA que se usa para investigar los patrones repetitivos de problemas de rendimiento. Muchos problemas de rendimiento repetitivas están provocados por cambios de carga de servidor programadas desde el servidor o desde los equipos cliente, lo que pueden suceder diaria o semanal. SPA proporciona un gráfico de tendencias de 24 horas y un gráfico de tendencias de 7 días para identificar estos problemas.

    El usuario puede elegir una o más series de datos a la vez, que es un valor numérico en el informe, como **promedio de uso de CPU total**. más específicamente, un valor numérico es un valor escalar desde un único servidor que genera un único punto de acceso a una instancia de tiempo determinado. SPA agrupa esos valores en 24 grupos, uno por cada hora del día (siete para un informe de 7 días, uno para cada día de la semana). SPA calcula el promedio, mínimo, máximo y desviaciones estándar para cada grupo.

* **Gráfico histórico** informe un SPA que se usa para mostrar los cambios en determinados numérico valores dentro únicos informes para un servidor determinado y Asesor de par de módulo con el tiempo. El usuario puede elegir varias series de datos y mostrarlos juntos en el gráfico para comprender la correlación entre las distintas series de datos histórico.

* **Serie de datos** datos numéricos que se recopilan desde el mismo origen de datos durante un período de tiempo. El mismo origen significa que los datos tienen que proceder del mismo servidor de destino, como la longitud de cola promedio de solicitud para IIS en un servidor.

* **Reglas** combinaciones de lógica, umbrales y descripciones. Representan un posible problema de rendimiento. Cada paquete de advisor contiene varias reglas. Cada regla se desencadena mediante un proceso de generación de informes. Una regla aplica la lógica y los umbrales para los datos de informe único. Si se cumplen los criterios, se genera una notificación de advertencia. Si no, la notificación está establecida en el **Aceptar** estado. Si no se aplica la regla, la notificación se establece como no aplicable (**NA**) estado.

* **Las notificaciones** información que se muestra una regla a los usuarios. Incluye el estado de la regla (**Aceptar**, **NA**, o un **advertencia**), el nombre de la regla y es posible, recomendaciones para solucionar los problemas de rendimiento.
