---
title: Configuraciones no compatibles de DirectAccess
description: En este tema se proporciona una lista de configuraciones no admitidas de DirectAccess en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-da
ms.topic: article
ms.assetid: 23d05e61-95c3-4e70-aa83-b9a8cae92304
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49fa86883e6a590ac8f7dcf0c724f87af419f88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828856"
---
# <a name="directaccess-unsupported-configurations"></a>Configuraciones no compatibles de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Revise la siguiente lista de configuraciones no admitidas de DirectAccess antes de iniciar la implementación para evitar tener que iniciar la implementación de nuevo.  

## <a name="bkmk_frs"></a>Distribución del servicio de replicación (FRS) de archivos de objetos de directiva de grupo (replicaciones SYSVOL)  
Implementar DirectAccess en entornos donde los controladores de dominio se ejecutan el servicio de replicación de archivos (FRS) para la distribución de los objetos de directiva de grupo (replicaciones SYSVOL). No se admite la implementación de DirectAccess cuando se usa FRS.  
  
Si tiene controladores de dominio que ejecutan Windows Server 2003 o Windows Server 2003 R2, que usa FRS. Además, puede que esté usando FRS si anteriormente usó Windows 2000 Server o los controladores de dominio de Windows Server 2003 y ha no migrado nunca la replicación de SYSVOL de FRS a replicación de sistema de archivos distribuido (DFS-R).  
  
Si implementa DirectAccess con la replicación de SYSVOL de FRS, se arriesga a la eliminación accidental de la directiva de grupo de DirectAccess los objetos que contienen el servidor de DirectAccess y la información de configuración de cliente. Si se eliminan estos objetos, la implementación de DirectAccess sufrirá una interrupción del servicio y los equipos cliente que usan DirectAccess no podrán conectarse a la red.  
  
Si planea implementar DirectAccess, debe usar controladores de dominio que ejecutan sistemas operativos posteriores a Windows Server 2003 R2, y debe usar DFS-R.  
  
Para obtener información sobre la migración de FRS a DFS-R, consulte el [Guía de migración de replicación de SYSVOL: FRS a replicación DFS](https://technet.microsoft.com/library/dd640019(v=ws.10).aspx).  
  
## <a name="bkmk_nap"></a>Protección de acceso a los clientes de DirectAccess  
Protección de acceso a redes (NAP) se usa para determinar si los equipos cliente remotos cumplen las políticas de TI antes de que se les concede acceso a la red corporativa. NAP ha quedado en desuso en Windows Server 2012 R2 y no se incluye en Windows Server 2016. Por este motivo, no se recomienda iniciar una nueva implementación de DirectAccess con NAP. Se recomienda un método de control de punto de conexión diferente para la seguridad de los clientes de DirectAccess.  
  
## <a name="bkmk_multi"></a>Compatibilidad de multisitio para clientes de Windows 7  
Cuando DirectAccess se configura en una implementación multisitio, Windows 10&reg;, Windows&reg; 8.1 y Windows&reg; 8 clientes tienen la capacidad de conectarse al sitio más cercano.  Windows 7&reg; los equipos cliente no tiene la misma funcionalidad. Selección de sitio para clientes de Windows 7 se establece en un sitio en particular en el momento de la configuración de directiva, y estos clientes siempre se conectarán a ese sitio designado, independientemente de su ubicación.  
  
## <a name="bkmk_user"></a>Control de acceso basado en usuarios  
Las directivas de DirectAccess son equipo en función, no en función de usuario. No se admite la especificación de las directivas de usuario de DirectAccess para controlar el acceso a la red corporativa.  
  
## <a name="bkmk_policy"></a>Personalización de la directiva de DirectAccess  
DirectAccess se puede configurar mediante el Asistente para configuración de DirectAccess, la consola de administración de acceso remoto o los cmdlets de acceso remoto de Windows PowerShell. Uso de cualquier medio que no sea el Asistente para configuración de DirectAccess para configurar DirectAccess, como modificar directamente los objetos de directiva de grupo de DirectAccess o modificar manualmente la configuración de directiva predeterminada en el servidor o cliente, no se admite. Estas modificaciones pueden dar lugar a una configuración no utilizable.  
  
## <a name="bkmk_kerb"></a>Autenticación KerbProxy  
Al configurar un servidor de DirectAccess con el Asistente para introducción, el servidor de DirectAccess se configura automáticamente para usar autenticación de KerbProxy para el equipo y la autenticación de usuario. Por este motivo, solo debe usar el Asistente para introducción para las implementaciones de sitio único que solo Windows 10&reg;, se implementan los clientes de Windows 8 o Windows 8.1.  
  
Además, las características siguientes no deben utilizarse con autenticación de KerbProxy:  
  
-   El equilibrio con un equilibrador de carga externo o carga de Windows   
    Equilibrador de  
  
-   Autenticación en dos fases que se requieren las tarjetas inteligentes o una contraseña de un solo uso (OTP)  
  
No se admiten los siguientes planes de implementación si habilita la autenticación de KerbProxy:  
  
-   La funcionalidad de multisitio.  
  
-   DirectAccess es compatible con los clientes de Windows 7.  
  
-   El túnel forzado. Para asegurarse de que no está habilitada la autenticación KerbProxy cuando se usa el túnel forzado, configure los siguientes elementos mientras se ejecuta al asistente:  
  
    -   Habilitar el túnel forzado  
  
    -   Permitir que los clientes de DirectAccess para Windows 7  
  
> [!NOTE]  
> Para las implementaciones anteriores, debe usar al Asistente de configuración avanzada, que usa una configuración de dos túneles con una autenticación basada en certificados de equipo y usuario. Para obtener más información, consulte [implementar un único servidor de DirectAccess con configuración avanzada](../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
## <a name="bkmk_isa"></a>Uso de ISATAP  
ISATAP es una tecnología de transición que proporciona conectividad IPv6 en redes corporativas de sólo IPv4. Se limita a las organizaciones pequeñas y medianas con una sola implementación de servidor de DirectAccess, y permite la administración remota de los clientes de DirectAccess. Si ISATAP es deployedin un multisitio, el equilibrio de carga o el entorno de varios dominios, debe quitarlo o moverlo a una implementación de IPv6 nativo antes de configurar DirectAccess.  
  
## <a name="bkmk_iphttps"></a>IP-HTTPS y configuración de extremo de la contraseña de un solo uso (OTP)  
Al usar IP-HTTPS, debe terminar la conexión IP-HTTPS en el servidor de DirectAccess, no en cualquier otro dispositivo, por ejemplo, un equilibrador de carga. De forma similar, debe terminar la conexión de capa de Sockets seguros (SSL) fuera de banda que se crea durante la autenticación de contraseña de un solo uso (OTP) en el servidor de DirectAccess. Todos los dispositivos entre los puntos de conexión de estas conexiones se deben configurar en el modo de acceso directo.  
  
## <a name="bkmk_ft"></a>Forzar túnel con autenticación OTP  
Implementar un servidor de DirectAccess con autenticación en dos fases con OTP y el túnel forzado, o se producirá un error de autenticación de OTP. Falta una conexión de capa de Sockets seguros (SSL) fuera de banda entre el servidor de DirectAccess y el cliente de DirectAccess. Esta conexión requiere una exención para enviar el tráfico fuera del túnel de DirectAccess. En una configuración de túnel forzado, todo el tráfico debe pasar a través de un túnel de DirectAccess y no se permite ninguna exención después de establecer el túnel. Por este motivo, no se admite tener la autenticación OTP en una configuración de túnel forzado.  
  
## <a name="bkmk_rodc"></a>Implementación de DirectAccess con un controlador de dominio de solo lectura  
Servidores de DirectAccess deben tener acceso a un controlador de dominio de solo lectura y no funcionan correctamente con un controlador de dominio de sólo lectura (RODC).  
  
Se requiere un controlador de dominio de lectura y escritura por diversos motivos, incluidos los siguientes:  
  
-   En el servidor de DirectAccess, un controlador de dominio de solo lectura se requiere para abrir remoto acceso a Microsoft Management Console (MMC).  
  
-   El servidor de DirectAccess debe leer y escribir en el cliente de DirectAccess y el servidor de DirectAccess objetos de directiva de grupo (GPO).  
  
-   El servidor de DirectAccess lee y escribe en el GPO de cliente específicamente desde el emulador de controlador de dominio principal (PDCe).  
  
Debido a estos requisitos, implementar DirectAccess con un RODC.  
  


