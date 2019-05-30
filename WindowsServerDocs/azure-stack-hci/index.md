---
title: Introducción a Azure Stack HCI
description: Azure Stack HCI es un clúster de Windows Server 2019 hiperconvergido que usa hardware validado para ejecutar cargas de trabajo virtualizadas en el entorno local y, opcionalmente, se conecta a servicios de Azure de copia de seguridad en la nube, recuperación de sitios y mucho más. Las soluciones de Azure Stack HCI usan hardware validado por Microsoft para garantizar la confiabilidad y un rendimiento óptimo, e incluyen compatibilidad con tecnologías como unidades NVMe, memoria persistente y redes de acceso directo a memoria remota (RDMA).
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/22/2019
ms.openlocfilehash: 92d600eeb833cd70bd714702b1fd950c4fe2cd87
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008974"
---
# <a name="azure-stack-hci-overview"></a>Introducción a Azure Stack HCI

>Se aplica a: Windows Server 2019

Azure Stack HCI es un clúster de Windows Server 2019 hiperconvergido que usa hardware validado para ejecutar cargas de trabajo virtualizadas en el entorno local y, opcionalmente, se conecta a servicios de Azure de copia de seguridad en la nube, recuperación de sitios y mucho más. Las soluciones de Azure Stack HCI usan hardware validado por Microsoft para garantizar la confiabilidad y un rendimiento óptimo, e incluyen compatibilidad con tecnologías como unidades NVMe, memoria persistente y redes de acceso directo a memoria remota (RDMA).

Azure Stack HCI es una solución que combina varios productos:

- Hardware de un socio OEM

- Windows Server 2019 Datacenter Edition

- Windows Admin Center

- Servicios de Azure (opcionales)

![Azure Stack HCI es la solución hiperconvergida de Microsoft disponible en una amplia gama de asociados de hardware.](media/AS_HCI_solution.png)

Azure Stack HCI es la solución hiperconvergida de Microsoft disponible en una amplia gama de asociados de hardware. Examine los siguientes escenarios para una solución hiperconvergida para ayudarle a determinar si Azure Stack HCI es la solución que mejor se adapta a sus necesidades:

- **Actualización de hardware antiguo.** Reemplace los servidores y la infraestructura de almacenamiento más antiguos y ejecute máquinas virtuales Windows y Linux en entorno local y perimetral con las herramientas y aptitudes de TI que ya tiene.

- **Consolidación de cargas de trabajo virtualizadas.** Consolide las aplicaciones heredadas en una infraestructura hiperconvergida eficaz. Disfrute de las mismas prestaciones eficientes que la nube utiliza para ejecutar centros de datos a hiperescala, como Microsoft Azure.

- **Conéctese a Azure para utilizar servicios en la nube híbrida.** Optimice el acceso a los servicios de seguridad y administración de la nube de Azure, incluidos copia de seguridad, recuperación de sitios, supervisión basada en la nube y mucho más.

## <a name="the-azure-stack-family"></a>La familia de Azure Stack

Azure Stack HCI es parte de la familia de Azure y Azure Stack, y utiliza el mismo software de proceso, almacenamiento y red que Azure Stack. Este es un resumen rápido de las diferentes soluciones:

- [Azure](https://azure.microsoft.com): use servicios en la nube pública.
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack): use servicios en la nube en el entorno local.
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci): ejecute aplicaciones virtualizadas en el entorno local con conexiones opcionales a Azure.

![Azure y Azure Stack ejecutan servicios en la nube, mientras que Azure Stack HCI ejecuta aplicaciones virtualizadas en el entorno local.](media/azure_family.png)

|Azure: use servicios en la nube pública|Azure Stack: use servicios en la nube en el entorno local|Azure Stack HCI: ejecute aplicaciones virtualizadas en el entorno local|
|-----------------|-----------------|-----------------|
|Para recursos de proceso de autoservicio a petición, para migrar y modernizar las aplicaciones existentes y crear nuevas aplicaciones nativas de la nube.|Compilar y ejecutar aplicaciones en la nube en la red perimetral, en caso de desconexión o para cumplir los requisitos normativos, mediante el uso de servicios de Azure coherentes en el entorno local.| Ejecutar aplicaciones virtualizadas en el entorno local, reemplazar y consolidar la antigua infraestructura de servidores y conectarse a Azure para utilizar servicios en la nube.|
|Más de 100 servicios disponibles en 54 regiones del mundo|Máquinas virtuales de Azure para Windows y Linux, Azure Web Apps y Azure Functions, Azure Key Vault, Azure Resource Manager, Azure Marketplace, contenedores, Azure IoT Hub y Azure Event Hubs, herramientas de administración (planes, ofertas, RBAC)|Soluciones HCI validadas con la tecnología Hyper-V y Espacios de almacenamiento directo con Windows Server 2019 y Windows Admin Center para la administración y el acceso integrado a los servicios de Azure.|

Para más información:

- Visite nuestro sitio web de soluciones de [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci).
- Acompañe a los expertos de Microsoft Jeff Woolsey y Vijay Tewari mientras [nos hablan sobre las nuevas soluciones de Azure Stack HCI](https://aka.ms/AzureStackOverviewVideo).

## <a name="hyperconverged-efficiencies"></a>Eficiencias hiperconvergidas

Las soluciones de Azure Stack HCI reúnen recursos virtualizados de red, proceso y almacenamiento en componentes y servidores x86 estándar. La combinación de recursos en el mismo clúster hace que resulte más fácil implementar, administrar y escalar. Elija la opción de administración: automatización de la línea de comandos o Windows Admin Center.

Consiga un rendimiento de máquina virtual óptimo para sus aplicaciones de servidor con Hyper-V, la tecnología de hipervisor fundamental de la nube de Microsoft, y la tecnología Espacios de almacenamiento directo con compatibilidad integrada para NVMe, memoria persistente y redes de acceso directo a memoria remota (RDMA).

Ayuda a proteger las aplicaciones y los datos con máquinas virtuales blindadas, microsegmentation de red y cifrado nativo de los datos en reposo y en tránsito.

## <a name="hybrid-capabilities"></a>Capacidades híbridas

Ahora puede trabajar con la nube y los entornos locales a la vez, y sacar partido de una plataforma de infraestructura hiperconvergida en la nube pública. El equipo puede empezar a desarrollar aptitudes en la nube con la integración incorporada en los servicios de administración de infraestructura de Azure:

- Azure Site Recovery para alta disponibilidad y recuperación ante desastres como servicio (DRaaS).

- Azure Monitor, un centro desde donde se realiza el seguimiento de lo que sucede entre las aplicaciones, la red y la infraestructura, análisis avanzados basados en inteligencia artificial.

- Cloud Witness, para usar Azure como agente de intermediación ligero del quórum de clúster.

- Azure Backup, para proteger los datos fuera del sitio y para protegerse contra ransomware.

- Azure Update Management, para evaluar las actualizaciones y la implementación de las actualizaciones en las máquinas virtuales Windows que se ejecutan en Azure y en el entorno local.

- Adaptador de red de Azure, para conectarse a los recursos locales con las máquinas virtuales en Azure mediante una red privada virtual de punto a sitio.

- Sincronización del servidor de archivos con la nube con Azure File Sync.

Para más información, consulte cómo [conectar Windows Server a los servicios híbridos de Azure](..\manage\windows-admin-center\azure\index.md).

## <a name="management-tools-and-system-center"></a>Herramientas de administración y System Center

Azure Stack HCI usa el mismo software de virtualización y de red y almacenamiento definido por software que Azure Stack. Con Azure Stack HCI, tiene derechos administrativos completos en el clúster: puede usar [Windows Admin Center](..\manage\windows-admin-center\overview.md), [System Center](https://www.microsoft.com/cloud-platform/system-center) y todas las características de [Hyper-V](../virtualization/hyper-v/hyper-v-on-windows-server.md), [ Espacios de almacenamiento directo](..\storage\storage-spaces\storage-spaces-direct-overview.md), PowerShell y herramientas de terceros, como 5Nine Manager.

Microsoft Azure se ejecuta en el mismo Windows Server y plataforma Hyper-V que la incluida en Windows Server. Windows Server y System Center incluyen mejoras y procedimientos recomendados obtenidos de la experiencia de Microsoft en la utilización de redes de centro de datos de escala global, como Microsoft Azure, para que pueda implementar las mismas tecnologías y obtener flexibilidad, automatización y control a la hora de usar las tecnologías de red diseñadas mediante software.

Implemente y administre la infraestructura con System Center Virtual Machine Management (VMM) y System Center Operations Manager. Con VMM, se aprovisionan y administran los recursos necesarios para crear e implementar máquinas virtuales y servicios en nubes privadas. Con Operations Manager, se supervisan los servicios, los dispositivos y las operaciones en toda la empresa para identificar problemas y tomar medidas de inmediato.

## <a name="hardware-partners"></a>Asociados de hardware

Puede adquirir las soluciones de Azure Stack HCI validadas que ejecutan Windows Server 2019 a 15 asociados. Su asociado de Microsoft preferido le ayudará a ponerse en funcionamiento sin largas esperas durante el diseño y la creación, y le ofrece un único punto de contacto para los servicios de implementación y soporte técnico.

Visite el [sitio web de Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) para ver nuestras 70 soluciones de Azure Stack HCI disponibles actualmente en estos asociados de Microsoft: ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, primeLine Solutions, QCT, SecureGUARD y Supermicro.

## <a name="faq"></a>Preguntas más frecuentes

### <a name="what-do-azure-stack-and-azure-stack-hci-solutions-have-in-common"></a>¿Qué tienen en común las soluciones de Azure Stack y de Azure Stack HCI?

Las soluciones de Azure Stack HCI cuentan con las mismas tecnologías de proceso, almacenamiento y red definidas por software y basadas en Hyper-V. Ambas ofertas cumplen los criterios más exigentes de pruebas y validación para garantizar la confiabilidad y compatibilidad con la plataforma de hardware subyacente.

### <a name="how-are-they-different"></a>¿En qué se diferencian?

Con Azure Stack, se ejecutan servicios en la nube en el entorno local. Puede ejecutar servicios IaaS y PaaS de Azure en el entorno local para compilar y ejecutar aplicaciones de nube en cualquier lugar, administradas con Azure Portal en el entorno local.

Con Azure Stack HCI, es ejecutan cargas de trabajo virtualizadas en el entorno local y se administran con Windows Admin Center y con herramientas conocidas de Windows Server. También puede conectarse a Azure para utilizar escenarios híbridos, como la supervisión o la recuperación de sitios basada en la nube, entre otros.

### <a name="why-is-microsoft-bringing-its-hci-offering-to-the-azure-stack-family"></a>¿Por qué Microsoft incorpora su oferta HCI a la familia de Azure Stack?

La tecnología hiperconvergida de Microsoft ya es la base de Azure Stack.

Muchos clientes de Microsoft tienen entornos de TI complejos y nuestro objetivo es proporcionar soluciones que les ofrezcan la tecnología adecuada para cada necesidad específica de la empresa. Azure Stack HCI es una evolución de las soluciones definidas por software de Windows Server 2016 (WSSD), que estaban disponibles con nuestros asociados de hardware. Lo hemos incorporado a la familia de Azure Stack porque hemos comenzado a ofrecer nuevas opciones para conectarse sin problemas con Azure para utilizar servicios de administración de la infraestructura.

### <a name="does-azure-stack-hci-need-to-be-connected-to-azure"></a>¿Azure Stack HCI necesita estar conectado a Azure?

No, es totalmente opcional. Puede aprovechar la integración con Azure en escenarios híbridos, por ejemplo, administración de actualizaciones, supervisión basada en la nube y copia de seguridad y recuperación ante desastres fuera del sitio, pero son opcionales. Comprendemos totalmente que prefiera o deba funcionar sin conexión.

### <a name="how-does-azure-stack-hci-relate-to-windows-server"></a>¿Qué relación tiene Azure Stack HCI con Windows Server?

Windows Server 2019 es la base de casi todos los productos de Azure. Todas las características que aprecia seguirán basándose y siendo compatibles con Windows Server. Azure Stack HCI es la manera recomendada de implementar HCI en el entorno local, utilizando hardware validado por Microsoft de nuestros asociados.

### <a name="will-i-be-able-to-upgrade-from-azure-stack-hci-to-azure-stack"></a>¿Podré actualizar de Azure Stack HCI a Azure Stack? 

No, pero los clientes pueden migrar sus cargas de trabajo de Azure Stack HCI a Azure Stack o Azure.

### <a name="what-azure-services-can-i-connect-to-azure-stack-hci"></a>¿Qué servicios de Azure puedo conectar a Azure Stack HCI?

Para obtener una lista actualizada de los servicios de Azure que puede conectar a Azure Stack HCI, consulte cómo [conectar Windows Server a los servicios híbridos de Azure](../azure-hybrid-services/index.md).

### <a name="how-does-the-cost-of-azure-stack-hci-compare-to-azure-stack"></a>¿Cuál es el costo de Azure Stack HCI en comparación con Azure Stack? 

Azure Stack se vende como un sistema completamente integrado que incluye servicios y soporte técnico. Puede adquirir Azure Stack como un sistema que administra, o como un servicio totalmente administrado de nuestros asociados. Además del sistema base, los servicios de Azure que se ejecutan en Azure Stack o en Azure se venden con la modalidad de pago por uso.

Las soluciones de Azure Stack HCI siguen el modelo de compra tradicional. Puede adquirir a los asociados de Azure Stack HCI hardware validado y software (Windows Server 2019 Datacenter Edition con funcionalidades de centro de datos definidas por software y Windows Admin Center) en varios canales disponibles. En el caso de los servicios de Azure que puede usar con Windows Admin Center, se pagan con una suscripción de Azure.

### <a name="how-do-i-buy-azure-stack-hci-solutions"></a>¿Cómo puedo comprar soluciones de Azure Stack HCI?

Siga estos pasos:

1. Adquiera un sistema de hardware validado por Microsoft al asociado de hardware que prefiera.
1. Instale Windows Server 2019 Datacenter Edition y Windows Admin Center para la administración y para poder conectarse a Azure y utilizar los servicios en la nube.
1. También puede utilizar su cuenta de Azure para conectar los servicios de administración y seguridad basados en la nube a las cargas de trabajo.

![Para comprar soluciones de Azure Stack HCI, elija la configuración y los asociados de hardware que mejor se ajusten a sus necesidades.](media/howbuy_ashci.png)

## <a name="compare-azure-stack-and-azure-stack-hci"></a>Comparación de Azure Stack y Azure Stack HCI

A medida que su organización se transforme digitalmente, es posible que avance más rápido usando los servicios en la nube pública para crear arquitecturas modernas y actualizar las aplicaciones heredadas. Sin embargo, por diversos motivos, entre los que se incluyen obstáculos tecnológicos y normativos, muchas cargas de trabajo deben permanecer en el entorno local. La tabla siguiente le ayuda a determinar qué estrategia de nube híbrida de Microsoft le proporciona lo que necesita cuando lo necesita, y le ofrece la innovación de la nube para las cargas de trabajo estén donde estén.

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Nuevas aptitudes, procesos innovadores|Las mismas aptitudes, procesos habituales|
|Servicios de Azure en su centro de datos|Conexión del centro de datos a los servicios de Azure|

### <a name="when-to-use-azure-stack"></a>Cuándo usar Azure Stack

|Azure Stack|Azure Stack HCI|
|--------|-------|
|Use Azure Stack para infraestructuras como servicio (IaaS) de autoservicio, con un aislamiento sólido, un seguimiento preciso del uso y la anulación de los costos en el caso de que varios inquilinos ocupen la misma ubicación. Ideal para proveedores de servicios y nubes privadas de empresa. Plantillas de Azure Marketplace.|Azure Stack HCI no proporciona ni aplica multiinquilinato de forma nativa.|
|Use Azure Stack para desarrollar y ejecutar en el entorno local aplicaciones basadas en servicios de plataforma como servicio (PaaS) como Web Apps, Functions o Event Hubs. Estos servicios se ejecutan en Azure Stack exactamente igual que en Azure, lo que proporciona un entorno de desarrollo y tiempo de ejecución híbrido y coherente.|Azure Stack HCI no ejecuta servicios PaaS en el entorno local.
|Use Azure Stack para modernizar la implementación y el uso de aplicaciones con las prácticas de DevOps como infraestructura como código, integración continua e implementación continua (CI/CD) y funciones muy útiles similares a las extensiones de máquina virtual coherentes con Azure. Ideal para equipos de desarrollo y DevOps.|Azure Stack HCI no incluye herramientas de DevOps de forma nativa.

### <a name="when-to-use-azure-stack-hci"></a>Cuándo usar Azure Stack HCI

|Azure Stack|Azure Stack HCI|
|---------------|---------------|
|Azure Stack requiere un mínimo de 4 nodos y sus propios conmutadores de red.|Use Azure Stack HCI para ocupar un espacio mínimo en oficinas remotas y sucursales. Comience con solo 2 nodos de servidor y redes opuestas sin conmutador para maximizar la simplicidad y la rentabilidad. Las ofertas de hardware comienzan con 4 unidades y 64 GB de memoria por mucho menos de 10 000 USD/nodo.
|Azure Stack limita el conjunto de características y la capacidad de configuración de Hyper V para mantener la coherencia con Azure.|Use Azure Stack HCI para la virtualización con Hyper-V de aplicaciones de empresa clásicas como Exchange, SharePoint y SQL Server, y para virtualizar los roles de Windows Server como servidor de archivos, DNS, DHCP, IIS y AD. Acceso sin restricciones a todas las características de Hyper-V, como las máquinas virtuales blindadas.|
|Azure Stack no expone estas tecnologías de infraestructura.|Usar Azure Stack HCI para contar una infraestructura definida por software en lugar de antiguas matrices de almacenamiento o dispositivos de red, sin realizar un rediseño profundo de la arquitectura. Las tecnologías integradas de Espacios de almacenamiento directo y redes definidas por software (SDN) ofrecen una integración perfecta con los entornos de Hyper-V.|