---
title: Información general del servicio de migración de almacenamiento
description: El servicio de migración de almacenamiento facilita la migración de almacenamiento a Windows Server o a Azure. Proporciona una herramienta gráfica que inventa datos en servidores Windows y Linux y, a continuación, transfiere los datos a servidores más recientes o a máquinas virtuales de Azure. El servicio de migración de almacenamiento también proporciona la opción de transferir la identidad de un servidor al servidor de destino para que las aplicaciones y los usuarios puedan acceder a sus datos sin cambiar los vínculos o las rutas de acceso.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 03/26/2020
ms.topic: article
ms.openlocfilehash: 18c9bb2bf22f107b988942706850907c67b5ec9a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939333"
---
# <a name="storage-migration-service-overview"></a>Información general del servicio de migración de almacenamiento

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canal semianual)

El servicio de migración de almacenamiento facilita la migración de almacenamiento a Windows Server o a Azure. Proporciona una herramienta gráfica que inventa datos en servidores Windows y Linux y, a continuación, transfiere los datos a servidores más recientes o a máquinas virtuales de Azure. El servicio de migración de almacenamiento también proporciona la opción de transferir la identidad de un servidor al servidor de destino para que las aplicaciones y los usuarios puedan acceder a sus datos sin cambiar los vínculos o las rutas de acceso.

En este tema se explica por qué desea usar el servicio de migración de almacenamiento, cómo funciona el proceso de migración, cuáles son los requisitos para los servidores de origen y destino, y las novedades [de Storage Migration Service](#whats-new-in-storage-migration-service).

## <a name="why-use-storage-migration-service"></a>Por qué usar el servicio de migración de almacenamiento

Use el servicio de migración de almacenamiento porque tiene un servidor (o muchos servidores) que desea migrar a máquinas virtuales o hardware más recientes. El servicio de migración de almacenamiento está diseñado para ayudarle a hacer lo siguiente:

- Inventario de varios servidores y sus datos
- Transferencia rápida de archivos, recursos compartidos de archivos y configuración de seguridad de los servidores de origen
- Opcionalmente, asuma la identidad de los servidores de origen (también conocido como recorte) para que los usuarios y las aplicaciones no tengan que cambiar nada para acceder a los datos existentes.
- Administrar una o varias migraciones desde la interfaz de usuario del centro de administración de Windows

![Diagrama que muestra el servicio de migración de almacenamiento migrando archivos & configuración de los servidores de origen a los servidores de destino, las máquinas virtuales de Azure o Azure File Sync.](media/overview/storage-migration-service-diagram.png)

**Figura 1: orígenes y destinos de los servicios de migración de almacenamiento**

## <a name="how-the-migration-process-works"></a>Cómo funciona el proceso de migración

La migración es un proceso de tres pasos:

1. **Servidores de inventario** para recopilar información sobre los archivos y la configuración (que se muestran en la figura 2).
2. **Transferir datos (copiarlos)** de los servidores de origen a los servidores de destino.
3. **Recorte a los nuevos servidores** (opcional).<br>Los servidores de destino suponen las identidades anteriores de los servidores de origen para que las aplicaciones y los usuarios no tengan que cambiar nada. <br>Los servidores de origen entran en un estado de mantenimiento donde todavía contienen los mismos archivos que siempre tienen (nunca se quitan archivos de los servidores de origen) pero no están disponibles para los usuarios y las aplicaciones. Después, puede retirar los servidores a su comodidad.

![Captura de pantalla que muestra un servidor listo para su examen ](media/migrate/inventory.png)
 **ilustración 2: servidores de inventario de migración de almacenamiento**

Este es un vídeo que muestra cómo usar el servicio de migración de almacenamiento para tomar un servidor, como un servidor Windows Server 2008 R2 que ahora no es compatible, y cómo trasladar el almacenamiento a un servidor más reciente.

> [!VIDEO https://www.youtube.com/embed/h-Xc9j1w144]

## <a name="requirements"></a>Requisitos

Para usar el servicio de migración de almacenamiento, necesita lo siguiente:

- Un **servidor de origen** o un **clúster de conmutación por error** del que migrar archivos y datos
- Un **servidor de destino** que ejecuta Windows Server 2019 (en clúster o independiente) al que migrar. Windows Server 2016 y Windows Server 2012 R2 funcionan también, pero son aproximadamente del 50% más lento
- Un **servidor de Orchestrator** que ejecute Windows Server 2019 para administrar la migración  <br>Si va a migrar solo unos pocos servidores y uno de ellos ejecuta Windows Server 2019, puede usarlo como el orquestador. Si va a migrar más servidores, se recomienda usar un servidor de Orchestrator independiente.
- Un **equipo o servidor que ejecute el [centro de administración de Windows](../../manage/windows-admin-center/overview.md) ** para ejecutar la interfaz de usuario del servicio de migración de almacenamiento, a menos que prefiera usar PowerShell para administrar la migración. La versión del centro de administración de Windows y de Windows Server 2019 debe ser al menos la versión 1809.

Se recomienda encarecidamente que el orquestador y los equipos de destino tengan al menos dos núcleos o dos vCPU, y al menos 2 GB de memoria. Las operaciones de inventario y transferencia son significativamente más rápidas con más procesadores y memoria.

### <a name="security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports"></a>Requisitos de seguridad, el servicio de proxy del servicio de migración de almacenamiento y los puertos del firewall

- Una cuenta de migración que sea un administrador en los equipos de origen y el equipo de Orchestrator.
- Una cuenta de migración que sea un administrador en los equipos de destino y el equipo de Orchestrator.
- El equipo de Orchestrator debe tener habilitada la regla de Firewall de uso compartido de archivos e impresoras (SMB de *entrada*).
- Los equipos de origen y de destino deben tener las siguientes reglas de Firewall habilitadas de *entrada* (aunque es posible que ya estén habilitadas):
  - Compartir archivos e impresoras (SMB de entrada)
  - Servicio NetLogon (NP-in)
  - Instrumental de administración de Windows (DCOM-In)
  - Instrumental de administración de Windows (WMI-In)

  > [!TIP]
  > Al instalar el servicio de servidor proxy de migración de almacenamiento en un equipo con Windows Server 2019, se abren automáticamente los puertos de Firewall necesarios en dicho equipo. Para ello, conéctese al servidor de destino en el centro de administración de Windows y, a continuación, vaya a **Administrador del servidor** (en el centro de administración de windows) > **roles y características**, seleccione **proxy de servicio de migración de almacenamiento**y, a continuación, seleccione **instalar**.


- Si los equipos pertenecen a un dominio de Active Directory Domain Services, todos deben pertenecer al mismo bosque. El servidor de destino también debe estar en el mismo dominio que el servidor de origen si desea transferir el nombre de dominio del origen al destino al realizar la reducción. El traslado técnicamente funciona entre dominios, pero el nombre de dominio completo del destino será diferente del origen...

### <a name="requirements-for-source-servers"></a>Requisitos para los servidores de origen

El servidor de origen debe ejecutar uno de los siguientes sistemas operativos:

- Windows Server, Canal semianual
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003
- Windows Small Business Server 2003 R2
- Windows Small Business Server 2008
- Windows Small Business Server 2011
- Windows Server 2012 Essentials
- Windows Server 2012 R2 Essentials
- Windows Server 2016 Essentials
- Windows Server 2019 Essentials
- Windows Storage Server 2008
- Windows Storage Server 2008 R2
- Windows Storage Server 2012
- Windows Storage Server 2012 R2
- Windows Storage Server 2016

Nota: Windows Small Business Server y Windows Server Essentials son controladores de dominio. El servicio de migración de almacenamiento todavía no puede pasar de los controladores de dominio, pero puede inventariar y transferir archivos de ellos.

Puede migrar los siguientes tipos de origen adicionales si el orquestador ejecuta Windows Server, versión 1903 o posterior, o si el orquestador está ejecutando una versión anterior de Windows Server con [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) instalado:

- Clústeres de conmutación por error que ejecutan Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019
- Servidores Linux que usan Samba. Hemos probado lo siguiente:
    - CentOS 7
    - Debian GNU/Linux 8
    - RedHat Enterprise Linux 7,6
    - SUSE Linux Enterprise Server (SLES) 11 SP4
    - Ubuntu 16,04 LTS y 12.04.5 LTS
    - Samba 4,8, 4,7, 4,3, 4,2 y 3,6

### <a name="requirements-for-destination-servers"></a>Requisitos para los servidores de destino

El servidor de destino debe ejecutar uno de los siguientes sistemas operativos:

- Windows Server, Canal semianual
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Los servidores de destino que ejecutan Windows Server 2019 o Windows Server, canal semianual o posterior, tienen el doble de rendimiento de transferencia de versiones anteriores de Windows Server. Esta mejora del rendimiento se debe a la inclusión de un servicio de proxy de servicio de migración de almacenamiento integrado, que también abre los puertos de Firewall necesarios si aún no están abiertos.

## <a name="azure-vm-migration"></a>Migración de máquinas virtuales de Azure

La versión 1910 del centro de administración de Windows permite implementar máquinas virtuales de Azure. Esto integra la implementación de la máquina virtual en el servicio de migración de almacenamiento. En lugar de crear nuevos servidores y máquinas virtuales en Azure portal manualmente antes de implementar la carga de trabajo, y posiblemente faltan los pasos necesarios y la configuración, el centro de administración de Windows puede implementar la máquina virtual de Azure, configurar su almacenamiento, unirlo al dominio, instalar roles y, a continuación, configurar el sistema distribuido.

   Este es un vídeo que muestra cómo usar el servicio de migración de almacenamiento para migrar a máquinas virtuales de Azure.
   > [!VIDEO https://www.youtube-nocookie.com/embed/k8Z9LuVL0xQ]

Si desea levantar y desplazar las máquinas virtuales a Azure sin migrar a un sistema operativo posterior, considere la posibilidad de usar Azure Migrate. Para obtener más información, vea [información general sobre Azure Migrate](https://go.microsoft.com/fwlink/?linkid=2056064).

## <a name="whats-new-in-storage-migration-service"></a>Novedades de Storage Migration Service

La versión 1910 del centro de administración de Windows agrega la capacidad de implementar máquinas virtuales de Azure. Esto integra la implementación de máquinas virtuales de Azure en el servicio de migración de almacenamiento. Para obtener más información, consulte [migración de máquinas virtuales de Azure](#azure-vm-migration).

Las siguientes características nuevas están disponibles al ejecutar el orquestador del servidor de migración de almacenamiento en Windows Server, versión 1903 o posterior, o una versión anterior de Windows Server con [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) instalado:

- Migrar usuarios y grupos locales al nuevo servidor.
- Migrar el almacenamiento de clústeres de conmutación por error, migrar a clústeres de conmutación por error y migrar entre servidores independientes y clústeres de conmutación por error
- Migrar el almacenamiento desde un servidor Linux que usa Samba.
- Sincronizar más fácilmente los recursos compartidos migrados en Azure mediante Azure File Sync.
- Migrar a nuevas redes, como Azure.

## <a name="additional-references"></a>Referencias adicionales

- [Migración de un servidor de archivos mediante el servicio de migración de almacenamiento](migrate-data.md)
- [Preguntas más frecuentes (p + f) sobre Storage Migration Services](faq.md)
- [Problemas conocidos del servicio de migración de almacenamiento](known-issues.md)
