---
title: Eventos de registro del sistema de clústeres de conmutación por error
description: Una lista de eventos de clústeres de conmutación por error en el registro del sistema de Windows Server. Estos eventos se pueden usar para solucionar problemas de un clúster.
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.date: 01/14/2020
ms.openlocfilehash: 17d61291822586013fa77bb1c7c399ab87dfef17
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957162"
---
# <a name="failover-clustering-system-log-events"></a>Eventos de registro del sistema de clústeres de conmutación por error

> Se aplica a: Windows Server 2019, Windows Server 2016

En este tema se enumeran los eventos de clústeres de conmutación por error del registro del sistema de Windows Server (visibles en Visor de eventos). Todos estos eventos comparten el origen de eventos de **FailoverClustering** y pueden ser útiles para solucionar problemas de un clúster.

## <a name="critical-events"></a>Eventos críticos

### <a name="event-1000-unexpected_fatal_error"></a>Evento 1000: UNEXPECTED_FATAL_ERROR

Servicio de clúster sufrió un error irrecuperable inesperado en la línea %1 del módulo de origen %2. El código de error era %3.

### <a name="event-1001-assertion_failure"></a>Evento 1001: ASSERTION_FAILURE

Servicio de clúster no se pudo comprobar la validez de la línea %1 del módulo de origen %2. ' %3 '

### <a name="event-1006-nm_event_membership_halt"></a>Evento 1006: NM_EVENT_MEMBERSHIP_HALT

Servicio de clúster se detuvo debido a una conectividad incompleta con otros nodos del clúster.

### <a name="event-1057-dm_database_corrupt_or_missing"></a>Evento 1057: DM_DATABASE_CORRUPT_OR_MISSING

No se pudo cargar la base de datos del clúster. Puede que falte el archivo o que esté dañado.
Se puede intentar la reparación automática.

### <a name="event-1070-nm_event_begin_join_failed"></a>Evento 1070: NM_EVENT_BEGIN_JOIN_FAILED

El nodo no pudo unirse al clúster de conmutación por error ' %1 ' debido al código de error ' %2 '.

### <a name="event-1073-cs_event_inconsistency_halt"></a>Evento 1073: CS_EVENT_INCONSISTENCY_HALT

Se detuvo el Servicio de clúster para evitar una incoherencia en el clúster de conmutación por error. El código de error era ' %1 '.

### <a name="event-1080-cs_diskwrite_failure"></a>Evento 1080: CS_DISKWRITE_FAILURE

El Servicio de clúster no pudo actualizar la base de datos del clúster (código de error ' %1 ').
Las causas posibles son insuficiente espacio en disco o daños en el sistema de archivos.

### <a name="event-1090-cs_event_reg_operation_failed"></a>Evento 1090: CS_EVENT_REG_OPERATION_FAILED

No se puede iniciar el Servicio de clúster. Error al tratar de leer los datos de configuración del registro de Windows. error: ' %1 '. Use el complemento Administración del clúster de conmutación por error para asegurarse de que este equipo es miembro de un clúster.
Si piensa agregar este equipo a un clúster existente, use el Asistente para agregar nodos. Como alternativa, si este equipo se ha configurado como miembro de un clúster, será necesario restaurar los datos de configuración que faltan necesarios para que el servicio de clúster identifique que es miembro de un clúster.
Realice una restauración del estado del sistema de este equipo para restaurar los datos de configuración.

### <a name="event-1092-nm_event_mm_form_failed"></a>Evento 1092: NM_EVENT_MM_FORM_FAILED

No se pudo formar el clúster ' %1 ' con el código de error ' %2 '. El clúster de conmutación por error no estará disponible.

### <a name="event-1093-nm_event_node_not_member"></a>Evento 1093: NM_EVENT_NODE_NOT_MEMBER

El Servicio de clúster no puede identificar el nodo ' %1 ' como miembro del clúster de conmutación por error ' %2 '. Si el nombre de equipo de este nodo ha cambiado recientemente, considere la posibilidad de revertir al nombre anterior. Como alternativa, agregue el nodo al clúster de conmutación por error y, si es necesario, vuelva a instalar las aplicaciones en clúster.

### <a name="event-1105-cs_event_rpc_init_failed"></a>Evento 1105: CS_EVENT_RPC_INIT_FAILED

No se pudo iniciar el Servicio de clúster porque no pudo registrar las interfaces con el servicio RPC. El código de error era ' %1 '.

### <a name="event-1135-event_node_down"></a>Evento 1135: EVENT_NODE_DOWN

El nodo de clúster ' %1 ' se quitó de la pertenencia al clúster de conmutación por error activa. Es posible que se haya detenido el Servicio de clúster en este nodo. Esto también podría deberse a que el nodo ha perdido la comunicación con otros nodos activos en el clúster de conmutación por error.
Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con los adaptadores de red de este nodo. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1146-rcm_event_resmon_died"></a>Evento 1146: RCM_EVENT_RESMON_DIED

Se terminó el proceso de Subsistema de hospedaje de recursos de clúster (RHS) y se reiniciará. Normalmente, se asocia a la detección y recuperación del mantenimiento de un recurso. Consulte el registro de eventos del sistema para determinar qué recurso y DLL de recursos están causando el problema.

### <a name="event-1177-mm_event_arbitration_failed"></a>Evento 1177: MM_EVENT_ARBITRATION_FAILED

El Servicio de clúster se está cerrando porque se perdió el cuórum. Esto podría deberse a la pérdida de conectividad de red entre algunos o todos los nodos del clúster, o una conmutación por error del disco testigo.

Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con el adaptador de red. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1181-netres_resource_start_error"></a>Evento 1181: NETRES_RESOURCE_START_ERROR

No se puede poner en línea el recurso de clúster ' %1 ' porque no se pudo iniciar el servicio asociado. El código de retorno del servicio es ' %2 '. Compruebe si hay eventos adicionales asociados con el servicio y asegúrese de que el servicio se inicia correctamente.

### <a name="event-1247-cluster_event_invalid_service_sid_type"></a>Evento 1247: CLUSTER_EVENT_INVALID_SERVICE_SID_TYPE

El tipo de identificador de seguridad (SID) para el servicio de clúster está configurado como ' %1 ', pero el tipo de SID esperado es ' Unrestricted '. El servicio de clúster modifica automáticamente su configuración de tipo de SID con el administrador de control de servicios (SCM) y se reiniciará para que este cambio surta efecto.

### <a name="event-1248-cluster_event_service_sid_missing"></a>Evento 1248: CLUSTER_EVENT_SERVICE_SID_MISSING

El identificador de seguridad (SID) ' %1 ' asociado con el servicio de clúster no está presente en el token de proceso. El servicio de Cluster Server corregirá automáticamente este problema y se reiniciará.

### <a name="event-1282-sm_event_handshake_timeout"></a>Evento 1282: SM_EVENT_HANDSHAKE_TIMEOUT

El protocolo de enlace de seguridad entre los extremos locales y remotos ' %2 ' no se completó en ' %1 ' segundos, el nodo finalizó la conexión

### <a name="event-1542-service_prerestore_failed"></a>Evento 1542: SERVICE_PRERESTORE_FAILED

Error en la solicitud de restauración de los datos de configuración del clúster. No se pudo realizar la restauración durante la fase de "restauración previa", lo que indica que algunos nodos que componen el clúster no están operativos actualmente. Asegúrese de que el servicio de clúster se está ejecutando correctamente en todos los nodos que componen este clúster.

### <a name="event-1543-service_postrestore_failed"></a>Evento 1543: SERVICE_POSTRESTORE_FAILED

No se pudo realizar la operación de restauración de los datos de configuración del clúster. No se pudo realizar la restauración durante la fase "posterior a la restauración", lo que indica que algunos nodos que componen el clúster no están operativos actualmente. Se recomienda reemplazar el archivo de datos de configuración de clúster (ClusDB) actual por ' %1 '.

### <a name="event-1546-service_form_version_incompatible"></a>Evento 1546: SERVICE_FORM_VERSION_INCOMPATIBLE

No se pudo formar un clúster de conmutación por error en el nodo ' %1 '. Esto se debe a uno o varios nodos que ejecutan versiones incompatibles del software del servicio de Cluster Server. Si el nodo ' %1 ' o un nodo diferente del clúster se han actualizado recientemente, vuelva a comprobar que todos los nodos ejecutan versiones compatibles del software de servicio de clúster.

### <a name="event-1547-service_connect_version_incompatible"></a>Evento 1547: SERVICE_CONNECT_VERSION_INCOMPATIBLE

El nodo ' %1 ' intentó unirse a un clúster de conmutación por error, pero no se pudo realizar la incompatibilidad entre las versiones del software de servicio de clúster. Si el nodo ' %1 ' o un nodo diferente del clúster se han actualizado recientemente, compruebe que se admite la implementación de clúster cambiada con versiones diferentes del software de servicio de clúster.

### <a name="event-1553-service_no_network_connectivity"></a>Evento 1553: SERVICE_NO_NETWORK_CONNECTIVITY

Este nodo de clúster no tiene conectividad de red. No puede participar en el clúster hasta que se restaure la conectividad. Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con el adaptador de red. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1554-service_network_connectivity_lost"></a>Evento 1554: SERVICE_NETWORK_CONNECTIVITY_LOST

Este nodo de clúster ha perdido toda la conectividad de red. No puede participar en el clúster hasta que se restaure la conectividad. Se terminará el servicio de clúster de este nodo.

### <a name="event-1556-service_unhandled_exception_in_worker_thread"></a>Evento 1556: SERVICE_UNHANDLED_EXCEPTION_IN_WORKER_THREAD

El servicio de clúster encontró un problema inesperado y se cerrará. El código de error era ' %2 '.

### <a name="event-1561-service_nonstorage_witness_better_tag"></a>Evento 1561: SERVICE_NONSTORAGE_WITNESS_BETTER_TAG

Servicio de clúster no se pudo iniciar porque este nodo detectó que no tiene la copia más reciente de los datos de configuración del clúster. Los cambios en el clúster se produjeron mientras este nodo no estaba en pertenencia y, como resultado, no podía recibir actualizaciones de datos de configuración.

#### <a name="guidance"></a>Instrucciones

Intente iniciar el servicio de clúster en todos los nodos del clúster para que los nodos con la última copia de los datos de configuración del clúster puedan formar el clúster por primera vez. Este nodo podrá unirse al clúster y obtendrá automáticamente los datos de configuración del clúster actualizados. Si no hay ningún nodo disponible con la última copia de los datos de configuración del clúster, ejecute el cmdlet de Windows PowerShell "Start-ClusterNode-FQ". El uso del parámetro ForceQuorum (FQ) iniciará el servicio de clúster y marcará la copia de este nodo de los datos de configuración del clúster para que sean autoritativos. Forzar el cuórum en un nodo con una copia obsoleta de la base de datos del clúster puede dar lugar a cambios en la configuración del clúster que se produjeron mientras el nodo no estaba participando en el clúster para ser perdido.

### <a name="event-1564-res_fsw_arbitratefailure"></a>Evento 1564: RES_FSW_ARBITRATEFAILURE

El recurso del testigo del recurso compartido de archivos ' %1 ' no pudo arbitrar el recurso compartido de archivos ' %2 '.
Asegúrese de que el recurso compartido de archivos ' %2 ' existe y es accesible para el clúster.

### <a name="event-1570-service_connect_authentication_failure"></a>Evento 1570: SERVICE_CONNECT_AUTHENTICATION_FAILURE

El nodo ' %1 ' no pudo establecer una sesión de comunicación al unirse al clúster.
Esto se debe a un error de autenticación. Compruebe que los nodos ejecutan versiones compatibles del software de servicio de clúster.

### <a name="event-1571-service_connect_authorizaion_failure"></a>Evento 1571: SERVICE_CONNECT_AUTHORIZAION_FAILURE

El nodo ' %1 ' no pudo establecer una sesión de comunicación al unirse al clúster.
Esto se debe a un error de autorización. Compruebe que los nodos ejecutan versiones compatibles del software de servicio de clúster.

### <a name="event-1572-service_netft_route_initial_failure"></a>Evento 1572: SERVICE_NETFT_ROUTE_INITIAL_FAILURE

El nodo ' %1 ' no pudo unirse al clúster porque no pudo enviar y recibir mensajes de red de detección de errores con otros nodos de clúster. Ejecute el Asistente para validar una configuración para garantizar la configuración de red. Compruebe también las reglas de los clústeres de conmutación por error de Firewall de Windows.

### <a name="event-1574-dm_database_unload_failed"></a>Evento 1574: DM_DATABASE_UNLOAD_FAILED

No se pudo descargar la base de datos del clúster de conmutación por error. Si al reiniciar el servicio de Cluster Server no se soluciona el problema, reinicie el equipo.

### <a name="event-1575-dm_database_corrupt_or_missing_fixquorum"></a>Evento 1575: DM_DATABASE_CORRUPT_OR_MISSING_FIXQUORUM

Error al intentar iniciar el servicio de clúster de forma forzada porque faltan los datos de configuración del clúster en este nodo o están dañados. Primero, inicie el servicio de clúster en otro nodo que tenga una copia intacta y válida de los datos de configuración del clúster. A continuación, vuelva a intentar la operación de inicio en este nodo (que intentará obtener automáticamente la información de configuración válida actualizada). Si no hay ningún otro nodo disponible, use WBAdmin. msc para realizar una restauración del estado del sistema de este nodo con el fin de restaurar los datos de configuración.

### <a name="event-1593-dm_could_not_discard_changes"></a>Evento 1593: DM_COULD_NOT_DISCARD_CHANGES

No se pudo descargar la base de datos del clúster de conmutación por error y no se pudieron descartar los cambios potencialmente incorrectos en la memoria. El servicio de Cluster Server intentará reparar la base de datos al recuperarla de otro nodo de clúster. Si el servicio de clúster no se conecta, reinicie el servicio de clúster en este nodo. Si al reiniciar el servicio de Cluster Server no se soluciona el problema, reinicie el equipo. Si el servicio de clúster no puede conectarse después de un reinicio, restaure la base de datos de clúster desde la última copia de seguridad. La base de datos actual se copió en ' %1 '. Si no hay copias de seguridad disponibles, copie ' %1 ' en ' %2 ' e intente iniciar el servicio de Cluster Server. Si el servicio de clúster se pone en línea en este nodo, es posible que se pierdan algunos cambios de configuración del clúster y que el clúster no funcione correctamente. Ejecute el Asistente para validar una configuración para comprobar la configuración del clúster y comprobar que los servicios hospedados y las aplicaciones están en línea y funcionan correctamente.

### <a name="event-1672-event_node_quarantined"></a>Evento 1672: EVENT_NODE_QUARANTINED

El nodo de clúster ' %1 ' se ha puesto en cuarentena. El nodo experimentó errores consecutivos ' %2 ' en un breve período de tiempo y se ha quitado del clúster para evitar interrupciones posteriores. El nodo se pondrá en cuarentena hasta ' %3 ' y, a continuación, el nodo intentará volver a unirse al clúster automáticamente.

Consulte los registros de eventos del sistema y de la aplicación para determinar los problemas de este nodo. Cuando se resuelve el problema, se puede borrar manualmente la cuarentena para permitir que el nodo se vuelva a unir con el cmdlet de Windows PowerShell "Start-ClusterNode – ClearQuarantine".

Nombre del nodo: %1

Número de pertenencias de clúster consecutivas: %2

Hora en que se borrará automáticamente la cuarentena: %3

## <a name="error-events"></a>Eventos de error

### <a name="event-1024-cp_reg_ckpt_restore_failed"></a>Evento 1024: CP_REG_CKPT_RESTORE_FAILED

No se pudo restaurar el punto de control del registro para el recurso de clúster ' %1 ' en la clave del registro HKEY_LOCAL_MACHINE \\ %2. Es posible que el recurso no funcione correctamente.
Asegúrese de que ningún otro proceso tiene identificadores abiertos a las claves del registro en este subárbol del registro.

### <a name="event-1034-res_disk_missing"></a>Evento 1034: RES_DISK_MISSING

No se puede poner en línea el recurso de disco físico de clúster ' %1 ' porque no se encontró el disco asociado. La firma esperada del disco era ' %2 '.
Si el disco se ha reemplazado o restaurado, en el complemento Administrador de clústeres de conmutación por error, puede usar la función de reparación (en la hoja de propiedades del disco) para reparar el disco nuevo o restaurado. Si no se va a reemplazar el disco, elimine el recurso de disco asociado.

### <a name="event-1035-res_disk_mount_failed"></a>Evento 1035: RES_DISK_MOUNT_FAILED

Mientras se estaba conectando el recurso de disco ' %1 ', no se pudo obtener acceso a uno o más volúmenes con el error ' %2 '. Ejecute el Asistente para validar una configuración para comprobar la configuración del almacenamiento. Opcionalmente, puede que desee ejecutar chkdsk para comprobar la integridad de todos los volúmenes de este disco.

### <a name="event-1037-res_disk_filesystem_failed"></a>Evento 1037: RES_DISK_FILESYSTEM_FAILED

El sistema de archivos de una o varias particiones en el disco para el recurso ' %1 ' puede estar dañado. Ejecute el Asistente para validar una configuración para comprobar la configuración del almacenamiento. Opcionalmente, puede que desee ejecutar chkdsk para comprobar la integridad de todos los volúmenes de este disco.

### <a name="event-1038-res_disk_reservation_lost"></a>Evento 1038: RES_DISK_RESERVATION_LOST

Este nodo ha perdido inesperadamente la propiedad del disco de clúster ' %1 '. Ejecute el Asistente para validar una configuración para comprobar la configuración del almacenamiento.

### <a name="event-1039-res_genapp_create_failed"></a>Evento 1039: RES_GENAPP_CREATE_FAILED

No se pudo poner en línea la aplicación genérica ' %1 ' (con el error ' %2 ') durante un intento de crear el proceso. Las posibles causas son: puede que la aplicación no esté presente en este nodo, que el nombre de la ruta de acceso se haya especificado incorrectamente, que el nombre binario se haya especificado de forma incorrecta.

### <a name="event-1040-res_gensvc_open_failed"></a>Evento 1040: RES_GENSVC_OPEN_FAILED

No se pudo poner en línea el servicio genérico ' %1 ' (con el error ' %2 ') durante un intento de abrir el servicio. Entre las posibles causas se incluyen: el servicio no está instalado o el nombre del servicio especificado no es válido.

### <a name="event-1041-res_gensvc_start_failed"></a>Evento 1041: RES_GENSVC_START_FAILED

No se pudo poner en línea el servicio genérico ' %1 ' (con el error ' %2 ') durante un intento de iniciar el servicio. Causa posible: puede que los parámetros de servicio especificados no sean válidos.

### <a name="event-1042-res_gensvc_failed_after_start"></a>Evento 1042: RES_GENSVC_FAILED_AFTER_START

No se pudo ejecutar el servicio genérico ' %1 '. error: ' %2 '. Examine el registro de eventos de la aplicación.

### <a name="event-1044-res_ipaddr_nbt_interface_create_failed"></a>Evento 1044: RES_IPADDR_NBT_INTERFACE_CREATE_FAILED

Se detectó un error al intentar crear una nueva interfaz NetBIOS al poner en línea el recurso ' %1 ' (código de error ' %2 '). Es posible que se haya superado el número máximo de nombres NetBIOS.

### <a name="event-1046-res_ipaddr_invalid_subnet"></a>Evento 1046: RES_IPADDR_INVALID_SUBNET

No se puede poner en línea el recurso de dirección IP del clúster ' %1 ' porque el valor de la máscara de subred no es válido. Compruebe las propiedades del recurso de dirección IP.

### <a name="event-1047-res_ipaddr_invalid_address"></a>Evento 1047: RES_IPADDR_INVALID_ADDRESS

No se puede poner en línea el recurso de dirección IP del clúster ' %1 ' porque el valor de la dirección no es válido. Compruebe las propiedades del recurso de dirección IP.

### <a name="event-1048-res_ipaddr_invalid_adapter"></a>Evento 1048: RES_IPADDR_INVALID_ADAPTER

No se pudo conectar el recurso de dirección IP del clúster ' %1 '. No se pudieron determinar los datos de configuración para el adaptador de red correspondiente a la interfaz de red del clúster ' %2 ' (código de error: ' %3 '). Compruebe que el recurso de dirección IP está configurado con la dirección y las propiedades de red correctas.

### <a name="event-1049-res_ipaddr_in_use"></a>Evento 1049: RES_IPADDR_IN_USE

El recurso de dirección IP del clúster ' %1 ' no se puede poner en línea porque se detectó una dirección IP duplicada ' %2 ' en la red. Asegúrese de que todas las direcciones IP son únicas.

### <a name="event-1050-res_netname_duplicate"></a>Evento 1050: RES_NETNAME_DUPLICATE

No se puede poner en línea el recurso de nombre de red de clúster ' %1 ' porque el nombre ' %2 ' coincide con este nombre de nodo de clúster. Asegúrese de que los nombres de red son únicos.

### <a name="event-1051-res_netname_no_ip_address"></a>Evento 1051: RES_NETNAME_NO_IP_ADDRESS

No se puede poner en línea el recurso de nombre de red de clúster ' %1 '. Asegúrese de que los adaptadores de red para los recursos de direcciones IP dependientes tienen acceso a al menos un servidor DNS. También puede habilitar NetBIOS para direcciones IP dependientes.

### <a name="event-1052-res_netname_cant_add_name_status"></a>Evento 1052: RES_NETNAME_CANT_ADD_NAME_STATUS

No se puede poner en línea el recurso de nombre de red de clúster ' %1 ' porque no se pudo agregar el nombre al sistema. El código de error asociado se almacena en la sección de datos.

### <a name="event-1053-res_smb_cant_create_share"></a>Evento 1053: RES_SMB_CANT_CREATE_SHARE

No se puede poner en línea el recurso compartido de archivos de clúster ' %1 ' porque no se pudo crear el recurso compartido.

### <a name="event-1054-res_smb_share_not_found"></a>Evento 1054: RES_SMB_SHARE_NOT_FOUND

Error en la comprobación de estado del recurso compartido de archivos ' %1 '. Al recuperar la información del recurso compartido ' %2 ' (cuyo ámbito es el nombre de red %3) se devolvió el código de error ' %4 '. Asegúrese de que el recurso compartido existe y es accesible.

### <a name="event-1055-res_smb_share_failed"></a>Evento 1055: RES_SMB_SHARE_FAILED

Error en la comprobación de estado del recurso compartido de archivos ' %1 '. Al recuperar la información del recurso compartido ' %2 ' (cuyo ámbito es el nombre de red %3) se indicó que el recurso compartido no existe (código de error ' %4 '). Asegúrese de que el recurso compartido existe y es accesible.

### <a name="event-1069-rcm_resource_failure"></a>Evento 1069: RCM_RESOURCE_FAILURE

Error del recurso de clúster ' %1 ' en el rol en clúster ' %2 '.

### <a name="event-1069-rcm_resource_failure_with_typename"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_TYPENAME

Error del recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 '.<br><br>En función de las directivas de error para el recurso y el rol, es posible que el servicio de clúster intente poner el recurso en línea en este nodo o mueva el grupo a otro nodo del clúster y, a continuación, reinícielo. Compruebe el estado de los recursos y grupos mediante Administrador de clústeres de conmutación por error o el cmdlet de Windows PowerShell Get-ClusterResource.

### <a name="event-1069-rcm_resource_failure_with_cause"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_CAUSE

Error del recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 '. El código de error era ' %5 ' (' %4 ').<br><br>En función de las directivas de error para el recurso y el rol, es posible que el servicio de clúster intente poner el recurso en línea en este nodo o mueva el grupo a otro nodo del clúster y, a continuación, reinícielo. Compruebe el estado de los recursos y grupos mediante Administrador de clústeres de conmutación por error o el cmdlet de Windows PowerShell Get-ClusterResource.

### <a name="event-1069-rcm_resource_failure_with_error_code"></a>Evento 1069: RCM_RESOURCE_FAILURE_WITH_ERROR_CODE

Error del recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 '. El código de error era ' %4 '.<br><br>En función de las directivas de error para el recurso y el rol, es posible que el servicio de clúster intente poner el recurso en línea en este nodo o mueva el grupo a otro nodo del clúster y, a continuación, reinícielo. Compruebe el estado de los recursos y grupos mediante Administrador de clústeres de conmutación por error o el cmdlet de Windows PowerShell Get-ClusterResource.

### <a name="event-1069-rcm_resource_failure_due_to_veto"></a>Evento 1069: RCM_RESOURCE_FAILURE_DUE_TO_VETO

No se pudo ejecutar el recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 ' debido a un intento de bloquear un cambio de Estado requerido en ese recurso de clúster.

### <a name="event-1077-res_ipaddr_ipv4_address_interface_failed"></a>Evento 1077: RES_IPADDR_IPV4_ADDRESS_INTERFACE_FAILED

Error en la comprobación de estado de la interfaz IP ' %1 ' (dirección ' %2 ') (el estado es ' %3 '). Ejecute el Asistente para validar una configuración para asegurarse de que el adaptador de red funciona correctamente.

### <a name="event-1078-res_ipaddr_wins_address_failed"></a>Evento 1078: RES_IPADDR_WINS_ADDRESS_FAILED

No se puede poner en línea el recurso de dirección IP del clúster ' %1 ' porque no se pudo registrar WINS en la interfaz ' %2 ' con el error ' %3 '. Asegúrese de que se ha especificado un servidor WINS válido y accesible.

### <a name="event-1121-cp_crypto_ckpt_restore_failed"></a>Evento 1121: CP_CRYPTO_CKPT_RESTORE_FAILED

La configuración cifrada para el recurso de clúster ' %1 ' no se pudo aplicar correctamente al nombre de contenedor ' %2 ' en este nodo.

### <a name="event-1127-tm_event_cluster_netinterface_failed"></a>Evento 1127: TM_EVENT_CLUSTER_NETINTERFACE_FAILED

Error de la interfaz de red de clúster ' %1 ' para el nodo de clúster ' %2 ' en la red ' %3 '. Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con el adaptador de red. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1129-tm_event_cluster_network_partitioned"></a>Evento 1129: TM_EVENT_CLUSTER_NETWORK_PARTITIONED

La red de clúster ' %1 ' tiene particiones. Algunos nodos de clúster de conmutación por error conectados no pueden comunicarse entre sí a través de la red. El clúster de conmutación por error no pudo determinar la ubicación del error. Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con el adaptador de red. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1130-tm_event_cluster_network_down"></a>Evento 1130: TM_EVENT_CLUSTER_NETWORK_DOWN

La red de clúster ' %1 ' está inactiva. Ninguno de los nodos disponibles puede comunicarse a través de esta red. Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con el adaptador de red. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1137-rcm_drain_move_failed"></a>Evento 1137: RCM_DRAIN_MOVE_FAILED

No se pudo completar el movimiento del rol de clúster ' %1 ' para el purgado. No se pudo realizar la operación. código de error: %2.

### <a name="event-1138-res_smb_cant_create_dfs_root"></a>Evento 1138: RES_SMB_CANT_CREATE_DFS_ROOT

No se puede poner en línea el recurso compartido de archivos de clúster ' %1 '. No se pudo crear la raíz del espacio de nombres DFS: ' %3 '. Esto puede deberse a un error al iniciar el servicio "espacio de nombres DFS" o a que no se pudo crear la raíz DFS-N para el recurso compartido "%2".

### <a name="event-1141-res_smb_cant_init_dfs_svc"></a>Evento 1141: RES_SMB_CANT_INIT_DFS_SVC

No se puede poner en línea el recurso compartido de archivos de clúster ' %1 '. No se pudo resincronizar el destino de raíz DFS ' %2 ' con el error ' %3 '.

### <a name="event-1142-res_smb_cant_online_dfs_root"></a>Evento 1142: RES_SMB_CANT_ONLINE_DFS_ROOT

No se puede poner en línea el recurso compartido de archivos de clúster ' %1 ' para el espacio de nombres DFS debido al error ' %2 '.

### <a name="event-1182-netres_resource_stop_error"></a>Evento 1182: NETRES_RESOURCE_STOP_ERROR

El recurso de clúster ' %1 ' no se puede poner en línea porque se produjo un error en el reinicio del servicio asociado. El código de error es ' %2 '. El servicio requirió un reinicio para actualizar los parámetros. Sin embargo, se produjo un error en el servicio antes de que se pudiera detener y reiniciar. Compruebe si hay eventos adicionales asociados con el servicio y asegúrese de que el servicio funciona correctamente.

### <a name="event-1183-res_disk_invalid_mp_source_not_clustered"></a>Evento 1183: RES_DISK_INVALID_MP_SOURCE_NOT_CLUSTERED

El recurso de disco de clúster ' %1 ' contiene un punto de montaje no válido. Tanto los discos de origen como los de destino asociados con el punto de montaje deben ser discos en clúster y deben ser miembros del mismo grupo. <br>El punto de montaje ' %2 ' para el volumen ' %3 ' hace referencia a un disco de origen no válido. Asegúrese de que el disco de origen sea también un disco en clúster y en el mismo grupo que el disco de destino (que hospeda el punto de montaje).

### <a name="event-1191-res_netname_delete_computer_account_failed_status"></a>Evento 1191: RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED_STATUS

El recurso de nombre de red de clúster ' %1 ' no pudo eliminar su objeto de equipo asociado en el dominio ' %2 '. El código de error era ' %3 '. <br>Pida a un administrador de dominio que elimine manualmente el objeto de equipo del dominio de Active Directory.

### <a name="event-1192-res_netname_delete_computer_account_failed"></a>Evento 1192: RES_NETNAME_DELETE_COMPUTER_ACCOUNT_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo eliminar su objeto de equipo asociado en el dominio ' %2 ' por el siguiente motivo:<br>%3. <br><br>Pida a un administrador de dominio que elimine manualmente el objeto de equipo del dominio de Active Directory.

### <a name="event-1193-res_netname_add_computer_account_failed_status"></a>Evento 1193: RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED_STATUS

El recurso de nombre de red de clúster ' %1 ' no pudo crear su objeto de equipo asociado en el dominio ' %2 ' por el siguiente motivo: %3.<br><br>El código de error asociado es: %5<br><br>
Trabaje con el administrador del dominio para asegurarse de que:

- La identidad del clúster ' %4 ' puede crear objetos de equipo. De forma predeterminada, todos los objetos de equipo se crean en el contenedor "equipos". Si esta ubicación ha cambiado, consulte al administrador del dominio.
- No se ha alcanzado la cuota de objetos de equipo.
- Si hay un objeto de equipo existente, compruebe que la identidad del clúster ' %4 ' tiene el permiso de control total para ese objeto de equipo mediante la herramienta Active Directory usuarios y equipos.

### <a name="event-1194-res_netname_add_computer_account_failed"></a>Evento 1194: RES_NETNAME_ADD_COMPUTER_ACCOUNT_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo crear su objeto de equipo asociado en el dominio ' %2 ' durante: %3.<br><br>El texto del código de error asociado es: %4

Trabaje con el administrador del dominio para asegurarse de que:

- La identidad del clúster ' %5 ' tiene permisos para crear objetos de equipo. De forma predeterminada, todos los objetos de equipo se crean en el mismo contenedor que la identidad de clúster ' %5 '.
- No se ha alcanzado la cuota de objetos de equipo.
- Si hay un objeto de equipo existente, compruebe que la identidad del clúster ' %5 ' tiene el permiso de control total para ese objeto de equipo mediante la herramienta Active Directory usuarios y equipos.

### <a name="event-1195-res_netname_dns_registration_failed_status"></a>Evento 1195: RES_NETNAME_DNS_REGISTRATION_FAILED_STATUS

El recurso de nombre de red de clúster ' %1 ' no pudo registrar uno o varios nombres DNS asociados. El código de error era ' %2 '. Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes estén configurados con acceso a al menos un servidor DNS.

### <a name="event-1196-res_netname_dns_registration_failed"></a>Evento 1196: RES_NETNAME_DNS_REGISTRATION_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar uno o varios nombres DNS asociados por el siguiente motivo:<br>%2.<br><br>Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes estén configurados con al menos un servidor DNS accesible.

### <a name="event-1205-rcm_event_group_failed_online_offline"></a>Evento 1205: RCM_EVENT_GROUP_FAILED_ONLINE_OFFLINE

El Servicio de clúster no pudo poner el rol en clúster ' %1 ' completamente en línea o sin conexión. Uno o más recursos pueden estar en un estado de error. Esto puede afectar a la disponibilidad del rol en clúster.

### <a name="event-1206-res_netname_update_computer_account_failed_status"></a>Evento 1206: RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED_STATUS

No se pudo actualizar el objeto de equipo asociado al recurso de nombre de red de clúster ' %1 ' en el dominio ' %2 '. El código de error era ' %3 '. Es posible que la identidad del clúster ' %4 ' carezca de los permisos necesarios para actualizar el objeto. Trabaje con el administrador del dominio para asegurarse de que la identidad del clúster puede actualizar objetos de equipo en el dominio.

### <a name="event-1207-res_netname_update_computer_account_failed"></a>Evento 1207: RES_NETNAME_UPDATE_COMPUTER_ACCOUNT_FAILED

No se pudo actualizar el objeto de equipo asociado al recurso de nombre de red de clúster ' %1 ' en el dominio ' %2 ' durante el <br>operación %3.<br><br>El texto del código de error asociado es: %4<br> <br>Es posible que la identidad de clúster ' %5 ' no disponga de los permisos necesarios para actualizar el objeto. Trabaje con el administrador del dominio para asegurarse de que la identidad del clúster puede actualizar objetos de equipo en el dominio.

### <a name="event-1208-res_disk_invalid_mp_target_not_clustered"></a>Evento 1208: RES_DISK_INVALID_MP_TARGET_NOT_CLUSTERED

El recurso de disco de clúster ' %1 ' contiene un punto de montaje no válido. Tanto los discos de origen como los de destino asociados con el punto de montaje deben ser discos en clúster y deben ser miembros del mismo grupo. <br>El punto de montaje ' %2 ' para el volumen ' %3 ' hace referencia a un disco de destino no válido. Asegúrese de que el disco de destino también sea un disco en clúster y en el mismo grupo que el disco de origen (que hospeda el punto de montaje).

### <a name="event-1211-res_netname_no_writeable_dc_status"></a>Evento 1211: RES_NETNAME_NO_WRITEABLE_DC_STATUS

No se puede poner en línea el recurso de nombre de red de clúster ' %1 '. Error al intentar localizar un controlador de dominio grabable (en el dominio %2) para crear o actualizar un objeto de equipo asociado al recurso. El código de error era ' %3 '.
Asegúrese de que un controlador de dominio grabable sea accesible para este nodo dentro del dominio configurado. Asegúrese también de que el servidor DNS se está ejecutando con el fin de resolver el nombre del controlador de dominio.

### <a name="event-1212-res_netname_no_writeable_dc"></a>Evento 1212: RES_NETNAME_NO_WRITEABLE_DC

No se puede poner en línea el recurso de nombre de red de clúster ' %1 '. Se produjo un error al intentar localizar un controlador de dominio grabable (en el dominio %2) para crear o actualizar un objeto de equipo asociado al recurso por la siguiente razón:<br>%3.<br><br> El código de error era ' %4 '. Asegúrese de que un controlador de dominio grabable sea accesible para este nodo dentro del dominio configurado. Asegúrese también de que el servidor DNS se está ejecutando con el fin de resolver el nombre del controlador de dominio.

### <a name="event-1213-res_netname_rename_restore_failed"></a>Evento 1213: RES_NETNAME_RENAME_RESTORE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo cambiar completamente el nombre del objeto de equipo asociado en el controlador de dominio ' %2 '. También se ha producido un error al intentar cambiar el nombre del objeto de equipo del nombre nuevo ' %3 ' a su nombre original ' %4 '. El código de error era ' %5 '. Esto puede afectar a la conectividad de cliente hasta que el nombre de red y su nombre de objeto de equipo asociado sean coherentes. Póngase en contacto con el administrador del dominio para cambiar manualmente el nombre del objeto de equipo.

### <a name="event-1214-res_netname_cant_add_name2"></a>Evento 1214: RES_NETNAME_CANT_ADD_NAME2

El recurso de nombre de red de clústeres no se puede registrar con NetBIOS. <br><br>Nombre de red: ' %3 ' <br>Motivo del error: %2 <br>Nombre del recurso: ' %1 ' <br><br> Ejecute Nbtstat como nombre de red para asegurarse de que el nombre no está registrado aún con NetBIOS.

### <a name="event-1215-res_netname_not_registered_with_rdr"></a>Evento 1215: RES_NETNAME_NOT_REGISTERED_WITH_RDR

Error de comprobación de estado del recurso de nombre de red en clúster ' %1 '. El nombre de red ' %2 ' ya no está registrado en este nodo. El código de error era ' %3 '. Compruebe si hay errores de hardware o software relacionados con el adaptador de red. Además, puede ejecutar el Asistente para validar una configuración para comprobar la configuración de red.

### <a name="event-1218-res_netname_online_rename_recovery_missing_account"></a>Evento 1218: RES_NETNAME_ONLINE_RENAME_RECOVERY_MISSING_ACCOUNT

El recurso de nombre de red de clúster ' %1 ' no pudo realizar una operación de cambio de nombre (intentando cambiar el nombre original ' %3 ' al nombre ' %4 '). No se encontró el objeto de equipo en el controlador de dominio ' %2 ' (donde se creó). Se intentará volver a crear el objeto de equipo la próxima vez que se conecte el recurso. Además, trabaje con el administrador del dominio para asegurarse de que el objeto de equipo existe en el dominio.

### <a name="event-1219-res_netname_online_rename_dc_not_found"></a>Evento 1219: RES_NETNAME_ONLINE_RENAME_DC_NOT_FOUND

El recurso de nombre de red de clúster ' %1 ' no pudo realizar una operación de cambio de nombre.
No se pudo establecer contacto con el controlador de dominio ' %2 ' en el que se cambió el nombre del objeto de equipo ' %3 '. El código de error era ' %4 '. Asegúrese de que se puede tener acceso a un controlador de dominio grabable y compruebe si hay algún problema de conectividad.

### <a name="event-1220-res_netname_online_rename_recovery_failed"></a>Evento 1220: RES_NETNAME_ONLINE_RENAME_RECOVERY_FAILED

No se pudo cambiar el nombre de la cuenta de equipo para el recurso ' %1 ' de %2 a %3 con el controlador de dominio %4. El código de error asociado se almacena en la sección de datos.<br>
<br>Se ha cambiado el nombre de la cuenta de equipo para este recurso y no se ha completado. Esto se detectó durante el proceso en línea para este recurso.
Para poder realizar la recuperación, se debe cambiar el nombre de la cuenta de equipo al valor actual de la propiedad nombre, es decir, el nombre que se presenta en la red.<br> <br>Es posible que el controlador de dominio en el que se ha intentado cambiar el nombre no esté disponible; Si es así, espere a que el controlador de dominio esté disponible de nuevo. El controlador de dominio podría denegar el acceso a la cuenta; después de resolver el acceso, intente volver a poner el nombre en línea.<br> <br>Si esto no es posible, deshabilite y vuelva a habilitar la autenticación Kerberos y se realizará un intento para encontrar la cuenta de equipo en un controlador de dominio diferente. Debe cambiar la propiedad nombre a %2 para poder usar la cuenta de equipo existente.

### <a name="event-1223-res_ipaddr_invalid_network_role"></a>Evento 1223: RES_IPADDR_INVALID_NETWORK_ROLE

No se puede poner en línea el recurso de dirección IP de clúster ' %1 ' porque la red de clúster ' %2 ' no está configurada para permitir el acceso de cliente. Use el complemento Administrador de clústeres de conmutación por error para comprobar las propiedades configuradas de la red de clústeres.

### <a name="event-1226-res_netname_tcb_not_held"></a>Evento 1226: RES_NETNAME_TCB_NOT_HELD

El recurso de nombre de red ' %1 ' (con el nombre de red asociado ' %2 ') tiene habilitada la compatibilidad con la autenticación Kerberos. No se pudieron agregar las credenciales requeridas a la LSA. el código de error asociado ' %3 ' indica que no hay suficientes privilegios necesarios para esta operación. El privilegio necesario es ' Trusted Computing base ' y debe estar habilitado localmente en cada nodo que contenga el clúster.

### <a name="event-1227-res_netname_lsa_error"></a>Evento 1227: RES_NETNAME_LSA_ERROR

El recurso de nombre de red ' %1 ' (con el nombre de red asociado ' %2 ') tiene habilitada la compatibilidad con la autenticación Kerberos. No se pudieron agregar las credenciales requeridas a la LSA. el código de error asociado es ' %3 '.

### <a name="event-1228-res_netname_clone_failure"></a>Evento 1228: RES_NETNAME_CLONE_FAILURE

El recurso de nombre de red de clúster ' %1 ' encontró un error al habilitar el nombre de red en este nodo. El motivo del error era: <br> ' %2 '.<br><br> El código de error era ' %3 '. <br><br> Puede volver a poner el recurso de nombre de red sin conexión y en línea de nuevo para volver a intentarlo.

### <a name="event-1229-res_netname_no_ips_for_dns"></a>Evento 1229: RES_NETNAME_NO_IPS_FOR_DNS

El recurso de nombre de red de clúster ' %1 ' no pudo identificar ninguna dirección IP para registrar con un servidor DNS. Asegúrese de que hay una o más redes habilitadas para el uso del clúster con la opción "permitir que los clientes se conecten a través de esta red" y de que cada nodo tiene una dirección IP válida configurada para las redes.

### <a name="event-1230-rcm_deadlock_or_crash_detected"></a>Evento 1230: RCM_DEADLOCK_OR_CRASH_DETECTED

Un componente en el servidor no respondió a tiempo. Esto causó que el recurso de clúster ' %1 ' (tipo de recurso ' %2 ', DLL ' %3 ') supere el umbral de tiempo de espera. Como parte de la detección del estado del clúster, se realizarán acciones de recuperación.
El clúster intentará recuperarse automáticamente finalizando y reiniciando el proceso de Subsistema de hospedaje de recursos (RHS) que está ejecutando este recurso. Compruebe que la infraestructura subyacente (como almacenamiento, redes o servicios) que están asociadas con el recurso funcionan correctamente.

### <a name="event-1230-rcm_resource_control_deadlock_detected"></a>Evento 1230: RCM_RESOURCE_CONTROL_DEADLOCK_DETECTED

Un componente en el servidor no respondió a tiempo. Esto causó que el recurso de clúster ' %1 ' (tipo de recurso ' %2 ', DLL ' %3 ') supere el umbral de tiempo de espera durante el procesamiento del código de control ' %4; '. Como parte de la detección del estado del clúster, se realizarán acciones de recuperación. El clúster intentará recuperarse automáticamente finalizando y reiniciando el proceso de Subsistema de hospedaje de recursos (RHS) que está ejecutando este recurso. Compruebe que la infraestructura subyacente (como almacenamiento, redes o servicios) que están asociadas con el recurso funcionan correctamente.

### <a name="event-1231-res_netname_logon_failure"></a>Evento 1231: RES_NETNAME_LOGON_FAILURE

El recurso de nombre de red de clúster ' %1 ' encontró un error al iniciar sesión en el dominio. El motivo del error era: <br> ' %2 '.<br><br> El código de error era ' %3 '.
<br><br> Asegúrese de que un controlador de dominio sea accesible para este nodo dentro del dominio configurado.

### <a name="event-1232-res_genscript_timeout"></a>Evento 1232: RES_GENSCRIPT_TIMEOUT

El punto de entrada ' %2 ' del recurso de script genérico de clúster ' %1 ' no completó la ejecución de manera oportuna. Esto puede deberse a un bucle infinito u otros problemas, lo que podría provocar una espera infinita. Como alternativa, el valor de tiempo de espera pendiente especificado puede ser demasiado corto para este recurso. Revise el punto de entrada del script ' %2 ' para asegurarse de que se han corregido todas las esperas infinitas posibles en el código de script. A continuación, considere la posibilidad de aumentar el valor de tiempo de espera pendiente si lo considera necesario.

### <a name="event-1233-res_genscript_hangmode"></a>Evento 1233: RES_GENSCRIPT_HANGMODE

Recurso de script genérico de clúster ' %1 ': no se procesará la solicitud para realizar la operación ' %2 '. Esto se debe a un intento con error antes de ejecutar el punto de entrada ' %3 ' de manera oportuna. Revise el código de script de este punto de entrada para asegurarse de que no exista ningún bucle infinito u otros problemas, lo que podría dar lugar a una espera infinita. A continuación, considere la posibilidad de aumentar el valor de tiempo de espera de recursos pendiente si es necesario.

### <a name="event-1234-cluster_event_account_missing_privs"></a>Evento 1234: CLUSTER_EVENT_ACCOUNT_MISSING_PRIVS

El Servicio de clúster ha detectado que faltan uno o varios de los privilegios necesarios en su cuenta de servicio. La lista de privilegios que faltan es: ' %1 ' y actualmente no se ha concedido a la cuenta de servicio. Use ' sc.exe qprivs ClusSvc ' para comprobar los privilegios del Servicio de clúster (ClusSvc). Compruebe también si hay directivas de seguridad o directivas de grupo en Active Directory Domain Services que puedan haber modificado los privilegios predeterminados. Escriba el comando siguiente para conceder a los Servicio de clúster los privilegios necesarios para que funcionen correctamente:

```
sc.exe privs
clussvc
SeBackupPrivilege/SeRestorePrivilege/SeIncreaseQuotaPrivilege/SeIncreaseBasePriorityPrivilege/SeTcbPrivilege/SeDebugPrivilege/SeSecurityPrivilege/SeAuditPrivilege/SeImpersonatePrivilege/SeChangeNotifyPrivilege/SeIncreaseWorkingSetPrivilege/SeManageVolumePrivilege/SeCreateSymbolicLinkPrivilege/SeLoadDriverPrivilege
```

### <a name="event-1242-res_ipaddr_lease_expired"></a>Evento 1242: RES_IPADDR_LEASE_EXPIRED

La concesión de la dirección IP ' %2 ' asociada con el recurso de dirección IP de clúster ' %1 ' ha expirado o está a punto de expirar y actualmente no se puede renovar. Asegúrese de que el servidor DHCP asociado es accesible y está correctamente configurado para renovar la concesión en esta dirección IP.

### <a name="event-1245-res_ipaddr_lease_renewal_failed"></a>Evento 1245: RES_IPADDR_LEASE_RENEWAL_FAILED

El recurso de dirección IP del clúster ' %1 ' no pudo renovar la concesión de la dirección IP ' %2 '.
Asegúrese de que el servidor DHCP es accesible y está correctamente configurado para renovar la concesión en esta dirección IP.

### <a name="event-1250-rcm_resource_embedded_failure"></a>Evento 1250: RCM_RESOURCE_EMBEDDED_FAILURE

El recurso de clúster ' %1 ' en el rol en clúster ' %2 ' ha recibido una notificación de estado crítico. En el caso de una máquina virtual, esto indica que una aplicación o servicio dentro de la máquina virtual se encuentra en un estado incorrecto. Compruebe la funcionalidad del servicio o la aplicación que se está supervisando dentro de la máquina virtual.

### <a name="event-1254-rcm_group_terminal_failure"></a>Evento 1254: RCM_GROUP_TERMINAL_FAILURE

El rol en clúster ' %1 ' superó el umbral de conmutación por error. Ha agotado el número configurado de intentos de conmutación por error dentro del tiempo de conmutación por error asignado a él y se dejará en un estado de error. No se realizarán más intentos para poner el rol en línea o conmutarlo por error a otro nodo del clúster.
Compruebe los eventos asociados al error. Después de resolver los problemas que causan el error, el rol se puede poner en línea manualmente o el clúster puede intentar volver a ponerlo en línea después del tiempo de retraso de reinicio.

### <a name="event-1255-rcm_resource_network_failure"></a>Evento 1255: RCM_RESOURCE_NETWORK_FAILURE

El recurso de clúster ' %1 ' en el rol en clúster ' %2 ' ha recibido una notificación de estado crítico. En el caso de una máquina virtual, esto indica que una red crítica de la máquina virtual se encuentra en un estado incorrecto. Compruebe la conectividad de red de la máquina virtual y las redes virtuales que la máquina virtual está configurada para usar.

### <a name="event-1256-res_netname_dns_registration_failed_dynamic_dns_zone"></a>Evento 1256: RES_NETNAME_DNS_REGISTRATION_FAILED_DYNAMIC_DNS_ZONE

Error de registro de recurso de nombre de red de clúster de uno o varios nombres DNS asociados porque la zona DNS correspondiente no acepta actualizaciones dinámicas.<br><br>Nombre de red en clúster: ' %1 '<br>Zona DNS: ' %2 '

#### <a name="guidance"></a>Instrucciones

Asegúrese de que el DNS esté configurado como una zona DNS dinámica. Si el servidor DNS no acepta actualizaciones dinámicas, desactive la casilla "registrar las direcciones de esta conexión en DNS" en las propiedades del adaptador de red.

### <a name="event-1257-res_netname_dns_registration_failed_secure_dns_zone"></a>Evento 1257: RES_NETNAME_DNS_REGISTRATION_FAILED_SECURE_DNS_ZONE

Error de registro de recurso de nombre de red de clúster de uno o varios nombres DNS asociados porque se denegó el acceso para actualizar la zona DNS segura.<br><br>Nombre de red en clúster: ' %1 '<br>Zona DNS: ' %2 '<br><br>Asegúrese de que el objeto de nombre de clúster (CNO) tiene permisos para la zona DNS segura.

### <a name="event-1258-res_netname_dns_registration_failed_timeout"></a>Evento 1258: RES_NETNAME_DNS_REGISTRATION_FAILED_TIMEOUT

Error de registro de recurso de nombre de red de clúster de uno o varios nombres DNS asociados porque no se pudo establecer contacto con el servidor DNS.<br><br>Nombre de red en clúster: ' %1 '<br>Zona DNS: ' %2 '<br>Servidor DNS: ' %3 '<br><br>Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes estén configurados con al menos un servidor DNS accesible.

### <a name="event-1259-res_netname_dns_registration_failed_cleanup"></a>Evento 1259: RES_NETNAME_DNS_REGISTRATION_FAILED_CLEANUP

Error de registro de recurso de nombre de red de clúster de uno o varios nombres DNS asociados porque el servicio de clúster no pudo limpiar los registros existentes correspondientes al nombre de red.<br><br>Nombre de red en clúster: ' %1 '<br>Zona DNS: ' %2 '<br><br>Asegúrese de que el objeto de nombre de clúster (CNO) tiene permisos para la zona DNS segura.

### <a name="event-1260-res_netname_dns_registration_modify_failed"></a>Evento 1260: RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED

El recurso de nombre de red de clústeres no pudo modificar el registro de DNS.<br><br>Nombre de red en clúster: ' %1 '<br>Código de error: ' %2 '

#### <a name="guidance"></a>Instrucciones

Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes estén configurados con acceso a al menos un servidor DNS.

### <a name="event-1261-res_netname_dns_registration_modify_failed_status"></a>Evento 1261: RES_NETNAME_DNS_REGISTRATION_MODIFY_FAILED_STATUS

El recurso de nombre de red de clústeres no pudo modificar el registro de DNS.<br><br>Nombre de red en clúster: ' %1 '<br>Motivo: ' %2 '

#### <a name="guidance"></a>Instrucciones

Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes estén configurados con acceso a al menos un servidor DNS.

### <a name="event-1262-res_netname_dns_registration_publish_ptr_failed"></a>Evento 1262: RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED

El recurso de nombre de red de clústeres no pudo publicar el registro PTR en la zona de búsqueda inversa de DNS.<br><br>Nombre de red en clúster: ' %1 '<br>Código de error: ' %2 '

#### <a name="guidance"></a>Instrucciones

Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes están configurados con acceso a al menos un servidor DNS y que existe la zona de búsqueda inversa DNS.

### <a name="event-1264-res_netname_dns_registration_publish_ptr_failed_status"></a>Evento 1264: RES_NETNAME_DNS_REGISTRATION_PUBLISH_PTR_FAILED_STATUS

El recurso de nombre de red de clústeres no pudo publicar el registro PTR en la zona de búsqueda inversa de DNS.<br><br>Nombre de red en clúster: ' %1 '<br>Motivo: ' %2 '

#### <a name="guidance"></a>Instrucciones

Asegúrese de que los adaptadores de red asociados a los recursos de direcciones IP dependientes están configurados con acceso a al menos un servidor DNS y que existe la zona de búsqueda inversa DNS.

### <a name="event-1265-res_type_control_timed_out"></a>Evento 1265: RES_TYPE_CONTROL_TIMED_OUT

Se agotó el tiempo de espera del tipo de recurso de clúster ' %1 ' al procesar el código de control %2. El clúster intentará recuperarse automáticamente finalizando y reiniciando el proceso de Subsistema de hospedaje de recursos (RHS) que estaba procesando la llamada.

### <a name="event-1289-netft_adapter_not_found"></a>Evento 1289: NETFT_ADAPTER_NOT_FOUND

El servicio de Cluster Server no pudo obtener acceso al adaptador de red ' %1 '. Compruebe que otros adaptadores de red funcionan correctamente y compruebe si hay errores asociados con el adaptador ' %1 ' en el administrador de dispositivos. Si se ha cambiado la configuración del adaptador ' %1 ', es posible que sea necesario volver a instalar la característica de clústeres de conmutación por error en este equipo.

### <a name="event-1360-res_ipaddr_invalid_network"></a>Evento 1360: RES_IPADDR_INVALID_NETWORK

No se pudo conectar el recurso de dirección IP del clúster ' %1 '. Asegúrese de que la propiedad de red ' %2 ' coincide con un nombre de red de clúster o que la propiedad de dirección ' %3 ' coincide con una de las subredes de una red de clústeres. Si se trata de un tipo de dirección IPv6, compruebe que la red de clústeres que coincide con este recurso tiene al menos un prefijo IPv6 que no es local de vínculo ni túnel.

### <a name="event-1361-res_ipaddr_missing_dependant"></a>Evento 1361: RES_IPADDR_MISSING_DEPENDANT

El recurso de dirección de túnel IPv6 ' %1 ' no pudo conectarse porque no depende de un recurso de dirección IP (IPv4). Se requiere una dependencia en un recurso de dirección IP (IPv4) como mínimo.

### <a name="event-1362-res_ipaddr_missing_data"></a>Evento 1362: RES_IPADDR_MISSING_DATA

El recurso de dirección IP del clúster ' %1 ' no pudo conectarse porque no se pudo leer la propiedad ' %2 '. Asegúrese de que el recurso está configurado correctamente.

### <a name="event-1363-res_ipaddr_no_isatap_support"></a>Evento 1363: RES_IPADDR_NO_ISATAP_SUPPORT

No se pudo conectar el recurso de dirección de túnel IPv6 ' %1 '. La red de clúster ' %2 ' asociada con el recurso de dirección IP dependiente (IPv4) ' %3 ' no admite la tunelización ISATAP. Asegúrese de que la red de clústeres sea compatible con la tunelización de ISATAP.

### <a name="event-1540-service_backup_noquorum"></a>Evento 1540: SERVICE_BACKUP_NOQUORUM

La operación de copia de seguridad de los datos de configuración del clúster se ha anulado porque todavía no se ha logrado el cuórum para el clúster. Vuelva a intentar la operación de copia de seguridad una vez que el clúster logre el cuórum.

### <a name="event-1554-service_restore_invaliduser"></a>Evento 1554: SERVICE_RESTORE_INVALIDUSER

No se pudo realizar la operación de restauración de los datos de configuración del clúster. Esto se debe a que no hay suficientes privilegios asociados con la cuenta de usuario que realiza la restauración. Asegúrese de que la cuenta de usuario tiene privilegios de administrador local.

### <a name="event-1557-service_witness_attach_failed"></a>Evento 1557: SERVICE_WITNESS_ATTACH_FAILED

Servicio de clúster no pudo actualizar los datos de configuración del clúster en el recurso de testigo. Asegúrese de que el recurso de testigo esté en línea y accesible.

### <a name="event-1559-res_witness_new_node_conflict"></a>Evento 1559: RES_WITNESS_NEW_NODE_CONFLICT

El recurso compartido de archivos ' %1 ' asociado al recurso de testigo del recurso compartido de archivos está hospedado actualmente en el servidor ' %2 '. Este servidor ' %2 ' se acaba de agregar como nuevo nodo en el clúster de conmutación por error. No se recomienda el hospedaje del testigo de recurso compartido de archivos por cualquier nodo que contenga el mismo clúster. Elija un testigo de recurso compartido de archivos que no esté hospedado en ningún nodo del mismo clúster y modifique la configuración del recurso de testigo del recurso compartido de archivos en consecuencia.

### <a name="event-1560-res_smb_share_conflict"></a>Evento 1560: RES_SMB_SHARE_CONFLICT

El recurso compartido de archivos de clúster ' %1 ' ha detectado conflictos de carpetas compartidas. Como resultado, es posible que algunas de estas carpetas compartidas no sean accesibles. Para rectificar esta situación, asegúrese de que varias carpetas compartidas no tengan el mismo nombre de recurso compartido.

### <a name="event-1563-res_fsw_onlinefailure"></a>Evento 1563: RES_FSW_ONLINEFAILURE

No se pudo conectar el recurso del testigo del recurso compartido de archivos ' %1 '. Asegúrese de que el recurso compartido de archivos ' %2 ' existe y es accesible para el clúster.

### <a name="event-1566-res_netname_timedout"></a>Evento 1566: RES_NETNAME_TIMEDOUT

No se puede poner en línea el recurso de nombre de red de clúster ' %1 '. El subsistema del host de recursos finalizó el recurso de nombre de red porque no completó una operación en un tiempo aceptable. Se agotó el tiempo de espera de la operación al realizar:<br> ' %2 '

### <a name="event-1567-service_failed_to_change_log_size"></a>Evento 1567: SERVICE_FAILED_TO_CHANGE_LOG_SIZE

Servicio de clúster no pudo cambiar el tamaño del registro de seguimiento. Compruebe el valor de ClusterLogSize con el cmdlet de PowerShell "Get-Cluster \| Format-List \* ". Además, use el complemento monitor de rendimiento para comprobar la configuración de la sesión de seguimiento de eventos para FailoverClustering.

### <a name="event-1567-res_vipaddr_address_interface_failed"></a>Evento 1567: RES_VIPADDR_ADDRESS_INTERFACE_FAILED

Error en la comprobación de estado de la interfaz IP ' %1 ' (dirección ' %2 ') (el estado es ' %3 '). Compruebe si hay errores de hardware o software relacionados con los adaptadores de red físicos o virtuales.

### <a name="event-1568-res_cloud_witness_cant_communicate_to_azure"></a>Evento 1568: RES_CLOUD_WITNESS_CANT_COMMUNICATE_TO_AZURE

El recurso de testigo en la nube no pudo acceder a los servicios de almacenamiento Microsoft Azure.<br><br>Recurso de clúster: %1 <br>Nodo de clúster: %2

#### <a name="guidance"></a>Instrucciones

Esto puede deberse a la comunicación de red entre el nodo de clúster y el servicio de Microsoft Azure que se está bloqueando. Compruebe la conectividad de Internet del nodo con Microsoft Azure. Conéctese al Microsoft Azure Portal y compruebe que la cuenta de almacenamiento existe.

### <a name="event-1569-service_using_restricted_network"></a>Evento 1569: SERVICE_USING_RESTRICTED_NETWORK

La red ' %1 ' que se ha deshabilitado para el uso del clúster de conmutación por error se ha encontrado como la única red posible que el nodo ' %2 ' puede usar para comunicarse con otros nodos del clúster. Esto puede afectar a la capacidad del nodo de participar en el clúster. Compruebe la conectividad de red del nodo ' %2 ' y habilite al menos una red para la comunicación del clúster. Ejecute el Asistente para validar una configuración para comprobar la configuración de red.

### <a name="event-1569-res_cloud_witness_token_expired"></a>Evento 1569: RES_CLOUD_WITNESS_TOKEN_EXPIRED

No se pudo autenticar el recurso de testigo en la nube con servicios de Microsoft Azure Storage. Se devolvió un error de acceso denegado al intentar ponerse en contacto con la cuenta de almacenamiento de Microsoft Azure. <br><br>Recurso de clúster: %1

#### <a name="guidance"></a>Instrucciones

Es posible que la clave de acceso de la cuenta de almacenamiento ya no sea válida. Use el Asistente para configurar Cuórum de clúster en el Administrador de clústeres de conmutación por error o el cmdlet de Windows PowerShell Set-ClusterQuorum para configurar el recurso de testigo en la nube con la clave de acceso de la cuenta de almacenamiento actualizada.

### <a name="event-1573-service_form_witness_failed"></a>Evento 1573: SERVICE_FORM_WITNESS_FAILED

No se pudo formar un clúster en el nodo ' %1 '. Esto se debe a que no se pudo obtener acceso al testigo. Asegúrese de que el recurso de testigo está en línea y disponible.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Evento 1580: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre ' %2 ' en el adaptador ' %4 ' en una zona DNS segura. Esto se debe a un registro existente con este nombre y la identidad del clúster no tiene los privilegios suficientes para actualizar ese registro. El código de error era ' %3 '. Póngase en contacto con el administrador del servidor DNS para comprobar que la identidad del clúster tiene permisos en el registro DNS ' %2 '.

### <a name="event-1580-res_netname_dns_registration_secure_zone_failed"></a>Evento 1580: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre ' %2 ' en el adaptador ' %4 ' en una zona DNS segura. Esto se debe a un registro existente con este nombre y la identidad del clúster no tiene los privilegios suficientes para actualizar ese registro. El código de error era ' %3 '. Póngase en contacto con el administrador del servidor DNS para comprobar que la identidad del clúster tiene permisos en el registro DNS ' %2 '.

### <a name="event-1585-res_fileserver_fscheck_srvsvc_stopped"></a>Evento 1585: RES_FILESERVER_FSCHECK_SRVSVC_STOPPED

Error en la comprobación de estado del recurso del servidor de archivos del clúster ' %1 '. Esto se debe a que no se ha iniciado el servicio servidor. Use Administrador del servidor para confirmar el estado del servicio de servidor en este nodo de clúster.

### <a name="event-1586-res_fileserver_fscheck_scoped_name_not_registered"></a>Evento 1586: RES_FILESERVER_FSCHECK_SCOPED_NAME_NOT_REGISTERED

Error en la comprobación de estado del recurso del servidor de archivos del clúster ' %1 '. Esto se debe a que no se pudo obtener acceso a algunas de sus carpetas compartidas. Compruebe que los clientes tienen acceso a las carpetas. Además, confirme el estado del servicio de servidor en este nodo de clúster mediante Administrador del servidor y busque otros eventos relacionados con el servicio de servidor en este nodo de clúster. Puede que sea necesario reiniciar el recurso de nombre de red ' %2 ' en este rol en clúster.

### <a name="event-1587-res_fileserver_fscheck_failed"></a>Evento 1587: RES_FILESERVER_FSCHECK_FAILED

Error en la comprobación de estado del recurso del servidor de archivos del clúster ' %1 '. Esto se debe a que no se pudo obtener acceso a algunas de sus carpetas compartidas. Compruebe que los clientes tienen acceso a las carpetas. Además, confirme el estado del servicio de servidor en este nodo de clúster mediante Administrador del servidor y busque otros eventos relacionados con el servicio de servidor en este nodo de clúster.

### <a name="event-1588-res_fileserver_share_cant_add"></a>Evento 1588: RES_FILESERVER_SHARE_CANT_ADD

No se puede poner en línea el recurso del servidor de archivos de clúster ' %1 '. El recurso no pudo crear el recurso compartido de archivos ' %2 ' asociado con el nombre de red ' %3 '. El código de error era ' %4 '. Compruebe que las carpetas existen y son accesibles. Además, confirme el estado del servicio de servidor en este nodo de clúster mediante Administrador del servidor y busque otros eventos relacionados en este nodo de clúster. Puede que sea necesario reiniciar el recurso de nombre de red ' %3 ' en este rol en clúster.

### <a name="event-1600-clusapi_create_cannot_set_ad_dacl"></a>Evento 1600: CLUSAPI_CREATE_CANNOT_SET_AD_DACL

Servicio de clúster no pudo establecer los permisos en el objeto de equipo de clúster ' %1 '. Póngase en contacto con el administrador de red para comprobar el descriptor de seguridad del clúster del objeto de equipo en Active Directory, compruebe que la DACL no es demasiado grande y quite las ACE adicionales innecesarias del objeto si es necesario.

### <a name="event-1603-res_fileserver_clone_failed"></a>Evento 1603: RES_FILESERVER_CLONE_FAILED

No se pudo iniciar el servidor de archivos porque no se encontró la dependencia esperada en el recurso ' Network Name ' o no se configuró correctamente. Error = 0x %2.

### <a name="event-1606-res_disk_cno_check_failed"></a>Evento 1606: RES_DISK_CNO_CHECK_FAILED

El recurso de disco de clúster ' %1 ' contiene un volumen protegido por BitLocker, ' %2 ', pero para este volumen, el Active Directory cuenta de nombre de clúster (también denominado objeto de nombre de clúster o CNO) no es un protector de BitLocker para el volumen. Esto es necesario para los volúmenes protegidos por BitLocker. Para corregirlo, quite primero el disco del clúster. A continuación, use la herramienta de línea de comandos Manage-bde.exe para agregar el nombre de clúster como un protector de ADAccountOrGroup, con el formato dominio \\ ClusterName \$ para el nombre del clúster. A continuación, vuelva a agregar el disco al clúster. Para obtener más información, consulte la documentación de Manage-bde.exe

### <a name="event-1607-res_disk_cno_unlock_failed"></a>Evento 1607: RES_DISK_CNO_UNLOCK_FAILED

El recurso de disco de clúster ' %1 ' no pudo desbloquear el volumen protegido por BitLocker ' %2 '. El objeto de nombre de clúster (CNO) no está configurado para ser un protector de BitLocker válido para este volumen. Para corregirlo, quite el disco del clúster. A continuación, use la herramienta de línea de comandos Manage-bde.exe para agregar el nombre de clúster como un protector de ADAccountOrGroup, con el formato dominio \\ ClusterName \$ , y vuelva a agregar el disco al clúster. Para obtener más información, consulte la documentación de Manage-bde.exe.

### <a name="event-1608-res_fileserver_leader_failed"></a>Evento 1608: RES_FILESERVER_LEADER_FAILED

No se pudo iniciar el servidor de archivos porque no se encontró la dependencia esperada en el recurso ' Network Name ' o no se configuró correctamente. Error = 0x %2.

### <a name="event-1609-res_soda_fileserver_leader_failed"></a>Evento 1609: RES_SODA_FILESERVER_LEADER_FAILED

Escalabilidad horizontal servidor de archivos no se pudo iniciar porque no se encontró la dependencia esperada en el recurso ' nombre de red distribuida ' o no se configuró correctamente. Error = 0x %2.

### <a name="event-1632-clusapi_create_mismatched_ou"></a>Evento 1632: CLUSAPI_CREATE_MISMATCHED_OU

No se pudo crear el clúster. No se puede crear el objeto de nombre de clúster ' %1 ' en la unidad organizativa ' %2 ' de Active Directory. El objeto ya existe en la unidad organizativa ' %3 '. Compruebe que la ruta de acceso de nombre distintivo especificada y el objeto de nombre de clúster son correctos. Si no se especifica la ruta de acceso del nombre distintivo, se usará el objeto de equipo existente ' %1 '.

### <a name="event-1652-service_tcp_connection_failure"></a>Evento 1652: SERVICE_TCP_CONNECTION_FAILURE

El nodo de clúster ' %1 ' no pudo unirse al clúster. No se pudo establecer una conexión TCP a los nodos ' %2 '. Compruebe la conectividad de red y la configuración de los firewalls de red.

### <a name="event-1652-service_udp_connection_failure"></a>Evento 1652: SERVICE_UDP_CONNECTION_FAILURE

El nodo de clúster ' %1 ' no pudo unirse al clúster. No se pudo establecer una conexión UDP a los nodos ' %2 '. Compruebe la conectividad de red y la configuración de los firewalls de red.

### <a name="event-1652-service_virtual_tcp_connection_failure"></a>Evento 1652: SERVICE_VIRTUAL_TCP_CONNECTION_FAILURE

El nodo de clúster ' %1 ' no pudo unirse al clúster. No se pudo establecer una conexión TCP con el adaptador virtual del clúster de conmutación por error de Microsoft en los nodos ' %2 '. Compruebe la conectividad de red y la configuración de los firewalls de red.

### <a name="event-1653-service_no_connectivity"></a>Evento 1653: SERVICE_NO_CONNECTIVITY

El nodo de clúster ' %1 ' no pudo unirse al clúster porque no se pudo comunicar a través de la red con ningún otro nodo del clúster. Compruebe la conectividad de red y la configuración de los firewalls de red.

### <a name="event-1654-res_vipaddr_invalid_adaptername"></a>Evento 1654: RES_VIPADDR_INVALID_ADAPTERNAME

No se pudo conectar el recurso de dirección IP separado del clúster ' %1 '. No se pudieron determinar los datos de configuración para el adaptador de red correspondiente al adaptador de red ' %2 ' (código de error: ' %3 '). Compruebe que el recurso de dirección IP está configurado con la dirección y las propiedades de red correctas.

### <a name="event-1655-res_vipaddr_invalid_vsid"></a>Evento 1655: RES_VIPADDR_INVALID_VSID

No se pudo conectar el recurso de dirección IP separado del clúster ' %1 '. No se pudieron determinar los datos de configuración para el adaptador de red correspondiente al ID. de subred virtual ' %2 ' y el ID. de dominio de enrutamiento ' %3 ' (código de error: ' %4 '). Compruebe que el recurso de dirección IP está configurado con la dirección y las propiedades de red correctas.

### <a name="event-1656-res_vipaddr_address_create_failed"></a>Evento 1656: RES_VIPADDR_ADDRESS_CREATE_FAILED

No se pudo agregar la dirección IP ' %2 ' para el recurso de dirección IP separado ' %1 ' (código de error: ' %3 '). Compruebe si hay errores de hardware o software relacionados con los adaptadores de red físicos o virtuales.

### <a name="event-1664-cluster_upgrade_incomplete"></a>Evento 1664: CLUSTER_UPGRADE_INCOMPLETE

No se pudo actualizar el nivel funcional del clúster. Compruebe que todos los nodos del clúster se estén ejecutando actualmente y que tengan la misma versión de Windows Server y, a continuación, vuelva a ejecutar el cmdlet Update-ClusterFunctionalLevel de Windows PowerShell.

### <a name="event-1676-event_local_node_quarantined"></a>Evento 1676: EVENT_LOCAL_NODE_QUARANTINED

' %1 ' ha puesto en cuarentena el nodo de clúster local. El nodo se pondrá en cuarentena hasta ' %2 ' y, a continuación, el nodo intentará volver a unirse al clúster automáticamente.
<br><br>Consulte los registros de eventos del sistema y de la aplicación para determinar los problemas de este nodo. Cuando se resuelve el problema, se puede borrar manualmente la cuarentena para permitir que el nodo se vuelva a unir con el cmdlet de Windows PowerShell "Start-ClusterNode – ClearQuarantine".<br><br>QuarantineType: en cuarentena por %1<br>Hora en que se borrará automáticamente la cuarentena: %2

### <a name="event-1677-event_node_drain_failed"></a>Evento 1677: EVENT_NODE_DRAIN_FAILED

Error de purga de nodo en el nodo de clúster %1. <br><br>Haga referencia a los registros de eventos del sistema y de la aplicación del nodo, así como a los registros de clúster para investigar la causa del error de purga. Una vez resuelto el problema, puede volver a intentar la operación de purga.

### <a name="event-1683-res_netname_computer_account_no_dc"></a>Evento 1683: RES_NETNAME_COMPUTER_ACCOUNT_NO_DC

El servicio de Cluster Server no pudo conectar con ningún controlador de dominio disponible en el dominio. Esto puede afectar a la funcionalidad que depende de la autenticación de nombres de red de clústeres.<br><br>Servidor DC: %1

#### <a name="guidance"></a>Instrucciones

Compruebe que los controladores de dominio son accesibles en la red a los nodos del clúster.

### <a name="event-1684-res_netname_computer_object_vco_not_found"></a>Evento 1684: RES_NETNAME_COMPUTER_OBJECT_VCO_NOT_FOUND

El recurso de nombre de red de clústeres no pudo encontrar el objeto de equipo asociado en Active Directory. Esto puede afectar a la funcionalidad que depende de la autenticación de nombres de red de clústeres.<br><br>Nombre de red: %1<br>Unidad organizativa: %2

#### <a name="guidance"></a>Instrucciones

Restaure el objeto de equipo para el nombre de red de la papelera de reciclaje de Active Directory. Como alternativa, desconecte el recurso de nombre de red en clúster y ejecute la acción reparar para volver a crear el objeto de equipo en Active Directory.

### <a name="event-1685-res_netname_computer_object_cno_not_found"></a>Evento 1685: RES_NETNAME_COMPUTER_OBJECT_CNO_NOT_FOUND

El recurso de nombre de red de clústeres no pudo encontrar el objeto de equipo asociado en Active Directory. Esto puede afectar a la funcionalidad que depende de la autenticación de nombres de red de clústeres.<br><br>Nombre de red: %1<br>Unidad organizativa: %2
#### <a name="guidance"></a>Instrucciones

Restaure el objeto de equipo para el nombre de red de la papelera de reciclaje de Active Directory.

### <a name="event-1686-res_netname_computer_object_vco_disabled"></a>Evento 1686: RES_NETNAME_COMPUTER_OBJECT_VCO_DISABLED

El recurso de nombre de red de clústeres encontró el objeto de equipo asociado en Active Directory que se va a deshabilitar. Esto puede afectar a la funcionalidad que depende de la autenticación de nombres de red de clústeres.<br><br>Nombre de red: %1<br>Unidad organizativa: %2

#### <a name="guidance"></a>Instrucciones

Habilite el objeto de equipo para el nombre de red en Active Directory.

### <a name="event-1687-res_netname_computer_object_cno_disabled"></a>Evento 1687: RES_NETNAME_COMPUTER_OBJECT_CNO_DISABLED

El recurso de nombre de red de clústeres encontró el objeto de equipo asociado en Active Directory que se va a deshabilitar. Esto puede afectar a la funcionalidad que depende de la autenticación de nombres de red de clústeres.<br><br>Nombre de red: %1<br>Unidad organizativa: %2

#### <a name="guidance"></a>Instrucciones

Habilite el objeto de equipo para el nombre de red en Active Directory. Como alternativa, desconecte el recurso de nombre de red en clúster y ejecute la acción reparar para habilitar el objeto de equipo en Active Directory.

### <a name="event-1688-res_netname_computer_object_failed"></a>Evento 1688: RES_NETNAME_COMPUTER_OBJECT_FAILED

El recurso de nombre de red en clúster detectó que el objeto de equipo asociado en Active Directory estaba deshabilitado y no se pudo habilitar. Esto puede afectar a la funcionalidad que depende de la autenticación de nombres de red de clústeres.<br><br>Nombre de red: %1<br>Unidad organizativa: %2

#### <a name="guidance"></a>Instrucciones

Habilite el objeto de equipo para el nombre de red en Active Directory.

### <a name="event-4608-nodecleanup_get_clustered_disks_failed"></a>Evento 4608: NODECLEANUP_GET_CLUSTERED_DISKS_FAILED

Servicio de clúster no pudo recuperar la lista de discos en clúster al destruir el clúster. El código de error era ' %1 '. Si no se puede acceder a estos discos, ejecute el cmdlet de PowerShell "Clear-ClusterDiskReservation".

### <a name="event-4611-nodecleanup_release_clustered_disks_from_partmgr_failed"></a>Evento 4611: NODECLEANUP_RELEASE_CLUSTERED_DISKS_FROM_PARTMGR_FAILED

El administrador de particiones no liberó el disco en clúster con el identificador ' %2 ' al destruir el clúster. El código de error era ' %1 '. Al reiniciar el equipo se asegurará de que el administrador de particiones libera el disco.

### <a name="event-4613-nodecleanup_clear_clusdisk_database_failed"></a>Evento 4613: NODECLEANUP_CLEAR_CLUSDISK_DATABASE_FAILED

El servicio de clúster no pudo limpiar correctamente un disco en clúster con el identificador ' %2 ' al destruir el clúster. El código de error era ' %1 '. Es posible que no pueda obtener acceso a este disco hasta que la limpieza se haya completado correctamente. Para la limpieza manual, elimine el valor ' AttachedDisks ' de la \\ clave ' HKEY_LOCAL_MACHINE System \\ CurrentControlSet \\ Services \\ ClusDisk \\ Parameters ' en el registro de Windows.

### <a name="event-4615-nodecleanup_disable_cluster_service_failed"></a>Evento 4615: NODECLEANUP_DISABLE_CLUSTER_SERVICE_FAILED

El servicio de Cluster Server se ha detenido y se ha establecido como deshabilitado como parte de la limpieza del nodo de clúster.

### <a name="event-4616-nodecleanup_disable_cluster_service_timeout"></a>Evento 4616: NODECLEANUP_DISABLE_CLUSTER_SERVICE_TIMEOUT

La terminación del servicio de clúster durante la limpieza del nodo de clúster no se ha completado dentro del período de tiempo esperado. Reinicie este equipo para asegurarse de que el servicio de clúster ya no se está ejecutando.

### <a name="event-4618-nodecleanup_reset_cluster_registry_entries_failed"></a>Evento 4618: NODECLEANUP_RESET_CLUSTER_REGISTRY_ENTRIES_FAILED

Error al restablecer las entradas del registro del servicio de clúster durante la limpieza del nodo de clúster.
El código de error era ' %1 '. Es posible que no pueda crear o unir un clúster con este equipo hasta que se haya completado correctamente la limpieza. Para la limpieza manual, ejecute el cmdlet de PowerShell ' Clear-ClusterNode ' en este equipo.

### <a name="event-4620-nodecleanup_unload_cluster_hive_failed"></a>Evento 4620: NODECLEANUP_UNLOAD_CLUSTER_HIVE_FAILED

Error al descargar el subárbol del registro del servicio de clúster durante la limpieza del nodo de clúster.
El código de error era ' %1 '. Es posible que no pueda crear o unir un clúster con este equipo hasta que se haya completado correctamente la limpieza. Para la limpieza manual, ejecute el cmdlet de PowerShell ' Clear-ClusterNode ' en este equipo.

### <a name="event-4622-nodecleanup_errors"></a>Evento 4622: NODECLEANUP_ERRORS

El Servicio de clúster encontró un error durante la limpieza del nodo. Es posible que no pueda crear o unir un clúster con este equipo hasta que se haya completado correctamente la limpieza. Use el cmdlet de PowerShell ' Clear-ClusterNode ' en este nodo.

### <a name="event-4624-nodecleanup_reset_nlbsflags_failed"></a>Evento 4624: NODECLEANUP_RESET_NLBSFLAGS_FAILED

No se pudo restablecer el valor del registro de tiempo de espera de la Asociación de seguridad IPSec durante la limpieza del nodo de clúster. El código de error era ' %1 '. Para la limpieza manual, ejecute el cmdlet de PowerShell ' Clear-ClusterNode ' en este equipo. Como alternativa, puede restablecer el tiempo de espera de la Asociación de seguridad de IPSec eliminando el valor ' %2 ' y el valor ' %3 ' de HKEY_LOCAL_MACHINE en el registro de Windows.

### <a name="event-4627-nodecleanup_delete_cluster_tasks_failed"></a>Evento 4627: NODECLEANUP_DELETE_CLUSTER_TASKS_FAILED

No se pudieron eliminar las tareas en clúster durante la limpieza del nodo. El código de error era ' %1 '.
Use Windows Programador de tareas para eliminar cualquier tarea en clúster restante.

### <a name="event-4629-nodecleanup_delete_local_account_failed"></a>Evento 4629: NODECLEANUP_DELETE_LOCAL_ACCOUNT_FAILED

Durante la limpieza del nodo, no se eliminó la cuenta de usuario local administrada por el clúster. El código de error era ' %1 '. Abra usuarios y grupos locales (lusrmgr. msc) para eliminar la cuenta.

### <a name="event-4864-res_vsstask_open_failed"></a>Evento 4864: RES_VSSTASK_OPEN_FAILED

No se pudo crear el recurso de tarea de servicio de instantáneas de volumen ' %1 '. El código de error era ' %2 '.

### <a name="event-4865-res_vsstask_terminate_task_failed"></a>Evento 4865: RES_VSSTASK_TERMINATE_TASK_FAILED

Error del recurso de tarea de servicio de instantáneas de volumen ' %1 '. El código de error era ' %2 '.
Esto se debe a que la tarea asociada no se pudo detener como parte de una operación sin conexión. Es posible que tenga que detenerlo manualmente con el complemento Programador de tareas.

### <a name="event-4866-res_vsstask_delete_task_failed"></a>Evento 4866: RES_VSSTASK_DELETE_TASK_FAILED

Error del recurso de tarea de servicio de instantáneas de volumen ' %1 '. El código de error era ' %2 '.
Esto se debe a que la tarea asociada no se pudo eliminar como parte de una operación sin conexión. Puede que tenga que eliminarlo manualmente con el complemento Programador de tareas.

### <a name="event-4867-res_vsstask_online_failed"></a>Evento 4867: RES_VSSTASK_ONLINE_FAILED

Error del recurso de tarea de servicio de instantáneas de volumen ' %1 '. El código de error era ' %2 '.
Esto se debe a que la tarea asociada no se pudo agregar como parte de una operación en línea. Use el complemento Programador de tareas para asegurarse de que las tareas están configuradas correctamente.

### <a name="event-4868-unable_to_start_autologger"></a>Evento 4868: UNABLE_TO_START_AUTOLOGGER

Servicio de clúster no pudo iniciar la sesión de seguimiento del registro del clúster. El código de error era ' %2 '. El clúster funcionará correctamente, pero falta información de registro complementario. Use el complemento del monitor de rendimiento para comprobar la configuración del canal de eventos para ' %1 '.

### <a name="event-4869-netft_watchdog_process_hung"></a>Evento 4869: NETFT_WATCHDOG_PROCESS_HUNG

La supervisión de estado del modo de usuario ha detectado que el sistema no responde. El adaptador virtual del clúster de conmutación por error perdió el contacto con el proceso ' %1 ' con un ID. de proceso ' %2 ', para ' %3 ' segundos. Use el monitor de rendimiento para evaluar el estado del sistema y determinar qué proceso puede afectar negativamente al sistema.

### <a name="event-4870-netft_watchdog_process_terminated"></a>Evento 4870: NETFT_WATCHDOG_PROCESS_TERMINATED

La supervisión de estado del modo de usuario ha detectado que el sistema no responde. El adaptador virtual del clúster de conmutación por error perdió el contacto con el proceso ' %1 ' con un ID. de proceso ' %2 ', para ' %3 ' segundos. Se realizará la acción de recuperación.

### <a name="event-4871-netft_miniport_initialization_failure"></a>Evento 4871: NETFT_MINIPORT_INITIALIZATION_FAILURE

No se pudo iniciar el servicio de Cluster Server. Esto se debe a que el adaptador virtual del clúster de conmutación por error no pudo inicializar el adaptador de minipuerto. El código de error era ' %1 '. Compruebe que otros adaptadores de red funcionan correctamente y compruebe si hay errores en el administrador de dispositivos. Si se cambió la configuración, puede que sea necesario volver a instalar la característica de clúster de conmutación por error en este equipo.

### <a name="event-4872-netft_missing_datalink_address"></a>Evento 4872: NETFT_MISSING_DATALINK_ADDRESS

El adaptador virtual del clúster de conmutación por error no pudo generar una dirección MAC única.
No se pudo encontrar un adaptador Ethernet físico desde el que generar una dirección única o la dirección generada entra en conflicto con otro adaptador de este equipo. Ejecute el Asistente para validar una configuración para comprobar la configuración de red.

### <a name="event-5122-dcm_event_root_creation_failed"></a>Evento 5122: DCM_EVENT_ROOT_CREATION_FAILED

Servicio de clúster no pudo crear el directorio raíz de volúmenes compartidos de clúster ' %2 '.
El mensaje de error era ' %1 '.

### <a name="event-5142-dcm_volume_no_access"></a>Evento 5142: DCM_VOLUME_NO_ACCESS

El Volumen compartido de clúster ' %1 ' (' %2 ') ya no es accesible desde este nodo de clúster debido al error ' %3 '. Solucione los problemas de conectividad de este nodo con el dispositivo de almacenamiento y la conectividad de red.

### <a name="event-5143-dcm_veto_resource_move_due_to_cc"></a>Evento 5143: DCM_VETO_RESOURCE_MOVE_DUE_TO_CC

El movimiento del disco (' %2 ') se ha vetado en función del estado actual del administrador de caché en el nodo ' %1 ' para evitar un posible interbloqueo. ' El administrador de caché de páginas desfasadas umbral ' es %3 y ' páginas desfasadas del administrador de caché ' es %4. Se permite el traslado si ' páginas desfasadas del administrador de caché ' es inferior al 70% de ' páginas desfasadas del administrador de caché umbral ' o si ' páginas desfasadas del administrador de caché umbral ' menos ' páginas desfasadas del administrador de caché ' es superior a 128000 páginas (aproximadamente 500 MB si un tamaño de página es de 4096 bytes).
Traslado de recursos vetados del clúster para evitar posibles interbloqueos debido a las escrituras almacenadas en búfer del administrador de caché mientras los volúmenes compartidos de clúster de este disco se están pausando.

### <a name="event-5144-dcm_snapshot_diff_area_failure"></a>Evento 5144: DCM_SNAPSHOT_DIFF_AREA_FAILURE

Al agregar el disco (' %1 ') a los volúmenes compartidos de clúster, no se pudo establecer la Asociación de área de diferencias de instantáneas explícitas para el volumen (' %2 ') con el error ' %3 '. La única asociación de área de diferencias de instantánea de software admitida para volúmenes compartidos de clúster es self.

### <a name="event-5145-dcm_snapshot_diff_area_delete_failure"></a>Evento 5145: DCM_SNAPSHOT_DIFF_AREA_DELETE_FAILURE

El recurso de disco de clúster ' %1 ' no pudo eliminar una instantánea de software. No se pudo desasociar el área de diferencia del volumen ' %3 ' del volumen ' %2 '. Esto puede deberse a instantáneas activas. Los volúmenes compartidos de clúster requieren que la instantánea de software se encuentre en el mismo disco.

### <a name="event-5146-dcm_veto_resource_move_due_to_dismount"></a>Evento 5146: DCM_VETO_RESOURCE_MOVE_DUE_TO_DISMOUNT

Se ha vetado el movimiento del recurso de Volumen compartido de clúster ' %1 ' porque uno de los volúmenes que pertenecen al recurso está en estado desmontado. Vuelva a intentar la acción una vez completada la operación de desmontaje.

### <a name="event-5147-dcm_veto_resource_move_due_to_snapshot"></a>Evento 5147: DCM_VETO_RESOURCE_MOVE_DUE_TO_SNAPSHOT

Se ha vetado el movimiento del recurso de Volumen compartido de clúster ' %1 ' porque uno de los volúmenes que pertenecen al recurso está en estado desmontado. Vuelva a intentar la acción una vez completada la operación de desmontaje.

### <a name="event-5148-dcm_veto_resource_move_due_to_io_mode_change"></a>Evento 5148: DCM_VETO_RESOURCE_MOVE_DUE_TO_IO_MODE_CHANGE

El movimiento del recurso de Volumen compartido de clúster ' %1 ' se ha vetado porque una operación de cambio de modo de e/s (e/s directa a e/s redirigida o viceversa) está en curso en uno de los volúmenes que pertenecen al recurso. Vuelva a intentar la acción una vez completada la operación.

### <a name="event-5150-dcm_set_resource_in_failed_state"></a>Evento 5150: DCM_SET_RESOURCE_IN_FAILED_STATE

Error en el recurso de disco físico de clúster ' %1 '. El Volumen compartido de clúster se puso en estado de error con el siguiente error: ' %2 '

### <a name="event-5200-cam_cannot_create_cno_token"></a>Evento 5200: CAM_CANNOT_CREATE_CNO_TOKEN

Servicio de clúster no pudo crear un token de identidad de clúster para los volúmenes compartidos de clúster. El código de error era ' %1 '. Asegúrese de que el controlador de dominio está accesible y compruebe si hay problemas de conectividad. Hasta que se recupere la conexión con el controlador de dominio, es posible que se produzcan errores en algunas operaciones en este nodo en los volúmenes compartidos de clúster.

### <a name="event-5216-csv_sw_snapshot_failed"></a>Evento 5216: CSV_SW_SNAPSHOT_FAILED

Error %3 en la creación de instantáneas de software en el Volumen compartido de clúster ' %1 ' (' %2 '). El recurso debe estar en línea para admitir la creación de instantáneas. Compruebe el estado del recurso.

### <a name="event-5217-csv_sw_snapshot_set_failed"></a>Evento 5217: CSV_SW_SNAPSHOT_SET_FAILED

No se pudo crear la instantánea de software en Volumen compartido de clúster (' %1 ') con ID. de conjunto de instantáneas ' %2 '. error: ' %3 '. Compruebe el estado de los recursos CSV y los eventos del sistema de los nodos del propietario del recurso.

### <a name="event-5219-csv_register_snapshot_prov_with_vss_failed"></a>Evento 5219: CSV_REGISTER_SNAPSHOT_PROV_WITH_VSS_FAILED

Servicio de clúster no pudo registrar el proveedor de instantáneas de volúmenes compartidos de clúster con el servicio de instantáneas de volumen (VSS). Esto puede deberse a que el servicio VSS se está cerrando o puede tratarse de un problema con el servicio VSS que tiene un problema que hace que no acepte las solicitudes entrantes. <br>Error: %1

### <a name="event-5377-operation_exceeded_timeout"></a>Evento 5377: OPERATION_EXCEEDED_TIMEOUT

Una operación de Servicio de clúster interna superó el umbral definido de ' %2 ' segundos. El Servicio de clúster ha finalizado para recuperarse. El administrador de control de servicios reiniciará el Servicio de clúster y el nodo volverá a unirse al clúster.

### <a name="event-5396-two_partitions_have_quorum"></a>Evento 5396: TWO_PARTITIONS_HAVE_QUORUM

El Servicio de clúster en este nodo se está cerrando porque ha detectado que hay otros nodos de clúster que tienen quórum. Esto sucede si el Servicio de clúster detecta otro nodo que se inició con el modificador Force Cuórum (/FQ). El nodo que se inició con el modificador forzar Cuórum seguirá ejecutándose. Use Administrador de clústeres de conmutación por error para comprobar que este nodo se unió automáticamente al clúster cuando se reinició el servicio de clúster.

### <a name="event-5397-rlua_account_failed"></a>Evento 5397: RLUA_ACCOUNT_FAILED

El recurso de clúster ' %1 ' no pudo crear o modificar la cuenta de usuario local replicada ' %2 ' en este nodo. Compruebe los registros de clúster para obtener más información.

### <a name="event-5398-nm_event_cluster_failed_to_form"></a>Evento 5398: NM_EVENT_CLUSTER_FAILED_TO_FORM

No se pudo iniciar el clúster. La copia más reciente de los datos de configuración del clúster no estaba disponible en el conjunto de nodos que intentan iniciar el clúster. Los cambios en el clúster se produjeron mientras el conjunto de nodos no estaba en pertenencia y, como resultado, no podía recibir actualizaciones de datos de configuración. .<br><br>Votos necesarios para iniciar el clúster: %1<br>Votos disponibles: %2<br>Nodos con votos: %3

#### <a name="guidance"></a>Instrucciones

Intente iniciar el servicio de clúster en todos los nodos del clúster para que los nodos con la última copia de los datos de configuración del clúster puedan formar el clúster por primera vez. El clúster se podrá iniciar y los nodos obtendrán automáticamente los datos de configuración del clúster actualizados. Si no hay ningún nodo disponible con la última copia de los datos de configuración del clúster, ejecute el cmdlet de Windows PowerShell "Start-ClusterNode-FQ". El uso del parámetro ForceQuorum (FQ) iniciará el servicio de clúster y marcará la copia de este nodo de los datos de configuración del clúster para que sean autoritativos. Forzar el cuórum en un nodo con una copia obsoleta de la base de datos del clúster puede dar lugar a cambios en la configuración del clúster que se produjeron mientras el nodo no estaba participando en el clúster para ser perdido.

## <a name="warning-events"></a>Eventos de advertencia

### <a name="event-1011-nm_node_evicted"></a>Evento 1011: NM_NODE_EVICTED

El nodo de clúster %1 se ha expulsado del clúster de conmutación por error.

### <a name="event-1045-res_ipaddr_ipv4_address_create_failed"></a>Evento 1045: RES_IPADDR_IPV4_ADDRESS_CREATE_FAILED

No se encontró ninguna interfaz de red coincidente para la dirección IP de recurso ' %1 ' ' %2 ' (el código de retorno era ' %3 '). Si los nodos del clúster abarcan subredes diferentes, puede ser normal.

### <a name="event-1066-res_disk_corrupt_disk"></a>Evento 1066: RES_DISK_CORRUPT_DISK

El recurso de disco de clúster ' %1 ' indica daños en el volumen ' %2 '. CHKDSK se ejecuta para reparar problemas. El disco no estará disponible hasta que se complete CHKDSK.
La salida de CHKDSK se registrará en el archivo ' %3 '.<br> CHKDSK también puede escribir información en el registro de eventos de la aplicación.

### <a name="event-1068-res_smb_share_cant_add"></a>Evento 1068: RES_SMB_SHARE_CANT_ADD

No se puede poner en línea el recurso compartido de archivos de clúster ' %1 '. No se pudo crear el recurso compartido de archivos ' %2 ' (cuyo ámbito es el nombre de red %3) debido al error ' %4 '. Esta operación se volverá a intentar automáticamente.

### <a name="event-1071-rcm_resource_online_blocked_by_locked_mode"></a>Evento 1071: RCM_RESOURCE_ONLINE_BLOCKED_BY_LOCKED_MODE

No se pudo completar la operación intentada en el recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 ' porque el recurso o uno de sus proveedores tiene el estado bloqueado.

### <a name="event-1071-rcm_resource_offline_blocked_by_locked_mode"></a>Evento 1071: RCM_RESOURCE_OFFLINE_BLOCKED_BY_LOCKED_MODE

No se pudo completar la operación intentada en el recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 ' porque el recurso o uno de sus elementos dependientes tiene el estado bloqueado.

### <a name="event-1094-sm_invalid_security_level"></a>Evento 1094: SM_INVALID_SECURITY_LEVEL

No se puede cambiar la propiedad común del clúster SecurityLevel en este clúster. No se puede cambiar el nivel de seguridad del clúster porque el clúster está configurado actualmente para el modo sin autenticación.

### <a name="event-1119-res_netname_dns_registration_missing"></a>Evento 1119: RES_NETNAME_DNS_REGISTRATION_MISSING

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre DNS ' %2 ' en el adaptador ' %4 ' por el siguiente motivo: <br><br>' %3 '

### <a name="event-1125-tm_event_cluster_netinterface_unreachable"></a>Evento 1125: TM_EVENT_CLUSTER_NETINTERFACE_UNREACHABLE

La interfaz de red de clústeres ' %1 ' para el nodo de clúster ' %2 ' en la red ' %3 ' no es accesible para al menos otro nodo de clúster conectado a la red. El clúster de conmutación por error no pudo determinar la ubicación del error. Ejecute el Asistente para validar una configuración para comprobar la configuración de red. Si la condición persiste, compruebe si hay errores de hardware o software relacionados con el adaptador de red. Compruebe también si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1149-res_netname_cant_delete_dns_records"></a>Evento 1149: RES_NETNAME_CANT_DELETE_DNS_RECORDS

Los registros de host DNS (A) y puntero (PTR) asociados con el recurso de clúster ' %1 ' no se quitaron del servidor DNS asociado al recurso. Si es necesario, se pueden eliminar manualmente. Póngase en contacto con el administrador de DNS para que le ayude.

### <a name="event-1150-res_netname_dns_ptr_record_delete_failed"></a>Evento 1150: RES_NETNAME_DNS_PTR_RECORD_DELETE_FAILED

No se pudo quitar el registro de puntero DNS (PTR) ' %2 ' para el host ' %3 ' que está asociado con el recurso de nombre de red de clúster ' %1 '. error: ' %4 '.
Si es necesario, el registro se puede eliminar manualmente. Póngase en contacto con su administrador de DNS para obtener ayuda.

### <a name="event-1151-res_netname_dns_a_record_delete_failed"></a>Evento 1151: RES_NETNAME_DNS_A_RECORD_DELETE_FAILED

No se pudo quitar el registro de host DNS (A) ' %2 ' asociado al recurso de nombre de red de clúster ' %1 '. error: ' %3 '. Si es necesario, el registro se puede eliminar manualmente. Póngase en contacto con su administrador de DNS para obtener ayuda.

### <a name="event-1155-rcm_event_exited_queuing"></a>Evento 1155: RCM_EVENT_EXITED_QUEUING

No se completó el movimiento pendiente para el rol ' %1 '.

### <a name="event-1197-res_netname_delete_disable_failed"></a>Evento 1197: RES_NETNAME_DELETE_DISABLE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo eliminar o deshabilitar su objeto de equipo asociado ' %2 ' durante la eliminación del recurso. El código de error era ' %3 '. <br>Compruebe si el sitio es de solo lectura. Asegúrese de que el objeto de nombre de clúster tenga los permisos adecuados en el objeto ' %2 ' en Active Directory.

### <a name="event-1198-res_netname_delete_vco_guid_failed"></a>Evento 1198: RES_NETNAME_DELETE_VCO_GUID_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo eliminar el objeto de equipo con el GUID ' %2 '. El código de error era ' %3 '. <br>Compruebe si el sitio es de solo lectura. Asegúrese de que el objeto de nombre de clúster tenga los permisos adecuados en el objeto ' %2 ' en Active Directory.

### <a name="event-1216-service_netname_change_warning"></a>Evento 1216: SERVICE_NETNAME_CHANGE_WARNING

Error en una operación de cambio de nombre en el recurso de nombre de dirección principal del clúster.
También se produjo un error al intentar revertir la operación de cambio de nombre al nombre original. El código de error era ' %1 '. Es posible que no pueda administrar de forma remota el clúster con el nombre del clúster hasta que esta situación se haya corregido manualmente.

### <a name="event-1221-res_netname_rename_out_of_synch_with_compobj"></a>Evento 1221: RES_NETNAME_RENAME_OUT_OF_SYNCH_WITH_COMPOBJ

El recurso de nombre de red de clúster ' %1 ' tiene un nombre ' %2 ' que no coincide con el nombre de objeto de equipo correspondiente ' %3 '. Es probable que un cambio de nombre anterior del objeto de equipo no se haya replicado en todos los controladores de dominio del dominio. No podrá cambiar el nombre del recurso de nombre de red hasta que los nombres sean coherentes. Si el objeto de equipo no se ha modificado recientemente, póngase en contacto con el administrador del dominio para cambiar el nombre del objeto de equipo y, por tanto, hacerlo coherente. Además, asegúrese de que la replicación a través de los controladores de dominio se ha completado correctamente.

### <a name="event-1222-res_netname_set_permissions_failed"></a>Evento 1222: RES_NETNAME_SET_PERMISSIONS_FAILED

No se pudo actualizar el objeto de equipo asociado al recurso de nombre de red de clúster ' %1 '.<br><br>El texto del código de error asociado es: %2<br> <br>Es posible que la identidad del clúster ' %3 ' carezca de los permisos necesarios para actualizar el objeto. Trabaje con el administrador del dominio para asegurarse de que la identidad del clúster puede actualizar objetos de equipo en el dominio.

### <a name="event-1240-res_ipaddr_obtain_lease_failed"></a>Evento 1240: RES_IPADDR_OBTAIN_LEASE_FAILED

El recurso de dirección IP del clúster ' %1 ' no pudo obtener una dirección IP concedida.

### <a name="event-1243-res_ipaddr_release_lease_failed"></a>Evento 1243: RES_IPADDR_RELEASE_LEASE_FAILED

El recurso de dirección IP del clúster ' %1 ' no pudo liberar la concesión de la dirección IP ' %2 '.

### <a name="event-1251-rcm_group_preempted"></a>Evento 1251: RCM_GROUP_PREEMPTED

El rol en clúster ' %2 ' se ha desconectado. El rol en clúster de mayor prioridad ' %1 ' ha adelantado a este rol. El servicio de clúster intentará poner en línea el rol en clúster ' %2 ' cuando haya recursos del sistema disponibles.

### <a name="event-1544-service_vss_onabort"></a>Evento 1544: SERVICE_VSS_ONABORT

Se ha cancelado la operación de copia de seguridad de los datos de configuración del clúster. El escritor de Servicio de instantáneas de volumen de clúster (VSS) recibió una solicitud de anulación.

### <a name="event-1548-service_connect_version_compatible"></a>Evento 1548: SERVICE_CONNECT_VERSION_COMPATIBLE

El nodo ' %1 ' estableció comunicación con el nodo ' %2 ' y detectó que está ejecutando una versión diferente, pero compatible, del sistema operativo. Se recomienda que todos los nodos ejecuten la misma versión del sistema operativo. Una vez actualizados todos los nodos, ejecute el cmdlet Update-ClusterFunctionalLevel de Windows PowerShell para completar la actualización del clúster.

### <a name="event-1550-service_connect_novercheck"></a>Evento 1550: SERVICE_CONNECT_NOVERCHECK

El nodo ' %1 ' estableció una sesión de comunicación con el nodo ' %2 ' sin realizar una comprobación de compatibilidad de versiones porque la comprobación de compatibilidad de la versión está deshabilitada. No se admite la deshabilitación de la comprobación de compatibilidad de versiones.

### <a name="event-1551-service_form_novercheck"></a>Evento 1551: SERVICE_FORM_NOVERCHECK

El nodo ' %1 ' formó un clúster de conmutación por error sin realizar una comprobación de compatibilidad de versiones porque la comprobación de compatibilidad de la versión está deshabilitada. No se admite la deshabilitación de la comprobación de compatibilidad de versiones.

### <a name="event-1555-service_netft_disable_autoconfig_failed"></a>Evento 1555: SERVICE_NETFT_DISABLE_AUTOCONFIG_FAILED

Error al intentar usar IPv4 para el adaptador de red ' %1 '. Esto se debe a un error al deshabilitar la configuración automática de IPv4 y DHCP. Se puede esperar si el servicio cliente DHCP ya está deshabilitado. IPv6 se usará si está habilitado; de lo contrario, es posible que este nodo no pueda participar en un clúster de conmutación por error.

### <a name="event-1558-service_witness_failover_attempt"></a>Evento 1558: SERVICE_WITNESS_FAILOVER_ATTEMPT

El servicio de clúster ha detectado un problema con el recurso de testigo. El recurso de testigo se conmutará por error a otro nodo del clúster al intentar restablecer el acceso a los datos de configuración del clúster.

### <a name="event-1562-res_fsw_alivefailure"></a>Evento 1562: RES_FSW_ALIVEFAILURE

Error en el recurso del testigo del recurso compartido de archivos ' %1 ' en una comprobación de mantenimiento periódica en el recurso compartido de archivos ' %2 '. Asegúrese de que el recurso compartido de archivos ' %2 ' existe y es accesible para el clúster.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Evento 1576: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre ' %2 ' en el adaptador ' %4 ' en una zona DNS segura. Esto se debe a un registro existente con este nombre y la identidad del clúster no tiene los privilegios suficientes para actualizar ese registro. El código de error era ' %3 '. Póngase en contacto con el administrador del servidor DNS para comprobar que la identidad del clúster tiene permisos en el registro DNS ' %2 '.

### <a name="event-1576-res_netname_dns_registration_secure_zone_refused"></a>Evento 1576: RES_NETNAME_DNS_REGISTRATION_SECURE_ZONE_REFUSED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre ' %2 ' en el adaptador ' %4 ' en una zona DNS segura. Esto se debe a un registro existente con este nombre y la identidad del clúster no tiene los privilegios suficientes para actualizar ese registro. El código de error era ' %3 '. Póngase en contacto con el administrador del servidor DNS para comprobar que la identidad del clúster tiene permisos en el registro DNS ' %2 '.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Evento 1577: RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre ' %2 ' en el adaptador ' %4 '. No se pudo establecer contacto con el servidor DNS. El código de error era ' %3. ' Asegúrese de que se puede tener acceso a un servidor DNS desde este nodo de clúster. El registro de DNS se volverá a intentar más tarde.

### <a name="event-1577-res_netname_dns_server_could_not_be_contacted"></a>Evento 1577: RES_NETNAME_DNS_SERVER_COULD_NOT_BE_CONTACTED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar el nombre ' %2 ' en el adaptador ' %4 '. No se pudo establecer contacto con el servidor DNS. El código de error era ' %3. ' Asegúrese de que se puede tener acceso a un servidor DNS desde este nodo de clúster. El registro de DNS se volverá a intentar más tarde.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Evento 1578: RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar las actualizaciones dinámicas para el nombre ' %2 ' en el adaptador ' %4 '. Es posible que el servidor DNS no esté configurado para aceptar actualizaciones dinámicas. El código de error era ' %3 '. Póngase en contacto con el administrador del servidor DNS para comprobar que el servidor DNS está disponible y configurado para actualizaciones dinámicas.<br><br>Como alternativa, puede deshabilitar las actualizaciones dinámicas de DNS desactivando la opción ' registrar las direcciones de esta conexión en DNS ' en la configuración avanzada de TCP/IP para el adaptador ' %4 ' en la pestaña DNS.

### <a name="event-1578-res_netname_dns_test_for_dynamic_update_failed"></a>Evento 1578: RES_NETNAME_DNS_TEST_FOR_DYNAMIC_UPDATE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo registrar las actualizaciones dinámicas para el nombre ' %2 ' en el adaptador ' %4 '. Es posible que el servidor DNS no esté configurado para aceptar actualizaciones dinámicas. El código de error era ' %3 '. Póngase en contacto con el administrador del servidor DNS para comprobar que el servidor DNS está disponible y configurado para actualizaciones dinámicas.<br><br>Como alternativa, puede deshabilitar las actualizaciones dinámicas de DNS desactivando la opción ' registrar las direcciones de esta conexión en DNS ' en la configuración avanzada de TCP/IP para el adaptador ' %4 ' en la pestaña DNS.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Evento 1579: RES_NETNAME_DNS_RECORD_UPDATE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo actualizar el registro DNS para el nombre ' %2 ' en el adaptador ' %4 '. El código de error era ' %3 '. Asegúrese de que se puede obtener acceso a un servidor DNS desde este nodo de clúster y póngase en contacto con el administrador del servidor DNS para comprobar que la identidad del clúster puede actualizar el registro DNS ' %2 '.

### <a name="event-1579-res_netname_dns_record_update_failed"></a>Evento 1579: RES_NETNAME_DNS_RECORD_UPDATE_FAILED

El recurso de nombre de red de clúster ' %1 ' no pudo actualizar el registro DNS para el nombre ' %2 ' en el adaptador ' %4 '. El código de error era ' %3 '. Asegúrese de que se puede obtener acceso a un servidor DNS desde este nodo de clúster y póngase en contacto con el administrador del servidor DNS para comprobar que la identidad del clúster puede actualizar el registro DNS ' %2 '.

### <a name="event-1581-clussvc_unable_to_move_hive_to_safe_file"></a>Evento 1581: CLUSSVC_UNABLE_TO_MOVE_HIVE_TO_SAFE_FILE

La solicitud de restauración de los datos de configuración del clúster no pudo realizar una copia del archivo de datos de configuración del clúster existente (ClusDB). Al intentar conservar la configuración existente, la operación de restauración no pudo crear una copia en la ubicación ' %1 '. Esto puede ocurrir si el archivo de datos de configuración existente estaba dañado. La operación de restauración ha continuado, pero es posible que los intentos de revertir a la configuración de clúster existente no sean posibles.

### <a name="event-1582-clussvc_unable_to_move_restored_hive_to_current"></a>Evento 1582: CLUSSVC_UNABLE_TO_MOVE_RESTORED_HIVE_TO_CURRENT

El servicio de Cluster Server no pudo trasladar el subárbol de clúster restaurado de ' %1 ' a ' %2 '. Esto puede impedir que la operación de restauración se realice correctamente. Si la configuración del clúster no se restauró correctamente, vuelva a intentar la acción de restauración.

### <a name="event-1583-clussvc_netft_disable_connectionsecurity_failed"></a>Evento 1583: CLUSSVC_NETFT_DISABLE_CONNECTIONSECURITY_FAILED

Servicio de clúster no pudo deshabilitar el protocolo de seguridad de Internet (IPsec) en el adaptador virtual del clúster de conmutación por error ' %1 '. Esto podría tener un impacto negativo en el rendimiento de la comunicación del clúster. Si el problema persiste, compruebe las directivas de seguridad de conexión local y de dominio que se aplican a IPSec y al firewall de Windows. Además, compruebe los eventos relacionados con el servicio del motor de filtrado de base.

### <a name="event-1584-shared_volume_not_ready_for_snapshot"></a>Evento 1584: SHARED_VOLUME_NOT_READY_FOR_SNAPSHOT

Una aplicación de copia de seguridad inició una instantánea de VSS en Volumen compartido de clúster ' %1 ' (' %3 ') sin preparar correctamente el volumen para la instantánea. Es posible que esta instantánea no sea válida y que la copia de seguridad no se pueda usar para las operaciones de restauración. Póngase en contacto con el proveedor de la aplicación de copia de seguridad para comprobar la compatibilidad con volúmenes compartidos de clúster.

### <a name="event-1589-res_netname_dns_returning_ip_that_is_not_provider"></a>Evento 1589: RES_NETNAME_DNS_RETURNING_IP_THAT_IS_NOT_PROVIDER

El recurso de nombre de red de clúster ' %1 ' encontró una o varias direcciones IP asociadas con el nombre DNS ' %2 ' que no son recursos de dirección IP dependientes. Las direcciones adicionales encontradas eran ' %3 '. Esto puede afectar a la conectividad de cliente hasta que el nombre de red y sus registros DNS asociados sean coherentes. Póngase en contacto con el administrador del servidor DNS para comprobar los registros asociados con el nombre ' %2 '.

### <a name="event-1604-res_disk_chkdsk_spotfix_needed"></a>Evento 1604: RES_DISK_CHKDSK_SPOTFIX_NEEDED

El recurso de disco de clúster ' %1 ' detectó daños en el volumen ' %2 '. Spotfix CHKDSK es necesario para reparar problemas.

### <a name="event-1605-res_disk_spotfix_performed"></a>Evento 1605: RES_DISK_SPOTFIX_PERFORMED

Se completó la ejecución del recurso de disco de clúster ' %1 ' ChkDsk.exe/spotfix x en el volumen ' %2 '.
El código de retorno era ' %4 '. La salida de ChkDsk se ha registrado en el archivo ' %3 '.<br>
Compruebe el registro de eventos de aplicación para obtener información adicional de ChkDsk.

### <a name="event-1671-res_disk_online_set_attributes_completed_failure"></a>Evento 1671: RES_DISK_ONLINE_SET_ATTRIBUTES_COMPLETED_FAILURE

El recurso de disco físico de clúster no se puede poner en línea.<br><br>Nombre de recurso de disco físico: %1<br>Código de error: %2<br>Tiempo transcurrido (segundos): %3

#### <a name="guidance"></a>Instrucciones

Ejecute el Asistente para validar una configuración para comprobar la configuración del almacenamiento. Si el código de error se ERROR_CLUSTER_SHUTDOWN, el administrador ha cancelado el estado en línea pendiente. Si se trata de un volumen replicado, esto podría ser el resultado de un error al establecer los atributos del disco. Revise los eventos de replicación de almacenamiento para obtener información adicional.

### <a name="event-1673-cluster_node_entered_isolated_state"></a>Evento 1673: CLUSTER_NODE_ENTERED_ISOLATED_STATE

El nodo de clúster ' %1 ' ha entrado en el estado aislado.

### <a name="event-1675-event_joining_node_quarantined"></a>Evento 1675: EVENT_JOINING_NODE_QUARANTINED

El nodo de clúster ' %1 ' se ha puesto en cuarentena por ' %2 ' y no puede unirse al clúster. El nodo se pondrá en cuarentena hasta ' %3 ' y, a continuación, el nodo intentará volver a unirse al clúster automáticamente. <br><br>Consulte los registros de eventos del sistema y de la aplicación para determinar los problemas de este nodo. Cuando se resuelve el problema, se puede borrar manualmente la cuarentena para permitir que el nodo se vuelva a unir con el cmdlet de Windows PowerShell "Start-ClusterNode – ClearQuarantine".<br><br>Nombre del nodo: %1<br>QuarantineType: cuarentena por %2<br>Hora en que se borrará automáticamente la cuarentena: %3

### <a name="event-1681-cluster_groups_unmonitored_on_node"></a>Evento 1681: CLUSTER_GROUPS_UNMONITORED_ON_NODE

Las máquinas virtuales del nodo ' %1 ' han entrado en un estado no supervisado. El estado de las máquinas virtuales se supervisará de nuevo cuando el nodo vuelva de un estado aislado o puede conmutar por error si el nodo no devuelve. La máquina virtual que ya no se está supervisando es: %2.

### <a name="event-1689-event_disable_and_stop_other_service"></a>Evento 1689: EVENT_DISABLE_AND_STOP_OTHER_SERVICE

El servicio de Cluster Server detectó un servicio que no es compatible con los clústeres de conmutación por error. El servicio se ha deshabilitado para evitar posibles problemas con el clúster de conmutación por error.<br><br>Nombre de servicio: ' %1 '

### <a name="event-4625-nodecleanup_reset_nlbsflags_preserved"></a>Evento 4625: NODECLEANUP_RESET_NLBSFLAGS_PRESERVED

No se pudo restablecer el valor del registro de tiempo de espera de la Asociación de seguridad IPSec durante la limpieza del nodo de clúster. Esto se debe a que el tiempo de espera de la Asociación de seguridad de IPSec se modificó después de configurar el equipo para que sea miembro de un clúster. Para la limpieza manual, ejecute el cmdlet de PowerShell ' Clear-ClusterNode ' en este equipo. Como alternativa, puede restablecer el tiempo de espera de la Asociación de seguridad de IPSec eliminando el valor ' %1 ' y el valor ' %2 ' de HKEY_LOCAL_MACHINE en el registro de Windows.

### <a name="event-4873-netft_clussvc_terminate_because_of_watchdog"></a>Evento 4873: NETFT_CLUSSVC_TERMINATE_BECAUSE_OF_WATCHDOG

El servicio de clúster ha detectado que el adaptador virtual del clúster de conmutación por error se ha detenido. Este es el resultado esperado cuando se realiza una CPU de reemplazo en caliente en este nodo.
Servicio de clúster se detendrá y se reiniciará automáticamente una vez completada la operación. Compruebe si hay eventos adicionales asociados con el servicio y asegúrese de que el servicio de clúster se ha reiniciado en este nodo.

### <a name="event-5120-dcm_volume_auto_pause_after_failure"></a>Evento 5120: DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE

Volumen compartido de clúster ' %1 ' (' %2 ') entró en pausa debido a ' %3 '.
Todas las e/s se pondrán en cola temporalmente hasta que se restablezca una ruta de acceso al volumen.

### <a name="event-5123-dcm_event_root_rename_success"></a>Evento 5123: DCM_EVENT_ROOT_RENAME_SUCCESS

El directorio raíz de volúmenes compartidos de clúster ' %1 ' ya existe. Se ha cambiado el nombre del directorio ' %1 ' a ' %2 '. Compruebe que las aplicaciones que tienen acceso a los datos de esta ubicación se han actualizado según sea necesario.

### <a name="event-5124-dcm_unsafe_filters_found"></a>Evento 5124: DCM_UNSAFE_FILTERS_FOUND

Volumen compartido de clúster ' %1 ' (' %3 ') identificó uno o varios controladores de filtro activos en esta pila de dispositivos que podrían interferir con las operaciones de CSV. El acceso de e/s se redirigirá al dispositivo de almacenamiento a través de la red a través de otro nodo de clúster. Esto puede dar lugar a un rendimiento degradado. Póngase en contacto con el proveedor del controlador de filtro para comprobar la interoperabilidad con volúmenes compartidos de clúster. <br><br>Controladores de filtro activos encontrados:<br>%2

### <a name="event-5125-dcm_unsafe_volfilter_found"></a>Evento 5125: DCM_UNSAFE_VOLFILTER_FOUND

Volumen compartido de clúster ' %1 ' (' %3 ') identificó uno o varios controladores de volumen activos en esta pila de dispositivos que podrían interferir con las operaciones de CSV. El acceso de e/s se redirigirá al dispositivo de almacenamiento a través de la red a través de otro nodo de clúster. Esto puede dar lugar a un rendimiento degradado. Póngase en contacto con el proveedor del controlador de volumen para comprobar la interoperabilidad con volúmenes compartidos de clúster. <br><br>Se encontraron controladores de volumen activos:<br>%2

### <a name="event-5126-dcm_event_cannot_disable_short_names"></a>Evento 5126: DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

El recurso de disco físico ' %1 ' no permite deshabilitar la generación de nombres cortos. Esto puede producir problemas de compatibilidad de aplicaciones. Use ' fsutil 8dot3name Set 2 ' para permitir la deshabilitación de la generación de nombres cortos y después sin conexión o en línea del recurso.

### <a name="event-5128-dcm_event_cannot_disable_short_names"></a>Evento 5128: DCM_EVENT_CANNOT_DISABLE_SHORT_NAMES

El recurso de disco físico ' %1 ' no permite deshabilitar la generación de nombres cortos. Esto puede producir problemas de compatibilidad de aplicaciones. Use ' fsutil 8dot3name Set 2 ' para permitir la deshabilitación de la generación de nombres cortos y después sin conexión o en línea del recurso.

### <a name="event-5133-dcm_cannot_restore_drive_letters"></a>Evento 5133: DCM_CANNOT_RESTORE_DRIVE_LETTERS

El disco de clúster ' %1 ' se ha quitado y se ha vuelto a colocar en el grupo de clúster ' almacenamiento disponible '. Durante este proceso, un intento de restaurar las letras de unidad originales ha tardado más de lo esperado, posiblemente debido a que las letras de unidad ya están en uso.

### <a name="event-5134-dcm_cannot_set_acl_on_root"></a>Evento 5134: DCM_CANNOT_SET_ACL_ON_ROOT

Servicio de clúster no pudo establecer los permisos (ACL) en el directorio raíz de volúmenes compartidos de clúster ' %1 '. Error ' %2 '.

### <a name="event-5135-dcm_cannot_set_acl_on_volume_folder"></a>Evento 5135: DCM_CANNOT_SET_ACL_ON_VOLUME_FOLDER

Servicio de clúster no pudo establecer los permisos en el directorio Volumen compartido de clúster ' %1 ' (' %2 '). Error ' %3 '.

### <a name="event-5136-dcm_csv_into_redirected_mode"></a>Evento 5136: DCM_CSV_INTO_REDIRECTED_MODE

Volumen compartido de clúster se activó el acceso Redirigido de ' %1 ' (' %2 '). El acceso al dispositivo de almacenamiento se redirigirá a través de la red desde todos los nodos de clúster que tengan acceso a este volumen. Esto puede dar lugar a un rendimiento degradado. Desactive el acceso redirigido para este volumen para reanudar las operaciones normales.

### <a name="event-5149-dcm_csv_block_cache_resized"></a>Evento 5149: DCM_CSV_BLOCK_CACHE_RESIZED

Se ha cambiado el tamaño de la memoria caché a ' %1 ' en función de la memoria rellenada; el usuario se ha definido como demasiado alto.

### <a name="event-5156-dcm_volume_auto_pause_after_snapshot_failure"></a>Evento 5156: DCM_VOLUME_AUTO_PAUSE_AFTER_SNAPSHOT_FAILURE

Volumen compartido de clúster ' %1 ' (' %2 ') entró en pausa debido a ' %3 '.
Este error se produce cuando se eliminan las instantáneas de volsnap subyacentes al volumen CSV fuera de una solicitud de usuario. Las posibles causas de las instantáneas que se eliminan son la falta de espacio en el volumen para que las instantáneas crezcan, o un error de e/s al intentar actualizar los datos de la instantánea. Todas las e/s se pondrán en cola temporalmente hasta que el estado de la instantánea esté sincronizado con volsnap.

### <a name="event-5157-dcm_volume_auto_pause_after_failure_expected"></a>Evento 5157: DCM_VOLUME_AUTO_PAUSE_AFTER_FAILURE_EXPECTED

Volumen compartido de clúster ' %1 ' (' %2 ') entró en pausa debido a ' %3 '.
Todas las e/s se pondrán en cola temporalmente hasta que se restablezca una ruta de acceso al volumen.
Este error se debe normalmente a un error de infraestructura. Por ejemplo, la pérdida de conectividad con el almacenamiento o el nodo propietario de la Volumen compartido de clúster que se va a quitar de la pertenencia al clúster activa.

### <a name="event-5394-pool_online_warnings"></a>Evento 5394: POOL_ONLINE_WARNINGS

El Servicio de clúster encontró algunos errores de almacenamiento al intentar poner en línea el bloque de almacenamiento. Ejecute la validación de almacenamiento del clúster para solucionar el problema.

### <a name="event-5395-rcm_group_auto_move_storage_pool"></a>Evento 5395: RCM_GROUP_AUTO_MOVE_STORAGE_POOL

El clúster está moviendo el grupo para el grupo de almacenamiento ' %1 ' porque el nodo actual ' %3 ' no tiene una conectividad óptima con los discos físicos del bloque de almacenamiento.

## <a name="informational-events"></a>Eventos informativos

### <a name="event-1592-clussvc_tcp_reconnect_connection_reestablished"></a>Evento 1592: CLUSSVC_TCP_RECONNECT_CONNECTION_REESTABLISHED

El nodo de clúster ' %1 ' perdió la comunicación con el nodo de clúster ' %2 '. Se ha restablecido la comunicación de red. Esto puede deberse a que la comunicación está bloqueada temporalmente por un firewall o una actualización de la Directiva de seguridad de conexión. Si el problema persiste y no se restablece la comunicación de red, el servicio de clúster en uno o varios nodos se detendrá. Si esto ocurre, ejecute el Asistente para validar una configuración para comprobar la configuración de red. Además, compruebe si hay errores de hardware o software relacionados con los adaptadores de red de este nodo y compruebe si hay errores en otros componentes de red a los que está conectado el nodo, como concentradores, conmutadores o puentes.

### <a name="event-1594-cluster_functional_level_upgrade_complete"></a>Evento 1594: CLUSTER_FUNCTIONAL_LEVEL_UPGRADE_COMPLETE

Servicio de clúster actualizó correctamente el nivel funcional del clúster. <br><br>
Nivel funcional: %1 <br> Versión de actualización: %2

### <a name="event-1635-rcm_resource_failure_info_with_typename"></a>Evento 1635: RCM_RESOURCE_FAILURE_INFO_WITH_TYPENAME

Error del recurso de clúster ' %1 ' del tipo ' %3 ' en el rol en clúster ' %2 '.

### <a name="event-1636-clussvc_password_changed"></a>Evento 1636: CLUSSVC_PASSWORD_CHANGED

El Servicio de clúster ha cambiado la contraseña de la cuenta ' %1 ' en el nodo ' %2 '.

### <a name="event-1680-cluster_node_exited_isolated_state"></a>Evento 1680: CLUSTER_NODE_EXITED_ISOLATED_STATE

El nodo de clúster ' %1 ' ha salido del estado aislado.

### <a name="event-1682-cluster_group_moved_no_longer_unmonitored"></a>Evento 1682: CLUSTER_GROUP_MOVED_NO_LONGER_UNMONITORED

Se ha realizado una conmutación por error de la máquina virtual ' %2 ', que no se ha supervisado en el nodo aislado ' %1 ', a otro nodo. El estado de la máquina virtual se está supervisando de nuevo.

### <a name="event-1682-cluster_multiple_groups_no_longer_unmonitored"></a>Evento 1682: CLUSTER_MULTIPLE_GROUPS_NO_LONGER_UNMONITORED

El nodo ' %1 ' se ha vuelto a unir al clúster y las siguientes máquinas virtuales que se ejecutan en ese nodo vuelven a tener su estado de mantenimiento supervisado: %2.

### <a name="event-4621-nodecleanup_success"></a>Evento 4621: NODECLEANUP_SUCCESS

Este nodo se quitó correctamente del clúster.

### <a name="event-5121-dcm_volume_no_direct_io_due_to_failure"></a>Evento 5121: DCM_VOLUME_NO_DIRECT_IO_DUE_TO_FAILURE

Volumen compartido de clúster ' %1 ' (' %2 ') ya no es accesible directamente desde este nodo de clúster. El acceso de e/s se redirigirá al dispositivo de almacenamiento a través de la red al nodo que posee el volumen. Si esto provoca un rendimiento degradado, solucione los problemas de conectividad de este nodo con el dispositivo de almacenamiento y la e/s se reanudará a un estado correcto una vez que se restablezca la conectividad con el dispositivo de almacenamiento.

### <a name="event-5218-csv_old_sw_snapshot_deleted"></a>Evento 5218: CSV_OLD_SW_SNAPSHOT_DELETED

El recurso de disco físico de clúster ' %1 ' eliminó una instantánea de software. La instantánea de software del Volumen compartido de clúster ' %2 ' se eliminó porque era anterior a ' %3 ' días. El ID. de instantánea era ' %4 ' y se creó a partir del nodo ' %5 ' en ' %6 '.
Se espera que una aplicación de copia de seguridad elimine las instantáneas una vez completado un trabajo de copia de seguridad. Esta instantánea superó el tiempo que se espera para que exista una instantánea. Compruebe con la aplicación de copia de seguridad que los trabajos de copia de seguridad se están completando correctamente.

## <a name="additional-references"></a>Referencias adicionales

-   [Información detallada sobre los eventos de los componentes de clústeres de conmutación por error en Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753362(v%3dws.10))
