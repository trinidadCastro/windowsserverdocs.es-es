---
title: Introducción a Azure HCI de pila
description: 'HCI de pila de Azure es un clúster de Windows Server 2019 hiperconvergido que usa valida hardware para ejecutar cargas de trabajo virtualizadas locales, opcionalmente, conectar a los servicios de Azure para la copia de seguridad basada en la nube, la recuperación del sitio y mucho más. Soluciones de pila HCI Azure usan hardware validado por Microsoft para garantizar la confiabilidad y un rendimiento óptimo e incluyen compatibilidad con tecnologías, como las unidades NVMe, memoria persistente y redes de memoria de remote direct access (RDMA).'
ms.technology: storage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/26/2019
---

# Introducción a Azure HCI de pila

>Se aplica a: Windows Server 2019

HCI de pila de Azure es un clúster de Windows Server 2019 hiperconvergido que usa valida hardware para ejecutar cargas de trabajo virtualizadas locales, opcionalmente, conectar a los servicios de Azure para la copia de seguridad basada en la nube, la recuperación del sitio y mucho más. Soluciones de pila HCI Azure usan hardware validado por Microsoft para garantizar la confiabilidad y un rendimiento óptimo e incluyen compatibilidad con tecnologías, como las unidades NVMe, memoria persistente y redes de memoria de remote direct access (RDMA).

## La familia de Azure Stack

Azure HCI de pila es parte de la Azure y familia de pila de Azure, con el mismo definido por software de cálculo, almacenamiento y redes software como Azure Stack. Este es un resumen rápido de las soluciones diferentes:

- [Azure](https://azure.microsoft.com) : servicios de nube pública de uso
- [Azure Stack](https://azure.microsoft.com/overview/azure-stack) - operar local de servicios en la nube
- [Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) - Run virtualizadas aplicaciones locales, con conexiones opcionales a Azure

![Azure y Azure Stack ejecutar servicios en la nube, mientras se ejecuta Azure Stack HCI virtualizadas de aplicaciones locales](media/azure-and-azure-stack-family.png)

Para obtener más información:

- Registrar nuestro [evento Virtual de nube híbrida](https://info.microsoft.com/ww-landing-building-a-successful-hybrid-cloud-strategy.html) de 28 de marzo de 2019.
- Más información en nuestro sitio Web de soluciones de [HCI de pila de Azure](https://azure.microsoft.com/overview/azure-stack/hci) .
- Mira expertos de Microsoft Jeff Woolsey y Vijay Tewari [Explicar las nuevas soluciones HCI de pila de Azure](https://aka.ms/AzureStackOverviewVideo).

## Partners de hardware

Puedes comprar soluciones de Azure Stack HCI validadas que ejecutan Windows Server 2019 de 15 asociados. El partner de Microsoft preferido obtiene de seguridad y ejecución sin prolongada diseño y tiempo de compilación y ofrece un único punto de contacto para servicios de implementación y soporte técnico.

Visita el [sitio Web de Azure Stack HCI](https://azure.microsoft.com/overview/azure-stack/hci) para ver nuestras 70 + Azure Stack HCI soluciones disponibles actualmente en estos socios de Microsoft: ASUS, Axellio, bluechip, DataON, Dell EMC, Fujitsu, HPE, Hitachi, Huawei, Lenovo, NEC, soluciones primeLine, QCT, SecureGUARD y Supermicro.

## Preguntas frecuentes

### ¿Qué soluciones de pila de Azure y Azure Stack HCI tienen en común? 
Las soluciones de pila HCI Azure cuentan con el mismo proceso de definido por software en función de Hyper-V, almacenamiento y tecnologías de redes como Azure Stack. Ambas ofertas cumplan los criterios rigurosos de pruebas y validación para garantizar la confiabilidad y compatibilidad con la plataforma de hardware subyacente.

### ¿Cómo son diferentes?
Con Azure Stack, ejecutan en la nube servicios locales. Puedes ejecutar IaaS de Azure y PaaS servicios locales para compilar y ejecutar aplicaciones de la nube en cualquier lugar, administradas con el Portal de Azure local de forma coherente.

Con HCI de pila de Azure, puedes ejecutar cargas de trabajo virtualizadas locales, se administran con Windows Admin Center y familiar de Windows Server herramientas. Opcionalmente, puedes conectarte a Azure para escenarios como la recuperación del sitio en la nube, supervisión y otros.

### ¿Por qué es Microsoft llevar su HCI ofrecer a la familia de Azure Stack? 
La tecnología de Microsoft hiperconvergido ya es la base de Azure Stack. 

Muchos clientes de Microsoft tienen entornos de TI complejos y nuestro objetivo es proporcionar soluciones que cumplan con ellos dónde se encuentran con la tecnología adecuada para las empresas derecho necesario. HCI de pila de Azure es una evolución de las soluciones de centro basados en Windows Server 2016 (WSSD) que estaba disponibles de nuestros socios de hardware. Hemos introducidas en la familia de Azure Stack porque hemos comenzado a ofrecer opciones nuevas que conectan a la perfección con Azure para servicios de infraestructura de administración. 

### ¿Podré actualizar de HCI de pila de Azure a Azure Stack? 
No, pero los clientes pueden migrar sus cargas de trabajo desde HCI de pila de Azure a Azure Stack o Azure.

### ¿Qué servicios de Azure puedo conectar para HCI de pila de Azure?

Para obtener una lista actualizada de que se puede conectar HCI de pila de Azure a los servicios de Azure, consulte [Conexión Windows Server a los servicios de implementación híbrida de Azure](../azure-hybrid-services/index.md).

### ¿Cómo se pueden comprar soluciones HCI de pila de Azure?
Sigue estos pasos:

1. Comprar un sistema de hardware validado por Microsoft desde tu partner de hardware preferido.
1. Instalar Windows Server 2019 Datacenter edition y Windows Admin Center para la administración y la capacidad para conectar a Azure para servicios en la nube
1. Opcionalmente, usa tu cuenta de Azure para asociar los servicios de administración y la seguridad basada en la nube a las cargas de trabajo.