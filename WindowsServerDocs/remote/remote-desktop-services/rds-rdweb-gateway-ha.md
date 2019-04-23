---
title: Agregar alta disponibilidad a la parte delantera de web Web de escritorio remoto y puerta de enlace
description: Proporciona los pasos necesarios para instalar a los servidores Web de escritorio remoto y puerta de enlace en una implementación de RDS.
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
ms.openlocfilehash: fa09532e0b327b24ebb1c0e155c26c25d1043b63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854606"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Agregar alta disponibilidad a la parte delantera de web Web de escritorio remoto y puerta de enlace

>Se aplica a: Windows Server (canal semianual), Windows Server 2016


Puede implementar un acceso Web a Escritorio remoto (RD Web Access) y la granja de servidores de puerta de enlace de escritorio remoto (puerta de enlace de escritorio remoto) para mejorar la disponibilidad y escalabilidad de una implementación de servicios de escritorio remoto (RDS) de Windows Server 

Siga estos pasos para agregar un servidor Web de escritorio remoto y puerta de enlace a una implementación básica de servicios de escritorio remoto existente.  

## <a name="pre-requisites"></a>Requisitos previos

Configurar un servidor para que actúe como un adicionales Web de escritorio remoto y puerta de enlace de escritorio remoto: Esto puede ser un servidor físico o máquina virtual. Esto incluye unir el servidor al dominio y habilitar la administración remota.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Paso 1: Configurar el nuevo servidor para que sea parte del entorno de RDS

1. Conéctese al servidor RDMS en Azure portal, mediante el cliente conexión a Escritorio remoto.
2. Agregue un nuevo servidor Web de escritorio remoto y puerta de enlace al administrador del servidor:
    1. Inicie el administrador del servidor, haga clic en **administrar > Agregar servidores**.   
    2. En el cuadro de diálogo Agregar servidores, haga clic en **Buscar ahora**.   
    3. Seleccione el servidor Web de escritorio remoto y puerta de enlace recién creado (por ejemplo, Contoso-WebGw2) y haga clic en **Aceptar**.
3. Agregar servidores Web de escritorio remoto y puerta de enlace a la implementación  
    1. Launch Server Manager .  
    2. Haga clic en **servicios de escritorio remoto > información general > servidores de implementación > tareas > Agregar servidores de acceso Web de escritorio remoto**.   
    3. Seleccione el servidor recién creado (por ejemplo, Contoso-WebGw2) y, a continuación, haga clic en **siguiente**.  
    4. En la página de confirmación, seleccione **reiniciar los equipos remotos según sea necesario**y, a continuación, haga clic en **agregar**.  
    5. Repita estos pasos para agregar el servidor de puerta de enlace de escritorio remoto, pero elija **servidores de puerta de enlace de escritorio remoto** en el paso b.
4. Volver a instalar certificados para los servidores de puerta de enlace de escritorio remoto:
    1.  Haga clic en Administrador del servidor en el servidor RDMS, **servicios de escritorio remoto > información general > tareas > Editar las propiedades de implementación**.  
    2.  Expanda **certificados**.  
    3.  Desplácese hacia abajo hasta la tabla. Haga clic en el escritorio remoto **servicio de rol puerta de enlace > Seleccionar certificado existente.**  
    4.  Haga clic en **elegir otro certificado** y, a continuación, vaya a la ubicación del certificado. Por ejemplo, \Contoso-CB1\Certificates). Seleccione el archivo de certificado para el servidor Web de escritorio remoto y puerta de enlace que se creó durante los requisitos previos (p. ej., ContosoRdGwCert) y, a continuación, haga clic en **abierto**.  
    5.  Escriba la contraseña del certificado, seleccione **permitir que el certificado que se agregarán al almacén de certificados entidades emisoras de certificados raíz de confianza en los equipos de destino**y, a continuación, haga clic en **Aceptar**.  
    6.  Haga clic en **Aplicar**.
    > [!Note] 
    > Es posible que deba reiniciar manualmente el servicio TSGateway que se ejecutan en cada servidor de puerta de enlace de escritorio remoto, ya sea a través del administrador del servidor o el Administrador de tareas.
    7.  Repita los pasos para el servicio de rol de acceso Web de escritorio remoto.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Paso 2: Configurar las propiedades Web de escritorio remoto y puerta de enlace de escritorio remoto en el nuevo servidor
1. Configurar el servidor para formar parte de una granja de servidores de puerta de enlace de escritorio remoto:
    1.  Haga clic en Administrador del servidor en el servidor RDMS, **todos los servidores**. Haga clic en uno de los servidores de puerta de enlace de escritorio remoto y, a continuación, haga clic en **conexión a Escritorio remoto**.
    2.  Inicie sesión en el servidor de puerta de enlace de escritorio remoto con una cuenta de administrador de dominio.  
    3.  En Administrador del servidor en el servidor de puerta de enlace de escritorio remoto, haga clic en **Herramientas > Servicios de escritorio remoto > Administrador de puerta de enlace de escritorio remoto**.  
    4.  En el panel de navegación, haga clic en el equipo local (por ejemplo, Contoso-WebGw1).  
    5.  Haga clic en **los miembros de la granja de servidores de puerta de enlace de escritorio remoto de agregar**.  
    6.  En el **granja de servidores** pestaña, escriba el nombre de cada servidor de puerta de enlace de escritorio remoto y, a continuación, haga clic en **agregar** y **aplicar**.  
    7.  Repita los pasos hasta la f en cada servidor de puerta de enlace de escritorio remoto para que reconocerse mutuamente como servidores de puerta de enlace de escritorio remoto en una granja de servidores. No se alarme si hay advertencias, como podría llevar tiempo para la configuración de DNS se propaguen.
2. Configurar el servidor para formar parte de una granja de servidores de acceso Web de escritorio remoto. El procedimiento siguiente configura la validación y descifrado de las claves de equipo para que sean iguales en ambos sitios RDWeb.
    1.  Haga clic en Administrador del servidor en el servidor RDMS, **todos los servidores**. Haga clic en el primer servidor de acceso Web de escritorio remoto (por ejemplo, Contoso-WebGw1) y, a continuación, haga clic en **conexión a Escritorio remoto**.  
    2.  Inicie sesión en el servidor de acceso Web de escritorio remoto con una cuenta de administrador de dominio.  
    3.  En Administrador del servidor en el servidor de acceso Web de escritorio remoto, haga clic en **Herramientas > Administrador de Internet Information Services (IIS)**.  
    4.  En el panel izquierdo del Administrador de IIS, expanda el **Server (por ejemplo, Contoso-WebGw1) > sitios > sitio Web predeterminado**y, a continuación, haga clic en **RDWeb**.  
    5.  Haga clic en **machineKey**y, a continuación, haga clic en **abrir característica**.
    6.  En la página clave del equipo, en el **acciones** panel, seleccione **Generate Keys**y, a continuación, haga clic en **aplicar**.
    7.  Copie la clave de validación (puede haga clic en la clave y, a continuación, haga clic en **copia**.)
    8.  En el Administrador de IIS, en **sitio Web predeterminado**, seleccione **fuente**, **FeedLogon** y **páginas** a su vez.
    9. Para cada uno:
        1.  Haga clic en **machineKey**y, a continuación, haga clic en **abrir característica**.
        2.  Para la clave de validación, desactive **generar automáticamente en tiempo de ejecución**y, a continuación, pegue la clave que copió en el paso g.
    10.  Minimizar la ventana conexión a Escritorio remoto a un servidor Web de escritorio remoto.  
    11.  Repita los pasos del b al e para el segundo servidor de acceso Web de RD, finalizando en la vista de características de **machineKey**.
    12. Para la clave de validación, desactive **generar automáticamente en tiempo de ejecución**y, a continuación, pegue la clave que copió en el paso g.
    13. Haga clic en **Aplicar**.
    14. Completar este proceso para el **RDWeb**, **fuente**, **FeedLogon** y **páginas** páginas.
    15. Minimizar la ventana conexión a Escritorio remoto en el segundo servidor de acceso Web de escritorio remoto y, a continuación, maximice la ventana de conexión a Escritorio remoto para el primer servidor de acceso Web de escritorio remoto.  
    16. Repita los pasos g a n para copiar la clave de descifrado.
    17. Cuando las claves de validación y descifrado son idénticas en ambos servidores de acceso Web de escritorio remoto para el **RDWeb**, **fuente**, **FeedLogon** y **páginas**páginas, cierre la sesión de todas las ventanas de conexión a Escritorio remoto.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Paso 3: Configurar Equilibrio de carga para los servidores Web de escritorio remoto y puerta de enlace de escritorio remoto

Si usa la infraestructura de Azure, puede crear un equilibrador de carga externo; Si no es así, puede configurar un equilibrador de carga de hardware o software independiente. Equilibrio de carga es clave para que sea tráfico uniformemente distribuido las conexiones de clientes de escritorio remoto, a través de la puerta de enlace de escritorio remoto a los servidores que los usuarios se ejecutarán sus cargas de trabajo de larga duración.

> [!Note] 
> Si el servidor anterior ejecuta Web de escritorio remoto y puerta de enlace de escritorio remoto ya se ha configurado detrás de un equilibrador de carga externo, ir directamente para el paso 4, seleccione el grupo de back-end existente y agregue un nuevo servidor al grupo.

1.  Crear un equilibrador de carga:  
    1.  En el portal Azure, haga clic en **examinar > equilibradores de carga > agregar**.  
    2.  Escriba un nombre, por ejemplo **WebGwLB**.  
    3.  Seleccione **pública** para el **esquema**, **dirección IP pública**y un **dirección IP pública**. Puede seleccionar una dirección IP pública existente o crear uno nuevo. 
    4.  Seleccione la **suscripción**, **grupo de recursos**, y **ubicación**.
    5.  Haga clic en **Crear**.  
2. Crear un [sondeo](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) para supervisar los servidores que están activos:  
    1.  En el portal Azure, haga clic en **examinar > equilibradores de carga**., el equilibrador de carga que acaba de crear, por ejemplo, WebGwLB y configuración  
    2.  Haga clic en **sondeos > agregar**.  
    3.  Escriba un nombre, por ejemplo, **HTTPS**, para el sondeo. Seleccione **TCP** como el **protocolo**y escriba **443** para el **puerto**, a continuación, haga clic en **Aceptar**.   
3.  Cree las reglas de equilibrio de carga HTTPS y UDP:  
    1.  En **configuración**, haga clic en **reglas de equilibrio de carga**.  
    2.  Seleccione **agregar** para el **regla HTTPS**.  
    3.  Escriba un nombre para la regla, por ejemplo, HTTPS y seleccione **TCP** para el **protocolo**. Escriba **443** para ambos **puerto** y **puerto back-end**y haga clic en **Aceptar**.  
    4.  En **reglas de equilibrio de carga**, haga clic en **agregar** para el **regla UDP**.  
    5.  Escriba un nombre para la regla, por ejemplo, **UDP**y seleccione **UDP** para el **protocolo**. Escriba **3391** para ambos **puerto** y **puerto back-end**y haga clic en **Aceptar**.  
4. Cree el grupo de back-end para los servidores Web de escritorio remoto y puerta de enlace de escritorio remoto:
      1. En **configuración**, haga clic en **grupos de direcciones de back-end > agregar**.   
      2. Escriba un nombre (por ejemplo, **WebGwBackendPool**), a continuación, haga clic en **agregar una máquina virtual**.  
      3. Elija un conjunto de disponibilidad (por ejemplo, WebGwAvSet) y, a continuación, haga clic en **Aceptar**.   
      4. Haga clic en **elegir las máquinas virtuales**, seleccione cada máquina virtual y, a continuación, haga clic en **Seleccionar > Aceptar > Aceptar**.
