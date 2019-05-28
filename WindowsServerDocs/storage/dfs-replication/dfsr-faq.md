---
Title: 'Replicación DFS: Preguntas más frecuentes'
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3782667e54f5e6b52c07645704b95fc9e7409a27
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476069"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replicación DFS: Preguntas más frecuentes


Actualizado: 30 de abril de 2019

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Estas P+F responden a preguntas acerca de la replicación del sistema de archivos distribuido (DFS) (también conocida como DFS-R o DFSR) para Windows Server.

Para obtener información sobre los espacios de nombres DFS, vea [Espacios de nombres DFS: Preguntas más frecuentes](https://technet.microsoft.com/en-us/library/ee404780).

Para obtener información sobre novedades de la replicación DFS, vea los temas siguientes:

  - [Espacios de nombres DFS y replicación DFS](http://technet.microsoft.com/en-us/library/jj127250) (en Windows Server 2012)  
      
  - [Novedades en el sistema de archivos distribuido](https://technet.microsoft.com/en-us/library/ee307957) tema en [cambios de funcionalidad de Windows Server 2008 a Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd391932)  
      
  - [Sistema de archivos distribuido](https://technet.microsoft.com/en-us/library/cc753479) tema en [cambios de funcionalidad de Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208)  
      

Para ver una lista de los cambios recientes realizados en este tema, consulte la sección [Historial de cambios](#change-history) de este tema.

      

## <a name="interoperability"></a>Interoperabilidad

### <a name="can-dfs-replication-communicate-with-frs"></a>¿La replicación DFS puede comunicarse con FRS?

No. La replicación DFS no se comunica con el servicio de replicación de archivos (FRS). La replicación DFS y FRS, pueden ejecutar en el mismo servidor al mismo tiempo, pero nunca deben estar configurados para replicar las mismas carpetas o subcarpetas porque esto puede causar pérdida de datos.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>Puede reemplazar la replicación DFS FRS para la replicación de SYSVOL

Sí, la replicación DFS puede reemplazar FRS para la replicación de SYSVOL en servidores que ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Servidores que ejecutan Windows Server 2003 R2 no admiten el uso de replicación DFS para replicar la carpeta SYSVOL.

Para obtener más información acerca de la replicación de SYSVOL con replicación DFS, consulte el [Guía de migración de replicación de SYSVOL: FRS a replicación DFS](https://technet.microsoft.com/en-us/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>¿Puedo actualizar de FRS a replicación DFS sin perder los valores de configuración?

Sí. Para migrar la replicación de FRS a replicación DFS, consulte los siguientes documentos:

  - Para migrar la replicación de carpetas que no sean la carpeta SYSVOL, consulte [Guía de operaciones de DFS: Migración de FRS a replicación DFS](http://go.microsoft.com/fwlink/?linkid=192776) y [FRS2DFSR: un de FRS a la utilidad de migración de DFSR](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Para migrar la replicación de la carpeta SYSVOL en la replicación DFS, consulte [Guía de migración de replicación de SYSVOL: FRS a replicación DFS](https://technet.microsoft.com/en-us/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>¿Puedo usar la replicación DFS en un entorno mixto de Windows y UNIX?

Sí. Aunque la replicación DFS sólo es compatible con el contenido de replicación entre servidores que ejecutan Windows Server, los clientes de UNIX pueden acceder a recursos compartidos de archivos en los servidores de Windows. Para ello, instale Servicios para Network File Systems (NFS) en el servidor de replicación DFS.

También puede usar la funcionalidad de cliente SMB/CIFS incluida en muchos clientes de UNIX que tengan acceso directo comparte el archivo de Windows, aunque esta funcionalidad se limita a menudo o requiere modificaciones en el entorno de Windows (por ejemplo, para deshabilitar la firma SMB mediante el uso de Directiva de grupo).

La replicación DFS interopera con NFS en un servidor que ejecuta un sistema operativo Windows Server, pero no se puede replicar un punto de montaje NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>¿Puedo usar el servicio de instantáneas de volumen con la replicación DFS?

Sí. La replicación DFS se admite en volúmenes del servicio de instantáneas de volumen (VSS) y se pueden restaurar instantáneas anteriores correctamente con el cliente de versiones anteriores.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>¿Puedo usar copia de seguridad de Windows (Ntbackup.exe) para realizar copias de seguridad de una carpeta replicada remotamente?

No, mediante la copia de seguridad de Windows (Ntbackup.exe) en un equipo que ejecuta Windows Server 2003 o anterior al realizar una copia de seguridad del contenido de una carpeta replicada en un equipo que ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 no se admite.

Para realizar una copia de seguridad de archivos que se almacenan en una carpeta replicada, use copias de seguridad de Windows Server o Microsoft® System Center Data Protection Manager. Para obtener información acerca de la funcionalidad de copia de seguridad y recuperación en Windows Server 2008 R2 y Windows Server 2008, consulte [copias de seguridad y recuperación](https://technet.microsoft.com/en-us/library/Cc754097). Para obtener más información, consulte [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>¿Las directivas del sistema de archivos afectan a la replicación DFS?

Sí. No configure las directivas del sistema de archivos en carpetas replicadas. La directiva de archivo del sistema vuelve a aplicar los permisos NTFS en cada intervalo de actualización de directiva de grupo. Esto puede producir infracciones de uso compartido porque no se replica un archivo abierto hasta que se cierre el archivo.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>¿La replicación DFS replica los buzones alojados en Microsoft Exchange Server?

No. No se puede usar la replicación DFS para replicar los buzones alojados en Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>¿La replicación DFS admite filtros de archivos creados por el Administrador de recursos del servidor de archivos?

Sí. Sin embargo, el archivo del Administrador de recursos de servidor de archivos (FSRM) configuración de filtrado debe coincidir en ambos extremos de la replicación. Además, la replicación DFS tiene su propio mecanismo de filtro de archivos y carpetas que puede usar para excluir determinados archivos y tipos de archivo de replicación.

Los siguientes son recomendaciones para implementar filtros de archivos o cuotas:

  - No debe ser la carpeta DfsrPrivate oculta sujetos a cuotas o filtros de archivos.  
      
  - Archivos filtrados no deben existir en cualquier carpeta replicada antes de habilita el filtrado.  
      
  - No hay carpetas pueden superar la cuota antes de habilita la cuota.  
      
  - Debe utilizar las cuotas de disco duras con precaución. Es posible que los miembros individuales de un grupo de replicación para mantenerse dentro de una cuota antes de la réplica, pero se supera cuando se replican archivos. Por ejemplo, si un usuario copia un archivo de 10 megabytes (MB) en un servidor (que es, a continuación, en el límite máximo) y otro usuario copia un archivo de 5 MB en el servidor B, cuando se produce la siguiente replicación, ambos servidores superará la cuota por 5 megabytes. Esto puede producir la replicación DFS continuamente reintente replicar los archivos, lo que provoca agujeros en el vector de versión y los posibles problemas de rendimiento.  
      

### <a name="is-dfs-replication-cluster-aware"></a>¿Es compatible con clúster de replicación DFS?

Sí, la replicación DFS en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 incluye la capacidad de agregar un clúster de conmutación por error como un miembro de un grupo de replicación. Para obtener más información, consulte [agregar un clúster de conmutación por error a un grupo de replicación](http://go.microsoft.com/fwlink/?linkid=155085) (http://go.microsoft.com/fwlink/?LinkId=155085). El servicio de replicación DFS en versiones de Windows anteriores a Windows Server 2008 R2 no está diseñado para coordinarse con un clúster de conmutación por error y el servicio no se conmute a otro nodo.


> [!NOTE]
> La replicación DFS no admite la replicación de archivos en volúmenes compartidos de clúster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>¿La replicación DFS es compatible con desduplicación de datos?

Sí, la replicación DFS puede replicar las carpetas de volúmenes que usan la desduplicación de datos en Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>¿Es compatible con RIS y WDS replicación DFS?

Sí. Replicación DFS replica los volúmenes en el que está habilitado el almacenamiento de instancia única (SIS). SIS está usando Servicios de instalación remota (RIS), servicios de implementación de Windows (WDS) y Windows Storage Server.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>¿Es posible usar la replicación DFS con archivos sin conexión?

Puede utilizar la replicación DFS y los archivos sin conexión juntos en escenarios cuando hay solo un usuario a la vez que escribe en los archivos. Esto es útil para los usuarios que viajan entre dos sucursales y desean poder acceder a sus archivos en cualquier rama o mientras sin conexión. Los archivos sin conexión almacena en caché los archivos localmente para su uso sin conexión y la replicación DFS replica los datos entre cada sucursal.

No utilice la replicación DFS con archivos sin conexión en un entorno multiusuario porque la replicación DFS no proporciona ningún mecanismo de bloqueo distribuido o capacidad de desprotección de archivos. Si dos usuarios modifican el mismo archivo al mismo tiempo en distintos servidores, replicación DFS mueve el archivo anterior a la DfsrPrivate\\ConflictandDeleted carpeta (situado en la ruta de acceso local de la carpeta replicada) durante la próxima replicación.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>¿Qué aplicaciones antivirus son compatibles con la replicación DFS?

Las aplicaciones antivirus pueden provocar un exceso de replicación si sus actividades análisis alteran los archivos en una carpeta replicada. Para obtener más información, [las pruebas de interoperabilidad de aplicaciones de Antivirus con la replicación DFS](http://go.microsoft.com/fwlink/?linkid=73990) (http://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>¿Cuáles son las ventajas de usar la replicación DFS en lugar de Windows SharePoint Services?

Windows® SharePoint® Services ofrece coherencia estricta en forma de funcionalidad de desprotección de archivos que la replicación DFS no. Si le preocupa que varias personas modifiquen el mismo archivo, se recomienda usar Windows SharePoint Services. Windows SharePoint Services 2.0 con Service Pack 2 está disponible como parte de Windows Server 2003 R2. Windows SharePoint Services se puede descargar desde el sitio Web de Microsoft; no se incluye en las versiones más recientes de Windows Server. Sin embargo, si va a replicar datos entre varios sitios y los usuarios no modificará los mismos archivos al mismo tiempo, la replicación DFS proporciona mayor ancho de banda y una administración más simple.

## <a name="limitations-and-requirements"></a>Requisitos y limitaciones

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>¿La replicación DFS puede replicar entre las sucursales sin una conexión VPN?

Sí, suponiendo que hay un vínculo de red de área extensa (WAN) privado (no por Internet) conecta las sucursales. Sin embargo, debe abrir los puertos adecuados en los firewalls externos. La replicación DFS usa el Endpoint Mapper de RPC (puerto 135) y un puerto efímero asignado de forma aleatoria por encima de 1024. Puede usar el **Dfsrdiag** herramienta de línea de comandos para especificar un puerto estático en lugar del puerto efímero. Para obtener más información sobre cómo especificar el Endpoint Mapper de RPC, vea [artículo 154596](http://go.microsoft.com/fwlink/?linkid=73991) en Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>¿La replicación DFS puede replicar archivos cifrados con el sistema de archivos cifrados?

No. La replicación DFS no se replicarán los archivos o carpetas que se cifran mediante el sistema de cifrado de archivos (EFS). Si un usuario cifra un archivo que se replicó previamente, la replicación DFS elimina el archivo de todos los demás miembros del grupo de replicación. Esto garantiza que la copia del archivo solo está disponible la versión cifrada en el servidor.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>¿Puede replicar la replicación DFS .pst de Outlook o archivos de base de datos de Microsoft Office Access?

La replicación DFS segura puede replicar archivos de carpetas personales (.pst) de Microsoft Outlook y Microsoft Access archivos solo si se almacenan para fines de archivado y no se tiene acceso a través de la red mediante el uso de un cliente, como Outlook o acceso (para abrir el acceso o .pst los archivos, copie primero los archivos en un dispositivo de almacenamiento local). Las razones para esto son los siguientes:

  - Abrir archivos .pst en conexiones de red podría provocar daños en los archivos .pst los datos. Para obtener más información acerca de por qué los archivos .pst no tengan un acceso seguro desde a través de una red, consulte [artículo 297019](http://go.microsoft.com/fwlink/?linkid=125363) en Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - archivos .pst y acceso tienden a permanezca abierta durante largos períodos de tiempo mientras se está accediendo a un cliente, como Outlook o Office Access. Esto evita que la replicación DFS para replicar estos archivos hasta que se han cerrado.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>¿Puedo usar la replicación DFS en un grupo de trabajo?

No. La replicación DFS se basa en Active Directory® Domain Services para la configuración. Solo funcionará en un dominio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>¿Más de una carpeta se puede replicar en un único servidor?

Sí. La replicación DFS puede replicar numerosas carpetas entre servidores. Asegúrese de que cada una de las carpetas replicadas tiene una única raíz la ruta de acceso y que no se superponen. Por ejemplo, D:\\Sales y D:\\cuentas pueden ser las rutas de acceso raíz para dos carpetas replicadas, pero D:\\Sales y D:\\ventas\\informes no pueden ser las rutas de acceso raíz para dos carpetas replicadas.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>¿La replicación DFS requiere espacios de nombres DFS?

No. La replicación DFS y espacios de nombres DFS pueden utilizarse por separado o conjuntamente. Además, se puede usar replicación DFS para replicar independiente espacios de nombres DFS, lo que no era posible con FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>¿La replicación DFS requiere sincronización temporal entre servidores?

No. La replicación DFS no requiere sincronización temporal entre servidores explícitamente. Sin embargo, la replicación DFS requiere que los relojes del servidor coinciden. Los relojes del servidor se deben establecer en cinco minutos entre sí (de forma predeterminada) para que la autenticación Kerberos funcione correctamente. Por ejemplo, la replicación DFS usa las marcas de tiempo para determinar qué archivo tiene prioridad si se produce un conflicto. Veces precisas también son importantes para la recolección de elementos, programaciones y otras características.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>¿Admite la replicación DFS replicar un volumen completo?

Sí. Sin embargo, primero debe instalar Windows Server 2003 Service Pack 2 o la revisión. Para obtener más información, consulte [artículo 920335](http://go.microsoft.com/fwlink/?linkid=76776) en Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=76776). Además, la replicación de un volumen completo puede producir los siguientes problemas:

  - Si el volumen contiene un archivo de paginación de Windows, la replicación se produce un error y registros de eventos DFSR 4312 en el registro de eventos del sistema.  
      
  - La replicación DFS establece los atributos del sistema y oculto en la carpeta replicada en los servidores de destino. Esto se produce porque Windows aplica los atributos del sistema y oculto a la carpeta raíz del volumen de forma predeterminada. Si la ruta de acceso local de la carpeta replicada en los servidores de destino también es una raíz de volumen, no se realiza ningún cambio adicional los atributos de carpeta.  
      
  - Al replicar un volumen que contiene la carpeta del sistema de Windows, la replicación DFS reconoce la carpeta % WINDIR % y no se replica. Sin embargo, la replicación DFS replicar las carpetas utilizadas por las aplicaciones que no sean de Microsoft, lo que podrían producir un error en las aplicaciones de en los servidores de destino si las aplicaciones tienen problemas de interoperabilidad con la replicación DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>¿La replicación DFS admite RPC sobre HTTP?

No.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>¿Funciona la replicación DFS a través de redes inalámbricas?

Sí. La replicación DFS es independiente del tipo de conexión.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>¿Funciona la replicación DFS en ReFS o volúmenes FAT?

No. La replicación DFS es compatible con volúmenes formateados con el sistema de archivos NTFS solamente; el sistema de archivos resistente (ReFS) y el sistema de archivos FAT no se admiten. La replicación DFS requiere NTFS porque usa el diario de cambios NTFS y otras características de NTFS que el sistema de archivos.

### <a name="does-dfs-replication-work-with-sparse-files"></a>¿La replicación DFS funciona con archivos dispersos?

Sí. Puede replicar archivos dispersos. El **Sparse** se conserva el atributo del miembro receptor.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>¿Es necesario iniciar sesión como administrador para replicar los archivos?

No. La replicación DFS es un servicio que se ejecuta bajo la cuenta sistema local, por lo que no es necesario iniciar sesión como administrador para replicar. Sin embargo, debe ser un administrador de dominio o un administrador local de los servidores de archivos afectados para realizar cambios en la configuración de replicación DFS.

Para obtener más información, vea "requisitos de seguridad de la replicación DFS y la delegación" en el [delegar la capacidad de administrar la replicación DFS](http://go.microsoft.com/fwlink/?linkid=182294) (http://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>¿Cómo puedo actualizar o reemplazar a un miembro de la replicación DFS?

Para actualizar o reemplazar a un miembro de la replicación DFS, consulte este blog en la Ask el blog del equipo de servicios de directorio: [Reemplazar SO o Hardware miembro DFSR](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>¿Es la replicación DFS adecuado para la replicación de los perfiles móviles?

Sí. Algunos escenarios se admiten al replicar los perfiles de usuario móviles. Para obtener información acerca de los escenarios admitidos, vea [soporte instrucción en torno a replicar datos de Microsoft de usuario](http://go.microsoft.com/fwlink/?linkid=201282) (http://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>¿Hay un límite de caracteres del archivo o el límite a la profundidad de la carpeta?

Windows y la replicación DFS admiten rutas de acceso de carpeta con caracteres de hasta 32 mil. La replicación DFS no se limita a las rutas de acceso de carpeta de 260 caracteres.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>¿Los miembros de un grupo de replicación deben residir en el mismo dominio?

No. Pueden abarcar grupos de replicación en varios dominios dentro de un único bosque, pero no a través de diferentes bosques.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>¿Cuáles son los límites admitidos de la replicación DFS?

En la lista siguiente proporciona un conjunto de directrices de escalabilidad que se han probado por Microsoft en Windows Server 2012 R2:

  - Tamaño de todos los archivos replicados en un servidor: 100 terabytes.  
      
  - Número de archivos replicados en un volumen: 70 millones.  
      
  - Tamaño máximo de archivo: 250 gigabytes.  
      


> [!IMPORTANT]
> Al crear grupos de replicación con un gran número o tamaño de archivos, se recomienda exportar un clon de la base de datos y usar técnicas de propagación previamente para minimizar la duración de la replicación inicial. Para obtener más información, consulte <A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">sincronización inicial de la replicación DFS en Windows Server 2012 R2: Ataque de los Clones</A>. 
<br>


En la lista siguiente proporciona un conjunto de directrices de escalabilidad que se han probado por Microsoft en Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008:

  - Tamaño de todos los archivos replicados en un servidor: 10 terabytes.  
      
  - Número de archivos replicados en un volumen: 11 millones.  
      
  - Tamaño máximo de archivo: 64 gigabytes.  
      


> [!NOTE]
> Ya no hay un límite al número de grupos de replicación, las carpetas replicadas, las conexiones o los miembros del grupo de replicación. 
<br>


Para obtener una lista de las instrucciones de escalabilidad que se han probado por Microsoft para Windows Server 2003 R2, consulte [directrices de escalabilidad de la replicación DFS](http://go.microsoft.com/fwlink/?linkid=75043) (http://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>¿Cuándo debo no usar la replicación DFS?

No utilice la replicación DFS en un entorno donde varios usuarios actualizar o modificar los mismos archivos al mismo tiempo en distintos servidores. Si lo hace, puede hacer que la replicación DFS mover las copias en conflicto de los archivos a la oculta DfsrPrivate\\ConflictandDeleted carpeta.

Cuando varios usuarios necesitan modificar los mismos archivos al mismo tiempo en distintos servidores, use la característica de protección y desprotección de archivos de Windows SharePoint Services para asegurarse de que solo un usuario está trabajando en un archivo. Windows SharePoint Services 2.0 con Service Pack 2 está disponible como parte de Windows Server 2003 R2. Windows SharePoint Services se puede descargar desde el sitio Web de Microsoft; no se incluye en las versiones más recientes de Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>¿Por qué es una actualización de esquema necesaria para la replicación DFS?

Replicación DFS usa los nuevos objetos en el contexto de nomenclatura de dominio de Active Directory Domain Services para almacenar información de configuración. Estos objetos se crean al actualizar el esquema de Servicios de dominio de Active Directory. Para obtener más información, consulte [revisión de requisitos para la replicación DFS](http://go.microsoft.com/fwlink/?linkid=182264) (http://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Herramientas de administración y supervisión

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>¿Automatizar el informe de mantenimiento para recibir advertencias?

Sí. Hay tres maneras de automatizar los informes de mantenimiento:

  - Usar el módulo de DFSR Windows PowerShell incluido en Windows Server 2012 R2 o DfsrAdmin.exe junto con las tareas programadas con regularidad generar informes de mantenimiento. Para obtener más información, consulte [automatización de informes de mantenimiento de replicación DFS](http://go.microsoft.com/fwlink/?linkid=74010) (http://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Use el módulo de administración de replicación DFS para System Center Operations Manager para crear alertas que se basan en las condiciones especificadas.  
      
  - Usar el proveedor WMI de replicación DFS para generar scripts de alertas.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>¿Puedo usar Microsoft System Center Operations Manager para supervisar la replicación DFS?

Sí. Para obtener más información, consulte el [DFS Replication Management Pack para System Center Operations Manager 2007](http://go.microsoft.com/fwlink/?linkid=182265) en Microsoft Download Center (http://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>¿La replicación DFS admite la administración remota?

Sí. La replicación DFS admite la administración remota mediante la consola de administración de DFS y la **Agregar grupo de replicación** comando. Por ejemplo, en el servidor A, puede conectarse a un grupo de replicación definido en el bosque con los servidores A y B como miembros.

Administración de DFS se incluye con Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003 R2. Para administrar la replicación DFS desde otras versiones de Windows, utilice Escritorio remoto o el [remoto Server herramientas de administración para Windows 7](https://technet.microsoft.com/en-us/library/Ee449475).


> [!IMPORTANT]
> Para ver o administrar grupos de replicación que contienen carpetas replicadas de solo lectura o miembros que son los clústeres de conmutación por error, debe usar la versión de administración de DFS que se incluye con Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, el <a href="http://go.microsoft.com/fwlink/p/?linkid=238560">Herramientas de administración remota del servidor para Windows 8</a>, o el <a href="https://technet.microsoft.com/en-us/library/ee449475">herramientas de administración remota del servidor para Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>¿Ultrasound y Sonar funcionan con la replicación DFS?

No. La replicación DFS tiene su propio conjunto de herramientas de supervisión y diagnóstico. Ultrasound y Sonar solo son capaces de supervisión de FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>¿Cómo se pueden recuperar archivos desde las carpetas ConflictAndDeleted y PreExisting?

Para recuperar archivos perdidos, restaure los archivos desde la carpeta sistema de archivos o una carpeta compartida mediante el historial de archivos, el **restaurar versiones anteriores** comando en el Explorador de archivos, o mediante la restauración de los archivos de copia de seguridad. Para recuperar archivos directamente desde la carpeta ConflictAndDeleted o PreExisting, use el `Get-DfsrPreservedFiles` y `Restore-DfsrPreservedFiles` cmdlets de Windows PowerShell (incluidos con el módulo DFSR en Windows Server 2012 R2), o la [RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr) script de ejemplo desde la Galería de código de MSDN. Este script está pensado solo para la recuperación ante desastres y se proporciona AS-es, sin ninguna garantía.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>¿Hay una manera de conocer el estado de replicación?

Sí. Hay varias maneras de supervisar la replicación:

  - La replicación DFS tiene un módulo de administración de System Center Operations Manager que proporciona supervisión proactiva.  
      
  - Administración de DFS tiene un informe de diagnóstico en el equipo para el trabajo pendiente de replicación, la eficacia de la replicación y el número de archivos y carpetas en un grupo de replicación determinado.  
      
  - El módulo de DFSR Windows PowerShell en Windows Server 2012 R2 contiene cmdlets para iniciar las pruebas de propagación y escribir los informes de mantenimiento y la propagación. Para obtener más información, consulte [distribuye los Cmdlets de replicación de archivos del sistema en Windows PowerShell](http://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe es una herramienta de línea de comandos que puede generar un recuento de trabajo pendiente o un desencadenador de una prueba de propagación. Ambos muestran el estado de replicación. La propagación se muestra si los archivos se replican en todos los nodos. Trabajo pendiente muestra cuántos archivos que replicar antes de que dos equipos están sincronizados. El recuento de trabajos pendientes es el número de actualizaciones que no ha procesado un miembro del grupo de replicación. En los equipos que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, Dfsrdiag.exe también puede mostrar las actualizaciones que se está replicando la replicación DFS.  
      
  - Las secuencias de comandos pueden utilizar WMI para recopilar información de trabajo pendiente: manualmente o a través de MOM.  
      

## <a name="performance"></a>Rendimiento

### <a name="does-dfs-replication-support-dial-up-connections"></a>¿Admite la replicación DFS telefónico?

Aunque la replicación DFS funciona a velocidades de acceso telefónico, puede obtener acumulado si hay grandes cantidades de cambios se repliquen. Si se realizan pequeños cambios en los archivos existentes, la replicación DFS con compresión diferencial remota (RDC) proporcionará un rendimiento mucho mayor que copiar el archivo directamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>¿La replicación DFS realizar la detección de ancho de banda?

No. La replicación DFS no lleva a cabo la detección de ancho de banda. Puede configurar la replicación DFS para usar una cantidad limitada de ancho de banda en una base por conexión (límite de ancho de banda). Sin embargo, la replicación DFS no reducir aún más el uso de ancho de banda si la interfaz de red se satura y replicación DFS puede saturar el vínculo durante breves períodos. Ancho de banda con la replicación DFS no es completamente exacto, porque la replicación DFS limita el ancho de banda por la limitación de las llamadas RPC. Como resultado, pueden interferir varios búferes en los niveles inferiores de la pila de red (incluida RPC), causando ráfagas de tráfico de red.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>¿La replicación DFS limitar el ancho de banda por programación, servidor o conexión?

Si configura el límite de ancho de banda al especificar la programación, todas las conexiones para dicho grupo de replicación utilizará ese valor para la limitación de ancho de banda. Limitación de ancho de banda se puede también establecer como una configuración de nivel de conexión mediante administración de DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>¿La replicación DFS usa Active Directory Domain Services para calcular los vínculos a sitios y los costos de conexión?

No. La replicación DFS usa la topología definida por el administrador, que es independiente del costo de sitio de Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>¿Cómo se puede mejorar el rendimiento de la replicación?

Para obtener información sobre los diferentes métodos de optimización de rendimiento de la replicación, vea [ajuste del rendimiento de replicación en DFSR](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) en el [formular el blog del equipo de servicios de directorio](http://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>¿Cómo evitar la replicación DFS saturar una conexión?

En la replicación DFS establece el ancho de banda máximo que desea usar en una conexión y el servicio mantiene ese nivel de uso de la red. Esto es diferente del servicio de transferencia inteligente en segundo plano (BITS) y replicación DFS no saturar la conexión si se establecen correctamente.

Sin embargo, no es exacta al 100% del límite de ancho de banda y replicación DFS puede saturar el vínculo durante breves períodos de tiempo. Esto es porque la replicación DFS limita el ancho de banda por la limitación de las llamadas RPC. Dado que este proceso se basa en varios búferes en los niveles inferiores de la pila de red, incluidas RPC, el tráfico de replicación suele viajar en ráfagas que a veces pueden saturar los vínculos de red.

La replicación DFS en Windows Server 2008 incluye varias mejoras de rendimiento, como se describe en [sistema de archivos distribuido](https://technet.microsoft.com/en-us/library/Cc753479), un tema en [cambios de funcionalidad de Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/en-us/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>¿Cómo se compara el rendimiento de la replicación DFS con FRS?

Replicación DFS es mucho más rápida que FRS, especialmente cuando se realizan pequeños cambios en archivos de gran tamaño y RDC está habilitado. Por ejemplo, con RDC, un pequeño cambio en una presentación MB PowerPoint® 2 puede provocar solo 60 kilobytes (KB) que se envían a través de la red, un ahorro del 97% de bytes transferidos.

RDC no se usa en los archivos inferiores a 64 KB y podría no ser beneficioso en LAN de alta velocidad donde el ancho de banda de red no es contenido. RDC se puede deshabilitar en una función de la conexión con administración de DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>¿La frecuencia con la replicación DFS replica los datos?

Replican los datos según la programación establecida. Por ejemplo, puede establecer la programación en intervalos de 15 minutos, siete días a la semana. Durante estos intervalos, la replicación está habilitada. Poco después de que se detecta un cambio de archivo (por lo general, en segundos) se inicia la replicación.

La programación del grupo de replicación puede establecerse para horario Universal coordinado (UTC) mientras se establece la programación de la conexión a la hora local del miembro receptor. Tenga esto en cuenta cuando el grupo de replicación abarque varias zonas horarias. Hora local significa la hora del miembro que hospeda la conexión de entrada. La programación de la conexión entrante y saliente correspondiente mostrada reflejan las diferencias de zona horaria cuando la programación está establecida en hora local.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>¿Cantidad de recursos del sistema de mi servidor consumirá la replicación DFS?

El disco, memoria y recursos de CPU utilizados por la replicación DFS dependen de una serie de factores, incluido el número y tamaño de los archivos, la tasa de cambio, el número de miembros del grupo de replicación y el número de carpetas replicadas. Además, algunos recursos son más difíciles de calcular. Por ejemplo, la tecnología de motor de almacenamiento Extensible (ESE) utilizada para la base de datos de replicación DFS puede consumir un gran porcentaje de memoria disponible, lo que libera a petición. Aplicaciones que no sean la replicación DFS pueden hospedarse en el mismo servidor según la configuración del servidor. Sin embargo, al hospedar varias aplicaciones o roles de servidor en un solo servidor, es importante que pruebe esta configuración antes de implementarlo en un entorno de producción.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>¿Qué ocurre si se produce un error en un vínculo WAN durante la replicación?

Si la conexión se interrumpe, la replicación DFS seguirá intentando replicar mientras que la programación está abierta. También habrá errores de conectividad que se ha indicado en el registro de eventos de la replicación DFS puede obtenerla mediante MOM (proactivamente a través de alertas) y el informe de mantenimiento de replicación DFS (de forma reactiva, por ejemplo, cuando un administrador lo ejecuta).

## <a name="remote-differential-compression-details"></a>Detalles de la compresión diferencial remota

### <a name="are-changes-compressed-before-being-replicated"></a>¿Se comprimen los cambios antes de que se va a replicar?

Sí. Las partes modificadas de los archivos se comprimen antes de ser enviados para todos los tipos, excepto los siguientes (que ya están comprimidos) de archivos: .wma, .wmv, .zip, .jpg, .mpg, .mpeg,. m1v,. mp2,. mp3, .mpa, .cab, .wav, .snd,. au, .asf, .wm, .avi, .z, .gz, tgz y .frx. Configuración de compresión para estos tipos de archivo no es configurable en Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>¿Puede un administrador desactivar RDC o cambiar el umbral?

Sí. Puede desactivar RDC a través de la página de propiedades de una conexión determinada. Deshabilitación de RDC puede reducir la CPU de latencia de replicación y uso en vínculos de red de área local (LAN) rápidas que no tienen ninguna restricción de ancho de banda o para grupos de replicación que constan principalmente de los archivos inferiores a 64 KB. Si decide desactivar RDC en una conexión, probar la eficacia de la replicación antes y después del cambio para comprobar que ha mejorado el rendimiento de la replicación.

Puede cambiar el umbral de tamaño RDC mediante el **Dfsradmin conexión establecida** comando, el proveedor WMI de replicación DFS, o editando manualmente el XML de configuración de archivo.

### <a name="does-rdc-work-on-all-file-types"></a>¿RDC funciona en todos los tipos de archivo?

Sí. RDC calcula las diferencias en el nivel de bloque con independencia del tipo de datos de archivo. Sin embargo, RDC funciona más eficazmente en ciertos tipos de archivo, como documentos de Microsoft Word, archivos PST e imágenes de disco duro virtual.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>¿Cómo funciona en un archivo comprimido RDC?

La replicación DFS usa RDC, que calcula los bloques en el archivo que han cambiado y envía solo los bloques a través de la red. La replicación DFS no necesita saber nada sobre el contenido del archivo, sólo los bloques se han cambiado.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>¿Es RDC entre archivos habilitada al actualizar a Windows Server Enterprise Edition o Datacenter Edition?

No se admiten las ediciones estándar de Windows Server RDC entre archivos. Sin embargo, este se habilita automáticamente cuando se actualiza a una edición que admite RDC entre archivos, o si un miembro de la conexión de replicación se está ejecutando una edición admitida. ¿Para obtener una lista de las ediciones que RDC entre archivos de soporte técnico, consulte Qué ediciones de la compatibilidad de sistema operativo Windows RDC entre archivos?

### <a name="is-rdc-true-block-level-replication"></a>¿Es la replicación de nivel de bloque RDC true?

No. RDC es un protocolo de uso general para la compresión de transferencia de archivos. La replicación DFS usa RDC en bloques en el nivel de archivo, no en el nivel de bloque de disco. RDC divide un archivo en bloques. Para cada bloque en un archivo, calcula una firma, que es un pequeño número de bytes que puede representar el bloque mayor. El conjunto de firmas se transfiere desde el servidor al cliente. El cliente compara las firmas de servidor a su propio. A continuación, el cliente las solicitudes que el servidor envíe solo los datos para las firmas que no están ya en el cliente.

### <a name="what-happens-if-i-rename-a-file"></a>¿Qué ocurre si cambia el nombre de un archivo?

La replicación DFS cambia el nombre del archivo en todos los demás miembros del grupo de replicación durante la próxima replicación. Los archivos están sometidas a seguimiento utilizando un identificador único, por lo que cambiar el nombre de un archivo y mover que el archivo dentro de la réplica no tiene ningún efecto en la capacidad de replicación DFS para replicar un archivo.

### <a name="what-is-cross-file-rdc"></a>¿Qué es RDC entre archivos?

RDC entre archivos RDC permite la replicación DFS utilizar RDC incluso cuando no existe un archivo con el mismo nombre en el extremo del cliente. RDC entre archivos RDC utiliza una heurística para determinar los archivos que son similares al archivo que se debe replicar y bloques de usos de los archivos similares que son idénticos para el archivo de replicación para minimizar la cantidad de datos se transfieren a través de la WAN. RDC entre archivos RDC puede utilizar bloques de hasta cinco archivos similares en este proceso.

Para utilizar RDC entre archivos, un miembro de la conexión de replicación debe ejecutar una edición de Windows que admite RDC entre archivos. ¿Para obtener una lista de las ediciones que RDC entre archivos de soporte técnico, consulte Qué ediciones de la compatibilidad de sistema operativo Windows RDC entre archivos?

### <a name="what-is-rdc"></a>¿Qué es RDC?

Compresión diferencial remota (RDC) es un protocolo cliente-servidor que puede usarse para actualizar los archivos de forma eficaz a través de una red de ancho de banda limitado. RDC detecta inserciones, eliminaciones y reorganizaciones de datos en archivos, habilitar la replicación DFS replicar únicamente los cambios cuando se actualizan los archivos. RDC se usa solo para los archivos que son de 64 KB o más grandes de forma predeterminada. RDC puede usar una versión anterior de un archivo con el mismo nombre en la carpeta replicada o en el DfsrPrivate\\carpeta ConflictandDeleted (situado en la ruta de acceso local de la carpeta replicada).

### <a name="when-is-rdc-used-for-replication"></a>¿Cuando se usa RDC para la replicación?

RDC se usa cuando el archivo supera un umbral de tamaño mínimo. Este umbral de tamaño es 64 KB de forma predeterminada. Después de que se ha replicado un archivo que supere ese umbral, las versiones actualizadas del archivo siempre usan RDC, a menos que una gran parte del archivo se cambia o se deshabilita RDC.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>¿RDC entre qué ediciones de la compatibilidad de sistema operativo de Windows?

Para utilizar RDC entre archivos, un miembro de la conexión de replicación debe ejecutar una edición del sistema operativo Windows que admite RDC entre archivos. La siguiente tabla muestra qué ediciones del sistema operativo Windows compatible con RDC entre archivos.

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>RDC entre archivos disponibilidad RDC en las ediciones del sistema operativo Windows

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
<td><p>Windows Server 2012 R2</p></td>
<td><p>Sí*</p></td>
<td><p>No disponible</p></td>
<td><p>Sí*</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>Sí</p></td>
<td><p>No disponible</p></td>
<td><p>Sí</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>No</p></td>
<td><p>Sí</p></td>
<td><p>Sí</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>No</p></td>
<td><p>Sí</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>No</p></td>
<td><p>Sí</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

\* Opcionalmente, puede deshabilitar RDC entre archivos en Windows Server 2012 R2.

## <a name="replication-details"></a>Detalles de la replicación

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>¿Puedo cambiar la ruta de acceso de una carpeta replicada después de crearla?

No. Si necesita cambiar la ruta de acceso de una carpeta replicada, debe eliminarlo de la administración de DFS y agregarlo como una nueva carpeta replicada. La replicación DFS, a continuación, utiliza compresión diferencial remota (RDC) para realizar una sincronización que determina si los datos son el mismo en los miembros remitentes y receptores. No replica todos los datos en la carpeta de nuevo.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>¿Puedo configurar se replican los atributos de archivo?

No, no se puede configurar los atributos de archivo que la replicación DFS replica.

Para obtener una lista de valores de atributo y sus descripciones, consulte [los atributos de archivo](http://go.microsoft.com/fwlink/?linkid=182268) en MSDN (http://go.microsoft.com/fwlink/?LinkId=182268).

Se han establecido los siguientes valores de atributo con el `SetFileAttributes dwFileAttributes` función y se replican mediante replicación DFS. Los cambios realizados en estos valores de atributo desencadenan la replicación de los atributos. El contenido del archivo no se replica a menos que el contenido también cambia. Para obtener más información, consulte [SetFileAttributes función](http://go.microsoft.com/fwlink/?linkid=182269) en MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269).

  - ARCHIVO\_ATRIBUTO\_HIDDEN  
      
  - ARCHIVO\_ATRIBUTO\_READONLY  
      
  - ARCHIVO\_ATRIBUTO\_SISTEMA  
      
  - ARCHIVO\_ATRIBUTO\_NO\_CONTENIDO\_INDEXADO  
      
  - ARCHIVO\_ATRIBUTO\_SIN CONEXIÓN  
      

Los siguientes valores de atributo se replican mediante replicación DFS, pero no desencadenan la replicación.

  - ARCHIVO\_ATRIBUTO\_ARCHIVE  
      
  - ARCHIVO\_ATRIBUTO\_NORMAL  
      

Los siguientes valores de atributo de archivo también desencadenan la replicación, aunque no pueden configurarse mediante el uso de la `SetFileAttributes` (función) (use el `GetFileAttributes` función para ver los valores de atributo).

  - ARCHIVO\_ATRIBUTO\_REANÁLISIS\_PUNTO  
      

> [!NOTE]
> La replicación DFS no replica los valores de atributo de punto de reanálisis, a menos que la etiqueta de reanálisis es IO_REPARSE_TAG_SYMLINK. Archivos con las etiquetas de reanálisis IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS o IO_REPARSE_TAG_HSM se replican como archivos normales. Sin embargo, la etiqueta de repetición de análisis y los búferes de datos de repetición de análisis no se replican en otros servidores porque el punto de reanálisis solo funciona en el sistema local. 
<br>

  - ARCHIVO\_ATRIBUTO\_COMPRIMIDA  
      
  - ARCHIVO\_ATRIBUTO\_CIFRADA  
      

> [!NOTE]
> La replicación DFS no replica los archivos que se cifran mediante el sistema de cifrado de archivos (EFS). La replicación DFS replique archivos que están cifrados mediante el uso de software ajeno a Microsoft, pero solo si no establece el valor del atributo FILE_ATTRIBUTE_ENCRYPTED en el archivo. 
<br>

  - ARCHIVO\_ATRIBUTO\_SPARSE\_ARCHIVO  
      
  - ARCHIVO\_ATRIBUTO\_DIRECTORIO  
      

La replicación DFS no se replica el archivo\_atributo\_valor temporal.

### <a name="can-i-control-which-member-is-replicated"></a>¿Puedo controlar qué miembros se replican?

Sí. Puede elegir una topología cuando se crea un grupo de replicación. O bien puede seleccionar **ninguna topología** y configurar manualmente las conexiones después de crear el grupo de replicación.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>¿Se pueden propagar a un miembro del grupo de replicación con los datos antes de la replicación inicial?

Sí. La replicación DFS es compatible con copia de archivos a un miembro del grupo de replicación antes de la replicación inicial. Esta sección "preconfiguración" puede reducir drásticamente la cantidad de datos replicados durante la replicación inicial.

La replicación inicial no es necesario replicar el contenido cuando los archivos difieren solo en atributos real o las marcas de tiempo. Un atributo real es un atributo que se puede establecer mediante la función de Win32 `SetFileAttributes`. Para obtener más información, consulte [SetFileAttributes función](http://go.microsoft.com/fwlink/?linkid=182269) en MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269). Si los dos archivos se diferencian en otros atributos, como la compresión, se replica el contenido del archivo.

Para preconfigurar a un miembro del grupo de replicación, copie los archivos en la carpeta correspondiente en los servidores de destino, cree el grupo de replicación y, a continuación, elija a un miembro primario. Seleccione al miembro que contenga los archivos más recientes que se va a replicar porque el contenido del miembro principal se considera "autoritativo". Esto significa que, durante la replicación inicial, los archivos del miembro principal siempre sobrescribirá otras versiones de los archivos en otros miembros del grupo de replicación.

Para obtener información acerca de la inicialización previa y clonar la base de datos DFSR, consulte [sincronización inicial de la replicación DFS en Windows Server 2012 R2: Ataque de los Clones](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Para obtener más información acerca de la replicación inicial, vea [crear un grupo de replicación](https://technet.microsoft.com/en-us/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>¿Replicación DFS superar los problemas comunes de servicio de replicación de archivos?

Sí. La replicación DFS supera tres problemas comunes de FRS:

  - Ajustes del diario: La replicación DFS se recupera desde el ajusta diario sobre la marcha. Cada archivo o carpeta existente se marca como journalWrap y comprueba con el sistema de archivos antes de que se vuelva a habilitar la replicación. Durante la recuperación, este volumen no está disponible para la replicación en cualquier dirección.  
      
  - Exceso de replicación: Para evitar el exceso de replicación, replicación DFS usa un sistema de créditos.  
      
  - Carpetas transformadas: Para evitar que los nombres de carpeta transformadas, replicación DFS almacena los datos en conflicto en un DfsrPrivate oculto\\carpeta ConflictandDeleted (situado en la ruta de acceso local de la carpeta replicada). Por ejemplo, crear varias carpetas al mismo tiempo con nombres idénticos en distintos servidores replicados usa FRS, FRS cambiar el nombre de las carpetas anteriores. En su lugar, la replicación DFS mueve las carpetas anteriores a la carpeta conflictos y eliminaciones local.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>¿La replicación DFS replica los archivos en orden cronológico?

No. Los archivos pueden replicarse en el orden correcto.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>¿La replicación DFS replica los archivos que está usando otra aplicación?

Si una aplicación abre un archivo y crea un bloqueo de archivo en él (impidiendo que se está usando en otras aplicaciones mientras está abierto), la replicación DFS no se replicarán el archivo hasta que se cierra. Si la aplicación abre el archivo con acceso de recurso compartido de lectura, todavía se puede replicar el archivo.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>¿La replicación DFS replicar los permisos de archivos NTFS, flujos de datos alternativos, vínculos físicos y puntos de reanálisis?

  - La replicación DFS replica los permisos de archivos NTFS y flujos de datos alternativos.  
      
  - Microsoft no admite crear vínculos físicos de NTFS a o desde los archivos en una carpeta replicada: Esto puede causar problemas de replicación con los archivos afectados. Archivos de vínculo físico se omiten por la replicación DFS y no se replican. Los puntos de unión también no se replican y replicación DFS registra eventos 4406 para cada punto de unión que se encuentra.  
      
  - Los puntos de reanálisis solo replicados mediante replicación DFS son aquellas que usan la E/S\_REANÁLISIS\_etiqueta\_una etiqueta de vínculo SIMBÓLICO; sin embargo, la replicación DFS no garantiza que también se replica el destino de un vínculo simbólico. Para obtener más información, consulte el [formular el blog del equipo de servicios de directorio](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Archivos con la operación de E/S\_de REANÁLISIS\_etiqueta\_DESDUPLICACIÓN, E/S\_de REANÁLISIS\_etiqueta\_SIS o E/S\_REANÁLISIS\_etiqueta\_se replican las etiquetas de repetición de análisis de HSM como archivos normales. La etiqueta de repetición de análisis y los búferes de datos de repetición de análisis no se replican a otros servidores porque el punto de reanálisis solo funciona en el sistema local. Por lo tanto, la replicación DFS puede replicar las carpetas de volúmenes que usan la desduplicación de datos en Windows Server 2012 o almacenamiento de instancia única (SIS), sin embargo, la información de desduplicación de datos se mantiene por separado por cada servidor en el que está habilitado el servicio de rol.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>¿La replicación DFS replica los cambios de marca de tiempo si ningún otro cambio se realiza en el archivo?

No, la replicación DFS no replica los archivos para el que el único cambio es un cambio en la marca de tiempo. Además, la marca de tiempo modificada no se replica en otros miembros del grupo de replicación a menos que se realizan otros cambios en el archivo.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>¿Realiza la replicación DFS replica permisos actualizados en un archivo o carpeta?

Sí. Replicación DFS replica los cambios de permisos de archivos y carpetas. Solo la parte del archivo asociado con la lista de Control de acceso (ACL) se replica, aunque la replicación DFS todavía debe leer todo el archivo en el área de ensayo.


> [!NOTE]
> Cambiar las ACL en un gran número de archivos puede tener un impacto en el rendimiento de replicación. Sin embargo, al usar RDC, la cantidad de datos transferidos es proporcional al tamaño de la ACL, no el tamaño del archivo completo. La cantidad de tráfico de disco es todavía proporcional al tamaño de los archivos porque se deben leer los archivos hacia y desde la carpeta provisional. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>¿Admite la replicación DFS combinar archivos de texto si se produce un conflicto?

La replicación DFS no combina archivos cuando hay un conflicto. Sin embargo, intenta conservar la versión anterior del archivo en el DfsrPrivate oculto\\ConflictandDeleted carpeta del equipo donde se detectó el conflicto.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>¿La replicación DFS usa cifrado durante la transmisión de datos?

Sí. La replicación DFS usa las conexiones de la llamada a procedimiento remoto (RPC) con el cifrado.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>¿Es posible deshabilitar el uso de RPC cifrada?

No. El servicio de replicación DFS usa llamadas a procedimiento remoto (RPC) a través de TCP para replicar los datos. Para proteger las transferencias de datos a través de Internet, el servicio de replicación DFS está diseñado para usar siempre la constante de nivel de autenticación, `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. Esto garantiza que siempre se cifra la comunicación de RPC a través de Internet. Por lo tanto, no es posible deshabilitar el uso de RPC cifrada por el servicio de replicación DFS.

Para obtener más información, consulte los siguientes sitios Web de Microsoft:

  - [Referencia técnica de RPC](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Acerca de la compresión diferencial remota](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes de nivel de autenticación](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>¿Cómo se administran las replicaciones simultáneas?

Hay un administrador de actualización por cada carpeta replicada. Actualizar el trabajo de administradores independientemente entre sí.

De forma predeterminada, un máximo de 16 (cuatro en Windows Server 2003 R2) descargas simultáneas se comparten entre todas las conexiones y los grupos de replicación. Porque no se serializan las conexiones y las actualizaciones del grupo de replicación, no hay ningún orden específico en el que se reciben las actualizaciones. Si se abren dos programaciones, las actualizaciones normalmente se reciben e instaladas de las dos conexiones al mismo tiempo.

### <a name="how-do-i-force-replication-or-polling"></a>¿Cómo se puede forzar la replicación o sondeo?

Puede forzar la replicación inmediatamente mediante el uso de administración de DFS, como se describe en [editar programaciones de replicación](https://technet.microsoft.com/en-us/library/Cc732278). También puede forzar la replicación mediante el uso de la `Sync-DfsReplicationGroup` cmdlet, incluido en el módulo de PowerShell DFSR que se introdujo con Windows Server 2012 R2, o la **Dfsrdiag SyncNow** comando. Puede forzar sondeo mediante el `Update-DfsrConfigurationFromAD` cmdlet, o la **Dfsrdiag PollAD** comando.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>¿Es posible configurar el tiempo de inactividad entre replicaciones para los archivos que cambian con frecuencia?

No. Si la programación está abierta, la replicación DFS se replicarán los cambios como avisos de ellos. No hay ninguna manera de configurar un tiempo de inactividad para los archivos.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>¿Es posible configurar la replicación unidireccional con la replicación DFS?

Sí. Si usa Windows Server 2012 o Windows Server 2008 R2, puede crear una carpeta replicada de solo lectura que se replica el contenido a través de una conexión unidireccional. Para obtener más información, consulte [hacer un solo lectura replica de carpeta en un miembro concreto](http://go.microsoft.com/fwlink/?linkid=156740) (http://go.microsoft.com/fwlink/?LinkId=156740).

No se admite la creación de una conexión de replicación unidireccional con la replicación DFS en Windows Server 2008 o Windows Server 2003 R2. Si lo hace, puede provocar varios problemas, incluidos los errores de topología de comprobación de estado, almacenamiento provisional de problemas con la base de datos de replicación DFS.

Si usa Windows Server 2008 o Windows Server 2003 R2, puede simular una conexión unidireccional mediante la realización de las siguientes acciones:

  - Entrenar a los administradores realizar cambios sólo en los servidores que desea designar como servidores principales. A continuación, permita que los cambios se repliquen en los servidores de destino.  
      
  - Configure los permisos de recurso compartido en los servidores de destino para que los usuarios finales no tiene permisos de escritura. Si no se permiten cambios en los servidores de sucursal, no hay nada que replicar, simulando una conexión unidireccional y mantener baja la utilización de la WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>¿Hay una manera de forzar una replicación completa de todos los archivos incluidos en los archivos sin modificar?

No. Si la replicación DFS considera que los archivos idénticos, no las replicará. Si no se han replicado los archivos modificados, la replicación DFS se replicarán automáticamente cuando se configura para ello. Para sobrescribir la programación configurada, utilice el método WMI **ForceReplicate()** . Sin embargo, esto solo es reemplazar una programación y no fuerza la replicación de archivos sin modificar o idénticos.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>¿Qué ocurre si el miembro principal sufre una pérdida de la base de datos durante la replicación inicial?

Durante la replicación inicial, los archivos del miembro principal siempre tendrá prioridad en la resolución de conflictos que se produce si los miembros receptores tienen diferentes versiones de archivos en el miembro principal. La designación del miembro principal se almacena en Active Directory Domain Services, y la designación se borra cuando esté listo para replicar el miembro principal, pero antes de que todos los miembros de la replicación se replican de grupo.

Si se produce un error en la replicación inicial o se reinicia el servicio de replicación DFS durante la replicación, el miembro principal ve la designación del miembro principal en la base de datos local de la replicación DFS y vuelve a intentar la replicación inicial. Si la base de datos de replicación DFS del miembro principal se pierde después de borrar la designación principal en servicios de dominio de Active Directory, pero antes de que todos los miembros del grupo de replicación complete la replicación inicial, no todos los miembros del grupo de replicación replicar la carpeta porque no hay ningún servidor se designa como el miembro principal. Si esto ocurre, utilice el **Dfsradmin pertenencia /Set /isprimary:true** comando en el servidor miembro principal para restaurar manualmente la designación del miembro principal.

Para obtener más información acerca de la replicación inicial, vea [crear un grupo de replicación](https://technet.microsoft.com/en-us/library/cc725893).


> [!WARNING]
> La designación del miembro principal se usa solo durante el proceso de replicación inicial. Si usas el <STRONG>Dfsradmin</STRONG> comando para especificar un miembro primario para una carpeta replicada después de la replicación se haya completado, la replicación DFS no designar el servidor como un miembro primario en Active Directory Domain Services. Sin embargo, si la base de datos de replicación DFS en el servidor posteriormente sufre daños irreversibles o pérdida de datos, el servidor intenta realizar una replicación inicial que el miembro primario en lugar de recuperar sus datos de otro miembro de la replicación grupo. En esencia, el servidor se convierte en un servidor principal no autorizado, que puede ocasionar conflictos. Por este motivo, especifique al miembro principal manualmente solo si está seguro de que todavía se puede no ha podido la replicación inicial. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>¿Qué ocurre si la programación de replicación se cierra mientras se está replicando un archivo?

Si está habilitada la compresión diferencial remota (RDC) en la conexión, la replicación de un archivo mayor que 64 KB que se inició la replicación inmediatamente antes del cierre de la programación de entrada (o cambiar a **hay ancho de banda**) continúa cuando el programar abre (o cambia a algo distinto **hay ancho de banda**). La replicación continúa desde el estado que tenía cuando se detiene la replicación.

Si RDC está desactivada, la replicación DFS completamente reinicia a la transferencia de archivos. Esto puede demorar cuando el archivo está disponible en el miembro receptor.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>¿Qué ocurre cuando dos usuarios actualizan simultáneamente el mismo archivo en servidores diferentes?

Cuando replicación DFS detecta un conflicto, usa la versión del archivo que se guardó por última vez. Mueve el otro archivo en el DfsrPrivate\\ConflictandDeleted carpeta (bajo la ruta de acceso local de la carpeta replicada en el equipo que resolvió el conflicto). Permanece allí hasta que la limpieza de carpetas conflictos y eliminaciones, que se produce cuando la carpeta conflictos y eliminaciones supera el tamaño configurado o replicación DFS detecta un error de espacio en disco fuera de. No se replica la carpeta conflictos y eliminaciones, y este método de resolución de conflictos evita el problema de directorios transformados que era posible en FRS.

Cuando se produce un conflicto, la replicación DFS registra un evento informativo al registro de eventos de replicación DFS. Este evento no requiere la intervención del usuario por las razones siguientes:

  - No es visible para los usuarios (es visible solo para los administradores del servidor).  
      
  - La replicación DFS, se trata la carpeta conflictos y eliminaciones como una memoria caché. Cuando se alcanza un umbral de cuota, limpia algunos de esos archivos. No hay ninguna garantía de que se guardarán los archivos en conflicto.  
      
  - El conflicto puede residir en un servidor diferente desde el origen del conflicto.  
      

## <a name="staging"></a>Preconfiguración

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>¿La replicación DFS sigue el almacenamiento provisional de archivos cuando se deshabilita una programación o el límite de cuota de ancho de banda, o cuando una conexión se ha deshabilitado manualmente?

No. La replicación DFS no continúa intermedias de archivos fuera de horas de replicación programada, si se ha superado la cuota de limitación de ancho de banda, o cuando las conexiones están deshabilitadas.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>¿La replicación DFS evita que otras aplicaciones tengan acceso a un archivo durante el almacenamiento provisional?

No. La replicación DFS abre los archivos de forma que no bloquea a los usuarios o aplicaciones pueda abrir archivos en la carpeta de replicación. Este método se conoce como "bloqueo oportunista".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>¿Es posible cambiar la ubicación de la carpeta provisional con la herramienta de administración de DFS?

Sí. La ubicación de carpeta provisional está configurada en el **avanzadas** pestaña de la **propiedades** cuadro de diálogo para cada miembro de un grupo de replicación.

### <a name="when-are-files-staged"></a>¿Cuándo se almacenan los archivos?

Los archivos se almacenan en el miembro remitente cuando el miembro receptor solicita el archivo (a menos que el archivo es de 64 KB o menos) como se muestra en la tabla siguiente. Si se deshabilita la compresión diferencial remota (RDC) en la conexión, el archivo se almacena provisionalmente a menos que sea de 256 KB o menos. Los archivos también se almacenan provisionalmente del miembro receptor medida que se transfieren si son inferiores a 64 KB de tamaño, aunque puede configurar esta configuración entre 16 KB y 1 MB. Si la programación está cerrada, no se almacenan los archivos.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Los tamaños de archivo mínimo para los archivos de almacenamiento provisional

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
<td><p>Envío de miembro</p></td>
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

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>¿Qué ocurre si se modifica un archivo después de se almacenan provisionalmente, pero antes de transmitirlos por completo en el sitio remoto?

Si ya se están transmitiendo cualquier parte del archivo, la replicación DFS continúa la transmisión. Si se cambia el archivo antes de la replicación DFS comienza a transmitir el archivo, se envía la versión más reciente del archivo.

## <a name="change-history"></a>Historial de cambios


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Fecha</th>
<th>Descripción</th>
<th>Reason</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 de noviembre de 2018</p></td>
<td><p>Actualizado para Windows Server de 2019.</p></td>
<td><p>Nuevo sistema operativo.</p></td>
</tr>
<tr class="even">
<td><p>9 de octubre de 2013</p></td>
<td><p>¿Actualiza los cuáles son los límites admitidos de la replicación DFS? sección con los resultados de pruebas en Windows Server 2012 R2.</p></td>
<td><p>Actualizaciones de la versión más reciente de Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 de enero de 2013</p></td>
<td><p>¿Agrega la replicación DFS Does continuar el almacenamiento provisional de archivos cuando se deshabilita una programación o el límite de cuota de ancho de banda, o cuando una conexión se ha deshabilitado manualmente? entrada.</p></td>
<td><p>Preguntas de los clientes</p></td>
</tr>
<tr class="even">
<td><p>31 de octubre de 2012</p></td>
<td><p>¿Puede editar los cuáles son los límites admitidos de la replicación DFS? entrada para aumentar el número de archivos replicados en un volumen probado.</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
<tr class="odd">
<td><p>15 de agosto de 2012</p></td>
<td><p>¿Editado Does replicación DFS replique NTFS archivo permisos, flujos de datos alternativos, vínculos físicos y puntos de reanálisis? puntos de entrada para aclarar cómo controla la replicación DFS vínculos físicos y de repetición de análisis.</p></td>
<td><p>Comentarios de los servicios de soporte técnico al cliente</p></td>
</tr>
<tr class="even">
<td><p>13 de junio de 2012</p></td>
<td><p>¿Puede editar el trabajo de la replicación DFS Does en ReFS o volúmenes FAT? entrada para agregar la discusión de ReFS.</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
<tr class="odd">
<td><p>25 de abril de 2012</p></td>
<td><p>¿Editado Does replicación DFS replique NTFS archivo permisos, flujos de datos alternativos, vínculos físicos y puntos de reanálisis? entrada para aclarar cómo controla la replicación DFS vínculos físicos.</p></td>
<td><p>Reducir la confusión potencial</p></td>
</tr>
<tr class="even">
<td><p>30 de marzo de 2011</p></td>
<td><p>¿Editar el .pst de Outlook de la replicación DFS puede replicar o archivos de base de datos de Microsoft Office Access? entrada para corregir el posible impacto del uso de la replicación DFS con archivos .pst y acceso.</p>
<p>¿Cómo puedo mejorar el rendimiento de replicación de agregado?</p></td>
<td><p>Preguntas de los clientes acerca de la entrada anterior, que indican incorrectamente que la replicación de .pst o acceder a los archivos podría dañar la base de datos de replicación DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 de enero de 2011</p></td>
<td><p>Agrega cómo se pueden recuperar archivos desde el ConflictAndDeleted o preexistente carpetas?</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
<tr class="even">
<td><p>20 de octubre de 2010</p></td>
<td><p>Agregado ¿cómo puedo actualizar o reemplazar a un miembro de la replicación DFS?</p></td>
<td><p>Comentarios del cliente</p></td>
</tr>
</tbody>
</table>

