---
title: Creación de máquinas virtuales para Escritorio remoto
description: Cree máquinas virtuales que hospeden componentes de Escritorio remoto en la nube.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: fa17c472e3311e4e34ac7b2176d0045886463274
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818468"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creación de máquinas virtuales para Escritorio remoto

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Siga estos pasos para crear los equipos virtuales en el entorno del inquilino que se usará para ejecutar los roles, servicios y características de Windows Server 2016 necesarias para una implementación que hospede un escritorio.   
  
Para este ejemplo de una implementación básica, se creará el mínimo de tres máquinas virtuales. Una de ellas hospedará los servicios de rol Servidor de licencias y Agente de conexión a Escritorio remoto (RD), y un recurso compartido de archivos para la implementación. Una segunda hospedará los servicios de rol Puerta de enlace y Acceso web de Escritorio remoto.  La tercera máquina virtual hospedará el servicio de rol Host de sesión de Escritorio remoto. En las implementaciones muy pequeñas se puede reducir los costos de máquinas virtuales mediante el uso de un proxy de aplicación de AAD para eliminar todos los puntos de conexión públicos de la implementación y combinar todos los servicios de rol en una sola máquina virtual. En el caso de implementaciones mayores, puede instalar los distintos servicios de rol en máquinas virtuales individuales para permitir un mejor escalado.  
  
En esta sección se describen los pasos necesarios para implementar máquinas virtuales para cada rol basadas en imágenes de Windows Server de [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Si necesitas crear máquinas virtuales a partir de una imagen personalizada, que requiere PowerShell, consulta [Creación de una máquina virtual Windows con Resource Manager y PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Luego, regresa aquí para conectar los discos de datos de Azure para el recurso compartido de archivos y especifica una dirección URL externa para la implementación.  
  
1. [Cree máquinas virtuales Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) para hospedar el Agente de conexión a Escritorio remoto, el servidor de licencias de Escritorio remoto y el servidor de archivos.  
  
   Para lograr nuestro fin, hemos usado las convenciones de nomenclatura siguientes:  
   - Agente de conexión a Escritorio remoto, servidor de licencias y servidor de archivos:   
       - Máquina virtual: Contoso-Cb1  
       - Conjunto de disponibilidad: CbAvSet    
   - Acceso web a Escritorio remoto y servidor de puerta de enlace de Escritorio remoto:   
       - Máquina virtual: Contoso-WebGw1  
       - Conjunto de disponibilidad: WebGwAvSet  
          
   - Host de sesión de Escritorio remoto:   
       - Máquina virtual: Contoso-Sh1  
       - Conjunto de disponibilidad: ShAvSet  
          
   Todas las máquinas virtuales usan el mismo grupo de recursos.  
2. Crea y adjunta un disco de datos de Azure para el recurso compartido del disco de perfil de usuario (UPD):  
   1.  En Azure Portal, haz clic en **Examinar > Grupos de recursos**, haz clic en el grupo de recursos de la implementación y, después, en la máquina virtual creada para el Agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb1).  
   2.  Haz clic en **Configuración > Discos > Conectar nuevo** .  
   3.  Acepta los valores predeterminados de nombre y tipo.  
   4.  Escribe un tamaño (en GB) suficiente para contener los recursos compartidos de red del entorno del inquilino, incluidos los certificados y los discos de perfil de usuario. Puedes tener aproximadamente 5 GB por usuario que planees tener  
   5.  Acepta los valores predeterminados de ubicación y almacenamiento en caché del host y, después, haz clic en **Aceptar**.  
3. Crea un equilibrador de carga externo para acceder a la implementación de forma externa:
   1. En Azure Portal, haz clic en **Examinar > Equilibradores de carga** y, después,haz clic en **Agregar**.
   2. Escribe un **nombre**, selecciona **Público** como **tipo** de equilibrador de carga y selecciona la **suscripción**, el **grupo de recursos** y la **ubicación** apropiados.
   3. Selecciona **Elegir una dirección IP pública** , **Crear nueva**, escribe un nombre y selecciona **Aceptar**.
   4. Selecciona **Crear** para crear el equilibrador de carga.
4. Configura el equilibrador de carga externo para la implementación
   1. En Azure Portal, haz clic en **Examinar > Grupos de recursos**, haz clic en el grupo de recursos de la implementación y, luego, en el equilibrador de carga que creaste para la implementación.
   2. Agrega un grupo de back-end para el equilibrador de carga para enviarle tráfico:
       1. Selecciona **Grupo de back-end**  y **Agregar**.
       2. Escribe un **nombre** y selecciona **\+ Agregar una máquina virtual**.
       3. Selecciona **Conjunto de disponibilidad** y **WebGwAvSet**.
       4. Selecciona **Máquinas virtuales**, **Contoso WebGw1**, **Seleccionar**, **Aceptar**, y **Aceptar**.
   3. Agrega un sondeo para que el equilibrador de carga sepa qué equipos están activos:
       1. Selecciona **Sondeos** y **Agregar**.
       2. Escribe un **nombre** (como HTTPS), selecciona **TCP**, escribe **Puerto** 443 y selecciona **Aceptar**.
   4. Especifica las reglas de equilibrio de carga para equilibrar el tráfico entrante:
      1. Selecciona **Reglas de equilibrio de carga** y **Agregar**
      2. Escribe un **nombre** (por ejemplo, HTTPS), selecciona **TCP**y 443 tanto en **Puerto** como en **Puerto back-end**.
          - En el caso de una implementación de Windows 10 y Windows Server 2016, deja en **Persistencia de la sesión**  el valor **Ninguno**; de lo contrario, selecciona **IP de cliente**.
      3. Selecciona **Aceptar** para aceptar la regla HTTPS.
      4. Crea una regla seleccionando **Agregar**.
      5. Escribe un **nombre** (por ejemplo, UDP), selecciona **UDP**y 3391 tanto para el <strong>puerto como para el ** puerto back-end</strong>.
          - En el caso de una implementación de Windows 10 y Windows Server 2016, deja en **Persistencia de la sesión**  el valor **Ninguno**; de lo contrario, selecciona **IP de cliente**.
      6. Selecciona **Aceptar** para aceptar la regla UDP.
   5. Especifica una regla NAT de entrada para conectar directamente con Contoso-WebGw1
       1. Selecciona **Reglas NAT de entrada**  y **Agregar**.
       2. Escribe un **nombre** (como RDP-Contoso-WebGw1), selecciona **Personalizado** en servicio, **TCP** en protocolo y escribe 14000 en **Puerto**.
       3. Selecciona **Elegir una máquina virtual**  y Contoso-WebGw1.
       4. Selecciona **Personalizado** en asignación de puertos, escribe 3389 en **Puerto de destino**y selecciona **Aceptar**.
5. Escriba un nombre DNS o una dirección URL externos para la implementación, con el fin de acceder a ellos externamente:  
   1.  En Azure Portal, haz clic en **Examinar > Grupos de recursos**, haz clic en el grupo de recursos de la implementación y, luego, en la dirección IP pública que creaste para Acceso web de Escritorio remoto y Puerta de enlace de Escritorio remoto.  
   2.  Haz clic en **Configuración**, escribe una etiqueta de nombre DNS (por ejemplo, contoso) y haz clic en **Guardar**. Esta etiqueta de nombre DNS (contoso.westus.cloudapp.azure.com) es el nombre DNS que usará para conectarse al servidor de Acceso web de Escritorio remoto y Puerta de enlace de Escritorio remoto.  

