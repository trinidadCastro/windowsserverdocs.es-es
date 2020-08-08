---
title: Guía de usuario de Server Performance Advisor
description: Guía de usuario de Server Performance Advisor
ms.assetid: be99a5a9-b9c0-436b-912c-e5c5839a533d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.openlocfilehash: 1a7ea3b902793f281156930a8e666c1d32d05cbe
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993121"
---
# <a name="server-performance-advisor-users-guide"></a>Guía de usuario de Server Performance Advisor

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8

En esta guía de usuario de Microsoft Server Performance Advisor (SPA) se proporcionan instrucciones sobre cómo usar SPA para identificar cuellos de botella de rendimiento en sistemas implementados en varios roles de servidor.

SPA puede ayudarle con lo siguiente:

* Administrar el rendimiento del servidor y solucionar problemas de rendimiento del servidor.

* Proporcionar informes de datos y recomendaciones sobre problemas comunes de configuración y rendimiento.

* Proporcionar recomendaciones de procedimientos recomendados en función de los datos recopilados.

> [!NOTE]
> La consola de SPA no realiza ningún cambio en los servidores.

Para obtener más información sobre el desarrollo de paquetes de Advisor SPA, consulte la [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

## <a name="server-performance-advisor-overview"></a>Información general de Server Performance Advisor


En esta sección se explica el historial de SPA, se describen los escenarios y la audiencia de destino, y se presenta información general sobre la arquitectura de la herramienta.

### <a name="history-of-spa"></a>Historial de SPA

La versión original de Microsoft Server Performance Advisor (SPA) se publicó en 2005-2006. Se destina principalmente a Windows Server 2003, incluidos el sistema operativo principal y los roles de servidor, como Internet Information Services (IIS) y Active Directory. El objetivo de la herramienta es ayudarle a evaluar el rendimiento del servidor y solucionar problemas de rendimiento del servidor.

SPA proporciona informes de datos y recomendaciones a los administradores del sistema acerca de los problemas comunes de configuración y rendimiento. SPA recopila datos relacionados con el rendimiento de varios orígenes en servidores, por ejemplo, contadores de rendimiento, claves del registro, consultas WMI, archivos de configuración y seguimiento de eventos para Windows (ETW). En función de los datos de rendimiento del servidor que recopila, SPA puede proporcionar una visión detallada de la situación actual del rendimiento del servidor y emitir recomendaciones sobre lo que se puede mejorar.

Ha habido más de 1 millón descargas de la herramienta original SPA y ha ayudado a muchos administradores del sistema a administrar el rendimiento de sus servidores. Ha sido una herramienta de solución de problemas de rendimiento muy correcta.

Desde la publicación del SPA original, hay varios cambios importantes en el entorno de rendimiento del servidor. En concreto, la versión de Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008, con las nuevas versiones de las funciones de servidor correspondientes, como Internet Information Services (IIS) 8,5. Las versiones más recientes de Server Performance Advisor amplían la funcionalidad del SPA original a los sistemas operativos de servidor y roles de servidor recientes.

Aunque SPA 3,1 comparte los mismos objetivos de diseño y el paradigma de análisis de rendimiento que la herramienta original, se ha vuelto a escribir con la tecnología más reciente. También tiene las siguientes mejoras clave:

* Puede realizar análisis en servidores que ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008.

* Admite la capacidad de análisis remoto, que permite a SPA 3,1 recopilar y analizar datos desde una consola central, sin necesidad de instalar código en los servidores que se van a analizar.

* Utiliza un Microsoft SQL Server para el análisis y el almacenamiento de datos, lo que permite procesar y almacenar grandes cantidades de datos.

* Separa la herramienta de los paquetes de Advisor. La lógica de análisis de rendimiento de cada rol de servidor se publica en forma de un paquete de asesores, que se puede publicar o actualizar por separado de la herramienta.

* Proporciona paquetes de asesores desarrollados en scripts SQL con una arquitectura abierta. Las entidades que no son de Microsoft pueden desarrollar paquetes de asesores o ampliar los paquetes de asesores existentes de Microsoft para cubrir las necesidades especiales.

* Admite nuevas características, como informes de comparación en paralelo, y gráficos de tendencias e históricos para ayudarle a encontrar anomalías.

* Proporciona características como la modificación, importación y exportación de umbrales para ayudarle a ajustar los informes y las notificaciones y a compartir dichas optimizaciones con otros usuarios de SPA.

* Admite varios proyectos, que se pueden usar para agrupar los servidores de destino.

* Proporciona la recopilación de datos periódica desde la consola de SPA.

* Habilita las consultas personalizadas y la generación de informes mediante Microsoft SQL Server (para usuarios avanzados).

* Los cmdlets de Windows PowerShell personalizados están disponibles para su uso con SPA

* Compatibilidad con versiones anteriores para importar y ver informes desde una base de datos SPA 3,0

### <a name="target-audience"></a>Audiencia de destino

La herramienta SPA está diseñada principalmente para los administradores del sistema que administran menos de 100 servidores en varios roles de servidor. También puede usarlos los ingenieros de soporte técnico para recopilar datos de rendimiento y solucionar problemas de rendimiento para los clientes.

SPA ofrece recomendaciones para ayudarle a solucionar problemas de rendimiento. sin embargo, se basa en suposiciones que podrían no ser aplicables a su entorno de servidor específico. Las recomendaciones que se ofrecen a través de SPA deben considerarse sugerencias. Esperamos que los usuarios de SPA tengan un buen conocimiento de la configuración del sistema, los casos de uso y el impacto de la optimización del sistema en el comportamiento general del sistema. Los administradores del sistema deben decidir si se deben aplicar las recomendaciones de SPA a su entorno.

### <a name="what-s-new-in-spa-31"></a><a href="" id="what-s-new-in-spa-3-1-"></a>Novedades de SPA 3,1

Se han agregado las siguientes características y mejoras en SPA 3,1:

* Active Directory Advisor Pack ayuda a analizar el rendimiento general de la función de servidor de servicios de dominio de Active Directory de servidor.

* La ayuda para que los desarrolladores agreguen una alerta de notificaciones de eventos de ETW perdidas en los paquetes de asesores y que la alerta esté visible cuando se ha generado un informe y se han perdido eventos debido a un registro de alta frecuencia en un disco lento o de gran actividad

### <a name="target-scenarios"></a>Escenarios de destino

Los siguientes son los escenarios de destino para SPA:

* **Entorno de servidor**

    SPA está diseñado para configurarlo y mantenerlo con facilidad. Se aplica a entornos de servidor que usan de 1 a 100 servidores. SPA no escala bien si intenta administrar el rendimiento de más de 100 servidores. En entornos más grandes o más sofisticados, considere la posibilidad de usar System Center Operations Manager.

* **Solución de problemas de rendimiento**

    Puede usar SPA como herramienta de solución de problemas de rendimiento. Proporciona la capacidad de recopilar datos de rendimiento de alto nivel, realiza un procesamiento posterior exhaustivo de los datos para proporcionar una mejor comprensión de su comportamiento general del sistema y marca las anomalías. Cuando un cliente sospecha que hay un problema de rendimiento, puede usar SPA para recopilar y analizar los datos de rendimiento del servidor.

    SPA genera un informe que puede ver. Los informes de SPA proporcionan una lista de notificaciones que resalta posibles problemas y una sección de datos que incluye varios índices de rendimiento, configuraciones y configuraciones para el servidor. Puede usar este informe para identificar el problema de rendimiento específico y usar las recomendaciones para buscar soluciones para el problema. Los informes se pueden comparar con otros informes que se generaron en un momento diferente o en un servidor diferente. Con esta comparación en paralelo, puede determinar las diferencias entre el comportamiento de línea base normal frente al de anómalo.

    **Nota:** SPA no está diseñado para ser una herramienta de depuración o de medición. Además, desde la perspectiva de los servidores, se puede considerar una herramienta de solo lectura y no modifica las configuraciones de los servidores.



* **Supervisión de índice de rendimiento**

    Puede usar SPA para supervisar el índice de rendimiento de los servidores. Puede optar por ejecutar SPA habitualmente en servidores para recopilar datos de rendimiento y, a continuación, ejecutar un gráfico de tendencias o un gráfico histórico para detectar las anomalías. Puede ver el informe para un análisis determinado, obtener más información acerca del problema de rendimiento y, a continuación, utilizar las recomendaciones u otros datos del informe para resolver el problema.

### <a name="spa-architecture"></a>Arquitectura de SPA

La lógica de recopilación de datos de SPA se basa en un protocolo de Windows llamado Registros y alertas de rendimiento (PLA). PLA permite que los programas recopilen datos de rendimiento de servidores locales o remotos, como los contadores de rendimiento, las consultas de WMI, los seguimientos de ETW, las claves del registro y los archivos de configuración. Cuando SPA ejecuta el análisis de rendimiento en un servidor de destino, crea un conjunto de recopiladores de datos PLA basado en el paquete de asesores de SPA específico que seleccionó. El paquete de Advisor contiene el origen de datos que se va a recopilar y el recurso compartido de archivos en el que se almacenarán los registros. La recopilación de datos de SPA solo almacena una cuenta de usuario única. la misma cuenta de usuario utilizada por PLA también se utiliza para escribir los registros.

SPA utiliza una base de datos SQL Server para almacenar los registros de rendimiento que se recopilan de los servidores de destino. SPA importa todos los registros de rendimiento del recurso compartido de archivos a la base de datos y, a continuación, usa la lógica de análisis de datos dentro de cada paquete de Advisor para procesar los datos y generar los informes. Un paquete de Advisor analiza los datos de rendimiento que se recopilaron de los servidores de destino y genera los informes de SPA. Para obtener más información sobre la creación de un paquete de Advisor, consulte la [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Las interfaces e interacciones de usuario de la consola de SPA se compilan como parte de la SPAConsole.exe. Puede usar la consola de para crear en una base de datos, agregar o quitar paquetes de Advisor, administrar servidores de destino, ejecutar análisis de rendimiento y ver informes de rendimiento.

La consola de SPA puede ejecutarse en los siguientes sistemas operativos:

* Windows 8.1

* Windows 8

* Windows 7

* Windows Server 2012 R2

* Windows Server 2012

* Windows Server 2008 R2

* Windows Server 2008

En una aplicación empresarial típica, hay tres niveles: el nivel de presentación, la capa de lógica de negocios y la capa de almacenamiento. SPA está diseñado como un producto de dos niveles en la consola y en la base de datos. La consola de actúa como una capa de presentación con cierta lógica relacionada con el proceso y la base de datos sirve como la capa de almacenamiento y la capa de lógica de negocios. La consola captura los datos proporcionados por el usuario y controla los pasos para la recopilación de datos, el procesamiento de datos y la generación de informes. SPA no depende de los servicios del sistema de Windows.

En el diagrama siguiente se muestra la arquitectura de alto nivel del sistema SPA. El proceso es:

1.  En la consola de SPA, se ejecuta el análisis de rendimiento en los servidores especificados.

2.  Cuando se recopilan los datos de rendimiento, PLA en los servidores de destino vuelve a escribir los registros en el recurso compartido de archivos especificado por el conjunto de recopiladores de datos.

3.  Una vez completada la recopilación de datos en un equipo de destino, la consola de SPA importa los registros en la base de datos de SQL Server.

4.  La consola de invoca la lógica de procesamiento de datos de un paquete de Advisor específico.

5.  El paquete de Advisor procesa los datos y llama a las API de SPA para insertar registros en las tablas de informes del sistema definidas por el marco de SPA.

6.  Puede usar el visor de informes en la consola de para ver los informes generados. Para ayudarle a solucionar problemas de rendimiento, SPA proporciona tres tipos de informes: informes únicos, informes en paralelo y gráficos de tendencias o históricos. Para obtener más información sobre los informes, vea [ver informes](#bkmk-viewingreports).

![arquitectura de spa](../media/server-performance-advisor/spa-user-manual-architecture.png)

## <a name="getting-started-with-spa"></a>Introducción a SPA


### <a name="requirements"></a>Requisitos

La consola de SPA se puede instalar en Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008. No se admite la ejecución de SPA en versiones anteriores del sistema operativo Windows Server. SPA se ejecuta en x86 o x64, pero no admite las arquitecturas IA64 o ARM.

SPA se basa en Windows Presentation Foundation (WPF) 2,0, que forma parte de Microsoft .NET Framework 4, por lo que se requiere .NET Framework 4.

También debe instalar SQL Server 2008 R2 Express en el mismo equipo en el que está instalado SPA. SQL Server 2008 R2 Express es un motor de base de datos gratuito publicado por Microsoft. Puede descargar Microsoft SQL Server 2008 R2 Express desde el centro de descarga de Microsoft. Aunque se recomienda SQL Server 2008 R2 Express, las versiones más recientes de SQL Server también pueden ser compatibles con SPA.

**Nota:** SPA no incluye SQL Server ni la .NET Framework como parte del paquete de instalación de SPA. Después de instalar Microsoft SQL Server 2008 R2 y .NET Framework 4,0, recomendamos que ejecute Windows Update antes de instalar SPA.



Dado que los usuarios pueden crear y administrar bases de datos con SPA, la cuenta de usuario que se usa para ejecutar SPA debe tener los mismos privilegios de administrador que SQL Server.

### <a name="setting-up-spa"></a><a href="" id="bkmk-setupspa"></a>Configuración de SPA

SPA se empaqueta como un archivo. cab que incluye todos los archivos binarios para el marco de SPA, los cmdlets de Windows PowerShell que se usan en escenarios avanzados y los siguientes paquetes de asesores: sistema operativo principal, Hyper-V, Active Directory e IIS. Después de extraer el archivo. cab en una carpeta, no se requiere ninguna instalación adicional. Sin embargo, para ejecutar SPA, debe habilitar la recopilación de datos de los servidores de destino de la siguiente manera:

* Para ejecutar la recopilación de datos de PLA, la cuenta de usuario que use para ejecutar la consola de SPA debe formar parte del grupo de seguridad Administrators en el servidor de destino. Si el servidor de destino y la consola de están en el mismo dominio, la cuenta de usuario de dominio debe formar parte del grupo de seguridad administradores en el servidor de destino. Si el servidor de destino y la consola no están en el mismo dominio, cree una cuenta de usuario administrativo en el servidor de destino con el mismo nombre de usuario y la misma contraseña que la cuenta de usuario que utiliza para ejecutar la consola de SPA.

* Cree una carpeta compartida para los resultados en el servidor.

* Asegúrese de que la cuenta de usuario que usa para ejecutar la consola de SPA tenga permisos de lectura y escritura en la carpeta compartida. PLA usa esta cuenta para escribir registros en la carpeta.
La consola de SPA usa la misma cuenta para leer los registros e importarlos en la base de datos.

    **Nota:** ETW implementa un búfer circular para almacenar el seguimiento y lo mueve a la carpeta compartida cuando sea posible. Si el servidor está ocupado o la operación de escritura es lenta, ETW quita los seguimientos cuando el búfer está lleno. Es importante que la carpeta compartida se encuentre en un servidor con acceso de e/s rápido. Se recomienda que cada servidor de destino tenga una carpeta compartida para minimizar la pérdida de datos causada por la e/s de archivos lenta.



* Para que PLA tenga acceso a los servidores de destino, establezca el Firewall de Windows para permitir el acceso de Registros y alertas de rendimiento remoto en los servidores de destino. PLA usa el puerto TCP 139.

* Asegúrese de que los **registros de rendimiento &** servicio de alertas se está ejecutando.

* Si la consola y el servidor de destino se encuentran en subredes diferentes, también debe establecer el campo dirección IP remota en las reglas de Firewall de entrada en la configuración del **ámbito** de la página **registros y alertas de rendimiento** , como se muestra aquí.

    ![propiedades Pla](../media/server-performance-advisor/spa-user-manual-pla-firewall.png)

* Active la detección de redes en la consola de y en cada uno de los servidores de destino.

* Si el servidor de destino no está unido a un dominio, habilite la siguiente configuración del registro: **HKLM \\ software \\ Microsoft \\ Windows \\ CurrentVersion \\ Policies \\ System \\ LocalAccountTokenFilterPolicy**.

**Nota:** De forma predeterminada, SPA escribe los registros de diagnóstico en la carpeta donde se encuentra SpaConsole.exe. Si SPA está instalado en la carpeta archivos de programa, SPA solo podrá escribir el registro cuando SpaConsole.exe se ejecute como administrador.

Si desea ejecutar el análisis de datos en el equipo de la consola de, debe ejecutar SPA como administrador. PLA pasa por una ruta de acceso de código diferente cuando se ejecuta en un equipo local, lo que requiere privilegios de administrador.

Si desea ejecutar el paquete de asesor de IIS SPA en sus servidores, debe habilitar la consulta WMI y el seguimiento de ETW para el servidor IIS. Puede hacerlo habilitando el **seguimiento** en el servicio de rol de **Estado y diagnóstico** , y con las **herramientas y los scripts de administración de IIS** en herramientas de **Administración** del rol de servidor **servidor Web (IIS)** .



### <a name="creating-your-first-project"></a>Crear su primer proyecto

Una vez que todo está configurado, puede crear su primer proyecto de SPA. Como se describe en la sección anterior, SPA almacena todo lo relacionado con el análisis de datos de rendimiento dentro de una base de datos. Cada proyecto SPA corresponde a una base de datos única. El **Asistente para usar la primera vez** le guía por los pasos siguientes.

**Para crear un proyecto SPA**

1.  Inicie SpaConsole.exe. La consola de entra en un modo sin conexión, donde SPA no está conectado a ninguna base de datos y la ventana principal está en blanco.

2.  Para crear un nuevo proyecto, haga clic en **archivo**y, a continuación, haga clic en **nuevo proyecto**. Esto inicia el Asistente para usar la primera vez. La primera página muestra los pasos que se siguen al usar el asistente:

    * Crear una base de datos

    * Aprovisionar paquetes de asesores

    * Agregar servidores a la lista de servidores de destino

3.  Haga clic en **Next**. La página **crear base de datos del proyecto** le pide que proporcione el nombre de la Microsoft SQL Server instancia en la que desea crear la base de datos. Por ejemplo, si se encuentra en el mismo equipo que la consola de, puede usar **localhost \\ &lt; el nombre &gt; de SQL Server**.

    **Nota:** El nombre de instancia predeterminado para una instalación SQL Server 2008 R2 Express es SQLExpress. En el caso de una instancia de SQL Server 2008 R2 Express instalada en el equipo local, la base de datos suele tener como valor predeterminado el **localhost \\ sqlexpress**. Sin embargo, es posible que se haya cambiado durante la instalación de SQL Server, por lo que debe asegurarse de que usa el nombre de instancia de SQL Server correcto.



4.  Proporcione el nombre de la base de datos. Solo se permiten letras, dígitos y caracteres de subrayado ( \_ ) como caracteres válidos para un nombre de base de datos. Una sugerencia razonable para el nombre de la base de datos de SPA sería **Spa**. Si escribe un nombre no válido, aparece un icono de error rojo. La información sobre herramientas asociada proporciona el motivo del error de validación.

    **Nota:** Es importante recordar el nombre de la base de datos y el nombre de la instancia del servidor, ya que estos son los únicos identificadores del proyecto. Debe proporcionar esta información si desea cambiar a esta base de datos.



5.  Una vez que proporcione el nombre de la instancia del servidor y el nombre de la base de datos, el Asistente para usar la primera vez generará la ubicación del archivo de base de datos.

6.  En la página **crear base de datos de proyecto** , haga clic en **siguiente**. El Asistente para usar la primera vez crea una base de datos y genera todos los esquemas de base de datos, funciones y procedimientos almacenados relacionados con SPA en la base de datos. Este paso puede tardar varios segundos en función del hardware y de la velocidad de la red.

    **Nota:** si se produce un error en este paso, aparece un mensaje de error. Algunos de los problemas comunes son: la consola no se puede conectar a la instancia de SQL Server, no tiene privilegios suficientes para crear la base de datos o el nombre de la base de datos ya existe.



7.  Cuando el paso anterior se ejecute correctamente, verá la página **aprovisionar el paquete de asesor** . Muestra todos los paquetes de Advisor que están disponibles en el equipo. SPA examina automáticamente la carpeta denominada **APS** en el directorio raíz de spa. Muestra el nombre completo, la versión y el autor de cada paquete de Advisor.

    **Nota** para obtener más información sobre cómo se usan el nombre completo y la versión en Spa, consulte [Administración de paquetes de Advisor](#bkmk-manageadvisorpacks) .



8.  Elija los paquetes de asesores que desea aprovisionar en la base de datos del proyecto y, a continuación, haga clic en **siguiente**. También puede hacer clic en **omitir** para pasar al siguiente paso sin aprovisionar ningún paquete de Advisor.

    **Nota:** Puede aprovisionar paquetes de asesores cada vez que use la herramienta. Para obtener más información, vea [administrar paquetes de Advisor](#bkmk-manageadvisorpacks).



9.  En la página **agregar servidores** , para cada servidor que se va a agregar a la lista de servidores de destino, hay dos campos obligatorios que se van a rellenar: **nombre del servidor y de la ubicación del** **recurso compartido de archivos**.

    **Nota:** También hay un campo de **Comentario** que se usa principalmente para clasificar o encontrar el servidor. En casos en los que tenga muchos servidores, puede importar un archivo de valores separados por comas (. csv) que contenga el nombre del servidor, la carpeta de resultados y el campo de comentario opcional. El campo de **Comentario** se utiliza para describir el servidor y el término se puede usar para filtrar los servidores para la recopilación de datos. Si está inicializando los servidores a través del archivo. csv, un error de análisis dentro del archivo no carga los servidores.



10. Es necesario establecer varias configuraciones para habilitar la recopilación de datos de PLA, como se describe en [configuración de spa](#bkmk-setupspa). La página **Agregar servidor** proporciona una funcionalidad de configuración de prueba que le ayudará a solucionar problemas de configuración. Active la casilla asociada al equipo y, a continuación, haga clic en **probar conectividad**. SPA intenta generar un conjunto de recopiladores de datos en los servidores de destino e intenta importar los resultados de nuevo a la base de datos. Si todo es correcto, el **Estado** muestra **Pass**. Si se produce un error, aparece una información sobre herramientas que describe el motivo del error.

11. Cada uno de los servidores se agrega automáticamente a la base de datos, incluso si no pasa la prueba de configuración. Para quitar servidores de la lista, seleccione el nombre del servidor y haga clic en **quitar**.

12. Cuando todo se haya completado, haga clic en **Finalizar** para cerrar el Asistente para usar por primera vez. Si cierra el Asistente de primer uso antes de finalizarlo, se conservan todos los pasos anteriores y ninguno de ellos se revierte automáticamente. Debe realizar manualmente los cambios futuros.

## <a name="running-analysis"></a>Análisis en ejecución


Después de configurar la base de datos, puede ejecutar el análisis de rendimiento en los servidores.

Cada vez que se inicia la consola de SPA, se abre automáticamente el último proyecto que usó el usuario actual. La ventana principal contiene una lista de servidores. Cada servidor tiene cuatro propiedades: el nombre del servidor, el resultado del análisis, el estado actual y el comentario.

* **Nombre del servidor** Nombre del servidor, que es el identificador del servidor. No se permiten nombres duplicados.

* **Resultado del análisis** De forma predeterminada, muestra el resultado de la ejecución del análisis de rendimiento más reciente en el servidor. Si no se ha ejecutado ningún análisis de rendimiento en el servidor, no se muestra **ningún informe**. Si aparece una advertencia generada por el informe, muestra una **ADVERTENCIA** y la marca de tiempo en que se generó el informe más reciente. Si no se ha encontrado ningún problema durante el análisis más reciente en el servidor, muestra **correcto** y la marca de tiempo.

    **Nota:** si ha cambiado recientemente una configuración del sistema, se recomienda volver a ejecutar el análisis para evaluar el impacto global del cambio y obtener un informe actualizado del estado del sistema. SPA no realiza un seguimiento de los cambios de configuración en el sistema sometido a prueba.



* **Estado actual** Muestra el estado de las tareas de análisis de rendimiento que se están ejecutando actualmente en el servidor. Puede cancelar una tarea en ejecución haciendo clic en el icono **Cancelar** , que se designa con una X roja.

* **Comentario** Describe el servidor de destino actual. Por ejemplo, puede describir el servidor mediante el rol de servidor (por ejemplo, SQL Server) o una ubicación (por ejemplo, Kent). SPA usa el **nombre del servidor** y el **Comentario** para buscar y encontrar el servidor adecuado. Puede escribir en el cuadro de texto de búsqueda. Si el **nombre del servidor** o las columnas de la **marca** contienen la cadena exacta que escribió en el cuadro de búsqueda, el servidor se mostrará en la lista de servidores.

Los controles siguientes también están disponibles en la consola de:

* **Repetir** Una casilla que describe la capacidad de repetir una colección con regularidad, en función de un intervalo de tiempo. Para la mayoría de las instalaciones de servidor, querrá que una colección SPA se repita cada hora para tener un historial suficiente para el análisis. Si desea ejecutar la colección solo una vez, no debe activar la casilla **repetir** .

* **quitar periodicidad** Un botón que permite cancelar un trabajo de repetición de la recopilación en curso. Cancela la colección de repetición, pero no la colección actual (si existe) en curso. Esta opción permite restablecer un nuevo intervalo de repetición de la recopilación o ejecutar la colección manualmente.

Antes de iniciar el análisis de rendimiento, seleccione la duración de la recopilación de datos. Aunque la recopilación de más datos ayuda a proporcionar una imagen más precisa de la situación de rendimiento del servidor, también genera un mayor número de registros y puede tener un mayor impacto potencial en el servidor. Elija la duración adecuada de la recopilación de datos en función de sus necesidades específicas. Cada paquete de Advisor define una duración mínima válida. La duración de la recopilación de datos que elija debe ser mayor que la duración mínima de los paquetes de Advisor seleccionados.

Para ejecutar el análisis de rendimiento en los servidores de destino, seleccione los servidores en los que desea ejecutar el análisis de rendimiento y haga clic en **ejecutar análisis** se abrirá un cuadro de diálogo en el que se le pedirá que seleccione los paquetes de Advisor que se ejecutarán en los servidores. Los paquetes de Advisor seleccionados se ejecutan simultáneamente. Los informes que se generan se basan en los datos de rendimiento que se recopilan durante el mismo período de tiempo.

**Nota:** si selecciona un servidor que tiene un análisis de rendimiento recurrente en ejecución, el botón **quitar periodicidad** le permite cancelar la recopilación de datos periódica. SPA no permite varias sesiones de recopilación de datos al mismo tiempo en el mismo equipo.



## <a name="viewing-reports"></a><a href="" id="bkmk-viewingreports"></a>Ver informes


En SPA, hay tres tipos de informe de análisis de rendimiento: informe único, informe en paralelo y gráficos de tendencias y históricos.

Después de ejecutar el análisis de rendimiento, se genera un informe para cada uno de los paquetes de Advisor que se ejecutan en el equipo de destino. En la lista de servidores de la ventana principal, puede expandir el **resultado del análisis** para ver todos los paquetes de Advisor que se han ejecutado en el servidor específico. Puede hacer clic en un nombre de informe para ver un solo informe.

Hay tres iconos junto al nombre del paquete de Advisor que muestran el estado de la ejecución del análisis más reciente en el servidor:

* El icono **más reciente** muestra el informe generado por el análisis de rendimiento más reciente de este servidor para el paquete de Advisor.

* El icono **Buscar** muestra la lista de informes de análisis de rendimiento, que le permite elegir el informe correcto. Los campos del **paquete de Advisor** y del **servidor de destino** se rellenan con la información actual del paquete de Advisor y del servidor de destino. El intervalo de tiempo predeterminado se establece en una semana y la fecha de finalización se establece en hoy. Si hace clic en el botón **Buscar** de la esquina superior derecha, puede obtener una lista de todos los informes de análisis de rendimiento del servidor y el paquete de Advisor seleccionados en el intervalo de tiempo.

* El icono **ver gráficos** abre la vista gráfico de tendencias y de gráficos históricos.

En la ilustración siguiente se muestran los iconos de los **gráficos** **más recientes**, **Buscar**y ver después de cada paquete de Advisor:

![iconos de informe](../media/server-performance-advisor/spa-user-manual-report-icons.png)

### <a name="searching-for-and-within-reports"></a>Buscar y en los informes

La búsqueda de informes se realiza mediante el **Explorador de informes**. Esto le permite buscar informes por intervalo de fechas, nombre de servidor y paquete de Advisor. Esta es la manera recomendada de buscar un informe de interés distinto del informe de última ejecución. El informe de última ejecución está disponible a través del **Informe de vista** para ese servidor.

Al ver un informe específico, puede desplazarse fácilmente al informe siguiente y anterior por tiempo o examinar un informe relacionado, como un AP diferente que se ejecute al mismo tiempo. Estas opciones están disponibles en **acciones**.

También es posible buscar en un informe. Varios informes tienen un cuadro de búsqueda **Buscar** cadena disponible para la búsqueda rápida de cadenas de texto en el informe. Para quitar el cuadro de texto, puede descartarlo. Para activar un cuadro de búsqueda (en ventanas que tienen búsqueda de texto), puede usar el acceso directo control + F. El cuadro **Buscar** permite al usuario especificar una búsqueda que distingue entre mayúsculas y minúsculas según corresponda con la opción **Coincidir mayúsculas y minúsculas** .

En la siguiente ilustración se muestra el cuadro de búsqueda **Buscar** con la cadena **Power** en la pestaña **Informe** .

![cuadro Buscar](../media/server-performance-advisor/spa-user-manual-find-search-box.png)

### <a name="single-report"></a>Informe único

Un único informe muestra los resultados del análisis de rendimiento de una ejecución única de un paquete de Advisor en un único equipo. El informe muestra el nombre del paquete de Advisor, el nombre del servidor de destino, la hora a la que se generó el informe y la duración de la recopilación de datos.

Un único informe contiene una sección de notificación y las secciones de datos.

### <a name="notification-section"></a>Sección de notificación

La sección de notificación se compone de un conjunto de reglas de análisis de rendimiento. Cada notificación contiene un origen de datos, algunos valores de umbral y algunas lógicas de negocios. Al ejecutar el análisis de rendimiento, la lógica de negocios evalúa los orígenes de datos con respecto a los umbrales para determinar si la regla se supera o no. Si no es así, aparece una advertencia para informarle sobre un posible problema de rendimiento. También se proporcionan recomendaciones para ayudarle a resolver el problema. La sección de notificación siempre es la primera de la vista de informe.

La sección de notificación se divide en dos partes: **ADVERTENCIA** y **otras notificaciones**.

Si el origen de datos de una regla cumple ciertas condiciones según la configuración de la lógica y el umbral, aparece una advertencia en el área de **ADVERTENCIA** . Una advertencia incluye las siguientes partes:

* Un icono de advertencia indica la existencia de un problema potencial.

* Nombre de la regla. Por ejemplo, la **pérdida de paquetes de recepción de red** es un vínculo que apunta a la página de detalles de la regla, como se describe en administración de paquetes de [Advisor](#bkmk-manageadvisorpacks).

* Una descripción sencilla sobre el posible problema.

* Una recomendación para una posible solución al problema de rendimiento potencial.

Los distintos servidores pueden tener patrones de configuración y uso drásticamente diferentes, y es imposible establecer los umbrales y las reglas que se aplican a todos los servidores en todas las condiciones. SPA proporciona la capacidad de modificar los umbrales. También puede optar por deshabilitar una regla si la regla no se aplica a su escenario. De forma predeterminada, todas las reglas están habilitadas. Una regla deshabilitada no aparece en el área de notificación. Para obtener más información, consulte [Administración de paquetes de Advisor](#bkmk-manageadvisorpacks).

El **otro área de notificaciones** contiene todas las demás reglas, donde no se genera ninguna advertencia o la regla no es aplicable. Contiene partes similares que se encuentran en el área de **ADVERTENCIA** . La diferencia más importante es que si no se genera ninguna advertencia o la regla no es aplicable, normalmente no se proporciona ninguna recomendación.

### <a name="data-sections"></a>Secciones de datos

Las secciones de datos contienen los datos de rendimiento generados por el módulo de Advisor en función de los datos sin procesar recopilados de los servidores de destino. Las secciones de datos incluyen un conjunto de secciones de nivel superior y varios niveles de subsecciones. Las secciones de nivel superior se presentan como pestañas. Todas las subsecciones de las secciones de nivel superior se muestran en áreas expansibles. Puede contraer o expandir cada una de las secciones para centrarse en el área de interés, tal como se muestra en la ilustración siguiente.

![secciones de datos](../media/server-performance-advisor/spa-user-manual-data-sections.png)

El módulo Core SPA Advisor y el paquete de asesor de IIS SPA contienen una sección de **información general del sistema** . En esta sección se incluye la información de nivel superior sobre el uso y la configuración de recursos. Otras secciones de nivel superior representan áreas de datos de rendimiento. SPA presenta los datos del informe de las siguientes maneras:

* **Valor único** Par clave-valor. La clave es una cadena, que representa el significado del valor. El valor puede ser una cadena, un valor numérico o un valor booleano. Esto se suele usar para mostrar información estática, como la configuración, por ejemplo, la arquitectura de la CPU, el tamaño total de la memoria y la versión del BIOS, que no cambian con el tiempo.

* **valor de lista** A veces, se trata de un par clave-valor, pero el valor de la lista puede contener varios campos. Por ejemplo, el atributo de la CPU se puede mostrar en una tabla con varias columnas y varias filas. Cada fila representa una CPU y cada columna representa un atributo de la CPU.

* **Estadísticas** de Se puede considerar un tipo especial de un único valor. Solo puede contener datos numéricos. En el momento de la recopilación de datos, muchos de los puntos de datos numéricos fluctúan en lugar de permanecer constantes. Por ejemplo, el uso de CPU cambia cada vez que PLA recopila el contador de rendimiento. Mostrar un solo valor no puede reflejar con precisión la situación de rendimiento. En lugar de mostrar solo un valor, el valor promedio, máximo, mínimo y 90% se usa para esos puntos de datos numéricos dinámicos. El valor 90% representa la actividad en el percentil 90, o por encima de él, en todos los eventos de ese contador en ese intervalo de recopilación determinado.

* **Lista superior** Normalmente contiene los principales consumidores de un recurso específico o las entidades principales que experimentaron ciertos eventos. Por ejemplo, los **10 procesos principales en términos de uso promedio de CPU** incluyen los diez procesos principales con el uso promedio de CPU más alto durante el tiempo de recopilación de datos. Dado que el uso de CPU también es un punto de datos numérico dinámico, también se incluyen en la lista otras estadísticas como el valor máximo, mínimo y 90% para proporcionar al usuario una imagen más completa del consumo de la CPU.

Como se mencionó en las secciones anteriores, SPA se basa en PLA para recopilar el seguimiento de ETW, las consultas de WMI, los contadores de rendimiento, las claves del registro y los archivos de configuración para generar el informe. Es importante comprender el origen de datos detrás de cada punto de datos del informe. SPA proporciona información a través de la información sobre herramientas. Puede mantener el mouse sobre las columnas o filas de clave para ver la información sobre herramientas de origen de datos. Por ejemplo, **WMI: Win32 \_ disdrive: Caption** significa que el origen de datos es de una consulta de WMI, el nombre de la clase WMI es Win32 \_ DiskDrive y la propiedad es **Caption**.

### <a name="side-by-side-report"></a><a href="" id="side-by-side-report-"></a>Informe en paralelo

Los informes únicos proporcionan notificaciones y una sección de datos para ayudar al usuario a encontrar posibles problemas de rendimiento, pero a menudo resulta difícil identificar un posible problema de rendimiento mediante la consulta directa de un solo informe. Un único informe puede contener demasiados puntos de datos, lo que dificulta la búsqueda de los posibles problemas.

Para solucionar este problema, SPA proporciona la capacidad de comparar dos informes. Puede comparar un informe con un posible problema con un informe de línea de base para que le resulte más fácil encontrar las diferencias.

Los informes en paralelo se pueden iniciar desde un visor de un solo informe. Los usuarios pueden hacer clic en **acciones**y, a continuación, en **comparar informes** para seleccionar los informes. Solo es significativo comparar informes del mismo paquete de Advisor. Puede optar por comparar el informe con un informe anterior en el tiempo, el siguiente informe en el tiempo o un informe arbitrario seleccionado a través de las capacidades de búsqueda. Por ejemplo, para aislar un comportamiento anómalo, puede comparar un informe de servidor de línea de base con un informe generado en un momento diferente en el mismo equipo o en un informe generado en otro equipo que tenga un rol de servidor y una carga similares.

Un informe en paralelo tiene un aspecto muy similar al de un único informe. Contiene una sección de notificación y secciones de datos. Contiene el mismo número de notificaciones y secciones de datos que el visor de informes único. La única diferencia es que los informes se muestran de forma paralela. Cada sección contiene los datos del informe de origen (Informe 1) y el informe de destino (Informe 2). El informe en paralelo muestra el nombre del paquete de Advisor, el nombre del servidor de destino (Informe 1 a la izquierda y el informe 2 de la derecha), la hora a la que se generó el informe y la duración de la recopilación de datos para cada informe.

Si descarta el cuadro de diálogo **Buscar** , puede reactivarlo escribiendo control + F. Este cuadro de diálogo busca y resalta las cadenas de texto dentro de la sección actual.

En la sección notificación, si alguno de los resultados de los dos informes que se comparan es una advertencia, se muestra en el área de **ADVERTENCIA** . De lo contrario, los resultados se muestran en el área **otras notificaciones** . Dado que la clave de un informe en paralelo es identificar las diferencias entre los informes, no se muestra información detallada sobre una regla. Los usuarios pueden hacer clic en el nombre de la regla para abrir el formulario de detalles de la regla y obtener más información acerca de la regla.

En las secciones de datos, los datos se presentan en paralelo con los datos del informe 1 a la izquierda y los datos del informe 2 de la derecha. SPA muestra valores únicos en la misma tabla, pero en lugar de etiquetar el **valor**de las columnas, se denominan **Informe 1** y **Informe 2** , respectivamente. En el informe en paralelo se muestran todas las demás formas de datos en tablas en paralelo.

El visor de informes en paralelo también proporciona información sobre herramientas sobre el origen de datos.

### <a name="historical-and-trend-charts"></a>Gráficos históricos y de tendencia

Solo tiene sentido mostrar los gráficos de tendencias y históricos para un servidor específico y un paquete de asesor específico. Debe elegir el intervalo de tiempo (que está establecido de forma predeterminada en la última semana) y, a continuación, hacer clic en **Aceptar** para abrir el visor de tendencias y gráficos históricos.

El visor de tendencias y gráficos históricos tiene tres pestañas: gráfico histórico, gráfico de tendencias de 24 horas y gráfico de tendencias de 7 días.

### <a name="historical-chart"></a>Gráfico histórico

El gráfico histórico muestra una serie de valores para un punto de datos numérico en el intervalo de tiempo determinado. Por ejemplo, la **latencia media de solicitudes** de IIS en un solo servidor durante los últimos 15 días. Cada punto de datos de un gráfico histórico representa el valor de un origen de datos concreto tomado en una sesión de análisis de rendimiento.

Hay un par de formas de usar un gráfico histórico:

1.  Para buscar anomalías en un momento determinado para un punto de datos, por ejemplo, a las 2:00 AM cada día, la **latencia media de solicitudes** de IIS salta de 200 ms a 500 ms.

2.  Para ayudar a correlacionar varios puntos de datos. Por ejemplo, mostrando la **latencia media de solicitudes** y el **número promedio de solicitudes** para los últimos 15 días. El informe puede mostrar que la latencia de solicitud y el número de solicitudes aumentan al mismo tiempo, lo que podría indicar que el aumento de la latencia de la solicitud se debe a un aumento en el número de solicitudes.

En un gráfico histórico, los usuarios pueden hacer lo siguiente:

* Muestra varias series de datos en el área de gráfico. Cada serie de datos se muestra como un gráfico de líneas en el visor de informes. Cada gráfico de líneas se escala automáticamente para ajustarse al visor de informes.

* Agregue o quite una serie de datos de la lista de series de datos en la parte inferior del visor de gráficos históricos.

* Mostrar u ocultar una serie de datos en la lista de series de datos. Los usuarios pueden hacer clic en una serie de datos concreta de la lista para resaltar el gráfico de líneas correspondiente en el área del gráfico.

* Acerque un período de tiempo determinado seleccionando el período de tiempo dentro del área de gráfico. Para alejar, haga clic en el botón que se encuentra en la esquina inferior izquierda del gráfico.

* Investigue un solo informe haciendo doble clic en un punto de datos determinado.

* Copie los datos y haga que estén disponibles para otros programas, como Microsoft Excel. Esto le permite aprovechar las capacidades de gráficos de Microsoft Excel, cuando sea necesario.

### <a name="trend-charts"></a>Gráficos de tendencias

Muchos problemas de rendimiento repetitivos están causados por tareas periódicas que se ejecutan en o en los servidores de destino. Por ejemplo, un sitio web orientado al entretenimiento podría obtener más visitas durante el fin de semana, o una tarea programar copia de seguridad en disco podría reducir el rendimiento de un servidor todos los días a las 2:00 A.M.

Un gráfico de tendencias está diseñado para ayudarle a encontrar problemas de rendimiento. Los problemas de rendimiento pueden producirse repetidamente en varios patrones. Los patrones más comunes son patrones diarios y patrones semanales en los que se producen problemas de rendimiento durante la misma hora de un día o del mismo día de la semana. Por lo tanto, SPA proporciona un gráfico de tendencias de 24 horas y un gráfico de tendencias de 7 días.

El gráfico de análisis de tendencias proporciona un nivel de investigación más profundo en un conjunto de datos y busca tendencias en función de la hora del día. El eje X se establece en un período de 24 horas, a partir de 0:00 (medianoche) y termina en 23:59. SPA no muestra las tendencias de varias series de datos al mismo tiempo. Puede hacer clic en **elegir serie de datos** para seleccionar una serie de datos para ver.

Para procesar los datos, SPA busca todas las instantáneas tomadas entre 0:00 y 0:59 por cada hora. SPA determina los valores mínimo, máximo, promedio y sigma del conjunto de instantáneas tomadas durante esa hora y los grafica como gráficos de vela japonesa. SPA repite el proceso para las instantáneas tomadas entre 1:00 y 1:59, después 2:00 a 2:59, etc. Si no existe ninguna instantánea para la hora determinada, SPA deja en blanco esa hora en el gráfico y continúa hasta la hora siguiente.

Un gráfico de tendencias de 7 días es muy similar al gráfico de tendencias de 24 horas. La única diferencia es que agrupa una serie de datos según el día de una semana en lugar de la hora del día.

La serie de datos seleccionada en los gráficos de tendencias y históricos se almacena como una preferencia de usuario. La próxima vez que se abra el visor de tendencias y gráficos históricos para el mismo módulo de asesor, el mismo conjunto de series de datos se mostrará como valor predeterminado.

## <a name="managing-reports"></a>Administrar informes


### <a name="deleting-reports"></a>eliminar informes

Los informes se pueden quitar para minimizar el número de informes que se deben administrar mediante SPA. En función de la frecuencia de los informes y el número de servidores, se recomienda eliminar informes innecesarios. Aunque SPA no tiene un límite en los informes que puede administrar, la base de datos subyacente puede tener una limitación de tamaño.

**Nota:** los informes eliminados no se pueden recuperar.



### <a name="exporting-and-importing-reports"></a>Exportar e importar informes

Los informes se pueden exportar a un archivo XML para transportarlo a otra consola de SPA o enviar un correo electrónico a otro usuario. Al exportar el informe no se elimina el informe. Para exportar el informe que está viendo actualmente, en el **visor de informes**, haga clic en **acciones**y, a continuación, haga clic en **exportar**. Para exportar varios informes, en el **Explorador de informes**, haga clic en **Habilitar selección múltiple**, seleccione varios informes en el cuadro de selección y, a continuación, haga clic en **exportar**. Esto exporta los informes en formato XML al directorio de destino seleccionado.

Un informe exportado se puede ver en SPA. los informes importados no se agregan a la base de datos de SPA. Están pensados principalmente para servir como una aplicación de visor XML para el informe exportado. No es necesario que el servidor del informe importado tenga instalados los mismos paquetes de Advisor que la consola del informe de SPA original.

## <a name="managing-advisor-packs"></a><a href="" id="bkmk-manageadvisorpacks"></a>Administrar paquetes de asesores


SPA incluye paquetes de Advisor para el sistema operativo principal, Hyper-V, Active Directory e IIS. SPA proporciona una arquitectura abierta para desarrollar paquetes de Advisor mediante SQL, por lo que también es posible que los desarrolladores que no sean de Microsoft creen versiones de los paquetes de Advisor. Hay cuatro opciones para administrar un paquete de asesores: aprovisionar, personalizar, restablecer o quitar.

### <a name="provision-new-advisor-packs"></a>Aprovisionar nuevos paquetes de asesores

Microsoft o los desarrolladores que no son de Microsoft pueden publicar nuevos paquetes de asesores. Un paquete de Advisor incluye un provisionMetaData.xml y un conjunto de scripts SQL que describen la lógica.

**Para aprovisionar un nuevo paquete de Advisor**

1.  Copie todo el contenido del paquete de Advisor en el directorio *% SpaRoot%* \\ APS.

2.  En la ventana principal, haga clic en **configuración**y, a continuación, haga clic en **configurar paquetes de Advisor**. Se abre el cuadro de diálogo **configurar paquetes de Advisor** .

    **Nota:** Este cuadro de diálogo es similar a la página **aprovisionar el paquete de asesor** en el Asistente para usar por primera vez. Muestra una lista de los paquetes de asesores disponibles para administrar. Cada paquete de asesores de la lista tiene propiedades como el nombre, la versión instalada, la versión y el autor. Nombre es el nombre completo del paquete de Advisor y la versión instalada es la versión de este paquete de Advisor que ya se ha aprovisionado en el proyecto. Si el módulo de Advisor no se ha aprovisionado en la base de datos actual, el cuadro de texto versión instalada muestra **no instalado**. El campo versión indica la versión de este paquete de asesores, que se encuentra en la carpeta de paquetes de Advisor.



3.  Seleccione el paquete de Advisor en la lista. Si no se ha aprovisionado el paquete de Advisor o si hay una versión más reciente en la carpeta de paquetes de Advisor que la de la base de datos, se habilita el botón **aprovisionar** . Haga clic en el botón **aprovisionar** .

4.  Una vez completado el aprovisionamiento, el campo **versión instalada** del paquete de Advisor seleccionado contiene la nueva información de versión.

### <a name="customize-advisor-packs"></a>Personalización de paquetes de Advisor

SPA define una arquitectura abierta que permite a los usuarios modificar los paquetes de Advisor. Los usuarios pueden modificar los archivos del paquete de Advisor cambiando los umbrales, compartiendo los umbrales y habilitando o deshabilitando las reglas.

para obtener más información sobre cómo cambiar y compilar paquetes de Advisor, consulte la [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="changing-thresholds"></a>Cambiar umbrales

Los umbrales se usan en SPA para determinar si se cumple la condición desencadenadora de una regla. Los umbrales reales para escenarios reales de clientes pueden variar drásticamente debido a la carga de trabajo, el entorno de hardware y las necesidades empresariales. Es posible que los umbrales predeterminados no sean adecuados para el caso del usuario actual, por lo que SPA proporciona la capacidad de modificar el umbral existente.

Para modificar los valores de umbral, haga clic en el nombre de la regla en un informe único o en paralelo. O bien, para administrar todas las reglas de un módulo de asesor determinado, pueden modificar los valores de umbral desde el menú de **configuración** .

**Para cambiar un valor de umbral**

1.  En el menú **configuración** , haga clic en **configurar paquetes de Advisor**, haga clic en el nombre del paquete de Advisor que se va a modificar y, a continuación, haga clic en **configurar**.

    **Nota:** Se le mostrará una lista de todas las reglas incluidas en el paquete de Advisor. La casilla situada a la izquierda del nombre del paquete de Advisor indica si la regla está habilitada. Si una regla está deshabilitada, está oculta en todos los informes.



2.  Haga clic en la regla específica que desea modificar. Se abre el formulario de **detalles** de la regla seleccionada.

El formulario detalles de la regla contiene información detallada acerca de una regla específica. Incluye el nombre, la descripción, el estado, los posibles resultados y los umbrales. SPA admite dos tipos de resultados de reglas: **ADVERTENCIA** y **correcto**. Para cada tipo, hay texto de recomendaciones y una recomendación.

Algunas de las reglas no tienen umbrales definidos. Por ejemplo, la regla **http Keep Alive** comprueba un valor BOOLEANO para IIS. Por lo tanto, la lista de umbrales puede estar vacía. De lo contrario, se enumeran todos los umbrales que usa la regla actual. Como parte de la descripción, se incluye una descripción detallada de cómo se usa un umbral en la regla.

Si el formulario de detalles de la **regla** se inicia desde el menú de **configuración** , la lista de umbrales tiene tres columnas: nombre, configuración original y cambiar configuración. Si se inicia desde un informe único o en paralelo, también se incluyen los valores de umbral que se usan en el informe. Los usuarios pueden modificar los valores de umbral actuales cambiando el valor de la columna **Cambiar configuración** y haciendo clic en **Guardar** para guardar los cambios en la base de datos.

Todos los cambios que se realizan en los umbrales solo se aplican a los informes que se generan después de los cambios. Los informes existentes no se ven afectados por estos cambios.

### <a name="sharing-thresholds"></a>Umbrales de uso compartido

Si administra los servidores en situaciones similares, puede optar por usar el mismo conjunto de umbrales. Puede exportar e importar umbrales para un paquete de Advisor específico mediante el menú de **configuración** . Puede seleccionar el paquete de Advisor específico y, a continuación, hacer clic en **configurar**. El archivo de umbral exportado está en formato XML.

Al importar un umbral, SPA valida el formato de archivo XML y comprueba que el archivo coincide con el paquete de Advisor seleccionado. Si esto se realiza correctamente, SPA importa todos los valores del archivo de umbral en la base de datos del proyecto actual. Al igual que en el escenario de umbrales variables anteriores, todos los cambios de valor de umbral solo surten efecto en los informes que se generan en el futuro. Los informes existentes no se ven afectados.

### <a name="enable-or-disable-rules"></a>Habilitar o deshabilitar reglas

Una regla se puede habilitar o deshabilitar en el formulario de detalles de la **regla** . Debe hacer clic en **Guardar** para conservar los cambios realizados. Si una regla está deshabilitada, no se mostrará en ninguno de los informes. Pero la lógica de negocios subyacente se desencadena mientras se genera el informe, por lo que cuando se elige volver a habilitar la regla, se vuelve a mostrar en los informes.

### <a name="reset-advisor-packs-to-original-state"></a>restablecer el estado original de los paquetes de asesores

Podría decidir modificar un paquete de asesor aprovisionado en la base de datos. Aparte de cambiar los umbrales, también puede cambiar los scripts SQL. SPA no admite el cambio de metadatos de informe o la adición o eliminación de reglas para un paquete de Advisor aprovisionado. La modificación manual de esas áreas podría provocar un comportamiento inesperado.

para obtener más información sobre cómo cambiar un paquete de Advisor aprovisionado, consulte la [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Si desea revertir los cambios realizados en un paquete de Advisor aprovisionado, puede optar por restablecer el paquete de Advisor. Esto Sobrescribe todos los scripts SQL relacionados con el paquete de Advisor y restablece todos los valores de umbrales predeterminados. Esto mantiene todos los informes existentes.

el restablecimiento del paquete de Advisor puede realizarse mediante el formulario **configurar paquetes de Advisor** . Debe seleccionar el paquete de Advisor que se va a restablecer y, a continuación, hacer clic en **restablecer**.

### <a name="remove-advisor-packs"></a>quitar paquetes de Advisor

Cuando ya no se necesita un módulo de asesor, los usuarios pueden quitarlo de la base de datos. al quitar el paquete de Advisor, se quita todo el paquete de Advisor de la base de datos, incluidas las reglas y los umbrales, todos los scripts SQL y todos los informes. Ninguna de las acciones se puede revertir.

la eliminación del paquete de Advisor puede realizarse mediante el formulario **configurar paquetes de Advisor** . Debe seleccionar el paquete de Advisor que se va a quitar y, a continuación, hacer clic en **desaprovisionamiento**.

### <a name="update-existing-advisor-packs"></a>Actualización de los paquetes de Advisor existentes

Actualizar los paquetes de Advisor existentes es muy similar a restablecer el paquete de Advisor a su estado original. Para actualizar el paquete de Advisor a una versión más reciente, copie el nuevo paquete de Advisor en la carpeta del paquete de Advisor. Un paquete de asesores se considera una actualización de un paquete de Advisor existente solo cuando no hay ningún cambio en los metadatos del informe. El informe y los identificadores de reglas deben ser idénticos. De lo contrario, deben tratarse como dos paquetes de asesores diferentes.

Si solo hay cambios en la lógica de negocios y no hay cambios en los metadatos de informes para un paquete de Advisor, se debe asignar un nuevo número de versión, por ejemplo, de 1,0 a 2,0. Si hay cambios en los metadatos del informe, se debe asignar un nombre completo diferente al paquete de Advisor. Por ejemplo, Microsoft. ServerPerformanceAdvisor. IIS. v1 podría cambiarse a Microsoft. ServerPerformanceAdvisor. IIS. v2.

Si existe una versión más reciente del paquete de Advisor, la lista del formulario **configurar paquetes de asesores** rellena automáticamente la columna **versión** con la versión más reciente del paquete de Advisor. Puede seleccionar el paquete de Advisor y, a continuación, hacer clic en **restablecer**. El paquete de Advisor se actualiza con la nueva lógica de negocios y los umbrales. Se conservan todos los informes de este paquete de asesores.

## <a name="managing-servers"></a>Administración de servidores


SPA proporciona capacidades básicas para administrar servidores de destino. Puede optar por agregar nuevos servidores a la lista de servidores de destino, quitar servidores de la lista o modificar los comentarios de los servidores.

**Para agregar o quitar servidores o modificar la información del servidor**

1.  Haga clic en el menú **configuración** , haga clic en **configurar servidores**.

2.  En la lista de servidores que existen actualmente en la base de datos del proyecto, la última fila está vacía. Haga clic en la fila y rellene los campos. Los campos **nombre de servidor** y ubicación de **recurso compartido de archivos** son obligatorios y el nombre del servidor debe ser único.

3.  Escriba otra información del servidor en la columna de **Comentario** de cada servidor.

    **Nota:** Este campo usa un formato de texto libre, por lo que puede usarlo como un campo de descripción. También puede usar este campo para etiquetar los servidores de manera que se puedan encontrar fácilmente en la ventana principal, o para agrupar los servidores, por ejemplo, por ubicación o rol de servidor.



4.  Si desea usar SPA con un gran número de servidores, SPA admite un formato de valores separados por comas (. csv) para la importación. El archivo debe contener al menos dos campos: la **Ubicación del recurso compartido de archivos**y el **servidor** . El tercer campo, el **Comentario** es opcional, pero se recomienda organizar los servidores. También puede exportar la lista de servidores a un archivo. csv para determinar el formato adecuado o realizar una copia de seguridad de la configuración del servidor.

### <a name="searching-and-filtering"></a>Búsqueda y filtrado

Si administra más de unos pocos servidores, SPA proporciona compatibilidad básica para encontrar rápidamente los servidores en la ventana principal. Puede hacer clic en el encabezado de columna para ordenar según el nombre del servidor, los resultados del análisis, el estado actual de la tarea o los comentarios. También puede optar por usar la funcionalidad de búsqueda. En la esquina superior derecha de la ventana principal, puede escribir una cadena para buscar. La lista de **servidores de destino** en la ventana principal utiliza la cadena para filtrar los servidores y para mostrar solo los servidores con los campos nombre o comentario que contiene la cadena de búsqueda.

En la siguiente ilustración se muestra cómo la cadena **Dell** coincide con los servidores con el campo de la cadena **Dell** o servidores con **Comentario** que contiene **Dell**.

![ejemplo de búsqueda y filtrado](../media/server-performance-advisor/spa-user-manual-find-search-example.png)

## <a name="advanced-functionality"></a>Funcionalidad avanzada


### <a name="working-with-spa-windows-powershell-cmdlets"></a>Trabajar con cmdlets de Windows PowerShell para SPA

La consola de SPA es compatible a través de la interfaz de usuario para la recopilación de datos periódica. Si esa funcionalidad no es suficiente para su entorno, hay cmdlets de Windows PowerShell que un administrador avanzado puede usar para personalizar la recopilación de datos. Estos cmdlets de Windows PowerShell permiten a los administradores del sistema ejecutar automáticamente el análisis de rendimiento en los servidores de destino y usar SPA para la asistencia al cliente remoto. Por ejemplo, los administradores del sistema pueden escribir scripts para invocar cmdlets SPA en determinados intervalos de tiempo para muestrear periódicamente la condición de rendimiento de los servidores de destino.

Se recomienda cerrar la aplicación SPAConsole.exe antes de usar estos cmdlets.

Antes de ejecutar cualquier cmdlet de Windows PowerShell, debe registrar los cmdlets en el equipo de la consola.

**Para registrar los cmdlets de Windows PowerShell**

1.  En un símbolo del sistema de Windows PowerShell con privilegios elevados, escriba **registerSpaCmdlets. cmd**. Aparece el mensaje **registrar los cmdlets de spa correctamente** .

2.  Ejecute **Spa-PowerShell. cmd**. Si pasa la ruta de acceso a un archivo de script de Windows PowerShell, ejecutará los scripts automáticamente. De lo contrario, abre un símbolo del sistema de Windows PowerShell, que está listo para ejecutar cmdlets SPA de Windows PowerShell.

En la tabla siguiente se describen los cmdlets de Windows PowerShell para SPA:

| Nombre del cmdlet | Parámetros | Descripción |
| ------ | ------- | ------ |
| Start-SpaAnalysis | **-ServerName** Nombre del servidor de destino.<br>**-AdvisorPackName** Nombre completo del paquete de Advisor que se va a poner en cola en el servidor. Cuando hay más de un paquete programado para ejecutarse al mismo tiempo, el valor del parámetro debe tener el formato AP1name, AP2name.<br>**-Duración** Duración de la recopilación de datos.<br>**-Credential** Credenciales de usuario para la cuenta que ejecuta la recopilación de datos en el servidor de destino.<br>**-SqlInstanceName** Nombre de la instancia de SQL Server.<br>**-SqlDatabaseName** Nombre de la base de datos del proyecto SPA. | Inicia una sesión de recopilación de datos SPA en el servidor especificado. |
| Stop-SpaAnalysis | **-SqlInstanceName** Nombre de la instancia de SQL Server.<br>**-SqlDatabaseName** Nombre de la base de datos del proyecto SPA.<br>**-ServerName** Nombre del servidor de destino. | Intenta detener una sesión SPA en ejecución. Si ya se ha completado una sesión, devuelve sin hacer nada. |
| Get-SpaServer | **-SqlInstanceName** Nombre de la instancia de SQL Server.<br>**-SqlDatabaseName** Nombre de la base de datos del proyecto SPA. | Obtiene la lista de servidores en la base de datos. Devuelve una lista de objetos, incluidas estas propiedades: nombre, estado, FileShare y comentario. |
| Get-SpaAdvisorPacks | **-SqlInstanceName** Nombre de la instancia de SQL Server<br>**-SqlDatabaseName** Nombre de la base de datos del proyecto SPA | Obtiene la lista de módulos de Advisor en la base de datos. Devuelve una lista de objetos, incluidas estas propiedades: Name, DisplayName, Author y version. |

Windows PowerShell proporciona la capacidad de pasar credenciales a través de archivos cifrados para habilitar escenarios de automatización. Para obtener más información sobre el uso de archivos cifrados para pasar credenciales a un cmdlet, consulte [crear scripts de Windows PowerShell que acepten credenciales](/previous-versions/technet-magazine/ff714574(v=msdn.10)).

### <a name="automating-spa-report-collection-by-using-windows-powershell"></a>Automatizar la recopilación de informes de SPA mediante Windows PowerShell

Los procedimientos siguientes representan un ejemplo de cómo automatizar una colección de informes SPA mediante los cmdlets de Windows PowerShell de SPA.

Las credenciales de usuario se pueden cifrar y almacenar en caché a través de Windows PowerShell. Estas credenciales se usan para iniciar sesión en servidores remotos. Aunque no es específico de SPA, en el ejemplo siguiente se muestra esta técnica:

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

Para poder ejecutar este archivo, debe ejecutar **Set-ExecutionPolicy remoteSigned** .

Puede ejecutarlo desde un archivo por lotes con el siguiente comando:

``` syntax
PowerShell -command "& '.\RunSpa.ps1' "
```

### <a name="use-t-sql-to-generate-reports"></a>Usar T-SQL para generar informes

SPA los datos de informe se pueden extraer mediante SQL para crear informes personalizados que no se proporcionan en el SPAConsole.exe.

por ejemplo, el siguiente comando de T-SQL proporciona una lista de servidores de 10 principales por CPU para el período de tiempo que abarca los últimos tres días:

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

En algunos casos, puede que desee crear particiones de las bases de datos de SQL Server que se usan en SPA. Esto puede ayudar a reducir el tamaño de los datos de la SQL Server base de datos o a crear particiones lógicas de los servidores. En estos casos, la consola de SPA admite varios proyectos. Puede crear una nueva base de datos de proyecto haciendo clic en **archivo**y, a continuación, en **nuevo proyecto**. Aparece el Asistente para usar la primera vez que ayuda a crear la nueva base de datos del proyecto.

Una vez creados los proyectos, puede hacer clic en **archivo**y, a continuación, en **Abrir proyecto** para cambiar entre proyectos. La consola de SPA no permite el cambio de bases de datos cuando se ejecuta un análisis de rendimiento pendiente en el proyecto actualmente abierto. Esto es para proteger la integridad de los datos de rendimiento.

Cuando elija crear una nueva base de datos de proyecto o abrir una base de datos de proyecto diferente, se cerrará el proyecto actual. Todos los informes abiertos se cierran cuando se cierra el proyecto actual.

**Nota:** SQL Server 2008 R2 Express tiene un límite de base de datos de 10 GB. Mediante el uso de varios proyectos, puede usar una o varias bases de datos de SQL Server y permanecer en el límite de 10 GB SQL Server 2008 R2 Express.



### <a name="logging-and-debugging"></a>Registro y depuración

SPA proporciona funcionalidad básica de registro. Solo permite escribir registros en un archivo de registro, que se encuentra en la misma carpeta que SPAConsole.exe. La cuenta de usuario que ejecuta la consola de SPA debe tener permiso de escritura en la carpeta en la que se instaló SPA para asegurarse de que los registros se pueden escribir en el archivo de registro.

SPA contiene los siguientes niveles de registro válidos:

* **Información** Vuelca registros para cada acción que realiza la consola de SPA y está diseñado principalmente para fines de depuración.

* **ADVERTENCIA** de Registra todos los errores y excepciones que se producen dentro de la consola de SPA. Algunos de los errores son simplemente errores de validación que se pueden administrar mediante la consola de SPA.

* **Nivel crítico** Registra solo errores y excepciones que no se pueden administrar mediante la consola de SPA. Estos errores hacen que la consola de SPA se bloquee. Los registros proporcionan la información de contexto para dichos errores.

De forma predeterminada, el nivel de registro es advertencia, lo que significa que SPA solo registra errores y excepciones que se producen en SPA. El nivel de registro se puede cambiar editando el archivo de **SpaConsole.exe.config** en la misma carpeta en la que se encuentra SpaConsole.exe. Todos los registros se escriben en log.txt archivo en la misma carpeta.

SPA también proporciona cierta funcionalidad básica para la depuración. Para activar la depuración de SPA, los usuarios deben modificar manualmente la base de datos del proyecto SPA. La configuración se almacena en una tabla de configuraciones. El usuario debe ejecutar el siguiente script de SQL para cambiar el proyecto SPA al modo de depuración:

``` syntax
UPdate [Configurations] SET Value = N'true' WHERE Name = N'Debugmode'.
```

En el modo de depuración, el marco de SPA conserva todos los datos temporales que se generan durante el análisis de rendimiento para que pueda solucionar problemas en los paquetes de Advisor. Este script está diseñado principalmente para que lo usen los desarrolladores del módulo de Advisor.

para obtener más información acerca de las funcionalidades de depuración en SPA, consulte la [Guía de desarrollo de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="managing-the-database"></a>Administrar la base de datos

### <a name="backup-and-restore-the-database"></a>Copia de seguridad y restauración de la base de datos

La consola de SPA no proporciona funcionalidad para realizar copias de seguridad y restaurar la base de datos de SPA. Debe usar scripts y herramientas de Microsoft SQL Server estándar para realizar copias de seguridad y restaurar bases de datos.

### <a name="clean-up-the-database"></a>limpiar la base de datos

Las bases de datos del proyecto SPA pueden aumentar de tamaño a medida que se ejecuta más análisis de rendimiento. De forma predeterminada, SPA mantiene todos los informes. Puede que desee quitar informes antiguos para ayudar a mejorar el rendimiento. Puede eliminar informes a través de la interfaz del **Explorador de informes** .

## <a name="privacy-and-security"></a>Privacidad y seguridad


SPA solo admite la recopilación de datos de los servidores de destino en la consola de SPA. No está diseñado para enviar información a desarrolladores de Microsoft o que no sean de Microsoft. Para obtener más información acerca de la privacidad de SPA, consulte los términos de licencia del software de Microsoft para el asesor de rendimiento del servidor.

Todos los datos recopilados por SPA se almacenan en las bases de datos del proyecto. Debido a la naturaleza de algunos de los seguimientos de ETW, SPA podría recopilar información confidencial que podría tener una importancia empresarial alta. Debe tener en cuenta el riesgo potencial asociado al uso compartido del acceso a las bases de datos del proyecto de SPA. Los archivos de registro temporales se guardan en las carpetas compartidas que se especifican en cada equipo de destino. Aunque SPA intenta eliminar esos archivos de registro temporales cuando se completa la importación de datos, no hay ninguna garantía de que los archivos de registro se eliminen siempre. Debe asegurarse de que las carpetas compartidas se encuentran en ubicaciones seguras.

SPA Advisor pack contiene scripts SQL para analizar y analizar registros de rendimiento para generar informes de rendimiento. SPA intenta limitar el privilegio en el que se ejecutan los scripts. Sin embargo, todavía existe la posibilidad de que los scripts puedan recopilar información confidencial a través de SPA desde los servidores de destino, u obtener o modificar la información confidencial almacenada en la misma base de datos de proyecto de SPA. Debe asegurarse de que todos los paquetes de Advisor aprovisionados en la base de datos de proyecto de SPA provienen de orígenes de confianza.

## <a name="errors-and-troubleshooting"></a>Errores y solución de problemas


### <a name="spaconsoleexe-does-not-start-or-write-log-file"></a>SPAConsole.exe no inicia ni escribe el archivo de registro

Al intentar ejecutar SPAConsole.exe por primera vez, si la .NET Framework no está instalada, la aplicación no inicia ni escribe un archivo de registro. Asegúrese de que el marco de trabajo de compatible.NET esté instalado y funcione correctamente antes de iniciar SPA.

### <a name="locating-log-information"></a>Buscar información de registro

SPA almacena la información de errores en el archivo de log.txt en la carpeta SPA. los mensajes de error detallados y la información de la pila de llamadas se escriben en esta carpeta. Si experimenta errores que necesitan más información para interpretar, puede abrir log.txt archivo para ver los detalles del error.

### <a name="database-size-limitations-for-sql-server-express"></a>Limitaciones de tamaño de base de datos para SQL Server Express

SQL Server Express tiene un límite de tamaño de 10 GB para una base de datos de usuario. Esta base de datos de tamaño tiene la capacidad de almacenar 20.000-30000 informes. Para reducir la posibilidad de alcanzar este límite de base de datos, se recomienda eliminar informes con regularidad o con otra versión de SQL Server que no tenga la limitación de base de datos de 10 GB.

### <a name="sql-server-express-log-size-and-disk-capacity"></a>Tamaño del registro de SQL Server Express y capacidad de disco

Si usa SQL Server Express, la base de datos de usuario está limitada a 10 GB, pero el archivo de registro correspondiente puede superar los 70 GB. Por estas razones, se recomienda 100 GB o más de espacio libre en disco para SQL Server Express. Este espacio en disco debe ser suficiente para almacenar aproximadamente de 20.000 a 30.000 informes. Este archivo de registro se denomina SPADB \_ log. ldf y se encuentra en **% archivos de programa% \\ Microsoft SQL Server \\ MSSQL10. \\ \\ Datos de SQLEXPRESS MSSQL**.

### <a name="failure-to-connect-to-target-server"></a>No se pudo conectar al servidor de destino

Al ejecutar el análisis de rendimiento en los servidores de destino, la cuenta de usuario que ejecuta la consola de SPA debe tener determinados privilegios para poner en cola el conjunto de recopiladores de datos en PLA en los servidores de destino. También es necesario cambiar la configuración del firewall de Windows para permitir el paso de las comunicaciones de PLA. Para obtener instrucciones de configuración detalladas, consulte [configuración de spa](#bkmk-setupspa).

### <a name="failure-to-create-pla-data-collection-set-on-the-target-server"></a>No se pudo crear el conjunto de recopilación de datos PLA en el servidor de destino

Si obtiene el mensaje no se puede crear el conjunto de recopilación de datos PLA en el servidor de destino, asegúrese de hacer lo siguiente:

* Asegúrese de que los **registros de rendimiento &** servicio de alertas se está ejecutando

* La configuración **de seguridad acceso de red: no permitir el almacenamiento de contraseñas y credenciales para la autenticación de red** está deshabilitada. La configuración de seguridad debe estar deshabilitada porque SPA debe usar las credenciales de usuario para crear el conjunto de recopilación de datos en el servidor de destino.

### <a name="running-spa-against-the-console"></a><a href="" id="running-spa-against-the-console-"></a>Ejecución de SPA en la consola

PLA pasa a través de un canal diferente si el servidor de destino es el mismo que el de la consola de SPA. Incluso si la cuenta de usuario ejecuta la consola de SPA con privilegios de administrador, se produce un error en PLA. Si la consola de SPA está instalada en un servidor de destino, los usuarios deben iniciar SPA como administrador para asegurarse de que la tarea de análisis de rendimiento se puede ejecutar en la consola.

### <a name="running-multiple-consoles-at-the-same-time"></a>Ejecución de varias consolas al mismo tiempo

SPA no admite varias consolas que se ejecutan en la misma base de datos de proyecto SPA al mismo tiempo. SPA tampoco proporciona el mecanismo de bloqueo y sincronización para evitar que se produzca. Si se ejecutan dos consolas de SPA al mismo tiempo, la consola se comportará de forma incoherente en función de la secuencia de tiempo en la que se ejecuten estas consolas de SPA. Para evitarlo, todas las sesiones de análisis de rendimiento en cola deben quitarse de la lista antes de ser procesadas por la consola de que inicia el análisis.

SPA protege la integridad de cada informe generado correctamente por SPA. al mismo tiempo, SPA no garantiza que se completen todas las tareas de análisis en cola. Si ve cambios de estado incoherentes para las sesiones de análisis de rendimiento o los errores que impidan que el sistema no encuentre registros de rendimiento generados por el conjunto de recopiladores de datos, es probable que se deba a que varias instancias de la consola de SPA se ejecutan en la misma base de datos de proyecto SPA.

La ejecución de cmdlets SPA de Windows PowerShell también podría verse afectada por una consola de SPA que se ejecuta en la misma base de datos de SPA. Se recomienda cerrar la consola de SPA antes de ejecutar los cmdlets de Windows PowerShell de SPA.

### <a name="spaconsoleexe-recurring-collection-is-disrupted"></a>SPAConsole.exe colección periódica se interrumpe

Al ejecutar el SPAConsole.exe y usar una recopilación de datos periódica (por ejemplo, una colección por hora), el servidor que ejecuta el SPAConsole.exe no debe estar en modo de ahorro de energía para que se pueda suspender. SPA no comprueba una directiva de ahorro de energía. Esta actividad suspendida puede afectar a la recopilación de datos periódica periódica.

### <a name="lost-etw-events"></a>Eventos ETW perdidos

Para crear un impacto mínimo en el rendimiento de los servidores de destino, PLA está diseñado para ejecutarse con prioridad baja mientras recopila información de rendimiento. Si el servidor de destino está ocupado, PLA puede quitar algunas de las tareas de recopilación de datos para proporcionar a las tareas de alta prioridad que se ejecutan en los servidores de destino. Debe considerar la posibilidad de configurar la carpeta compartida en la que los eventos se escriben en un disco que no entra en conflicto con la e/s de la carga de trabajo o una unidad más rápida, como una SSD. Cuando se quitan los eventos, se notifican en la vista de informe, ya que los eventos perdidos pueden afectar a la confiabilidad de las métricas de rendimiento generadas.

Algunos problemas de permisos también pueden provocar que se omitan las consultas del registro o WMI. Sin embargo, es mucho menos probable que se produzca la pérdida de eventos de ETW. Como resultado, los resultados del conjunto de recopiladores de datos a veces no contienen todos los valores solicitados. Debe asegurarse de que la situación la controlan los scripts de T-SQL para todos los paquetes de Advisor. Si los datos no existen en el resultado de la recopilación de datos, se marcan como no datos en los informes.

Dado que la pérdida de eventos de ETW es común para PLA, los puntos de datos que se generan en función de un seguimiento de ETW podrían no ser coherentes con los puntos de datos que se generan basándose en, por ejemplo, los contadores de rendimiento. Por ejemplo, es posible ver que el uso total de la CPU por parte de IIS es 80% (que proviene de los contadores de rendimiento) y que las direcciones URL principales solo usan el 10% de todo el tiempo de CPU (que es un punto de datos que procede del seguimiento de ETW). Normalmente, el origen de datos de un punto de datos se puede ver a través de la información sobre herramientas del punto de datos. Debe tener en cuenta el impacto de la pérdida de datos.

Para evitar este tipo de pérdida de eventos, la carpeta de resultados debe cerrarse en el servidor de destino.

Si los resultados del recopilador de datos contienen datos incompletos distintos de la pérdida del seguimiento de ETW y el desarrollador del paquete de Advisor hubiera agregado compatibilidad con la notificación de pérdida de eventos de ETW, se muestra una barra de información en la parte superior del único informe para notificar al usuario sobre el posible Informe incoherente causado por la pérdida de datos. puede encontrar información detallada sobre la pérdida de datos en el archivo de log.txt.

## <a name="glossary"></a>Glosario


Estos son algunos de los términos que se usan con SPA:

* **Paquete de Advisor** Colección de metadatos y scripts de T-SQL que procesan los registros de rendimiento que se recopilan desde el servidor de destino. Después, el paquete de Advisor genera informes a partir de los datos del registro de rendimiento. Los metadatos del paquete de Advisor definen los datos que se van a recopilar del servidor de destino para las mediciones de rendimiento. Los metadatos también definen el conjunto de reglas, los umbrales y el formato del informe. A menudo, un paquete de Advisor se escribe específicamente para un rol de servidor único, por ejemplo, Internet Information Services (IIS).

* La **consola de spa** SpaConsole.exe, que es la parte central de spa. SPA no necesita ejecutarse en el servidor de destino que está probando. La consola de SPA contiene todas las interfaces de usuario para SPA, desde la configuración del proyecto hasta la ejecución de análisis y la visualización de informes. Por diseño, SPA es una aplicación de dos niveles. La consola de SPA contiene la capa de la interfaz de usuario y parte de la capa de lógica empresarial. La consola de SPA programa y procesa las solicitudes de análisis de rendimiento.

* **Plataforma Spa** Proporciona todas las interfaces de usuario, el procesamiento del registro de rendimiento, la configuración, el control de errores y las API de base de datos, y los procedimientos de administración.

* **Proyecto Spa** Base de datos que contiene toda la información sobre los servidores de destino, los paquetes de asesores y los informes de análisis de rendimiento que se generan en los servidores de destino para los paquetes de Advisor. Puede comparar y ver el historial y los gráficos de tendencias en el mismo proyecto de SPA. Puede crear más de un proyecto. Los proyectos de SPA son independientes entre sí y no hay ningún dato compartido entre los proyectos.

* **Servidor de destino** El equipo físico o la máquina virtual que ejecuta Windows Server con determinados roles de servidor, como IIS.

* **Sesión de análisis de datos** Un análisis de rendimiento en un servidor de destino específico. Una sesión de análisis de datos puede incluir varios paquetes de Advisor. Los conjuntos de recopiladores de datos de esos paquetes de asesores se combinan en un único conjunto de recopiladores de datos. Todos los registros de rendimiento de una sola sesión de análisis de datos se recopilan durante el mismo período de tiempo. Analizar los informes generados por los paquetes de Advisor que se ejecutan en la misma sesión de análisis de datos puede ayudar a los usuarios a comprender la situación de rendimiento general e identificar las causas principales de los problemas de rendimiento.

* **Seguimiento de eventos para Windows** Sistema de seguimiento escalable y de alto rendimiento que se proporciona en Windows. Proporciona capacidades de generación de perfiles y depuración, que se pueden utilizar para solucionar diversos escenarios. SPA usa eventos ETW como origen de datos para generar los informes de rendimiento. Para obtener información general sobre ETW, vea [mejorar la depuración y el ajuste del rendimiento con ETW](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw).

* **Instrumental de administración de Windows (WMI)** La infraestructura de datos y operaciones de administración de Windows. Puede escribir scripts o aplicaciones de WMI para automatizar las tareas administrativas en equipos remotos. WMI también proporciona datos de administración a otras partes del sistema operativo y a los productos. SPA usa información de clase WMI y puntos de datos como orígenes para generar informes de rendimiento.

* **Contadores de rendimiento** Se usa para proporcionar información sobre el rendimiento del sistema operativo o de una aplicación, servicio o controlador. Los datos del contador de rendimiento pueden ayudar a determinar los cuellos de botella del sistema y ajustar el rendimiento del sistema y de las aplicaciones. El sistema operativo, la red y los dispositivos proporcionan datos de contador que una aplicación puede usar para proporcionar a los usuarios una vista gráfica de cómo funciona el sistema. SPA usa información de contadores de rendimiento y puntos de datos como orígenes para generar informes de rendimiento.

* **Registros y alertas de rendimiento (PLA)** Recopila registros y seguimientos de rendimiento y genera alertas de rendimiento cuando se cumplen determinados desencadenadores. PLA se puede usar para recopilar contadores de rendimiento, seguimiento de eventos para Windows (ETW), consultas WMI, claves del registro y archivos de configuración. PLA también admite la recopilación remota de datos a través de llamadas a procedimientos remotos (RPC). El usuario define un conjunto de recopiladores de datos, que incluye información sobre los datos que se van a recopilar, la frecuencia de recopilación de datos, la duración de la recopilación de datos, los filtros y una ubicación para guardar los archivos de resultados. SPA usa PLA para recopilar todos los datos de rendimiento de los servidores de destino.

* **Informe único** Un informe de SPA que se genera en función de una sesión de análisis de datos para un paquete de Advisor en un solo servidor de destino. Puede contener notificaciones y varias secciones de datos.

* **Informe en paralelo** Un informe de SPA que compara dos informes únicos para el mismo paquete de Advisor. Los dos informes pueden generarse desde distintos servidores de destino o desde diferentes ejecuciones de análisis de rendimiento en el mismo servidor de destino. El informe en paralelo crea la capacidad de comparar dos informes para ayudar a los usuarios a identificar comportamientos anómalos o valores de configuración en uno de los informes. Un informe en paralelo contiene notificaciones y varias secciones de datos. En cada sección, los datos de ambos informes se enumeran en paralelo.

* **Gráfico de tendencias** Un informe de SPA que se usa para investigar patrones repetitivos de problemas de rendimiento. Muchos problemas de rendimiento repetitivos están causados por cambios de carga del servidor programados desde el servidor o desde equipos cliente, que pueden producirse diaria o semanalmente. SPA proporciona un gráfico de tendencias de 24 horas y un gráfico de tendencias de 7 días para identificar estos problemas.

    El usuario puede elegir una o varias series de datos a la vez, que es un valor numérico dentro del único informe, como el **uso total de CPU promedio**. más concretamente, un valor numérico es un valor escalar de un único servidor generado por un único punto de actividad en una instancia de tiempo determinada. SPA agrupa esos valores en 24 grupos, uno por cada hora del día (siete para un informe de 7 días, uno para cada día de la semana). SPA calcula el promedio, el mínimo, el máximo y las desviaciones estándar para cada grupo.

* **Gráfico histórico** Un informe de SPA que se usa para mostrar los cambios en determinados valores numéricos dentro de informes únicos para un par de paquete de asesor y servidor determinado a lo largo del tiempo. El usuario puede elegir varias series de datos y mostrarlas juntas en el gráfico histórico para comprender la correlación entre diferentes series de datos.

* **Serie de datos** Datos numéricos que se recopilan del mismo origen de datos durante un período de tiempo. El mismo origen significa que los datos tienen que provienen del mismo servidor de destino, como la longitud media de la cola de solicitudes para IIS en un servidor.

* **Reglas** de Combinaciones de lógica, umbrales y descripciones. Representan un posible problema de rendimiento. Cada paquete de Advisor contiene varias reglas. Cada regla se desencadena mediante un proceso de generación de informes. Una regla aplica la lógica y los umbrales a los datos en un único informe. Si se cumplen los criterios, se genera una notificación de advertencia. Si no es así, la notificación se establece en el estado **correcto** . Si no se aplica la regla, la notificación se establece en el estado no aplicable (**na**).

* **Notificaciones** Información que una regla muestra a los usuarios. Incluye el estado de la regla (**correcto**, **na**o **ADVERTENCIA**), el nombre de la regla y las posibles recomendaciones para solucionar los problemas de rendimiento.