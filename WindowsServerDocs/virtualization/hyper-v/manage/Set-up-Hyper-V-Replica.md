---
title: Configuración de la réplica de Hyper-V
description: Proporciona instrucciones para configurar la réplica, probar la conmutación por error y realizar una primera replicación.
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/10/2016
ms.openlocfilehash: f34a44ef1e721d2e217fb7c6dd0915ed87007eda
ms.sourcegitcommit: f86cd62a3d43de656b7b9d8e54118a52fda2753b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91844737"
---
# <a name="set-up-hyper-v-replica"></a>Configuración de la réplica de Hyper-V

> Se aplica a: Windows Server 2016

La réplica de Hyper-V es una parte integral del rol de Hyper-V. Contribuye a su estrategia de recuperación ante desastres mediante la replicación de máquinas virtuales de un servidor host de Hyper-V a otro para mantener las cargas de trabajo disponibles.  Réplica de Hyper-V crea una copia de una máquina virtual en directo en una máquina virtual de réplica sin conexión. Tenga en cuenta lo siguiente:

- **Hosts de Hyper-V**: los servidores de host principal y secundario pueden estar colocados físicamente o en ubicaciones geográficas independientes con replicación a través de un vínculo WAN. Los hosts de Hyper-V pueden ser independientes, agrupados o una combinación de ambos. No hay ninguna dependencia Active Directory entre los servidores y no es necesario que sean miembros del dominio.

- **Replicación y seguimiento de cambios**: cuando se habilita la réplica de Hyper-V para una máquina virtual específica, la replicación inicial crea una máquina virtual de réplica idéntica en un servidor host secundario. Una vez hecho esto, el seguimiento de cambios de réplica de Hyper-V crea y mantiene un archivo de registro que captura los cambios en un VHD de máquina virtual. El archivo de registro se reproduce en orden inverso al VHD de réplica en función de la configuración de la frecuencia de replicación. Esto significa que los cambios más recientes se almacenan y replican de forma asincrónica. La replicación puede realizarse a través de HTTP o HTTPS.

- **Replicación extendida (encadenada)**: permite replicar una máquina virtual desde un host principal a un host secundario y, a continuación, replicar el host secundario en un tercer host. Tenga en cuenta que no puede replicar desde el host principal directamente en el segundo y el tercero.

    Esta característica hace que la réplica de Hyper-V sea más sólida para la recuperación ante desastres, ya que si se produce una interrupción, se puede recuperar de la réplica principal y la réplica extendida.  Puede conmutar por error a la réplica extendida si la ubicación principal y la secundaria dejan de funcionar. Tenga en cuenta que la réplica extendida no admite la replicación coherente con la aplicación y debe utilizar los mismos discos duros virtuales que usa la réplica secundaria.

- **Conmutación por error**: si se produce una interrupción en la ubicación principal (o secundaria en caso de extensión extendida), puede iniciar manualmente una conmutación por error de prueba, planeada o no planeada.

    | Pregunta | Prueba | Planeado | No planeado |
    |--|--|--|--|
    | ¿Cuándo debo ejecutarlo? | Comprobar que una máquina virtual puede conmutar por error e iniciar en el sitio secundario<p>Útil para pruebas y entrenamiento | Durante las interrupciones y el tiempo de inactividad planeado | Durante eventos inesperados |
    | ¿Se ha creado una máquina virtual duplicada? | Sí | No | No |
    | ¿Dónde se inicia? | En la máquina virtual de réplica | Se inicia en la principal y se completa en la secundaria | En la máquina virtual de réplica |
    | ¿Con qué frecuencia se debe ejecutar? | Se recomienda una vez al mes para las pruebas | Una vez cada seis meses o de acuerdo con los requisitos de cumplimiento | Solo en caso de desastre cuando la máquina virtual principal no está disponible |
    | ¿La máquina virtual principal continúa replicándose? | Sí | Sí. Cuando se resuelve la interrupción, la replicación inversa replica los cambios de nuevo en el sitio primario para que las principales y secundarias se sincronicen. | No |
    | ¿Hay alguna pérdida de datos? | None | Ninguno. Después de la conmutación por error, réplica de Hyper-V replica el último conjunto de cambios sometidos a seguimiento en el servidor principal para garantizar que no se pierdan datos. | Depende del evento y los puntos de recuperación |
    | ¿Hay algún tiempo de inactividad? | Ninguno. No afecta al entorno de producción. Crea una máquina virtual de prueba duplicada durante la conmutación por error. Una vez finalizada la conmutación por error, seleccione **conmutación por error** en la máquina virtual de réplica y se limpia y elimina automáticamente. | La duración de la interrupción planeada | La duración de la interrupción no planeada |

- **Puntos de recuperación**: al configurar las opciones de replicación de una máquina virtual, se especifican los puntos de recuperación que se desean almacenar en ella. Los puntos de recuperación representan una instantánea en el tiempo desde la que puede recuperar una máquina virtual. Obviamente se pierden menos datos si se recupera desde un punto de recuperación muy reciente. Puede tener acceso a los puntos de recuperación hace hasta 24 horas.

## <a name="deployment-prerequisites"></a>Requisitos previos de implementación

Esto es lo que debe comprobar antes de empezar:

- **Averigüe qué discos duros virtuales deben replicarse**. En concreto, los VHD que contienen datos que cambian rápidamente y que no usa el servidor réplica después de la conmutación por error, como los discos de archivo de paginación, deben excluirse de la replicación para conservar el ancho de banda de red. Anote los discos duros virtuales que se pueden excluir.

- **Decida con qué frecuencia necesita sincronizar los datos**: los datos del servidor réplica se sincronizan de acuerdo con la frecuencia de replicación que configure (30 segundos, 5 minutos o 15 minutos). La frecuencia que elija debe tener en cuenta lo siguiente: ¿las máquinas virtuales ejecutan datos críticos con un RPO bajo? ¿Cuáles son las consideraciones de ancho de banda?  Las máquinas virtuales de gran importancia tendrán obviamente una replicación más frecuente.

- **Decidir cómo recuperar datos**: de forma predeterminada, la réplica de Hyper-V solo almacena un único punto de recuperación que será la replicación más reciente enviada desde el servidor principal al secundario. Sin embargo, si desea que la opción recupere los datos a un momento anterior en el tiempo, puede especificar que se deben almacenar puntos de recuperación adicionales (hasta un máximo de 24 puntos cada hora). Si necesita puntos de recuperación adicionales, debe tener en cuenta que esto requiere más sobrecarga en los recursos de procesamiento y almacenamiento.

- **Averigüe qué cargas de trabajo va a replicar**: la replicación de réplicas de Hyper-V estándar mantiene la coherencia en el estado del sistema operativo de la máquina virtual después de una conmutación por error, pero no el estado de las aplicaciones que se ejecutan en la máquina virtual. Si desea poder recuperar el estado de la carga de trabajo, puede crear puntos de recuperación coherentes con la aplicación. Tenga en cuenta que la recuperación coherente con la aplicación no está disponible en el sitio de réplica extendida si utiliza la replicación extendida (encadenada).

- **Decidir cómo realizar la replicación inicial de los datos de la máquina virtual**: la replicación se inicia mediante la transferencia de las necesidades para transferir el estado actual de las máquinas virtuales. Este estado inicial puede transmitirse directamente por la red existente, ya sea de manera inmediata o en un momento posterior que usted configure. También puede usar una máquina virtual restaurada ya existente (por ejemplo, si ha restaurado una copia de seguridad anterior de la máquina virtual en el servidor réplica) como copia inicial. O bien, puede ahorrar ancho de banda de la red si copia la copia inicial en medios externos y, a continuación, entrega físicamente los medios al sitio donde se encuentre el servidor Réplica.  Si desea usar una máquina virtual preexistente, elimine todas las instantáneas anteriores asociadas a ella.

## <a name="deployment-steps"></a>Pasos de implementación

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Paso 1: configuración de los hosts de Hyper-V

Necesitará al menos dos hosts de Hyper-V con una o más máquinas virtuales en cada servidor. [Introducción a Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). El servidor host en el que va a replicar las máquinas virtuales tendrá que configurarse como el servidor de réplicas.

1. En la configuración de Hyper-V del servidor en el que va a replicar las máquinas virtuales, en **configuración de replicación**, seleccione **habilitar este equipo como servidor de réplicas**.

2. Puede replicar a través de HTTP o HTTPS cifrado. Seleccione **usar Kerberos (http)** o **usar autenticación basada en certificados (https**). De forma predeterminada, HTTP 80 y HTTPS 443 están habilitadas como excepciones de firewall en el servidor de Hyper-V de réplica. Si cambia la configuración de Puerto predeterminada, deberá cambiar también la excepción de Firewall. Si va a replicar a través de HTTPS, deberá seleccionar un certificado y debe tener la autenticación de certificado configurada.

3. En autorización, seleccione **permitir la replicación desde cualquier servidor autenticado** para permitir que el servidor réplica acepte el tráfico de replicación de máquinas virtuales de cualquier servidor principal que se autentique correctamente. Seleccione **permitir la replicación desde los servidores especificados** para aceptar solo el tráfico de los servidores principales que seleccione específicamente.

    En ambas opciones puede especificar dónde se deben almacenar los discos duros virtuales replicados en el servidor de Hyper-V de réplica.

4. Haga clic en **Aceptar** o en **Aplicar**.

### <a name="step-2-set-up-the-firewall"></a>Paso 2: configuración del firewall

Para permitir la replicación entre los servidores principal y secundario, el tráfico debe pasar por el Firewall de Windows (o cualquier otro Firewall de terceros). Al instalar el rol de Hyper-V en los servidores de de forma predeterminada, se crean excepciones para HTTP (80) y HTTPS (443). Si usa estos puertos estándar, solo tendrá que habilitar las reglas:

-  Para habilitar las reglas en un servidor host independiente:

    1. Abra Firewall de Windows con seguridad avanzada y haga clic en **Reglas de entrada**.

    2. Para habilitar la autenticación HTTP (Kerberos), haga clic con el botón derecho en el **agente de escucha http de réplica de Hyper-V (TCP-in)**  >  **Habilitar regla**. Para habilitar la autenticación basada en certificados HTTPS, haga clic con el botón derecho en el **agente de escucha https de réplica de Hyper-V (TCP-in)**  >  **Habilitar regla**.

-  Para habilitar las reglas en un clúster de Hyper-V, abra una sesión de Windows PowerShell con **Ejecutar como administrador**y luego ejecute uno de estos comandos:

    - Para HTTP:  `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`

    - Para HTTPS: `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`

### <a name="enable-virtual-machine-replication"></a>Habilitar la replicación de máquinas virtuales

Haga lo siguiente en cada máquina virtual que quiera replicar:

1. En el panel de **detalles** del administrador de Hyper-V, haga clic en una máquina virtual para seleccionarla.
    Haga clic con el botón secundario en la máquina virtual seleccionada y haga clic en **Habilitar replicación** para abrir el Asistente para habilitar replicación.

2. En la página **Antes de comenzar**, haga clic en **Siguiente**.

3. En la página **especificar servidor de réplicas** , en el cuadro servidor de réplicas, escriba el nombre NetBIOS o el FQDN del servidor réplica. Si el servidor réplica forma parte de un clúster de conmutación por error, escriba el nombre del agente de réplicas de Hyper-V. Haga clic en **Next**.

4. En la página **especificar parámetros de conexión** , réplica de Hyper-V recupera automáticamente la configuración de autenticación y puerto que configuró para el servidor réplica. Si los valores no se recuperan, compruebe que el servidor está configurado como un servidor de réplicas y que está registrado en DNS. Si es necesario, escriba manualmente la configuración.

5. En la página **elegir discos duros virtuales de replicación** , asegúrese de que están seleccionados los discos duros virtuales que desea replicar y desactive las casillas de los discos duros virtuales que desee excluir de la replicación. A continuación, haga clic en **Siguiente**.

6. En la página **configurar frecuencia de replicación** , especifique con qué frecuencia se deben sincronizar los cambios de principal a secundario. A continuación, haga clic en **Siguiente**.

7. En la página **configurar puntos de recuperación adicionales** , seleccione si desea mantener solo el punto de recuperación más reciente o crear puntos adicionales. Si desea recuperar de forma coherente aplicaciones y cargas de trabajo que tienen sus propios escritores de VSS, le recomendamos que seleccione **servicio de instantáneas de volumen (VSS)** y especifique la frecuencia de creación de instantáneas coherentes con la aplicación. Tenga en cuenta que el servicio de solicitante de VMM de Hyper-V debe ejecutarse en los servidores de Hyper-V principal y secundario. A continuación, haga clic en **Siguiente**.

8. En la página **elegir la replicación inicial** , seleccione el método de replicación inicial que se va a usar.  La configuración predeterminada para enviar copia inicial a través de la red copiará el archivo de configuración de la máquina virtual principal (VMCX) y los archivos de disco duro virtual (VHDX y VHD) seleccionados a través de la conexión de red. Compruebe la disponibilidad del ancho de banda de red si va a usar esta opción. Si la máquina virtual principal ya está configurada en el sitio secundario como una máquina virtual de replicación, puede ser útil seleccionar  **usar una máquina virtual existente en el servidor de replicación como copia inicial**. Puede usar la exportación de Hyper-V para exportar la máquina virtual principal e importarla como una máquina virtual de réplica en el servidor secundario. En el caso de las máquinas virtuales más grandes o el ancho de banda limitado, puede elegir que la replicación inicial a través de la red se realice más tarde y, a continuación, configurar las horas de poca actividad, o bien enviar la información de replicación inicial como medio sin conexión.

    Si realiza la replicación sin conexión, transportará la copia inicial en el servidor secundario mediante un medio de almacenamiento externo, como un disco duro o una unidad USB. Para ello, necesitará conectar el almacenamiento externo al servidor principal (o al nodo propietario de un clúster) y, a continuación, al seleccionar enviar copia inicial mediante medios externos, puede especificar una ubicación localmente o en el medio externo donde se puede almacenar la copia inicial.  Se crea una máquina virtual de marcador de posición en el sitio de réplica. Una vez completada la replicación inicial, se puede enviar el almacenamiento externo al sitio de réplica. Allí conectará el medio externo al servidor secundario o al nodo propietario del clúster secundario. A continuación, importará la réplica inicial en una ubicación especificada y la combinará en la máquina virtual del marcador de posición.

9. En la página **finalización de la habilitación de la replicación** , revise la información del Resumen y, a continuación, haga clic en **Finalizar.** Los datos de la máquina virtual se transferirán de acuerdo con la configuración elegida. y aparece un cuadro de diálogo que indica que la replicación se ha habilitado correctamente.

10. Si desea configurar la replicación extendida (encadenada), abra el servidor réplica y haga clic con el botón secundario en la máquina virtual que desea replicar. Haga clic en **replicación**  >  **extender replicación** y especifique la configuración de replicación.

## <a name="run-a-failover"></a>Ejecución de la conmutación por error

Después de completar estos pasos de implementación, el entorno replicado está en funcionamiento. Ahora puede ejecutar conmutaciones por error según sea necesario.

**Conmutación por error de prueba**: Si desea ejecutar una conmutación por error de prueba, haga clic con el **Replication**botón derecho en la máquina virtual principal y seleccione  >  **conmutación por error de prueba**de replicación. Seleccione el punto de recuperación más reciente u otro punto de recuperación si está configurado. Se creará una nueva máquina virtual de prueba y se iniciará en el sitio secundario. Una vez finalizada la prueba, seleccione  **detener conmutación por error de prueba** en la máquina virtual de réplica para limpiarla. Tenga en cuenta que para una máquina virtual solo puede ejecutar una conmutación por error de prueba a la vez. [Más información](https://docs.microsoft.com/virtualization/community/team-blog/2012/20120725-types-of-failover-operations-in-hyper-v-replica-part-i-test-failover).

**Conmutación por error planeada**: para ejecutar una conmutación por error planeada haga clic con **Replication**el botón derecho en la máquina virtual principal y seleccione  >  **conmutación por error planeada**de replicación. La conmutación por error planeada realiza comprobaciones de requisitos previos para garantizar que no se produzca ninguna pérdida de datos. Comprueba que la máquina virtual principal está apagada antes de comenzar la conmutación por error. Una vez realizada la conmutación por error de la máquina virtual, comienza a replicar los cambios de nuevo en el sitio primario cuando está disponible. Tenga en cuenta que para que funcione el servidor principal, debe configurarse para recepción la replicación desde el servidor secundario o desde el agente de réplicas de Hyper-V en el caso de un clúster principal. La conmutación por error planeada envía el último conjunto de cambios sometidos a seguimiento. [Más información](https://docs.microsoft.com/virtualization/community/team-blog/2012/20120731-types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover).

**Conmutación por error no planeada**: para ejecutar una conmutación por error no planeada, haga clic con el botón derecho en la máquina virtual de réplica y seleccione **replicación**no  >  **planeada conmutación por error** desde el administrador de Hyper-V o el administrador de clústeres de conmutación por error. Si esta opción está habilitada, puede recuperar desde el punto de recuperación más reciente o desde puntos de recuperación anteriores. Después de la conmutación por error, compruebe que todo funciona de la forma esperada en la máquina virtual conmutada por error y, a continuación, haga clic en **completar** en la máquina virtual de réplica. [Más información](https://docs.microsoft.com/virtualization/community/team-blog/2012/20120808-types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover).
