---
title: Administrar Windows Server
description: Aprende sobre herramientas, recomendaciones y orientación para administrar Windows Server
ms.prod: windows-server-threshold
ms.technology: manage
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 03/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4faadb811927626c26a5b01e2ce0598d40792b68
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066939"
---
# Administrar Windows Server

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Echa un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

 <ul class="cardse panelContent cols cols3">
    <li>
        <a href="https://docs.microsoft.com/windows-insider/at-work-pro/wip-4-biz-feedback-hub">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
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

## Administrar los sistemas y entornos de Windows Server
Las herramientas que puedes usar para administrar las instancias de Windows Server dependen, en gran medida, de los tipos de sistemas que hayas implementado (servidor de Windows con Experiencia de escritorio frente a Server Core), máquinas físicas frente a virtuales y donde se encuentran tus servidores. Utiliza la siguiente información para realizar tareas básicas de administración en Windows Server.

Usa la tabla siguiente para determinar qué herramientas usar en cada momento.

| Estoy haciendo lo siguiente   | Instalar y ejecutar Windows Admin Center | Ejecutar el Administrador de servidores en Windows Server | Ejecutar el Administrador de servidores en RSAT en Windows 10 |
|--------|----------------------|--------------------------------------|------------------------------------------|
| Sentado frente a un PC con Windows 10 | X  |                                      | X                                        |
| Sentado frente a un sistema Windows Server que ejecuta la experiencia de escritorio | X | X | X |
| Sentado frente a un sistema Windows Server que ejecuta Server Core |X (instalar en Windows 10, usar para administrar Server Core) | | X |
| Sentado lejos de mi sistema Windows Server |X | | X |
| Sentado lejos de mi sistema de Windows Server, pero TIENE experiencia de escritorio |X | Usar RDS para conectar remotamente con el servidor y, a continuación, usar el Administrador de servidores. | X |

Además de las herramientas que se mencionan a continuación, también puedes usar [Servicios de Escritorio remoto](../remote/remote-desktop-services/welcome-to-rds.md) para acceder a los servidores locales, remotos y virtuales. A continuación, puedes usar el Administrador de servidores para realizar tareas de administración.

### Administrar en sistemas locales, sistemas remotos y sistemas sin interfaz de usuario con Windows Admin Center
[Windows Admin Center](../manage/windows-admin-center/overview.md) es una aplicación de administración basada en explorador que permite la administración local de los servidores de Windows sin ninguna dependencia de Azure o la nube. Windows Admin Center te ofrece el control total sobre todos los aspectos de la infraestructura de servidores y es especialmente útil para la administración de las redes privadas que no están conectadas a Internet. Puedes instalar Windows Admin Center en Windows 10, en un servidor de puerta de enlace o directamente en el sistema de Windows Server que deseas administrar.

>[!NOTE]
>Windows Admin Center es el nombre oficial de lo que denominábamos "Proyecto Honolulu".

### Administrar sistemas locales con el Administrador de servidores
[Administrador de servidores](server-manager/server-manager.md) es una consola de administración incluida en la instalación completa de Windows Server. (No está disponible para las instalaciones que no tienen interfaz de usuario: Server Core no incluye el Administrador de servidores). Usa el Administrador de servidores para instalar y quitar roles de servidor, agregar y quitar servidores remotos, iniciar y detener los servicios y ver datos recopilados sobre tu entorno.

### Administrar sistemas remotos y sistemas sin interfaz de usuario con las herramientas de administración de servidores remotos (RSAT)
Si tu entorno incluye instalaciones de Server Core o servidores remotos (ya sea localmente o en máquinas virtuales), puedes usar las [herramientas de administración de servidores remotos (RSAT)](../remote/remote-server-administration-tools.md) para administrar esos sistemas. RSAT incluye el Administrador de servidores, para que puedas usarlo para administrar todos los servidores.

> [!IMPORTANT]
> RSAT se ejecuta en Windows 10. No puedes instalar RSAT en Windows Server Core.

También puedes administrar instalaciones de Server Core desde la línea de comandos. Consulta [Tareas de administración básicas en Server Core](server-core/server-core-administer.md).

### Administrar las actualizaciones de sistemas Windows Server
Puedes usar [Windows Server Update Services (WSUS)](windows-server-update-services/get-started/windows-server-update-services-wsus.md) para administrar e implementar actualizaciones de los sistemas en tu entorno de Windows Server.

## Recopilar información sobre tu entorno
Muchas de las decisiones como administrador dependen de datos sobre los sistemas y los usuarios de tu entorno. Usa la siguiente información y las siguientes herramientas para recopilar esos datos.

Comienza con [Configurar los datos de diagnóstico de Windows en la organización](/windows/configuration/configure-windows-diagnostic-data-in-your-organization) para obtener información sobre los datos de diagnóstico que pueden recopilarse de Windows 10 y Windows Server.

### [Recopilación de eventos de configuración y arranque](get-started-with-setup-and-boot-event-collection.md)
Recopilación de eventos de configuración y arranque te permite designar un equipo "recopilador" que puede recopilar diversos eventos importantes que se producen en otros ordenadores cuando arrancan o pasan por el proceso de instalación. Puedes analizar, más adelante, los eventos recopilados con Visor de eventos, Analizador de mensajes, Wevtutil o cmdlets de Windows PowerShell. 

### [Registro de inventario de software (SIL)](software-inventory-logging/get-started-with-software-inventory-logging.md)

El registro de inventario de software de Windows Server es una función dotada de un sencillo conjunto de cmdlets de PowerShell que ayuda a los administradores de servidores a recuperar una lista del software de Microsoft instalado en sus servidores. También proporciona la capacidad de recopilar y reenviar estos datos periódicamente a través de la red a un servidor web de destino, mediante el protocolo HTTPS, para la agregación. También se usan comandos de PowerShell para administrar la función, en particular para recopilar datos cada hora y reenviarlos.

### [Registro de acceso de usuarios (UAL)](user-access-logging/get-started-with-user-access-logging.md)

El registro de acceso de usuarios agrega eventos únicos de dispositivos de clientes y de solicitudes de usuarios que se registran en un ordenador que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 a una base de datos local. Después, estos registros se ponen a disposición de los usuarios (a través de una consulta realizada por un administrador del servidor) para recuperar cantidades e instancias por rol de servidor, por usuario, por dispositivo, por el servidor local y por fecha. Además, UAL permite también que los desarrolladores de software que no es de Microsoft instrumenten sus eventos de UAL a agregar. 

## Ajustar el entorno de Windows Server para conseguir rendimiento
Usa la siguiente información para ayudar a ajustar tu entorno para conseguir rendimiento.

### [Directrices de ajuste de rendimiento](performance-tuning/index.md)
Revisa un conjunto de directrices que puedes usar para ajustar la configuración del servidor en Windows Server 2016 y obtener ganancias de rendimiento incremental o eficiencia energética, especialmente cuando la naturaleza de la carga de trabajo varía poco con el tiempo.

### [Microsoft Server Performance Advisor](server-performance-advisor/microsoft-server-performance-advisor.md)

Con Microsoft Server Performance Advisor (SPA), puede recopilar métricas para diagnosticar problemas de rendimiento en los servidores de Windows discretamente, sin agregar agentes de software ni volver a configurar los servidores de producción. SPA genera completos informes de rendimiento y gráficos históricos con recomendaciones.


## Automatiza la administración de Windows Server

Windows Server incluye un conjunto de comandos y módulos de Windows PowerShell que puedes usar para automatizar tareas de administración.

### [Windows PowerShell](/powershell/scripting/powershell-scripting?view=powershell-5.1)
Windows PowerShell es un lenguaje de shell de línea de comandos y de scripting diseñado para permitirte automatizar rápidamente tareas administrativas. 

### [Comandos de Windows](windows-commands/windows-commands.md)

Las herramientas de línea de comandos de Windows se usan para realizar tareas administrativas en Windows. Puedes usar la referencia de comandos para familiarizarte con las herramientas de línea de comandos, para obtener información sobre el shell de comandos y para automatizar tareas de línea de comandos usando archivos por lotes o herramientas de scripting.

## Windows Server Insider Preview
### [Información del sistema](..\manage\system-insights\overview.md)
Información del sistema es una característica nueva que presenta a Windows Server análisis predictivo de forma nativa. Estas capacidades predictivas localmente analizan datos del sistema de Windows Server, como contadores de rendimiento o eventos ETW y ayudan a los administradores de TI de forma proactiva a detectar y abordar el comportamiento problemático en sus implementaciones. 
