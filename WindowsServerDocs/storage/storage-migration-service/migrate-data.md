---
title: Migración de un servidor de archivos mediante el servicio de migración de almacenamiento
description: Breve descripción del tema de los resultados del motor de búsqueda
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 0b5b473460bf72143f517443eadad831dd2502c5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865150"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Usar el servicio de migración de almacenamiento para migrar un servidor

En este tema se describe cómo migrar un servidor, incluidos los archivos y la configuración, a otro servidor mediante el [servicio de migración de almacenamiento](overview.md) y el centro de administración de Windows. La migración realiza tres pasos una vez que haya instalado el servicio y abierto los puertos de Firewall necesarios: realice un inventario de los servidores, transfiera los datos y recorte a los nuevos servidores.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Paso 0: Instalación del servicio de migración de almacenamiento y comprobación de los puertos del firewall

Antes de empezar, instale el servicio de migración de almacenamiento y asegúrese de que estén abiertos los puertos de Firewall necesarios.

1. Compruebe los [requisitos del servicio de migración de almacenamiento](overview.md#requirements) e instale el [centro de administración de Windows](../../manage/windows-admin-center/understand/windows-admin-center.md) en su PC o un servidor de administración si aún no lo ha hecho.
2. En el centro de administración de Windows, conéctese al servidor de Orchestrator que ejecuta Windows Server 2019. <br>Este es el servidor en el que instalará el servicio de migración de almacenamiento y lo usará para administrar la migración. Si va a migrar un solo servidor, puede usar el servidor de destino siempre y cuando ejecute Windows Server 2019. Se recomienda usar un servidor de orquestación independiente para las migraciones de varios servidores.
1. Vaya a **Administrador del servidor** (en el centro de administración de Windows) > **servicio de migración de almacenamiento** y seleccione **instalar** para instalar el servicio de migración de almacenamiento y sus componentes necesarios (se muestra en la figura 1).
    ![Captura de pantalla de la página del servicio de migración de](media/migrate/install.png) almacenamiento que muestra el botón **instalar, figura 1: Instalando el servicio de migración de almacenamiento**
1. Instale el proxy del servicio de migración de almacenamiento en todos los servidores de destino que ejecutan Windows Server 2019. Esto duplica la velocidad de transferencia cuando está instalado en los servidores de destino. <br>Para ello, conéctese al servidor de destino en el centro de administración de Windows y, a continuación, vaya a **Administrador del servidor** (en el centro de administración de windows) > **roles y características**, seleccione **proxy de servicio de migración de almacenamiento**y, a continuación, seleccione **instalar**.
1. En todos los servidores de origen y en los servidores de destino que ejecutan Windows Server 2012 R2 o Windows Server 2016, en el centro de administración de Windows, conéctese a cada servidor, vaya a **Administrador del servidor** (en el centro de administración de windows) > **firewall**  >   **Reglas de entrada**y, a continuación, compruebe que están habilitadas las siguientes reglas:
    - Compartir archivos e impresoras (SMB de entrada)
    - Servicio NetLogon (NP-in)
    - Instrumental de administración de Windows (DCOM-in)
    - Instrumental de administración de Windows (WMI-In)

   > [!NOTE]
   > Si utiliza firewalls de terceros, los intervalos de puertos de entrada que se van a abrir son TCP/445 (SMB), TCP/135 (asignador de extremos de RPC/DCOM) y TCP 1025-65535 (puertos efímeros de RPC/DCOM). Los puertos del servicio de migración de almacenamiento son TCP/28940 (Orchestrator) y TCP/28941 (proxy).

1. Si utiliza un servidor de Orchestrator para administrar la migración y desea descargar eventos o un registro de los datos que se transfieren, compruebe que la regla de Firewall compartir archivos e impresoras (SMB-in) también está habilitada en ese servidor.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Paso 1: Creación de un trabajo e inventario de los servidores para averiguar qué migrar

En este paso, especificará qué servidores se van a migrar y, a continuación, los examinará para recopilar información sobre sus archivos y configuraciones.

1. Seleccione **nuevo trabajo**, asigne un nombre al trabajo y, a continuación, seleccione **Aceptar**.
1. En la página **escribir credenciales** , escriba las credenciales de administrador que funcionan en los servidores desde los que desea migrar y, a continuación, seleccione **siguiente**.
1. Seleccione **Agregar un dispositivo**, escriba un nombre de servidor de origen y, luego, haga clic en **Aceptar**. <br>Repita este paso para cualquier otro servidor que desee inventariar.
1. Seleccione **iniciar examen**.<br>La página se actualiza para mostrar Cuándo se completa el análisis.
    ![Captura de pantalla en la que se muestra un](media/migrate/inventory.png) servidor listo para analizar **la figura 2: Inventario de servidores**
1. Seleccione cada servidor para revisar los recursos compartidos, la configuración, los adaptadores de red y los volúmenes inventariados. <br><br>El servicio de migración de almacenamiento no transferirá los archivos o carpetas que sabemos que podrían interferir con el funcionamiento de Windows, por lo que en esta versión verá ADVERTENCIAS para los recursos compartidos ubicados en la carpeta del sistema de Windows. Tendrá que omitir estos recursos compartidos durante la fase de transferencia. Para obtener más información, consulte [qué archivos y carpetas se excluyen de las transferencias](faq.md#what-files-and-folders-are-excluded-from-transfers).
1. Seleccione **siguiente** para pasar a transferir datos.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Paso 2: Transferencia de datos de los servidores antiguos a los servidores de destino

En este paso, transferirá los datos después de especificar dónde colocarlos en los servidores de destino.

1. En la página **transferir datos** > **Escriba las credenciales** , escriba las credenciales de administrador que funcionan en los servidores de destino a los que desea migrar y, a continuación, seleccione **siguiente**.
2. En la página **Agregar un dispositivo de destino y asignaciones** , se muestra el primer servidor de origen. Escriba el nombre del servidor al que desea migrar y, a continuación, seleccione **scan Device**.
3. Asigne los volúmenes de origen a los volúmenes de destino, desactive la casilla **incluir** para los recursos compartidos que no desea transferir (incluidos los recursos compartidos administrativos ubicados en la carpeta del sistema de Windows) y, a continuación, seleccione **siguiente**.
   ![Captura de pantalla que muestra un servidor de origen y sus volúmenes y recursos compartidos y donde se](media/migrate/transfer.png) transferirán a la figura 3 de destino **: Un servidor de origen y la ubicación a la que se transferirá el almacenamiento**
4. Agregue un servidor de destino y asignaciones para más servidores de origen y, a continuación, seleccione **siguiente**.
5. También puede ajustar la configuración de la transferencia y seleccionar **siguiente**.
6. Seleccione **validar** y, a continuación, seleccione **siguiente**.
7. Seleccione **iniciar transferencia** para iniciar la transferencia de datos.<br>La primera vez que se transfiere, se mueven los archivos existentes en un destino a una carpeta de copia de seguridad. En las transferencias posteriores, de forma predeterminada, actualizaremos el destino sin realizar primero una copia de seguridad. <br>Además, el servicio de migración de almacenamiento es lo suficientemente inteligente como para tratar con los recursos compartidos superpuestos, por lo que no se copiarán las mismas carpetas dos veces en el mismo trabajo.
8. Una vez completada la transferencia, revise el servidor de destino para asegurarse de que todo se ha transferido correctamente. Seleccione **registro de errores solo** si desea descargar un registro de los archivos que no se han transferido.

   > [!NOTE]
   > Si desea mantener un rastro de auditoría de las transferencias o planea realizar más de una transferencia en un trabajo, haga clic en **transferir registro** para guardar una copia de CSV. Cada transferencia subsiguiente sobrescribe la información de base de datos de una ejecución anterior. 

En este momento, tiene tres opciones:

- **Vaya al paso siguiente**, reduciendo para que los servidores de destino adopten las identidades de los servidores de origen.
- **Tenga en cuenta que la migración se ha completado** sin asumir las identidades de los servidores de origen.
- **Transferir de nuevo**, copiando solo los archivos que se actualizaron desde la última transferencia.

Si su objetivo es sincronizar los archivos con Azure, puede configurar los servidores de destino con Azure File Sync después de transferir los archivos o después de recurrir a los servidores de destino (consulte [planeación de una implementación de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>Paso 3: Opcionalmente, puede pasar a los nuevos servidores.

En este paso, se recortan de los servidores de origen a los servidores de destino y se mueven las direcciones IP y los nombres de equipo a los servidores de destino. Una vez finalizado este paso, las aplicaciones y los usuarios accederán a los nuevos servidores a través de los nombres y las direcciones de los servidores desde los que migró.

 1. Si ha navegado fuera del trabajo de migración, en el centro de administración de Windows, vaya a **Administrador del servidor** > **servicio de migración de almacenamiento** y, a continuación, seleccione el trabajo que desea completar. 
 1. En la página **resaltar a los nuevos servidores** > ,**Escriba las credenciales** , seleccione **siguiente** para usar las credenciales que escribió anteriormente.
 1. En la página **configurar traslado** , especifique los adaptadores de red que se van a usar para la configuración de cada dispositivo de origen. Esto mueve la dirección IP del origen al destino como parte de la transferencia.
 1. Especifique la dirección IP que se usará para el servidor de origen después de que el traslado traslade su dirección al destino. Puede usar DHCP o una dirección estática. Si usa una dirección estática, la nueva subred debe ser la misma que la antigua o bien se producirá un error en la misma.
    ![Captura de pantalla que muestra un servidor de origen y sus direcciones IP y nombres de equipo y de qué se reemplazarán después de la figura 4 de la transferencia](media/migrate/cutover.png)
    :** Un servidor de origen y cómo se moverá su configuración de red al destino**
 1. Especifique cómo cambiar el nombre del servidor de origen después de que el servidor de destino adopte el nombre. Puede usar un nombre generado de forma aleatoria o escribir uno personalmente. Después, seleccione **siguiente**.
 1. Seleccione **siguiente** en la página **ajustar la configuración de transferencia** .
 1. Seleccione **validar** en la página **validar el dispositivo de origen y de destino** y, a continuación, seleccione **siguiente**.
 1. Cuando esté listo para realizar el traslado, seleccione **iniciar el traslado**. <br>Es posible que los usuarios y las aplicaciones experimenten una interrupción mientras se mueven la dirección y los nombres y los servidores se reinician varias veces, pero no se verán afectados por la migración. El tiempo que tarda la transferencia depende de la rapidez con que se reinicien los servidores, así como de los tiempos de replicación de DNS y Active Directory.

## <a name="see-also"></a>Vea también

- [Información general del servicio de migración de almacenamiento](overview.md)
- [Preguntas más frecuentes (p + f) sobre Storage Migration Services](faq.md)
- [Planeación de una implementación de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)
