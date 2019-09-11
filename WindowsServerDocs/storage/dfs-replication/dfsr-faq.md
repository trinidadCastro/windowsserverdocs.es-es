---
title: 'Replicación DFS: Preguntas más frecuentes'
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 5bf85938f242ec29d75b32cdcb8b03c5f34bd1bb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871974"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replicación DFS: Preguntas más frecuentes


Actualizado: 30 de abril de 2019

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Estas preguntas más frecuentes responden a preguntas acerca de la replicación de Sistema de archivos distribuido (DFS) (también conocida como DFS-R o DFSR) para Windows Server.

Para obtener información sobre los espacios de nombres DFS, vea [Espacios de nombres DFS: Preguntas](https://technet.microsoft.com/library/ee404780)más frecuentes.

Para obtener información sobre las novedades de Replicación DFS, vea los temas siguientes:

  - [Información general sobre espacios de nombres DFS y replicación DFS](https://technet.microsoft.com/library/jj127250) (en Windows Server 2012)  
      
  - [Novedades de sistema de archivos distribuido](https://technet.microsoft.com/library/ee307957) tema en [cambios de funcionalidad de Windows Server 2008 a Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - [Sistema de archivos distribuido](https://technet.microsoft.com/library/cc753479) tema de [los cambios de funcionalidad de Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Para ver una lista de los cambios recientes realizados en este tema, consulte la sección [Historial de cambios](#change-history) de este tema.

      

## <a name="interoperability"></a>Interoperabilidad

### <a name="can-dfs-replication-communicate-with-frs"></a>¿Puede Replicación DFS comunicarse con FRS?

No. Replicación DFS no se comunica con el servicio de replicación de archivos (FRS). Replicación DFS y FRS pueden ejecutarse en el mismo servidor al mismo tiempo, pero nunca deben estar configurados para replicar las mismas carpetas o subcarpetas, ya que esto puede provocar la pérdida de datos.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Replicación DFS puede reemplazar FRS para la replicación de SYSVOL

Sí, Replicación DFS puede reemplazar FRS para la replicación de SYSVOL en servidores que ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Los servidores que ejecutan Windows Server 2003 R2 no admiten el uso de Replicación DFS para replicar la carpeta SYSVOL.

Para obtener más información acerca de la replicación de SYSVOL mediante replicación DFS, [consulte la guía de migración de replicación de SYSVOL: FRS que se](https://technet.microsoft.com/library/dd640019)va a replicación DFS.

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>¿Puedo actualizar de FRS a Replicación DFS sin perder la configuración?

Sí. Para migrar la replicación de FRS a Replicación DFS, consulte los siguientes documentos:

  - Para migrar la replicación de carpetas que no sean la carpeta Sysvol [, consulte la guía de operaciones de DFS: Migración de FRS a replicación DFS](http://go.microsoft.com/fwlink/?linkid=192776) y [FRS2DFSR: una utilidad de migración de FRS a DFSR](http://go.microsoft.com/fwlink/?linkid=195437) (. http://go.microsoft.com/fwlink/?LinkID=195437)  
      
  - Para migrar la replicación de la carpeta Sysvol a replicación DFS, [consulte la guía de migración de replicación de SYSVOL: FRS que se](https://technet.microsoft.com/library/dd640019)va a replicación DFS.  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>¿Puedo usar Replicación DFS en un entorno mixto de Windows o UNIX?

Sí. Aunque Replicación DFS solo admite la replicación de contenido entre servidores que ejecutan Windows Server, los clientes de UNIX pueden tener acceso a recursos compartidos de archivos en los servidores de Windows. Para ello, instale servicios para Network File System (NFS) en el servidor de Replicación DFS.

También puede usar la funcionalidad de cliente SMB/CIFS incluida en muchos clientes UNIX para acceder directamente a los recursos compartidos de archivos de Windows, aunque esta funcionalidad suele ser limitada o requiere modificaciones en el entorno de Windows (por ejemplo, deshabilitar la firma SMB mediante Directiva de grupo).

Replicación DFS interopera con NFS en un servidor que ejecuta un sistema operativo Windows Server, pero no puede replicar un punto de montaje de NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>¿Puedo usar el Servicio de instantáneas de volumen con Replicación DFS?

Sí. Replicación DFS es compatible con los volúmenes de Servicio de instantáneas de volumen (VSS) y las instantáneas anteriores se pueden restaurar correctamente con el cliente de versiones anteriores.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>¿Puedo usar copias de seguridad de Windows (NTBackup. exe) para realizar una copia de seguridad de una carpeta replicada de forma remota?

No, no se admite el uso de copias de seguridad de Windows (NTBackup. exe) en un equipo que ejecute Windows Server 2003 o una versión anterior para realizar una copia de seguridad del contenido de una carpeta replicada en un equipo que ejecute Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Para realizar una copia de seguridad de los archivos que se almacenan en una carpeta replicada, use Copias de seguridad de Windows Server o Microsoft® System Center Data Protection Manager. Para obtener información acerca de la funcionalidad de copia de seguridad y recuperación en Windows Server 2008 R2 y Windows Server 2008, consulte [copia de seguridad y recuperación](https://technet.microsoft.com/library/Cc754097). Para obtener más información, consulte [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>¿Afectan las directivas del sistema de archivos Replicación DFS?

Sí. No configure las directivas del sistema de archivos en las carpetas replicadas. La Directiva del sistema de archivos vuelve a aplicar los permisos NTFS cada directiva de grupo intervalo de actualización. Esto puede dar lugar a infracciones de uso compartido porque un archivo abierto no se replica hasta que se cierra el archivo.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>¿Replicación DFS Replica buzones hospedados en Microsoft Exchange Server?

No. No se puede usar Replicación DFS para replicar buzones hospedados en Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>¿Replicación DFS admiten los filtros de archivos creados por el servidor de archivos Administrador de recursos?

Sí. Sin embargo, la configuración del filtrado de archivos del Administrador de recursos del servidor de archivos (FSRM) debe coincidir con los dos extremos de la replicación. Además, Replicación DFS tiene su propio mecanismo de filtro para archivos y carpetas que puede usar para excluir determinados archivos y tipos de archivo de la replicación.

A continuación se muestran los procedimientos recomendados para implementar filtros de archivos o cuotas:

  - La carpeta DfsrPrivate oculta no debe estar sujeta a cuotas ni filtros de archivos.  
      
  - Los archivos filtrados no deben existir en ninguna carpeta replicada antes de habilitar la comprobación.  
      
  - Ninguna carpeta puede superar la cuota antes de habilitar la cuota.  
      
  - Debe utilizar las cuotas fuertes con precaución. Es posible que los miembros individuales de un grupo de replicación permanezcan dentro de una cuota antes de la replicación, pero lo superan cuando se replican los archivos. Por ejemplo, si un usuario copia un archivo de 10 megabytes (MB) en el servidor A (que, a continuación, se encuentra en el límite máximo) y otro usuario copia un archivo de 5 MB en el servidor B, cuando se produzca la siguiente replicación, los dos servidores superarán la cuota de 5 megabytes. Esto puede hacer que Replicación DFS reintente continuamente la replicación de los archivos, lo que provocará lagunas en el vector de versión y posibles problemas de rendimiento.  
      

### <a name="is-dfs-replication-cluster-aware"></a>¿Es Replicación DFS compatible con clústeres?

Sí, Replicación DFS en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 incluyen la posibilidad de agregar un clúster de conmutación por error como miembro de un grupo de replicación. Para obtener más información, vea [Agregar un clúster de conmutación por error a un grupo de replicación](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). El servicio Replicación DFS de las versiones de Windows anteriores a Windows Server 2008 R2 no está diseñado para coordinarse con un clúster de conmutación por error y el servicio no conmutará por error a otro nodo.


> [!NOTE]
> Replicación DFS no admite la replicación de archivos en volúmenes compartidos de clúster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>¿Es Replicación DFS compatible con la desduplicación de datos?

Sí, Replicación DFS pueden replicar carpetas en volúmenes que usan la desduplicación de datos en Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>¿Es Replicación DFS compatible con RIS y WDS?

Sí. Replicación DFS Replica volúmenes en los que está habilitado el almacenamiento de instancia única (SIS). Los servicios de instalación remota (RIS), los servicios de implementación de Windows (WDS) y Windows Storage Server usan SIS.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>¿Es posible usar Replicación DFS con Archivos sin conexión?

Puede usar Replicación DFS y Archivos sin conexión de forma segura en escenarios en los que solo haya un usuario a la vez que escriba en los archivos. Esto resulta útil para los usuarios que viajan entre dos sucursales y desean tener acceso a sus archivos en cualquier rama o mientras están sin conexión. Archivos sin conexión almacena en caché los archivos localmente para su uso sin conexión y Replicación DFS replica los datos entre cada sucursal.

No utilice Replicación DFS con Archivos sin conexión en un entorno de varios usuarios porque Replicación DFS no proporciona ningún mecanismo de bloqueo distribuido o capacidad de desprotección de archivos. Si dos usuarios modifican el mismo archivo al mismo tiempo en servidores diferentes, replicación DFS mueve el archivo anterior a la carpeta\\ConflictandDeleted de DfsrPrivate (que se encuentra en la ruta de acceso local de la carpeta replicada) durante la siguiente replicación.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>¿Qué aplicaciones antivirus son compatibles con Replicación DFS?

Las aplicaciones antivirus pueden provocar una replicación excesiva si sus actividades de exploración modifican los archivos de una carpeta replicada. Para obtener más información, [pruebe la interoperabilidad de las aplicaciones antivirus con replicación DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>¿Cuáles son las ventajas de usar Replicación DFS en lugar de Windows SharePoint Services?

Windows® SharePoint® Services proporciona coherencia estrecha en forma de funcionalidad de desprotección de archivos que Replicación DFS no. Si le preocupa que varias personas modifiquen el mismo archivo, se recomienda usar Windows SharePoint Services. Windows SharePoint Services 2,0 con Service Pack 2 está disponible como parte de Windows Server 2003 R2. Windows SharePoint Services se puede descargar desde el sitio web de Microsoft; no se incluye en las versiones más recientes de Windows Server. Sin embargo, si va a replicar datos en varios sitios y los usuarios no editarán los mismos archivos al mismo tiempo, Replicación DFS proporciona un mayor ancho de banda y una administración más sencilla.

## <a name="limitations-and-requirements"></a>Limitaciones y requisitos

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>¿Replicación DFS puede replicar entre sucursales sin una conexión VPN?

Sí, suponiendo que hay un vínculo de red de área extensa (WAN) privado (no Internet) que conecta las sucursales. Sin embargo, debe abrir los puertos adecuados en los firewalls externos. Replicación DFS utiliza el asignador de extremos de RPC (Puerto 135) y un puerto efímero asignado aleatoriamente por encima de 1024. Puede usar la herramienta de línea de comandos **Dfsrdiag** para especificar un puerto estático en lugar del puerto efímero. Para obtener más información acerca de cómo especificar el asignador de extremos de RPC, consulte el [artículo 154596](http://go.microsoft.com/fwlink/?linkid=73991) en http://go.microsoft.com/fwlink/?LinkId=73991) Microsoft Knowledge base (.

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>¿Replicación DFS replicar los archivos cifrados con el Sistema de cifrado de archivos?

No. Replicación DFS no replicará los archivos o carpetas cifrados mediante el Sistema de cifrado de archivos (EFS). Si un usuario cifra un archivo que se ha replicado anteriormente, Replicación DFS elimina el archivo de todos los demás miembros del grupo de replicación. Esto garantiza que la única copia disponible del archivo sea la versión cifrada en el servidor.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>¿Replicación DFS puede replicar archivos de base de datos de Outlook. PST o Microsoft Office Access?

Replicación DFS puede replicar de forma segura archivos de carpeta personal de Microsoft Outlook (. pst) y archivos de Microsoft Access solo si se almacenan para fines de archivado y no se tiene acceso a ellos a través de la red mediante un cliente como Outlook o Access (para abrir. PST o Access) archivos, copie primero los archivos en un dispositivo de almacenamiento local). Las razones para esto son las siguientes:

  - Abrir archivos. pst a través de conexiones de red podría provocar daños en los datos de los archivos. pst. Para obtener más información acerca de por qué no se puede tener acceso de forma segura a los archivos. pst desde una red, vea el http://go.microsoft.com/fwlink/?LinkId=125363) [artículo 297019](http://go.microsoft.com/fwlink/?linkid=125363) en Microsoft Knowledge base (.  
      
  - los archivos. PST y Access tienden a permanecer abiertos durante largos períodos de tiempo mientras se accede a ellos mediante un cliente como Outlook u Office Access. Esto impide que Replicación DFS repliquen estos archivos hasta que se cierren.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>¿Puedo usar Replicación DFS en un grupo de trabajo?

No. Replicación DFS confía en Active Directory® servicios de dominio para la configuración. Solo funcionará en un dominio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>¿Se puede replicar más de una carpeta en un solo servidor?

Sí. Replicación DFS puede replicar varias carpetas entre servidores. Asegúrese de que cada una de las carpetas replicadas tiene una ruta de acceso raíz única y que no se superponen. Por ejemplo, d:\\sales y d:\\Accounting puede ser la ruta de acceso raíz de dos carpetas replicadas, pero\\d: sales y\\d\\: los informes de ventas no pueden ser las rutas de acceso raíz de dos carpetas replicadas.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>¿Replicación DFS requiere espacios de nombres DFS?

No. Replicación DFS y espacios de nombres DFS se pueden usar por separado o juntos. Además, Replicación DFS se puede usar para replicar espacios de nombres DFS independientes, que no eran posibles con FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>¿Replicación DFS requiere sincronización de tiempo entre servidores?

No. Replicación DFS no requiere explícitamente la sincronización de la hora entre los servidores. Sin embargo, Replicación DFS requiere que los relojes del servidor coincidan de cerca. Los relojes del servidor deben establecerse en un plazo de cinco minutos (de forma predeterminada) para que la autenticación Kerberos funcione correctamente. Por ejemplo, Replicación DFS usa marcas de tiempo para determinar qué archivo tiene prioridad en caso de conflicto. Los tiempos precisos también son importantes para la recolección de elementos no utilizados, programaciones y otras características.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>¿Replicación DFS admite la replicación de un volumen completo?

Sí. Sin embargo, primero debe instalar Windows Server 2003 Service Pack 2 o la revisión. Para obtener más información, vea el [artículo 920335](http://go.microsoft.com/fwlink/?linkid=76776) de Microsoft Knowledge base http://go.microsoft.com/fwlink/?LinkId=76776) (. Además, la replicación de un volumen completo puede producir los problemas siguientes:

  - Si el volumen contiene un archivo de paginación de Windows, se produce un error en la replicación y se registra el evento 4312 de DFSR en el registro de eventos del sistema.  
      
  - Replicación DFS establece el sistema y los atributos ocultos en la carpeta replicada en los servidores de destino. Esto se debe a que Windows aplica de forma predeterminada el sistema y los atributos ocultos a la carpeta raíz del volumen. Si la ruta de acceso local de la carpeta replicada en los servidores de destino también es una raíz de volumen, no se realizarán más cambios en los atributos de carpeta.  
      
  - Al replicar un volumen que contiene la carpeta del sistema de Windows, Replicación DFS reconoce la carpeta% WINDIR% y no la replica. Sin embargo, Replicación DFS Replica carpetas utilizadas por aplicaciones que no son de Microsoft, lo que puede provocar errores en las aplicaciones en los servidores de destino si las aplicaciones tienen problemas de interoperabilidad con Replicación DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>¿Replicación DFS admite RPC a través de HTTP?

No.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>¿Replicación DFS funciona a través de redes inalámbricas?

Sí. Replicación DFS es independiente del tipo de conexión.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>¿Replicación DFS funciona en volúmenes ReFS o FAT?

No. Replicación DFS solo admite volúmenes formateados con el sistema de archivos NTFS; no se admiten el sistema de archivos resistente (ReFS) ni el sistema de archivos FAT. Replicación DFS requiere NTFS porque utiliza el diario de cambios de NTFS y otras características del sistema de archivos NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>¿Replicación DFS funciona con archivos dispersos?

Sí. Puede replicar archivos dispersos. El atributo **Sparse** se conserva en el miembro receptor.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>¿Es necesario iniciar sesión como administrador para replicar archivos?

No. Replicación DFS es un servicio que se ejecuta en la cuenta de sistema local, por lo que no es necesario iniciar sesión como administrador para replicar. Sin embargo, debe ser administrador de dominio o administrador local de los servidores de archivos afectados para realizar cambios en la configuración de Replicación DFS.

Para obtener más información, vea "Replicación DFS requisitos de seguridad y delegación" en la sección delegación [de la capacidad de administrar replicación DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>¿Cómo puedo actualizar o reemplazar un miembro de Replicación DFS?

Para actualizar o reemplazar un miembro de Replicación DFS, consulte esta entrada de blog en el blog del equipo de Ask The Directory Services: [Reemplazar el hardware o el sistema operativo de los miembros de DFSR](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>¿Es Replicación DFS adecuado para replicar perfiles móviles?

Sí. Se admiten ciertos escenarios al replicar perfiles de usuario móviles. Para obtener información acerca de los escenarios admitidos, vea la [declaración de soporte técnico de Microsoft sobre los datos de Perfil de usuario replicados](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>¿Hay un límite de caracteres de archivo o un límite en la profundidad de la carpeta?

Windows y Replicación DFS admiten rutas de acceso de carpeta con un máximo de 32000 caracteres. Replicación DFS no se limita a las rutas de acceso de carpeta de 260 caracteres.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>¿Los miembros de un grupo de replicación deben residir en el mismo dominio?

No. Los grupos de replicación pueden abarcar varios dominios dentro de un solo bosque, pero no en bosques distintos.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>¿Cuáles son los límites admitidos de Replicación DFS?

La lista siguiente proporciona un conjunto de instrucciones de escalabilidad probadas por Microsoft en Windows Server 2012 R2:

  - Tamaño de todos los archivos replicados en un servidor: 100 terabytes.  
      
  - Número de archivos replicados en un volumen: 70 millones.  
      
  - Tamaño máximo de archivo: 250 gigabytes.  
      


> [!IMPORTANT]
> Al crear grupos de replicación con un gran número o tamaño de archivos, se recomienda exportar un clon de la base de datos y usar técnicas de inicialización previa para minimizar la duración de la replicación inicial. Para obtener más información, <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">vea replicación DFS sincronización inicial en Windows Server 2012 R2: Ataque de los clones</A>. 
<br>


La lista siguiente proporciona un conjunto de instrucciones de escalabilidad probadas por Microsoft en Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008:

  - Tamaño de todos los archivos replicados en un servidor: 10 terabytes.  
      
  - Número de archivos replicados en un volumen: 11 millones.  
      
  - Tamaño máximo de archivo: 64 gigabytes.  
      


> [!NOTE]
> Ya no hay un límite en cuanto al número de grupos de replicación, carpetas replicadas, conexiones o miembros del grupo de replicación. 
<br>


Para obtener una lista de las instrucciones de escalabilidad probadas por Microsoft para Windows Server 2003 R2, consulte [instrucciones de escalabilidad de replicación DFS](http://go.microsoft.com/fwlink/?linkid=75043) (. http://go.microsoft.com/fwlink/?LinkId=75043)

### <a name="when-should-i-not-use-dfs-replication"></a>¿Cuándo no se debe utilizar Replicación DFS?

No utilice Replicación DFS en un entorno en el que varios usuarios actualicen o modifiquen los mismos archivos simultáneamente en servidores diferentes. Esto puede hacer que replicación DFS mueva copias en conflicto de los archivos a la carpeta DfsrPrivate\\ConflictandDeleted oculta.

Cuando varios usuarios necesitan modificar los mismos archivos al mismo tiempo en servidores diferentes, utilice la característica de desprotección de archivos de Windows SharePoint Services para asegurarse de que solo un usuario está trabajando en un archivo. Windows SharePoint Services 2,0 con Service Pack 2 está disponible como parte de Windows Server 2003 R2. Windows SharePoint Services se puede descargar desde el sitio web de Microsoft; no se incluye en las versiones más recientes de Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>¿Por qué se requiere una actualización de esquema para Replicación DFS?

Replicación DFS usa nuevos objetos en el contexto de nombres de dominio de Active Directory Domain Services para almacenar información de configuración. Estos objetos se crean al actualizar el esquema de Servicios de dominio de Active Directory. Para obtener más información, vea [revisar los requisitos de replicación DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Herramientas de supervisión y administración

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>¿Puedo automatizar el informe de mantenimiento para recibir advertencias?

Sí. Existen tres formas de automatizar los informes de mantenimiento:

  - Use el módulo de Windows PowerShell de DFSR incluido en Windows Server 2012 R2 o DfsrAdmin. exe junto con las tareas programadas para generar informes de mantenimiento con regularidad. Para obtener más información, vea [automatizar informes de estado de replicación DFS](http://go.microsoft.com/fwlink/?linkid=74010) (. http://go.microsoft.com/fwlink/?LinkId=74010)  
      
  - Utilice el módulo de administración de Replicación DFS para System Center Operations Manager con el fin de crear alertas basadas en condiciones especificadas.  
      
  - Use el proveedor de Replicación DFS WMI para generar scripts de alertas.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>¿Puedo usar Microsoft System Center Operations Manager para supervisar Replicación DFS?

Sí. Para obtener más información, consulte el [módulo de administración de replicación DFS para System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) en el centro http://go.microsoft.com/fwlink/?LinkId=182265) de descarga de Microsoft (.

### <a name="does-dfs-replication-support-remote-management"></a>¿Replicación DFS admite la administración remota?

Sí. Replicación DFS admite la administración remota mediante la consola de administración de DFS y el comando **Agregar grupo de replicación** . Por ejemplo, en el servidor A, puede conectarse a un grupo de replicación definido en el bosque con los servidores A y B como miembros.

La administración de DFS se incluye con Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003 R2. Para administrar Replicación DFS desde otras versiones de Windows, use Escritorio remoto o [herramientas de administración remota del servidor para Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Para ver o administrar los grupos de replicación que contienen carpetas o miembros replicados de solo lectura que son clústeres de conmutación por error, debe usar la versión de administración de DFS que se incluye con Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, el <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">remoto Herramientas de administración del servidor para Windows 8</a>o el <a href="https://technet.microsoft.com/library/ee449475">herramientas de administración remota del servidor para Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>¿Funcionan Ultrasound y Sónar con Replicación DFS?

No. Replicación DFS tiene su propio conjunto de herramientas de supervisión y diagnóstico. Ultrasound y Sónar solo son capaces de supervisar FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>¿Cómo se pueden recuperar los archivos de las carpetas ConflictAndDeleted o preexisting?

Para recuperar los archivos perdidos, restaure los archivos de la carpeta del sistema de archivos o de la carpeta compartida mediante el historial de archivos, el comando **restaurar versiones anteriores** en el explorador de archivos o restaurando los archivos desde la copia de seguridad. Para recuperar archivos directamente desde la carpeta ConflictAndDeleted o preexisting, use `Get-DfsrPreservedFiles` los `Restore-DfsrPreservedFiles` cmdlets de Windows PowerShell y (incluidos con el módulo DFSR en Windows Server 2012 R2) o el script de ejemplo [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) de MSDN Galería de código. Este script está pensado únicamente para la recuperación ante desastres y se proporciona tal cual, sin garantía.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>¿Hay alguna manera de conocer el estado de la replicación?

Sí. Hay varias maneras de supervisar la replicación:

  - Replicación DFS tiene un módulo de administración para System Center Operations Manager que proporciona supervisión proactiva.  
      
  - La administración de DFS tiene un informe de diagnóstico integrado para el trabajo pendiente de replicación, la eficacia de la replicación y el número de archivos y carpetas de un grupo de replicación determinado.  
      
  - El módulo de Windows PowerShell de DFSR en Windows Server 2012 R2 contiene cmdlets para iniciar pruebas de propagación y escribir informes de propagación y estado. Para obtener más información, vea [sistema de archivos distribuido cmdlets de replicación en Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag. exe es una herramienta de línea de comandos que puede generar un recuento de trabajos pendientes o desencadenar una prueba de propagación. Ambos muestran el estado de replicación. La propagación muestra si los archivos se replican en todos los nodos. Trabajo pendiente muestra cuántos archivos se deben replicar antes de que dos equipos estén sincronizados. El recuento de trabajos pendientes es el número de actualizaciones que un miembro del grupo de replicación no ha procesado. En los equipos que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, Dfsrdiag. exe también puede mostrar las actualizaciones que Replicación DFS está replicando actualmente.  
      
  - Los scripts pueden utilizar WMI para recopilar información de trabajos pendientes, manualmente o a través de MOM.  
      

## <a name="performance"></a>Rendimiento

### <a name="does-dfs-replication-support-dial-up-connections"></a>¿Replicación DFS admite conexiones de acceso telefónico?

Aunque Replicación DFS funcionará en velocidades de acceso telefónico, puede obtener una copia de seguridad si hay un gran número de cambios para replicar. Si se realizan pequeños cambios en los archivos existentes, Replicación DFS con compresión diferencial remota (RDC) proporcionará un rendimiento mucho mayor que copiar el archivo directamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>¿Replicación DFS realiza el sensor de ancho de banda?

No. Replicación DFS no realiza el sensor del ancho de banda. Puede configurar Replicación DFS para usar una cantidad limitada de ancho de banda en cada conexión (límite de ancho de banda). Sin embargo, Replicación DFS no reduce aún más el uso de ancho de banda si la interfaz de red se satura y Replicación DFS puede saturar el vínculo durante breves períodos. El límite de ancho de banda con Replicación DFS no es completamente preciso porque Replicación DFS limita el ancho de banda al limitar las llamadas RPC. Como resultado, varios búferes en niveles inferiores de la pila de red (incluido RPC) pueden interferir, lo que provoca ráfagas de tráfico de red.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>¿Replicación DFS limita el ancho de banda por programación, por servidor o por conexión?

Si configura el límite de ancho de banda al especificar la programación, todas las conexiones para ese grupo de replicación usarán esa configuración para la limitación de ancho de banda. El límite de ancho de banda también se puede establecer como una configuración de nivel de conexión mediante administración de DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>¿Replicación DFS usa Active Directory Domain Services para calcular los vínculos a sitios y los costos de conexión?

No. Replicación DFS usa la topología definida por el administrador, que es independiente de los costos de Active Directory Domain Services sitio.

### <a name="how-can-i-improve-replication-performance"></a>¿Cómo puedo mejorar el rendimiento de la replicación?

Para obtener información acerca de los distintos métodos para optimizar el rendimiento de la replicación, vea [optimizar el rendimiento de la replicación en DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) en el [blog del equipo de servicios de directorio](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>¿Cómo Replicación DFS evitar saturar una conexión?

En Replicación DFS se establece el ancho de banda máximo que se desea usar en una conexión y el servicio mantiene ese nivel de uso de la red. Esto es diferente del Servicio de transferencia inteligente en segundo plano (BITS) y Replicación DFS no satura la conexión si se establece correctamente.

Sin embargo, el límite de ancho de banda no es 100% preciso y Replicación DFS puede saturar el vínculo durante breves períodos de tiempo. Esto se debe a que Replicación DFS limita el ancho de banda mediante la limitación de las llamadas RPC. Dado que este proceso se basa en varios búferes en niveles inferiores de la pila de red, incluido RPC, el tráfico de replicación tiende a viajar en ráfagas que, en ocasiones, pueden saturar los vínculos de red.

Replicación DFS en Windows Server 2008 incluye varias mejoras de rendimiento, como se describe en [sistema de archivos distribuido](https://technet.microsoft.com/library/Cc753479), un tema de [cambios en la funcionalidad de Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>¿Cómo se compara Replicación DFS rendimiento con FRS?

Replicación DFS es mucho más rápido que FRS, especialmente cuando se realizan pequeños cambios en archivos de gran tamaño y RDC está habilitado. Por ejemplo, con RDC, un pequeño cambio en una presentación de® de PowerPoint de 2 MB solo puede dar lugar a que solo 60 kilobytes (KB) se envíen a través de la red, lo que supone un ahorro del 97 por ciento en bytes transferidos.

RDC no se utiliza en archivos de menos de 64 KB y es posible que no sea beneficioso en las LAN de alta velocidad en las que el ancho de banda de red no está previsto. RDC se puede deshabilitar en cada conexión mediante administración de DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>¿Con qué frecuencia Replicación DFS replican los datos?

Los datos se replican según la programación establecida. Por ejemplo, puede establecer la programación en intervalos de 15 minutos, siete días a la semana. Durante estos intervalos, se habilita la replicación. La replicación se inicia poco después de que se detecte un cambio de archivo (normalmente en segundos).

La programación del grupo de replicación se puede establecer en horario universal coordinado (UTC), mientras que la programación de la conexión se establece en la hora local del miembro receptor. Tenga esto en cuenta cuando el grupo de replicación abarca varias zonas horarias. Hora local significa la hora del miembro que hospeda la conexión entrante. La programación mostrada de la conexión entrante y la correspondiente conexión saliente reflejan las diferencias de zona horaria cuando la programación se establece en hora local.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>¿Cuánto Replicación DFS consumirán los recursos del sistema de mi servidor?

El disco, la memoria y los recursos de CPU utilizados por Replicación DFS dependen de varios factores, como el número y el tamaño de los archivos, la velocidad de cambio, el número de miembros del grupo de replicación y el número de carpetas replicadas. Además, algunos recursos son más difíciles de calcular. Por ejemplo, la tecnología del motor de almacenamiento extensible (ESE) utilizada para la base de datos de Replicación DFS puede consumir un gran porcentaje de memoria disponible, que se publica a petición. Las aplicaciones que no sean Replicación DFS pueden hospedarse en el mismo servidor en función de la configuración del servidor. Sin embargo, al hospedar varias aplicaciones o roles de servidor en un solo servidor, es importante que pruebe esta configuración antes de implementarla en un entorno de producción.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>¿Qué ocurre si se produce un error en un vínculo WAN durante la replicación?

Si la conexión deja de funcionar, Replicación DFS seguirá intentando realizar la replicación mientras la programación está abierta. También habrá errores de conectividad indicados en el Replicación DFS registro de eventos que se pueden recopilar mediante MOM (de forma proactiva a través de alertas) y el informe de estado de la Replicación DFS (de forma reactiva, como cuando un administrador lo ejecuta).

## <a name="remote-differential-compression-details"></a>Detalles de compresión diferencial remota

### <a name="are-changes-compressed-before-being-replicated"></a>¿Los cambios se comprimen antes de replicarse?

Sí. Las partes modificadas de los archivos se comprimen antes de enviarse para todos los tipos de archivo excepto los siguientes (que ya están comprimidos):. WMA,. wmv,. zip,. jpg,. mpg,. MPEG,. M1V,. MP2,. mp3,. MPa,. cab,. wav,. SND,. au,. ASF,. WM,. avi,. z,. gz,. tgz y. FRx. La configuración de compresión de estos tipos de archivo no es configurable en Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>¿Un administrador puede desactivar RDC o cambiar el umbral?

Sí. Puede desactivar RDC a través de la página de propiedades de una conexión determinada. Deshabilitar RDC puede reducir el uso de la CPU y la latencia de replicación en vínculos de red de área local (LAN) rápidos que no tienen restricciones de ancho de banda o para los grupos de replicación que se componen principalmente de archivos menores de 64 KB. Si decide deshabilitar RDC en una conexión, pruebe la eficacia de la replicación antes y después del cambio para comprobar que ha mejorado el rendimiento de la replicación.

Puede cambiar el umbral de tamaño de RDC mediante el comando de **conjunto de conexiones Dfsradmin** , el replicación DFS proveedor WMI o editando manualmente el archivo XML de configuración.

### <a name="does-rdc-work-on-all-file-types"></a>¿Funciona RDC en todos los tipos de archivo?

Sí. RDC calcula las diferencias en el nivel de bloque, independientemente del tipo de datos de archivo. Sin embargo, RDC funciona de forma más eficaz en determinados tipos de archivo, como documentos de Word, archivos PST e imágenes VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>¿Cómo funciona RDC en un archivo comprimido?

Replicación DFS usa RDC, que calcula los bloques del archivo que han cambiado y envía solo los bloques a través de la red. Replicación DFS no necesita saber nada sobre el contenido del archivo, solo los bloques que han cambiado.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>¿Está habilitado RDC entre archivos al actualizar a Windows Server Enterprise Edition o Datacenter Edition?

Las ediciones Standard de Windows Server no admiten RDC entre archivos. Sin embargo, se habilita automáticamente cuando se actualiza a una edición que admite RDC entre archivos, o si un miembro de la conexión de replicación está ejecutando una edición admitida. Para obtener una lista de las ediciones que admiten RDC entre archivos, consulte ¿Qué ediciones del sistema operativo Windows admiten RDC entre archivos?

### <a name="is-rdc-true-block-level-replication"></a>¿Es una replicación de nivel de bloque real de RDC?

No. RDC es un protocolo de uso general para comprimir la transferencia de archivos. Replicación DFS usa RDC en los bloques en el nivel de archivo, no en el nivel de bloque de disco. RDC divide un archivo en bloques. Para cada bloque de un archivo, calcula una firma, que es un número pequeño de bytes que puede representar el bloque más grande. El conjunto de firmas se transfiere desde el servidor al cliente. El cliente compara las firmas del servidor con las suyas propias. A continuación, el cliente solicita al servidor que envíe solo los datos de las firmas que no estén ya en el cliente.

### <a name="what-happens-if-i-rename-a-file"></a>¿Qué ocurre si se cambia el nombre de un archivo?

Replicación DFS cambia el nombre del archivo en todos los demás miembros del grupo de replicación durante la siguiente replicación. Se realiza el seguimiento de los archivos mediante un identificador único, por lo que cambiar el nombre de un archivo y mover el archivo dentro de la réplica no afecta a la capacidad de Replicación DFS para replicar un archivo.

### <a name="what-is-cross-file-rdc"></a>¿Qué es RDC entre archivos?

RDC entre archivos permite a Replicación DFS usar RDC incluso cuando no existe un archivo con el mismo nombre en el extremo del cliente. RDC entre archivos utiliza una heurística para determinar archivos similares al archivo que se debe replicar y usa bloques de archivos similares que son idénticos al archivo de replicación para minimizar la cantidad de datos transferidos a través de la WAN. RDC entre archivos puede usar bloques de hasta cinco archivos similares en este proceso.

Para usar RDC entre archivos, un miembro de la conexión de replicación debe estar ejecutando una edición de Windows que admita RDC entre archivos. Para obtener una lista de las ediciones que admiten RDC entre archivos, consulte ¿Qué ediciones del sistema operativo Windows admiten RDC entre archivos?

### <a name="what-is-rdc"></a>¿Qué es RDC?

La compresión diferencial remota (RDC) es un protocolo cliente-servidor que se puede usar para actualizar archivos de forma eficaz a través de una red de ancho de banda limitado. RDC detecta inserciones, eliminaciones y reorganizaciones de datos en archivos, lo que permite a Replicación DFS replicar solo los cambios cuando se actualizan los archivos. RDC solo se usa para archivos de 64 KB o más grandes de forma predeterminada. RDC puede usar una versión anterior de un archivo con el mismo nombre en la carpeta replicada o en la carpeta\\DfsrPrivate ConflictandDeleted (que se encuentra en la ruta de acceso local de la carpeta replicada).

### <a name="when-is-rdc-used-for-replication"></a>¿Cuándo se usa RDC para la replicación?

RDC se usa cuando el archivo supera un umbral de tamaño mínimo. Este umbral de tamaño es de 64 KB de forma predeterminada. Una vez replicado un archivo que supere ese umbral, las versiones actualizadas del archivo siempre usan RDC, a menos que se cambie una parte grande del archivo o se deshabilite RDC.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>¿Qué ediciones del sistema operativo Windows admiten RDC entre archivos?

Para usar RDC entre archivos, un miembro de la conexión de replicación debe estar ejecutando una edición del sistema operativo Windows que admita RDC entre archivos. En la tabla siguiente se muestra qué ediciones del sistema operativo Windows admiten RDC entre archivos.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Disponibilidad de RDC entre archivos en las ediciones del sistema operativo Windows

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Versión de sistema operativo</th>
<th>Standard Edition</th>
<th>Enterprise Edition</th>
<th>Datacenter Edition</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Sí<em></p></td>
<td><p>No disponible</p></td>
<td><p>Sí</em></p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Sí</p></td>
<td><p>No disponible</p></td>
<td><p>Sí</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>Sin</p></td>
<td><p>Sí</p></td>
<td><p>Sí</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>Sin</p></td>
<td><p>Sí</p></td>
<td><p>Sin</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>Sin</p></td>
<td><p>Sí</p></td>
<td><p>Sin</p></td>
</tr>
</tbody>
</table>

\*Opcionalmente, puede deshabilitar RDC entre archivos en Windows Server 2012 R2.

## <a name="replication-details"></a>Detalles de la replicación

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>¿Se puede cambiar la ruta de acceso de una carpeta replicada una vez creada?

No. Si necesita cambiar la ruta de acceso de una carpeta replicada, debe eliminarla en administración de DFS y volver a agregarla como una nueva carpeta replicada. A continuación, Replicación DFS usa la compresión diferencial remota (RDC) para realizar una sincronización que determina si los datos son los mismos en los miembros de envío y recepción. No vuelve a replicar todos los datos de la carpeta.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>¿Puedo configurar los atributos de archivo que se van a replicar?

No, no puede configurar los atributos de archivo que Replicación DFS Replica.

Para obtener una lista de valores de atributo y sus descripciones, vea atributos http://go.microsoft.com/fwlink/?LinkId=182268) de [archivo](http://go.microsoft.com/fwlink/?linkid=182268) en MSDN (.

Los siguientes valores de atributo se establecen mediante la `SetFileAttributes dwFileAttributes` función, y se replican mediante replicación DFS. Los cambios en estos valores de atributo desencadenan la replicación de los atributos. El contenido del archivo no se replica a menos que el contenido cambie también. Para obtener más información, vea [función SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) en MSDN Library (http://go.microsoft.com/fwlink/?LinkId=182269).

  - \_ATRIBUTO\_DE ARCHIVO OCULTO  
      
  - \_ATRIBUTO\_DE ARCHIVO READONLY  
      
  - SISTEMA\_DE\_ATRIBUTOS DE ARCHIVO  
      
  - ATRIBUTO\_\_DE ARCHIVONO\_INDIZADODECONTENIDO\_  
      
  - \_ATRIBUTO\_DE ARCHIVO SIN CONEXIÓN  
      

Replicación DFS replica los siguientes valores de atributo, pero no desencadenan la replicación.

  - ARCHIVO\_DE\_ATRIBUTOS DE ARCHIVO  
      
  - \_ATRIBUTO\_DE ARCHIVO NORMAL  
      

Los siguientes valores de atributo de archivo también desencadenan la replicación, aunque no se pueden `SetFileAttributes` establecer mediante la función `GetFileAttributes` (use la función para ver los valores de atributo).

  - PUNTO\_DE\_REANÁLISIS\_DEL ATRIBUTO DE ARCHIVO  
      

> [!NOTE]
> Replicación DFS no replica los valores de atributo de punto de reanálisis a menos que la etiqueta de reanálisis sea IO_REPARSE_TAG_SYMLINK. Los archivos con las etiquetas de reanálisis IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS o IO_REPARSE_TAG_HSM se replican como archivos normales. Sin embargo, los búferes de datos de reanálisis y de etiqueta de reanálisis no se replican en otros servidores porque el punto de reanálisis solo funciona en el sistema local. 
<br>

  - \_ATRIBUTO\_DE ARCHIVO COMPRIMIDO  
      
  - \_ATRIBUTO\_DE ARCHIVO CIFRADO  
      

> [!NOTE]
> Replicación DFS no replica los archivos que se cifran mediante el Sistema de cifrado de archivos (EFS). Replicación DFS Replica archivos que se cifran mediante software que no es de Microsoft, pero solo si no se establece el valor del atributo FILE_ATTRIBUTE_ENCRYPTED en el archivo. 
<br>

  - ARCHIVO\_DISPERSO\_DEATRIBUTODE\_ARCHIVO  
      
  - DIRECTORIO\_DE\_ATRIBUTOS DE ARCHIVO  
      

Replicación DFS no replica el valor temporal\_del\_atributo de archivo.

### <a name="can-i-control-which-member-is-replicated"></a>¿Puedo controlar qué miembro se replica?

Sí. Puede elegir una topología al crear un grupo de replicación. O bien, puede seleccionar **ninguna topología** y configurar manualmente las conexiones una vez creado el grupo de replicación.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>¿Puedo inicializar un miembro del grupo de replicación con datos antes de la replicación inicial?

Sí. Replicación DFS admite la copia de archivos en un miembro del grupo de replicación antes de la replicación inicial. Este "preconfiguración" puede reducir drásticamente la cantidad de datos replicados durante la replicación inicial.

La replicación inicial no necesita replicar contenido cuando los archivos solo se diferencian en los atributos reales o las marcas de tiempo. Un atributo real es un atributo que se puede establecer mediante la función `SetFileAttributes`de Win32. Para obtener más información, vea [función SetFileAttributes](http://go.microsoft.com/fwlink/?linkid=182269) en MSDN Library (http://go.microsoft.com/fwlink/?LinkId=182269). Si dos archivos difieren de otros atributos, como la compresión, se replica el contenido del archivo.

Para preconfigurar un miembro del grupo de replicación, copie los archivos en la carpeta adecuada en los servidores de destino, cree el grupo de replicación y, a continuación, elija un miembro principal. Elija el miembro que tiene los archivos más actualizados que quiere replicar porque el contenido del miembro principal se considera "autoritativo". Esto significa que durante la replicación inicial, los archivos del miembro principal siempre sobrescribirán otras versiones de los archivos en otros miembros del grupo de replicación.

Para obtener información sobre la inicialización previa y la clonación de la base [de datos de DFSR, vea replicación DFS sincronización inicial en Windows Server 2012 R2: Ataque de los clones](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Para obtener más información sobre la replicación inicial, consulte [crear un grupo de replicación](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>¿Replicación DFS supera los problemas comunes del servicio de replicación de archivos?

Sí. Replicación DFS supera tres problemas comunes de FRS:

  - Ajuste del diario: Replicación DFS recupera de los ajustes del diario sobre la marcha. Cada archivo o carpeta existente se marcará como journalWrap y se comprobará en el sistema de archivos antes de que se vuelva a habilitar la replicación. Durante la recuperación, este volumen no está disponible para la replicación en ninguna dirección.  
      
  - Replicación excesiva: Para evitar la replicación excesiva, Replicación DFS usa un sistema de créditos.  
      
  - Carpetas orientadas: Para evitar los nombres de carpetas translocadas, replicación DFS almacena los datos en conflicto\\en una carpeta DfsrPrivate ConflictandDeleted oculta (que se encuentra en la ruta de acceso local de la carpeta replicada). Por ejemplo, al crear varias carpetas al mismo tiempo con nombres idénticos en servidores diferentes replicados con FRS, FRS cambia el nombre de las carpetas más antiguas. En su lugar Replicación DFS mueve las carpetas anteriores a la carpeta conflictos y eliminaciones locales.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>¿Replicación DFS Replica archivos en orden cronológico?

No. Los archivos se pueden replicar desordenados.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>¿Replicación DFS replicar los archivos que se están usando en otra aplicación?

Si una aplicación abre un archivo y crea un bloqueo de archivo (evitando que lo usen otras aplicaciones mientras está abierto), Replicación DFS no replicará el archivo hasta que se cierre. Si la aplicación abre el archivo con acceso de lectura y uso compartido, el archivo todavía se puede replicar.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>¿Replicación DFS replican los permisos de archivos NTFS, los flujos de datos alternativos, los vínculos físicos y los puntos de reanálisis?

  - Replicación DFS replica los permisos de archivo NTFS y los flujos de datos alternativos.  
      
  - Microsoft no admite la creación de vínculos físicos NTFS a o desde archivos en una carpeta replicada; si lo hace, puede producir problemas de replicación con los archivos afectados. Replicación DFS omite los archivos de vínculo físico y no se replican. Los puntos de Unión tampoco se replican y Replicación DFS registra el evento 4406 para cada punto de Unión que encuentra.  
      
  - Los únicos puntos de reanálisis replicados por replicación DFS son aquellos que usan la\_etiqueta SYMLINK\_de\_la etiqueta de reanálisis de e/s; sin embargo, replicación DFS no garantiza que también se replique el destino de un vínculo simbólico. Para obtener más información, consulte el [blog del equipo de servicios de directorio](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Archivos con la etiqueta\_de reanálisis\_de e\_/\_s desduplicación, la etiqueta\_\_de reanálisis\_\_de e/s y las etiquetas de\_reanálisis de HSM de la etiqueta se replican como archivos normales. Los búferes de datos de reanálisis y de etiqueta de reanálisis no se replican en otros servidores porque el punto de reanálisis solo funciona en el sistema local. Por lo tanto, Replicación DFS puede replicar carpetas en volúmenes que usan la desduplicación de datos en Windows Server 2012 o almacenamiento de instancia única (SIS); sin embargo, la información de desduplicación de datos se mantiene por separado en cada servidor en el que está habilitado el servicio de rol.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>¿Replicación DFS replicar los cambios de marca de tiempo si no se realizan otros cambios en el archivo?

No, Replicación DFS no replica los archivos para los que el único cambio es un cambio en la marca de tiempo. Además, la marca de tiempo modificada no se replica en otros miembros del grupo de replicación a menos que se realicen otros cambios en el archivo.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>¿Replicación DFS replicar los permisos actualizados en un archivo o carpeta?

Sí. Replicación DFS replica los cambios de permisos de los archivos y las carpetas. Solo se replica la parte del archivo asociada a la lista de Access Control (ACL), aunque Replicación DFS todavía debe leer el archivo completo en el área de ensayo.


> [!NOTE]
> Cambiar las ACL en un gran número de archivos puede afectar al rendimiento de la replicación. Sin embargo, cuando se usa RDC, la cantidad de datos transferidos es proporcional al tamaño de las ACL, no al tamaño del archivo completo. La cantidad de tráfico de disco sigue siendo proporcional al tamaño de los archivos, ya que los archivos se deben leer y desde la carpeta de almacenamiento provisional. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>¿Replicación DFS admite la combinación de archivos de texto en caso de conflicto?

Replicación DFS no combina archivos cuando hay un conflicto. Sin embargo, intenta conservar la versión anterior del archivo en la carpeta DfsrPrivate\\ConflictandDeleted oculta en el equipo en el que se detectó el conflicto.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>¿Replicación DFS usa el cifrado al transmitir datos?

Sí. Replicación DFS utiliza conexiones RPC (llamada a procedimiento remoto) con cifrado.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>¿Es posible deshabilitar el uso de RPC cifrado?

No. El servicio Replicación DFS usa llamadas a procedimiento remoto (RPC) a través de TCP para replicar los datos. Para proteger las transferencias de datos a través de Internet, el servicio Replicación DFS está diseñado para usar siempre la constante `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`de nivel de autenticación. Esto garantiza que la comunicación RPC a través de Internet esté siempre cifrada. Por lo tanto, no es posible deshabilitar el uso de RPC cifrado por parte del servicio Replicación DFS.

Para obtener más información, vea los siguientes sitios web de Microsoft:

  - [Referencia técnica de RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Acerca de la compresión diferencial remota](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes de nivel de autenticación](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>¿Cómo se administran las replicaciones simultáneas?

Hay un administrador de actualizaciones por cada carpeta replicada. Los administradores de actualizaciones funcionan de forma independiente entre sí.

De forma predeterminada, se comparten un máximo de 16 (cuatro en Windows Server 2003 R2) descargas simultáneas entre todas las conexiones y los grupos de replicación. Dado que las conexiones y las actualizaciones del grupo de replicación no se serializan, no hay ningún orden específico en el que se reciban las actualizaciones. Si se abren dos programaciones, normalmente las actualizaciones se reciben e instalan desde ambas conexiones al mismo tiempo.

### <a name="how-do-i-force-replication-or-polling"></a>¿Cómo forzar la replicación o el sondeo?

Puede forzar la replicación de inmediato mediante administración de DFS, tal como se describe en [Editar programaciones de replicación](https://technet.microsoft.com/library/Cc732278). También puede forzar la replicación mediante el `Sync-DfsReplicationGroup` cmdlet, incluido en el módulo DFSR de PowerShell que se presentó con Windows Server 2012 R2, o el comando **Dfsrdiag SyncNow** . Puede forzar el sondeo mediante el `Update-DfsrConfigurationFromAD` cmdlet o el comando **Dfsrdiag PollAD** .

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>¿Es posible configurar un tiempo de inactividad entre las replicaciones de archivos que cambian con frecuencia?

No. Si la programación está abierta, Replicación DFS replicarán los cambios a medida que los advierte. No hay ninguna manera de configurar un tiempo de inactividad para los archivos.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>¿Es posible configurar la replicación unidireccional con Replicación DFS?

Sí. Si usa Windows Server 2012 o Windows Server 2008 R2, puede crear una carpeta replicada de solo lectura que Replique contenido a través de una conexión unidireccional. Para obtener más información, vea [crear una carpeta replicada de solo lectura en un miembro determinado](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

No se admite la creación de una conexión de replicación unidireccional con Replicación DFS en Windows Server 2008 o Windows Server 2003 R2. Si lo hace, pueden producirse numerosos problemas, como errores de topología de comprobación de estado, problemas de ensayo y problemas con la base de datos de Replicación DFS.

Si usa Windows Server 2008 o Windows Server 2003 R2, puede simular una conexión unidireccional mediante la realización de las siguientes acciones:

  - Entrenar a los administradores para que realicen cambios solo en los servidores que desea designar como servidores principales. A continuación, permita que los cambios se repliquen en los servidores de destino.  
      
  - Configure los permisos de recurso compartido en los servidores de destino para que los usuarios finales no tengan permisos de escritura. Si no se permiten cambios en los servidores de la sucursal, no hay nada que volver a replicar, simulando una conexión unidireccional y manteniendo baja la utilización de la WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>¿Hay alguna manera de forzar una replicación completa de todos los archivos, incluidos los archivos sin cambios?

No. Si Replicación DFS considera que los archivos son idénticos, no los replicará. Si los archivos modificados no se han replicado, Replicación DFS los replicará automáticamente cuando se configure para ello. Para sobrescribir la programación configurada, use el método WMI **ForceReplicate ()** . Sin embargo, esto solo es una invalidación de programación y no fuerza la replicación de archivos no modificados o idénticos.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>¿Qué ocurre si el miembro principal sufre una pérdida de base de datos durante la replicación inicial?

Durante la replicación inicial, los archivos del miembro principal siempre tendrán prioridad en la resolución de conflictos que se produce si los miembros receptores tienen versiones diferentes de los archivos en el miembro principal. La designación del miembro principal se almacena en Active Directory Domain Services y la designación se borra después de que el miembro principal esté listo para la replicación, pero antes de que todos los miembros del grupo de replicación se repliquen.

Si se produce un error en la replicación inicial o el servicio Replicación DFS se reinicia durante la replicación, el miembro principal verá la designación del miembro principal en la base de datos de Replicación DFS local y volverá a intentar la replicación inicial. Si se pierde la base de datos de Replicación DFS del miembro principal después de borrar la designación principal en Active Directory Domain Services, pero antes de que todos los miembros del grupo de replicación completen la replicación inicial, se producirá un error en todos los miembros del grupo de replicación. Replique la carpeta porque no hay ningún servidor designado como miembro principal. Si esto ocurre, use el comando **Dfsradmin Membership/Set/isprimary: true** en el servidor miembro principal para restaurar la designación del miembro principal manualmente.

Para obtener más información sobre la replicación inicial, consulte [crear un grupo de replicación](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> La designación del miembro principal solo se utiliza durante el proceso de replicación inicial. Si usa el comando <STRONG>Dfsradmin</STRONG> para especificar un miembro principal para una carpeta replicada una vez completada la replicación, replicación DFS no designa el servidor como miembro principal en Active Directory Domain Services. Sin embargo, si el Replicación DFS base de datos del servidor sufre daños irreversibles o pérdida de datos, el servidor intenta realizar una replicación inicial como miembro principal en lugar de recuperar los datos de otro miembro de la replicación. agrupamiento. En esencia, el servidor se convierte en un servidor principal no autorizado, lo que puede causar conflictos. Por esta razón, especifique el miembro principal manualmente solo si está seguro de que se ha producido un error irrecuperable en la replicación inicial. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>¿Qué ocurre si la programación de replicación se cierra mientras se replica un archivo?

Si la compresión diferencial remota (RDC) está habilitada en la conexión, la replicación de entrada de un archivo superior a 64 KB que comenzó a replicarse inmediatamente antes del cierre de la programación (o el cambio a **ningún ancho de banda**) continúa cuando se abre la programación (o cambios en algo distinto de **ancho de banda**). La replicación continúa desde el estado en que se encontraba cuando se detuvo la replicación.

Si RDC está desactivado, Replicación DFS reinicia completamente la transferencia de archivos. Esto puede retrasar cuando el archivo está disponible en el miembro receptor.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>¿Qué ocurre cuando dos usuarios actualizan simultáneamente el mismo archivo en diferentes servidores?

Cuando Replicación DFS detecta un conflicto, usa la versión del archivo que se guardó en último lugar. Mueve el otro archivo a la carpeta DfsrPrivate\\ConflictandDeleted (en la ruta de acceso local de la carpeta replicada en el equipo que resolvió el conflicto). Permanece allí hasta que la limpieza de la carpeta conflictos y eliminaciones, que se produce cuando la carpeta conflictos y eliminaciones supera el tamaño configurado o el Replicación DFS encuentra un error de espacio insuficiente en disco. La carpeta conflictos y eliminaciones no se replica y este método de resolución de conflictos evita el problema de los directorios transformados que era posible en FRS.

Cuando se produce un conflicto, Replicación DFS registra un evento informativo en el registro de eventos de Replicación DFS. Este evento no requiere la intervención del usuario por las siguientes razones:

  - No es visible para los usuarios (solo es visible para los administradores del servidor).  
      
  - Replicación DFS trata la carpeta conflictos y eliminaciones como una caché. Cuando se alcanza un umbral de cuota, limpia algunos de esos archivos. No hay ninguna garantía de que se guarden los archivos en conflicto.  
      
  - El conflicto puede residir en un servidor diferente del origen del conflicto.  
      

## <a name="staging"></a>Preconfiguración

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>¿Replicación DFS continúa el almacenamiento provisional de los archivos cuando la replicación está deshabilitada por una cuota de limitación de ancho de banda o programación, o cuando una conexión está deshabilitada manualmente?

No. Replicación DFS no sigue almacenando provisionalmente archivos fuera de los tiempos de replicación programados, si se ha superado la cuota de límite de ancho de banda o si las conexiones están deshabilitadas.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>¿Replicación DFS evitar que otras aplicaciones tengan acceso a un archivo durante el almacenamiento provisional?

No. Replicación DFS abre archivos de forma que no impide que los usuarios o las aplicaciones abran archivos en la carpeta de replicación. Este método se conoce como "bloqueo oportunista".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>¿Es posible cambiar la ubicación de la carpeta de almacenamiento provisional con la herramienta de administración de DFS?

Sí. La ubicación de la carpeta de almacenamiento provisional se configura en la pestaña **avanzadas** del cuadro de diálogo **propiedades** de cada miembro de un grupo de replicación.

### <a name="when-are-files-staged"></a>¿Cuándo se almacenan provisionalmente los archivos?

Los archivos se almacenan provisionalmente en el miembro remitente cuando el miembro receptor solicita el archivo (a menos que el archivo tenga un tamaño de 64 KB o más pequeño) como se muestra en la tabla siguiente. Si la compresión diferencial remota (RDC) está deshabilitada en la conexión, el archivo se almacenará provisionalmente, a menos que tenga un tamaño de 256 KB o inferior. Los archivos también se almacenan provisionalmente en el miembro receptor, ya que se transfieren si tienen menos de 64 KB de tamaño, aunque puede configurar esta opción entre 16 KB y 1 MB. Si la programación está cerrada, los archivos no se almacenan provisionalmente.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Los tamaños de archivo mínimos para los archivos de almacenamiento provisional

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC habilitado</th>
<th>RDC deshabilitado</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Miembro remitente</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>Miembro receptor</p></td>
<td><p>64 KB de forma predeterminada</p></td>
<td><p>64 KB de forma predeterminada</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>¿Qué ocurre si un archivo se cambia después de que se almacene provisionalmente, pero antes de que se transmita por completo al sitio remoto?

Si ya se está transmitiendo alguna parte del archivo, Replicación DFS continúa la transmisión. Si se cambia el archivo antes de que Replicación DFS empiece a transmitir el archivo, se envía la versión más reciente del archivo.

## <a name="change-history"></a>Historial de cambios


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Date</th>
<th>Descripción</th>
<th>Reason</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 de noviembre de 2018</p></td>
<td><p>Actualizado para Windows Server 2019.</p></td>
<td><p>Nuevo sistema operativo.</p></td>
</tr>
<tr class="even">
<td><p>9 de octubre de 2013</p></td>
<td><p>Se actualizaron los límites admitidos de Replicación DFS? Sección con los resultados de las pruebas en Windows Server 2012 R2.</p></td>
<td><p>Actualizaciones de la versión más reciente de Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 de enero de 2013</p></td>
<td><p>¿Se ha agregado la Replicación DFS continuar los archivos de almacenamiento provisional cuando la replicación está deshabilitada por una cuota de límite de ancho de banda o programación, o cuando una conexión está deshabilitada manualmente? movimientos.</p></td>
<td><p>Preguntas de clientes</p></td>
</tr>
<tr class="even">
<td><p>31 de octubre de 2012</p></td>
<td><p>¿Cuáles son los límites admitidos de Replicación DFS? entrada para aumentar el número probado de archivos replicados en un volumen.</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
<tr class="odd">
<td><p>15 de agosto de 2012</p></td>
<td><p>¿Editó el Replicación DFS replicar los permisos de archivos NTFS, los flujos de datos alternativos, los vínculos físicos y los puntos de reanálisis? entrada para aclarar mejor cómo Replicación DFS controla los vínculos físicos y los puntos de reanálisis.</p></td>
<td><p>Comentarios de los servicios de soporte al cliente</p></td>
</tr>
<tr class="even">
<td><p>13 de junio de 2012</p></td>
<td><p>¿Editó el Replicación DFS funciona en los volúmenes ReFS o FAT? entrada para agregar una discusión de ReFS.</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
<tr class="odd">
<td><p>25 de abril de 2012</p></td>
<td><p>¿Editó el Replicación DFS replicar los permisos de archivos NTFS, los flujos de datos alternativos, los vínculos físicos y los puntos de reanálisis? entrada para aclarar cómo Replicación DFS controla los vínculos físicos.</p></td>
<td><p>Reducir la posible confusión</p></td>
</tr>
<tr class="even">
<td><p>30 de marzo de 2011</p></td>
<td><p>¿Se ha editado el Replicación DFS puede replicar archivos de base de datos de Outlook. PST o Microsoft Office Access? entrada para corregir el impacto potencial del uso de Replicación DFS con archivos. PST y Access.</p>
<p>¿Cómo puedo mejorar el rendimiento de la replicación?</p></td>
<td><p>Preguntas de los clientes sobre la entrada anterior, que indicaron incorrectamente que la replicación de archivos. PST o de Access podría dañar la base de datos de Replicación DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 de enero de 2011</p></td>
<td><p>¿Cómo se pueden recuperar los archivos de las carpetas ConflictAndDeleted o preexisting?</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
<tr class="even">
<td><p>20 de octubre de 2010</p></td>
<td><p>¿Cómo puedo actualizar o reemplazar un miembro de Replicación DFS?</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
</tbody>
</table>

