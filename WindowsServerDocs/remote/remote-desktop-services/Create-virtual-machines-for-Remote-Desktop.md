---
title: Creación de máquinas virtuales para Escritorio remoto
description: Crear máquinas virtuales que hospede los componentes de escritorio remoto en la nube.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: 5c61d9f08cb799d6a63a004bedab924a6ba37fdc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446730"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creación de máquinas virtuales para Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016

Siga estos pasos para crear las máquinas virtuales en el entorno del inquilino que se usará para ejecutar los roles de Windows Server 2016, servicios y características necesarias para una implementación de hospedaje de escritorio.   
  
Este ejemplo de una implementación básica, se creará el mínimo de 3 máquinas virtuales. Una máquina virtual va a hospedar los servicios de rol Agente de conexión a Escritorio remoto (RD) y el servidor de licencias y un recurso compartido de archivos para la implementación. Una segunda máquina virtual va a hospedar los servicios de rol puerta de enlace de escritorio remoto y acceso Web.  Una tercera máquina virtual hospedar el servicio de rol Host de sesión de escritorio remoto. Implementaciones muy pequeñas, puede reducir los costos de la máquina virtual mediante el Proxy de aplicación de AAD para eliminar todos los puntos de conexión públicos de la implementación y combinar todos los servicios de rol en una sola máquina virtual. Para implementaciones más grandes, puede instalar los servicios de rol en máquinas virtuales individuales para permitir una mejor escala.  
  
En esta sección se describe los pasos necesarios para implementar máquinas virtuales para cada rol basadas en imágenes de Windows Server en el [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Si necesita crear máquinas virtuales desde una imagen personalizada, que requiere PowerShell, consulte [crear una máquina virtual de Windows con Resource Manager y PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). A continuación, regrese aquí para conectar discos de datos de Azure para el recurso compartido de archivos y especifique una dirección URL externa para su implementación.  
  
1. [Creación de máquinas virtuales de Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) para hospedar el servidor de agente de conexión a Escritorio remoto, el servidor de licencias de escritorio remoto y el archivo.  
  
   Para nuestro propósito, hemos usado las convenciones de nomenclatura siguientes:  
   - Agente de conexión a Escritorio remoto, el servidor de licencias y el servidor de archivos:   
       - VM: Contoso-Cb1  
       - Conjunto de disponibilidad: CbAvSet    
   - Acceso Web de escritorio remoto y servidor de puerta de enlace de escritorio remoto:   
       - VM: Contoso-WebGw1  
       - Conjunto de disponibilidad: WebGwAvSet  
          
   - Host de sesión de escritorio remoto:   
       - VM: Contoso-SHA-1  
       - Conjunto de disponibilidad: ShAvSet  
          
   Cada máquina virtual usa el mismo grupo de recursos.  
2. Crear y adjuntar un disco de datos de Azure para el recurso compartido (UPD) de disco de perfil de usuario:  
   1.  En el portal Azure, haga clic en **examinar > grupos de recursos**, haga clic en el grupo de recursos para la implementación y, a continuación, haga clic en la máquina virtual creada para el agente de conexión a Escritorio remoto (por ejemplo, Contoso-Cb1).  
   2.  Haga clic en **configuración > discos > adjuntar nuevo**.  
   3.  Acepte los valores predeterminados para el nombre y tipo.  
   4.  Escriba un tamaño (en GB) que es lo suficientemente grande como para contener los recursos compartidos de red para el entorno del inquilino, incluidos los certificados y los discos de perfil de usuario. Puede aproximar 5 GB por usuario que pretende tener  
   5.  Acepte los valores predeterminados para la ubicación y el almacenamiento en caché de host y, a continuación, haga clic en **Aceptar**.  
3. Crear un equilibrador de carga externo para acceder a la implementación externa:
   1. En el portal Azure, haga clic en **examinar > equilibradores de carga**y, a continuación, haga clic en **agregar**.
   2. Escriba un **nombre**, seleccione **pública** como el **tipo** del equilibrador de carga y seleccione la adecuada **suscripción**,  **Grupo de recursos**, y **ubicación**.
   3. Seleccione **elegir una dirección IP pública**, **crear nuevo**, escriba un nombre y seleccione **Aceptar**.
   4. Seleccione **crear** para crear el equilibrador de carga.
4. Configurar el equilibrador de carga externo para la implementación
   1. En el portal Azure, haga clic en **examinar > grupos de recursos**, haga clic en el grupo de recursos para la implementación y, a continuación, haga clic en el equilibrador de carga que creó para la implementación.
   2. Agregar un grupo de back-end del equilibrador de carga enviar tráfico al:
       1. Seleccione **grupo back-end** y **agregar**.
       2. Escriba un **nombre** y seleccione  **\+ agregar una máquina virtual**.
       3. Seleccione **conjunto de disponibilidad** y **WebGwAvSet**.
       4. Seleccione **máquinas virtuales**, **Contoso WebGw1**, **seleccione**, **Aceptar**, y **Aceptar**.
   3. Agregar un sondeo para que el equilibrador de carga sepa qué equipos están activos:
       1. Seleccione **sondeos** y **agregar**.
       2. Escriba un **nombre** (por ejemplo, HTTPS), seleccione **TCP**, escriba **puerto** 443 y, a continuación, seleccione **Aceptar**.
   4. Especificar reglas para equilibrar el tráfico entrante de equilibrio de carga:
      1. Seleccione **reglas de equilibrio de carga** y **agregar**
      2. Escriba un **nombre** (por ejemplo, HTTPS), seleccione **TCP**y 443 para ambos el **puerto** y **puerto back-end**.
          - Para Windows 10 y la implementación de Windows Server 2016, deje **persistencia de la sesión** como **ninguno**, en caso contrario, seleccione **IP de cliente**.
      3. Seleccione **Aceptar** para aceptar la regla HTTPS.
      4. Crear una nueva regla seleccionando **agregar**.
      5. Escriba un **nombre** (por ejemplo, UDP), seleccione **UDP**y 3391 tanto para el <strong>puerto y la ** puerto back-end</strong>.
          - Para una implementación de Windows 10 y Windows Server 2016, deje **persistencia de la sesión** como **ninguno**, en caso contrario, seleccione **IP de cliente**.
      6. Seleccione **Aceptar** para aceptar la regla UDP.
   5. Especifique una regla NAT de entrada para conectarse directamente a Contoso-WebGw1
       1. Seleccione **reglas NAT de entrada** y **agregar**.
       2. Escriba un **nombre** (por ejemplo, RDP-Contoso-WebGw1), seleccione **Customm** para el servicio, **TCP** para el protocolo y escriba 14000 para el **puerto**.
       3. Seleccione **elige una máquina virtual** y WebGw1 de Contoso.
       4. Seleccione **personalizado** para la asignación de puertos, escriba 3389 para el **puerto de destino**y seleccione **Aceptar**.
5. Escriba un nombre DNS o dirección URL externo para la implementación acceder a él externamente:  
   1.  En el portal de Azure, haga clic en **examinar > grupos de recursos**, haga clic en el grupo de recursos para la implementación y, a continuación, haga clic en la dirección IP pública que creó para el acceso Web de escritorio remoto y puerta de enlace de escritorio remoto.  
   2.  Haga clic en **configuración**, escriba una etiqueta de nombre DNS (por ejemplo, contoso) y, a continuación, haga clic en **guardar**. Esta etiqueta de nombre DNS (contoso.westus.cloudapp.azure.com) es el nombre DNS que va a usar para conectarse al servidor de acceso Web de escritorio remoto y puerta de enlace de escritorio remoto.  

