---
title: Configuración de la réplica de Hyper-V
ms.technology: compute-hyper-v
description: Proporciona instrucciones para la configuración de réplica, conmutación por error de prueba y hacer una primera replicación.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
author: KBDAzure
ms.author: kathydav
ms.date: 10/10/2016
ms.openlocfilehash: b8dcf23946d99509aafba0f8af58bf633bedd069
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280231"
---
# <a name="set-up-hyper-v-replica"></a>Configuración de la réplica de Hyper-V

>Se aplica a: Windows Server 2016

La réplica de Hyper-V es una parte integral del rol de Hyper-V. Contribuye a su estrategia de recuperación ante desastres mediante la replicación de máquinas virtuales de un servidor host de Hyper-V a otro para mantener las cargas de trabajo disponibles.  Réplica de Hyper-V crea una copia de una máquina virtual en directo a una máquina virtual de réplica sin conexión. Tenga en cuenta lo siguiente:  

-   **Hosts de Hyper-V**: Los servidores host principales y secundarios pueden ser la misma ubicación física o en ubicaciones geográficas distintas con la replicación en una WAN vincular. Hosts de Hyper-V pueden ser independientes, en el clúster, o una combinación de ambos. No hay ninguna dependencia de Active Directory entre los servidores y no necesitan ser miembros del dominio.  

-   **Replicación y seguimiento de cambios**: Cuando se habilita la réplica de Hyper-V para una máquina virtual específica, la replicación inicial crea una máquina virtual de réplica idéntico en un servidor secundario. Cuando esto sucede, el seguimiento de cambios de réplica de Hyper-V crea y mantiene un archivo de registro que captura los cambios en una máquina virtual del disco duro virtual. El archivo de registro se reproduce en orden inverso a la réplica en que disco duro virtual en función de la configuración de frecuencia de replicación. Esto significa que los cambios más recientes se almacenan y replican de forma asincrónica. La replicación puede ser a través de HTTP o HTTPS.  

-   **Extendidos de replicación (encadenada)** : Esto le permite replicar una máquina virtual desde un host principal a un host secundario y, a continuación, se replican hospedar la base de datos secundaria para hospedar una tercera. Tenga en cuenta que no se replique desde el host principal directamente en el segundo y el tercero.  

    Esta característica hace que la réplica de Hyper-V más sólido para la recuperación ante desastres porque si se produce una interrupción puede recuperarse la réplica principal y extendida.  Puede conmutar por error al servidor réplica extendido si las ubicaciones principales y secundarias dejan de funcionar. Tenga en cuenta que el servidor réplica extendido no admite la replicación coherente con la aplicación y debe usar los mismos discos duros virtuales que usa la réplica secundaria.  

-   **Conmutación por error**: Si se produce una interrupción en su objeto principal (o extendido de base de datos secundaria en caso de) ubicación, puede iniciar manualmente una prueba, planeada o conmutación por error imprevista.  

    ||Prueba|Planeado|No planeado|  
    |-|--------|-----------|-------------|  
    |¿Cuándo debo ejecutar?|Compruebe que una máquina virtual puede conmutar por error y arrancar en el sitio secundario<br /><br />Útil para pruebas y formación|Durante el tiempo de inactividad planeado e interrupciones|Durante los eventos inesperados|  
    |¿Se crea una máquina virtual duplicada?|Sí|No|No|  
    |¿Dónde se inicia?|En la máquina virtual de réplica|Se inicia en la principal y se completa en la secundaria|En la máquina virtual de réplica|  
    |¿Con qué frecuencia debo ejecutar?|Se recomienda una vez al mes para pruebas|Una vez cada seis meses o de conformidad con los requisitos de cumplimiento|Solo en caso de desastre cuando la máquina virtual principal no está disponible|  
    |¿La máquina virtual principal sigue replicar?|Sí|Sí. Cuando termine la interrupción resolver los cambios de vuelta al sitio principal para que se sincronizan principales y secundarias se replica la replicación inversa.|No|  
    |¿Hay una pérdida de datos?|Ninguno|Ninguno. Después de la conmutación por error de réplica de Hyper-V replica el último conjunto de cambios realizados a la réplica principal para garantizar la pérdida de datos.|Depende de los puntos de recuperación y eventos|  
    |¿Hay algún tiempo de inactividad?|Ninguno. No afecta a su entorno de producción. Crea una máquina virtual de prueba duplicada durante la conmutación por error. Cuando finalice de conmutación por error seleccione **conmutación por error** en la réplica de máquinas virtuales y se ha limpiado y eliminado automáticamente.|El tiempo que dure la interrupción planeada|El tiempo que dure la interrupción no planeada|  

-   **Puntos de recuperación**: Al configurar la configuración de replicación para una máquina virtual, especifique los puntos de recuperación que desea almacenar de él. Puntos de recuperación representan una instantánea en el tiempo desde el que puede recuperar una máquina virtual. Si recupera desde un punto de recuperación muy reciente, se pierden obviamente menos datos. Puede tener acceso a los puntos de recuperación hace hasta 24 horas.  

## <a name="deployment-prerequisites"></a>Requisitos previos de implementación  
Aquí es lo que debe comprobar antes de empezar:  

-   **Averiguar qué necesidad de los discos duros virtuales se replican**. En concreto, los discos duros virtuales que contienen datos que está cambiando rápidamente y no usan el servidor de réplica después de conmutación por error, como los discos de archivo de página, se debe excluir de la replicación para conservar el ancho de banda de red. Tome nota de los cuales se pueden excluir los discos duros virtuales.  

-   **Decidir con qué frecuencia necesita sincronizar los datos**:  Los datos en el servidor de réplica se sincronizan actualizado según la frecuencia de replicación que configure (30 segundos, 5 minutos o 15 minutos). La frecuencia que elija debe considerar lo siguiente: ¿Las máquinas virtuales ejecutan datos críticos con un RPO bajo? ¿Qué está consideraciones de ancho de banda?  Las máquinas virtuales críticas altamente tendrá obviamente replicaciones más frecuentes.  

-   **Decidir cómo recuperar datos**:  De forma predeterminada, réplica de Hyper-V solo almacena un único punto de recuperación que será la última replicación enviada desde el servidor principal al secundario. Sin embargo, si desea que la opción para recuperar datos en un momento anterior en el tiempo puede especificar que se deben almacenar puntos de recuperación adicionales (hasta un máximo de 24 puntos por hora). Si necesita puntos de recuperación adicionales debe tener en cuenta que esto requiere más recursos en sobrecarga de procesamiento y almacenamiento.  

-   **Averiguar qué cargas de trabajo que se van a replicar**: Replicación de réplica de Hyper-V estándar mantiene la coherencia en el estado del sistema de operativo de máquina virtual después de una conmutación por error, pero no el estado de las aplicaciones que se ejecuta en la máquina virtual. Si desea ser capaz de puntos de recuperación coherente con la aplicación puede crear el estado de la carga de trabajo de recuperación. Tenga en cuenta que la recuperación coherente con la aplicación no está disponible en el sitio de réplica extendido si usa replicación (encadenada) extendida.  

-   **Decidir cómo realizar la replicación inicial de datos de la máquina virtual**: Se inicia la replicación mediante la transferencia de las necesidades para transferir el estado actual de las máquinas virtuales. Este estado inicial puede transmitirse directamente por la red existente, ya sea de manera inmediata o en un momento posterior que usted configure. También puede usar una máquina virtual restaurada preexistente (por ejemplo, si ha restaurado una copia de seguridad anterior de la máquina virtual en el servidor réplica) como copia inicial. O bien, puedes ahorrar ancho de banda de la red si copias la copia inicial en medios externos y, a continuación, entregas físicamente los medios al sitio donde se encuentre el servidor Réplica.  Si desea usar un preexistente máquina virtual elimina todas las instantáneas anteriores asociadas con él.  

## <a name="deployment-steps"></a>Pasos de implementación  

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Paso 1: Configurar los hosts de Hyper-V  
Necesitará al menos dos hosts de Hyper-V con una o varias máquinas virtuales en cada servidor. [Introducción a Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). El servidor host que se van a replicar las máquinas virtuales debe estar configurado como el servidor de réplica.  

1.  En la configuración de Hyper-V para el servidor a replicar máquinas virtuales en, in **configuración de replicación**, seleccione **habilitar este equipo como un servidor de réplica**.  

2.  Puede replicar a través de HTTP o HTTPS cifrados. Seleccione **usar Kerberos (HTTP)** o **usar la autenticación basada en certificados (HTTPS**). De forma predeterminada como excepciones de firewall en el servidor de Hyper-V de réplica se habilitan HTTP 80 y HTTPS 443. Si cambia la configuración del puerto predeterminado, deberá cambiar también la excepción de firewall. Si va a replicar a través de HTTPS, deberá seleccionar un certificado y debe configurar la autenticación de certificado.  

3.  Para la autorización, seleccione **permitir la replicación desde cualquier servidor autenticado** para permitir que el servidor de réplica Aceptar tráfico de replicación de máquinas virtuales desde cualquier servidor principal que se autentica correctamente. Seleccione **permitir la replicación desde los servidores especificados** para aceptar tráfico solo desde los servidores principales, seleccione específicamente.  

    Para ambas opciones puede especificar dónde se deben almacenar los discos duros virtuales replicados en el servidor de Hyper-V de réplica.  

4.  Haga clic en **Aceptar** o en **Aplicar**.  

### <a name="step-2-set-up-the-firewall"></a>Paso 2: Configurar el firewall  
Para permitir la replicación entre los servidores principales y secundarias, debe obtener el tráfico a través del firewall de Windows (o los otros firewalls de terceros). Cuando instala el rol Hyper-V en los servidores de las excepciones de forma predeterminada para HTTP (80) y HTTPS (443) se crean. Si usa estos puertos estándares, solo deberá habilitar las reglas:  

-  Para habilitar las reglas en un servidor independiente:  

    1. Abra Firewall de Windows con seguridad avanzada y haga clic en **reglas de entrada**.  

    2. Para habilitar la autenticación HTTP (Kerberos), haga clic en **escucha de HTTP de réplica de Hyper-V (TCP-In)**  >**Habilitar regla.** Para habilitar la autenticación basada en certificados HTTPS, haga clic en **escucha HTTPS de réplica Hyper-V (TCP-In)** > E**Habilitar regla**.  

-  Para habilitar las reglas en un clúster de Hyper-V, abra una sesión de Windows PowerShell con **ejecutar como administrador**, a continuación, ejecute uno de estos comandos:  

    -   Para HTTP:  `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`  

    -   Para HTTPS: `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`  

### <a name="enable-virtual-machine-replication"></a>Habilitar replicación de máquina virtual  
Siga este procedimiento en cada máquina virtual que va a replicar:  

1.  En el **detalles** panel del Administrador de Hyper-V, seleccione una máquina virtual haciendo clic en él.  
    Haga clic en la máquina virtual seleccionada y haga clic en **Habilitar replicación** para abrir el Asistente para habilitar la replicación.  

2.  En la página **Antes de comenzar** , haga clic en **Siguiente**.  

3.  En el **especificar el servidor de réplica** página, en el cuadro de servidor de réplica, escriba el nombre NetBIOS o FQDN del servidor réplica. Si el servidor réplica forma parte de un clúster de conmutación por error, escriba el nombre del agente de réplica de Hyper-V de. Haz clic en **Siguiente**.  

4.  En el **especificar parámetros de conexión** página, réplica de Hyper-V recupera automáticamente la configuración de autenticación y el puerto que ha configurado para el servidor de réplica. Si no está siendo valores recuperados de la comprobación de que el servidor está configurado como un servidor de réplica, y que está registrado en DNS. Si es necesario el tipo en la configuración manualmente.  

5.  En el **elegir replicación discos duros virtuales** página, asegúrese de que los discos duros virtuales que desea replicar están activados y desactive las casillas de los discos duros virtuales que desea excluir de la replicación. Haga clic en **Siguiente**.  

6.  En el **configurar frecuencia de replicación** , especifique con qué frecuencia se deben sincronizar los cambios del sitio principal al secundario. Haga clic en **Siguiente**.  

7.  En el **configurar puntos de recuperación adicionales** , seleccione si desea mantener el último punto de recuperación o para crear puntos adicionales.    Si desea recuperar de forma coherente las aplicaciones y cargas de trabajo que tienen sus propios escritores VSS, le recomendamos que seleccione **frequenc de servicio de instantáneas de volumen (VSS)** y y especifique con qué frecuencia se crearán instantáneas coherentes con la aplicación. Tenga en cuenta que debe estar ejecutando el servicio de solicitante de VMM de Hyper-V en los servidores Hyper-V principales y secundarios. Haga clic en **Siguiente**.  

8.  En el **elegir la replicación inicial** página, seleccione el método de replicación inicial para usar.  El valor predeterminado para enviar copia inicial a través de la red se copiará el archivo de configuración de máquina virtual principal (VMCX) y los archivos de disco duro virtual (VHD y VHDX) seleccionado a través de la conexión de red. Compruebe la disponibilidad de ancho de banda de red si va a usar esta opción. Si la máquina virtual principal ya está configurada en el sitio secundario como una máquina virtual de replicación puede ser útil seleccionar **usar una máquina virtual existente en el servidor de replicación como copia inicial**. Puede usar Hyper-V exportación para exportar la máquina virtual principal e impórtelo como una máquina virtual de réplica en el servidor secundario. Para máquinas virtuales más grandes o ancho de banda limitado puede optar por tener la replicación inicial a través de la red se producen en un momento posterior y, a continuación, configure las horas de, o enviar la información de la replicación inicial de elementos multimedia sin conexión.  

    Si lo hace la replicación sin conexión le transporte la copia inicial en el servidor secundario mediante un medio de almacenamiento externo como un disco duro o una unidad USB. Para ello, deberá conectar la externa almacenamiento al servidor principal (o nodo de propietario de un clúster) y, a continuación, al seleccionar envían copia inicial mediante medios externos, que puede especificar una ubicación local o en el medio externo donde se puede almacenar la copia inicial.  Se crea una máquina virtual de marcador de posición en el sitio de réplica. Una vez completada la replicación inicial, el almacenamiento externo puede enviarse al sitio de réplica. Se conectará el medio externo al servidor secundario o en el nodo propietario del clúster secundario. A continuación, podrá importar la réplica inicial en una ubicación especificada y combinarlo con la máquina virtual de marcador de posición.  

9. En el **completar Habilitar replicación** , revise la información del resumen y, a continuación, haga clic en **Finalizar.** . Los datos de la máquina virtual, se transferirán según la configuración elegida. y aparecerá un cuadro de diálogo indicando que la replicación se habilitó correctamente.  

10. Si desea configurar la replicación (encadenada) extendida, abra el servidor de réplica y haga clic en la máquina virtual que desea replicar. Haga clic en **replicación** > **extender replicación** y especifique la configuración de replicación.  

## <a name="run-a-failover"></a>Ejecutar una conmutación por error  
Después de completar estos pasos de implementación su entorno replicado está en funcionamiento. Ahora puede ejecutar conmutaciones por error según sea necesario.  

**Probar conmutación por error**:  Si desea ejecutar un conmutación por error de prueba con el botón secundario en la máquina virtual principal y seleccione **replicación** > **probar conmutación por error**. Elija el punto de recuperación más reciente o de otro si ha configurado. Se crea una nueva máquina virtual de prueba y ha iniciado en el sitio secundario. Cuando haya terminado las pruebas, seleccione **detener conmutación por error de prueba** en la máquina virtual de réplica para limpiarlos. Tenga en cuenta que para una máquina virtual que solo se puede ejecutar una prueba de conmutación por error a la vez. [Obtenga más](https://blogs.technet.com/b/virtualization/archive/2012/07/26/types-of-failover-operations-in-hyper-v-replica.aspx).  

**Conmutación por error planeada**: Para ejecutar un secundario de conmutación por error planeada de la máquina virtual principal y seleccione **replicación** > **conmutación por error planeada**. Conmutación por error planeada realiza comprobaciones de requisitos previos para garantizar la pérdida de datos. Comprueba que la máquina virtual principal está apagada antes de comenzar la conmutación por error. Después de la máquina virtual se conmuta por error, se inicia replicar los cambios de vuelta al sitio principal cuando esté disponible. Tenga en cuenta que para que funcione el servidor principal debe configurarse para recibir replicación desde el servidor secundario o desde el agente de réplica de Hyper-V en el caso de un clúster principal. Planifica el último conjunto de cambios sometidos a seguimiento de envíos de conmutación por error. [Obtenga más](https://blogs.technet.com/b/virtualization/archive/2012/07/31/types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover.aspx).  

**Conmutación por error imprevista**: Para ejecutar una conmutación por error no planeada, haga doble clic en la máquina virtual de réplica y seleccione **replicación** > **planeadas** desde el Administrador de Hyper-V o el Administrador de clústeres de conmutación por error. Puede recuperar desde el punto de recuperación más reciente o desde puntos de recuperación anteriores si esta opción está habilitada. Después de la conmutación por error, compruebe que todo funciona según lo previsto en el error a través de la máquina virtual, a continuación, haga clic en **completar** en la máquina virtual de réplica. [Obtenga más](https://blogs.technet.com/b/virtualization/archive/2012/08/08/types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover.aspx).  
