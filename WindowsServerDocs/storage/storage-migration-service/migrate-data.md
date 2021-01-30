---
title: Migración de un servidor de archivos mediante el servicio de migración de almacenamiento
description: Breve descripción del tema de los resultados del motor de búsqueda
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 01/29/2021
ms.topic: article
ms.openlocfilehash: d38a6c058d6629f0b73e613e417dea76a22bc71b
ms.sourcegitcommit: f89c1bc137ff92eeca2499131854287f28851f63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99084953"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Usar el servicio de migración de almacenamiento para migrar un servidor

En este tema se describe cómo migrar un servidor, incluidos los archivos y la configuración, a otro servidor mediante el [servicio de migración de almacenamiento](overview.md) y el centro de administración de Windows. La migración realiza tres pasos una vez que haya instalado el servicio y abierto los puertos de Firewall necesarios: realice un inventario de los servidores, transfiera los datos y recorte a los nuevos servidores.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Paso 0: instalación del servicio de migración de almacenamiento y comprobación de los puertos del firewall

Antes de empezar, instale el servicio de migración de almacenamiento y asegúrese de que estén abiertos los puertos de Firewall necesarios.

1. Compruebe los [requisitos del servicio de migración de almacenamiento](overview.md#requirements) e instale el [centro de administración de Windows](../../manage/windows-admin-center/overview.md) en su PC o un servidor de administración si aún no lo ha hecho. Si va a migrar los equipos de origen Unidos a un dominio, debe instalar y ejecutar el servicio de migración de almacenamiento en un servidor que esté unido al mismo dominio o bosque que los equipos de origen.
2. En el centro de administración de Windows, conéctese al servidor de Orchestrator que ejecuta Windows Server 2019. <br>Este es el servidor en el que instalará el servicio de migración de almacenamiento y lo usará para administrar la migración. Si va a migrar un solo servidor, puede usar el servidor de destino siempre y cuando ejecute Windows Server 2019. Se recomienda usar un servidor de orquestación independiente para las migraciones de varios servidores.
3. Vaya a **Administrador del servidor** (en el centro de administración de Windows) > **servicio de migración de almacenamiento** y seleccione **instalar** para instalar el servicio de migración de almacenamiento y sus componentes necesarios (se muestra en la figura 1).
    ![Captura de pantalla de la página del servicio de migración de almacenamiento que muestra el botón instalar, ](media/migrate/install.png) **figura 1: instalación del servicio de migración de almacenamiento**
4. Instale el proxy del servicio de migración de almacenamiento en todos los servidores de destino que ejecutan Windows Server 2019. Esto duplica la velocidad de transferencia cuando está instalado en los servidores de destino. <br>Para ello, conéctese al servidor de destino en el centro de administración de Windows y, a continuación, vaya a **Administrador del servidor** (en el centro de administración de windows) > **roles y características**, > **características**, seleccione el **proxy del servicio de migración de almacenamiento** y, a continuación, seleccione **instalar**.
5. Si tiene previsto migrar a o desde clústeres de conmutación por error de Windows, instale las herramientas de clúster de conmutación por error en el servidor de Orchestrator. <br>Para ello, conéctese al servidor de Orchestrator en el centro de administración de Windows y, a continuación, vaya a **Administrador del servidor** (en el centro de administración de windows) > **roles y características**, > **características**, > **herramientas de administración remota del servidor**, > herramientas de **Administración de características**, seleccione Herramientas de **clúster de conmutación por error** y, a continuación, seleccione **instalar**.
6. En todos los servidores de origen y en los servidores de destino que ejecutan Windows Server 2012 R2 o Windows Server 2016, en el centro de administración de Windows, conéctese a cada servidor, vaya a **Administrador del servidor** (en el centro de administración de Windows) > reglas de entrada de **firewall**  >  y, a continuación, compruebe que están habilitadas las siguientes reglas:
    - Compartir archivos e impresoras (SMB de entrada)
    - Servicio NetLogon (NP-in)
    - Instrumental de administración de Windows (DCOM-In)
    - Instrumental de administración de Windows (WMI-In)

   Si utiliza firewalls de terceros, los intervalos de puertos de entrada que se van a abrir son TCP/445 (SMB), TCP/135 (asignador de extremos de RPC/DCOM) y TCP 1025-65535 (puertos efímeros de RPC/DCOM). Los puertos del servicio de migración de almacenamiento son TCP/28940 (Orchestrator) y TCP/28941 (proxy).

7. Si utiliza un servidor de Orchestrator para administrar la migración y desea descargar eventos o un registro de los datos que se transfieren, compruebe que la regla de Firewall compartir archivos e impresoras (SMB-in) también está habilitada en ese servidor.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Paso 1: creación de un trabajo e inventario de los servidores para averiguar qué migrar

En este paso, especificará qué servidores se van a migrar y, a continuación, los examinará para recopilar información sobre sus archivos y configuraciones.

1. Seleccione **nuevo trabajo**, asigne un nombre al trabajo y, a continuación, seleccione si desea migrar los servidores y clústeres de Windows o los servidores Linux que usan Samba. Después, seleccione **Aceptar**.
2. En la página **escribir credenciales** , escriba las credenciales de administrador que funcionan en los servidores desde los que desea migrar y, a continuación, seleccione **siguiente**. <br>Si va a migrar desde servidores Linux, escriba las credenciales en las páginas **credenciales de samba** y **credenciales de Linux** , incluida una contraseña SSH o una clave privada.

3. Seleccione **Agregar un dispositivo**, escriba un nombre de servidor de origen o el nombre de un servidor de archivos en clúster y, después, seleccione **Aceptar**. <br>Repita este paso para cualquier otro servidor que desee inventariar.

4. Seleccione **iniciar examen**.<br>La página se actualiza para mostrar Cuándo se completa el análisis.
    ![Captura de pantalla que muestra un servidor listo para su examen ](media/migrate/inventory.png) **ilustración 2: inventariar servidores**
5. Seleccione cada servidor para revisar los recursos compartidos, la configuración, los adaptadores de red y los volúmenes inventariados. <br><br>El servicio de migración de almacenamiento no transferirá los archivos o carpetas que sabemos que podrían interferir con el funcionamiento de Windows, por lo que en esta versión verá ADVERTENCIAS para los recursos compartidos ubicados en la carpeta del sistema de Windows. Tendrá que omitir estos recursos compartidos durante la fase de transferencia. Para obtener más información, consulte [qué archivos y carpetas se excluyen de las transferencias](faq.md#what-files-and-folders-are-excluded-from-transfers).
6. Seleccione **siguiente** para pasar a transferir datos.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Paso 2: transferencia de datos de los servidores antiguos a los servidores de destino

En este paso, transferirá los datos después de especificar dónde colocarlos en los servidores de destino.

1. En la página **transferir datos**  >  **Escriba las credenciales** , escriba las credenciales de administrador que funcionan en los servidores de destino a los que desea migrar y, a continuación, seleccione **siguiente**.
2. En la página **Agregar un dispositivo de destino y asignaciones** , se muestra el primer servidor de origen. Escriba el nombre del servidor o el servidor de archivos en clúster al que desea migrar y, a continuación, seleccione **scan Device**. Si realiza la migración desde un equipo de origen unido a un dominio, el servidor de destino debe estar unido al mismo dominio. También puede hacer clic en "crear una nueva máquina virtual de Azure" y después usar el Asistente para implementar un nuevo servidor de destino en Azure. Esto ajustará automáticamente el tamaño de la máquina virtual, aprovisionará el almacenamiento, formateará los discos, unirá el dominio y agregará el proxy del servicio de migración de almacenamiento a un destino de Windows Server 2019. Puede elegir entre las máquinas virtuales de Windows Server 2019 (recomendado), Windows Server 2016 y Windows Server 2012 R2 de cualquier tamaño y usar discos administrados.

    > [!NOTE]
    > El uso de "creación de una nueva máquina virtual de Azure" requiere lo siguiente:
    > - Una suscripción válida a Azure.
    > - Un grupo de recursos de Azure Compute existente en el que haya creado derechos.
    > - Un Virtual Network de Azure y una subred existentes.
    > - Una solución de Azure Expressroute o VPN vinculada a la Virtual Network y la subred que permite la conectividad desde esta máquina virtual de IaaS de Azure a los clientes locales, los controladores de dominio, el equipo del orquestador de migración de almacenamiento, el equipo del centro de administración de Windows y el equipo de origen que se va a migrar.

    Este es un vídeo que muestra cómo usar el servicio de migración de almacenamiento para migrar a máquinas virtuales de Azure.
    > [!VIDEO https://www.youtube-nocookie.com/embed/k8Z9LuVL0xQ]

3. Asigne los volúmenes de origen a los volúmenes de destino, desactive la casilla **incluir** para los recursos compartidos que no desea transferir (incluidos los recursos compartidos administrativos ubicados en la carpeta del sistema de Windows) y, a continuación, seleccione **siguiente**.
   ![Captura de pantalla que muestra un servidor de origen y sus volúmenes y recursos compartidos, y donde se transferirán a la ](media/migrate/transfer.png) **figura 3 de destino: un servidor de origen y donde se transferirá el almacenamiento**
4. Agregue un servidor de destino y asignaciones para más servidores de origen y, a continuación, seleccione **siguiente**.
5. En la página **ajustar la configuración de transferencia** , especifique si desea migrar usuarios y grupos locales en los servidores de origen y, a continuación, seleccione **siguiente**. Esto le permite volver a crear los usuarios y grupos locales en los servidores de destino para que no se pierdan los permisos de archivos o recursos compartidos establecidos para los usuarios y grupos locales. Estas son las opciones para migrar usuarios y grupos locales:

    - **Cambiar el nombre de las cuentas con el mismo nombre** está seleccionado de forma predeterminada y migra todos los usuarios y grupos locales en el servidor de origen. Si encuentra usuarios o grupos locales con el mismo nombre en el origen y el destino, los cambia de nombre en el destino, a menos que estén integrados (por ejemplo, el usuario administrador y el grupo administradores). No use esta opción si el servidor de origen o de destino es un controlador de dominio.
    - **La reutilización de cuentas con el mismo nombre** asigna idénticos usuarios y grupos en el origen y el destino. No use esta opción si el servidor de origen o de destino es un controlador de dominio.
    - **No transferir usuarios y grupos** omite la migración de usuarios y grupos locales, lo que es necesario cuando el origen o el destino es un controlador de dominio, o cuando se propagan datos para Replicación DFS (replicación DFS no admite grupos y usuarios locales).

   > [!NOTE]
   > Las cuentas de usuario migradas están deshabilitadas en el destino y tienen asignada una contraseña de 127 caracteres que es compleja y aleatoria, por lo que tendrá que habilitarlas y asignar una nueva contraseña cuando haya terminado de usarlas. Esto ayuda a garantizar que las cuentas anteriores con contraseñas olvidadas y no seguras del origen no sigan siendo un problema de seguridad en el destino. También puede que desee consultar la [solución de contraseña de administrador local (laps)](https://www.microsoft.com/download/details.aspx?id=46899) como una manera de administrar contraseñas de administrador local.

6. Seleccione **validar** y, a continuación, seleccione **siguiente**.
7. Seleccione **iniciar transferencia** para iniciar la transferencia de datos.<br>La primera vez que se transfiere, se mueven los archivos existentes en un destino a una carpeta de copia de seguridad. En las transferencias posteriores, de forma predeterminada, actualizaremos el destino sin realizar primero una copia de seguridad. <br>Además, el servicio de migración de almacenamiento es lo suficientemente inteligente como para tratar con los recursos compartidos superpuestos, por lo que no se copiarán las mismas carpetas dos veces en el mismo trabajo.
8. Una vez completada la transferencia, revise el servidor de destino para asegurarse de que todo se ha transferido correctamente. Seleccione **registro de errores solo** si desea descargar un registro de los archivos que no se han transferido.

   > [!NOTE]
   > Si desea mantener un rastro de auditoría de las transferencias o tiene previsto realizar más de una transferencia en un trabajo, haga clic en **transferir registro** o en las demás opciones para guardar el registro para guardar una copia de CSV. Cada transferencia subsiguiente sobrescribe la información de base de datos de una ejecución anterior.

En este momento, tiene tres opciones:

- **Vaya al paso siguiente**, reduciendo para que los servidores de destino adopten las identidades de los servidores de origen.
- **Tenga en cuenta que la migración se ha completado** sin asumir las identidades de los servidores de origen.
- **Transferir de nuevo**, copiando solo los archivos que se actualizaron desde la última transferencia.

Si su objetivo es sincronizar los archivos con Azure, puede configurar los servidores de destino con Azure File Sync después de transferir los archivos o después de recurrir a los servidores de destino (consulte [planeación de una implementación de Azure File Sync](/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-cut-over-to-the-new-servers"></a>Paso 3: pasar a los nuevos servidores

En este paso, se recortan de los servidores de origen a los servidores de destino y se mueven las direcciones IP y los nombres de equipo a los servidores de destino. Una vez finalizado este paso, las aplicaciones y los usuarios accederán a los nuevos servidores a través de los nombres y las direcciones de los servidores desde los que migró.

1. Si ha navegado fuera del trabajo de migración, en el centro de administración de Windows, vaya a **Administrador del servidor**  >  **servicio de migración de almacenamiento** y, a continuación, seleccione el trabajo que desea completar.
2. En la página **resaltar a los nuevos servidores**  >  ,**Escriba las credenciales** , seleccione **siguiente** para usar las credenciales que escribió anteriormente.

   Si el destino es un servidor de archivos en clúster, es posible que tenga que proporcionar credenciales con permisos para quitar el clúster del dominio y, a continuación, volver a agregarlo con el nuevo nombre.

3. En la página **configurar traslado** , especifique qué adaptador de red del destino debe tomar la configuración de cada adaptador del origen. De esta forma, se mueve la dirección IP del origen al destino como parte de la transferencia, y se asigna al servidor de origen una nueva dirección IP DHCP o estática. Tiene la opción de omitir todas las migraciones de red o ciertas interfaces.
4. Especifique la dirección IP que se usará para el servidor de origen después de que el traslado traslade su dirección al destino. Puede usar DHCP o una dirección estática. Si usa una dirección estática, la nueva subred debe ser la misma que la antigua o bien se producirá un error en la misma.
    ![Captura de pantalla que muestra un servidor de origen y sus direcciones IP y nombres de equipo y de qué se reemplazarán después de la ](media/migrate/cutover.png) **figura 4 de destino: un servidor de origen y cómo se moverá su configuración de red al destino**
5. Especifique cómo cambiar el nombre del servidor de origen después de que el servidor de destino adopte el nombre. Puede usar un nombre generado de forma aleatoria o escribir uno personalmente. A continuación, seleccione **Siguiente**.
6. Seleccione **siguiente** en la página **ajustar la configuración de transferencia** .
7. Seleccione **validar** en la página **validar el dispositivo de origen y de destino** y, a continuación, seleccione **siguiente**.
8. Cuando esté listo para realizar el traslado, seleccione **iniciar el traslado**. <br>Es posible que los usuarios y las aplicaciones experimenten una interrupción mientras se mueven la dirección y los nombres y los servidores se reinician varias veces, pero no se verán afectados por la migración. El tiempo que tarda la transferencia depende de la rapidez con que se reinicien los servidores, así como de los tiempos de replicación de DNS y Active Directory.

## <a name="post-migration-operations"></a>Operaciones posteriores a la migración

Después de migrar un servidor o clúster, evalúe el entorno para ver si se pueden realizar operaciones posteriores a la migración: 

- **Cree un plan para el servidor de origen que se ha retirado ahora**. El servicio de migración de almacenamiento usa el proceso de traslado para que un servidor de destino asuma la identidad de un servidor de origen, cambiando los nombres y las direcciones IP del origen para que los usuarios y las aplicaciones dejen de tener acceso. No obstante, no se desactiva o se altera de algún otro modo el contenido del servidor de origen. Debe crear un plan para retirar el servidor de origen. Se recomienda dejar el origen en línea durante al menos dos semanas en caso de que se pierdan datos en uso, para que los archivos se puedan recuperar fácilmente sin necesidad de restaurar la copia de seguridad sin conexión. Después de ese período, se recomienda desactivar el servidor durante otras cuatro semanas para que siga estando disponible para la recuperación de datos, pero ya no consuma recursos operativos o de energía. Después de ese período, realice una copia de seguridad completa final del servidor y, a continuación, evalúe la reasignación si es un servidor físico o elimine si se trata de una máquina virtual.      
- **Vuelva a emitir los certificados en el nuevo servidor de destino**. En el momento en que el servidor de destino estaba en línea pero aún no se ha reducido, es posible que se le hayan emitido certificados a través de la inscripción automática u otros procesos. Cambiar el nombre de un servidor de Windows no cambia ni reemite automáticamente los certificados existentes, por lo que los certificados existentes pueden contener el nombre del servidor antes del traslado de ti. Debe examinar los certificados existentes en ese servidor y volver a emitir los nuevos según sea necesario.   

## <a name="additional-references"></a>Referencias adicionales

- [Información general del servicio de migración de almacenamiento](overview.md)
- [Preguntas más frecuentes (p + f) sobre Storage Migration Services](faq.md)
- [Planeamiento de una implementación de Azure File Sync](/azure/storage/files/storage-sync-files-planning)
