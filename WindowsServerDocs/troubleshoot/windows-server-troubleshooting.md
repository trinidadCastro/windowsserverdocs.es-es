---
title: Solución de problemas de Windows Server
description: Obtener vínculos a los artículos de solución de problemas de Windows Server
ms.date: 1/24/2020
author: kaushika-msft
ms.author: kaushika
ms.topic: troubleshooting
ms.openlocfilehash: d438f8c2a0e17e13e01c198a55c7dfd3f234da14
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949661"
---
# <a name="troubleshooting-windows-server-components"></a>Solucionar problemas de componentes de Windows Server

> [!TIP]
> ¿Busca información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com.
>
> También puede [buscar en este sitio](/search/index?dataSource=previousVersions&search=Windows+Server) para obtener información específica.

Microsoft publica regularmente ambas actualizaciones para Windows Server. Para asegurarse de que los servidores pueden recibir actualizaciones futuras, incluidas las actualizaciones de seguridad, es importante mantener actualizados los servidores. Consulte el historial de actualizaciones de [Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history) para obtener una lista completa de las actualizaciones publicadas.

Esta sección contiene temas de solución de problemas avanzados y vínculos para ayudarle a resolver problemas con Windows Server. Se agregarán temas adicionales a medida que estén disponibles.

## <a name="troubleshoot-activation"></a>Solucionar problemas de activación

- [Solución de problemas de activación del volumen de Windows](../get-started/activation-troubleshooting-guide.md)
- [Instrucciones para solucionar problemas de KMS](../get-started/activation-troubleshoot-kms-general.md)
- [Opciones de Slmgr.vbs para obtener información de activación de volúmenes](../get-started/activation-slmgr-vbs-options.md)
- [Resolver códigos de error de activación de Windows](../get-started/activation-error-codes.md)
- [Problemas conocidos de activación de KMS](../get-started/activation-troubleshoot-kms-issues.md)
- [Problemas conocidos de la activación de MAK](../get-started/activation-troubleshoot-mak-issues.md)
- [Instrucciones para solucionar problemas de activación relacionados con DNS](../get-started/common-troubleshooting-procedures-kms-dns.md)
- [Volver a generar el archivo Tokens.dat](../get-started/activation-rebuild-tokens-dat-file.md)
- [Solución de problemas de clientes de ADBA](../get-started/activation-troubleshoot-adba-clients.md)

## <a name="troubleshoot-startup-and-restart"></a>Solucionar problemas de inicio y reinicio

- [Solución avanzada de problemas de inicio de Windows](/windows/client-management/troubleshoot-windows-startup)
- [Determinar el tamaño apropiado del archivo de paginación para las versiones de 64 bits de Windows](/windows/client-management/determine-appropriate-page-file-size)
- [Generar un volcado de memoria de kernel o completo](/windows/client-management/generate-kernel-or-complete-crash-dump)
- [Introducción al archivo de paginación](/windows/client-management/introduction-page-file)
- [Configurar las opciones de recuperación y errores del sistema en Windows](/windows/client-management/system-failure-recovery-options)
- [Solución avanzada de problemas de arranque de Windows](/windows/client-management/advanced-troubleshooting-boot-problems)
- [Solución avanzada de problemas de inmovilización de equipos basados en Windows](/windows/client-management/troubleshoot-windows-freeze)
- [Solución avanzada de problemas de error de detención o de pantalla azul](/windows/client-management/troubleshoot-stop-errors)
- [Solución avanzada de problemas para detener el error 7B o Inaccessible_Boot_Device](/windows/client-management/troubleshoot-inaccessible-boot-device)
- [Solución avanzada de problemas para el ID. de evento 41 "el sistema se ha reiniciado sin cerrarse primero correctamente"](/windows/client-management/troubleshoot-event-id-41-restart)
- [Se produce un error de detención al actualizar el controlador del adaptador de red de Broadcom en la caja](/windows/client-management/troubleshoot-stop-error-on-broadcom-driver-update)

## <a name="troubleshoot-ad-forest-recovery"></a>Solucionar problemas de la recuperación del bosque de AD

- [Recuperación del bosque de AD: preguntas frecuentes](../identity/ad-ds/manage/ad-forest-recovery-faq.md)

## <a name="troubleshoot-ad-replication"></a>Solución de problemas de replicación de AD

- [Solución de problemas de replicación de Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Solucionar problemas de controladores de dominio virtualizados](../identity/ad-ds/manage/virtual-dc/virtualized-domain-controller-troubleshooting.md)
- [Solución de problemas de implementación de controladores de dominio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Configuración de un equipo para la solución de problemas](../identity/ad-ds/manage/troubleshoot/configuring-a-computer-for-troubleshooting.md)

## <a name="troubleshoot-ad-fs"></a>Solucionar problemas AD FS

- [Solución de problemas de AD FS](../identity/ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
- [AD FS solución de problemas: auditoría de eventos y registro](../identity/ad-fs/troubleshooting/ad-fs-tshoot-logging.md)
- [Solución de problemas de AD FS-conectividad de SQL](../identity/ad-fs/troubleshooting/ad-fs-tshoot-sql.md)
- [Solución de problemas de AD FS: emisión de notificaciones](../identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-issuance.md)
- [Solución de problemas de AD FS: detección de bucles](../identity/ad-fs/troubleshooting/ad-fs-tshoot-loop.md)
- [Solución de problemas de AD FS: certificados](../identity/ad-fs/troubleshooting/ad-fs-tshoot-certs.md)
- [Solución de problemas de AD FS: Fiddler](../identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler.md)
- [Solución de problemas de AD FS-Fiddler-WS-Federation](../identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler-ws-fed.md)
- [Solución de problemas de AD FS: reglas de notificaciones](../identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-rules.md)
- [Solución de problemas de AD FS la autenticación integrada de Windows](../identity/ad-fs/troubleshooting/ad-fs-tshoot-iwa.md)
- [Solución de problemas de AD FS-Azure AD](../identity/ad-fs/troubleshooting/ad-fs-tshoot-azure.md)
- [Preguntas más frecuentes sobre AD FS](../identity/ad-fs/overview/ad-fs-faq.md)
- [Analizador de diagnósticos de la Ayuda de AD FS](../identity/ad-fs/troubleshooting/ad-fs-diagnostics-analyzer.md)

## <a name="troubleshoot-aovpn"></a>Solución de problemas de AoVPN

- [Solucionar problemas de VPN de Always On](../remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy-troubleshooting.md)

## <a name="troubleshoot-converged-nic"></a>Solucionar problemas de la NIC convergente

- [Solución de problemas de configuraciones de NIC convergentes](../networking/technologies/conv-nic/cnic-app-troubleshoot.md)

## <a name="troubleshoot-dfsr"></a>Solucionar problemas de DFSR

- [Replicación DFS: Preguntas más frecuentes (P+F)](../storage/dfs-replication/dfsr-faq.md)

## <a name="troubleshoot-directaccess"></a>solución de problemas de DirectAccess

- [Solución de problemas de DirectAccess](../remote/remote-access/directaccess/troubleshooting-directaccess.md)

## <a name="troubleshoot-disk-management"></a>Solución de problemas de administración de discos

- [Solución de problemas de Administración de discos](../storage/disk-management/troubleshooting-disk-management.md)

## <a name="troubleshoot-dns"></a>Solucionar problemas de DNS

- [Solución de problemas del sistema de nombres de dominio (DNS)](../networking/dns/troubleshoot/troubleshoot-dns-data-collection.md)
- [Solución de problemas de clientes DNS](../networking/dns/troubleshoot/troubleshoot-dns-client.md)
- [Deshabilitar el almacenamiento en caché del lado cliente DNS en clientes DNS](../networking/dns/troubleshoot/disable-dns-client-side-caching.md)
- [Solución de problemas de servidores DNS](../networking/dns/troubleshoot/troubleshoot-dns-server.md)

## <a name="troubleshoot-failover-cluster"></a>Solucionar problemas del clúster de conmutación por error

- [Solución de problemas de clúster de conmutación por error con el Informe de errores de Windows](../failover-clustering/troubleshooting-using-wer-reports.md)
- [Actualización compatible con clústeres: preguntas más frecuentes](../failover-clustering/cluster-aware-updating-faq.md)
- [Solución de problemas de clúster con el id. de evento 1135](./troubleshooting-cluster-event-id-1135.md)
- [Problema con los nodos que se quitan de la pertenencia al clúster de conmutación por error activa](./problem-nodes-failover-cluster.md)
- [Nodos que se quitan de la pertenencia al clúster de conmutación por error en VMWare ESX](./nodes-failover-cluster-vmware.md)
- [IaaS con SQL AlwaysOn: Ajuste de umbrales de red de clústeres de conmutación por error](./iaas-sql-failover-cluster.md)

## <a name="troubleshoot-dhcp"></a>Solucionar problemas de DHCP

- [Guía de solución de problemas del Protocolo de configuración dinámica de host (DHCP)](./troubleshoot-dhcp-issue.md)
- [Aspectos básicos de DHCP (Protocolo de configuración dinámica de host)](./dynamic-host-configuration-protocol-basics.md)
- [Instrucciones generales para la solución de problemas de DHCP](./general-guidance-to-troubleshoot-dhcp.md)
- [Uso del direccionamiento automático de TCP/IP sin un servidor DHCP](./how-to-use-automatic-tcpip-addressing-without-a-dh.md)
- [Solución de problemas en el cliente DHCP](./troubleshoot-problems-on-dhcp-client.md)
- [Solución de problemas en el servidor DHCP](./troubleshoot-problems-on-dhcp-server.md)

## <a name="troubleshoot-fsrm"></a>Solucionar problemas de FSRM

- [Solución de problemas del Administrador de recursos del servidor de archivos](../storage/fsrm/troubleshooting-file-server-resource-manager.md)

## <a name="troubleshoot-guarded-fabric"></a>Solución de problemas de tejido protegido

- [Solución de problemas con la herramienta de diagnóstico de tejido protegido](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-diagnostics.md)
- [Solución de problemas del servicio de protección de host](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hgs.md)
- [Solución de problemas del servicio de protección de host](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts.md)

## <a name="troubleshoot-multi-site-ras"></a>Solución de problemas de RAS de varios sitios

- [Solucionar problemas relacionados con la activación de multisitio](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-enabling-multisite.md)
- [Solucionar problemas de agregar puntos de entrada](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-adding-entry-points.md)
- [Solucionar problemas relacionados con el establecimiento del controlador de dominio de punto de entrada](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-setting-the-entry-point-domain-controller.md)
- [Solucionar problemas relacionados con las direcciones URL de sondeo web](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-web-probe-urls.md)

## <a name="troubleshoot-nano-server"></a>Solución de problemas de nano Server

- [Solución de problemas de Nano Server](../get-started/troubleshooting-nano-server.md)

## <a name="troubleshoot-nic-teaming"></a>Solucionar problemas de formación de equipos NIC

- [Solución de problemas de formación de equipos NIC](../networking/technologies/nic-teaming/troubleshooting-nic-teaming.md)

## <a name="troubleshoot-otp-authentication"></a>Solucionar problemas de autenticación de OTP

- [Solucionar problemas relacionados con la autenticación](../remote/remote-access/ras/otp/troubleshoot/troubleshooting-authentication-issues.md)
- [Solucionar problemas relacionados con la activación de OTP](../remote/remote-access/ras/otp/troubleshoot/troubleshooting-enabling-otp.md)

## <a name="troubleshoot-qos"></a>Solución de problemas de QoS

- [Preguntas más frecuentes sobre QoS](../networking/technologies/qos/qos-policy-faq.md)

## <a name="troubleshoot-s2d"></a>Solucionar problemas de S2D

- [Solución de problemas de Espacios de almacenamiento directo](../storage/storage-spaces/troubleshooting-storage-spaces.md)
- [Preguntas más frecuentes sobre Espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-faq.md)
- [Espacios de almacenamiento directo Estados operativos y de estado](../storage/storage-spaces/storage-spaces-states.md)
- [Recopilación de datos de diagnóstico con Espacios de almacenamiento directo](../storage/storage-spaces/data-collection.md)
- [Administración de estado de la memoria de clase de almacenamiento (NVDIMM-N) en Windows](../storage/storage-spaces/storage-class-memory-health.md)

## <a name="troubleshoot-sdn"></a>Solucionar problemas de SDN

- [Solucionar problemas de SDN](../networking/sdn/troubleshoot/troubleshoot-software-defined-networking.md)
- [Solución de problemas de la pila de redes definidas por software de Windows Server](../networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack.md)

## <a name="troubleshoot-rds-session-connectivity"></a>Solucionar problemas de conectividad de sesión de RDS

- [Solución de problemas generales de conexión con Escritorio remoto](../remote/remote-desktop-services/troubleshoot/rdp-error-general-troubleshooting.md)
- [Los clientes no pueden conectarse y obtener el error de clase no registrada](../remote/remote-desktop-services/troubleshoot/rdp-error-class-not-registered.md)
- [Los clientes no se pueden conectar y no ve el error no hay licencias disponibles](../remote/remote-desktop-services/troubleshoot/rdp-error-no-licenses-available.md)
- [Los usuarios no se pueden autenticar o deben autenticarse dos veces](../remote/remote-desktop-services/troubleshoot/cannot-authenticate-or-must-authenticate-twice.md)
- [Al conectarse, el usuario recibe el mensaje de Escritorio remoto el servicio está ocupado actualmente](../remote/remote-desktop-services/troubleshoot/remote-desktop-service-currently-busy.md)
- [El cliente de Escritorio remoto se desconecta y no puede volver a conectarse a la misma sesión](../remote/remote-desktop-services/troubleshoot/rdp-client-disconnects-cannot-reconnect-same-session.md)
- [Los portátiles remotos se desconectan de la red inalámbrica](../remote/remote-desktop-services/troubleshoot/remote-laptop-disconnects-wireless-network.md)
- [Problemas de bajo rendimiento o de aplicaciones durante la conexión a escritorio remoto](../remote/remote-desktop-services/troubleshoot/poor-performance-or-application-problems.md)

## <a name="troubleshoot-shielded-vm"></a>Solución de problemas de máquinas virtuales blindadas

- [Solución de problemas de máquinas virtuales blindadas](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms.md)

## <a name="troubleshoot-software-restriction-policies"></a>Solucionar problemas de directivas de restricción de software

- [Solución de problemas de las directivas de restricción de software](../identity/software-restriction-policies/troubleshoot-software-restriction-policies.md)

## <a name="troubleshoot-storage-migration"></a>Solucionar problemas de migración de almacenamiento

- [Problemas conocidos del servicio de migración de almacenamiento](../storage/storage-migration-service/known-issues.md)
- [Preguntas más frecuentes sobre el servicio de migración de almacenamiento (p + f)](../storage/storage-migration-service/faq.md)

## <a name="troubleshoot-storage-replica"></a>Solucionar problemas de réplica de almacenamiento

- [Problemas conocidos de Réplica de almacenamiento](../storage/storage-replica/storage-replica-known-issues.md)
- [Preguntas frecuentes acerca de Réplica de almacenamiento](../storage/storage-replica/storage-replica-frequently-asked-questions.md)

## <a name="troubleshoot-user-profiles"></a>Solucionar problemas de perfiles de usuario

- [Solución de problemas de perfiles de usuario con eventos](../storage/folder-redirection/troubleshoot-user-profiles-events.md)

## <a name="troubleshoot-vrss"></a>Solucionar problemas de vRSS

- [Preguntas más frecuentes sobre vRSS](../networking/technologies/vrss/vrss-faq.md)

## <a name="troubleshoot-webproxy"></a>Solucionar problemas de WebProxy

- [Solución de problemas del Proxy de aplicación web](../remote/remote-access/web-application-proxy/troubleshooting-web-application-proxy.md)

## <a name="troubleshoot-windows-admin-center"></a>Solución de problemas de Windows Admin Center

- [Pasos comunes de solución de problemas del centro de administración de Windows](../manage/windows-admin-center/support/troubleshooting.md)
- [Problemas conocidos del centro de administración de Windows](../manage/windows-admin-center/support/known-issues.md)
- [Preguntas más frecuentes acerca de Windows Admin Center](../manage/windows-admin-center/understand/faq.md)