---
title: Administrar Windows Server
description: Información sobre herramientas, recomendaciones e instrucciones sobre la administración de Windows Server
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2edfddb644a2c92a05be8d2d943c089bf6774d06
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879328"
---
# <a name="manage-windows-server"></a>Administrar Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

>[!TIP]
> ¿Busca información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puede [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="manage icon" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h2>Administrar</h2>
                <p>Una vez que hayas implementado Windows Server en tu entorno, incluyendo los roles específicos para las características y funciones que necesitas, el siguiente paso es la administración de esos servidores. Windows Server incluye una serie de herramientas para ayudarte a comprender el entorno de Windows Server, administrar servidores específicos, ajustar con precisión el rendimiento y finalmente automatizar muchas tareas de administración. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
</ul>

## <a name="manage-windows-server-systems-and-environments"></a>Administrar los sistemas y entornos de Windows Server
Las herramientas que puedes usar para administrar las instancias de Windows Server dependen, en gran medida, de los tipos de sistemas que hayas implementado (servidor de Windows con Experiencia de escritorio frente a Server Core), máquinas físicas frente a virtuales y donde se encuentran tus servidores. Utiliza la siguiente información para realizar tareas básicas de administración en Windows Server.

Usa la tabla siguiente para determinar qué herramientas usar en cada momento.

| Estoy haciendo lo siguiente   | Instalar y ejecutar Windows Admin Center | Ejecutar el Administrador de servidores en Windows Server | Ejecutar el Administrador de servidores en RSAT en Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Sentado frente a un PC con Windows 10 | X  |                                      | X                                        |
| Sentado frente a un sistema Windows Server que ejecuta la experiencia de escritorio | X | X | X |
| Sentado frente a un sistema Windows Server que ejecuta Server Core |X (instalar en Windows 10, usar para administrar Server Core) | | X |
| Sentado lejos de mi sistema Windows Server |X | | X |
| Sentado lejos de mi sistema de Windows Server, pero TIENE experiencia de escritorio |X | Usar RDS para conectar remotamente con el servidor y, a continuación, usar el Administrador de servidores | X |

Además de las herramientas que se mencionan a continuación, también puedes usar [Servicios de Escritorio remoto](../remote/remote-desktop-services/welcome-to-rds.md) para acceder a los servidores locales, remotos y virtuales. A continuación, puedes usar el Administrador de servidores para realizar tareas de administración.

### <a name="manage-on-premises-systems-remote-systems-and-systems-without-ui-with-windows-admin-center"></a>Administrar en sistemas locales, sistemas remotos y sistemas sin interfaz de usuario con Windows Admin Center
El [centro de administración de Windows](../manage/windows-admin-center/overview.md) es una aplicación de administración basada en explorador que permite la administración local de servidores de Windows sin dependencias de Azure o de la nube. El centro de administración de Windows ofrece control total sobre todos los aspectos de la infraestructura de servidor y es especialmente útil para la administración en redes privadas que no están conectadas a Internet. Puedes instalar Windows Admin Center en Windows 10, en un servidor de puerta de enlace o directamente en el sistema de Windows Server que deseas administrar.

>[!NOTE]
>El centro de administración de Windows es el nombre oficial de lo que se usa para llamar a "Project Honolulu".

### <a name="manage-on-premises-systems-with-server-manager"></a>Administrar sistemas locales con el Administrador de servidores
[Administrador del servidor](server-manager/server-manager.md) es una consola de administración incluida en la instalación completa de Windows Server. (No está disponible para las instalaciones que no tienen interfaz de usuario. Server Core no incluye el administrador del servidor). Use el Administrador del servidor para instalar y quitar roles de servidor, agregar y quitar servidores remotos, iniciar y detener los servicios y ver los datos recopilados sobre el entorno.

### <a name="manage-remote-systems-and-systems-without-ui-with-remote-server-administration-tools-rsat"></a>Administrar sistemas remotos y sistemas sin interfaz de usuario con las herramientas de administración de servidores remotos (RSAT)
Si su entorno incluye instalaciones de Server Core o servidores remotos (ya sean locales o máquinas virtuales), puede usar las [herramientas de administración remota del servidor (RSAT)](../remote/remote-server-administration-tools.md) para administrar esos sistemas. RSAT incluye el Administrador de servidores, para que puedas usarlo para administrar todos los servidores.

> [!IMPORTANT]
> RSAT se ejecuta en Windows 10. No puedes instalar RSAT en Windows Server Core.

También puedes administrar instalaciones básicas desde la línea de comandos. Consulte [tareas de administración básicas en Server Core](server-core/server-core-administer.md).

### <a name="manage-updates-to-windows-server-systems"></a>Administrar las actualizaciones de sistemas Windows Server
Puede usar [Windows Server Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) para administrar e implementar actualizaciones en los sistemas de su entorno de Windows Server.

## <a name="gather-information-about-your-environment"></a>Recopilar información sobre tu entorno
Muchas de las decisiones que tome como administrador dependen de los datos de los sistemas y los usuarios de su entorno. Utilice la información y las herramientas siguientes para recopilar los datos.

Comience con la configuración de los [datos de diagnóstico de Windows en su organización](/windows/configuration/configure-windows-diagnostic-data-in-your-organization) para obtener información sobre los datos de diagnóstico que se pueden recopilar de Windows 10 y Windows Server.

### <a name="setup-and-boot-event-collection"></a>[Recopilación de eventos de configuración y arranque](get-started-with-setup-and-boot-event-collection.md)
Recopilación de eventos de configuración y arranque te permite designar un equipo "recopilador" que puede recopilar diversos eventos importantes que se producen en otros ordenadores cuando arrancan o pasan por el proceso de instalación. Puedes analizar, más adelante, los eventos recopilados con Visor de eventos, Analizador de mensajes, Wevtutil o cmdlets de Windows PowerShell.

### <a name="software-inventory-logging-sil"></a>[Registro de inventario de software (SIL)](software-inventory-logging/get-started-with-software-inventory-logging.md)

El registro de inventario de software de Windows Server es una característica dotada de un sencillo conjunto de cmdlets de PowerShell que ayuda a los administradores de servidores a recuperar una lista del software de Microsoft instalado en sus servidores. También proporciona la capacidad de recopilar y reenviar estos datos periódicamente a través de la red a un servidor web de destino, mediante el protocolo HTTPS, para la agregación. También se usan comandos de PowerShell para administrar la característica, en particular para recopilar datos cada hora y reenviarlos.

### <a name="user-access-logging-ual"></a>[Registro de acceso de usuarios (UAL)](user-access-logging/get-started-with-user-access-logging.md)

El registro de acceso de usuarios agrega eventos únicos de dispositivos de clientes y de solicitudes de usuarios que se registran en un ordenador que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 a una base de datos local. Después, estos registros se ponen a disposición de los usuarios (a través de una consulta realizada por un administrador del servidor) para recuperar cantidades e instancias por rol de servidor, por usuario, por dispositivo, por el servidor local y por fecha. Además, UAL permite también que los desarrolladores de software que no es de Microsoft instrumenten sus eventos de UAL a agregar.

## <a name="tune-your-windows-server-environment-for-performance"></a>Ajustar el entorno de Windows Server para conseguir rendimiento
Use la siguiente información como ayuda para optimizar el rendimiento del entorno.

### <a name="performance-tuning-guidelines"></a>[Directrices de ajuste del rendimiento](performance-tuning/index.md)
Revise un conjunto de instrucciones que puede usar para optimizar la configuración del servidor en Windows Server 2016 y obtener mejoras de rendimiento incremental o de eficacia energética, especialmente cuando la naturaleza de la carga de trabajo varía ligeramente con el tiempo.

### <a name="microsoft-server-performance-advisor"></a>[Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

Con Microsoft Server Performance Advisor (SPA), puede recopilar métricas para diagnosticar problemas de rendimiento en los servidores de Windows discretamente, sin agregar agentes de software ni volver a configurar los servidores de producción. SPA genera completos informes de rendimiento y gráficos históricos con recomendaciones.


## <a name="automate-windows-server-management"></a>Automatizar la administración de Windows Server

Windows Server incluye un conjunto de comandos y módulos de Windows PowerShell que puede usar para automatizar las tareas de administración.

### <a name="windows-powershell"></a>[Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
Instalador PowerShell es un shell de línea de comandos y un lenguaje de scripting diseñado para permitirle automatizar rápidamente las tareas administrativas.

### <a name="windows-commands"></a>[Comandos de Windows](windows-commands/windows-commands.md)

Las herramientas de línea de comandos de Windows se usan para realizar tareas administrativas en Windows. Puedes usar la referencia de comandos para familiarizarte con las herramientas de línea de comandos, para obtener información sobre el shell de comandos y para automatizar tareas de línea de comandos usando archivos por lotes o herramientas de scripting.

## <a name="windows-server-insider-preview"></a>Windows Server Insider Preview
### <a name="system-insights"></a>[Conclusiones del sistema](../manage/system-insights/overview.md)
System Insights es una nueva característica que introduce análisis predictivos de forma nativa en Windows Server. Estas capacidades predictivas analizan de forma local los datos del sistema de Windows Server, como los contadores de rendimiento o los eventos de ETW, que ayudan a los administradores de ti a detectar y solucionar de forma proactiva el comportamiento problemático en sus implementaciones.
