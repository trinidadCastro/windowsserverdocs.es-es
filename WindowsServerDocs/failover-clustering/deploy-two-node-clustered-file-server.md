---
title: Implementación de un servidor de archivos en clúster de dos nodos
description: En este artículo se describe cómo crear un clúster de servidores de archivos de dos nodos
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 02/01/2019
ms.openlocfilehash: 1f5c3dfadc295caa6f3232c9cb1e98b3ed7861c1
ms.sourcegitcommit: d1815253b47e776fb96a3e91556fd231bef8ee6d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99042531"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Implementación de un servidor de archivos en clúster de dos nodos

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad de las aplicaciones y los servicios. Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno de los nodos del clúster, otro comienza a dar servicio (proceso que se denomina conmutación por error). Los usuarios experimentarán un número de interrupciones mínimo en el servicio.

En esta guía se describen los pasos para instalar y configurar un clúster de conmutación por error de servidor de archivos de uso general que tiene dos nodos. Al crear la configuración de esta guía, puede obtener información sobre los clústeres de conmutación por error y familiarizarse con la interfaz del complemento Administración del clúster de conmutación por error en Windows Server 2019 o Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Información general para un clúster de servidores de archivos de dos nodos

Los servidores de un clúster de conmutación por error pueden funcionar en diversos roles, incluidos los roles del servidor de archivos, el servidor de Hyper-V o el servidor de base de datos, y pueden proporcionar alta disponibilidad para otros servicios y aplicaciones. En esta guía se describe cómo configurar un clúster de servidores de archivos de dos nodos.

Un clúster de conmutación por error normalmente incluye una unidad de almacenamiento que está conectada físicamente a todos los servidores del clúster, aunque solo un servidor tiene acceso cada vez a cualquier volumen determinado el almacenamiento. En el siguiente diagrama se muestra un clúster de conmutación por error de dos nodos conectado a una unidad de almacenamiento.

![Clúster de dos nodos](media/Cluster-File-Server/Cluster-FS-Overview.png)

Los volúmenes de almacenamiento o números de unidad lógica (LUN) expuestos a los nodos de un clúster no se deben exponer a otros servidores, incluidos los servidores de otro clúster. En el diagrama siguiente se ilustra esto.

![LUN en almacenamiento](media/Cluster-File-Server/Cluster-FS-LUNs.png)

Tenga en cuenta que para la disponibilidad máxima de cualquier servidor, es importante seguir los procedimientos recomendados de administración del servidor, por ejemplo, administrar con cuidado el entorno físico de los servidores, probar los cambios de software antes de implementarlos por completo y realizar un cuidadoso seguimiento de las actualizaciones de software y los cambios de configuración en todos los servidores en clúster.

El escenario siguiente describe cómo se puede configurar un clúster de conmutación por error de servidor de archivos. Los archivos que se están compartiendo están en el almacenamiento de clúster y cualquier servidor en clúster puede actuar como servidor de archivos que los comparte.

## <a name="shared-folders-in-a-failover-cluster"></a>Carpetas compartidas en un clúster de conmutación por error

En la siguiente lista se describe la funcionalidad de configuración de carpetas compartidas que se integra en los clústeres de conmutación por error:

- La visualización solo está orientada a carpetas compartidas en clúster (no mezclar con carpetas compartidas no en clúster): cuando un usuario ve carpetas compartidas mediante la especificación de la ruta de acceso de un servidor de archivos en clúster, la presentación incluirá solo las carpetas compartidas que forman parte del rol de servidor de archivos específico. Excluirá las carpetas compartidas no en clúster y los recursos compartidos que formen parte de roles de servidor de archivos independientes que se encuentren en un nodo del clúster.

- Enumeración basada en el acceso: puede usar la enumeración basada en el acceso para ocultar una carpeta especificada de la vista de los usuarios. En lugar de permitir a los usuarios ver la carpeta pero no tener acceso a nada de ella, puede elegir impedir que vean la carpeta en su totalidad. Puede configurar la enumeración basada en el acceso para una carpeta compartida en clúster de la misma manera que para una carpeta compartida no en clúster.

- Acceso sin conexión: puede configurar el acceso sin conexión (almacenamiento en caché) para una carpeta compartida en clúster de la misma manera que para una carpeta compartida no agrupada.

- Los discos en clúster siempre se reconocen como parte del clúster: Si usa la interfaz de clúster de conmutación por error, el explorador de Windows o el complemento Administración de almacenamiento y recursos compartidos, Windows reconoce si un disco se ha designado como en el almacenamiento de clúster. Si este disco ya se ha configurado en la administración de clúster de conmutación por error como parte de un servidor de archivos en clúster, puede usar cualquiera de las interfaces mencionadas anteriormente para crear un recurso compartido en el disco. Si ese tipo de disco no se ha configurado como parte de un servidor de archivos en clúster, no puede crear por error un recurso compartido en él. En su lugar, un error indica que el disco se debe configurar primero como parte de un servidor de archivos en clúster antes de que se pueda compartir.

- Integración de servicios para Network File System: el rol servidor de archivos en Windows Server incluye el servicio de rol opcional denominado servicios para Network File System (NFS). Instalando el servicio de rol y configurando carpetas compartidas con Servicios para NFS, puede crear un servidor de archivos en clúster que admita clientes basados en UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Requisitos para un clúster de conmutación por error de dos nodos

Para un clúster de conmutación por error en Windows Server 2016 o Windows Server 2019 para que Microsoft considere una solución oficialmente compatible con Microsoft, la solución debe cumplir los siguientes criterios.

- Todos los componentes de hardware y software deben cumplir con los requisitos del logotipo que corresponda. En Windows Server 2016, este es el logotipo "certificado para Windows Server 2016". En Windows Server 2019, este es el logotipo "certificado para Windows Server 2019". Para obtener más información acerca de los sistemas de software y hardware que se han certificado, visite el sitio del catálogo de Microsoft [Windows Server](https://www.windowsservercatalog.com/default.aspx) .

- La solución completamente configurada (servidores, red y almacenamiento) debe pasar todas las pruebas del Asistente para validación, que forma parte del complemento clúster de conmutación por error.

Se necesitará lo siguiente para un clúster de conmutación por error de dos nodos.

- **Servidores:** Se recomienda usar equipos coincidentes con el mismo componente o con componentes similares.  Los servidores de un clúster de conmutación por error de dos nodos deben ejecutar la misma versión de Windows Server. También deben tener las mismas actualizaciones de software (revisiones).

- **Adaptadores de red y cable:** El hardware de red, como otros componentes de la solución de clúster de conmutación por error, debe ser compatible con Windows Server 2016 o Windows Server 2019. Si usa iSCSI, los adaptadores de red deben estar dedicados a la comunicación de red o iSCSI, no a ambos. En la infraestructura de red que conecta los nodos de clúster, evite tener puntos de concentración de errores. Hay varias maneras de lograr esto. Puede conectar los nodos del clúster mediante varias redes distintas. Alternativamente, puede conectar los nodos del clúster con una red construida con adaptadores de red en equipo, conmutadores redundantes, enrutadores redundantes u otro hardware similar que elimine los puntos de concentración de errores.

   > [!NOTE]
   > Si los nodos del clúster están conectados a una red única, la red pasará el requisito de redundancia en el Asistente para validar una configuración.  Sin embargo, el informe incluirá una advertencia de que la red no debe tener un único punto de error.

- **Controladores de dispositivo o adaptadores adecuados para el almacenamiento:**
    - **SCSI conectado en serie o canal de fibra:** Si usa SCSI conectado en serie o Canal de fibra, en todos los servidores en clúster, todos los componentes de la pila de almacenamiento deben ser idénticos. Es necesario que el software de e/s de múltiples rutas (MPIO) y los componentes de software del módulo específico del dispositivo (DSM) sean idénticos.  Se recomienda que los controladores de dispositivos de almacenamiento, es decir, el adaptador de bus host (HBA), los controladores HBA y el firmware de HBA que están conectados al almacenamiento de clúster sean idénticos. Si utiliza unos HBA distintos, debería comprobar con el proveedor de almacenamiento que está siguiendo sus configuraciones compatibles o recomendadas.
    - **iSCSI:** Si usa iSCSI, cada servidor en clúster debe tener uno o varios adaptadores de red o adaptadores de bus host dedicados al almacenamiento ISCSI. La red que utiliza para iSCSI no se puede utilizar para la comunicación de red. En todos los servidores en clúster, los adaptadores de red que usa para conectar con el destino de almacenamiento de iSCSI deben ser idénticos y se recomienda utilizar Gigabit Ethernet o superior.

- **Almacenamiento:** Debe usar el almacenamiento compartido que está certificado para Windows Server 2016 o Windows Server 2019.

    En el caso de un clúster de conmutación por error de dos nodos, el almacenamiento debe contener al menos dos volúmenes independientes (LUN) si usa un disco testigo para el cuórum. El disco testigo es un disco del almacenamiento de clúster designado para mantener una copia de la base de datos de configuración del clúster. En este ejemplo de clúster de dos nodos, la configuración de quórum será mayoría de disco y nodo. Mayoría de disco y nodo significa que los nodos y el disco testigo contienen cada uno copias de la configuración del clúster y el clúster tiene quórum siempre y cuando esté disponible una mayoría (dos de tres) de estas copias. El otro volumen (LUN) contendrá los archivos que se comparten con los usuarios.

    Entre los requisitos de almacenamiento se incluyen:

    - Para usar el soporte de disco nativo incluido en el clúster de conmutación por error, utilice discos básicos, no discos dinámicos.
    - Se recomienda formatear las particiones con NTFS (para el disco testigo, la partición debe ser NTFS).
    - Para el estilo de partición del disco, puede usar el registro de arranque maestro (MBR) o la tabla de particiones GUID (GPT).
    - El almacenamiento debe responder correctamente a comandos SCSI específicos; el almacenamiento debe seguir el estándar denominado comandos principales SCSI-3 (SPC-3). En concreto, el almacenamiento debe admitir reservas persistentes como se especifica en el estándar SPC-3.
    -  El minicontrolador de puerto utilizado para el almacenamiento debe funcionar con el controlador de almacenamiento de Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Implementación de redes de área de almacenamiento con clústeres de conmutación por error

Al implementar una red de área de almacenamiento (SAN) con un clúster de conmutación por error, deben cumplirse las siguientes directrices.

- **Confirme la certificación del almacenamiento:** Con el sitio del [Catálogo de Windows Server](https://www.windowsservercatalog.com/default.aspx) , confirme que el almacenamiento del proveedor, incluidos los controladores, el firmware y el software, está certificado para windows Server 2016 o windows Server 2019.

- **Aísle los dispositivos de almacenamiento, un clúster por dispositivo:** Los servidores de clústeres diferentes no deben poder tener acceso a los mismos dispositivos de almacenamiento. En la mayoría de los casos, un LUN que se usa para un conjunto de servidores de clúster se debe aislar de todos los demás servidores a través de máscaras o zonas de LUN.

- **Considere la posibilidad de usar software de e/s de múltiples rutas:** En un tejido de almacenamiento de alta disponibilidad, puede implementar clústeres de conmutación por error con varios adaptadores de bus host mediante el software de e/s de múltiples rutas. Esto proporciona el mayor nivel de redundancia y disponibilidad. La solución de múltiples rutas debe estar basada en e/s de múltiples rutas (MPIO) de Microsoft. El fabricante del hardware de almacenamiento puede proporcionar un módulo específico del dispositivo (DSM) MPIO para el hardware, aunque Windows Server 2016 y Windows Server 2019 incluyen uno o más DSM como parte del sistema operativo.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Requisitos de infraestructura de red y cuentas de dominio

Para un clúster de conmutación por error de dos nodos, necesitará la siguiente infraestructura de red y una cuenta administrativa con los siguientes permisos de dominio:

- **Configuración de red y direcciones IP:** Cuando se usan adaptadores de red idénticos para una red, también se usa una configuración de comunicaciones idéntica en esos adaptadores (por ejemplo, velocidad, modo dúplex, control de flujo y tipo de medio). Además, compare la configuración entre el adaptador de red y el conmutador al que se conecta, y asegúrese de que ninguna configuración está en conflicto.

    Si tiene redes privadas que no se enrutan al resto de su infraestructura de red, asegúrese de que cada una de estas redes privadas usa una subred única. Esto es necesario aunque asigne una dirección IP única a cada adaptador de red. Por ejemplo, si tiene un nodo de clúster en una oficina central que usa una red física, y otro nodo en una sucursal que utiliza una red física independiente, no especifique 10.0.0.0/24 para ambas redes, incluso aunque asigne una dirección IP única a cada adaptador.

    Para obtener más información sobre los adaptadores de red, vea Requisitos de hardware para un clúster de conmutación por error de dos nodos, anteriormente en esta guía.

- **DNS:** Los servidores del clúster deben usar el sistema de nombres de dominio (DNS) para la resolución de nombres. Se puede utilizar el protocolo de actualización dinámica DNS.

- **Rol de dominio:** Todos los servidores del clúster deben estar en el mismo dominio Active Directory. Como procedimiento recomendado, todos los servidores en clúster deben tener el mismo rol de dominio (a sea servidor miembro o controlador de dominio). El rol recomendado es el de servidor miembro.

- **Controlador de dominio:** Se recomienda que los servidores en clúster sean servidores miembro. Si lo son, necesita un servidor adicional que actúe como controlador de dominio en el dominio que contiene el clúster de conmutación por error.

- **Clientes:** Según sea necesario para las pruebas, puede conectar uno o más clientes en red al clúster de conmutación por error que cree y observar el efecto en un cliente cuando se mueve o conmuta por error el servidor de archivos en clúster de un nodo de clúster al otro.

- **Cuenta para administrar el clúster:** Al crear un clúster por primera vez o agregarle servidores, debe haber iniciado sesión en el dominio con una cuenta que tenga derechos de administrador y permisos en todos los servidores de ese clúster. La cuenta no tiene por qué ser una cuenta de administrador de dominio; puede ser una cuenta de usuarios de dominio que pertenezca al grupo Administradores de cada servidor en clúster. Además, si la cuenta no es una cuenta de Admins. del dominio, la cuenta (o el grupo del que la cuenta es miembro) debe tener asignados los permisos **crear objetos de equipo** y **leer todas las propiedades** en la unidad organizativa (OU) del dominio que se encuentra en.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Pasos para instalar un clúster de servidores de archivos de dos nodos

Debe completar los pasos siguientes para instalar un clúster de conmutación por error de servidor de archivos de dos nodos.

Paso 1: conexión de los servidores de clúster a las redes y almacenamiento

Paso 2: instalar la característica de clúster de conmutación por error

Paso 3: validar la configuración del clúster

Paso 4: Creación del clúster

Si ya ha instalado los nodos de clúster y desea configurar un clúster de conmutación por error de servidor de archivos, vea Pasos para configurar un clúster de servidores de archivos de dos nodos, más adelante en esta guía.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Paso 1: conexión de los servidores de clúster a las redes y almacenamiento

Para una red de clúster de conmutación por error, evite tener puntos de concentración de errores. Hay varias maneras de lograr esto. Puede conectar los nodos del clúster mediante varias redes distintas. O bien, puede conectar sus nodos de clúster con una red creada con adaptadores de red unidos, conmutadores redundantes, enrutadores redundantes o hardware similar que quite los puntos de concentración de errores (si utiliza una red para iSCSI, debe crear esta red además de las demás redes).

En el caso de un clúster de servidores de archivos de dos nodos, al conectar los servidores al almacenamiento de clúster, debe exponer dos volúmenes (LUN) por lo menos. Puede exponer volúmenes adicionales según sea necesario para realizar pruebas en profundidad de su configuración. No exponga los volúmenes en clúster a servidores que no estén en el clúster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Para conectar los servidores del clúster a las redes y almacenamiento

1. Revise los detalles sobre las redes en requisitos de hardware para un clúster de conmutación por error de dos nodos e infraestructura de red y requisitos de cuenta de dominio para un clúster de conmutación por error de dos nodos, anteriormente en esta guía.

2. Conecte y configure las redes que los servidores del clúster utilizarán.

3. Si la configuración de pruebas incluye clientes o un controlador de dominio no agrupado, asegúrese de que estos equipos pueden conectar a los servidores en clúster a través de, al menos, una red.

4. Siga las instrucciones del fabricante para conectar físicamente los servidores al almacenamiento .

5. Asegúrese de que los discos (LUN) que desea utilizar en el clúster se exponen en los servidores que agrupará (y solo en esos servidores). Puede usar cualquiera de las siguientes interfaces para exponer discos o LUN:

    - La interfaz proporcionada por el fabricante del almacenamiento.

    - Si utiliza iSCSI, una interfaz iSCSI adecuada.

6. Si ha comprado software que controla el formato o la función del disco, siga las instrucciones del proveedor sobre cómo usar ese software con Windows Server.

7. En uno de los servidores que desea poner en clúster, haga clic sucesivamente en Inicio, Herramientas administrativas, Administración de equipos y Administración de discos. (Si aparece el cuadro de diálogo control de cuentas de usuario, confirme que la acción que se muestra es la que desea y, a continuación, haga clic en continuar). En administración de discos, confirme que los discos del clúster están visibles.

8. Si desea tener un volumen de almacenamiento mayor de 2 terabytes y está utilizando la interfaz de Windows para controlar el formato del disco, convierta ese disco al estilo de partición denominado tabla de particiones GUID (GPT). Para ello, haga copia de seguridad de los datos del disco, elimine todos los volúmenes del disco y, en Administración de discos, haga clic con el botón secundario en el disco (no en una partición) y haga clic en Convertir en disco GPT.  Para los volúmenes menores de 2 terabytes, en lugar de usar GPT, puedes usar el estilo de partición denominado registro de arranque maestro (MBR).

9. Compruebe el formato de cualquier volumen o LUN expuesto. Se recomienda usar NTFS para el formato (para el disco testigo, debe utilizar NTFS).

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Paso 2: instalar el rol de servidor de archivos y la característica de clúster de conmutación por error

En este paso, se instalará el rol de servidor de archivos y la característica de clúster de conmutación por error. Ambos servidores deben ejecutar Windows Server 2016 o Windows Server 2019.

#### <a name="using-server-manager"></a>Usar Administrador del servidor

1. Abra **Administrador del servidor** y, en la lista desplegable **administrar** , seleccione **Agregar roles y características**.

   ![Captura de pantalla de la lista desplegable administrar de Administrador del servidor con la opción Agregar roles y características resaltada.](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. Si se abre la ventana **antes de comenzar** , elija **siguiente**.

3. En **tipo de instalación**, seleccione Instalación basada en **características o en roles** y **siguiente**.

4. Asegúrese de **seleccionar un servidor del grupo de servidores** está seleccionado, el nombre del equipo está resaltado y **siguiente**.

5. Para el rol de servidor, en la lista de roles, Abra **servicios de archivo**, seleccione servidor de **archivos** y **siguiente**.

   ![Captura de pantalla de la página roles del servidor del cuadro de diálogo Agregar roles y características que muestra la opción servidor de archivos seleccionada y resaltada.](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. Para las características, en la lista de características, seleccione **clúster de conmutación por error**.  Aparecerá un cuadro de diálogo emergente en el que se enumeran las herramientas de administración que también se están instalando.  Mantenga todos los seleccionados, elija **Agregar características** y **siguiente**.

   ![Incorporación de características](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. En la página Confirmación, seleccione Instalar.

8. Una vez finalizada la instalación, reinicie el equipo.

9. Repita los pasos en el segundo equipo.

#### <a name="using-powershell"></a>Usar PowerShell

1. Abra una sesión de PowerShell de administración haciendo clic con el botón derecho en el botón Inicio y seleccionando **Windows PowerShell (admin)**.
2. Para instalar el rol de servidor de archivos, ejecute el comando:

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Para instalar la característica de clúster de conmutación por error y sus herramientas de administración, ejecute el comando:

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Una vez que se han completado, puede comprobar que se instalan con los comandos:

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Una vez que haya comprobado que están instalados, reinicie el equipo con el comando:

    ```PowerShell
    Restart-Computer
    ```

6. Repita los pasos en el segundo servidor.

### <a name="step-3-validate-the-cluster-configuration"></a>Paso 3: validar la configuración del clúster

Antes de crear un clúster, se recomienda encarecidamente que valide su configuración. La validación le ayuda a confirmar que la configuración de sus servidores, red y almacenamiento cumple un conjunto de requisitos concretos para los clústeres de conmutación por error.

#### <a name="using-failover-cluster-manager"></a>Usar el Administrador de clústeres de conmutación por error

1. En **Administrador del servidor**, elija la lista desplegable **herramientas** y seleccione **Administrador de clústeres de conmutación por error**.

2. En **Administrador de clústeres de conmutación por error**, vaya a la columna central en **Administración** y elija **validar configuración**.

3. Si se abre la ventana **antes de comenzar** , elija **siguiente**.

4. En la ventana **seleccionar servidores o un clúster** , agregue los nombres de los dos equipos que serán los nodos del clúster.  Por ejemplo, si los nombres son NODO1 y NODO2, escriba el nombre y seleccione **Agregar**.  También puede elegir el botón **examinar** para buscar los nombres de Active Directory.  Una vez que ambos se muestran en **servidores seleccionados**, elija **siguiente**.

5. En la ventana **Opciones de prueba** , seleccione **ejecutar todas las pruebas (recomendado)** y **siguiente**.

6. En la página **confirmación** , se mostrará la lista de todas las pruebas que comprobará.  Elija **siguiente** y se iniciarán las pruebas.

7. Una vez finalizada, la página **Resumen** aparece después de ejecutar las pruebas. Para ver los temas de Ayuda que le ayudarán a interpretar los resultados, haga clic en **Más información acerca de las pruebas de validación de clúster**.

8. Todavía en la página Resumen, haga clic en Ver informe y lea los resultados de las pruebas. Realice los cambios necesarios en la configuración y vuelva a ejecutar las pruebas. <br>Para ver los resultados de las pruebas después de cerrar el asistente, vea *SystemRoot\Cluster\Reports\Validation Report Date and time.html*.

9. Para ver los temas de Ayuda sobre la validación del clúster después de cerrar el asistente, en Administración de clúster de conmutación por error, haga clic sucesivamente en Ayuda, Temas de Ayuda y en la pestaña Contenido, expanda el contenido correspondiente a la Ayuda de clúster de conmutación por error y, a continuación, haga clic en Validar una configuración de clúster de conmutación por error.

#### <a name="using-powershell"></a>Usar PowerShell

1. Abra una sesión de PowerShell de administración haciendo clic con el botón derecho en el botón Inicio y seleccionando **Windows PowerShell (admin)**.

2. Para validar las máquinas (por ejemplo, los nombres de equipo que son el NODO1 y el NODO2) para los clústeres de conmutación por error, ejecute el comando:

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Para ver los resultados de las pruebas después de cerrar el asistente, vea el archivo especificado (en SystemRoot\Cluster\Reports \) , realice los cambios necesarios en la configuración y vuelva a ejecutar las pruebas.

Para obtener más información, consulte [validar una configuración de clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Paso 4: crear el clúster

Lo siguiente creará un clúster fuera de los equipos y la configuración que tiene.

#### <a name="using-failover-cluster-manager"></a>Usar el Administrador de clústeres de conmutación por error

1. En **Administrador del servidor**, elija la lista desplegable **herramientas** y seleccione **Administrador de clústeres de conmutación por error**.

2. En **Administrador de clústeres de conmutación por error**, vaya a la columna central en **Administración** y elija **crear clúster**.

3. Si se abre la ventana **antes de comenzar** , elija **siguiente**.

4. En la ventana **seleccionar servidores** , agregue los nombres de los dos equipos que serán los nodos del clúster.  Por ejemplo, si los nombres son NODO1 y NODO2, escriba el nombre y seleccione **Agregar**.  También puede elegir el botón **examinar** para buscar los nombres de Active Directory.  Una vez que ambos se muestran en **servidores seleccionados**, elija **siguiente**.

5. En la ventana **punto de acceso para administrar el clúster** , escriba el nombre del clúster que va a usar.  Tenga en cuenta que no es el nombre que va a usar para conectarse a los recursos compartidos de archivos con.  Esto es para administrar simplemente el clúster.

   > [!NOTE]
   > Si utiliza direcciones IP estáticas, debe seleccionar la red que se va a usar y especificar la dirección IP que utilizará para el nombre del clúster.  Si usa DHCP para las direcciones IP, la dirección IP se configurará automáticamente.

6. Elija **Siguiente**.

7. En la página **confirmación** , compruebe lo que ha configurado y seleccione **siguiente** para crear el clúster.

8. En la página **Resumen** , le proporcionará la configuración que ha creado.  Puede seleccionar ver informe para ver el informe de la creación.

#### <a name="using-powershell"></a>Usar PowerShell

1. Abra una sesión de PowerShell de administración haciendo clic con el botón derecho en el botón Inicio y seleccionando **Windows PowerShell (admin)**.

2. Ejecute el siguiente comando para crear el clúster si utiliza direcciones IP estáticas.  Por ejemplo, los nombres de equipo son NODO1 y NODO2, el nombre del clúster será CLUSTER y la dirección IP será la de la.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Ejecute el siguiente comando para crear el clúster si usa DHCP para direcciones IP.  Por ejemplo, los nombres de equipo son NODO1 y NODO2, y el nombre del clúster será CLUSTER.

   ```PowerShell
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Pasos para configurar un clúster de conmutación por error de servidor de archivos

Para configurar un clúster de conmutación por error de servidor de archivos, siga estos pasos.

1. En **Administrador del servidor**, elija la lista desplegable **herramientas** y seleccione **Administrador de clústeres de conmutación por error**.

2. Cuando se abre Administrador de clústeres de conmutación por error, debe traer automáticamente el nombre del clúster que ha creado.  Si no es así, vaya a la columna central bajo **Administración** y elija **conectar al clúster**.  Escriba el nombre del clúster que ha creado y **Aceptar**.

3. En el árbol de consola, haga clic en el signo ">" situado junto al clúster que creó para expandir los elementos que se encuentran debajo de él.

4. Haga clic con el botón derecho en **roles** y seleccione **configurar rol**.

5. Si se abre la ventana **antes de comenzar** , elija **siguiente**.

6. En la lista de roles, elija **servidor de archivos** y **siguiente**.

7. En tipo de servidor de archivos, seleccione **servidor de archivos para uso general** y **siguiente**.<br>Para obtener información sobre Scale-Out servidor de archivos, consulte [información general sobre servidor de archivos de escalabilidad horizontal](sofs-overview.md).

   ![Tipo de servidor de archivos](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. En la ventana **punto de acceso cliente** , escriba el nombre del servidor de archivos que va a usar.  Tenga en cuenta que no es el nombre del clúster.  Esto es para la Conectividad del recurso compartido de archivos.  Por ejemplo, si deseo conectarse al \\ servidor, el nombre que se proporciona sería servidor.

   > [!NOTE]
   > Si utiliza direcciones IP estáticas, debe seleccionar la red que se va a usar y especificar la dirección IP que utilizará para el nombre del clúster.  Si usa DHCP para las direcciones IP, la dirección IP se configurará automáticamente.

9. Elija **Siguiente**.

10. En la ventana **seleccionar almacenamiento** , seleccione la unidad adicional (no el testigo) que contendrá los recursos compartidos y haga clic en **siguiente**.

11. En la página **confirmación** , Compruebe la configuración y seleccione **siguiente**.

12. En la página **Resumen** , le proporcionará la configuración que ha creado.  Puede seleccionar ver informe para ver el informe de la creación del rol de servidor de archivos.

   > [!NOTE]
   > Si el rol no se agrega o se inicia correctamente, puede que el CNO (objeto de nombre de clúster) no tenga permiso para crear objetos en Active Directory. El rol servidor de archivos requiere un objeto de equipo con el mismo nombre que el "punto de acceso de cliente" proporcionado en el paso 8.

13. En **roles** en el árbol de consola, verá el nuevo rol que creó como el nombre que creó.  Con esta opción resaltada, en el panel **acciones** de la derecha, elija **Agregar un recurso compartido**.

14. Ejecute el Asistente para compartir que incluye lo siguiente:

    - Tipo de recurso compartido que será
    - La ubicación o ruta de acceso de la carpeta compartida será
    - El nombre del recurso compartido al que se conectarán los usuarios
    - Opciones de configuración adicionales, como la enumeración basada en el acceso, el almacenamiento en caché, el cifrado, etc.
    - Permisos de nivel de archivo si son distintos de los predeterminados

15. En la página **confirmación** , compruebe lo que ha configurado y seleccione **crear** para crear el recurso compartido de servidor de archivos.

16. En la página **resultados** , seleccione cerrar Si creó el recurso compartido.  Si no pudo crear el recurso compartido, le proporcionará los errores que se han producido.

17. Elija **Cerrar**.

18. Repita este proceso para todos los recursos compartidos adicionales.
