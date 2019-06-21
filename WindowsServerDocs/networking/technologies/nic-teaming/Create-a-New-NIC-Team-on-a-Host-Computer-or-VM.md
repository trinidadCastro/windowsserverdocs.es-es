---
title: Crear un nuevo equipo NIC en un equipo host o máquina virtual
description: En este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual de Hyper-V (VM) que ejecuta Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 5380cb2007bab1a296e0facc12885d47c6afc708
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282099"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Crear un nuevo equipo NIC en un equipo host o máquina virtual

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual de Hyper-V (VM) que ejecuta Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Requisitos de configuración de red  
Antes de poder crear un nuevo equipo NIC, debe implementar un host de Hyper-V con dos adaptadores de red que se conectan a diferentes conmutadores físicos. También debe configurar los adaptadores de red con direcciones IP que están en el mismo intervalo de direcciones IP.  

El conmutador físico, el conmutador Virtual de Hyper-V, red de área local (LAN) y los requisitos de formación de equipos NIC para la creación de un equipo NIC en una máquina virtual son:  

-   El equipo que ejecuta Hyper-V debe tener dos o más adaptadores de red.  

-   Si los adaptadores de red se conecta a varios conmutadores físicos, deben ser los conmutadores físicos en la misma subred de nivel 2.  

-   Debe usar el Administrador de Hyper-V o Windows PowerShell para crear dos conmutadores virtuales de Hyper-V externo, cada uno conectado a un adaptador de red físico diferente.  

-   La máquina virtual debe conectarse a dos conmutadores virtuales externos que cree.  

-   Formación de equipos NIC, en Windows Server 2016, es compatible con los equipos con dos miembros en las máquinas virtuales. Puede crear los equipos más grandes, pero no hay compatibilidad.

-   Si está configurando un equipo NIC en una máquina virtual, debe seleccionar un **modo de formación de equipos** de _independiente del conmutador_ y un **modo de equilibrio de carga** de _deHashdedirección_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Paso 1. Configurar la red física y virtual  
En este procedimiento, crea dos conmutadores virtuales de Hyper-V externo, conecte una máquina virtual con los conmutadores de y, a continuación, configurar las conexiones de máquina virtual a los conmutadores.  

### <a name="prerequisites"></a>Requisitos previos

Debe tener pertenencia en **administradores**, o equivalente.  

### <a name="procedure"></a>Procedimiento

1.  En el host de Hyper-V, abra el Administrador de Hyper-V y en acciones, haga clic en **Administrador de conmutadores virtuales**.  

   ![Administrador de conmutadores virtuales](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  En el Administrador de conmutadores virtuales, asegúrese de que **externo** está seleccionada y, a continuación, haga clic en **crear conmutador Virtual**.  

   ![Crear conmutador Virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  En las propiedades del conmutador Virtual, escriba un **nombre** para el conmutador virtual y, agregar **notas** según sea necesario.  

4.  En **tipo de conexión**, en **red externa**, seleccione el adaptador de red física a la que desea asociar el conmutador virtual.  

5.  Configurar propiedades del conmutador adicionales para la implementación y, a continuación, haga clic en **Aceptar**.  

6.  Cree un segundo conmutador virtual externo repitiendo los pasos anteriores. Conectar el segundo conmutador externo a un adaptador de red diferente.  

7.  En el Administrador de Hyper-V, en **máquinas virtuales**, haga clic en la máquina virtual que desea configurar y, a continuación, haga clic en **configuración**.  

   La máquina virtual **configuración** abre el cuadro de diálogo.

8.  Asegúrese de que la máquina virtual no se ha iniciado. Si se ha iniciado, realice un cierre antes de configurar la máquina virtual.  

8.  En **Hardware**, haga clic en **adaptador de red**.  

   ![Adaptador de red](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. En **adaptador de red** propiedades, seleccione uno de los conmutadores virtuales que creó en los pasos anteriores y, a continuación, haga clic en **aplicar**.  

    ![Seleccione un conmutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. En **Hardware**, haga clic para expandir el signo más (+) junto a **adaptador de red**. 

11. Haga clic en **características avanzadas** para habilitar la formación de equipos NIC mediante la interfaz gráfica de usuario. 

    >[!TIP]
    >También puede habilitar la formación de equipos NIC con un comando de Windows PowerShell: 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Seleccione **dinámica** para la dirección MAC. 

    b. Haga clic para seleccionar **red protegido**. 

    c. Haga clic para seleccionar **habilitar este adaptador de red para formar parte de un equipo en el sistema operativo invitado**. 

    d. Haga clic en **Aceptar**.  

    ![Agregar un adaptador de red a un equipo](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Para agregar un segundo adaptador de red, en el Administrador de Hyper-V, en **máquinas virtuales**, haga clic en la misma máquina virtual y, a continuación, haga clic en **configuración**.  

    La máquina virtual **configuración** abre el cuadro de diálogo.

14. En **agregar Hardware**, haga clic en **adaptador de red**y, a continuación, haga clic en **agregar**.  

   ![Agregar un adaptador de red](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. En **adaptador de red** propiedades, seleccione el segundo conmutador virtual que creó en los pasos anteriores y, a continuación, haga clic en **aplicar**.  

   ![Aplicar un conmutador virtual](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. En **Hardware**, haga clic para expandir el signo más (+) junto a **adaptador de red**. 

17. Haga clic en **características avanzadas**, desplácese hacia abajo hasta **formación de equipos NIC**y haga clic para seleccionar **habilitar este adaptador de red para formar parte de un equipo en el sistema operativo invitado**. 

18. Haga clic en **Aceptar**.  

_ **¡Enhorabuena!** _  Ha configurado la red física y virtual.  Ahora puede continuar a la creación de un nuevo equipo NIC.  

## <a name="step-2-create-a-new-nic-team"></a>Paso 2. Crear un nuevo equipo NIC

Cuando se crea un nuevo equipo NIC, debe configurar las propiedades del equipo NIC:  

-   Nombre de equipo  

-   Adaptadores de miembros  

-   Modo de formación de equipos  

-   Modo de equilibrio de carga  

-   Adaptador de reserva  

Opcionalmente, también puede configurar la interfaz principal del equipo y configurar un número virtual de LAN (VLAN).  

Para obtener más información sobre estas opciones, consulte [configuración de formación de equipos NIC](nic-teaming-settings.md).

### <a name="prerequisites"></a>Requisitos previos

Debe tener pertenencia en **administradores**, o equivalente.  

### <a name="procedure"></a>Procedimiento

1. En el Administrador del servidor, haga clic en **Servidor local**.  

2. En el **propiedades** panel, en la primera columna, busque **formación de equipos NIC**y, a continuación, haga clic en el **deshabilitado** vínculo.  

   El **formación de equipos NIC** abre el cuadro de diálogo.

   ![Cuadro de diálogo Formación de equipos NIC](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. En **adaptadores y las Interfaces**, seleccione los adaptadores de red de uno o más que desee agregar a un equipo NIC.  

4. Haga clic en **tareas**y haga clic en **agregar al nuevo equipo**.  

   El **nuevo equipo** cuadro de diálogo se abre y muestra los adaptadores de red y los miembros del equipo.

5. En **nombre equipo**, escriba un nombre para el nuevo equipo NIC y, a continuación, haga clic en **propiedades adicionales**.  

6. En **propiedades adicionales**, seleccione los valores para:

   - **Modo de formación de equipos**. Las opciones de modo de formación de equipos son **independiente del conmutador** y **dependientes del conmutador**. Incluye el modo dependientes del conmutador **formación de equipos estática** y **protocolo de Control de agregación de vínculos (LACP)** . 

     - **Independiente de conmutador.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Dependiente del conmutador.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Formación de equipos estática**              |                                                                                                                                              Requiere que configure manualmente el conmutador y el host para identificar qué vínculos forman el equipo. Se trata de una solución configurada estáticamente, no hay ningún protocolo adicional para ayudarle en el conmutador y el host para identificar de forma incorrecta conectado cables u otros errores que podrían provocar que el equipo no se pueda realizar. Este modo lo suelen ofrecer los conmutadores de clase de servidor.                                                                                                                                              |
       | **Protocolo de Control de agregación de vínculos (LACP)** | A diferencia de formación de equipos estática, modo de formación de equipos LACP dinámicamente identifica los vínculos que están conectados entre el host y el conmutador. Esta conexión dinámica permite la creación automática de un equipo y, en teoría, pero rara vez en la práctica, la expansión y reducción de un equipo simplemente mediante la transmisión o recepción de paquetes LACP de la entidad del mismo nivel. Todos los conmutadores de clase de servidor admiten LACP y todos requieren que el operador de red habilitar administrativamente LACP en el puerto del conmutador. Al configurar un modo de formación de equipos de LACP, formación de equipos NIC funciona siempre en modo activo del LACP con un temporizador corto.  Ninguna opción está actualmente disponible para modificar el temporizador o cambiar el modo LACP. |

       ---

   - **Modo de equilibrio de carga**. Las opciones de equilibrio de carga de modo de distribución son **Hash de dirección**, **puerto Hyper-V**, y **dinámica**.

     - **Hash de dirección.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Puerto de Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dinámica.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Adaptador de reserva**. Las opciones de adaptador del modo de espera son **None (todos los adaptadores activos)** o la selección de un adaptador de red específico en el equipo NIC que actúa como un adaptador de modo de espera.

   > [!TIP]  
   > Si está configurando un equipo NIC en una máquina virtual (VM), debe seleccionar un **modo de formación de equipos** de _independiente del conmutador_ y un **modo de equilibrio de carga** de  _Hash de direcciones_.  

7. Si desea configurar el nombre de la interfaz principal del equipo o asignar un número de VLAN al equipo NIC, haga clic en el vínculo a la derecha del **interfaz de equipo principal**.  

    El **nueva interfaz de equipo** abre el cuadro de diálogo.

    ![Nueva interfaz de equipo](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. Según sus requisitos, realice una de las siguientes acciones:  

   -   Proporcione un nombre de la interfaz tNIC.  

   -   Configurar la pertenencia a VLAN: haga clic en **VLAN específica** y escriba la información de VLAN. Por ejemplo, si desea agregar este equipo NIC a la VLAN de contabilidad número 44, tipo 44 contabilidad - VLAN.   

9. Haga clic en **Aceptar**.  

_ **¡Enhorabuena!** _  Ha creado un nuevo equipo NIC en un equipo host o máquina virtual.

## <a name="related-topics"></a>Temas relacionados

- [Formación de equipos NIC](NIC-Teaming.md): En este tema, le proporcionamos una visión general de formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. Formación de equipos NIC permite agrupar entre 1 y 32 adaptadores de red Ethernet físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.   

- [Administración y uso de direcciones MAC de la formación de equipos NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Al configurar un equipo NIC con el modo independiente de conmutador y hash de dirección o distribución de carga dinámica, el equipo usa que la media access control dirección (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo desde el conjunto inicial de los miembros del equipo.

- [Configuración de formación de equipos NIC](nic-teaming-settings.md): En este tema, nos ofrecerle una visión general de las propiedades del equipo NIC como formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles sobre la configuración del adaptador en espera y la propiedad de la interfaz principal del equipo. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador de modo de espera para tolerancia a errores.

- [Solución de problemas de formación de equipos NIC](Troubleshooting-NIC-Teaming.md): En este tema, se describen formas de solucionar la formación de equipos NIC, como hardware, los valores de conmutador físico y habilitación o deshabilitación de los adaptadores de red mediante Windows PowerShell. 

---