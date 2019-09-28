---
title: 'Recuperación del bosque de AD: preguntas frecuentes'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 49cd12621c6ddf89393f0463e4856555ca241491
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369108"
---
# <a name="ad-forest-recovery---faq"></a>Recuperación del bosque de AD: preguntas frecuentes

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2, Windows Server 2003

Este documento contiene las preguntas más frecuentes (p + f) relacionadas con la recuperación de bosque:  

## <a name="general-recovery"></a>Recuperación general

**RESPUESTAS ¿Qué puedo hacer para acelerar la recuperación?**

Aunque la velocidad de recuperación no es el objetivo principal de esta guía, puede lograr tiempos de recuperación más cortos:  
  
- Crear un plan detallado de recuperación de bosques, actualizarlo con regularidad y practicarlo en un entorno de prueba simulado de tamaño razonable al menos una vez al año  
- Uso de la clonación de controladores de dominio virtualizados (DC)  
   - La clonación de controladores de dominio virtualizados acelera el proceso de obtención de controladores de dominio adicionales que se ejecutan después de que se restaure un DC desde la copia de seguridad en cada dominio. Los controladores de DC virtualizados adicionales se pueden clonar en lugar de esperar a que se completen las instalaciones de AD DS largas y a la finalización de la replicación no crítica después de la instalación.  
   - Los bosques en los que los controladores de DC virtuales se hospedan en un número relativamente pequeño de centros de datos bien conectados pueden beneficiarse de la clonación durante la recuperación. Sin embargo, todos los entornos en los que haya varios controladores de dominio virtualizados para el mismo dominio colocados en el mismo host de hipervisor deberían beneficiarse.  
- Implementar controladores de dominio de solo lectura (RODC)  
   - Los RODC pueden proporcionar continuidad empresarial durante el proceso de recuperación porque no es necesario desconectarse de la red como los controladores de red de escritura. Los RODC no realizan la replicación de salida. Por lo tanto, no presentan el mismo riesgo que los controladores de seguridad de escritura suponen la replicación de datos dañinos en el entorno recuperado.  
  
Entre otros factores que afectan a la duración del proceso de recuperación del bosque se incluyen los siguientes:  
  
- Al restaurar los controladores de seguridad a partir de las copias de seguridad, se tarda en realizar lo siguiente:  
   - Busque el medio físico de copia de seguridad, como las cintas.  
   - Vuelva a instalar el sistema operativo.  
   - Restaure los datos desde un medio de copia de seguridad.  
      - Puede reducir el tiempo necesario para volver a instalar el sistema operativo y restaurar los datos de la copia de seguridad realizando una recuperación completa del servidor en lugar de restaurar el estado del sistema. Dado que la recuperación completa del servidor se basa en binario, se completa mucho más rápido que la restauración del estado del sistema.  
      - Sin embargo, si el servidor contiene datos que se excluyen de los datos de estado del sistema que no desea restaurar, es posible que la recuperación completa del servidor no sea una alternativa viable a la restauración del estado del sistema. Tenga en cuenta las ventajas de realizar una recuperación completa del servidor en lugar de una restauración del estado del sistema para los servidores de forma específica y prepárese en consecuencia realizando el tipo de copia de seguridad adecuado que planea restaurar más adelante.  
- Al volver a generar los controladores de red, se tarda tiempo en replicar los datos para las promociones basadas en red.  
   - Puede reducir el tiempo necesario para restaurar los controladores de DC realizando los pasos siguientes:  
- Reduzca el tiempo de recuperación de medios de copia de seguridad:  
   - Usar la herramienta de montaje de bases de datos de Active Directory (DSAMain. exe) para identificar la mejor copia de seguridad que se va a usar para las operaciones de restauración. Para obtener más información acerca del uso de la herramienta de montaje de bases de datos de Active Directory, consulte la [Guía paso a paso de la herramienta de montaje de Active Directory Database](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - Etiquete claramente el medio de copia de seguridad y almacene los medios de forma organizada en una ubicación cómoda y segura que permita una recuperación rápida.  
   - El uso del Servicio de instantáneas de volumen con una red de área de almacenamiento (SAN) para mantener copias de seguridad desde diferentes puntos en el tiempo. Para obtener más información, vea [Windows Server 2003 Active Directory recuperación rápida con servicio de instantáneas de volumen y el servicio de disco virtual](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Fuerce la eliminación de AD DS del DC en lugar de volver a instalar el sistema operativo. Si se ha identificado que la causa del error en todo el bosque se encuentra únicamente dentro del ámbito de AD DS, no es necesario volver a instalar el sistema operativo en los controladores de sesión.  
   - Para obtener más información sobre cómo forzar la eliminación de AD DS desde un controlador de dominio que ejecute Windows Server 2008 o posterior, consulte [forzar la eliminación de un controlador de dominio de Windows server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Para obtener más información sobre cómo forzar la eliminación de AD DS de un controlador de dominio que ejecute Windows Server 2003, vea el [artículo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) de Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Use dispositivos de cinta o copias de seguridad de disco más rápidos para reducir el tiempo necesario para las operaciones de restauración.  
  
También puede ayudar a acelerar las instalaciones de AD DS mediante la característica de instalación desde medios (IFM) para volver a generar los controladores de dominio de cada dominio. IFM reduce la latencia de replicación que se produce cuando se vuelven a generar los controladores de dominio en cada dominio.  
  
Las empresas que tienen un contrato de nivel de servicio (SLA) más agresivo pueden considerar la modificación de los procedimientos de recuperación de bosques para acelerar la recuperación.  
  
**RESPUESTAS ¿Puedo automatizar el proceso de recuperación del bosque?**

Debido a la naturaleza compleja y crítica del proceso de recuperación del bosque, actualmente no hay ninguna automatización completa del mismo. El proceso de recuperación del bosque es más un desafío logístico y organizacional de restaurar la continuidad empresarial que un problema técnico de la automatización de procesos. Por lo tanto, la persona que administra el entorno debe crear un plan de recuperación de bosque específico para ese entorno y, a continuación, automatizar las secciones que se pueden automatizar correctamente.  
  
Puede realizar la mayoría de los pasos de recuperación de bosque mediante herramientas de línea de comandos. Por lo tanto, la mayoría de los pasos se pueden incluir en scripts. Por ejemplo, Ntdsutil. exe es una de las herramientas que se usan con más frecuencia en el proceso de recuperación del bosque.  
  
Aunque los scripts pueden acelerar la recuperación, debe probarlos exhaustivamente antes de aplicarlos en un entorno real. Además, debe actualizarlos según los cambios en el entorno de Active Directory, como la adición de un nuevo dominio o DC, o una nueva versión de Active Directory.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación del bosque de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosque de AD: diseño de un plan de recuperación de bosque personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación del bosque de AD: identificación del problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación del bosque de AD: determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosque de AD: realizar la recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosque de AD: preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosque de AD: recuperación de un solo dominio en un bosque de varios dominios](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosque de AD: recuperación de bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
