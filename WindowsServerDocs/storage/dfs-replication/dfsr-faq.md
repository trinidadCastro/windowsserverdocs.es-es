---
title: 'Replicación DFS: Preguntas más frecuentes (P+F)'
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 1e11f6c596d7e5eb0bdf379adcf47d21e74e9f6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815628"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>Replicación DFS: Preguntas más frecuentes


Actualizado: 30 de abril de 2019

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

En estas preguntas frecuentes se responde a preguntas sobre la característica Replicación de Sistema de archivos distribuido (DFS) (también conocida como DFS-R o DFSR) para Windows Server.

Para obtener información sobre los espacios de nombres DFS, vea [Espacios de nombres DFS: Preguntas más frecuentes](https://technet.microsoft.com/library/ee404780).

Para obtener más información sobre las novedades de Replicación DFS, consulta los siguientes temas:

  - [Introducción a Espacios de nombres DFS y Replicación DFS](https://technet.microsoft.com/library/jj127250) (en Windows Server 2012)  
      
  - El tema [Novedades de Sistema de archivos distribuido](https://technet.microsoft.com/library/ee307957) de [Cambios de funcionalidad de Windows Server 2008 a Windows Server 2008 R2](https://technet.microsoft.com/library/dd391932)  
      
  - El tema [Sistema de archivos distribuido](https://technet.microsoft.com/library/cc753479) en [Cambios de funcionalidad de Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208)  
      

Para ver una lista de los cambios recientes realizados en este tema, consulte la sección [Historial de cambios](#change-history) de este tema.

      

## <a name="interoperability"></a>Interoperabilidad

### <a name="can-dfs-replication-communicate-with-frs"></a>¿Puede Replicación DFS comunicarse con FRS?

No. Replicación DFS no se comunica con Servicio de replicación de archivos (FRS). Replicación DFS y FRS pueden ejecutarse en el mismo servidor al mismo tiempo, pero nunca deben se deben configurar para replicar las mismas carpetas o subcarpetas, ya que podría provocar la pérdida de datos.

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>¿Puede Replicación DFS reemplazar a FRS para la replicación de SYSVOL?

Sí, Replicación DFS puede reemplazar a FRS para la replicación de SYSVOL en servidores que ejecutan Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Los servidores que ejecutan Windows Server 2003 R2 no admiten el uso de Replicación DFS para replicar la carpeta SYSVOL.

Para obtener más información sobre la replicación de SYSVOL mediante Replicación DFS, consulta la [Guía de migración de la replicación de SYSVOL: FRS a Replicación DFS](https://technet.microsoft.com/library/dd640019).

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>¿Puedo actualizar de FRS a Replicación DFS sin perder los ajustes de configuración?

Sí. Para migrar la replicación de FRS a Replicación DFS, consulta los siguientes documentos:

  - Para migrar la replicación de otras carpetas que no son la carpeta SYSVOL, consulta la [Guía de operaciones DFS: Migración de FRS a Replicación DFS](https://go.microsoft.com/fwlink/?linkid=192776) y [FRS2DFSR: utilidad de migración de FRS a DFSR](https://go.microsoft.com/fwlink/?linkid=195437) (https://go.microsoft.com/fwlink/?LinkID=195437).  
      
  - Para migrar la replicación de la carpeta SYSVOL a Replicación DFS, consulta la [Guía de migración de la replicación de SYSVOL: FRS a Replicación DFS](https://technet.microsoft.com/library/dd640019).  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>¿Puedo usar Replicación DFS en un entorno mixto de Windows/UNIX?

Sí. Aunque Replicación DFS solo admite la replicación de contenido entre servidores que ejecutan Windows Server, los clientes UNIX pueden tener acceso a los recursos compartidos de archivos en instancias de Windows Server. Para ello, instala los servicios para Network File System (NFS) en el servidor de Replicación DFS.

También puedes usar la funcionalidad de cliente SMB/CIFS incluida en muchos clientes UNIX para acceder directamente a los recursos compartidos de archivos de Windows, aunque esta funcionalidad suele estar limitada o requerir modificaciones en el entorno de Windows (por ejemplo, deshabilitar la firma SMB mediante la directiva de grupo).

Replicación DFS interopera con NFS en un servidor que ejecuta un sistema operativo Windows Server, pero no puede replicar un punto de montaje de NFS.

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>¿Puedo usar Servicio de instantáneas de volumen con Replicación DFS?

Sí. Replicación DFS es compatible con los volúmenes de Servicio de instantáneas de volumen (VSS) y las instantáneas anteriores se pueden restaurar correctamente con el cliente de versiones anteriores.

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>¿Puedo usar Copias de seguridad de Windows (Ntbackup.exe) para realizar una copia de seguridad de una carpeta replicada de forma remota?

No, no se admite el uso de Copias de seguridad de Windows (Ntbackup.exe) en un equipo que ejecuta Windows Server 2003 o una versión anterior para realizar una copia de seguridad del contenido de una carpeta replicada en un equipo que ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Para realizar una copia de seguridad de los archivos almacenados en una carpeta replicada, usa Copias de seguridad de Windows Server o Microsoft&reg; System Center Data Protection Manager. Para obtener información sobre la funcionalidad de copia de seguridad y recuperación en Windows Server 2008 R2 y Windows Server 2008, consulta el tema [Copia de seguridad y recuperación](https://technet.microsoft.com/library/Cc754097). Para obtener más información, consulta el tema [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=182261) (https://go.microsoft.com/fwlink/?LinkId=182261).

### <a name="do-file-system-policies-impact-dfs-replication"></a>¿Afectan las directivas del sistema de archivos a Replicación DFS?

Sí. No configures las directivas del sistema de archivos en carpetas replicadas. La directiva del sistema de archivos vuelve a aplicar los permisos NTFS en cada intervalo de actualización de la directiva de grupo. Esto puede dar lugar a infracciones del uso compartido porque un archivo abierto no se replica hasta que se cierra.

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>¿Replica Replicación DFS los buzones hospedados en Microsoft Exchange Server?

No. Replicación DFS no se puede usar para replicar los buzones hospedados en Microsoft Exchange Server.

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>¿Admite Replicación DFS los filtros de archivos creados por Administrador de recursos del servidor de archivos?

Sí. Sin embargo, la configuración del filtrado de archivos de Administrador de recursos del servidor de archivos (FSRM) debe coincidir en ambos extremos de la replicación. Además, Replicación DFS tiene su propio mecanismo de filtro para archivos y carpetas que puedes usar para excluir determinados archivos y tipos de archivo de la replicación.

A continuación, se muestran los procedimientos recomendados para implementar filtros de archivos o cuotas:

  - La carpeta DfsrPrivate oculta no debe estar sujeta a cuotas ni filtros de archivos.  
      
  - Los archivos filtrados no deben existir en ninguna carpeta replicada antes de habilitar el filtrado.  
      
  - Ninguna carpeta puede superar la cuota antes de habilitar la cuota.  
      
  - Debes usar las cuotas máximas con precaución. Es posible que los miembros individuales de un grupo de replicación permanezcan dentro de una cuota antes de la replicación, pero la superen al replicar los archivos. Por ejemplo, si un usuario copia un archivo de 10 megabytes (MB) en el servidor A (que se encuentra entonces al límite máximo) y otro usuario copia un archivo de 5 MB en el servidor B, cuando se produzca la siguiente replicación, los dos servidores superarán la cuota en 5 megabytes. Esto puede hacer que la Replicación DFS reintente continuamente la replicación de los archivos, lo que provocará lagunas en el vector de versión y posibles problemas de rendimiento.  
      

### <a name="is-dfs-replication-cluster-aware"></a>¿Es Replicación DFS compatible con clústeres?

Sí, Replicación DFS de Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 incluye la posibilidad de agregar un clúster de conmutación por error como miembro de un grupo de replicación. Para obtener más información, consulta [Agregar un clúster de conmutación por error a un grupo de replicación](https://go.microsoft.com/fwlink/?linkid=155085) (https://go.microsoft.com/fwlink/?LinkId=155085). El servicio Replicación DFS en versiones de Windows anteriores a Windows Server 2008 R2 no está pensado para coordinarse con un clúster de conmutación por error y el servicio no conmutará por error a otro nodo.


> [!NOTE]
> Replicación DFS no admite la replicación de archivos en volúmenes compartidos de clúster. 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>¿Es Replicación DFS compatible con la desduplicación de datos?

Sí, Replicación DFS puede replicar carpetas en volúmenes que usan la desduplicación de datos en Windows Server.

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>¿Es Replicación DFS compatible con RIS y WDS?

Sí. Replicación DFS replica volúmenes en los que está habilitado el almacenamiento de instancia única (SIS). Los servicios de instalación remota (RIS), Servicios de implementación de Windows (WDS) y Windows Storage Server usan SIS.

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>¿Es posible usar Replicación DFS con Archivos sin conexión?

Puedes usar Replicación DFS y Archivos sin conexión de forma segura en escenarios en los que solo haya un usuario cada vez que escriba en los archivos. Esto resulta útil para los usuarios que viajan entre dos sucursales y desean tener acceso a sus archivos en cualquiera de ellas o mientras están sin conexión. Archivos sin conexión almacenan en caché local los archivos para su uso sin conexión y Replicación DFS replica los datos entre cada sucursal.

No uses Replicación DFS con Archivos sin conexión en un entorno de varios usuarios porque Replicación DFS no proporciona ningún mecanismo de bloqueo distribuido ni capacidad de desprotección de archivos. Si dos usuarios modifican el mismo archivo al mismo tiempo en servidores diferentes, Replicación DFS mueve el archivo anterior a la carpeta DfsrPrivate\\ConflictandDeleted (que se encuentra en la ruta de acceso local de la carpeta replicada) durante la siguiente replicación.

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>¿Qué aplicaciones antivirus son compatibles con Replicación DFS?

Las aplicaciones antivirus pueden provocar una replicación excesiva si sus actividades de exploración modifican los archivos de una carpeta replicada. Para más información, [Prueba de la interoperabilidad de las aplicaciones antivirus con Replicación DFS ](https://go.microsoft.com/fwlink/?linkid=73990) (https://go.microsoft.com/fwlink/?LinkId=73990).

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>¿Cuáles son las ventajas de usar Replicación DFS en lugar de Windows SharePoint Services?

Windows&reg; SharePoint&reg; Services proporciona una estricta coherencia en forma de funcionalidad de desprotección de archivos que Replicación DFS no ofrece. Si te preocupa que varias personas editen el mismo archivo, se recomienda usar Windows SharePoint Services. Windows SharePoint Services 2.0 con Service Pack 2 está disponible como parte de Windows Server 2003 R2. Windows SharePoint Services se puede descargar desde el sitio web de Microsoft; no se incluye en las versiones más recientes de Windows Server. Sin embargo, si vas a replicar datos en varios sitios y los usuarios no editarán los mismos archivos al mismo tiempo, Replicación DFS proporciona un mayor ancho de banda y una administración más sencilla.

## <a name="limitations-and-requirements"></a>Limitaciones y requisitos

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>¿Puede Replicación DFS replicar entre sucursales sin una conexión VPN?

Sí, suponiendo que haya un vínculo de red de área extensa (WAN) privada (no por Internet) que conecte las sucursales. Sin embargo, debes abrir los puertos adecuados en los firewalls externos. Replicación DFS utiliza el asignador de puntos de conexión de RPC (puerto 135) y un puerto efímero asignado aleatoriamente por encima de 1024. Puedes usar la herramienta de línea de comandos **Dfsrdiag** para especificar un puerto estático en lugar de uno dinámico. Para obtener más información sobre cómo especificar el asignador de puntos de conexión de RPC, consulta el [artículo 154596](https://go.microsoft.com/fwlink/?linkid=73991) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=73991).

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>¿Puede Replicación DFS replicar archivos cifrados con el sistema de cifrado de archivos?

No. Replicación DFS no replicará los archivos o carpetas cifrados con EFS (Sistema de cifrado de archivos). Si un usuario cifra un archivo que se había replicado anteriormente, Replicación DFS elimina el archivo de todos los demás miembros del grupo de replicación. Esto garantiza que la única copia disponible del archivo sea la versión cifrada en el servidor.

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>¿Puede Replicación DFS replicar archivos de base de datos de Outlook.pst o Microsoft Office Access?

Replicación DFS puede replicar de forma segura archivos de carpeta personales de Microsoft Outlook (.pst) y archivos de Microsoft Access solo si se almacenan para fines de archivado y no se accede a ellos a través de la red mediante un cliente, como Outlook o Access (para abrir archivos .pst o Access, copia primero los archivos en un dispositivo de almacenamiento local). Los motivos para ello son los siguientes:

  - Abrir archivos .pst a través de conexiones de red podría provocar daños en los datos de los archivos .pst. Para obtener más información sobre por qué no se puede tener acceso seguro a los archivos .pst a través de una red, consulta el [artículo 297019](https://go.microsoft.com/fwlink/?linkid=125363) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=125363).  
      
  - Los archivos .pst y Access tienden a permanecer abiertos durante largos períodos de tiempo mientras un cliente, como Outlook u Office Access, accede a ellos. Esto impide que Replicación DFS replique estos archivos hasta que se cierren.  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>¿Puedo usar Replicación DFS en un grupo de trabajo?

No. Replicación DFS confía en Active Directory&reg; Domain Services para la configuración. Solo funcionará en un dominio.

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>¿Se puede replicar más de una carpeta en un solo servidor?

Sí. Replicación DFS puede replicar varias carpetas entre servidores. Asegúrate de que cada una de las carpetas replicadas tenga una ruta de acceso raíz única y que no se superpongan. Por ejemplo, D:\\Ventas y D:\\Contabilidad pueden ser rutas de acceso raíz de dos carpetas replicadas, pero D:\\Ventas y D:\\Ventas\\Informes no pueden ser rutas de acceso raíz de dos carpetas replicadas.

### <a name="does-dfs-replication-require-dfs-namespaces"></a>¿Requiere Replicación DFS espacios de nombres DFS?

No. Replicación DFS y los espacios de nombres DFS se pueden usar por separado o juntos. Además, Replicación DFS se puede usar para replicar espacios de nombres DFS independientes, lo que no era posible con FRS.

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>¿Requiere Replicación DFS la sincronización de la hora entre servidores?

No. Replicación DFS no requiere explícitamente la sincronización de la hora entre servidores. Sin embargo, Replicación DFS requiere que los relojes de los servidores coincidan estrechamente. Los relojes de los servidores deben establecerse con una diferencia máxima de cinco minutos (de forma predeterminada) para que la autenticación Kerberos funcione correctamente. Por ejemplo, Replicación DFS usa marcas de tiempo para determinar qué archivo tiene prioridad en caso de conflicto. Los tiempos precisos también son importantes en la recolección de elementos no utilizados, las programaciones y otras características.

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>¿Admite Replicación DFS la replicación de un volumen completo?

Sí. Sin embargo, primero debes instalar Windows Server 2003 Service Pack 2 o la revisión. Para obtener más información, consulta el [artículo 920335](https://go.microsoft.com/fwlink/?linkid=76776) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=76776). Además, la replicación de un volumen completo puede producir los problemas siguientes:

  - Si el volumen contiene un archivo de paginación de Windows, se produce un error en la replicación y se registra el evento 4312 de DFSR en el registro de eventos del sistema.  
      
  - Replicación DFS establece los atributos Sistema y Oculto en la carpeta replicada en los servidores de destino. Esto se debe a que Windows aplica de forma predeterminada los atributos Sistema y Oculto a la carpeta raíz del volumen. Si la ruta de acceso local de la carpeta replicada en los servidores de destino también es una raíz de volumen, no se realizarán más cambios en los atributos de carpeta.  
      
  - Al replicar un volumen que contiene la carpeta del sistema de Windows, Replicación DFS reconoce la carpeta %WINDIR% y no la replica. Sin embargo, Replicación DFS replica las carpetas usadas por aplicaciones que no son de Microsoft, lo que puede provocar errores en las aplicaciones en los servidores de destino si las aplicaciones tienen problemas de interoperabilidad con Replicación DFS.  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>¿Admite Replicación DFS RPC a través de HTTP?

No.

### <a name="does-dfs-replication-work-across-wireless-networks"></a>¿Funciona Replicación DFS a través de redes inalámbricas?

Sí. Replicación DFS es independiente del tipo de conexión.

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>¿Funciona Replicación DFS en volúmenes ReFS o FAT?

No. Replicación DFS es compatible con volúmenes con formato de sistema de archivos NTFS solamente; no es compatible con los sistemas de archivos ReFS (Sistema de archivos resistentes) y FAT. Replicación DFS requiere NTFS porque utiliza el diario de cambios de NTFS y otras características del sistema de archivos NTFS.

### <a name="does-dfs-replication-work-with-sparse-files"></a>¿Funciona Replicación DFS con archivos dispersos?

Sí. Puedes replicar archivos dispersos. El atributo **Disperso** se conserva en el miembro receptor.

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>¿Es necesario iniciar sesión como administrador para replicar archivos?

No. Replicación DFS es un servicio que se ejecuta en la cuenta local del sistema, por lo que no es necesario iniciar sesión como administrador. Sin embargo, debes ser administrador del dominio o administrador local de los servidores de archivos afectados para realizar cambios en la configuración de Replicación DFS.

Para obtener más información, consulta "Requisitos de seguridad y delegación de Replicación DFS" en el apartado [Delegar la capacidad de administrar Replicación DFS](https://go.microsoft.com/fwlink/?linkid=182294) (https://go.microsoft.com/fwlink/?LinkId=182294).

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>¿Cómo puedo actualizar o reemplazar un miembro de Replicación DFS?

Para actualizar o reemplazar un miembro de Replicación DFS, consulta esta entrada en Pregunta en el Blog del equipo de servicios de directorio: [Sustitución del hardware o sistema operativo de un miembro de DFSR](https://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx).

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>¿Es Replicación DFS adecuada para replicar perfiles itinerantes?

Sí. Se admiten ciertos escenarios al replicar perfiles de usuario itinerantes. Para obtener información sobre los escenarios admitidos, consulta [Instrucción de soporte técnico de Microsoft sobre los datos de perfil de usuario replicados](https://go.microsoft.com/fwlink/?linkid=201282) (https://go.microsoft.com/fwlink/?LinkId=201282).

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>¿Hay un límite de caracteres de archivo o un límite para la profundidad de la carpeta?

Windows y Replicación DFS admiten rutas de acceso de carpeta con un máximo de 32 000 caracteres. Replicación DFS no tiene la limitación de rutas de acceso de carpeta de 260 caracteres.

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>¿Deben residir los miembros de un grupo de replicación en el mismo dominio?

No. Los grupos de replicación pueden abarcar varios dominios dentro de un solo bosque, pero no de bosques distintos.

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>¿Cuáles son los límites admitidos de Replicación DFS?

En la lista siguiente se proporciona un conjunto de directrices de escalabilidad probadas por Microsoft y que se aplican a Windows Server 2012 R2, Windows Server 2016 y Windows Server 2019

  - Tamaño de todos los archivos replicados en un servidor: 100 terabytes.  
      
  - Número de archivos replicados en un volumen: 70 millones.  
      
  - Tamaño máximo de archivo: 250 gigabytes.  
      


> [!IMPORTANT]
> Al crear grupos de replicación con un gran número de archivos o archivos de gran tamaño, se recomienda exportar un clon de la base de datos y usar técnicas de inicialización previa para minimizar la duración de la replicación inicial. Para obtener más información, consulta [Sincronización inicial de Replicación DFS en Windows Server 2012 R2: ataque de los clones](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877). 
<br>


En la lista siguiente se proporciona un conjunto de directrices de escalabilidad probadas por Microsoft en Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008:

  - Tamaño de todos los archivos replicados en un servidor: 10 terabytes.  
      
  - Número de archivos replicados en un volumen: 11 millones.  
      
  - Tamaño máximo de archivo: 64 gigabytes.  
      


> [!NOTE]
> Ya no hay un límite en cuanto al número de grupos de replicación, carpetas replicadas, conexiones o miembros del grupo de replicación. 
<br>


Para obtener una lista de las instrucciones de escalabilidad probadas por Microsoft para Windows Server 2003 R2, consulta [Instrucciones de escalabilidad de Replicación DFS](https://go.microsoft.com/fwlink/?linkid=75043) (https://go.microsoft.com/fwlink/?LinkId=75043).

### <a name="when-should-i-not-use-dfs-replication"></a>¿Cuándo no se debe utilizar Replicación DFS?

No utilices Replicación DFS en un entorno en el que varios usuarios actualizan o modifican los mismos archivos simultáneamente en servidores diferentes. Esto puede provocar que Replicación DFS mueva copias en conflicto de los archivos a la carpeta oculta DfsrPrivate\\ConflictandDeleted.

Cuando varios usuarios necesitan modificar los mismos archivos al mismo tiempo en servidores diferentes, utiliza la característica de desprotección de archivos de Windows SharePoint Services para asegurarte de que solo un usuario está trabajando en un archivo. Windows SharePoint Services 2.0 con Service Pack 2 está disponible como parte de Windows Server 2003 R2. Windows SharePoint Services se puede descargar desde el sitio web de Microsoft; no se incluye en las versiones más recientes de Windows Server.

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>¿Por qué se necesita actualizar el esquema para Replicación DFS?

Replicación DFS usa nuevos objetos en el contexto de nombres de dominio de Active Directory Domain Services para almacenar la información de configuración. Estos objetos se crean al actualizar el esquema de Servicios de dominio de Active Directory. Para obtener más información, consulta [Revisar los requisitos de Replicación DFS](https://go.microsoft.com/fwlink/?linkid=182264) (https://go.microsoft.com/fwlink/?LinkId=182264).

## <a name="monitoring-and-management-tools"></a>Herramientas de supervisión y administración

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>¿Puedo automatizar el informe de mantenimiento para recibir advertencias?

Sí. Existen tres formas de automatizar los informes de mantenimiento:

  - Usar el módulo de Windows PowerShell de DFSR incluido en Windows Server 2012 R2 o DfsrAdmin.exe junto con tareas programadas para generar informes de mantenimiento con regularidad. Para obtener más información, consulta [Automatización de informes de mantenimiento de Replicación DFS](https://go.microsoft.com/fwlink/?linkid=74010) (https://go.microsoft.com/fwlink/?LinkId=74010).  
      
  - Usar el módulo de administración de Replicación DFS para System Center Operations Manager con el fin de crear alertas basadas en condiciones especificadas.  
      
  - Usar el proveedor de WMI de Replicación DFS para crear scripts de alertas.  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>¿Puedo usar Microsoft System Center Operations Manager para supervisar Replicación DFS?

Sí. Para obtener más información, consulte [Módulo de administración de Replicación DFS para System Center Operations Manager 2007](https://go.microsoft.com/fwlink/?linkid=182265) en el centro de descarga Microsoft (https://go.microsoft.com/fwlink/?LinkId=182265).

### <a name="does-dfs-replication-support-remote-management"></a>¿Admite Replicación DFS la administración remota?

Sí. Replicación DFS admite la administración remota mediante la consola de administración de DFS y el comando **Agregar grupo de replicación**. Por ejemplo, en el servidor A, puedes conectarte a un grupo de replicación definido en el bosque con los servidores A y B como miembros.

Administración de DFS se incluye en Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 y Windows Server 2003 R2. Para administrar Replicación DFS desde otras versiones de Windows, usa Escritorio remoto o [Herramientas de administración remota del servidor para Windows 7](https://technet.microsoft.com/library/Ee449475).


> [!IMPORTANT]
> Para ver o administrar los grupos de replicación que contienen carpetas o miembros replicados de solo lectura que son clústeres de conmutación por error, debes usar la versión de Administración de DFS que se incluye con Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, <a href="https://go.microsoft.com/fwlink/p/?linkid=238560">Herramientas de administración remota del servidor para Windows 8</a> o <a href="https://technet.microsoft.com/library/ee449475">Herramientas de administración remota del servidor para Windows 7</a>. 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>¿Funcionan Ultrasound y Sonar con Replicación DFS?

No. Replicación DFS tiene su propio conjunto de herramientas de supervisión y diagnóstico. Ultrasound y Sonar solo son capaces de supervisar FRS.

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>¿Cómo se pueden recuperar los archivos de las carpetas ConflictAndDeleted o PreExisting?

Para recuperar los archivos perdidos, restaure los archivos de la carpeta del sistema de archivos o de la carpeta compartida mediante el historial de archivos, el comando **Restaurar versiones anteriores** de Explorador de archivos o mediante la restauración de los archivos desde la copia de seguridad. Para recuperar archivos directamente desde las carpetas ConflictAndDeleted o PreExisting, use los cmdlets de Windows PowerShell `Get-DfsrPreservedFiles` y `Restore-DfsrPreservedFiles` (incluidos con el módulo DFSR en Windows Server 2012 R2) o el script de ejemplo [RestoreDFSR](https://code.msdn.microsoft.com/restoredfsr) de la galería de código de MSDN. Este script está pensado únicamente para la recuperación ante desastres y se proporciona tal cual, sin garantía.

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>¿Hay alguna manera de conocer el estado de la replicación?

Sí. Hay varias maneras de supervisar la replicación:

  - Replicación DFS dispone de un módulo de administración para System Center Operations Manager que proporciona una supervisión proactiva.  
      
  - Administración de DFS incluye un informe de diagnóstico del trabajo pendiente de replicación, la eficacia de la replicación y el número de archivos y carpetas de un grupo de replicación determinado.  
      
  - El módulo de Windows PowerShell de DFSR en Windows Server 2012 R2 contiene cmdlets para iniciar pruebas de propagación y escribir informes de propagación y mantenimiento. Para obtener más información, consulta [Cmdlets de replicación de Sistema de archivos distribuido en Windows PowerShell](https://technet.microsoft.com/library/dn296601.aspx).  
      
  - Dfsrdiag.exe es una herramienta de línea de comandos que puede generar un recuento de los trabajos pendientes o desencadenar una prueba de propagación. Ambas acciones muestran el estado de replicación. La propagación muestra si los archivos se replican en todos los nodos. El trabajo pendiente muestra cuántos archivos se deben replicar aún para que los dos equipos estén sincronizados. El recuento de trabajos pendientes es el número de actualizaciones que un miembro del grupo de replicación no ha procesado. En los equipos que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, Dfsrdiag.exe también se pueden mostrar las actualizaciones que Replicación DFS está replicando actualmente.  
      
  - Los scripts pueden usar WMI para recopilar la información de los trabajos pendientes, manualmente o a través de MOM.  
      

## <a name="performance"></a>Rendimiento

### <a name="does-dfs-replication-support-dial-up-connections"></a>¿Admite Replicación DFS conexiones de acceso telefónico?

Aunque Replicación DFS funcionará en velocidades de acceso telefónico, pueden generarse trabajos pendientes si hay un gran número de cambios para replicar. Si se realizan pequeños cambios en los archivos existentes, Replicación DFS con compresión diferencial remota (RDC) proporcionará un rendimiento mucho mayor que copiar el archivo directamente.

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>¿Realiza Replicación DFS la detección de ancho de banda?

No. Replicación DFS no realiza la detección de ancho de banda. Puedes configurar Replicación DFS para usar una cantidad limitada de ancho de banda en cada conexión (límite de ancho de banda). Sin embargo, Replicación DFS no reduce más el uso de ancho de banda si la interfaz de red se satura y Replicación DFS puede saturar el vínculo durante breves períodos. El límite de ancho de banda con Replicación DFS no es completamente preciso porque Replicación DFS limita el ancho de banda mediante la limitación de las llamadas RPC. Como resultado, pueden interferir varios búferes de niveles inferiores de la pila de red (incluido RPC), lo que provoca ráfagas de tráfico de red.

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>¿Limita Replicación DFS el ancho de banda por programación, servidor o conexión?

Si configuras el límite de ancho de banda al especificar la programación, todas las conexiones para ese grupo de replicación usarán esa configuración para la limitación de ancho de banda. El límite de ancho de banda también se puede establecer como configuración de nivel de conexión mediante Administración de DFS.

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>¿Usa Replicación DFS Active Directory Domain Services para calcular los vínculos a sitios y los costes de conexión?

No. Replicación DFS usa la topología definida por el administrador, que es independiente de la gestión de costes de sitios de Active Directory Domain Services.

### <a name="how-can-i-improve-replication-performance"></a>¿Cómo puedo mejorar el rendimiento de la replicación?

Para obtener información acerca de los distintos métodos para optimizar el rendimiento de la replicación, consulta [Ajuste del rendimiento de la replicación en DFSR](https://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx) en [Pregunta en el Blog del equipo de servicios de directorio](https://blogs.technet.com/b/askds/).

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>¿Cómo evita Replicación DFS la saturación de una conexión?

En Replicación DFS, puedes establecer el ancho de banda máximo que deseas usar en una conexión y el servicio mantiene ese nivel de uso de red. Este comportamiento es diferente del de Servicio de transferencia inteligente en segundo plano (BITS) y Replicación DFS no satura la conexión si se establece correctamente.

Sin embargo, el límite de ancho de banda no tiene una precisión de 100 % y Replicación DFS puede saturar el vínculo durante breves períodos de tiempo. Esto se debe a que Replicación DFS limita el ancho de banda mediante la limitación de las llamadas RPC. Dado que este proceso se basa en varios búferes en niveles inferiores de la pila de red, incluido RPC, el tráfico de la replicación tiende a viajar en ráfagas que, en ocasiones, pueden saturar los vínculos de red.

Replicación DFS en Windows Server 2008 incluye varias mejoras de rendimiento, como se describe en [Sistema de archivos distribuido](https://technet.microsoft.com/library/Cc753479), un tema de [Cambios en la funcionalidad de Windows Server 2003 con SP1 a Windows Server 2008](https://technet.microsoft.com/library/cc753208).

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>¿Cómo es el rendimiento de Replicación DFS en comparación con FRS?

Replicación DFS es mucho más rápida que FRS, especialmente cuando se realizan pequeños cambios en archivos de gran tamaño y RDC está habilitado. Por ejemplo, con RDC, un pequeño cambio en una presentación de PowerPoint&reg; de 2 MB puede dar lugar a que solo se envíen 60 kilobytes (KB) a través de la red, lo que supone un ahorro del 97 % de bytes transferidos.

RDC no se usa en archivos de menos de 64 KB y es posible que no sea beneficioso en las LAN de alta velocidad en las que no existe contención sobre el ancho de banda de red. RDC se puede deshabilitar en cada conexión mediante Administración de DFS.

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>¿Con qué frecuencia replica Replicación DFS los datos?

Los datos se replican según la programación que establezcas. Por ejemplo, puedes establecer la programación en intervalos de 15 minutos, siete días a la semana. Durante estos intervalos, se habilita la replicación. La replicación se inicia poco después de que se detecte un cambio en el archivo (normalmente en segundos).

La programación del grupo de replicación puede establecerse en horario universal coordinado (UTC), mientras que la programación de la conexión se establece en la hora local del miembro receptor. Ten esto en cuenta cuando el grupo de replicación abarque varias zonas horarias. La hora local es la hora del miembro que hospeda la conexión entrante. La programación mostrada de la conexión entrante y la correspondiente conexión saliente reflejan las diferencias de zona horaria cuando la programación se establece en la hora local.

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>¿Cuántos recursos del sistema de mi servidor consumirá Replicación DFS?

Los recursos de disco, memoria y CPU que Replicación DFS usa dependen de varios factores, como el número y el tamaño de los archivos, la velocidad de cambio, el número de miembros del grupo de replicación y el número de carpetas replicadas. Además, algunos recursos son más difíciles de estimar. Por ejemplo, la tecnología de Motor de almacenamiento extensible (ESE) utilizada para la base de datos de Replicación DFS puede consumir un gran porcentaje de memoria disponible, que libera a petición. Las aplicaciones distintas de Replicación DFS pueden hospedarse en el mismo servidor en función de la configuración del servidor. Sin embargo, al hospedar varias aplicaciones o roles de servidor en un solo servidor, es importante que pruebe esta configuración antes de implementarla en un entorno de producción.

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>¿Qué ocurre si se produce un error en un vínculo WAN durante la replicación?

Si la conexión deja de funcionar, Replicación DFS seguirá intentando realizar la replicación mientras la programación esté abierta. También se anotarán errores de conectividad en el registro de eventos de Replicación DFS, recopilados mediante MOM (de forma proactiva a través de alertas) y en el informe de mantenimiento de Replicación DFS (de forma reactiva, por ejemplo, cuando un administrador lo ejecuta).

## <a name="remote-differential-compression-details"></a>Detalles de la compresión diferencial remota

### <a name="are-changes-compressed-before-being-replicated"></a>¿Se comprimen los cambios antes de su replicación?

Sí. Las partes modificadas de los archivos se comprimen antes de enviarse para todos los tipos de archivo excepto los siguientes (que ya están comprimidos): .wma, .wmv, .zip, .jpg, .mpg, .mpeg, .m1v, .mp2, .mp3, .mpa, .cab, .wav, .snd, .au, .asf, .wm, .avi, .z, .gz, .tgz y .frx. La configuración de la compresión de estos tipos de archivo no es configurable en Windows Server 2003 R2.

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>¿Puede un administrador desactivar RDC o cambiar el umbral?

Sí. Puedes desactivar RDC a través de la página de propiedades de una conexión determinada. Deshabilitar RDC puede reducir el uso de CPU y la latencia de replicación en vínculos de alta velocidad de red de área local (LAN) que no tienen restricciones de ancho de banda o para los grupos de replicación que se componen principalmente de archivos menores de 64 KB. Si decides deshabilitar RDC en una conexión, prueba la eficacia de la replicación antes y después del cambio para comprobar que ha mejorado el rendimiento de la replicación.

Puedes cambiar el umbral del tamaño de RDC mediante el comando **Dfsradmin Connection Set** (Conjunto de conexiones Dfsradmin), el proveedor WMI de Replicación DFS o mediante la edición manual del archivo XML de configuración.

### <a name="does-rdc-work-on-all-file-types"></a>¿Funciona RDC en todos los tipos de archivo?

Sí. RDC calcula las diferencias en el nivel de bloque, independientemente del tipo de datos del archivo. Sin embargo, RDC funciona de forma más eficaz en determinados tipos de archivo, como documentos de Word, archivos PST e imágenes VHD.

### <a name="how-does-rdc-work-on-a-compressed-file"></a>¿Cómo funciona RDC en un archivo comprimido?

Replicación DFS usa RDC, que calcula los bloques del archivo que han cambiado y envía solo esos bloques a través de la red. Replicación DFS no necesita saber nada sobre el contenido del archivo, solo los bloques que han cambiado.

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>¿Se habilita RDC entre archivos al actualizar a Windows Server Enterprise Edition o Datacenter Edition?

Las ediciones Standard de Windows Server no admiten RDC entre archivos. Sin embargo, se habilita automáticamente cuando se actualiza a una edición que admite RDC entre archivos, o si un miembro de la conexión de replicación está ejecutando una edición compatible. Para obtener una lista de las ediciones que admiten RDC entre archivos, consulta ¿Qué ediciones del sistema operativo Windows admiten RDC entre archivos?

### <a name="is-rdc-true-block-level-replication"></a>¿Es RDC una replicación de nivel de bloque verdadero?

No. RDC es un protocolo de uso general para comprimir la transferencia de archivos. Replicación DFS usa RDC en los bloques en el nivel de archivo, no en el nivel de bloque de disco. RDC divide un archivo en bloques. Para cada bloque de un archivo, calcula una firma, que es un número pequeño de bytes que puede representar al bloque mayor. El conjunto de firmas se transfiere desde el servidor al cliente. El cliente compara las firmas del servidor con las suyas propias. A continuación, el cliente solicita al servidor que envíe solo los datos de las firmas que no estén ya en el cliente.

### <a name="what-happens-if-i-rename-a-file"></a>¿Qué ocurre si cambio el nombre de un archivo?

Replicación DFS cambia el nombre del archivo en todos los demás miembros del grupo de replicación durante la siguiente replicación. Se realiza un seguimiento de los archivos mediante un identificador único, por lo que cambiar el nombre de un archivo y mover el archivo dentro de la réplica no afecta a la capacidad que tiene Replicación DFS para replicar un archivo.

### <a name="what-is-cross-file-rdc"></a>¿Qué es RDC entre archivos?

RDC entre archivos permite a Replicación DFS usar RDC, incluso cuando no existe un archivo con el mismo nombre en el extremo del cliente. RDC entre archivos usa una heurística para determinar archivos similares al archivo que se debe replicar y usa bloques de archivos similares que son idénticos al archivo de replicación para minimizar la cantidad de datos transferidos a través de la WAN. RDC entre archivos puede usar bloques de hasta cinco archivos similares en este proceso.

Para usar RDC entre archivos, un miembro de la conexión de replicación debe estar ejecutando una edición de Windows que admita RDC entre archivos. Para obtener una lista de las ediciones que admiten RDC entre archivos, consulta ¿Qué ediciones del sistema operativo Windows admiten RDC entre archivos?

### <a name="what-is-rdc"></a>¿Qué es RDC?

RDC (compresión diferencial remota) es un protocolo cliente-servidor que se puede usar para actualizar archivos de forma eficaz a través de una red de ancho de banda limitado. RDC detecta las inserciones, las eliminaciones y las reorganizaciones de datos en archivos, lo que permite a Replicación DFS replicar solo las partes modificadas de un archivo. RDC solo se usa para archivos de 64 KB o mayores de forma predeterminada. RDC puede usar una versión anterior de un archivo con el mismo nombre en la carpeta replicada o en la carpeta DfsrPrivate\\ConflictandDeleted (que se encuentra en la ruta de acceso local de la carpeta replicada).

### <a name="when-is-rdc-used-for-replication"></a>¿Cuándo se usa RDC para la replicación?

RDC se usa cuando el archivo supera un umbral de tamaño mínimo. Este umbral de tamaño es de 64 KB de forma predeterminada. Una vez replicado un archivo que supera ese umbral, las versiones actualizadas del archivo siempre usan RDC, a menos que se cambie una parte grande del archivo o se deshabilite RDC.

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>¿Qué ediciones del sistema operativo Windows admite RDC entre archivos?

Para usar RDC entre archivos, un miembro de la conexión de replicación debe estar ejecutando una edición del sistema operativo Windows que admita RDC entre archivos. En la siguiente tabla se muestran las ediciones del sistema operativo Windows que admiten RDC entre archivos.

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

\* Opcionalmente, puedes deshabilitar RDC entre archivos en Windows Server 2012 R2.

## <a name="replication-details"></a>Detalles de la replicación

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>¿Puedo cambiar la ruta de acceso de una carpeta replicada una vez creada?

No. Si necesitas cambiar la ruta de acceso de una carpeta replicada, debes eliminarla en Administración de DFS y volver a agregarla como una nueva carpeta replicada. A continuación, Replicación DFS usa RDC (compresión diferencial remota) para realizar una sincronización que determine si los datos son los mismos en los miembros de envío y recepción. No vuelve a replicar todos los datos de la carpeta.

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>¿Puedo configurar qué atributos de archivo se van a replicar?

No, no puedes configurar los atributos de archivo que Replicación DFS replica.

Para obtener una lista de valores de atributo y sus descripciones, consulta [Atributos de archivo](https://go.microsoft.com/fwlink/?linkid=182268) en MSDN (https://go.microsoft.com/fwlink/?LinkId=182268).

Los siguientes valores de atributo se establecen mediante la función `SetFileAttributes dwFileAttributes`, y se replican mediante Replicación DFS. Los cambios en estos valores de atributo desencadenan la replicación de los atributos. El contenido del archivo no se replica a menos que también cambie el contenido. Para obtener más información, consulta [Función SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) en MSDN Library (https://go.microsoft.com/fwlink/?LinkId=182269).

  - FILE\_ATTRIBUTE\_HIDDEN  
      
  - FILE\_ATTRIBUTE\_READONLY  
      
  - FILE\_ATTRIBUTE\_SYSTEM  
      
  - FILE\_ATTRIBUTE\_NOT\_CONTENT\_INDEXED  
      
  - FILE\_ATTRIBUTE\_OFFLINE  
      

Replicación DFS replica los siguientes valores de atributo, pero no desencadenan la replicación.

  - FILE\_ATTRIBUTE\_ARCHIVE  
      
  - FILE\_ATTRIBUTE\_NORMAL  
      

Los siguientes valores de atributo de archivo también desencadenan la replicación, aunque no se pueden establecer con la función `SetFileAttributes` (usa la función `GetFileAttributes` para ver los valores de atributo).

  - FILE\_ATTRIBUTE\_REPARSE\_POINT  
      

> [!NOTE]
> Replicación DFS no replica los valores de atributo de punto de reanálisis a menos que la etiqueta de reanálisis sea IO_REPARSE_TAG_SYMLINK. Los archivos con las etiquetas de reanálisis IO_REPARSE_TAG_DEDUP, IO_REPARSE_TAG_SIS o IO_REPARSE_TAG_HSM se replican como archivos normales. Sin embargo, la etiqueta de reanálisis y los búferes de datos de reanálisis no se replican en otros servidores porque el punto de reanálisis solo funciona en el sistema local. 
<br>

  - FILE\_ATTRIBUTE\_COMPRESSED  
      
  - FILE\_ATTRIBUTE\_ENCRYPTED  
      

> [!NOTE]
> Replicación DFS no replica los archivos cifrados con EFS (Sistema de cifrado de archivos). Replicación DFS replica los archivos cifrados mediante software que no es de Microsoft, pero solo si no se establece el valor del atributo FILE_ATTRIBUTE_ENCRYPTED en el archivo. 
<br>

  - FILE\_ATTRIBUTE\_SPARSE\_FILE  
      
  - FILE\_ATTRIBUTE\_DIRECTORY  
      

Replicación DFS no replica el atributo FILE\_ATTRIBUTE\_TEMPORARY.

### <a name="can-i-control-which-member-is-replicated"></a>¿Puedo controlar qué miembro se replica?

Sí. Puedes elegir una topología al crear un grupo de replicación. O bien, puedes seleccionar **Sin topología** y configurar manualmente las conexiones una vez creado el grupo de replicación.

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>¿Puedo inicializar un miembro del grupo de replicación con datos antes de la replicación inicial?

Sí. Replicación DFS admite la copia de archivos en un miembro del grupo de replicación antes de la replicación inicial. Esta "preconfiguración" puede reducir drásticamente la cantidad de datos replicados durante la replicación inicial.

La replicación inicial no necesita replicar contenido cuando los archivos solo se diferencian en los atributos reales o las marcas de tiempo. Un atributo real es un atributo que se puede establecer mediante la función `SetFileAttributes` de Win32. Para obtener más información, consulta [Función SetFileAttributes](https://go.microsoft.com/fwlink/?linkid=182269) en MSDN Library (https://go.microsoft.com/fwlink/?LinkId=182269). Si dos archivos difieren en otros atributos, como la compresión, se replica el contenido del archivo.

Para preconfigurar un miembro del grupo de replicación, copia los archivos en la carpeta adecuada de los servidores de destino, crea el grupo de replicación y, a continuación, elige un miembro principal. Selecciona el miembro que contenga los archivos más actualizados que deseas replicar, ya que el contenido del miembro principal se considera "autoritativo". Esto significa que, durante la replicación inicial, los archivos del miembro principal siempre sobrescribirán otras versiones de los archivos en otros miembros del grupo de replicación.

Para obtener información sobre la inicialización previa y la clonación de la base de datos de DFSR, consulta [Sincronización inicial de Replicación DFS en Windows Server 2012 R2: ataque de los clones](https://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx).

Para obtener más información sobre la replicación inicial, consulta [Crear un grupo de replicación](https://technet.microsoft.com/library/cc725893).

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>¿Soluciona Replicación DFS los problemas comunes del servicio de replicación de archivos?

Sí. Replicación DFS soluciona tres problemas comunes de FRS:

  - Ajustes del diario: Replicación DFS recupera los ajustes del diario sobre la marcha. Cada archivo o carpeta existente se marcará como journalWrap y se comprobará con el sistema de archivos antes de que se vuelva a habilitar la replicación. Durante la recuperación, este volumen no está disponible para la replicación en ninguna dirección.  
      
  - Replicación excesiva: Para evitar la replicación excesiva, Replicación DFS usa un sistema de créditos.  
      
  - Carpetas modificadas: Para evitar los nombres de carpetas modificados, Replicación DFS almacena los datos en conflicto en una carpeta DfsrPrivate\\ConflictandDeleted oculta (que se encuentra en la ruta de acceso local de la carpeta replicada). Por ejemplo, al crear varias carpetas al mismo tiempo con nombres idénticos en servidores diferentes replicados mediante FRS, se cambia el nombre de las carpetas más antiguas. En su lugar, Replicación DFS mueve las carpetas antiguas a la carpeta local de conflictos y eliminaciones.  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>¿Replica Replicación DFS los archivos en orden cronológico?

No. Los archivos se pueden replicar desordenados.

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>¿Replica Replicación DFS los archivos que se están usando en otra aplicación?

Si una aplicación abre un archivo y crea un bloqueo de archivo (evitando que lo usen otras aplicaciones mientras está abierto), Replicación DFS no replicará el archivo hasta que se cierre. Si la aplicación abre el archivo con acceso de lectura y uso compartido, el archivo se puede replicar.

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>¿Replica Replicación DFS los permisos de archivos NTFS, los flujos de datos alternativos, los vínculos físicos y los puntos de reanálisis?

  - Replicación DFS replica los permisos de archivo NTFS y los flujos de datos alternativos.  
      
  - Microsoft no admite la creación de vínculos físicos NTFS en o desde archivos de una carpeta replicada; si lo haces, pueden producirse problemas de replicación con los archivos afectados. Replicación DFS omite los archivos de vínculo físico y no se replican. Los puntos de unión tampoco se replican y Replicación DFS registra el evento 4406 para cada punto de unión que encuentra.  
      
  - Los únicos puntos de reanálisis que Replicación DFS replica son los que usan la etiqueta IO\_REPARSE\_TAG\_SYMLINK; sin embargo, Replicación DFS no garantiza que también se replique el destino de symlink. Para obtener más información, consulta [Pregunta en el Blog del equipo de servicios de directorio](https://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx).  
      
  - Los archivos con las etiquetas de reanálisis IO\_REPARSE\_TAG\_DEDUP, IO\_REPARSE\_TAG\_SIS o IO\_REPARSE\_TAG\_HSM se replican como archivos normales. La etiqueta de reanálisis y los búferes de datos de reanálisis no se replican en otros servidores porque el punto de reanálisis solo funciona en el sistema local. Por lo tanto, Replicación DFS puede replicar carpetas en volúmenes que usan la desduplicación de datos en Windows Server 2012 o Almacenamiento de instancia única (SIS); sin embargo, la información de desduplicación de datos se mantiene por separado en cada servidor en el que está habilitado el servicio de rol.  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>¿Replica Replicación DFS los cambios de marca de tiempo si no se realizan otros cambios en el archivo?

No, Replicación DFS no replica los archivos cuyo único cambio es el de la marca de tiempo. Además, la marca de tiempo modificada no se replica en otros miembros del grupo de replicación a menos que se realicen otros cambios en el archivo.

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>¿Replica Replicación DFS los permisos actualizados en un archivo o carpeta?

Sí. Replicación DFS replica los cambios de permisos de los archivos y las carpetas. Solo se replica la parte del archivo asociada a la lista de control de acceso (ACL), aunque Replicación DFS debe leer el archivo completo en el área de almacenamiento provisional.


> [!NOTE]
> El cambio de las ACL en un gran número de archivos puede afectar al rendimiento de la replicación. Sin embargo, cuando se usa RDC, la cantidad de datos transferidos es proporcional al tamaño de las ACL, no al tamaño del archivo completo. La cantidad de tráfico de disco sigue siendo proporcional al tamaño de los archivos, ya que los archivos se deben leer en la carpeta de almacenamiento provisional y desde ella. 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>¿Admite Replicación DFS la combinación de archivos de texto en caso de conflicto?

Replicación DFS no combina archivos cuando hay un conflicto. Sin embargo, intenta conservar la versión anterior del archivo en la carpeta DfsrPrivate\\ConflictandDeleted oculta del equipo en el que se detectó el conflicto.

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>¿Usa Replicación DFS el cifrado al transmitir datos?

Sí. Replicación DFS usa conexiones RPC (llamada a procedimiento remoto) con cifrado.

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>¿Es posible deshabilitar el uso de RPC cifrado?

No. El servicio Replicación DFS usa llamadas a procedimiento remoto (RPC) a través de TCP para replicar los datos. Para proteger las transferencias de datos a través de Internet, el servicio Replicación DFS está diseñado para usar siempre la constante de nivel de autenticación `RPC_C_AUTHN_LEVEL_PKT_PRIVACY`. De este modo se garantiza que la comunicación RPC a través de Internet esté siempre cifrada. Y, por lo tanto, no es posible deshabilitar el uso de RPC cifrado por parte del servicio Replicación DFS.

Para más información, visita los siguientes sitios web de Microsoft:

  - [Referencia técnica de RPC](https://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [Acerca de la compresión diferencial remota](https://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [Constantes de nivel de autenticación](https://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>¿Cómo se controlan las replicaciones simultáneas?

Hay un administrador de actualizaciones por cada carpeta replicada. Los administradores de actualizaciones funcionan de forma independiente entre sí.

De forma predeterminada, se comparten un máximo de 16 (cuatro en Windows Server 2003 R2) descargas simultáneas entre todas las conexiones y los grupos de replicación. Dado que las conexiones y las actualizaciones del grupo de replicación no se serializan, no hay ningún orden específico en el que se reciben las actualizaciones. Si se abren dos programaciones, normalmente las actualizaciones se reciben e instalan desde ambas conexiones a la vez.

### <a name="how-do-i-force-replication-or-polling"></a>¿Cómo fuerzo la replicación o el sondeo?

Puedes forzar la replicación de inmediato mediante Administración de DFS, tal como se describe en [Edición de programaciones de replicación](https://technet.microsoft.com/library/Cc732278). También puedes forzar la replicación mediante el cmdlet `Sync-DfsReplicationGroup`, incluido en el módulo de PowerShell de DFSR, que se presentó con Windows Server 2012 R2, o el comando **Dfsrdiag SyncNow**. Puedes forzar el sondeo mediante el cmdlet `Update-DfsrConfigurationFromAD` o el comando **Dfsrdiag PollAD**.

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>¿Es posible configurar un tiempo de espera entre las replicaciones de los archivos que cambian con frecuencia?

No. Si la programación está abierta, Replicación DFS replicará los cambios cuando los advierta. No hay manera de configurar un tiempo de espera para los archivos.

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>¿Es posible configurar la replicación unidireccional con Replicación DFS?

Sí. Si usas Windows Server 2012 o Windows Server 2008 R2, puedes crear una carpeta replicada de solo lectura que replique el contenido a través de una conexión unidireccional. Para obtener más información, consulta [Hacer que una carpeta replicada sea de solo lectura en un miembro determinado](https://go.microsoft.com/fwlink/?linkid=156740) (https://go.microsoft.com/fwlink/?LinkId=156740).

No se admite la creación de una conexión de replicación unidireccional con Replicación DFS en Windows Server 2008 o Windows Server 2003 R2. Si lo haces, pueden producirse numerosos problemas, como errores de topología de comprobación de mantenimiento, problemas de almacenamiento provisional y problemas con la base de datos de Replicación DFS.

Si usas Windows Server 2008 o Windows Server 2003 R2, puedes simular una conexión unidireccional mediante la realización de las siguientes acciones:

  - Entrenar los administradores para que realicen cambios solo en los servidores que deseas designar como servidores principales. A continuación, permitir que los cambios se repliquen en los servidores de destino.  
      
  - Configurar los permisos de recursos compartidos en los servidores de destino para que los usuarios finales no tengan permisos de escritura. Si no se permiten cambios en los servidores de sucursal, no hay nada que replicar y se simula una conexión unidireccional y se mantiene un uso bajo de la WAN.  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>¿Hay alguna manera de forzar una replicación completa de todos los archivos, incluidos los archivos sin cambios?

No. Si Replicación DFS considera que los archivos son idénticos, no los replicará. Si los archivos cambiados no se han replicado, Replicación DFS los replicará automáticamente si se ha configurado para ello. Para sobrescribir la programación configurada, usa el método WMI **ForceReplicate ()** . Sin embargo, solo reemplaza la programación y no fuerza la replicación de archivos sin cambios o idénticos.

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>¿Qué ocurre si el miembro principal sufre la pérdida de la base de datos durante la replicación inicial?

Durante la replicación inicial, los archivos del miembro principal siempre tendrán prioridad en la resolución de los conflictos que se producen si los miembros receptores tienen versiones diferentes de los archivos del miembro principal. La designación de miembro principal se almacena en Active Directory Domain Services y la designación se borra después de que el miembro principal esté listo para la replicación, pero antes de que se repliquen todos los miembros del grupo de replicación.

Si se produce un error en la replicación inicial o el servicio Replicación DFS se reinicia durante la replicación, el miembro principal verá la designación de miembro principal en la base de datos de Replicación DFS local y volverá a intentar la replicación inicial. Si se pierde la base de datos de Replicación DFS del miembro principal después de borrar la designación principal en Active Directory Domain Services, pero antes de que se complete la replicación inicial de todos los miembros del grupo de replicación, se producirá un error en todos los miembros del grupo de replicación al replicar la carpeta porque no hay ningún servidor designado como miembro principal. Si ocurre esto, usa el comando **Dfsradmin membership /set /isprimary:true** en el servidor del miembro principal para restaurar manualmente la designación del miembro principal.

Para obtener más información sobre la replicación inicial, consulta [Crear un grupo de replicación](https://technet.microsoft.com/library/cc725893).


> [!WARNING]
> La designación de miembro principal solo se usa durante el proceso de replicación inicial. Si usas el comando <STRONG>Dfsradmin</STRONG> para especificar un miembro principal para una carpeta replicada una vez completada la replicación, Replicación DFS no designa el servidor como miembro principal en Active Directory Domain Services. Sin embargo, si la base de datos de Replicación DFS del servidor sufre daños irreversibles o una pérdida de datos, el servidor intentará realizar una replicación inicial como miembro principal en lugar de recuperar los datos de otro miembro del grupo de replicación. En esencia, el servidor se convierte en un servidor principal no autorizado, lo que puede causar conflictos. Por esta razón, especifica el miembro principal manualmente solo si estás seguro de que se ha producido un error irrecuperable en la replicación inicial. 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>¿Qué ocurre si la programación de replicación se cierra mientras se replica un archivo?

Si la compresión diferencial remota (RDC) está habilitada en la conexión, la replicación de entrada de un archivo superior a 64 KB que comenzó a replicarse inmediatamente antes del cierre de la programación (o el cambio a **No bandwidth** [Sin ancho de banda]) continúa cuando se abre la programación (o cuando se produce un cambio en un valor distinto de **No bandwidth** [Sin ancho de banda]). La replicación continúa desde el estado en que se encontraba cuando se detuvo la replicación.

Si RDC se desactiva, Replicación DFS reinicia completamente la transferencia de archivos. Esto puede retrasar el momento en que el archivo está disponible en el miembro receptor.

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>¿Qué ocurre cuando dos usuarios actualizan simultáneamente el mismo archivo en diferentes servidores?

Cuando Replicación DFS detecta un conflicto, usa la versión del archivo que se guardó en último lugar. Mueve el otro archivo a la carpeta DfsrPrivate\\ConflictandDeleted (en la ruta de acceso local de la carpeta replicada del equipo que solucionó el conflicto). Permanece allí hasta la limpieza de la carpeta de conflictos y eliminaciones, que se produce cuando la carpeta de conflictos y eliminaciones supera el tamaño configurado o Replicación DFS encuentra un error de espacio en disco insuficiente. La carpeta de conflictos y eliminaciones no se replica y este método de resolución de conflictos evita el problema de los directorios no autorizados que podía producirse en FRS.

Cuando se produce un conflicto, Replicación DFS registra un evento informativo en el registro de eventos de Replicación DFS. Este evento no requiere la intervención del usuario por los siguientes motivos:

  - No es visible para los usuarios (solo es visible para los administradores del servidor).  
      
  - Replicación DFS trata la carpeta de conflictos y eliminaciones como memoria caché. Cuando se alcanza un umbral de cuota, limpia algunos de esos archivos. No hay ninguna garantía de que se guardarán los archivos en conflicto.  
      
  - El conflicto puede residir en un servidor diferente del origen del conflicto.  
      

## <a name="staging"></a>Preconfiguración

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>¿Continúa Replicación DFS el almacenamiento provisional de los archivos cuando se deshabilita la replicación por la cuota de limitación de ancho de banda o una programación, o cuando una conexión se deshabilita manualmente?

No. Replicación DFS no sigue almacenando provisionalmente los archivos fuera de los tiempos de replicación programados, si se ha superado la cuota de límite de ancho de banda o si se deshabilitan las conexiones.

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>¿Impide Replicación DFS que otras aplicaciones tengan acceso a un archivo durante el almacenamiento provisional?

No. Replicación DFS abre los archivos de forma que no impide que los usuarios o las aplicaciones abran los archivos de la carpeta de replicación. Este método se conoce como "bloqueo oportunista".

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>¿Es posible cambiar la ubicación de la carpeta de almacenamiento provisional con la herramienta Administración de DFS?

Sí. La ubicación de la carpeta de almacenamiento provisional se configura en la pestaña **Avanzadas** del cuadro de diálogo **Propiedades** para cada miembro de un grupo de replicación.

### <a name="when-are-files-staged"></a>¿Cuándo se almacenan provisionalmente los archivos?

Los archivos se almacenan provisionalmente en el miembro remitente cuando el miembro receptor solicita el archivo (a menos que el archivo tenga un tamaño de 64 KB o inferior) como se muestra en la tabla siguiente. Si la compresión diferencial remota (RDC) se deshabilita en la conexión, el archivo se almacena provisionalmente, a menos que tenga un tamaño de 256 KB o inferior. Los archivos también se almacenan provisionalmente en el miembro receptor, ya que se transfieren si tienen un tamaño inferior a 64 KB, aunque puedes configurar esta opción entre 16 KB y 1 MB. Si se cierra la programación, los archivos no se almacenan provisionalmente.

### <a name="the-minimum-file-sizes-for-staging-files"></a>Los tamaños de archivo mínimos para el almacenamiento provisional de archivos

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC habilitada</th>
<th>RDC deshabilitada</th>
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

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>¿Qué ocurre si se cambia un archivo después de que se almacene provisionalmente, pero antes de que se transmita por completo al sitio remoto?

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
<th>Fecha</th>
<th>Descripción</th>
<th>Razón</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>15 de noviembre de 2018</p></td>
<td><p>Actualizado para Windows Server 2019.</p></td>
<td><p>Sistema operativo nuevo</p></td>
</tr>
<tr class="even">
<td><p>9 de octubre de 2013</p></td>
<td><p>Se actualizó la sección ¿Cuáles son los límites admitidos de Replicación DFS? con los resultados de las pruebas en Windows Server 2012 R2.</p></td>
<td><p>Actualizaciones para la última versión de Windows Server</p></td>
</tr>
<tr class="odd">
<td><p>30 de enero de 2013</p></td>
<td><p>Se agregó la entrada ¿Continúa Replicación DFS el almacenamiento provisional de los archivos cuando se deshabilita la replicación por la cuota de limitación de ancho de banda o una programación, o cuando una conexión se deshabilita manualmente?</p></td>
<td><p>Preguntas de clientes</p></td>
</tr>
<tr class="even">
<td><p>31 de octubre de 2012</p></td>
<td><p>Se editó la entrada ¿Cuáles son los límites admitidos de Replicación DFS? para aumentar el número probado de archivos replicados en un volumen.</p></td>
<td><p>Comentarios de los clientes</p></td>
</tr>
<tr class="odd">
<td><p>15 de agosto de 2012</p></td>
<td><p>Se editó la entrada ¿Replica Replicación DFS los permisos de archivos NTFS, los flujos de datos alternativos, los vínculos físicos y los puntos de reanálisis? para aclarar mejor cómo Replicación DFS controla los vínculos físicos y los puntos de reanálisis.</p></td>
<td><p>Comentarios de Servicios de atención al cliente</p></td>
</tr>
<tr class="even">
<td><p>13 de junio de 2012</p></td>
<td><p>Se editó la entrada ¿Funciona Replicación DFS en volúmenes ReFS o FAT? para agregar una discusión de ReFS.</p></td>
<td><p>Comentarios de los clientes</p></td>
</tr>
<tr class="odd">
<td><p>25 de abril de 2012</p></td>
<td><p>Se editó la entrada ¿Replica Replicación DFS los permisos de archivos NTFS, los flujos de datos alternativos, los vínculos físicos y los puntos de reanálisis? para aclarar cómo Replicación DFS controla los vínculos físicos.</p></td>
<td><p>Reducir la posible confusión</p></td>
</tr>
<tr class="even">
<td><p>30 de marzo de 2011</p></td>
<td><p>Se editó la entrada ¿Puede Replicación DFS replicar archivos de base de datos de Outlook.pst o Microsoft Office Access? para corregir el impacto potencial del uso de Replicación DFS con archivos .pst y Access.</p>
<p>Se agregó ¿Cómo puedo mejorar el rendimiento de la replicación?</p></td>
<td><p>Preguntas de los clientes sobre la entrada anterior, que indicaron incorrectamente que la replicación de archivos .pst o Access podría dañar la base de datos de Replicación DFS.</p></td>
</tr>
<tr class="odd">
<td><p>26 de enero de 2011</p></td>
<td><p>Se agregó ¿Cómo se pueden recuperar los archivos de las carpetas ConflictAndDeleted o PreExisting?</p></td>
<td><p>Comentarios de los clientes</p></td>
</tr>
<tr class="even">
<td><p>20 de octubre de 2010</p></td>
<td><p>Se agregó ¿Cómo puedo actualizar o reemplazar un miembro de Replicación DFS?</p></td>
<td><p>Comentarios de los clientes</p></td>
</tr>
</tbody>
</table>

