---
redirect_url: /windows-server/windows-server
ms.openlocfilehash: 6f6e0d21fdf43ce3cf9f713d5731cfea5bb069de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812276"
---
# <a name="windows-server-2016"></a>Windows Server 2016

En esta biblioteca se ofrece información a los profesionales de TI para evaluar, planificar, implementar, proteger y administrar Windows Server 2016.

> [!Note] 
> La próxima versión de Windows Server está cambiando. Puedes encontrar información detallada sobre las novedades si visitas [Introducción al Canal semianual de Windows Server](./get-started/semi-annual-channel-overview.md). 

[![Windows Server 2016 Overview Video](media/front-page-video.png)](https://www.youtube-nocookie.com/embed/V8oF0JpDzaM)

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/what-s-new-in-windows-server-2016">
        <img height=145 src="media/whats-new-highlight.png" alt="What's new icon" title="Novedades en Windows Server 16"/></a>
        <br/>Novedades
    </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/get-started/server-basics">
        <img height=145 src="media/1-getstarted.png" alt="get started icon" title="Introducción a Windows Server 16" /></a>
      <br/>Comenzar </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/administration/index">
        <img height=145 src="media/8-management.png" alt="administer icon" title="Administración de Windows Server" /></a>
      <br/>Administración </td>
    <td align='center' style="width:25%; border:0;">
      <a href="/windows-server/failover-clustering/failover-clustering-overview">
        <img height=145 src="media/3-failover.png" alt="Failover clustering icon" title="Clústeres de conmutación por error de Windows Server" /></a>
      <br/>Clústeres de conmutación por error </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/identity/identity-and-access">
        <img height=145 src="media/4-identity.png" alt="Identity and access icon" title="Identidad y acceso de Windows Server" /></a>
      <br>Identidad y acceso </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/networking/networking">
        <img height=145 src="media/6-networking.png" alt="Networking icon" title="Redes en Windows Server" />
        </a>
      <br/>Funciones de red </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/remote/index">
        <img height=145 src="media/remote.png" alt="remote icon" title="Administración de servidor y acceso remoto" />
        </a>
      <br/>Acceso remoto </td>
    <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/security/security-and-assurance">
        <img height=145 src="media/5-security.png" alt="Security icon" title="Seguridad y control de Windows Server" />
      </a>
      <br/>Seguridad y control </td>
  </tr>
  <tr style="text-align:center;">
    <td align='center' style="width:25%; border:0;">&nbsp;</td>
    <td align='center' style="width:25%; border:0;"><br>
      <a href="/windows-server/storage/storage">
        <img height=145 src="media/7-storage.png" alt="Storage icon" title="Almacenamiento de Windows Server" />
      </a>
      <br/>Almacenamiento </td>
   <td align='center' style="width:25%; border:0;"><br/>
      <a href="/windows-server/virtualization/virtualization">
        <img height=145 src="media/virtualization.png" alt="virtualization icon" title="Virtualización de Windows Server" /></a>
      <br/>Virtualización </td>
    <td align='center' style="width:25%; border:0;">&nbsp; </td>
  </tr>
</table>

<br/>

> [!Note] 
> Para experimentar de primera mano nuevas características y funciones disponibles en Windows Server 2016, puede descargar una versión de evaluación; para ello, visite [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). 


## <a name="windows-server-2016-editions"></a>Ediciones de Windows Server 2016

Windows Server 2016 está disponible en las ediciones Standard, Datacenter y Essentials. Windows Server 2016 Datacenter incluye derechos ilimitados de virtualización más nuevas características para crear un centro de datos definido por el software. Windows Server 2016 Standard ofrece características de clase empresarial con derechos limitados de virtualización. Windows Server Essentials es un primer servidor ideal conectado a la nube. Tiene su propia [documentación amplia](https://go.microsoft.com/fwlink/?LinkID=827171): el contenido de aquí se centra en las ediciones Standard y Datacenter. La siguiente tabla resume brevemente las diferencias clave entre las ediciones Standard y Datacenter:

|Característica|Datacenter|Standard|  
|-------------------|----------|-----------------------|  
|Funcionalidad básica de Windows Server| sí| sí|
|Contenedores de Hyper-V/OSE|sin límite|   2|
|Contenedores de Windows Server|sin límite|   sin límite|
|Servicio de protección de host| sí| sí|
|Opción de instalación Nano Server| sí| sí|
|Características de almacenamiento, incluidos Espacios de almacenamiento directo y Réplica de almacenamiento| sí| no|
|Máquinas virtuales blindadas| sí| no|
|Infraestructura de red definida por el software (controlador de red, equilibrador de carga de software y puerta de enlace de varios inquilinos)| sí| no|

Para obtener más información, vea [Pricing and licensing for Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server-pricing) (Precios y licencias de Windows Server 2016) y [Compare features in Windows Server versions](https://www.microsoft.com/en-us/cloud-platform/windows-server-comparison) (Comparación de características de versiones de Windows Server).

## <a name="installation-options"></a>Opciones de instalación

Las ediciones Standard y Datacenter ofrecen tres opciones de instalación:

- **Server Core:** reduce el espacio necesario en el disco, la superficie expuesta a ataques potenciales y, especialmente, los requisitos de mantenimiento. Se trata de la **recomienda** a menos que tenga una necesidad concreta de elementos de la interfaz de usuario adicionales y de las herramientas de administración gráfica.
- **Servidor con Experiencia de escritorio:** instala la interfaz de usuario estándar y todas las herramientas, incluidas las características de experiencia de cliente que requieren una instalación independiente de Windows Server 2012 R2. Los roles y características de servidor se instalan con Administrador del servidor o con otros métodos.
- **Nano Server:** es un sistema operativo de servidor administrado de forma remota y optimizado para centros de datos y nubes privadas. Es similar a Windows Server en modo Server Core, pero mucho más pequeño; no tiene ninguna capacidad de inicio de sesión local y solo es compatible con agentes, herramientas y aplicaciones de 64 bits. Ocupa menos espacio en disco, se configura significativamente más rápido y requiere muchas menos actualizaciones y reinicios que las otras opciones.

>[!Note]
> A diferencia de algunas versiones anteriores de Windows Server, no se puede convertir entre Server Core y Servidor con Experiencia de escritorio tras la instalación. Por ejemplo, si se instala Server Core y más adelante se decide usar Servidor con Experiencia de escritorio, será necesario realizar una instalación nueva (y viceversa).


Ahora que sabe qué opción de instalación y qué edición son adecuadas, haga clic a continuación para empezar a trabajar con Windows Server 2016.
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:33%; border:0;">
      <a  href="/windows-server/get-started/getting-started-with-nano-server"> <img width="175" src="media/nano.png" alt="Icon representing Nano server" title="Nano Server: menor peso" /><br/>Nano Server: <br/>Menor peso</a>
    </td>
    <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-core"> <img width="175" src="media/servercore.png" alt="Icon representing the Server Core installation" title="Server Core: recomendado" /><br/>Server Core: <br/>Recomienda</a></td>
   <td align='center' style="width:33%; border:0;"><a href="/windows-server/get-started/getting-started-with-server-with-desktop-experience"><img width="175" src="media/desktop.png" alt="Icon representing the full desktop experience installation option for Windows Server" title="Experiencia de escritorio: experiencia completa" /><br/>Experiencia de escritorio: <br/>Toda la interfaz</a></td>
  </tr>
</table>

## <a name="windows-server-software-defined-datacenter-sddc"></a>Centro de datos definido por software de Windows Server (SDDC)

Las tecnologías de almacenamiento, redes, seguridad y administración virtualizadas son los bloques de creación del Centro de datos definido por software de Windows Server (SDDC).
<br/>
<br/>

<table border="0" width="100%" align='center'>
  <tr style="text-align:center;">
    <td align='center' style="width:10%; border:0;"></td>
    <td align='center' style="width:50%; border:0;"><a href="/windows-server/sddc"><img width="400" src="media/sddc/WS16-heading.png" alt="Icon representing SDDC" title="Centro de datos definido por software de Windows Server (SDDC)" /><br/>Windows Server definido por Software Datacenter (SDDC)</a></td>
    <td align='center' style="width:10%; border:0;"></td>
  </tr>
</table>
