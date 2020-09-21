---
title: What's New in Windows Server 2016 (Novedades en Windows Server 2016)
description: ¿Cuáles son las nuevas características de proceso, identidad, administración, automatización, redes, seguridad y almacenamiento?
ms.date: 05/21/2019
ms.topic: article
ms.assetid: 2827f332-44d4-4785-8b13-98429087dcc7
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 5dfed9e70e4f0406c59c31201c8d2d1a9b3caafe
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766868"
---
# <a name="whats-new-in-windows-server-2016"></a>What's New in Windows Server 2016 (Novedades en Windows Server 2016)

>Se aplica a: Windows Server 2016

![Icono que muestra un periódico](media/whats-new.png) Para obtener información sobre las características más recientes de Windows, consulta [Novedades de Windows Server](whats-new-in-windows-server.md). El contenido de esta sección describe las novedades y los cambios de Windows Server&reg; 2016. Las nuevas características y los cambios que se muestran aquí son los que probablemente tengan un mayor impacto al trabajar con esta versión.

## <a name="compute"></a>[Compute](../virtualization/virtualization.yml)

El área Virtualización incluye características y productos de virtualización destinados a los profesionales de TI para diseñar, implementar y mantener Windows Server.

### <a name="general"></a>General
Las máquinas físicas y virtuales se benefician de una mayor precisión temporal gracias a las mejoras en los servicios de sincronización de hora de Hyper-V y Win32. Windows Server ahora puede hospedar servicios que cumplen con las próximas normas que requieren una precisión de 1 ms con respecto a UTC.

### <a name="hyper-v"></a>Hyper-V
-   [Novedades de Hyper-V en Windows Server 2016](../virtualization/hyper-v/What-s-new-in-Hyper-V-on-Windows.md). En este tema se explica la funcionalidad nueva y modificada del rol de Hyper-V en Windows Server 2016, el cliente Hyper-V que se ejecuta en Windows 10 y Microsoft Hyper-V Server 2016.

-   [Contenedores Windows](/virtualization/windowscontainers/):  los contenedores Windows Server 2016 aportan mejoras de rendimiento, una administración de red simplificada y compatibilidad para contenedores de Windows en Windows 10. Para más información sobre los contenedores, consulta [Containers: Docker, Windows and Trends](https://azure.microsoft.com/blog/2015/08/17/containers-docker-windows-and-trends/) (Contenedores: Docker, Windows y tendencias).

### <a name="nano-server"></a>Nano Server
Novedades de [Nano Server](getting-started-with-nano-server.md). Nano Server cuenta con un módulo actualizado para compilar imágenes de Nano Server, que incluye una mayor separación de la funcionalidad del host físico y la máquina virtual de invitado y compatibilidad con las diferentes ediciones de Windows Server.

También hay mejoras en la Consola de recuperación, como la separación de reglas de firewall de entrada y salida, y la posibilidad de reparar la configuración de WinRM.

### <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas
Windows Server 2016 proporciona una nueva máquina virtual blindada basada en Hyper-V para proteger cualquier máquina virtual de generación 2 de un tejido comprometido. Entre las características introducidas en Windows Server 2016 destacan las siguientes:

- El nuevo modo "Cifrado admitido" que ofrece un nivel de protección mayor que el de una máquina virtual común, pero menor que el modo "Blindado", mientras se continúa admitiendo vTPM; el cifrado de disco; el cifrado de tráfico y Migración en vivo; y otras características, incluidas las facilidades en la administración directa del tejido, como las conexiones de consola de máquina virtual y Powershell Direct.

- Soporte completo para la conversión de las máquinas virtuales no blindadas de segunda generación a máquinas virtuales blindadas, incluido el cifrado de disco automatizado.

- Hyper-V Virtual Machine Manager ahora puede ver los tejidos sobre los que puede ejecutarse una máquina virtual blindada, lo que permite al administrador de tejidos abrir un protector de clave de la máquina virtual blindada y ver los tejidos sobre los que puede ejecutar.

- Puede cambiar los modos de atestación en un Servicio de protección de host. Ahora puede cambiar sobre la marcha entre la atestación basada en Active Directory, menos segura pero más sencilla, y la atestación basada en TPM.

- Las herramientas de diagnóstico integrales basadas en Windows PowerShell que pueden detectar configuraciones incorrectas o errores en ambos hosts protegidos de Hyper-V y el Servicio de protección de host.

- Un entorno de recuperación que ofrece un medio para solucionar problemas y reparar máquinas virtuales blindadas dentro del tejido en el que se ejecutan normalmente ofreciendo al mismo tiempo un nivel de protección idéntico al de la propia máquina virtual blindada.

- Compatibilidad con el Servicio de protección de host para proteger el entorno existente de Active Directory: puede dirigir el Servicio de protección de host para usar un bosque existente de Active Directory como su Active Directory en lugar de crear su propia instancia de Active Directory

Para obtener más detalles e instrucciones para trabajar con máquinas virtuales blindadas, consulte [Shielded VMs and Guarded Fabric Validation Guide for Windows Server 2016 (TPM)](../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md) (Máquinas virtuales blindadas y guía de validación de tejido protegido para Windows Server 2016 [TPM]).

## <a name="identity-and-access"></a>[Identidad y acceso](../identity/Identity-and-Access.yml)
Las nuevas características de Identidad aumentan la capacidad de las organizaciones de proteger los entornos de Active Directory y les ayudan a migrar a implementaciones de solo en la nube e implementaciones híbridas, donde algunas aplicaciones y servicios se hospedan en la nube y otros se hospedan de forma local.

### <a name="active-directory-certificate-services"></a>Servicios de certificados de Active Directory
Servicios de certificados de Active Directory (AD CS) en Windows Server 2016 aumenta la compatibilidad para la atestación de claves de TPM: ahora puedes usar el KSP de tarjeta inteligente para atestación de la clave, y los dispositivos que no están unidos al dominio ahora pueden utilizar la inscripción NDES para obtener los certificados que pueden recibir atestación para las claves que están en TPM.

### <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory
Los Servicios de dominio de Active Directory incluyen mejoras que ayudan a las organizaciones a proteger los entornos de Active Directory y mejoran la administración de identidades tanto para dispositivos personales como corporativos. Para más información, vea [What's new in Active Directory Domain Services (AD DS) in Windows Server 2016](../identity/whats-new-active-directory-domain-services.md) (Novedades de Active Directory Domain Services para Windows Server 2016).

### <a name="active-directory-federation-services"></a>Servicios de federación de Active Directory (AD FS)
Novedades de Servicios de federación de Active Directory Los Servicios de federación de Active Directory (ADFS) en Windows Server 2016 incluyen nuevas características que le permiten configurar AD FS para la autenticación de usuarios almacenados en directorios de protocolo ligero de acceso a directorios (LDAP). Para más información, consulta [What's new in Active Directory Federation Services for Windows Server 2016 Technical Preview](../identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server.md) (Novedades en AD FS para Windows Server 2016).

### <a name="web-application-proxy"></a>Proxy de aplicación web
La versión más reciente del Proxy de aplicación web se centra en las nuevas características que permiten la publicación y la autenticación previa de más aplicaciones y una experiencia de usuario mejorada. Consulte la lista completa de las nuevas características que incluye autenticación previa para aplicaciones de cliente enriquecidas Exchange ActiveSync y dominios con comodín para una publicación más sencilla de aplicaciones de SharePoint. Para más información, consulta [Proxy de aplicación web en Windows Server 2016](../remote/remote-access/web-application-proxy/web-application-proxy-windows-server.md).

##  <a name="administration"></a>[Administración](../administration/manage-windows-server.yml)
El área Administración y automatización se centra en la información de referencia y las herramientas para profesionales de TI que desean ejecutar y administrar Windows Server 2016, incluido Windows PowerShell.

Windows PowerShell 5.1 incluye nuevas e importantes características, entre las que se incluyen el soporte para el desarrollo con clases y las nuevas características de seguridad, que amplían y mejoran su uso, y le permiten controlar y administrar entornos basados en Windows de manera más sencilla y completa. Consulta [Nuevos escenarios y características de WMF 5.1](/powershell/wmf/5.1/scenarios-features) para obtener más información.

Las incorporaciones nuevas de Windows Server 2016 incluyen: la capacidad de ejecutar PowerShell.exe localmente en Nano Server (ya no solo de manera remota), nuevos cmdlets de usuarios y grupos locales para reemplazar la GUI y la incorporación de compatibilidad con la depuración de PowerShell y en Nano Server para la transcripción y el registro de seguridad y JEA.

A continuación se indican algunas de las otras nuevas características de administración:

### <a name="powershell-desired-state-configuration-dsc-in-windows-management-framework-wmf-5"></a>Servicio de configuración estado deseado de PowerShell en Windows Management Framework (WMF) 5
Windows Management Framework 5 incluye actualizaciones a la configuración de estado deseado de Windows PowerShell (DSC), la Administración remota de Windows (WinRM) y el Instrumental de administración de Windows (WMI).

Para obtener más información sobre cómo probar las características de DSC de Windows Management Framework 5, consulta la serie de entradas de blog que se encuentran en [Validación de características de la configuración de estado deseado de PowerShell DSC](https://devblogs.microsoft.com/powershell/validate-features-of-powershell-dsc/). Para realizar la descarga, consulta [Windows Management Framework 5.1 ](/powershell/scripting/wmf/setup/install-configure).

### <a name="packagemanagement-unified-package-management-for-software-discovery-installation-and-inventory"></a>PackageManagement ha unificado la administración de paquetes para inventario, instalación y detección de software
Windows Server 2016 y Windows 10 incluyen la nueva característica PackageManagement (anteriormente denominada OneGet) que permite a profesionales de TI o de DevOps automatizar la detección de software, la instalación y el inventario, de manera local o remota, con independencia de la tecnología de instalador y de dónde se encuentra el software.

Para más información, consulta [https://github.com/OneGet/oneget/wiki](https://github.com/OneGet/oneget/wiki).

### <a name="powershell-enhancements-to-assist-digital-forensics-and-help-reduce-security-breaches"></a>Mejoras de PowerShell para ayudar a realizar análisis forenses digitales y a reducir las infracciones de seguridad
Para ayudar al equipo responsable a investigar sistemas comprometidos, a veces conocido como el “equipo azul”, se ha agregado la funcionalidad adicional de registros de PowerShell y otra funcionalidad de análisis forenses digitales, ademá de una funcionalidad para ayudar a reducir las vulnerabilidades en scripts, como PowerShell restringido y API CodeGeneration seguras.

Para obtener información, consulta [PowerShell ♥ the Blue Team](https://devblogs.microsoft.com/powershell/powershell-the-blue-team/).

## <a name="networking"></a>[Redes](../networking/index.yml)
Esta área abarca los productos y las características de redes dirigidos al profesional de TI para diseñar, implementar y mantener Windows Server 2016.

### <a name="software-defined-networking"></a>Redes definidas por software
Ahora puede reflejar y enrutar tráfico a dispositivos virtuales nuevos o existentes. Junto con un firewall distribuido y los grupos de seguridad de red, esto le permite segmentar dinámicamente y proteger las cargas de trabajo de una manera similar a Azure. En segundo lugar, puede implementar y administrar por completo las redes definidas por software (SDN) con System Center Virtual Machine Manager. Por último, puede usar Docker para administrar las redes de contenedor de Windows Server y asociar directivas de SDN no solo con las máquinas virtuales, sino también con contenedores. Para más información, consulta [Planeación de una infraestructura de red definida por software](../networking/sdn/plan/plan-a-software-defined-network-infrastructure.md).

### <a name="tcp-performance-improvements"></a>Mejoras en el rendimiento de TCP
El valor predeterminado del intervalo de congestión inicial (ICW) se ha aumentado de 4 a 10 y TCP Fast Open (TFO) se ha implementado. TFO reduce la cantidad de tiempo necesario para establecer una conexión TCP y el ICW aumentado permite la transferencia de objetos más grandes a la ráfaga inicial. Esta combinación puede reducir significativamente el tiempo necesario para transferir un objeto de Internet entre el cliente y la nube.

Para mejorar el comportamiento de TCP al realizar la recuperación de pérdida de paquetes, se han implementado Tail Loss Probe (TLP) y Recent Acknowledgement (RACK) en TCP. TLP ayuda a convertir los tiempos de espera de retransmisión (RTO) para recuperaciones rápidas y RACK reduce el tiempo necesario para que la recuperación rápida retransmita un paquete perdido. 

## <a name="security-and-assurance"></a>[Seguridad y control](../security/Security-and-Assurance.yml)
Incluye soluciones y características de seguridad para que los profesionales de TI implementen en su entorno de nube y centro de datos. Para obtener información sobre la seguridad en Windows Server 2016 en general, vea [Seguridad y control](../security/Security-and-Assurance.yml).

### <a name="just-enough-administration"></a>Just Enough Administration (JEA)
Just Enough Administration (JEA) en Windows Server 2016 es la tecnología de seguridad que habilita la administración delegada para todo lo que se puede administrar con Windows PowerShell. Entre las funcionalidades se incluyen compatibilidad para la ejecución bajo una identidad de red, conexión a través de PowerShell Direct, copia segura hacia y desde puntos de conexión de JEA, y configuración de la consola de PowerShell para iniciar en un contexto de JEA de manera predeterminada. Para más información, consulte [JEA en GitHub](https://aka.ms/JEA).

### <a name="credential-guard"></a>Credential Guard
Credential Guard usa la seguridad basada en virtualización para aislar los secretos de forma que solo el software de sistema con privilegios pueda acceder a ellos. Vea [Protección de credenciales de dominio derivadas con Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

###  <a name="remote-credential-guard"></a>Credential Guard remoto
Credential Guard incluye compatibilidad con sesiones RDP para que las credenciales de usuario permanezcan en el lado cliente y no se expongan en el lado servidor. También proporciona inicio de sesión único para las conexiones a Escritorio remoto. Consulta [Proteger las credenciales de dominio derivadas con Credential Guard de Windows Defender](/windows/access-protection/credential-guard/credential-guard).

### <a name="device-guard-code-integrity"></a>Device Guard (integridad de código)
Device Guard proporciona integridad de código del modo kernel (KMCI) e integridad de código del modo de usuario (UMCI) mediante la creación de directivas que especifican qué código se puede ejecutar en el servidor. Consulta [Introducción a Device Guard de Windows Defender: directivas de integridad de código y seguridad basada en virtualización](/windows/device-security/device-guard/introduction-to-device-guard-virtualization-based-security-and-code-integrity-policies).


### <a name="windows-defender"></a>Windows Defender
[Información general acerca de Windows Defender para Windows Server 2016](../security/windows-defender/windows-defender-overview-windows-server.md). Windows Server Antimalware está instalado y habilitado de forma predeterminada en Windows Server 2016, aunque no así su interfaz de usuario. A pesar de ello, Windows Server Antimalware actualizará las definiciones de antimalware y protegerá el equipo sin la interfaz de usuario. Si necesita la interfaz de usuario de Windows Server Antimalware, puede instalarla después de haber instalado el sistema operativo mediante el Asistente para agregar roles y características.

### <a name="control-flow-guard"></a>Protección de flujo de control
Protección de flujo de control (CFG) es una característica de seguridad de plataforma que se creó para luchar contra vulnerabilidades de corrupción de memoria. Para obtener más información, vea [Protección de flujo de control](/windows/win32/secbp/control-flow-guard).


## <a name="storage"></a>[Almacenamiento](../storage/storage.yml)

El almacenamiento en Windows Server 2016 incluye nuevas características y mejoras de almacenamiento definido por el software, así como servidores de archivos tradicionales. A continuación se muestran algunas de las nuevas características; para consultar más mejoras y detalles, vea [Novedades de Espacios de almacenamiento en Windows Server 2016](../storage/whats-new-in-storage.md).

### <a name="storage-spaces-direct"></a>Espacios de almacenamiento directos

Espacios de almacenamiento directo permite la creación de almacenamiento altamente disponible y escalable con servidores de almacenamiento local. Simplifica la implementación y administración de los sistemas de almacenamiento definidos por software y desbloquea el uso de las nuevas clases de dispositivos de disco, como SSD de SATA y dispositivos de disco NVMe, que no estaban disponibles con Espacios de almacenamiento de clúster con discos compartidos.

Para obtener más información, vea [Espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-overview.md).

### <a name="storage-replica"></a>Réplica de almacenamiento

Réplica de almacenamiento (SR) permite la replicación sincrónica independiente del almacenamiento y a nivel de bloque entre servidores o clústeres para la recuperación ante desastres, así como la extensión de un clúster de conmutación por error entre sitios. La replicación sincrónica permite el reflejo de datos en sitios físicos con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos. La replicación asincrónica permite la extensión de sitios más allá del área metropolitana con la posibilidad de pérdida de datos.

Para más información, vea [Réplica de almacenamiento](../storage/storage-replica/storage-replica-overview.md).

### <a name="storage-quality-of-service-qos"></a>Calidad de servicio (QoS) del almacenamiento

Ahora puede usar la calidad de servicio del almacenamiento para supervisar de manera centralizada el rendimiento del almacenamiento de extremo a extremo y crear directivas de administración mediante Hyper-V y clústeres de CSV en Windows Server 2016.

Para más información, vea [Calidad de servicio del almacenamiento](../storage/storage-qos/storage-qos-overview.md).

## <a name="failover-clustering"></a>[Clúster de conmutación por error](../failover-clustering/whats-new-in-failover-clustering.md)

Windows Server 2016 incluye una serie de nuevas características y mejoras para varios servidores que se agrupan en un único clúster tolerante a errores mediante la característica Clústeres de conmutación por error. Algunas de las adiciones se enumeran a continuación; para obtener una lista más completa, vea [Novedades de los clústeres de conmutación por error de Windows Server 2016](../failover-clustering/whats-new-in-failover-clustering.md).

### <a name="cluster-operating-system-rolling-upgrade"></a>Actualización gradual del sistema operativo del clúster

La actualización gradual del sistema operativo del clúster permite a un administrador actualizar el sistema operativo de los nodos del clúster de Windows Server 2012 R2 a Windows Server 2016 sin detener la función Hyper-V o las cargas de trabajo del Servidor de archivos de escalabilidad horizontal. Con esta característica, se pueden evitar las penalizaciones de tiempo de inactividad en los acuerdos de nivel de servicio (SLA).

Para más información, vea [Actualización gradual del sistema operativo del clúster](../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md).

### <a name="cloud-witness"></a>Testigo en la nube

Testigo en la nube es un nuevo tipo de testigo de cuórum de clúster de conmutación por error en Windows Server 2016 que utiliza Microsoft Azure como punto de arbitraje. El testigo en la nube, como cualquier otro testigo de cuórum, obtiene un voto y pueden participar en los cálculos de cuórum. Puede configurar el testigo en la nube como un testigo de cuórum el Asistente para configurar un cuórum de clúster.

Para más información, vea [Implementación de un testigo en la nube](../failover-clustering/deploy-cloud-witness.md).

### <a name="health-service"></a>Servicio de mantenimiento

El Servicio de mantenimiento mejora la supervisión diaria, las operaciones y la experiencia de mantenimiento de recursos de clúster en un clúster de Espacios de almacenamiento directo.

Para más información, vea [Servicio de mantenimiento](../failover-clustering/health-service-overview.md).

## <a name="application-development"></a>Desarrollo de aplicaciones

### <a name="internet-information-services-iis-100"></a>Internet Information Services (IIS) 10.0
Entre las nuevas características proporcionadas por el servidor web IIS 10.0 en Windows Server 2016 se incluyen:

- Compatibilidad con el protocolo HTTP/2 en la pila de red e integrada con IIS 10.0, lo que permite a los sitios web de IIS 10.0 atender a automáticamente las solicitudes HTTP/2 para las configuraciones admitidas. Esto permite numerosas mejoras frente a HTTP/1.1, como una reutilización más eficaz de las conexiones y una menor latencia, lo que mejora los tiempos de carga de páginas web.
- Capacidad para ejecutar y administrar IIS 10.0 en Nano Server. Consulta [IIS en Nano Server](iis-on-nano-server.md).
- Compatibilidad con encabezados de host comodín, lo que permite a los administradores configurar un servidor web para un dominio y luego hacer que el servidor web atienda las solicitudes de un subdominio cualquiera.
- Un nuevo módulo de PowerShell (IISAdministration) para administrar IIS.

Para obtener más detalles, consulta [IIS](https://iis.net/learn).

### <a name="distributed-transaction-coordinator-msdtc"></a>Coordinador de transacciones distribuidas (MSDTC)
En Microsoft Windows 10 y Windows Server 2016, se agregaron tres características nuevas:

- Un administrador de recursos puede usar una nueva interfaz para Rejoin del Administrador de recursos para determinar el resultado de una transacción dudosa después de que una base de datos se reinicie debido a un error. Para obtener más información, consulta [IResourceManagerRejoinable::Rejoin](/previous-versions/windows/desktop/mt203799(v=vs.85)).

- El límite de nombre DSN se amplió de 256 bytes a 3072 bytes. Para obtener más información, consulta [IDtcToXaHelperFactory::Create](/previous-versions/windows/desktop/ms686861(v=vs.85)), [IDtcToXaHelperSinglePipe::XARMCreate](/previous-versions/windows/desktop/ms679248(v=vs.85)) o [IDtcToXaMapper::RequestNewResourceManager](/previous-versions/windows/desktop/ms680310(v=vs.85)).

- Se mejoró el seguimiento, lo que te permite establecer una clave del registro para incluir una ruta de acceso del archivo de imagen en el nombre de archivo del registro de seguimiento, para que puedas saber qué archivo del registro de seguimiento debes comprobar. Para obtener más información sobre cómo configurar el seguimiento de MSDTC, consulta [Cómo habilitar el seguimiento de diagnóstico para MS DTC en un equipo basado en Windows](https://support.microsoft.com/kb/926099).



## <a name="see-also"></a>Consulte también
-   [Notas de la versión: Problemas importantes en Windows Server 2016](Windows-Server-2016-GA-Release-Notes.md)