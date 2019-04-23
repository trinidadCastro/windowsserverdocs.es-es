---
Title: Información general sobre el servicio de migración de almacenamiento
description: Breve descripción del tema de los resultados del motor de búsqueda
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 09/24/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: edc82c996f6877f770454fc6e27ccf5205a7d540
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843866"
---
# <a name="storage-migration-service-overview"></a>Información general sobre el servicio de migración de almacenamiento

Servicio de migración de almacenamiento facilita la migración de servidores a una versión más reciente de Windows Server. Proporciona una herramienta gráfica que hará un inventario de datos en los servidores y, a continuación, transfiere los datos y la configuración a los servidores más recientes, todo ello sin tener que cambiar nada de usuarios o aplicaciones.

Este tema explica por qué querría usar el servicio de migración de almacenamiento, cómo funciona el proceso de migración y cuáles son los requisitos para los servidores de origen y destino.

## <a name="why-use-storage-migration-service"></a>¿Por qué usar el servicio de migración de almacenamiento

Use el servicio de migración de almacenamiento porque tiene un servidor (o una gran cantidad de servidores) que van a migrar a versiones más recientes hardware o máquinas virtuales. Servicio de migración de almacenamiento está diseñado para ayudar a haciendo lo siguiente:

- Inventario de varios servidores y sus datos
- Transferencia rápida de archivos, recursos compartidos de archivos y configuración de seguridad de los servidores de origen
- Opcionalmente, asumir la identidad de los servidores de origen (también conocida como cortar a través) para que los usuarios y aplicaciones no tienen que cambiar nada para tener acceso a datos existentes
- Administrar una o varias de las migraciones desde la interfaz de usuario de Windows Admin Center

![Diagrama que muestra los archivos de migración de servicio de migración de almacenamiento & configuración de los servidores de origen en los servidores de destino, las máquinas virtuales de Azure o Azure File Sync.](media\overview\storage-migration-service-diagram.png)

**Figura 1: Storage Migration Service orígenes y destinos**

## <a name="how-the-migration-process-works"></a>Cómo funciona el proceso de migración

La migración es un proceso de tres pasos:

1. **Inventario de servidores** para recopilar información acerca de sus archivos y configuración (que se muestra en la figura 2).
2. **Transferencia de datos (copia)** desde los servidores de origen a los servidores de destino.
3. **Pasar a los nuevos servidores** (opcional).<br>Los servidores de destino suponen el origen de identidades anterior de los servidores para que las aplicaciones y los usuarios no tengan que cambiar nada. <br>Los servidores de origen entrar en un estado de mantenimiento donde aún contienen el mismo tienen siempre los archivos (nunca quitar archivos de los servidores de origen), pero no están disponibles para los usuarios y aplicaciones. A continuación, puede retirar los servidores en su comodidad.

![Captura de pantalla muestra un servidor listo para ser examinado](media/migrate/inventory.png)
**figura 2: Servicio de migración de almacenamiento para realizar un inventario de servidores**

## <a name="requirements"></a>Requisitos

Para usar el servicio de migración de almacenamiento, necesita lo siguiente:

- Un **servidor de origen** para migrar archivos y datos de
- Un **servidor de destino** ejecuta Windows Server 2019 para migrar a: Windows Server 2016 y Windows Server 2012 R2 también funcionan correctamente, pero son aproximadamente el 50% más lento
- Un **servidor orchestrator** ejecuta Windows Server 2019 para administrar la migración  <br>Si está migrando sólo unos pocos servidores y uno de los servidores se está ejecutando Windows Server 2019, puede usar como el orquestador. Si va a migrar más servidores, se recomienda usar un servidor independiente de orchestrator.
- Un **PC o servidor que ejecuta [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)**  para ejecutar la interfaz de usuario del servicio de migración de almacenamiento, a menos que prefiera usar PowerShell para administrar la migración. La versión de Windows Admin Center y Windows Server 2019 debe ser al menos la versión 1809. 

### <a name="security-requirements"></a>Requisitos de seguridad

- Una cuenta de migración que sea administrador en los equipos de origen.
- Una cuenta de migración que sea administrador en los equipos de destino.
- El equipo de orchestrator debe tener la regla de firewall (SMB de entrada) de compartir archivos e impresoras habilitada *entrante*.
- Los equipos de origen y destino deben tener las siguientes reglas de firewall habilitadas *entrante* (aunque puede que les habilitado):
  - Compartir archivos e impresoras (SMB de entrada)
  - Servicio Netlogon (NP-In)
  - Instrumental de administración de Windows (DCOM-In)
  - Instrumental de administración de Windows (WMI-In)
  
  > [!TIP]
  > Instalar automáticamente el servicio de Proxy de servicio de migración de almacenamiento en un equipo Windows Server 2019 abre los puertos de firewall necesarias en ese equipo.
- Si los equipos pertenecen a un dominio de Active Directory Domain Services, todas deben pertenecer al mismo bosque. El servidor de destino también debe ser en el mismo dominio que el servidor de origen, si desea transferir el nombre de dominio de origen al destino cuando actualice a través. Traslado técnicamente funciona entre dominios, pero el nombre de dominio completo del destino será diferente de origen...

### <a name="requirements-for-source-servers"></a>Requisitos para los servidores de origen

El servidor de origen debe ejecutar uno de los siguientes sistemas operativos:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

### <a name="requirements-for-destination-servers"></a>Requisitos para los servidores de destino

El servidor de destino debe ejecutar uno de los siguientes sistemas operativos:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Servidores de destino que ejecuta Windows Server 2019 tienen duplicaron el rendimiento de transferencia de las versiones anteriores de Windows Server. Esta mejora del rendimiento es debido a la inclusión de un servicio de proxy de servicio de migración de almacenamiento integrada, que también se abre el firewall es necesario abrir puertos si no están ya.

## <a name="see-also"></a>Vea también

- [Migrar un servidor de archivos mediante el servicio de migración de almacenamiento](migrate-data.md)
- [Preguntas más frecuentes (P+F) de servicios de migración de almacenamiento](faq.md)
- [Servicio de migración de almacenamiento problemas conocidos](known-issues.md)