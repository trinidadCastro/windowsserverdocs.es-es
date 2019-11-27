---
title: Planear la implementación de WSUS
description: 'Tema de Windows Server Update Service (WSUS): información general sobre el proceso de planeamiento de la implementación con vínculos a los temas relacionados'
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 35865398-b011-447a-b781-1c52bc0c9e3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/24/2018
ms.openlocfilehash: 37e3a7788ccd409f4002f5fe2d7ea087e89b3419
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369840"
---
# <a name="plan-your-wsus-deployment"></a>Planear la implementación de WSUS

>Se aplica a: Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El primer paso de la implementación de Windows Server Update Services (WSUS) es tomar decisiones importantes, tales como determinar el escenario de implementación de WSUS, elegir una topología de red y comprender los requisitos del sistema. La siguiente lista de comprobación resume los pasos para preparar la implementación de WSUS.

|Tarea|Descripción|
|----|--------|
|[1.1. Revisar consideraciones iniciales y requisitos del sistema](#11-review-considerations-and-system-requirements)|Revise la lista de consideraciones y requisitos del sistema para asegurarse de tener todo el hardware y software necesario para implementar WSUS.|
|[1.2. Elegir el escenario de implementación de WSUS](#12-choose-a-wsus-deployment-scenario)|Decida el escenario de implementación de WSUS que se va a usar.|
|[1.3. Elegir la estrategia de almacenamiento de WSUS](#13-choose-a-wsus-storage-strategy)|Decida cuál es la estrategia de almacenamiento de WSUS que mejor se adapta a su implementación.|
|[1.4. Elegir idiomas de actualización de WSUS](#14-choose-wsus-update-languages)|Decida los idiomas de actualización de WSUS que se van a instalar.|
|[1.5. Planear grupos de equipos WSUS](#15-plan-wsus-computer-groups)|Planee el enfoque de grupos de equipos WSUS que usará en la implementación.|
|[1.6. Planear consideraciones sobre el rendimiento de WSUS: servicio de transferencia inteligente en segundo plano](#16-plan-wsus-performance-considerations)|Planee un diseño de WSUS para su mejor rendimiento.|
|[1.7. Planear la configuración de actualizaciones automáticas](#17-plan-automatic-updates-settings)|Planee la manera en que configurará las actualizaciones automáticas en su escenario.|

## <a name="11-review-considerations-and-system-requirements"></a>1.1. Revisar consideraciones iniciales y requisitos del sistema

### <a name="system-requirements"></a>Requisitos del sistema

Antes de habilitar el rol de servidor de WSUS, compruebe que el servidor cumpla con los requisitos del sistema y que usted tenga los permisos necesarios para completar la instalación. Para ello, siga estas instrucciones:

-   Los requisitos de hardware del servidor para habilitar el rol de WSUS están enlazados a los requisitos del hardware. Los requisitos mínimos de hardware para WSUS son:

    -   **Procesador:** procesador x64 de 1,4 gigahercios (GHz) (se recomienda de 2 Ghz o más rápido)

    -   **Memoria:** WSUS requiere 2 GB adicionales de RAM más de lo que requiere el servidor y los demás servicios o software.

    -   **Espacio en disco disponible:** 10 GB (40 GB o más recomendado)

    -   **Adaptador de red:** 100 megabits por segundo (Mbps) o superior

-   Requisitos de software:

    -   Para ver informes, WSUS necesita [Microsoft Report Viewer Redistributable 2008](https://www.microsoft.com/download/details.aspx?id=6576). En Windows Server 2016, WSUS requiere [Microsoft Report Viewer Runtime 2012](https://www.microsoft.com/download/details.aspx?id=35747)

-   Si instala roles o actualizaciones de software que al finalizar requieren el reinicio del servidor, reinícielo antes de habilitar el rol de servidor WSUS.

-   Se debe instalar Microsoft .NET Framework 4.0 en el servidor que tendrá instalado el rol de servidor de WSUS.

-   Para que el complemento de administración de WSUS se muestre correctamente, la cuenta NT Authority o de servicio de red debe tener permisos de control total para las siguientes carpetas:

    -   %windir%\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files

        > [!NOTE]
        > Esta ruta de acceso podría no existir antes de instalar el rol del servidor web que contiene Internet Information Services (IIS).

    -   %windir%\Temp

-   Confirme que la cuenta que tiene previsto usar para instalar WSUS es miembro del grupo local Administradores.

### <a name="installation-considerations"></a>Consideraciones para la instalación

Durante el proceso de instalación, WSUS instalará lo siguiente de manera predeterminada:

-   API de .NET y cmdlets de Windows PowerShell

-   Windows Internal Database (WID), que usa WSUS

-   Servicios que usa WSUS:

    -   Servicio de actualización

    -   Servicio web de informes

    -   Servicio web de cliente

    -   Servicio web de autenticación simple

    -   Servicio de sincronización de servidores

    -   Servicio web de autenticación DSS

### <a name="features-on-demand-considerations"></a>Consideraciones de las características a petición

Tenga en cuenta que la configuración de los equipos cliente (incluidos los servidores) para actualizar mediante WSUS provocará las siguientes limitaciones:

1. Los roles de servidor a los que se ha quitado sus cargas mediante características a petición no se pueden instalar a petición desde Microsoft Update. Debes facilitar un origen de instalación en el momento en que se intentan instalar estos roles de servidor o configurar un origen para las características a petición en la directiva de grupo.

2. Las ediciones cliente de Windows no podrán instalar .NET 3.5 a petición desde la web. Las mismas consideraciones de los roles de servidor se aplican a .NET 3.5.

   > [!NOTE]
   > WSUS no está relacionado con la configuración de un origen de instalación de características a petición. Para obtener información sobre cómo configurar las características, consulte [Configuración de características a petición en Windows Server](https://technet.microsoft.com/library/jj127275.aspx).

3. Los dispositivos empresariales que ejecutan Windows 10, versión 1709 o 1803, no pueden instalar características a petición directamente desde WSUS. Para instalar características a petición, [crea un archivo de características (almacén en paralelo)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275%28v=ws.11%29#create-a-feature-file-or-side-by-side-store) u obtén el paquete de características a petición de uno de los orígenes siguientes:
   - [Centro de servicio de licencias por volumen](https://www.microsoft.com/licensing/servicecenter) (VLSC): se necesita acceso a VL
   - Portal de OEM: se necesita acceso a OEM
   - Descarga de MSDN: se requiere una suscripción a MSDN

     Los paquetes de Función a petición obtenidos de forma individual se pueden instalar mediante [opciones de línea de comandos de DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options).

### <a name="wsus-database-requirements"></a>Requisitos de base de datos de WSUS
WSUS requiere una de las siguientes bases de datos:

-   Windows Internal Database (WID)

-   Microsoft SQL Server 2017

-   Microsoft SQL Server 2016

-   Microsoft SQL Server 2014

-   Microsoft SQL Server 2012

-   Microsoft SQL Server 2008 R2

Las siguientes ediciones de SQL Server son compatibles con WSUS:

-   Standard

-   Empresas

-   Express

> [!NOTE]
> SQL Server Express 2008 R2 admite un tamaño límite de base de datos de 10 GB. Aunque probablemente este tamaño de base de datos es suficiente para WSUS, su uso no representa ningún beneficio con respecto a WID. La base de datos WID tiene un requisito mínimo de memoria de RAM de 2 GB, por encima de los requisitos estándares de un sistema Windows Server.

Puede instalar el rol de WSUS en un equipo independiente del equipo servidor de base de datos. En este caso, se aplican los siguientes criterios adicionales:

1.  El servidor de base de datos no puede ser un controlador de dominio.

2.  El servidor WSUS no puede ejecutar Servicios de Escritorio remoto.

3.  El servidor de base de datos debe estar en el mismo dominio de Active Directory que el servidor WSUS o debe existir una relación de confianza entre ellos.

4.  El servidor WSUS y el servidor de base de datos deben estar en la misma zona horaria o deben estar sincronizados en la misma Hora universal coordinada (Hora del meridiano de Greenwich).

## <a name="12-choose-a-wsus-deployment-scenario"></a>1.2. Elegir el escenario de implementación de WSUS
Esta sección describe las características básicas de todas las implementaciones de WSUS. Use esta sección para familiarizarse con una implementación simple que implica un servidor WSUS único; además de escenarios más complejos, como una jerarquía de servidores WSUS o un servidor WSUS en un segmento aislado de red.

### <a name="simple-wsus-deployment"></a>Implementación simple de WSUS
La implementación de WSUS más básica consiste en un servidor dentro de un firewall corporativo que sirve a equipos cliente en una intranet privada. El servidor WSUS se conecta con Microsoft Update para descargar actualizaciones. Esto se conoce como *sincronización*. En cada sincronización, WSUS determina si, desde la última realizada, se ha puesto a disposición alguna actualización nueva. Si es la primera vez que se sincroniza WSUS, todas las actualizaciones están disponibles para su descarga.

> [!NOTE]
> La sincronización inicial puede tardar más de una hora. Todas las sincronizaciones siguientes deberán ser mucho más rápidas.

De manera predeterminada, el servidor WSUS usa el puerto 80 para el protocolo HTTP y el puerto 443 para el protocolo HTTPS, a fin de obtener actualizaciones de Microsoft. Si hay un firewall corporativo entre su red e Internet, deberá abrir estos puertos en el servidor que se comunica directamente con Microsoft Update. Si tienes previsto usar puertos personalizados para esta comunicación, abre esos puertos. Varios servidores WSUS se pueden sincronizar con un servidor WSUS primario. De forma predeterminada, el servidor WSUS usa el puerto 8530 para el protocolo HTTP y el puerto 8531 para el protocolo HTTPS, a fin de proporcionar actualizaciones a estaciones de trabajo cliente.

### <a name="multiple-wsus-servers"></a>Varios servidores WSUS
Los administradores pueden implementar varios servidores que ejecutan WSUS para que sincronicen todo el contenido dentro de la intranet de la organización. Podrías exponer solo un servidor a Internet, que sería el único servidor que descarga actualizaciones de Microsoft Update. Este servidor se configura como el servidor que precede en la cadena el origen con el que se sincronizan los servidores que siguen en la cadena. Cuando es aplicable, los servidores pueden estar geográficamente dispersos en una red para proporcionar una mejor conectividad a todos los equipos cliente.

### <a name="disconnected-wsus-server"></a>Servidor WSUS desconectado
Si una directiva corporativa u otras condiciones limitan el acceso de los equipos a Internet, los administradores pueden configurar un servidor interno para que ejecute WSUS. Un ejemplo es un servidor conectado a la intranet, pero aislado de Internet. Después de descargar, probar y aprobar las actualizaciones en este servidor, un administrador exportaría los metadatos y el contenido de las actualizaciones a un DVD, para después importarlos a servidores que ejecutan WSUS en la intranet.

### <a name="wsus-server-hierarchies"></a>Jerarquías de servidores WSUS
Es posible crear jerarquías complejas de servidores WSUS. Dado que los servidores WSUS se pueden sincronizar entre sí en lugar de hacerlo con Microsoft Update, solo es necesario un servidor WSUS único que se conecte a Microsoft Update. Cuando piensa en vincular servidores WSUS, hay uno que precede en la cadena y otro que sigue en la cadena. Una implementación de jerarquía de servidores WSUS ofrece los siguientes beneficios:

-   Puede descargar actualizaciones desde Internet una sola vez y después distribuirlas a los equipos cliente mediante servidores que siguen en la cadena. Este método ahorra ancho de banda en la conexión corporativa a Internet.

-   Puede descargar actualizaciones en un servidor WSUS que esté físicamente cerca de los equipos cliente, por ejemplo, en sucursales.

-   Puede configurar servidores WSUS independientes para equipos cliente que usan productos de Microsoft en distintos idiomas.

-   Puede escalar WSUS para una organización grande, cuyos equipos cliente superan la cantidad que un servidor WSUS puede administrar con eficacia.

> [!NOTE] 
> Se recomienda no crear una jerarquía de servidores WSUS con una profundidad mayor que tres niveles. Cada nivel requiere tiempo para propagar actualizaciones a todos los servidores conectados. Si bien no hay un límite teórico de jerarquía, Microsoft solo ha probado implementaciones con una jerarquía de cinco niveles de profundidad.
>
> Además, los servidores que siguen en la cadena deben tener la misma versión o una versión anterior de WSUS como origen de la sincronización del servidor de nivel superior.

Puede conectar servidores WSUS en modo autónomo (para una administración distribuida) o en modo de réplica (para una administración centralizada). No es necesario implementar una jerarquía de servidores en un solo modo: puede implementar una solución WSUS que use servidores WSUS en ambos modos, autónomo y de réplica.

#### <a name="autonomous-mode"></a>Modo autónomo
El modo autónomo, también denominado administración distribuida, es la opción de instalación predeterminada para WSUS. En este modo, un servidor WSUS que precede en la cadena comparte actualizaciones con los servidores que siguen en la cadena, durante la sincronización. Los servidores WSUS que siguen en la cadena se administran de forma independiente y no reciben el estado de aprobación de una actualización ni información del grupo de equipos del servidor que precede en la cadena. Al usar el modelo de administración distribuida, el administrador de cada servidor WSUS selecciona idiomas de actualización, crea grupos de equipos, asigna equipos a grupos, prueba y aprueba actualizaciones y se asegura de que las actualizaciones correctas se instalen en los grupos de equipos adecuados. La siguiente imagen muestra cómo se podrían implementar servidores WSUS autónomos en un entorno de sucursal:

#### <a name="replica-mode"></a>Modo de réplica
El modo de réplica, también denominado administración centralizada, funciona con un servidor WSUS que precede en la cadena y que comparte actualizaciones, estados de aprobación y grupos de equipos con los servidores que siguen en la cadena. Los servidores de réplica heredan las aprobaciones de actualización y no pueden administrarse al margen del servidor WSUS que los precede . La siguiente imagen muestra cómo se podrían implementar servidores WSUS de réplica en un entorno de sucursal:

> [!NOTE]
> Si configura varios servidores de réplica para que se conecten con un servidor WSUS único que los precede, no programe la sincronización para que se ejecute al mismo tiempo en todos los servidores de réplica. Este procedimiento evitará un repentino aumento del uso del ancho de banda.

### <a name="branch-offices"></a>Sucursales
Puede aprovechar la característica de sucursal de Windows para optimizar la implementación de WSUS. Este tipo de implementación ofrece las siguientes ventajas:

1.  Ayuda a reducir el uso del vínculo WAN y mejora la capacidad de respuesta de la aplicación. Para habilitar la aceleración BranchCache del contenido proporcionado por un servidor WSUS, instale la característica BranchCache en el servidor y en los equipos cliente, y asegúrese de que el servicio BranchCache se haya iniciado. No es necesario realizar otros pasos.

2.  La característica de sucursal también se puede usar en sucursales que tengan conexiones de ancho de banda reducido con la oficina central, pero conexiones de ancho de banda alto con Internet. En este caso, quizás desee configurar servidores WSUS que siguen en la cadena para que obtengan información sobre las actualizaciones que se deben instalar desde el servidor WSUS central, pero que las descarguen desde Microsoft Update.

### <a name="network-load-balancing"></a>Equilibrio de carga de red
El equilibrio de carga de red (NLB) aumenta la confiabilidad y el rendimiento de la red WSUS. Puedes configurar varios servidores WSUS para que compartan un solo clúster de conmutación por error que ejecuta SQL Server, como SQL Server 2008 R2 SP1. En esta configuración, debe usar una instalación completa de SQL Server, no la instalación de Windows Internal Database que se proporciona con WSUS, y el rol de base de datos debe instalarse en todos los servidores WSUS front-end. Además puede hacer que todos los servidores WSUS usen un sistema de archivos distribuido (DFS) para almacenar su contenido.

**Programa de instalación de WSUS para NLB:** en comparación con el programa de instalación de WSUS 3.2 para NLB, ya no son necesarios ni parámetros ni una llamada de configuración especial para configurar WSUS para NLB. Solo es necesario instalar los servidores WSUS con las consideraciones siguientes.

-   WSUS debe instalarse con la opción de base de datos SQL en lugar de WID.

-   Si almacena las actualizaciones de forma local, debe compartirse la misma carpeta de contenido entre los servidores WSUS que comparten la misma base de datos SQL.

-   La instalación de WSUS debe realizarse en serie. Las tareas posteriores a la instalación no se pueden ejecutar en varios servidores al mismo tiempo cuando se comparte la misma base de datos SQL.

### <a name="wsus-deployment-with-roaming-client-computers"></a>Implementación de WSUS con equipos cliente móviles
Si la red incluye usuarios móviles que inician sesión desde distintas ubicaciones, puede configurar WSUS para permitir que estos usuarios actualicen sus equipos cliente desde el servidor WSUS que tengan geográficamente más cerca. Por ejemplo, puedes implementar un servidor WSUS en cada región y usar una subred DNS diferente para cada región. Todos los equipos cliente se dirigen al mismo servidor WSUS, que se resuelve en cada subred como el servidor WSUS físico más cercano.

## <a name="13-choose-a-wsus-storage-strategy"></a>1.3. Elegir la estrategia de almacenamiento de WSUS
Windows Server Update Services (WSUS) usa dos tipos de sistemas de almacenamiento: una base de datos para almacenar la configuración de WSUS y los metadatos de actualizaciones, y un sistema de archivos local opcional para almacenar archivos de actualización. Antes de instalar WSUS, debe decidir la manera en que implementará el almacenamiento.

Las actualizaciones se componen de dos partes: los metadatos que describen la actualización y los archivos necesarios para instalarla. Los metadatos de una actualización normalmente son mucho más pequeños que la actualización en sí y se almacenan en la base de datos de WSUS. Los archivos de una actualización se almacenan en un servidor WSUS local o en un servidor web de Microsoft Update.

### <a name="wsus-database"></a>Base de datos de WSUS
WSUS requiere una base de datos para cada servidor WSUS. WSUS admite el uso de una base de datos que resida en otro equipo que no sea el servidor WSUS, con algunas restricciones. Para obtener una lista de las bases de datos admitidas y las limitaciones de bases de datos remotas, consulta la sección "1.1 Revisar consideraciones iniciales y requisitos del sistema" de esta guía.

La base de datos WSUS almacena la siguiente información:

-   Información de configuración de servidores WSUS

-   Metadatos que describen cada actualización

-   Información sobre equipos cliente, actualizaciones e interacciones

Si instala varios servidores WSUS, debe tener una base de datos independiente para cada uno de ellos, sean autónomos o de réplica. No puede almacenar varias bases de datos de WSUS en una instancia única de SQL Server, salvo en clústeres de equilibrio de carga de red que usan la conmutación por error de SQL Server.

SQL Server, SQL Server Express y Windows Internal Database proporcionan las mismas características de rendimiento para una configuración de servidor único, donde la base de datos y el servicio de WSUS se ubican en el mismo equipo. Una configuración de servidor único puede admitir miles de equipos cliente de WSUS.

> [!NOTE]
> No intente administrar WSUS accediendo directamente a la base de datos. La manipulación directa de la base de datos puede dañarla. Los daños podrían no detectarse de inmediato, pero pueden impedir actualizaciones a las siguientes versiones del producto. Para administrar WSUS, puede usar la consola de WSUS o las interfaces de programación de aplicaciones (API) de WSUS.

#### <a name="wsus-with-windows-internal-database"></a>WSUS con Windows Internal Database
De manera predeterminada, el asistente para instalación crea y usa Windows Internal Database con el nombre SUSDB.mdf. Esta base de datos se ubica en la carpeta %windir%\wid\data\, donde %windir% es la unidad local en la que se instala el software del servidor WSUS.

> [!NOTE]
> Windows Internal Database (WID) se incorporó en Windows Server 2008.

WSUS admite la autenticación de Windows solo para la base de datos. No se puede usar la autenticación de SQL Server con WSUS. Si usa Windows Internal Database para la base de datos de WSUS, el programa de instalación de WSUS crea una instancia de SQL Server con el nombre server\Microsoft##WID, donde server es el nombre del equipo. Con cualquiera de las opciones de base de datos, el programa de instalación de WSUS crea una base de datos con el nombre SUSDB. El nombre de esta base de datos no es configurable.

Se recomienda que use Windows Internal Database en los siguientes casos:

-   La organización aún no ha adquirido y no necesita un producto de SQL Server para ninguna otra aplicación.

-   La organización no necesita una solución NLB WSUS.

-   Se intenta implementar varios servidores WSUS (por ejemplo, en sucursales). En este caso, debe considerar usar Windows Internal Database en servidores secundarios, aun si va a usar SQL Server para el servidor WSUS raíz. Dado que cada servidor WSUS requiere una instancia independiente de SQL Server, pronto experimentará problemas de rendimiento de la base de datos si una sola instancia de SQL Server administra varios servidores WSUS.

Windows Internal Database no proporciona una interfaz de usuario ni herramientas de administración de bases de datos. Si selecciona esta base de datos para WSUS, debe usar herramientas externas para administrarla. Para más información, consulta lo siguiente:

-   [Copias de seguridad y restauraciones de datos de WSUS y copias de seguridad de tu servidor](https://technet.microsoft.com/library/dd939904(WS.10).aspx)

-   [Reindización de la base de datos de WSUS](https://technet.microsoft.com/library/dd939795(WS.10).aspx)

#### <a name="wsus-with-sql-server"></a>WSUS con SQL Server
Se recomienda que use SQL Server con WSUS en los siguientes casos:

1.  Necesita una solución NLB WSUS.

2.  Ya tiene por lo menos una instancia de SQL Server instalada.

3.  No puede ejecutar el servicio de SQL Server con una cuenta local no del sistema ni usando la autenticación de SQL Server. WSUS admite la autenticación de Windows únicamente.

### <a name="wsus-update-storage"></a>Almacenamiento de actualizaciones de WSUS
Cuando se sincronizan las actualizaciones con su servidor WSUS, los archivos de actualización y los metadatos se almacenan en dos ubicaciones independientes. Los metadatos se almacenan en la base de datos de WSUS. Los archivos de actualización se pueden almacenar en el servidor WSUS o en servidores de Microsoft Update, en función de cómo haya configurado las opciones de sincronización. Si decide almacenar los archivos de actualización en el servidor WSUS, los equipos cliente descargarán actualizaciones aprobadas desde el servidor WSUS local. En caso contrario, los equipos cliente descargarán actualizaciones aprobadas directamente desde Microsoft Update. La opción que mejor se adapte a su organización dependerá del ancho de banda de red para conectarse a Internet, del ancho de banda de red de la intranet y de la disponibilidad de almacenamiento local.

Puede seleccionar otra solución de almacenamiento de actualizaciones para cada servidor WSUS que implemente.

#### <a name="local-wsus-server-storage"></a>Almacenamiento en servidor WSUS local
El almacenamiento local de archivos de actualización es la opción predeterminada cuando instala y configura WSUS. Esta opción puede ahorrar ancho de banda en la conexión corporativa a Internet, porque los equipos cliente descargan actualizaciones directamente desde el servidor WSUS local.

Esta opción requiere que el servidor tenga suficiente espacio en disco para almacenar todas las actualizaciones necesarias. Como mínimo, WSUS requiere 20 GB para almacenar actualizaciones de forma local. Sin embargo, se recomienda tener 30 GB en función de variables probadas.

#### <a name="remote-storage-on-microsoft-update-servers"></a>Almacenamiento remoto en servidores de Microsoft Update
Puede almacenar actualizaciones de forma remota en servidores de Microsoft Update. Esta opción es útil si la mayoría de los equipos cliente se conecta al servidor WSUS con una conexión WAN lenta, pero se conectan a Internet con una conexión de ancho de banda alto.

En este caso, el servidor WSUS raíz se sincroniza con Microsoft Update y recibe los metadatos de las actualizaciones. Una vez que se aprueben las actualizaciones, los equipos cliente las descargarán desde servidores de Microsoft Update.

## <a name="14-choose-wsus-update-languages"></a>1.4. Elegir idiomas de actualización de WSUS
Cuando implementa una jerarquía de servidores WSUS, debe determinar los idiomas de actualización que se usan en toda la organización. Debe configurar el servidor WSUS raíz para que descargue actualizaciones en todos los idiomas utilizados en la organización.

Por ejemplo, la oficina principal necesita actualizaciones en francés e inglés, pero una sucursal las necesita en inglés, francés y alemán, y otra sucursal las necesita en inglés y español. En esta situación, se configuraría el servidor WSUS raíz para que descargue actualizaciones en inglés, francés, alemán y español. Después se configuraría el servidor WSUS de la primera sucursal para que descargue solo actualizaciones en inglés, francés y alemán, y se configuraría la segunda sucursal para que descargue solo actualizaciones en inglés y español.

La página **Elegir idiomas** del Asistente para la configuración de WSUS permite obtener actualizaciones en todos los idiomas o en un subconjunto de idiomas. Si seleccionas un subconjunto de idiomas, ahorras espacio en disco. No obstante, es IMPORTANTE elegir todos los idiomas que se necesitan para todos los servidores que siguen en la cadena y los equipos cliente de un servidor WSUS.

A continuación encontrarás notas IMPORTANTES sobre el idioma de actualización que debes tener en cuenta, antes de configurar esta opción:

-   Siempre incluya inglés además de cualquier otro idioma necesario en la organización. Todas las actualizaciones se basan en paquetes de idioma para inglés.

-   Los servidores que siguen en la cadena y los equipos cliente no recibirán todas las actualizaciones adecuadas, si no se han seleccionado todos los idiomas necesarios para el servidor que precede en la cadena. Asegúrese de seleccionar todos los idiomas que necesitan todos los equipos cliente asociados con los servidores que siguen en la cadena.

-   Por lo general, se deben descargar actualizaciones en todos los idiomas en el servidor WSUS raíz que se sincroniza con Microsoft Update. Esta selección garantiza que todos los servidores que siguen en la cadena y los equipos cliente recibirán las actualizaciones en los idiomas adecuados.

Si almacena actualizaciones de forma local y ha configurado un servidor WSUS para que descargue una cantidad limitada de idiomas, podrá notar actualizaciones en idiomas distintos a los que especificó. Muchos archivos de actualización son grupos de varios idiomas, que incluyen al menos uno de los idiomas especificados en el servidor.

**Servidores que preceden en la cadena**

> [!NOTE]
> Configure los servidores que preceden en la cadena para que sincronicen las actualizaciones en todos los idiomas que requieren los servidores de réplica que siguen en la cadena. No se le notificarán las actualizaciones necesarias en los idiomas no sincronizados.

Las actualizaciones se mostrarán como **No aplicables** en los equipos cliente que requieran el idioma. Para evitar esto, asegúrese de que se incluyen todos los idiomas de sistemas operativos en las opciones de sincronización del servidor WSUS. Puedes ver todos los idiomas del sistema operativo en la vista **Equipos** de la consola de administración de WSUS y ordenar los equipos por idioma del sistema operativo. Sin embargo, puede que quieras incluir más idiomas si hay aplicaciones de Microsoft en más de un idioma (por ejemplo, si se instala la versión en francés de Microsoft Word en algunos equipos que usan la versión en inglés de Windows 8).

La elección de idiomas de un servidor que precede en la cadena no es lo mismo que elegir idiomas para un servidor que sigue en la cadena. En los procedimientos siguientes se explican las diferencias.

#### <a name="to-choose-update-languages-for-a-server-synchronizing-from-microsoft-update"></a>Elección de idiomas de actualización para un servidor de sincronización de Microsoft Update

1.  En el Asistente para configuración de WSUS:

    -   Para obtener actualizaciones en todos los idiomas, haga clic en **Descargar actualizaciones en todos los idiomas, incluyendo los más recientes**.

    -   Para obtener actualizaciones solo de idiomas concretos, haga clic en **Descargar actualizaciones solamente en estos idiomas**, y luego seleccione los idiomas para los que quiere actualizaciones.

#### <a name="to-choose-update-languages-for-a-downstream-server"></a>Elección de idiomas de actualización para un servidor que sigue en la cadena

1.  Si el servidor que precede en la cadena se configuró para descargar archivos de actualización de un subconjunto de idiomas: En el Asistente para configuración de WSUS, haga clic en **Descargar actualizaciones solamente en estos idiomas (el servidor que precede en la cadena solo admite los idiomas marcados con un asterisco)** y luego seleccione los idiomas para los que quiere actualizaciones.

> [!NOTE]
> Debe hacer esto aunque quiera que el servidor que sigue en la cadena descargue los mismos idiomas que el servidor que precede en la cadena.

2. Si el servidor que precede en la cadena se configuró para descargar archivos de actualización de todos los idiomas: En el Asistente para configuración de WSUS, haga clic en **Descargar actualizaciones en todos los idiomas que admite el servidor que precede en**.

> [!NOTE]
> Debe hacer esto aunque quiera que el servidor que sigue en la cadena descargue los mismos idiomas que el servidor que precede en la cadena. Esta configuración hace que el servidor que precede en la cadena descargue actualizaciones en todos los idiomas, incluidos los idiomas que no se configuraron originalmente para el servidor que precede en la cadena. Si agrega idiomas al servidor que precede en la cadena, debe copiar las nuevas actualizaciones en sus servidores de réplica.
>
> Si se cambian las opciones de idioma solo en el servidor que precede en la cadena, se puede provocar una falta de coincidencia entre el número de actualizaciones aprobadas en el servidor central y el número de actualizaciones aprobadas en los servidores de réplica.

## <a name="15-plan-wsus-computer-groups"></a>1.5. Planear grupos de equipos WSUS
WSUS le permite dirigir actualizaciones a grupos de equipos cliente para asegurarse de que determinados equipos reciban siempre las actualizaciones correspondientes en los momentos más oportunos. Por ejemplo, si todos los equipos de un departamento (como el equipo de Contabilidad) tienen una configuración específica, puede configurar un grupo para ese equipo, decidir las actualizaciones que se instalarán y después usar informes de WSUS para evaluar las actualizaciones correspondientes al equipo.

> [!NOTE]
> Si un servidor WSUS se ejecuta en el modo de réplica, no se pueden crear grupos en ese servidor. Todos los grupos de equipos necesarios para equipos cliente del servidor de réplicas deben crearse en el servidor WSUS que se encuentra en la raíz de la jerarquía de servidores WSUS. Para obtener más información acerca del modo de réplica, consulta Administrar servidores de réplica WSUS [Administrar servidores de réplica WSUS](https://technet.microsoft.com/library/dd939893(WS.10).aspx) en el manual de operaciones de WSUS 3.0 SP2.

Los equipos siempre se asignan al grupo **Todos los equipos** y permanecen en el grupo **Equipos sin asignar** hasta que los asignes a otro grupo. Los equipos pueden pertenecer a más de un grupo.

Los grupos de equipos pueden configurarse en jerarquías (por ejemplo, el grupo Nómina y el grupo Cuentas por pagar debajo del grupo Contabilidad). Las actualizaciones aprobadas para un grupo superior se implementarán automáticamente en los grupos inferiores. En este ejemplo, si aprueba Update1 para el grupo Contabilidad, la actualización se implementará en todos los equipos de este grupo, en todos los equipos del grupo Nómina y en todos los equipos del grupo Cuentas por pagar.

Dado que los equipos pueden asignarse a varios grupos, es posible que una actualización se apruebe más de una vez para el mismo equipo. No obstante, la actualización se implementará una sola vez y cualquier conflicto se resolverá en el servidor WSUS. Para continuar con el ejemplo anterior, si el EquipoA se asigna al grupo Nómina y al grupo Cuentas por pagar y Update1 se aprueba para ambos grupos, esta se implementará una sola vez.

Puede asignar equipos a grupos de equipos por medio de dos métodos: asignación del lado servidor o asignación del lado cliente. A continuación se encuentran las definiciones de cada método:

-   **Asignación del lado servidor**: se asigna de forma manual uno o más equipos cliente a varios grupos simultáneamente.

-   **Asignación del lado cliente**: se usa la directiva de grupo o se modifica la configuración del registro en los equipos cliente para habilitar esos equipos de manera que se agreguen automáticamente a los grupos de equipos creados anteriormente.

### <a name="conflict-resolution"></a>Resolución de conflictos
El servidor aplica las siguientes reglas para resolver conflictos y determinar la acción correspondiente en los clientes:

1.  Prioridad

2.  Instalar o desinstalar

3.  Fecha límite

#### <a name="priority"></a>Prioridad
Las acciones asociadas al grupo de mayor prioridad reemplazan las acciones de otros grupos. Cuanto mayor sea la profundidad de un grupo dentro de la jerarquía de grupos, mayor será su prioridad. La prioridad solo se asigna en función de la profundidad; todas las ramas tienen la misma prioridad. Por ejemplo, un grupo dos niveles por debajo de la rama Equipos de escritorio tiene una prioridad más alta que un grupo un nivel por debajo de la rama Servidor.

En el siguiente ejemplo de texto del panel de jerarquías de la consola de Update Services, para un servidor WSUS denominado WSUS-01, se han agregado grupos de equipos denominados Equipos de escritorio y Servidor al grupo predeterminado **Todos los equipos** . Tanto el grupo Equipos de escritorio como el grupo Servidor tienen el mismo nivel jerárquico.

-   **Update Services**

    -   **WSUS-01**

        -   **Actualizaciones**

        -   **equipos**

            -   **Todos los equipos**

                -   **Equipos sin asignar**

                -   **Equipos de escritorio**

                    -   **Desktops-L1**

                        -   **Desktops-L2**

                -   **Servidores**

                    -   **Servers-L1**

        -   **Servidores que siguen en la cadena**

        -   **Sincronizaciones**

        -   **Informes**

        -   **Opciones**

En este ejemplo, el grupo que está dos niveles por debajo de la rama Equipos de escritorios (Desktops L2) tiene mayor prioridad que el grupo que está un nivel por debajo de la rama Servidor (Servers L1). Por lo tanto, para un equipo que pertenezca tanto al grupo Desktops-L2 como al grupo Servers-L1, todas las acciones del grupo Desktops-L2 tendrán prioridad sobre las acciones especificadas para el grupo Servers-L1.

#### <a name="priority-of-install-and-uninstall"></a>Prioridad de la instalación y desinstalación
Instalar acciones reemplaza a desinstalar acciones. Las instalaciones obligatorias reemplazan a las instalaciones opcionales (estas últimas están disponibles solo a través de la API y cambiar la aprobación para una actualización con la Consola de administración de WSUS borrará todas las aprobaciones opcionales).

#### <a name="priority-of-deadlines"></a>Prioridad de las fechas de entrega
Las acciones que tienen una fecha límite reemplazan a las que no la tienen.  Las acciones con fechas límite anteriores reemplazan a las que tienen fechas límite posteriores.

## <a name="16-plan-wsus-performance-considerations"></a>1.6. Planear consideraciones sobre el rendimiento de WSUS
Antes de implementar WSUS, debes planear con cuidado algunas áreas para obtener un mejor rendimiento. Las áreas clave son:

-   Configuración de red

-   Descarga diferida

-   Filtros

-   Instalación

-   Implementaciones de actualizaciones grandes

-   Servicio de transferencia inteligente en segundo plano (BITS)

### <a name="network-setup"></a>Configuración de red
Para optimizar el rendimiento en redes de WSUS, considere estas sugerencias:

1.  Configure redes de WSUS en una topología de concentrador y radio, en lugar de una topología jerárquica.

2.  Use el orden de máscara de red DNS para equipos cliente móviles y configure estos equipos para que obtengan actualizaciones del servidor WSUS local.

### <a name="deferred-download"></a>Descarga diferida
Puede aprobar actualizaciones y descargar los metadatos antes de descargar los archivos de actualización. Este método se denomina *descarga diferida*. Al diferir descargas, una actualización se descarga solo después de haber sido aprobada. Se recomienda diferir descargas porque de esta manera se optimiza el ancho de banda de red y el espacio en disco.

En una jerarquía de servidores WSUS, WSUS establece automáticamente la configuración de descarga diferida del servidor WSUS raíz en todos los servidores que siguen en la cadena. Esta configuración predeterminada se puede modificar. Por ejemplo, puede configurar un servidor que precede en la cadena para que realice sincronizaciones completas e inmediatas, y después configurar un servidor que sigue en la cadena para que difiera las descargas.

Si implementa una jerarquía de servidores WSUS conectados, se recomienda que no anide servidores con profundidad. Si habilitas las descargas diferidas y un servidor que sigue en la cadena solicita una actualización no aprobada en el servidor que precede, esta solicitud fuerza una descarga en el servidor que precede. El servidor que sigue en la cadena después descarga la actualización en una sincronización posterior. En una jerarquía profunda de servidores WSUS, se pueden producir demoras porque las actualizaciones se solicitan, se descargan y después se pasan a través de la jerarquía. De manera predeterminada, las descargas diferidas se habilitan cuando las actualizaciones se guardan de forma local. Puede cambiar esta opción manualmente.

### <a name="filters"></a>Filtros
WSUS le permite filtrar sincronizaciones de actualizaciones por idioma, producto y clasificación. En una jerarquía de servidores WSUS, WSUS establece automáticamente las opciones de filtrado seleccionadas en el servidor WSUS raíz en todos los servidores que siguen en la cadena. Puede volver a configurar los servidores de descarga para que reciban un subconjunto de idiomas únicamente.

De manera predeterminada, los productos que se van actualizar son Windows y Office, y las clasificaciones predeterminadas son actualizaciones críticas, de seguridad y de definiciones. Para conservar el ancho de banda y el espacio en disco, se recomienda limitar los idiomas a aquellos que realmente usa.

### <a name="installation"></a>Instalación
Las actualizaciones normalmente consisten en nuevas versiones de archivos que ya existen en un equipo que se está actualizando. En un nivel binario, estos archivos existentes podrían no diferir demasiado de las versiones actualizadas. La característica de archivos de instalación rápida identifica el número exacto de bytes entre versiones, crea y distribuye actualizaciones solo para esas diferencias y después combina el archivo existente con los bytes actualizados.

A veces esta característica se denomina "entrega de diferencia" porque descarga únicamente la diferencia (delta) entre dos versiones de un archivo. Los archivos de instalación rápida son más grandes que las actualizaciones que se distribuyen a equipos cliente, porque un archivo de instalación rápida contiene todas las versiones posibles de cada archivo que se va a actualizar.

Puede usar archivos de instalación rápida para limitar el ancho de banda que se consume en la red local, ya que WSUS transmite solo el delta aplicable a una versión determinada de un componente actualizado. Sin embargo, esto requiere ancho de banda adicional entre el servidor WSUS, los servidores WSUS que preceden en la cadena y Microsoft Update, además de espacio adicional en el disco local. De manera predeterminada, WSUS no usa archivos de instalación rápida.

No todas las actualizaciones son óptimas para su distribución con archivos de instalación rápida. Si selecciona esta opción, obtendrá archivos de instalación rápida para todas las actualizaciones. Si no almacena las actualizaciones de forma local, el agente de Windows Update decidirá si se deben descargar los archivos de instalación rápida o las distribuciones de actualización completas.

### <a name="large-update-deployment"></a>Implementaciones de actualizaciones grandes
Cuando implementa actualizaciones grandes (como Service Packs), puede evitar que se sature la red de la siguiente manera:

1.  Use el límite del Servicio de transferencia inteligente en segundo plano (BITS). Las limitaciones de ancho de banda de BITS se pueden controlar por hora del día, pero se aplican a todas las aplicaciones que usan BITS. Para obtener información sobre cómo controlar la limitación de BITS, consulta [Directivas de grupo](https://msdn.microsoft.com/library/windows/desktop/aa362844(v=vs.85).aspx).

2.  Use el límite de Internet Information Services (IIS) para limitar uno o más servicios web.

3.  Use grupos de equipos para controlar el lanzamiento. Un equipo cliente se identifica a sí mismo como miembro de un determinado grupo de equipos, cuando envía información al servidor WSUS. El servidor WSUS usa esta información para determinar las actualizaciones que deben implementarse en ese equipo. Puede configurar varios grupos de equipos y aprobar secuencialmente grandes descargas de Service Packs para un subconjunto de estos grupos.

### <a name="background-intelligent-transfer-service"></a>Servicio de transferencia inteligente en segundo plano
WSUS usa el protocolo Servicio de transferencia inteligente en segundo plano (BITS) para todas sus tareas de transferencia de archivos. Esto incluye descargas a equipos cliente y sincronizaciones de servidores. BITS habilita programas para que descarguen archivos usando ancho de banda de reserva. BITS mantiene las transferencias de archivos cuando se producen desconexiones de red y se reinicia el equipo. Para más información, consulta lo siguiente: [Servicio de transferencia inteligente en segundo plano](https://msdn.microsoft.com/library/bb968799.aspx).

## <a name="17-plan-automatic-updates-settings"></a>1.7. Planear la configuración de actualizaciones automáticas
Puede especificar una fecha límite para aprobar actualizaciones en el servidor WSUS. La fecha límite hace que los equipos cliente instalen la actualización en un momento específico, pero hay situaciones que varían según si la fecha límite expiró, si hay otras actualizaciones en la cola para instalarse en el equipo y si la actualización (u otra actualización en la cola) requiere el reinicio.

De manera predeterminada, el servicio Actualizaciones automáticas sondea el servidor WSUS para detectar actualizaciones aprobadas, cada 22 horas, con un desplazamiento aleatorio. Si hay nuevas actualizaciones que necesitan instalarse, estas se descargan. El tiempo transcurrido entre cada ciclo de detección se puede manipular entre 1 y 22 horas.

Puede manipular las opciones de notificación de la siguiente manera:

1.  Si Actualizaciones automáticas se configura para que notifique al usuario cuando haya actualizaciones listas para instalarse, la notificación se envía al registro del sistema y al área de notificación del equipo cliente.

2.  Cuando un usuario con credenciales apropiadas hace clic en el icono del área de notificación, Actualizaciones automáticas muestra las actualizaciones disponibles para instalar. El usuario debe hacer clic en **Instalar** para que la instalación comience. Si la actualización requiere el reinicio del equipo para completarse, aparecerá un mensaje. Si se solicita el reinicio, el servicio Actualizaciones automáticas no podrá detectar otras actualizaciones hasta que el equipo no se reinicie.

Si este servicio se configura para instalar actualizaciones con una programación establecida, las actualizaciones aplicables se descargarán y marcarán como listas para instalar. Actualizaciones automáticas notifica a los usuarios que tienen credenciales apropiadas mediante el icono del área de notificación y un evento se registra en el registro del sistema.

En el día y hora programados, Actualizaciones automáticas instala la actualización y reinicia el equipo (si es necesario), aun cuando ningún administrador local haya iniciado sesión. Si un administrador local inicia sesión y el equipo debe reiniciarse, Actualizaciones automáticas muestra una advertencia y una cuenta regresiva para el reinicio. De lo contrario, la instalación se produce en segundo plano.

Si el equipo debe reiniciarse y cualquier usuario inició sesión, se muestra un cuadro de diálogo de cuenta regresiva similar que advierte al usuario sobre el reinicio inminente. Puede manipular el reinicio del equipo con la directiva de grupo.

Una vez descargadas las actualizaciones nuevas, Actualizaciones automáticas sondea la lista de paquetes aprobados en el servidor WSUS para confirmar que los paquetes que descargó aún son válidos y están aprobados. Esto significa que si un administrador de WSUS elimina actualizaciones de la lista de actualizaciones aprobadas mientras Actualizaciones automáticas está realizando una descarga, solo se instalarán las actualizaciones que aún estén aprobadas.

