---
title: Migrar un servidor de archivos mediante el servicio de migración de almacenamiento
description: Breve descripción del tema de los resultados del motor de búsqueda
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 856eb7c2c2dfe0e0e3300fcf826e75b56258dc1b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447650"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Usar Storage Migration Service para migrar un servidor

En este tema se describe cómo migrar un servidor, incluidos sus archivos y configuración a otro servidor mediante el uso de [Storage Migration Service](overview.md) y Windows Admin Center. Migración de proceso de tres pasos una vez que haya instalado el servicio y abrir los puertos de firewall necesarias: los servidores del inventario, transferencia de datos y cambio definitivo a los nuevos servidores.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Paso 0: Instalar el servicio de migración de almacenamiento y comprobar los puertos de firewall

Antes de empezar a instalar el servicio de migración de almacenamiento y asegúrese de que estén abiertos los puertos de firewall necesarias.

1. Compruebe el [los requisitos de servicio de migración de almacenamiento](overview.md#requirements) e instale [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) en su PC o un servidor de administración si no lo ha hecho ya.
2. En Windows Admin Center, conecte el servidor de orchestrator que ejecuta Windows Server 2019. <br>Este es el servidor que se instalará el servicio de migración de almacenamiento en y usar para administrar la migración. Si va a migrar un solo servidor, puede usar el servidor de destino mientras se está ejecutando Windows Server 2019. Se recomienda que usar un servidor de una orquestación independiente para las migraciones de varios servidores.
1. Vaya a **administrador del servidor** (en Windows Admin Center) > **Storage Migration Service** y seleccione **instalar** para instalar el servicio de migración de almacenamiento y los componentes necesarios (se muestra en la figura 1).
    ![Captura de pantalla de la página de servicio de migración de almacenamiento que muestra el botón instalar](media/migrate/install.png) **figura 1: Instalar el servicio de migración de almacenamiento**
1. Instale al proxy de servicio de migración de almacenamiento en todos los servidores de destino que ejecuta Windows Server 2019. Esto duplica la velocidad de transferencia cuando se instala en los servidores de destino. <br>Para ello, conéctese al servidor de destino en Windows Admin Center y, a continuación, vaya a **administrador del servidor** (en Windows Admin Center) > **Roles y características**, seleccione **Storage Migration Service Proxy**y, a continuación, seleccione **instalar**.
1. En todos los servidores de origen y en los servidores de destino que ejecutan Windows Server 2012 R2 o Windows Server 2016, en Windows Admin Center, conéctese a cada servidor, vaya a **administrador del servidor** (en Windows Admin Center) > **Firewall**   >  **Reglas entrantes**y, a continuación, compruebe que están habilitadas las siguientes reglas:
    - Compartir archivos e impresoras (SMB de entrada)
    - Servicio Netlogon (NP-In)
    - Instrumental de administración de Windows (DCOM-In)
    - Instrumental de administración de Windows (WMI-In)

   > [!NOTE]
   > Si usas firewalls de terceros, los intervalos de puerto de entrada para abrir son TCP/445 (SMB) TCP/135 (asignador de extremos de RPC/DCOM) y TCP 1025-65535 (puertos efímeros de RPC/DCOM).

1. Si usa un servidor de orchestrator para administrar la migración y desea descargar eventos o un registro de los datos se transfieren, compruebe que la regla de firewall (SMB de entrada) de compartir archivos e impresoras está habilitada en ese servidor también.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Paso 1: Crear un trabajo y el inventario de los servidores para averiguar por qué migrar

En este paso, especifique qué servidores para migrar y, después, escanéelos para recopilar información en sus archivos y configuraciones.

1. Seleccione **nuevo trabajo**, asignar nombre al trabajo y, a continuación, seleccione **Aceptar**.
1. En el **escriba credenciales** página, escriba las credenciales de administrador que trabajan en los servidores que van a migrar desde y, a continuación, seleccione **siguiente**.
1. Seleccione **agregar un dispositivo**, escriba un nombre de servidor de origen y, a continuación, seleccione **Aceptar**. <br>Repita este paso para todos los servidores que desee inventariar.
1. Seleccione **iniciar examen**.<br>La página se actualiza para que se muestra cuando se complete la digitalización.
    ![Captura de pantalla muestra un servidor listo para ser examinado](media/migrate/inventory.png) **figura 2: Realizar un inventario de servidores**
1. Seleccione cada servidor para revisar los volúmenes que se han inventariado, adaptadores de red, configuración y recursos compartidos. <br><br>Servicio de migración de almacenamiento no transferencia de archivos o carpetas que sabemos que podría interferir con la operación de Windows, por lo que en esta versión, verá advertencias para los recursos compartidos que se encuentra en la carpeta de sistema de Windows. Tendrá que omitir estos recursos compartidos durante la fase de transferencia. Para obtener más información, consulte [qué archivos y carpetas se excluirán de las transferencias](faq.md#excluded-files).
1. Seleccione **siguiente** para pasar a la transferencia de datos.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Paso 2: Transferir datos desde los servidores antiguos a los servidores de destino

En este paso, transferir los datos después de especificar su ubicación en los servidores de destino.

1. En el **transferir datos** > **escriba credenciales** página, escriba las credenciales de administrador que trabajan en los servidores de destino que desea migrar a y, a continuación, seleccione **siguiente**.
2. En el **agregar un dispositivo de destino y las asignaciones** aparece la página, el primer servidor de origen. Escriba el nombre del servidor al que desea migrar y, a continuación, seleccione **Buscar dispositivo**.
3. Los volúmenes de origen se asignan a los volúmenes de destino, desactive la **Include** casilla de verificación de los recursos compartidos no desea transferir (incluidos los recursos compartidos administrativos ubicados en la carpeta de sistema de Windows) y, a continuación, seleccione **siguiente** .
   ![Captura de pantalla con un servidor de origen y de sus volúmenes y recursos compartidos y donde se podrá transferir a en el destino](media/migrate/transfer.png) **figura 3: Un servidor de origen y que se transferirán a su almacenamiento**
4. Agregar un servidor de destino y las asignaciones de los servidores de origen más y, a continuación, seleccione **siguiente**.
5. Opcionalmente, ajuste la configuración de transferencia y, a continuación, seleccione **siguiente**.
6. Seleccione **validar** y, a continuación, seleccione **siguiente**.
7. Seleccione **iniciar transferencia** para iniciar la transferencia de datos.<br>La primera vez que se transfiere, pasaremos los archivos existentes en un destino en una carpeta de copia de seguridad. En las transferencias siguientes, de forma predeterminada se actualizará el destino sin realizar copias de seguridad de la primera. <br>Además, servicio de migración de almacenamiento es lo suficientemente inteligente como para tratar con recursos compartidos de superpuestas, no copiaremos las mismas carpetas dos veces en el mismo trabajo.
8. Una vez completada la transferencia, consulte el servidor de destino para asegurarse de que todo lo que ha transferido correctamente. Seleccione **sólo el registro de errores** si desea descargar un registro de los archivos que no se transfirió.

   > [!NOTE]
   > Si desea mantener una pista de auditoría de las transferencias o va a realizar más de una transferencia de un trabajo, haga clic en **registro transferencia** para guardar una copia CSV. Cada transferencia subsiguiente sobrescribe la información de la base de datos de una ejecución anterior. 

En este punto, tiene tres opciones:

- **Vaya al paso siguiente**, cortar a través de modo que los servidores de destino adoptarán las identidades de los servidores de origen.
- **Considere la posibilidad de completar la migración** sin asumir las identidades de los servidores de origen.
- **Transferir nuevo**, copiar solo los archivos que se actualizaron desde la última transferencia.

Si su objetivo es sincronizar los archivos con Azure, puede configurar los servidores de destino con Azure File Sync después de transferir archivos, o después corte sobre a los servidores de destino (consulte [para planear una implementación de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>Paso 3: Opcionalmente, pasar a los nuevos servidores

En este paso se recorta a través de los servidores de origen a los servidores de destino, mover las direcciones IP y nombres de equipo a los servidores de destino. Una vez finalizado este paso, las aplicaciones y los usuarios tener acceso a los nuevos servidores a través de los nombres y direcciones de los servidores que ha migrado desde.

 1. Si se ha desplazado fuera del trabajo de migración de Windows Admin Center, vaya a **administrador del servidor** > **Storage Migration Service** y, a continuación, seleccione el trabajo que desea completar. 
 1. En el **definitivo a los nuevos servidores** > **escriba credenciales** página, seleccione **siguiente** para usar las credenciales que escribió anteriormente.
 1. En el **configurar traslado** página, especifique qué adaptadores de red para tomar el control de configuración de cada dispositivo de origen. Mueve la dirección IP desde el origen al destino como parte de la transición.
 1. Especifique qué dirección IP para que use para el servidor de origen después de migración mueve su dirección en el destino. Puede utilizar DHCP o una dirección estática. Si usa una dirección estática, la nueva subred debe ser que la misma que la subred antigua o traslado se producirá un error.
    ![Captura de pantalla con un servidor de origen y sus direcciones IP y nombres de equipo y lo se reemplazarán con después de la transición](media/migrate/cutover.png)
    **figura 4: Un servidor de origen y la forma en que su configuración de red se moverá al destino**
 1. Especifique cómo cambiar el nombre del servidor de origen después de que el servidor de destino tiene sobre su nombre. Puede usar un nombre generado al azar o escribir uno propio. A continuación, seleccione **siguiente**.
 1. Seleccione **siguiente** en el **ajústela traslado** página.
 1. Seleccione **validar** en el **dispositivo de origen y destino validar** página y, a continuación, seleccione **siguiente**.
 1. Cuando esté listo para realizar la transición, seleccione **iniciar transición**. <br>Los usuarios y aplicaciones podrían experimentar una interrupción mientras se mueven a la dirección y los nombres y los servidores reinician varias veces cada uno, pero en caso contrario, se verán afectadas por la migración. ¿Durante cuánto tiempo tarda traslado depende rápidamente cómo reiniciarán los servidores, así como tiempos de replicación de Active Directory y DNS.

## <a name="see-also"></a>Vea también

- [Información general sobre el servicio de migración de almacenamiento](overview.md)
- [Preguntas más frecuentes (P+F) de servicios de migración de almacenamiento](faq.md)
- [Planeamiento de una implementación de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)