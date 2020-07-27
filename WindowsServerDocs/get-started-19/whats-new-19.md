---
title: Novedades de Windows Server 2019
description: Una descripción general de las nuevas funciones de Windows Server 2019, incluida la Experiencia de escritorio, el servicio de migración de almacenamiento, la información del sistema, el adaptador de red de Azure, mejoras en Espacios de almacenamiento directo y otros cambios.
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 06/04/2019
ms.openlocfilehash: fd094347679d147a04faefdf3741a06addda2026
ms.sourcegitcommit: 78b59522234825c43b00c271a04c35f3fd9d65e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2020
ms.locfileid: "86946583"
---
# <a name="whats-new-in-windows-server-2019"></a>Novedades de Windows Server 2019

> Se aplica a: Windows Server 2019

En este tema se describen algunas de las nuevas características de Windows Server 2019. Windows Server 2019 se basa en Windows Server 2016 e incluye numerosas innovaciones en cuatro temas claves: nube híbrida, seguridad, plataforma de aplicaciones e infraestructuras hiperconvergidas (HCI).

Para descubrir las novedades del canal semianual de Windows Server, consulta [Novedades de Windows Server](../get-started/whats-new-in-windows-server.md).

## <a name="general"></a>General

### <a name="windows-admin-center"></a>Windows Admin Center

Windows Admin Center es una aplicación implementada localmente, basada en explorador para la administración de servidores, clústeres, infraestructura hiperconvergida y PC con Windows 10. Se ofrece sin costo adicional además del de Windows y está listo para usarse en producción.

Puede instalar Windows Admin Center en Windows Server 2019, así como Windows 10 y versiones anteriores de Windows y Windows Server, y usarlo para administrar servidores y clústeres con Windows Server 2008 R2 y versiones posteriores.

Para obtener más información, consulta [Windows Admin Center](../manage/windows-admin-center/overview.md).

### <a name="desktop-experience"></a>Experiencia de escritorio

Dado que Windows Server 2019 es una versión de canal de servicio a largo plazo (LTSC), incluye la <b>Experiencia de escritorio</b>. (Las versiones \(SAC\) de Canal semianual no incluyen la experiencia de escritorio por diseño; son estrictamente versiones de imagen de contenedor Server Core y Nano Server). Al igual que con Windows Server 2016, durante la instalación del sistema operativo, es posible elegir entre las instalaciones de Server Core o las instalaciones de Server con Experiencia de escritorio.

### <a name="system-insights"></a>Conclusiones del sistema

Información del sistema es una nueva característica disponible en Windows Server 2019 que aporta funcionalidades de análisis predictivo local de forma nativa a Windows Server. Estas capacidades predictivas, cada una de ellas respaldada por un modelo de aprendizaje automático, analizan localmente datos del sistema de Windows Server, como los eventos y contadores de rendimiento, que proporcionan información sobre el funcionamiento de los servidores y ayudan a reducir los gastos operativos asociados a la administración reactiva de problemas en tus implementaciones de Windows Server.

## <a name="hybrid-cloud"></a>Nube híbrida

### <a name="server-core-app-compatibility-feature-on-demand"></a>Función de compatibilidad de aplicaciones de Server Core a petición

La [característica de compatibilidad de aplicaciones de Server Core a petición (FOD)](./install-fod-19.md) mejora significativamente la compatibilidad de las aplicaciones de la opción de instalación de Windows Server Core, al incluir un subconjunto de archivos binarios y componentes de Windows Server con la experiencia de escritorio sin agregar el propio entorno gráfico de la experiencia de escritorio de Windows Server.  Esto se hace para aumentar la funcionalidad y la compatibilidad de Server Core a la vez que se mantiene lo más simple posible.  

Esta característica opcional a petición está disponible en una ISO independiente y se puede agregar solo a imágenes e instalaciones de Windows Server Core mediante DISM. 

## <a name="security"></a>Seguridad

### <a name="windows-defender-advanced-threat-protection-atp"></a>Protección contra amenazas avanzada de Windows Defender (ATP)

Los sensores de plataforma profunda y las acciones de respuesta de ATP exponen ataques de nivel de kernel y memoria, y responden suprimiendo archivos malintencionados y finalizando procesos dañinos.

-   Para obtener más información acerca de ATP de Windows Defender, consulta [Información general de las funcionalidades de ATP de Windows Defender](/windows/security/threat-protection/windows-defender-atp/overview).

-   Para obtener más información sobre la incorporación de servidores, consulta [Incorporar servidores al servicio de ATP de Windows Defender](/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection).

**Protección contra vulnerabilidades de ATP de Windows Defender** es un nuevo conjunto de funcionalidades de prevención de intrusiones de host. Los cuatro componentes de Protección contra vulnerabilidades de seguridad de Windows Defender están diseñados para bloquear el dispositivo frente a una amplia variedad de vectores de ataque y bloquear comportamientos usados normalmente en ataques de malware, mientras te permite equilibrar los requisitos de productividad y riesgos de seguridad.

-   [Reducción de la superficie expuesta a ataques (ASR)](/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) es un conjunto de controles que las empresas pueden habilitar para impedir que el malware entre en la máquina bloqueando archivos sospechosos malintencionados (por ejemplo, archivos de Office), scripts, desplazamiento lateral, comportamiento de ransomware y amenazas basadas en correo electrónico.

-   [Protección de red](/windows/security/threat-protection/microsoft-defender-atp/network-protection) protege el punto de conexión frente a amenazas basadas en web bloqueando cualquier proceso de salida en el dispositivo para hosts o direcciones IP que no son de confianza a través de SmartScreen de Windows Defender.

-   [Acceso controlado a carpetas](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc) protege los datos confidenciales del ransomware bloqueando para los procesos que no son de confianza el acceso a las carpetas protegidas.

-   [Protección contra vulnerabilidades](/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) es un conjunto de las mitigaciones para vulnerabilidades de seguridad (reemplazando EMET) que se pueden configurar con facilidad para proteger sus aplicaciones y el sistema.

[Control de aplicaciones de Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (también conocido como la directiva de integridad de código (CI)) se lanzó en Windows Server 2016.
Los comentarios de los clientes sugieren que es un concepto excelente, pero difícil de implementar.
Para solucionar esta problemática, se crearon directivas de CI predeterminadas que habilitan a todos los archivos incluidos en Windows y aplicaciones de Microsoft, como SQL Server, y bloquean archivos ejecutables conocidos que pueden omitir la CI. 

### <a name="security-with-software-defined-networking-sdn"></a>Seguridad con Redes definidas por software (SDN)

[Seguridad con SDN](../networking/sdn/security/sdn-security-top.md) ofrece muchas características para aumentar la confianza del cliente en la ejecución de cargas de trabajo, de forma local, o como proveedor de servicios en la nube. 

Estas mejoras de seguridad están integradas en la plataforma completa de SDN que se introdujo en Windows Server 2016.

Para obtener una lista completa de las novedades de SDN, consulta [Novedades de SDN para Windows Server 2019](../networking/sdn/sdn-whats-new.md).

### <a name="shielded-virtual-machines-improvements"></a>Mejoras de las máquinas virtuales blindadas

- **Mejoras de sucursal**

    Ahora puedes ejecutar máquinas virtuales blindadas en máquinas con conectividad intermitente para el Servicio de protección de host al aprovechar las nuevas características [HGS reserva](../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#fallback-configuration) y [modo sin conexión](../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#offline-mode). HGS de reserva te permite configurar un segundo conjunto de direcciones URL para Hyper-V para probar si no puede conectar con el servidor HGS principal.

    El modo sin conexión te permite continuar iniciando las máquinas virtuales blindadas, incluso si no se puede conectar a HGS, siempre y cuando la máquina virtual se haya iniciado correctamente una vez y no haya cambiado la configuración de seguridad del host.

- **Solución de problemas de las mejoras**

    También hemos facilitado la [solución de problemas de las máquinas virtuales blindadas](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms.md) habilitando el soporte con el modo de sesión mejorada de VMConnect y PowerShell Direct. Estas herramientas son especialmente útiles si perdiste la conectividad de red con la VM y necesitas actualizar su configuración para restaurar el acceso. 

    No es necesario configurar estas características y están disponibles automáticamente cuando se coloca una VM blindada en un host de Hyper-V con Windows Server, versión 1803 o posterior.

- **Compatibilidad con Linux**

    Si ejecutas entornos de sistemas operativos mixtos, Windows Server 2019 admite ahora la ejecución de Ubuntu, Red Hat Enterprise Linux y SUSE Linux Enterprise Server dentro de las máquinas virtuales blindadas.

### <a name="http2-for-a-faster-and-safer-web"></a>HTTP/2 para una Web más rápida y segura

- Fusión mejorada de las conexiones para ofrecer una experiencia de exploración sin interrupciones y cifrada correctamente.

- Negociación mejorada del conjunto de aplicaciones de cifrado de lado de servidor HTTP/2 para la mitigación automática de errores de conexión y la facilidad de implementación.

- Nuestro proveedor de congestión TCP predeterminado es ahora Cubic, para ofrecerte un mayor rendimiento.

## <a name="storage"></a>Almacenamiento

Estos son algunos de los cambios que hemos realizado para el almacenamiento en Windows Server 2019. Para obtener más información, consulta [Novedades del almacenamiento](../storage/whats-new-in-storage.md).

### <a name="storage-migration-service"></a>Servicio de migración de almacenamiento

Servicio de migración de almacenamiento es una nueva tecnología que facilita migrar servidores a una versión más reciente de Windows Server. Proporciona una herramienta gráfica que inventaría los datos en los servidores, transfiere los datos y la configuración a servidores más recientes y entonces, opcionalmente, pasa las identidades de los servidores antiguos a los nuevos servidores, para que las aplicaciones y los usuarios no tengan que cambiar nada. Para obtener más información, consulta [Servicio de migración de almacenamiento](../storage/storage-migration-service/overview.md).

### <a name="storage-spaces-direct"></a>Espacios de almacenamiento directos

Esta es una lista de novedades de Espacios de almacenamiento directo. Para obtener más información, consulta [Novedades de Espacios de almacenamiento directo](../storage/whats-new-in-storage.md#storage-spaces-direct). Consulta también [Azure Stack HCI](/azure-stack/operator/azure-stack-hci-overview) para más información acerca de cómo adquirir sistemas de espacio de almacenamiento directo validados.

- **Desduplicación y compresión de volúmenes ReFS**
- **Compatibilidad nativa con memoria persistente**
- **Resistencia anidada para la infraestructura hiperconvergida de dos nodos en el borde**
- **Clústeres de dos servidores usando una unidad flash USB como testigo**
- **Soporte técnico de Windows Admin Center**
- **Historial de rendimiento**
- **Escalar hasta 4 PB por clúster**
- **La paridad acelerada por reflejos es el doble de rápida**
- **Detección de valores atípicos de latencia de unidad**
- **Delimitar manualmente la asignación de volúmenes para aumentar la tolerancia a errores**

### <a name="storage-replica"></a>Réplica de almacenamiento

Estas son las novedades de Réplica de almacenamiento. Para obtener más información, consulta [Novedades de Réplica de almacenamiento](../storage/whats-new-in-storage.md#storage-replica).

- Réplica de almacenamiento ahora está disponible en Windows Server 2019 Standard Edition.
- La conmutación por error es una nueva función que permite el montaje de almacenamiento de destino para validar la replicación o datos de copia de seguridad. Para obtener más información, consulta [Preguntas frecuentes acerca de Réplica de almacenamiento](../storage/storage-replica/storage-replica-frequently-asked-questions.md).
- Mejoras de rendimiento de registro de Réplica de almacenamiento
- Soporte técnico de Windows Admin Center

## <a name="failover-clustering"></a>Clúster de conmutación por error

Esta es una lista de novedades de clústeres de conmutación por error. Para obtener más información, consulta [Novedades de clústeres de conmutación por error](../failover-clustering/whats-new-in-failover-clustering.md).

- **Conjuntos de clústeres**
- **Clústeres con reconocimiento de Azure**
- **Migración de clúster entre dominios**
- **Testigo USB**
- **Mejoras de infraestructura de clústeres**
- **La actualización con reconocimiento de clúster es compatible con Espacios de almacenamiento directo**
- **Mejoras de testigo de recurso compartido de archivo**
- **Refuerzo de clústeres**
- **Clúster de conmutación por error ya no usa autenticación NTLM**

## <a name="application-platform"></a>Plataformas de aplicaciones

### <a name="linux-containers-on-windows"></a>Contenedores de Linux en Windows

Ahora es posible ejecutar contenedores basados en Linux y Windows en el mismo host de contenedor, usando el mismo demonio de Docker. Esto te permite tener un entorno de host de contenedor heterogéneo al tiempo que proporciona flexibilidad a los desarrolladores de aplicaciones.

### <a name="built-in-support-for-kubernetes"></a>Soporte técnico integrado para Kubernetes

Windows Server 2019 continúa con las mejoras de cálculo, redes y almacenamiento de las versiones del canal semianual necesarias para la compatibilidad con Kubernetes en Windows. Habrá más detalles disponibles en las próximas versiones de Kubernetes.

- [Redes de contenedores](../networking/sdn/technologies/containers/container-networking-overview.md) en Windows Server 2019 mejora en gran medida la facilidad de uso de Kubernetes en Windows al mejorar la resistencia de las redes de plataforma y el soporte de los complementos de redes de contenedor.

- Las cargas de trabajo implementadas en Kubernetes pueden usar la seguridad de red para proteger los servicios de Windows y Linux con herramientas incrustadas.

### <a name="container-improvements"></a>Mejoras de contenedor
    
- **Mejora de la identidad integrada**

    Hemos facilitado la autenticación integrada de Windows en contenedores y la hemos vuelto más confiable, abordando varias limitaciones de versiones anteriores de Windows Server.

- **Mejor compatibilidad de aplicaciones**

    Inclusión de aplicaciones para Windows en contenedores más fácil: La compatibilidad de aplicaciones para la imagen de *windowsservercore* existente ha aumentado. Para las aplicaciones con dependencias de API adicionales, ahora hay una imagen de terceros: *windows*.

- **Tamaño reducido y mayor rendimiento**

    Se han mejorado los tamaños de descarga de imagen de contenedor base, el tamaño en el disco y los tiempos de inicio. Esto acelera los flujos de trabajo de contenedor.

- **Experiencia de administración con Windows Admin Center \((versión preliminar)\)**

    Hemos hecho que sea más sencillo que nunca antes la acción de ver qué contenedores se ejecutan en el equipo y de administrar contenedores individuales con una nueva extensión para Windows Admin Center. Busca la extensión "Contenedores" en la [fuente pública de Windows Admin Center](../manage/windows-admin-center/configure/using-extensions.md).

### <a name="encrypted-networks"></a>Redes cifradas

[Redes cifradas](../networking/sdn/sdn-whats-new.md): el cifrado de red virtual permite el cifrado del tráfico de red virtual entre máquinas virtuales que se comunican entre sí dentro de subredes marcadas como **Cifrado habilitado**. También utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes. DTLS protege frente a las interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física.

### <a name="network-performance-improvements-for-virtual-workloads"></a>Mejoras de rendimiento de red para las cargas de trabajo virtuales

Las [mejoras de rendimiento de red para las cargas de trabajo virtuales](../networking/technologies/hpn/hpn-insider-preview.md) maximizan el rendimiento de red para las máquinas virtuales sin necesidad de realizar ajustes constantemente o de aprovisionar en exceso al host. Esto reduce las operaciones y el costo de mantenimiento a la vez que aumenta la densidad disponible de los hosts. Estas nuevas características son:

* Recibir segmentos de recepción en el vSwitch

* Cola múltiple de máquina virtual dinámica (d.VMMQ)

### <a name="low-extra-delay-background-transport"></a>Transporte en segundo plano de retraso adicional bajo

El transporte en segundo plano de retraso adicional bajo (LEDBAT) es un proveedor de controles de congestión de red de optimización de latencia diseñado para producir automáticamente ancho de banda a usuarios y aplicaciones, a la vez que consume todo el ancho de banda disponible cuando no se está usando la red.   
Esta tecnología está pensada para usarla en la implementación de actualizaciones críticas de gran tamaño en un entorno de TI sin afectar a los servicios orientados a los clientes y al ancho de banda asociado.

### <a name="windows-time-service"></a>Servicio de hora de Windows

El [Servicio de hora de Windows](../networking/windows-time-service/insider-preview.md) incluye soporte de segundo intercalar compatible con UTC real, un nuevo protocolo de hora denominado protocolo de tiempo de precisión y rastreabilidad integral.


### <a name="high-performance-sdn-gateways"></a>Puertas de enlace SDN de alto rendimiento

[Puertas de enlace SDN de alto rendimiento](../networking/sdn/gateway-performance.md) en Windows Server 2019 mejora considerablemente el rendimiento para las conexiones IPsec y GRE, proporcionando un rendimiento muy elevado con un uso mucho menor de la CPU.
<br/>

### <a name="new-deployment-ui-and-windows-admin-center-extension-for-sdn"></a>Nueva extensión de Windows Admin Center e interfaz de usuario de implementación para SDN

Ahora, con Windows Server 2019, la implementación y administración son fáciles a través de una nueva interfaz de usuario de implementación y una extensión de Windows Admin Center que permiten que cualquier persona pueda aprovechar la capacidad de SDN. 

### <a name="persistent-memory-support-for-hyper-v-vms"></a>Soporte de memoria persistente para máquinas virtuales de Hyper-V

Para aprovechar el alto rendimiento y la baja latencia de la memoria persistente (también conocida como memoria de clase de almacenamiento) en máquinas virtuales, ahora se puede proyectar directamente en las máquinas virtuales. Esto puede ayudar a reducir drásticamente la latencia de transacción de base de datos o reducir los tiempos de recuperación para bases de datos en memoria de latencia baja en caso de error.
