---
title: Management
description: Obtén información sobre herramientas, recomendaciones y orientación para administrar Windows Server
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: manage
ms.topic: landing-page
author: lizap
ms.author: elizapo
ms.localizationpriority: high
ms.openlocfilehash: e6a5357e3e33b3d3318a3e281bbb5c80be842155
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "63721375"
---
# <a name="management"></a>Management


>[!TIP]
> ¿Busca información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puede [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<hr />

Una vez que hayas implementado Windows Server en tu entorno, incluyendo los roles específicos para las características y funciones que necesitas, el siguiente paso es la administración de esos servidores. Windows Server incluye una serie de herramientas para ayudarte a comprender el entorno de Windows Server, administrar servidores específicos, ajustar con precisión el rendimiento y finalmente automatizar muchas tareas de administración. 

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

<HR />

<ul class="cardsI panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Administrar los sistemas y entornos de Windows Server</h3>
<HR />
                        <p><h3><a href="../manage/windows-admin-center/overview.md">Administrar en sistemas locales, sistemas remotos y sistemas sin interfaz de usuario con Windows Admin Center</a></h3>Una aplicación de administración basada en explorador que permite la administración local de Windows Servers sin ninguna dependencia de Azure o la nube. Windows Admin Center (anteriormente denominado "Proyecto Honolulu") ofrece un control completo sobre todos los aspectos de la infraestructura de servidor y es especialmente útil para la administración de redes privadas que no están conectadas a Internet. Puedes instalar Windows Admin Center en Windows 10, en un servidor de puerta de enlace o directamente en el sistema de Windows Server que deseas administrar.</p>
<HR />
                        <p><h3><a href="server-manager/server-manager.md">Administrar sistemas locales con el Administrador de servidores</a></h3>Una consola de administración incluida en la instalación completa de Windows Server. (No está disponible para las instalaciones que no tienen interfaz de usuario. Server Core no incluye el administrador del servidor). Use el Administrador del servidor para instalar y quitar roles de servidor, agregar y quitar servidores remotos, iniciar y detener los servicios y ver los datos recopilados sobre el entorno.</p>
<HR />
                        <p><h3><a href="../remote/remote-server-administration-tools.md">Administrar sistemas remotos y sistemas sin interfaz de usuario con las herramientas de administración de servidores remotos (RSAT)</a></h3>Si tu entorno incluye instalaciones de Server Core o servidores remotos (en local o máquinas virtuales), puedes usar RSAT para administrar esos sistemas. RSAT incluye el Administrador de servidores, para que puedas usarlo para administrar todos los servidores. Ten en cuenta que RSAT se ejecuta en Windows 10. No puedes instalar RSAT en Windows Server Core. También puedes administrar instalaciones básicas desde la línea de comandos. Consulta <a href="server-core/server-core-administer.md">Tareas de administración básicas en Server Core</a>
<HR />
                        <p><h3><a href="windows-server-update-services/get-started/windows-server-update-services-wsus.md">Administrar las actualizaciones de sistemas Windows Server</a></h3>Usa Windows Server Update Services (WSUS) para administrar e implementar actualizaciones en los sistemas de tu entorno de Windows Server.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Recopilar información sobre tu entorno</h3>
<HR />
                        <p><h3><a href="get-started-with-setup-and-boot-event-collection.md">Recopilación de eventos de configuración y arranque</a></h3>Recopilación de eventos de configuración y arranque te permite designar un equipo "recopilador" que puede recopilar diversos eventos importantes que se producen en otros ordenadores cuando arrancan o pasan por el proceso de instalación. Puedes analizar, más adelante, los eventos recopilados con Visor de eventos, Analizador de mensajes, Wevtutil o cmdlets de Windows PowerShell. </p>
<HR />
                        <p><h3><a href="software-inventory-logging/get-started-with-software-inventory-logging.md">Registro de inventario de software (SIL)</a></h3>El registro de inventario de software de Windows Server es una característica dotada de un sencillo conjunto de cmdlets de PowerShell que ayuda a los administradores de servidores a recuperar una lista del software de Microsoft instalado en sus servidores. También proporciona la capacidad de recopilar y reenviar estos datos periódicamente a través de la red a un servidor web de destino, mediante el protocolo HTTPS, para la agregación. También se usan comandos de PowerShell para administrar la característica, en particular para recopilar datos cada hora y reenviarlos.</p>
<HR />
                        <p><h3><a href="user-access-logging/get-started-with-user-access-logging.md">Registro de acceso de usuarios (UAL)</a></h3>El registro de acceso de usuarios agrega eventos únicos de dispositivos de clientes y de solicitudes de usuarios que se registran en un ordenador que ejecuta Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 a una base de datos local. Después, estos registros se ponen a disposición de los usuarios (a través de una consulta realizada por un administrador del servidor) para recuperar cantidades e instancias por rol de servidor, por usuario, por dispositivo, por el servidor local y por fecha. Además, UAL permite también que los desarrolladores de software que no es de Microsoft instrumenten sus eventos de UAL a agregar. </a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Ajustar el entorno de Windows Server para conseguir rendimiento</h3>
<HR />
                        <p><h3><a href="performance-tuning/index.md">Directrices de ajuste del rendimiento</a></h3>Revisa un conjunto de directrices que puedes usar para ajustar la configuración del servidor en Windows Server y obtener ganancias de rendimiento incremental o eficiencia energética, especialmente cuando la naturaleza de la carga de trabajo varía poco con el tiempo.</p>
<HR />
                        <p><h3><a href="server-performance-advisor/microsoft-server-performance-advisor.md">Microsoft Server Performance Advisor</a></h3>Con Microsoft Server Performance Advisor (SPA), puede recopilar métricas para diagnosticar problemas de rendimiento en los servidores de Windows discretamente, sin agregar agentes de software ni volver a configurar los servidores de producción. SPA genera completos informes de rendimiento y gráficos históricos con recomendaciones.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatizar la administración de Windows Server</h3>
<HR />
                        <p><h3><a href="https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-5.1">Windows PowerShell</a></h3>Windows PowerShell es un lenguaje de shell de línea de comandos y de scripting diseñado para permitirte automatizar rápidamente tareas administrativas. </p>
<HR />
                        <p><h3><a href="windows-commands/windows-commands.md">Comandos de Windows</a></h3>Las herramientas de línea de comandos de Windows se usan para realizar tareas administrativas en Windows. Puedes usar la referencia de comandos para familiarizarte con las herramientas de línea de comandos, para obtener información sobre el shell de comandos y para automatizar tareas de línea de comandos usando archivos por lotes o herramientas de scripting.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-manage.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3>Automatizar la administración de Windows Server</h3>
<HR />
                        <p><h3><a href="..\manage\system-insights\overview.md">Conclusiones del sistema</h3></a>Los análisis predictivos nativos analizan localmente datos del sistema de Windows Server, como los contadores de rendimiento y eventos ETW, ayudando a los administradores de TI a detectar y abordar de forma proactiva el comportamiento problemático en las implementaciones.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>