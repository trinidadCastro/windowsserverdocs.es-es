---
title: 'Recuperación del bosque de AD: preguntas frecuentes'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 36c9560b490cc28f006770c869bdcdf2b152dd74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878456"
---
# <a name="ad-forest-recovery---faq"></a>Recuperación del bosque de AD: preguntas frecuentes

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2, Windows Server 2003

Este documento contiene las preguntas más frecuentes (P+f) sobre la recuperación de bosques:  

## <a name="general-recovery"></a>Recuperación generales

**P: ¿ ¿Qué puedo hacer para aumentar la velocidad de recuperación?**

Aunque la velocidad de recuperación no es el principal objetivo de esta guía, puede lograr tiempos de recuperación más cortos por:  
  
- Crear un plan de recuperación de bosque detallada, actualizarlo de forma periódica y practicar en un entorno de pruebas simuladas de tamaño razonable al menos una vez al año  
- Uso de la clonación del controlador (DC) de dominio virtualizados  
   - Clonación de controlador de dominio virtualizado acelera el proceso para obtener otros controladores de dominio ejecutan después de un que controlador de dominio se restaura desde la copia de seguridad en cada dominio. Los otros controladores de dominio virtualizados se pueden clonar en lugar de esperar potencialmente larga AD DS en las instalaciones se complete y la finalización de la replicación no crítica después de la instalación.  
   - Los bosques donde se hospedan controladores de dominio virtuales en un número relativamente pequeño de centros de datos bien conectados potencialmente beneficiarían de la clonación durante la recuperación. Sin embargo, debe aprovechar cualquier entorno donde varios controladores de dominio virtualizados para el mismo dominio se encuentren en el mismo host del hipervisor.  
- Implementación de controladores de dominio de solo lectura (RODC)  
   - Los RODC pueden proporcionar continuidad empresarial durante el proceso de recuperación porque no tienen que estar desconectado de la red, igual que los controladores de dominio grabables. Los RODC no lleve a cabo la replicación saliente. Por lo tanto, no presentan el mismo riesgo que suponen los DC de escritura para replicar los datos de daños en el entorno recuperado.  
  
Otros factores que afectan a la duración del proceso de recuperación de bosque incluyen lo siguiente:  
  
- Cuando los controladores de dominio se restauración desde copias de seguridad, se necesita tiempo para:  
   - Busque los medios de copia de seguridad físicos, como las cintas.  
   - Vuelva a instalar el sistema operativo.  
   - Restaurar datos desde medios de copia de seguridad.  
      - Puede reducir el tiempo necesario para volver a instalar el sistema operativo y restaurar los datos de copia de seguridad mediante la realización de recuperación completa del servidor en lugar de restauración del estado del sistema. Como recuperación completa del servidor está basado en el archivo binario, completa mucho más rápido que la restauración del estado del sistema.  
      - Sin embargo, si el servidor contiene datos que se excluirán de los datos de estado del sistema que no desea restaurar, recuperación de servidor completo no puede ser una alternativa viable a la restauración del estado del sistema. Tenga en cuenta las ventajas de realizar una recuperación completa del servidor en lugar de una restauración del estado del sistema para los servidores en concreto y preparar según corresponda mediante la realización de un tipo adecuado de copia de seguridad que va a restaurar más adelante.  
- Cuando se regenera los controladores de dominio, toma tiempo para replicar datos para las promociones basadas en red.  
   - Puede reducir el tiempo necesario para restaurar los controladores de dominio mediante la realización de los pasos siguientes:  
- Reducir el tiempo de recuperación de medios de copia de seguridad por:  
   - Uso de la herramienta de montaje del base de datos de Active Directory (Dsamain.exe) para identificar la mejor copia de seguridad que se utilizará para las operaciones de restauración. Para obtener más información sobre cómo usar la herramienta de montaje de base de datos de Active Directory, consulte el [Active directorio base de datos de montaje herramienta guía paso a paso](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - Etiquetado con claridad los medios de copia de seguridad y almacenar el medio de forma organizada en una cómoda aún, ubicación protegida que permite una recuperación rápida.  
   - Usar el servicio de instantáneas de volumen con una red de área de almacenamiento (SAN) para mantener copias de seguridad desde diferentes puntos de tiempo. Para obtener más información, consulte [Windows Server 2003 rápida recuperación de Active Directory con el servicio de instantáneas de volumen y el servicio de disco Virtual](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Forzar la desinstalación de AD DS desde los controladores de dominio en lugar de volver a instalar el sistema operativo. Si se ha identificado la causa del error de todo el bosque para que sea estrictamente dentro del ámbito de AD DS, no es necesario volver a instalar el sistema operativo en los controladores de dominio.  
   - Para obtener más información acerca de cómo forzar la desinstalación de AD DS de un controlador de dominio que ejecuta Windows Server 2008 o posterior, consulte [forzar la eliminación de un controlador de dominio de Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Para obtener más información acerca de cómo forzar la desinstalación de AD DS de un controlador de dominio que ejecuta Windows Server 2003, consulte [artículo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Usar dispositivos de cinta más rápidos o las copias de seguridad de disco para reducir el tiempo necesario para las operaciones de restauración.  
  
También puede ayudar a acelerar las instalaciones de AD DS mediante el uso de la instalación de la característica de medios (IFM) para volver a generar los controladores de dominio en cada dominio. IFM reduce la latencia de replicación que se incurre al volver a generar los controladores de dominio en cada dominio.  
  
Las empresas que tienen un acuerdo de nivel de servicio (SLA) más agresivo es posible que considere la posibilidad de modificar los procedimientos de recuperación de bosque para acelerar la recuperación.  
  
**P: ¿ ¿Automatizar el proceso de recuperación de bosque?**

Dada la naturaleza compleja y crítica del proceso de recuperación de bosque, no es actualmente automatización to-end de la misma. El proceso de recuperación del bosque es más un desafío logístico y organización de la continuidad del negocio que un problema técnico de automatización de procesos de restauración. Por lo tanto, la persona que administra el entorno debe crear un plan de recuperación de bosque es específico de ese entorno y, a continuación, automatizar secciones de la misma que se pueden automatizar correctamente.  
  
Puede realizar la mayoría de los pasos de recuperación de bosque mediante las herramientas de línea de comandos. Por lo tanto, la mayoría de los pasos son secuencias de comandos. Por ejemplo, Ntdsutil.exe es una de las herramientas utilizadas con frecuencia en el proceso de recuperación del bosque.  
  
Aunque las secuencias de comandos pueden acelerar la recuperación, debe probar exhaustivamente estas secuencias de comandos antes de aplicar en un entorno real. Además, debe actualizar según los cambios en el entorno de Active Directory, como la adición de un nuevo dominio o controlador de dominio o una nueva versión de Active Directory.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de bosques de AD: requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperación de bosques de AD - idear un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosques de AD: identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperación de bosques de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperación de bosques de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
- [Recuperación de bosques de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperación de bosques de AD - recuperar un único dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperación de bosques de AD: recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
