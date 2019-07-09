---
title: Adición de alta disponibilidad al front-end web de la puerta de enlace y de la web de Escritorio remoto
description: Proporciona los pasos necesarios para instalar los servidores de puerta de enlace y web de Escritorio remoto en una implementación de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: 4e185e51b09d2e2f8ac4527f9de339de27e02f24
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805137"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Adición de alta disponibilidad al front-end web de la puerta de enlace y de la web de Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016


Puede implementar un acceso web de Escritorio remoto y una granja de puerta de enlace de Escritorio remoto para mejorar la disponibilidad y escalar una implementación de los Servicios de Escritorio remoto (RDS) de Windows. 

Siga estos pasos para agregar un servidor de puerta de enlace y de web de Escritorio remoto a una implementación básica de Servicios de Escritorio remoto existente.  

## <a name="pre-requisites"></a>Requisitos previos

Configura un servidor para que actúe como una puerta de enlace de Escritorio remoto y de acceso web de Escritorio remoto: puede ser un servidor físico o una máquina virtual. Esto incluye unir el servidor al dominio y habilitar la administración remota.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Paso 1: Configuración del nuevo servidor para que forme parte del entorno de RDS

1. Conéctate al servidor RDMS en Azure Portal, mediante el cliente de Conexión a Escritorio remoto.
2. Agrega un nuevo servidor de acceso web y de puerta de enlace de Escritorio remoto a Administrador del servidor:
    1. Inicia Administrador del servidor, haz clic en **Administrar > Agregar servidores**.   
    2. En el cuadro de diálogo Agregar servidores, haz clic en **Buscar ahora**.   
    3. Selecciona el servidor de acceso web y de puerta de enlace de Escritorio remoto recién creado (por ejemplo, Contoso-WebGw2) y haz clic en **Aceptar**.
3. Adición de los servidores de puerta de enlace y acceso web de Escritorio remoto a la implementación  
    1. Inicia el Administrador del servidor.  
    2. Haz clic en **Servicios de Escritorio remoto > Información general > Servidores de implementación > Tareas > Agregar servidores de Acceso web a Escritorio remoto**.   
    3. Selecciona el servidor recién creado (por ejemplo, Contoso-WebGw2) y haz clic en **Siguiente**.  
    4. En la página de confirmación, selecciona **Reiniciar equipos remotos según sea necesario** y, a continuación, haz clic en **Agregar**.  
    5. Repite estos pasos para agregar el servidor de Puerta de enlace de Escritorio remoto, pero elige **Servidores de Puerta de enlace de Escritorio remoto** en el paso b.
4. Vuelve a instalar los certificados para los servidores de Puerta de enlace de Escritorio remoto:
   1. En el Administrador del servidor en el servidor de RDMS, haz clic en **Servicios de Escritorio remoto > Información general > Tareas > Editar propiedades de implementación**.  
   2. Expande **Certificados**.  
   3. Desplázate hacia abajo por la tabla. Haz clic en **Servicio de rol Puerta de enlace de Escritorio remoto > Seleccionar certificado existente.**  
   4. Haz clic en **Elija un certificado distinto** y, después, dirígete a la ubicación del certificado. Por ejemplo, \Contoso-CB1\Certificates. Selecciona el archivo de certificado para el servidor de acceso web y de puerta de enlace de Escritorio remoto que se creó durante los requisitos previos (p. ej., ContosoRdGwCert) y, después, haz clic en **Abrir**.  
   5. Escribe la contraseña del certificado, selecciona **Permitir agregar el certificado al almacén de certificados Entidades de certificación raíz de confianza en los equipos de destino** y haz clic en **Aceptar**.  
   6. Haga clic en **Aplicar**.
      > [!NOTE] 
      > Es posible que tengas que reiniciar manualmente el servicio TSGateway que se ejecuta en cada servidor de Puerta de enlace de Escritorio remoto, ya sea a través del Administrador del servidor o del Administrador de tareas.
   7. Repite los pasos de la a a la f para el servicio de rol de acceso web de Escritorio remoto.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Paso 2: Configuración de las propiedades de Puerta de enlace de Escritorio remoto y de Acceso web de Escritorio remoto en el nuevo servidor
1. Configura el servidor para que forme parte de una granja de servidores de Puerta de enlace de Escritorio remoto:
    1.  En Administrador del servidor en el servidor RDMS, haz clic en **Todos los servidores**. Haz clic con el botón derecho en uno de los servidores de Puerta de enlace de Escritorio remoto y, después, haz clic en **Conexión a Escritorio remoto**.
    2.  Inicia sesión en el servidor de Puerta de enlace de Escritorio remoto mediante una cuenta de administrador de dominio.  
    3.  En Administrador del servidor en el servidor de Puerta de enlace de Escritorio remoto, haz clic en **Herramientas > Servicios de Escritorio remoto > Administrador de puerta de enlace de Escritorio remoto**.  
    4.  En el panel de navegación, haz clic en el equipo local (por ejemplo, Contoso-WebGw1).  
    5.  Haz clic en **Agregar miembros de la granja de servidores de puerta de enlace de Escritorio remoto**.  
    6.  En la pestaña **Granja de servidores**, escribe el nombre de cada servidor de Puerta de enlace de Escritorio remoto y haz clic en **Agregar** y **Aplicar**.  
    7.  Repite los pasos de la a a la f en cada servidor de Puerta de enlace de Escritorio remoto para que se reconozcan entre sí como servidores de Puerta de enlace de Escritorio remoto en una granja de servidores. No te alarmes si hay advertencias, ya que la propagación de la configuración de DNS puede llevar tiempo.
2. Configura el servidor para que forme parte de una granja de servidores de Acceso web de Escritorio remoto. Los siguientes pasos configuran las claves de la máquina de validación y descifrado para que sean las mismas en ambos sitios web de Escritorio remoto.
    1.  En Administrador del servidor en el servidor RDMS, haz clic en **Todos los servidores**. Haz clic con el botón derecho en el primer servidor de Acceso web de Escritorio remoto (por ejemplo, Contoso-WebGw1) y, después, haz clic en **Conexión a Escritorio remoto**.  
    2.  Inicia sesión en el servidor de Acceso web de Escritorio remoto mediante una cuenta de administrador de dominio.  
    3.  En Administrador del servidor, en el servidor de Acceso web de Escritorio remoto, haz clic en **Herramientas > Administrador de Internet Information Services (IIS)** .  
    4.  En el panel izquierdo del Administrador de IIS, expande el **servidor (por ejemplo, Contoso-WebGw1) > Sitios > Sitio web predeterminado** y, a continuación, haz clic en **RDWeb**.  
    5.  Haz clic con el botón derecho en **Clave del equipo** y, después, haz clic en **Abrir característica**.
    6.  En la página Clave del equipo, en el panel **Acciones**, selecciona **Generar claves** y, después, haz clic en **Aplicar**.
    7.  Copia la clave de validación (puedes hacer clic con el botón derecho en la clave y, después, haz clic en **Copiar**).
    8.  En el Administrador de IIS, en **Default Web Site** (Sitio web predeterminado), selecciona sucesivamente **Feed** (Fuente), **FeedLogon** y **Pages** (Páginas).
    9. Para cada uno:
        1.  Haz clic con el botón derecho en **Clave del equipo** y, después, haz clic en **Abrir característica**.
        2.  Para la clave de validación, desactiva **Generar automáticamente en tiempo de ejecución** y, después, pega la clave copiada del paso g.
    10.  Minimiza la ventana Conexión a Escritorio remoto a este servidor web de Escritorio remoto.  
    11.  Repite los pasos del b al e para el segundo servidor de Acceso web de Escritorio remoto, finalizando en la vista de características de **Clave del equipo**.
    12. Para la clave de validación, desactiva **Generar automáticamente en tiempo de ejecución** y, después, pega la clave copiada del paso g.
    13. Haga clic en **Aplicar**.
    14. Completa este proceso para las páginas **RDWeb**, **Feed** (Fuente), **FeedLogon** y **Pages** (Páginas).
    15. Minimice la ventana de conexión a Escritorio remoto al segundo servidor de Acceso web de Escritorio remoto y, después, maximiza la ventana de la conexión al Escritorio remoto al primer servidor de Acceso web de Escritorio remoto.  
    16. Repite los pasos g al n para copiar la clave de descifrado.
    17. Cuando las claves de validación y de descifrado son idénticas en ambos servidores de Acceso web de Escritorio remoto para las páginas **RDWeb**, **Feed** (Fuente), **FeedLogon** y **Pages** (Páginas), sal de todas las ventanas de conexión a Escritorio remoto.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Paso 3: Configuración de equilibrio de carga para los servidores de Acceso web de Escritorio remoto y de Puerta de enlace de Escritorio remoto

Si usas la infraestructura de Azure, puedes crear un equilibrador de carga externo de Azure; si no, puedes configurar un equilibrador de carga de hardware o software independiente. El equilibrio de carga es clave para que el tráfico distribuya uniformemente las conexiones de larga duración de los clientes de Escritorio remoto, mediante la Puerta de enlace de Escritorio remoto, a los servidores en los que los usuarios ejecutarán sus cargas de trabajo.

> [!NOTE] 
> Si tu servidor anterior que ejecutaba Acceso web de Escritorio remoto y Puerta de enlace de Escritorio remoto ya estaba configurado detrás de un equilibrador de carga externo, vete al paso 4, selecciona el grupo de back-end existente y agrega el nuevo servidor al grupo.

1.  Crea un equilibrador de carga de Azure:  
    1.  En Azure Portal, haz clic en **Examinar > Equilibradores de carga > Agregar**.  
    2.  Escribe un nombre, por ejemplo **WebGwLB**.  
    3.  Selecciona **Público** en **Esquema**, **Dirección IP pública** y una **Dirección IP pública**. Puedes seleccionar una dirección IP pública existente o crear una nueva. 
    4.  Selecciona los valores apropiados para la **suscripción**, el **grupo de recursos** y la **ubicación**.
    5.  Haga clic en **Crear**.  
2. Crea un [sondeo](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) para supervisar los servidores que están activos:  
    1.  En Azure Portal, haz clic en **Examinar > Equilibradores de carga** y busca el equilibrador de carga que acabas de crear, por ejemplo, WebGwLB y, finalmente, en Configuración  
    2.  Haz clic en **Sondeos > Agregar**.  
    3.  Escribe un nombre, por ejemplo, **HTTPS**, para el sondeo. Selecciona **TCP** en **Protocolo** y escribe **443** para **Puerto**; a continuación,haz clic en **Aceptar**.   
3.  Crea las reglas de equilibrio de carga de HTTPS y UDP:  
    1.  En **Configuración**, haz clic en **Reglas de equilibrio de carga**.  
    2.  Selecciona **Agregar** para la **regla HTTPS**.  
    3.  Escribe un nombre para la regla, por ejemplo, HTTPS y selecciona **TCP** en **Protocolo**. Escribe **443** en **Puerto** y **Puerto back-end** y haz clic en **Aceptar**.  
    4.  En **Reglas de equilibrio de carga**, haz clic en **Agregar** para la **regla UDP**.  
    5.  Escribe un nombre para la regla, por ejemplo, **UDP** y selecciona **UDP** en **Protocolo**. Escribe **3391** en **Puerto** y **Puerto back-end** y haz clic en **Aceptar**.  
4. Crea el grupo de back-end para los servidores de Acceso web de Escritorio remoto y Puerta de enlace de Escritorio remoto:
      1. En **Configuración**, haz clic en **Grupos de direcciones de back-end > Agregar**.   
      2. Escribe un nombre (por ejemplo, **WebGwBackendPool**) y haz clic en **Agregar una máquina virtual**.  
      3. Elige un conjunto de disponibilidad (por ejemplo, WebGwAvSet) y haz clic en **Aceptar**.   
      4. Haz clic en **Elegir las máquinas virtuales**, selecciona cada máquina virtual y haz clic en **Seleccionar > Aceptar > Aceptar**.
