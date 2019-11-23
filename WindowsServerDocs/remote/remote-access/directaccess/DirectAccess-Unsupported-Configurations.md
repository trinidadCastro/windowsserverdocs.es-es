---
title: Configuraciones no compatibles de DirectAccess
description: En este tema se proporciona una lista de las configuraciones de DirectAccess no compatibles en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e652083d4accf90b542a16d51e314299303954f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388843"
---
# <a name="directaccess-unsupported-configurations"></a>Configuraciones no compatibles de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Revise la siguiente lista de configuraciones de DirectAccess no admitidas antes de comenzar la implementación para evitar tener que volver a iniciar la implementación.  

## <a name="bkmk_frs"></a>Distribución del servicio de replicación de archivos (FRS) de objetos de directiva de grupo (replicaciones de SYSVOL)  
No implemente DirectAccess en entornos en los que los controladores de dominio ejecuten el servicio de replicación de archivos (FRS) para la distribución de objetos de directiva de grupo (replicaciones de SYSVOL). La implementación de DirectAccess no se admite cuando se usa FRS.  
  
Usa FRS si tiene controladores de dominio que ejecutan Windows Server 2003 o Windows Server 2003 R2. Además, es posible que esté usando FRS si anteriormente usó controladores de dominio de Windows 2000 Server o Windows Server 2003 y nunca migró replicación de SYSVOL de FRS a Sistema de archivos distribuido replicación (DFS-R).  
  
Si implementa DirectAccess con replicación SYSVOL de FRS, corre el riesgo de la eliminación accidental de los objetos de directiva de grupo de DirectAccess que contienen la información de configuración del cliente y el servidor de DirectAccess. Si se eliminan estos objetos, la implementación de DirectAccess sufrirá una interrupción y los equipos cliente que usen DirectAccess no podrán conectarse a la red.  
  
Si planea implementar DirectAccess, debe usar controladores de dominio que ejecuten sistemas operativos posteriores a Windows Server 2003 R2 y debe usar DFS-R.  
  
Para obtener información acerca de la migración de FRS a DFS-R, consulte la [Guía de migración de replicación de SYSVOL: FRS to replicación DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Protección de acceso a redes para clientes de DirectAccess  
La protección de acceso a redes (NAP) se usa para determinar si los equipos cliente remotos cumplen las directivas de ti antes de que se les conceda acceso a la red corporativa. NAP quedó en desuso en Windows Server 2012 R2 y no se incluye en Windows Server 2016. Por esta razón, no se recomienda iniciar una nueva implementación de DirectAccess con NAP. Se recomienda un método diferente de control de punto final para la seguridad de los clientes de DirectAccess.  
  
## <a name="bkmk_multi"></a>Compatibilidad con multisitio para clientes de Windows 7  
Cuando DirectAccess se configura en una implementación multisitio, los clientes de Windows 10&reg;, Windows&reg; 8,1 y Windows&reg; 8 tienen la capacidad de conectarse al sitio más cercano.  Los equipos cliente de Windows 7&reg; no tienen la misma capacidad. La selección de sitio para los clientes de Windows 7 se establece en un sitio determinado en el momento de la configuración de la Directiva, y estos clientes siempre se conectarán a ese sitio designado, independientemente de su ubicación.  
  
## <a name="bkmk_user"></a>Control de acceso basado en usuario  
Las directivas de DirectAccess están basadas en equipos, no en función del usuario. No se admite la especificación de directivas de usuario de DirectAccess para controlar el acceso a la red corporativa.  
  
## <a name="bkmk_policy"></a>Personalización de la Directiva de DirectAccess  
DirectAccess se puede configurar mediante el Asistente para configuración de DirectAccess, la consola de administración de acceso remoto o los cmdlets de acceso remoto de Windows PowerShell. No se admite el uso de ningún medio que no sea el Asistente para configuración de DirectAccess para configurar DirectAccess, como la modificación de objetos de directiva de grupo de DirectAccess directamente o la modificación manual de la configuración de directivas predeterminada en el servidor o cliente. Estas modificaciones pueden dar lugar a una configuración inutilizable.  
  
## <a name="bkmk_kerb"></a>Autenticación de KerbProxy  
Al configurar un servidor de DirectAccess con el Asistente para Introducción, el servidor de DirectAccess se configura automáticamente para usar la autenticación KerbProxy para la autenticación de equipos y usuarios. Por este motivo, solo debe usar el Asistente para Introducción para implementaciones de sitio único donde solo se implementan clientes de Windows 10&reg;, Windows 8.1 o Windows 8.  
  
Además, las siguientes características no deben usarse con la autenticación de KerbProxy:  
  
-   Equilibrio de carga mediante un equilibrador de carga externo o carga de Windows   
    Equilibrador  
  
-   Autenticación en dos fases en la que se requieren tarjetas inteligentes o una contraseña de un solo tiempo (OTP)  
  
No se admiten los planes de implementación siguientes si habilita la autenticación de KerbProxy:  
  
-   Multisitio.  
  
-   Compatibilidad de DirectAccess con clientes de Windows 7.  
  
-   Tunelización forzada. Para asegurarse de que la autenticación de KerbProxy no está habilitada cuando se usa el túnel forzado, configure los siguientes elementos mientras se ejecuta el asistente:  
  
    -   Habilitar el túnel forzado  
  
    -   Habilitación de DirectAccess para clientes de Windows 7  
  
> [!NOTE]  
> En el caso de las implementaciones anteriores, debe usar el Asistente para configuración avanzada, que usa una configuración de dos túneles con un equipo basado en certificados y la autenticación de usuario. Para obtener más información, vea [implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Usar ISATAP  
ISATAP es una tecnología de transición que proporciona conectividad IPv6 en redes corporativas solo IPv4. Está limitado a organizaciones pequeñas y medianas con una única implementación de servidor de DirectAccess y permite la administración remota de los clientes de DirectAccess. Si ISATAP está implementado en un entorno multisitio, de equilibrio de carga o de multidominio, debe quitarlo o moverlo a una implementación IPv6 nativa antes de configurar DirectAccess.  
  
## <a name="bkmk_iphttps"></a>IPHTTPS y configuración de punto de conexión de contraseña de un solo tiempo (OTP)  
Cuando se usa IPHTTPS, la conexión IPHTTPS debe finalizar en el servidor de DirectAccess, no en ningún otro dispositivo, como un equilibrador de carga. Del mismo modo, la conexión de Capa de sockets seguros fuera de banda (SSL) que se crea durante la autenticación de contraseña de un solo tiempo (OTP) debe finalizar en el servidor de DirectAccess. Todos los dispositivos entre los puntos de conexión de estas conexiones deben configurarse en modo de paso a través.  
  
## <a name="bkmk_ft"></a>Forzar túnel con autenticación OTP  
No implemente un servidor de DirectAccess con autenticación en dos fases con OTP y tunelización forzada, o se producirá un error en la autenticación de OTP. Se requiere una conexión de Capa de sockets seguros fuera de banda (SSL) entre el servidor de DirectAccess y el cliente de DirectAccess. Esta conexión requiere una exención para enviar el tráfico fuera del túnel de DirectAccess. En una configuración de túnel forzado, todo el tráfico debe fluir a través de un túnel de DirectAccess y no se permite ninguna exención una vez establecido el túnel. Por este motivo, no se admite la autenticación de OTP en una configuración de túnel forzada.  
  
## <a name="bkmk_rodc"></a>Implementación de DirectAccess con un controlador de dominio de solo lectura  
Los servidores de DirectAccess deben tener acceso a un controlador de dominio de lectura y escritura y no funcionar correctamente con un controlador de dominio de solo lectura (RODC).  
  
Se requiere un controlador de dominio de lectura y escritura por muchos motivos, incluidos los siguientes:  
  
-   En el servidor de DirectAccess, se necesita un controlador de dominio de lectura y escritura para abrir Microsoft Management Console (MMC) de acceso remoto.  
  
-   El servidor de DirectAccess debe leer y escribir en el cliente de DirectAccess y el servidor de DirectAccess directiva de grupo objetos (GPO).  
  
-   El servidor de DirectAccess Lee y escribe en el GPO de cliente específicamente desde el emulador de controlador de dominio principal (PDCe).  
  
Debido a estos requisitos, no implemente DirectAccess con un RODC.  
  


