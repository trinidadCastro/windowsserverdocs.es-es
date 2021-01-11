---
title: Instalación de Nano Server
description: Instalación limpia, actualización, migración y evaluación de Nano Server
manager: dougkim
ms.date: 09/06/2017
ms.topic: how-to
ms.assetid: 2c2fa45b-6f3b-4663-b421-2da6ecc463bf
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: c0666a974cdb36e689a729c748e7aa047dba585f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947571"
---
# <a name="install-nano-server"></a>Instalación de Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consulta [Cambios en Nano Server](nano-in-semi-annual-channel.md) para obtener más información.

Windows Server 2016 ofrece una nueva opción de instalación: Nano Server. Nano Server es un sistema operativo de servidor administrado de forma remota y optimizado para centros de datos y nubes privadas. Es similar a Windows Server en modo Server Core, pero mucho más pequeño; no tiene ninguna capacidad de inicio de sesión local y solo es compatible con agentes, herramientas y aplicaciones de 64 bits. Ocupa menos espacio en disco, se configura significativamente más rápido y requiere muchas menos actualizaciones y reinicios que Windows Server. Cuando se reinicia, lo hace mucho más rápido. La opción de instalación de Nano Server está disponible para las ediciones Standard y Datacenter de Windows Server 2016.

Nano Server es ideal para una serie de escenarios:

-   Como un host de proceso para las máquinas virtuales de Hyper-V, ya sea en clústeres o no

-   Como un host de almacenamiento para el servidor de archivos de escalabilidad horizontal

-   Como un servidor DNS

-   Como un servidor web que ejecuta Internet Information Services (IIS)

-   Como host para aplicaciones desarrolladas mediante patrones de aplicación en la nube y que se ejecutan en un contenedor o sistema operativo invitado de máquina virtual

## <a name="important-differences-in-nano-server"></a>Diferencias importantes en Nano Server

Debido a que Nano Server está optimizado como un sistema operativo ligero para ejecutar aplicaciones nativas en la nube basadas en contenedores y microservicios o como un host de centro de datos rápido y económico con muy poca huella, hay diferencias importantes en la instalación de Nano Server en comparación con las instalaciones de Server Core o Server con Experiencia de escritorio:

- Nano Server es un equipo sin periféricos; es decir, no hay interfaz de usuario gráfica ni capacidades de inicio de sesión local.
- Solo se admiten aplicaciones, herramientas y agentes de 64 bits.
- Nano Server no puede actuar como un controlador de dominio de Active Directory.
- No se admiten directivas de grupo. Sin embargo, puede usar [configuración de estado deseado](/previous-versions/dn387184(v=vs.85)) para aplicar opciones de configuración a escala.
- Nano Server no puede configurarse para utilizar un servidor proxy para acceder a Internet.
- No se admite la formación de equipos NIC (concretamente, el equilibrio de carga y la conmutación por error o LBFO). En su lugar, se admite Switch Embedded Teaming (SET).
- Microsoft Endpoint Configuration Manager y System Center Data Protection Manager no son compatibles.
- Los cmdlets Analizador de procedimientos recomendados (BPA) y la integración de BPA con el Administrador del servidor no son compatibles.
- Nano Server no admite adaptadores de bus host virtuales (HBA).
- No es necesario activar Nano Server con una clave de producto. Cuando Nano Server funciona como host de Hyper-V, no admite la [activación automática de máquina virtual](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) (AVMA). Las máquinas virtuales que se ejecutan en un host de Nano Server pueden activarse mediante el [Servicio de administración de claves](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj612867(v=ws.11)) (KMS) con una clave de licencia de volumen genérica o usando la [activación basada en Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502534(v=ws.11)).
- La versión de Windows PowerShell proporcionada con Nano Server tiene diferencias importantes. Para obtener más información, consulte [PowerShell en Nano Server](PowerShell-on-Nano-Server.md).
- Nano Server solo es compatible con el modelo de Rama actual para empresas (CBB), pues actualmente no hay ninguna versión de Rama de mantenimiento a largo plazo (LTSB). Consulte las siguientes subsección para obtener más información.

### <a name="current-branch-for-business"></a>Rama actual para empresas
Nano Server se provee con un modelo más activo, llamado Rama actual para empresas (CBB) para ofrecer compatibilidad con clientes que están migrando en una cadencia en la nube, con ciclos de desarrollo rápidos. En este modelo, las actualizaciones de características de Nano Server se publicarán de dos a tres veces al año. Este modelo requiere [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx) para Nano Server implementados y operados en producción. Para mantener la compatibilidad, los administradores no deben tener más de dos versiones atrasadas de CBB con respecto a la actual. Sin embargo, estas versiones no actualizan automáticamente las implementaciones existentes; los administradores deben realizar una instalación manual de las nuevas versiones de CBB según les convenga. Para obtener más información, consulte [Windows Server 2016 new Current Branch for Business servicing option](https://cloudblogs.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/) (Nueva opción de mantenimiento de Rama actual para empresas de Windows Server 2016).

Las opciones de instalación Server Core y Server con experiencia de escritorio todavía se proporcionan en el [modelo de Rama de mantenimiento a largo plazo (LTSB)](https://support.microsoft.com/lifecycle#gp%2Fgp_msl_policy), que comprende 5 años de soporte general y 5 años de soporte extendido.

## <a name="installation-scenarios"></a>Escenarios de instalación

### <a name="evaluation"></a>Evaluación
Puede obtener una copia de evaluación con una licencia para 180 días de Windows Server desde [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Para probar Nano Server, elige **Nano Server | opción EXE de 64 bits** y luego vuelve a [Inicio rápido de Nano Server](Nano-Server-Quick-Start.md) o a [Implementación de Nano Server](Deploy-Nano-Server.md) para empezar.

### <a name="clean-installation"></a>Instalación limpia
Dado que instala Nano Server mediante la configuración de un VHD, una instalación limpia es el método de implementación más rápido y fácil.

- Para empezar a trabajar rápidamente con una implementación básica de Nano Server usando DHCP para obtener una dirección IP, vea [Inicio rápido de Nano Server](Nano-Server-Quick-Start.md)
- Si ya está familiarizado con los aspectos básicos de Nano Server, los temas más detallados a partir de [Implementación de Nano Server](Deploy-Nano-Server.md) ofrecen un completo conjunto de instrucciones para personalizar las imágenes, trabajar con dominios, instalar paquetes para roles de servidor y otras características en línea y sin conexión y mucho más.

> [!IMPORTANT]
> Una vez completada la instalación e inmediatamente después de instalar todos los roles de servidor y las características que necesita, busque e instale las actualizaciones disponibles para Windows Server 2016. Para Nano Server, consulta la sección Administración de actualizaciones en Nano Server de [Administración de Nano Server](Manage-Nano-Server.md).

### <a name="upgrade"></a>Actualizar versión
Puesto que Nano Server es nuevo para Windows Server 2016, no hay una ruta de actualización desde versiones anteriores de sistema operativo a Nano Server.

### <a name="migration"></a>Migración
Puesto que Nano Server es nuevo para Windows Server 2016, no hay una ruta de migración desde versiones anteriores de sistema operativo a Nano Server.

-------------------------------------
Si necesita una opción de instalación diferente, puede ir [a la página principal de Windows Server 2016](../index.yml)