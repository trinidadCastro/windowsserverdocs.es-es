---
title: Implementación de un servidor de archivos en clúster de dos nodos
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: En este artículo describe cómo crear un clúster de servidores de archivos de dos nodos
ms.openlocfilehash: fbfde60f60df64514a6a0f514cbabd005544af84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846416"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Implementación de un servidor de archivos en clúster de dos nodos

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad de las aplicaciones y los servicios. Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno de los nodos del clúster, otro comienza a dar servicio (proceso que se denomina conmutación por error). Los usuarios experimentarán un número de interrupciones mínimo en el servicio.

Esta guía describe los pasos para instalar y configurar un clúster de conmutación por error de servidor de archivos de propósito general que tiene dos nodos. Al crear la configuración en esta guía, puede obtener información acerca de los clústeres de conmutación por error y familiarizarse con la interfaz del complemento Administración de clúster de conmutación por error en Windows Server 2019 o Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Información general para un clúster de servidores de archivos de dos nodos

Servidores en un clúster de conmutación por error pueden funcionar en diversos roles, incluidos los roles de servidor de archivos, servidor de Hyper-V o el servidor de base de datos y pueden proporcionar una alta disponibilidad para una variedad de otros servicios y aplicaciones. En esta guía se describe cómo configurar un clúster de servidores de archivos de dos nodos.

Un clúster de conmutación por error normalmente incluye una unidad de almacenamiento que está conectada físicamente a todos los servidores del clúster, aunque solo un servidor tiene acceso cada vez a cualquier volumen determinado el almacenamiento. En el siguiente diagrama se muestra un clúster de conmutación por error de dos nodos conectado a una unidad de almacenamiento.

![Clúster de dos nodos](media\Cluster-File-Server\Cluster-FS-Overview.png)

Los volúmenes de almacenamiento o números de unidad lógica (LUN) expuestos a los nodos de un clúster no se deben exponer a otros servidores, incluidos los servidores de otro clúster. En el diagrama siguiente se ilustra esto.

![LUN de almacenamiento](media\Cluster-File-Server\Cluster-FS-LUNs.png)

Tenga en cuenta que para la disponibilidad máxima de cualquier servidor, es importante seguir los procedimientos recomendados de administración del servidor, por ejemplo, administrar con cuidado el entorno físico de los servidores, probar los cambios de software antes de implementarlos por completo y realizar un cuidadoso seguimiento de las actualizaciones de software y los cambios de configuración en todos los servidores en clúster.

El escenario siguiente describe cómo se puede configurar un clúster de conmutación por error de servidor de archivos. Los archivos que se están compartiendo están en el almacenamiento de clúster y cualquier servidor en clúster puede actuar como servidor de archivos que los comparte.

## <a name="shared-folders-in-a-failover-cluster"></a>Carpetas compartidas en un clúster de conmutación por error

La lista siguiente describe la funcionalidad de configuración de carpeta compartida que está integrada en la agrupación en clústeres de conmutación por error:

- Para mostrar se limita a agrupado carpetas compartidas solo (no se combinan con las carpetas compartidas no agrupado): Cuando un usuario ve las carpetas compartidas especificando la ruta de acceso de un servidor de archivos en clúster, la presentación incluirá solo las carpetas compartidas que forman parte de la función de servidor de archivo específico. Excluirá las carpetas compartidas no agrupado y parte de los recursos compartidos de los roles de servidor de archivos independientes que se producen en un nodo del clúster.

- Enumeración basada en acceso: puede utilizar la enumeración basada en el acceso para ocultar una carpeta determinada de la vista de los usuarios. En lugar de permitir a los usuarios ver la carpeta pero no tener acceso a nada de ella, puede elegir impedir que vean la carpeta en su totalidad. Puede configurar la enumeración basada en acceso para una carpeta compartida en clúster en la misma manera que una carpeta compartida no clúster.

- Acceso sin conexión: puede configurar el acceso sin conexión (almacenamiento en caché) para una carpeta compartida en clúster de la misma manera que para una carpeta compartida no clúster.

- Los discos en clúster siempre se reconocen como parte del clúster: Si usa la interfaz de clúster de conmutación por error, el Explorador de Windows o el complemento de administración de almacenamiento y recursos compartidos, Windows reconoce si un disco se ha designado como si estuviera en el almacenamiento del clúster. Si ese tipo de disco ya se ha configurado en administración de clúster de conmutación por error como parte de un servidor de archivos en clúster, a continuación, puede usar cualquiera de las interfaces mencionadas anteriormente para crear un recurso compartido en el disco. Si ese tipo de disco no se ha configurado como parte de un servidor de archivos en clúster, no puede crear por error un recurso compartido en él. En su lugar, un error indica que el disco se debe configurar primero como parte de un servidor de archivos en clúster antes de que se pueda compartir.

- Integración de servicios para Network File System: El rol de servidor de archivos en Windows Server incluye el servicio de rol opcional denominado Servicios para Network File System (NFS). Instalando el servicio de rol y configurando carpetas compartidas con Servicios para NFS, puede crear un servidor de archivos en clúster que admita clientes basados en UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Requisitos para un clúster de conmutación por error de dos nodos

Para un clúster de conmutación por error en Windows Server 2016 o Windows Server 2019 para considerarse una solución oficialmente admitida por Microsoft, la solución debe cumplir los siguientes criterios.

- Todos los componentes de hardware y software deben cumplir con los requisitos del logotipo que corresponda. Para Windows Server 2016, este es el logotipo "Certificado para Windows Server 2016". Para Windows Server 2019, este es el logotipo "Certificado para Windows Server 2019". Para obtener más información sobre qué sistemas de hardware y software certificadas, visite el Microsoft [Windows Server Catalog](https://www.windowsservercatalog.com/default.aspx) sitio.

- La solución totalmente configurada (servidores, red y almacenamiento) debe superar todas las pruebas en el Asistente de validación, que forma parte del complemento clúster de conmutación por error.

Se necesitará para un clúster de conmutación por error de dos nodos.

- **servidores:** Se recomienda usar los equipos que cumplan con los componentes iguales o similares.  Los servidores de un clúster de conmutación por error de dos nodos deben ejecutar la misma versión de Windows Server. También deben tener las mismas actualizaciones de software (revisiones).

- **Adaptadores de red y cable:** El hardware de red, como otros componentes de la solución de clúster de conmutación por error, debe ser compatible con Windows Server 2016 o Windows Server 2019. Si usas iSCSI, los adaptadores de red deben estar dedicados a la comunicación de red o iSCSI, pero no ambos. En la infraestructura de red que conecta los nodos de clúster, evite tener puntos de concentración de errores. Hay varias maneras de lograr esto. Puede conectar los nodos del clúster mediante varias redes distintas. Alternativamente, puede conectar los nodos del clúster con una red construida con adaptadores de red en equipo, conmutadores redundantes, enrutadores redundantes u otro hardware similar que elimine los puntos de concentración de errores.

   > [!NOTE]
   > Si los nodos del clúster están conectados con una única red, la red pasará el requisito de redundancia de validar un asistente de configuración.  Sin embargo, el informe incluirá una advertencia de que la red no debe tener un único punto de error.

- **Controladores de dispositivo o adaptadores apropiados para el almacenamiento:**
    - **Canal de fibra o SCSI había conectado en serie:** Si está utilizando SCSI conectado en serie o Fibre Channel, todos los componentes de la pila de almacenamiento deberían ser idénticos en todos los servidores en clúster. Es necesario que el software de múltiples rutas (MPIO) de E/S y los componentes de software del módulo específico de dispositivo (DSM) sean idénticos.  Se recomienda que los controladores de dispositivos de almacenamiento, es decir, el adaptador de bus host (HBA), los controladores HBA y el firmware de HBA que están conectados al almacenamiento de clúster sean idénticos. Si utiliza unos HBA distintos, debería comprobar con el proveedor de almacenamiento que está siguiendo sus configuraciones compatibles o recomendadas.
    - **iSCSI:** Si está usando iSCSI, cada servidor agrupado debe tener uno o más adaptadores de red o adaptadores de bus host dedicados al almacenamiento ISCSI. La red que utiliza para iSCSI no se puede utilizar para la comunicación de red. En todos los servidores en clúster, los adaptadores de red que usa para conectar con el destino de almacenamiento de iSCSI deben ser idénticos y se recomienda utilizar Gigabit Ethernet o superior.  

- **Almacenamiento:** Debe usar almacenamiento compartido que está certificada para Windows Server 2016 o Windows Server 2019.
  
    Para un clúster de conmutación por error de dos nodos, el almacenamiento debe contener al menos dos volúmenes independientes (LUN) si usa un disco testigo de quórum. El disco testigo es un disco del almacenamiento de clúster designado para mantener una copia de la base de datos de configuración del clúster. En este ejemplo de clúster de dos nodos, la configuración de quórum será la mayoría de disco y nodo. Nodo y mayoría de disco significa que los nodos y el disco testigo contienen copias de la configuración del clúster, y el clúster tiene quórum siempre y cuando esté disponible una mayoría (dos de tres) de estas copias. El otro volumen (LUN) contendrá los archivos que se comparten con los usuarios.

    Los requisitos de almacenamiento incluyen lo siguiente:

    - Para usar el soporte de disco nativo incluido en el clúster de conmutación por error, utilice discos básicos, no discos dinámicos.
    - Se recomienda formatear las particiones con NTFS (para el disco testigo, la partición debe ser NTFS).
    - Para el estilo de partición del disco, puede usar el registro de arranque maestro (MBR) o la tabla de particiones GUID (GPT).
    - El almacenamiento debe responder correctamente a comandos SCSI específicos, el almacenamiento debe seguir el estándar denominado SCSI Primary Commands-3 (SPC-3). En concreto, el almacenamiento debe admitir reservas persistentes como se especifica en el estándar SPC-3. 
    -  El minicontrolador de puerto utilizado para el almacenamiento debe funcionar con el controlador de almacenamiento de Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implementación de redes de área de almacenamiento con clústeres de conmutación por error

Al implementar una red de área de almacenamiento (SAN) con un clúster de conmutación por error, deben tenerse en cuenta las siguientes directrices.

- **Confirme la certificación del almacenamiento:** Mediante el [Windows Server Catalog](https://www.windowsservercatalog.com/default.aspx) de sitio, confirme el almacenamiento del proveedor, los controladores, firmware y software, está certificada para Windows Server 2016 o Windows Server 2019.

- **Aislar dispositivos de almacenamiento, un clúster por dispositivo:** los servidores de clústeres diferentes no deben poder tener acceso a los mismos dispositivos de almacenamiento. En la mayoría de los casos, un LUN que se usa para un conjunto de servidores de clúster se debe aislar de todos los demás servidores a través de máscaras o zonas de LUN.

- **Considere el uso de software de E/S de múltiples rutas:** en un tejido de almacenamiento de alta disponibilidad, puede implementar los clústeres de conmutación por error con varios adaptadores de bus host utilizando software de E/S de múltiples rutas. Esto proporciona el mayor nivel de redundancia y disponibilidad. La solución de múltiples rutas debe estar basada en E/S de múltiples rutas de Microsoft (MPIO). El proveedor de hardware de almacenamiento puede proporcionar un módulo de específico del dispositivo (DSM) MPIO para el hardware, aunque Windows Server 2016 y Windows Server 2019 incluyen uno o más DSM como parte del sistema operativo.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Requisitos de infraestructura de red y cuentas de dominio

Para un clúster de conmutación por error de dos nodos, necesitará la siguiente infraestructura de red y una cuenta administrativa con los siguientes permisos de dominio:

- **Configuración de red y direcciones IP:** cuando use adaptadores de red idénticos para una red, utilice también una configuración de comunicaciones idéntica en esos adaptadores (por ejemplo, Velocidad, Modo dúplex, Control de flujo y Tipo de medios). Además, compare la configuración entre el adaptador de red y el conmutador al que se conecta, y asegúrese de que ninguna configuración está en conflicto.

    Si tiene redes privadas que no se enrutan al resto de su infraestructura de red, asegúrese de que cada una de estas redes privadas usa una subred única. Esto es necesario aunque asigne una dirección IP única a cada adaptador de red. Por ejemplo, si tiene un nodo de clúster en una oficina central que usa una red física, y otro nodo en una sucursal que utiliza una red física independiente, no especifique 10.0.0.0/24 para ambas redes, incluso aunque asigne una dirección IP única a cada adaptador.

    Para obtener más información acerca de los adaptadores de red, consulte los requisitos de Hardware para un clúster de conmutación por error de dos nodos, anteriormente en esta guía.

- **DNS:** los servidores del clúster deben usar el Sistema de nombres de dominio (DNS) para la resolución de nombres. Se puede utilizar el protocolo de actualización dinámica DNS.

- **Rol de dominio:** todos los servidores del clúster deben estar en el mismo dominio de Active Directory. Como procedimiento recomendado, todos los servidores en clúster deben tener el mismo rol de dominio (a sea servidor miembro o controlador de dominio). El rol recomendado es el de servidor miembro.

- **Controlador de dominio:** se recomienda que sus servidores en clúster sean servidores miembro. Si lo son, necesita un servidor adicional que actúe como controlador de dominio en el dominio que contiene el clúster de conmutación por error.

- **Clientes:** Tal y como se necesita para las pruebas, puede conectar uno o más clientes en red al clúster de conmutación por error que cree y observar el efecto en un cliente al mover o conmutar por error el servidor de archivos en clúster de un nodo de clúster al otro.

- **Cuenta para administrar el clúster:** al crear un clúster por primera vez o agregarle servidores, debe haber iniciado sesión en el dominio con una cuenta que tenga derechos y permisos de administrador en todos los servidores de ese clúster. La cuenta no tiene por qué ser una cuenta de administrador de dominio; puede ser una cuenta de usuarios de dominio que pertenezca al grupo Administradores de cada servidor en clúster. Además, si la cuenta no es una cuenta de administradores de dominio, la cuenta (o el grupo que la cuenta es miembro) debe proporcionarse la **crear objetos de equipo** y **leer todas las propiedades** permisos en el unidad organizativa de dominio (OU) que es residirá en.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Pasos para instalar un clúster de servidores de archivos de dos nodos

Debe completar los pasos siguientes para instalar un clúster de conmutación por error de servidor de archivos de dos nodos.

Paso 1: Conectar los servidores del clúster a las redes y almacenamiento

Paso 2: Instalar la característica de clúster de conmutación por error

Paso 3: Validar la configuración de clústeres

Paso 4: Crear el clúster

Si ya ha instalado los nodos del clúster y desea configurar un clúster de conmutación por error de servidor de archivos, consulte los pasos para configurar un clúster de servidor de archivos de dos nodos, más adelante en esta guía.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Paso 1: Conectar los servidores del clúster a las redes y almacenamiento

Para una red de clúster de conmutación por error, evite tener puntos de concentración de errores. Hay varias maneras de lograr esto. Puede conectar los nodos del clúster mediante varias redes distintas. O bien, puede conectar sus nodos de clúster con una red creada con adaptadores de red unidos, conmutadores redundantes, enrutadores redundantes o hardware similar que quite los puntos de concentración de errores (si utiliza una red para iSCSI, debe crear esta red además de las demás redes).

En el caso de un clúster de servidores de archivos de dos nodos, al conectar los servidores al almacenamiento de clúster, debe exponer dos volúmenes (LUN) por lo menos. Puede exponer volúmenes adicionales según sea necesario para realizar pruebas en profundidad de su configuración. No exponga los volúmenes en clúster a servidores que no estén en el clúster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Para conectar los servidores del clúster a las redes y almacenamiento

1. Revise los detalles acerca de las redes en los requisitos de Hardware para un clúster de conmutación por error de dos nodos y los requisitos de cuenta de red infraestructura y el dominio para un clúster de conmutación por error de dos nodos, anteriormente en esta guía.

2. Conecte y configure las redes que los servidores del clúster utilizarán.

3. Si la configuración de pruebas incluye clientes o un controlador de dominio no agrupado, asegúrese de que estos equipos pueden conectar a los servidores en clúster a través de, al menos, una red.

4. Siga las instrucciones del fabricante para conectar físicamente los servidores al almacenamiento .

5. Asegúrese de que los discos (LUN) que desea utilizar en el clúster se exponen en los servidores que agrupará (y solo en esos servidores). Puede usar cualquiera de las siguientes interfaces para exponer discos o LUN:

    - La interfaz proporcionada por el fabricante del almacenamiento.

    - Si utiliza iSCSI, una interfaz iSCSI adecuada.

6. Si ha comprado software que controla el formato o la función del disco, siga las instrucciones del proveedor sobre cómo usar ese software con Windows Server.

7. En uno de los servidores que desea que el clúster y, a continuación, haga clic en Inicio, haga clic en Herramientas administrativas, haga clic en administración de equipos y, a continuación, haga clic en administración de discos. (Si aparece el cuadro de diálogo Control de cuentas de usuario, confirme que la acción que se muestra es lo que desea y, a continuación, haga clic en continuar.) En administración de discos, confirme que están visibles los discos de clúster.

8. Si desea tener un volumen de almacenamiento mayor de 2 terabytes y está utilizando la interfaz de Windows para controlar el formato del disco, convierta ese disco al estilo de partición denominado tabla de particiones GUID (GPT). Para ello, copia de seguridad de los datos en el disco, elimine todos los volúmenes en el disco y, a continuación, en administración de discos, haga clic en el disco (no en una partición) y haga clic en convertir en disco GPT.  Para los volúmenes menores de 2 terabytes, en lugar de usar GPT, puedes usar el estilo de partición denominado registro de arranque maestro (MBR).

9. Compruebe el formato de cualquier volumen o LUN expuesto. Se recomienda usar NTFS para el formato (para el disco testigo, debe utilizar NTFS).

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Paso 2: Instalar la característica de clúster de conmutación por error y el rol de servidor de archivo

En este paso, se instalará la característica de clúster de conmutación por error y el rol de servidor de archivo. Ambos servidores deben ejecutar Windows Server 2016 o Windows Server 2019.

#### <a name="using-server-manager"></a>Con Administrador del servidor

1. Abra **administrador del servidor** y, en el **administrar** lista desplegable, seleccione **agregar Roles y características**.

   ![Agregar función](media\Cluster-File-Server\Cluster-FS-Add-Feature.png)

2. Si el **antes de comenzar** abre la ventana, elija **siguiente**.

3. Para el **tipo de instalación**, seleccione **instalación basada en roles o basada en características** y **siguiente**.

4. Asegúrese de **seleccionar un servidor del grupo de servidores** está seleccionada, se resalta el nombre de la máquina, y **siguiente**.

5. Para el rol de servidor, en la lista de roles, abra **servicios de archivo**, seleccione **servidor de archivos**, y **siguiente**.

   ![Agregar rol](media\Cluster-File-Server\Cluster-FS-Add-FS-Role-1.png)

6. Para las características de la lista de características, seleccione **agrupación en clústeres de conmutación por error**.  Se mostrará un cuadro de diálogo emergente que muestra las herramientas de administración también está instaladas.  Guardar todos los seleccionados, elija **agregar características** y **siguiente**.

   ![Agregar función](media\Cluster-File-Server\Cluster-FS-Add-WSFC-1.png)

7. En la página de confirmación, seleccione instalar.

8. Una vez finalizada la instalación, reinicie el equipo.

9. Repita los pasos en el segundo equipo.

#### <a name="using-powershell"></a>Con PowerShell

1. Abra una sesión de PowerShell administrativa haciendo clic en el botón Inicio y, a continuación, seleccione **Windows PowerShell (Admin)**.
2. Para instalar el rol de servidor de archivos, ejecute el comando:

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Para instalar la característica clúster de conmutación por error y sus herramientas de administración, ejecute el comando:

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Una vez que han completado, puede comprobar que se instalan con los comandos:

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Una vez comprobado que se instalan, reinicie el equipo con el comando:

    ```PowerShell
    Restart-Computer
    ```

6. Repita los pasos en el segundo servidor.

### <a name="step-3-validate-the-cluster-configuration"></a>Paso 3: Validar la configuración de clústeres

Antes de crear un clúster, se recomienda encarecidamente que valide su configuración. La validación le ayuda a confirmar que la configuración de sus servidores, red y almacenamiento cumple un conjunto de requisitos concretos para los clústeres de conmutación por error.

#### <a name="using-failover-cluster-manager"></a>Usar el Administrador de clústeres de conmutación por error

1. Desde **administrador del servidor**, elija el **herramientas** lista desplegable y seleccione **Administrador de clústeres de conmutación por error**.

2. En **Administrador de clústeres de conmutación por error**, vaya a la columna central en **administración** y elija **validar configuración**.

3. Si el **antes de comenzar** abre la ventana, elija **siguiente**.

4. En el **seleccionar servidores o un clúster** ventana, agregue los nombres de los dos equipos que serán los nodos del clúster.  Por ejemplo, si los nombres son Nodo1 y Nodo2, escriba el nombre y seleccione **agregar**.  También puede elegir el **examinar** botón para buscar los nombres de Active Directory.  Una vez que ambos se muestran bajo **servidores seleccionados**, elija **siguiente**.

5. En el **opciones de pruebas** ventana, seleccione **ejecutar todas las pruebas (recomendadas)**, y **siguiente**.

6. En el **confirmación** página, le proporcionará la lista de todas las pruebas que comprobará.  Elija **siguiente** comenzarán a las pruebas.

7. Una vez completado, el **resumen** página aparece después de ejecutarán las pruebas. Para ver los temas de Ayuda que le ayudarán a interpretar los resultados, haga clic en **Más información acerca de las pruebas de validación de clúster**.

8. Mientras sigue en la página Resumen, haga clic en Ver informe y leer los resultados de pruebas. Realice los cambios necesarios en la configuración y vuelva a ejecutar las pruebas. <br>Para ver los resultados de las pruebas después de cerrar el asistente, consulte *fecha del informe SystemRoot\Cluster\Reports\Validation and time.html*.

9. Para ver los temas de ayuda sobre la validación de clúster después de cerrar al asistente, en administración de clúster de conmutación por error, haga clic en Ayuda, haga clic en los temas de ayuda, haga clic en la ficha contenido, expanda el contenido de la Ayuda de clúster de conmutación por error y haga clic en validar una configuración de clúster de conmutación por error .

#### <a name="using-powershell"></a>Con PowerShell

1. Abra una sesión de PowerShell administrativa haciendo clic en el botón Inicio y, a continuación, seleccione **Windows PowerShell (Admin)**.

2. Para validar las máquinas (por ejemplo, los nombres de máquina que se va a Nodo1 y Nodo2) para la agrupación en clústeres de conmutación por error, ejecute el comando:

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Para ver los resultados de las pruebas después de cerrar el asistente, consulte el archivo especificado (en SystemRoot\Cluster\Reports\), a continuación, realice los cambios necesarios en la configuración y vuelva a ejecutar las pruebas.

Para obtener más información, consulte [validar una configuración de clúster de conmutación por error](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Paso 4: Crear el clúster

El siguiente creará un clúster fuera de las máquinas y la configuración que tiene.

#### <a name="using-failover-cluster-manager"></a>Usar el Administrador de clústeres de conmutación por error

1. Desde **administrador del servidor**, elija el **herramientas** lista desplegable y seleccione **Administrador de clústeres de conmutación por error**.

2. En **Administrador de clústeres de conmutación por error**, vaya a la columna central en **administración** y elija **crear clúster**.

3. Si el **antes de comenzar** abre la ventana, elija **siguiente**.

4. En el **seleccionar servidores** ventana, agregue los nombres de los dos equipos que serán los nodos del clúster.  Por ejemplo, si los nombres son Nodo1 y Nodo2, escriba el nombre y seleccione **agregar**.  También puede elegir el **examinar** botón para buscar los nombres de Active Directory.  Una vez que ambos se muestran bajo **servidores seleccionados**, elija **siguiente**.

5. En el **punto de acceso para administrar el clúster** ventana, escriba el nombre del clúster que se va a usar.  Tenga en cuenta que esto no es el nombre que se va a usar para conectarse a los recursos compartidos de archivos con.  Esto es para simplemente administrar el clúster.

   > [!NOTE]
   > Si usa direcciones IP estáticas, deberá seleccionar la red para usar y escriba la dirección IP utilizará para el nombre del clúster.  Si usa DHCP para sus direcciones IP, la dirección IP se configurarán automáticamente para usted.

6. Elija **siguiente**.

7. En el **confirmación** página, compruebe lo que ha configurado y seleccione **siguiente** para crear el clúster.

8. En el **resumen** página, se le ofrecerá la configuración ha creado.  Puede seleccionar Ver informe para ver el informe de la creación.

#### <a name="using-powershell"></a>Con PowerShell

1. Abra una sesión de PowerShell administrativa haciendo clic en el botón Inicio y, a continuación, seleccione **Windows PowerShell (Admin)**.

2. Ejecute el siguiente comando para crear el clúster si usa direcciones IP estáticas.  Por ejemplo, los nombres de equipo son Nodo1 y Nodo2, el nombre del clúster será el clúster y la dirección IP será 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Ejecute el siguiente comando para crear el clúster si usa DHCP para las direcciones IP.  Por ejemplo, los nombres de equipo son Nodo1 y Nodo2 y el nombre del clúster será el clúster.

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Pasos para configurar un clúster de conmutación por error de servidor de archivos

Para configurar un clúster de conmutación por error de servidor de archivos, siga los pasos siguientes.

1. Desde **administrador del servidor**, elija el **herramientas** lista desplegable y seleccione **Administrador de clústeres de conmutación por error**.

2. Cuando se abre el Administrador de clústeres de conmutación por error, traerá automáticamente el nombre del clúster que creó.  Si no es así, vaya a la columna central en **administración** y elija **conectar al clúster**.  Escriba el nombre del clúster que creó y **Aceptar**.

3. En el árbol de consola, haga clic en el ">" situado junto a la del clúster que creó para expandir los elementos debajo de ella.

4. Haga clic derecho del ratón en **Roles** y seleccione **configurar rol**.

5. Si el **antes de comenzar** abre la ventana, elija **siguiente**.

6. En la lista de roles, elija **servidor de archivos** y **siguiente**.

7. El tipo de servidor de archivo, seleccione **servidor de archivos para uso general** y **siguiente**.<br>Para obtener información sobre el servidor de archivos de escalabilidad horizontal, consulte [Introducción a Scale-Out File Server](sofs-overview.md).

   ![Tipo de servidor de archivos](media\Cluster-File-Server\Cluster-FS-File-Server-Type.png)

8. En el **punto de acceso cliente** ventana, escriba el nombre del servidor de archivos que se va a usar.  Tenga en cuenta que esto no es el nombre del clúster.  Esto es para la conectividad de recurso compartido de archivos.  Por ejemplo, si desea conectarse a \\SERVER, el nombre de la entrada sería SERVER.

   > [!NOTE]
   > Si usa direcciones IP estáticas, deberá seleccionar la red para usar y escriba la dirección IP utilizará para el nombre del clúster.  Si usa DHCP para sus direcciones IP, la dirección IP se configurarán automáticamente para usted.

6. Elija **siguiente**.

7. En el **Seleccionar almacenamiento** ventana, seleccione la unidad adicional (no el testigo) que contendrá los recursos compartidos y **siguiente**.

8. En el **confirmación** página, compruebe la configuración y seleccione **siguiente**.

9. En el **resumen** página, se le ofrecerá la configuración ha creado.  Puede seleccionar Ver informe para ver el informe de la creación de rol de servidor de archivos.

10. En **Roles** en el árbol de consola, verá la nueva función que creó enumeradas como el nombre que creó.  Con resaltado, en el **acciones** panel de la derecha, elija **agregar un recurso compartido**.

11. Ejecute el Asistente de recurso compartido de escribir lo siguiente:

    - Tipo de recurso compartido será
    - Ubicación y ruta de acceso será la carpeta compartida
    - El nombre de los usuarios del recurso compartido se conectará a
    - Otras opciones como el acceso según la enumeración, almacenamiento en caché, el cifrado, etcetera
    - Permisos de nivel de archivo si sean distintos de los valores predeterminados

12. En el **confirmación** página, compruebe lo que ha configurado y seleccione **crear** para crear el recurso compartido de archivos.

13. En el **resultados** , seleccione Cerrar si crea el recurso compartido.  Si no se pudo crear el recurso compartido, le proporcionará los errores producidos.

14. Elija **cerrar**.

15. Repita este proceso para los recursos compartidos adicionales.