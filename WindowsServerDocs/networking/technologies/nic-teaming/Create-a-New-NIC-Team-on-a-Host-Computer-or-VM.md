---
title: Crear un nuevo equipo NIC en un equipo host o una máquina virtual
description: En este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual (VM) de Hyper-V que ejecuta Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 1785b34741ce525a5bdd27b77a0e52fc2ca6c1b6
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591100"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Crear un nuevo equipo NIC en un equipo host o una máquina virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual (VM) de Hyper-V que ejecuta Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Requisitos de configuración de red  
Para poder crear un nuevo equipo NIC, debe implementar un host de Hyper-V con dos adaptadores de red que se conecten a diferentes conmutadores físicos. También debe configurar los adaptadores de red con direcciones IP que sean del mismo intervalo de direcciones IP.  

El conmutador físico, el conmutador virtual de Hyper-V, la red de área local (LAN) y los requisitos de formación de equipos NIC para crear un equipo NIC en una máquina virtual son los siguientes:  

-   El equipo que ejecuta Hyper-V debe tener dos o más adaptadores de red.  

-   Si conecta los adaptadores de red a varios conmutadores físicos, los conmutadores físicos deben estar en la misma subred de nivel 2.  

-   Debe usar el administrador de Hyper-V o Windows PowerShell para crear dos conmutadores virtuales de Hyper-V externos, cada uno conectado a un adaptador de red físico diferente.  

-   La máquina virtual debe conectarse a los conmutadores virtuales externos que cree.  

-   La formación de equipos NIC, en Windows Server 2016, es compatible con equipos con dos miembros en máquinas virtuales. Puede crear equipos más grandes, pero no se admiten.

-   Si está configurando un equipo NIC en una máquina virtual, debe seleccionar un **modo de formación de equipos** _independiente_ y un modo de **equilibrio de carga** de hash de _Dirección_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Paso 1. Configuración de la red física y virtual  
En este procedimiento, creará dos conmutadores virtuales de Hyper-V externos, conectará una máquina virtual a los conmutadores y, a continuación, configurará las conexiones de máquina virtual a los conmutadores.  

### <a name="prerequisites"></a>Requisitos previos

Debe ser miembro de **los administradores**o un grupo equivalente.  

### <a name="procedure"></a>Procedimiento

1.  En el host de Hyper-V, abra el administrador de Hyper-V y, en acciones, haga clic en **Administrador de conmutadores virtuales**.  

   ![Administrador de conmutadores virtuales](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  En el administrador de conmutadores virtuales, asegúrese de que **external** está seleccionado y, a continuación, haga clic en **crear conmutador virtual**.  

   ![Crear conmutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  En propiedades de conmutador virtual, escriba un **nombre** para el conmutador virtual y agregue **notas** según sea necesario.  

4.  En **tipo de conexión**, en **red externa**, seleccione el adaptador de red físico al que desea conectar el conmutador virtual.  

5.  Configure propiedades de conmutador adicionales para la implementación y, a continuación, haga clic en **Aceptar**.  

6.  Cree un segundo conmutador virtual externo repitiendo los pasos anteriores. Conecte el segundo conmutador externo a un adaptador de red diferente.  

7.  En el administrador de Hyper-V, en **virtual machines**, haga clic con el botón secundario en la máquina virtual que desea configurar y, a continuación, haga clic en **configuración**.  

   Se abre el cuadro de diálogo **configuración** de VM.

8.  Asegúrese de que la máquina virtual no se ha iniciado. Si se ha iniciado, realice un cierre antes de configurar la máquina virtual.  

8.  En **hardware**, haga clic en **adaptador de red**.  

   ![Adaptador de red](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. En propiedades de **adaptador de red** , seleccione uno de los conmutadores virtuales que creó en los pasos anteriores y, a continuación, haga clic en **aplicar**.  

    ![Seleccionar un conmutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. En **hardware**, haga clic para expandir el signo más (+) junto a **adaptador de red**. 

11. Haga clic en **características avanzadas** para habilitar la formación de equipos NIC mediante la interfaz gráfica de usuario. 

    >[!TIP]
    >También puede habilitar la formación de equipos NIC con un comando de Windows PowerShell: 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Seleccione **dinámico** para dirección Mac. 

    b. Haga clic para seleccionar **red protegida**. 

    c. Haga clic para seleccionar **habilitar este adaptador de red para que forme parte de un equipo en el sistema operativo invitado**. 

    d. Haz clic en **Aceptar**.  

    ![Agregar un adaptador de red a un equipo](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Para agregar un segundo adaptador de red, en el administrador de Hyper-V, en **virtual machines**, haga clic con el botón secundario en la misma máquina virtual y, a continuación, haga clic en **configuración**.  

    Se abre el cuadro de diálogo **configuración** de VM.

14. En **agregar hardware**, haga clic en **adaptador de red**y, a continuación, haga clic en **Agregar**.  

   ![Agregar un adaptador de red](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. En propiedades de **adaptador de red** , seleccione el segundo conmutador virtual que creó en los pasos anteriores y, a continuación, haga clic en **aplicar**.  

   ![Aplicar un conmutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. En **hardware**, haga clic para expandir el signo más (+) junto a **adaptador de red**. 

17. Haga clic en **características avanzadas**, desplácese hacia abajo hasta **formación de equipos NIC**y haga clic para seleccionar **habilitar este adaptador de red para que forme parte de un equipo en el sistema operativo invitado**. 

18. Haz clic en **Aceptar**.  

_**Finaliza!**_  Ha configurado la red física y virtual.  Ahora puede continuar con la creación de un nuevo equipo NIC.  

## <a name="step-2-create-a-new-nic-team"></a>Paso 2. Crear un nuevo equipo NIC

Al crear un nuevo equipo NIC, debe configurar las propiedades del equipo NIC:  

-   Nombre del equipo  

-   Adaptadores de miembros  

-   Modo de formación de equipos  

-   Modo de equilibrio de carga  

-   Adaptador en espera  

También puede configurar opcionalmente la interfaz de equipo principal y configurar un número de LAN virtual (VLAN).  

Para obtener más información sobre estas opciones, consulte Configuración de la [formación de equipos NIC](nic-teaming-settings.md).

### <a name="prerequisites"></a>Requisitos previos

Debe ser miembro de **los administradores**o un grupo equivalente.  

### <a name="procedure"></a>Procedimiento

1. En el Administrador del servidor, haga clic en **Servidor local**.  

2. En el panel **propiedades** , en la primera columna, busque **formación de equipos NIC**y, a continuación, haga clic en el vínculo **deshabilitado** .  

   Se abre el cuadro de diálogo **formación de equipos NIC** .

   ![Cuadro de diálogo Formación de equipos NIC](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. En **adaptadores e interfaces**, seleccione uno o más adaptadores de red que desee agregar a un equipo NIC.  

4. Haga clic en **tareas**y haga clic en **Agregar al nuevo equipo**.  

   Se abrirá el cuadro de diálogo **nuevo equipo** y se mostrarán los adaptadores de red y los miembros del equipo.

5. En **nombre del equipo**, escriba un nombre para el nuevo equipo NIC y, a continuación, haga clic en **propiedades adicionales**.  

6. En **propiedades adicionales**, seleccione valores para:

   - **Modo de formación de equipos**. Las opciones para el modo de formación de equipos son **independiente del conmutador** y **dependiente del conmutador**. El modo dependiente del conmutador incluye la **formación de equipos estáticos** y el **Protocolo de control de agregación de vínculos (LACP)** . 

     - **Independiente del conmutador.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Dependiente del modificador.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Formación de equipos estática**              |                                                                                                                                              Requiere la configuración manual del conmutador y el host para identificar qué vínculos forman el equipo. Dado que se trata de una solución configurada de forma estática, no hay ningún protocolo adicional para ayudar al conmutador y el host a identificar cables conectados incorrectamente u otros errores que puedan hacer que el equipo no funcione. Este modo lo suelen ofrecer los conmutadores de clase de servidor.                                                                                                                                              |
       | **Protocolo de control de agregación de vínculos (LACP)** | A diferencia de la formación de equipos estática, el modo de formación de equipos de LACP identifica dinámicamente los vínculos que están conectados entre el host y el conmutador. Esta conexión dinámica permite la creación automática de un equipo y, en teoría, pero raramente en la práctica, la expansión y reducción de un equipo simplemente por la transmisión o recepción de paquetes LACP de la entidad del mismo nivel. Todos los conmutadores de clase de servidor admiten LACP y todos requieren que el operador de red Habilite administrativamente LACP en el puerto del conmutador. Cuando se configura un modo de formación de equipos de LACP, la formación de equipos NIC siempre funciona en el modo activo de LACP.  De forma predeterminada, la formación de equipos NIC usa un temporizador breve (3 segundos), pero puede configurar un temporizador largo (90 segundos) con `Set-NetLbfoTeam`. |

       ---

   - **Modo de equilibrio de carga**. Las opciones del modo de distribución de equilibrio de carga son **hash de dirección**, **Puerto de Hyper-V**y **dinámico**.

     - **Hash de dirección.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Puerto de Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dinámica.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Adaptador en espera**. Las opciones del adaptador en espera son **ninguno (todos los adaptadores activos)** o la selección de un adaptador de red específico en el equipo NIC que actúa como adaptador en espera.

   > [!TIP]  
   > Si está configurando un equipo NIC en una máquina virtual (VM), debe seleccionar un **modo de formación de equipos** _independiente_ y un **modo de equilibrio de carga** de _hash de dirección_.  

7. Si desea configurar el nombre de la interfaz de equipo principal o asignar un número de VLAN al equipo NIC, haga clic en el vínculo a la derecha de la **interfaz de equipo principal**.  

    Se abrirá el cuadro de diálogo **nueva interfaz de equipo** .

    ![Nueva interfaz de equipo](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. En función de sus requisitos, realice una de las acciones siguientes:  

   -   Proporcione un nombre de interfaz tNIC.  

   -   Configurar la pertenencia a VLAN: haga clic en **VLAN específica** y escriba la información de la VLAN. Por ejemplo, si desea agregar este equipo NIC al número 44 de la VLAN de contabilidad, escriba Accounting 44-VLAN.   

9. Haz clic en **Aceptar**.  

_**Finaliza!**_  Ha creado un nuevo equipo NIC en un equipo host o máquina virtual.

## <a name="related-topics"></a>Temas relacionados

- [Formación de equipos NIC](NIC-Teaming.md): en este tema se proporciona información general sobre la formación de equipos de tarjetas de interfaz de red (NIC) en Windows Server 2016. La formación de equipos NIC le permite agrupar entre uno y 32 adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.   

- [Administración y uso de direcciones MAC de formación de equipos NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): al configurar un equipo NIC con el modo independiente del conmutador y la distribución de la carga dinámica o el hash de la dirección, el equipo usa la dirección Media Access Control (Mac) del miembro del equipo NIC principal en el saliente. entrante. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo del conjunto inicial de miembros del equipo.

- [Configuración de la formación de equipos NIC](nic-teaming-settings.md): en este tema se proporciona información general de las propiedades del equipo NIC, como la formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles acerca de la configuración del adaptador en espera y la propiedad de la interfaz de equipo principal. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador en espera para la tolerancia a errores.

- [Solución de problemas de formación de equipos NIC](Troubleshooting-NIC-Teaming.md): en este tema se describen las formas de solucionar problemas de formación de equipos NIC, como hardware, los valores de los conmutadores físicos y la deshabilitación o habilitación de adaptadores de red con Windows PowerShell. 

---
