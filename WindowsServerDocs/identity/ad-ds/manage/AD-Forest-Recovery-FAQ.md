---
title: "Recuperación de bosque de AD - preguntas más frecuentes"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adfs
ms.openlocfilehash: 839e01c88b7ced22501154d8752c6c71cc52b696
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---faq"></a>Recuperación de bosque de AD - preguntas más frecuentes

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2, Windows Server 2003

Este documento contiene las preguntas más frecuentes (P+f) sobre la recuperación de bosque:  
 
## <a name="general-recovery"></a>Recuperación de general
  
 
**P: ¿Qué puedo hacer para acelerar la recuperación?** 
 
Aunque la velocidad de recuperación no es el principal objetivo de esta guía, puedes lograr tiempos de recuperación por:  
  
-   Crear un plan de recuperación de bosque detallada, actualizarla de forma regular y practicando en un entorno de prueba simulado de un tamaño razonable al menos una vez al año  
  
-   Uso de clonación de controlador de dominio virtualizada  
  
     Clonación virtualizada de DC acelera el proceso para obtener controladores adicionales que ejecute después de un que controlador de dominio se restaura desde la copia de seguridad en cada dominio. Los controladores de dominio virtualizadas adicionales se pueden clonar en lugar de esperar prolongada AD DS instalaciones que se complete y para la finalización de replicación no crítica tras la instalación.  
  
     Bosques donde virtuales controladores de dominio se hospedan en un número relativamente pequeño de centros de datos bien conectados potencialmente beneficiarán más de clonación durante la recuperación. Sin embargo, cualquier entorno donde varios controladores de dominio virtualizadas para el mismo dominio se encuentren en el mismo host hipervisor disfruten.  
  
-   Implementación de controladores de dominio de solo lectura (RODC)  
  
     RODC proporcionar continuidad del negocio durante el proceso de recuperación como no tienen que estar desconectado de la red, como controladores de dominio grabables. RODC no realizan replicación saliente. Por lo tanto, no presentan el mismo riesgo que suponen controladores de dominio grabables para replicar datos daños en el entorno recuperado.  
  
 Otros factores que afectan a la duración del proceso de recuperación bosque incluyen lo siguiente:  
  
-   Cuando se restauran controladores de dominio de las copias de seguridad, se necesita tiempo para:  
  
    -   Localice el medio de copia de seguridad físico, como las cintas.  
  
    -   Reinstalar el sistema operativo.  
  
    -   Restaurar los datos desde un medio de copia de seguridad.  
  
     Puedes reducir el tiempo necesario para instalar el sistema operativo y restaurar los datos de copia de seguridad mediante la realización de recuperación completa del servidor en lugar de restauración del estado del sistema. Dado recuperación completa del servidor se basa en el archivo binario, se completa con mayor rapidez que la restauración del estado del sistema.  
  
     Sin embargo, si el servidor contiene datos que se excluyen de datos de estado del sistema que no desea restaurar, recuperación completa del servidor no sea una alternativa viable a la restauración del estado del sistema. Ten en cuenta las ventajas de realizar una recuperación completa del servidor en lugar de una restauración del estado del sistema para los servidores específicamente y preparar según corresponda al realizar el tipo apropiado de copia de seguridad que deseas restaurar más adelante.  
  
-   Volver a crear controladores de dominio, tarda tiempo para replicar datos para promociones basado en red.  
  
 Puede reducir el tiempo necesario para restaurar los controladores de dominio, efectuando los pasos siguientes:  
  
-   Reducir el tiempo para recuperar el medio de copia de seguridad por:  
  
    -   Usando la herramienta de montaje de base de datos de Active Directory de (Dsamain.exe) para identificar la copia de seguridad recomendado para usar para operaciones de restauración. Para obtener más información sobre cómo usar la herramienta de montaje de la base de datos de Active Directory, consulte la [herramienta de montaje de base de datos de Active Directory Step-by-Step guía](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
  
    -   Etiquetado de claramente el medio de copia de seguridad y almacenar el medio de forma organizada en una práctica aún seguro, ubicación que permite su rápida recuperación.  
  
    -   Usar el servicio de instantáneas de volumen con una red de área de almacenamiento (SAN) para mantener las copias de seguridad de diferentes puntos en el tiempo. Para obtener más información, consulta [Windows Server 2003 rápida recuperación de Active Directory con el servicio de instantáneas de volumen y el servicio de disco Virtual](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
  
-   Forzar la eliminación de AD DS desde los controladores de dominio en lugar de volver a instalar el sistema operativo. Si se ha identificado la causa del error de todo el bosque para que sea exclusivamente dentro del ámbito de AD DS, no tienes que volver a instalar el sistema operativo en los controladores de dominio.  
  
     Para obtener más información acerca de cómo forzar la eliminación de AD DS de un controlador de dominio que ejecute Windows Server 2008 o posterior, consulta [forzar la eliminación de un controlador de dominio de Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Para obtener más información acerca de cómo forzar la eliminación de AD DS de un controlador de dominio que ejecuta Windows Server 2003, vea [artículo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) en Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780).  
  
-   Usar dispositivos más rápidos de cinta o las copias de seguridad de disco para reducir el tiempo que se necesita para restauración las operaciones.  
  
 También puede ayudar a acelerar las instalaciones de AD DS con la instalación de la característica de medios (IFM) para volver a compilar controladores de dominio en cada dominio. IFM reduce la latencia de replicación que se produce cuando vuelva a generar controladores de dominio en cada dominio.  
  
 Las empresas que tienen un contrato de nivel de servicio (SLA) más intenso considerar si se modifican los procedimientos de recuperación de bosque acelerar la recuperación.  
  

**P: ¿automatizar el proceso de recuperación de bosque?**  
 Dada la naturaleza compleja y crítica del proceso de recuperación de bosque, actualmente no hay ninguna automatización de principio a fin de ella. El proceso de recuperación de bosque es más un desafío logístico y organización de continuidad del negocio que un problema técnico de automatización del proceso de restauración. Por lo tanto, la persona que administra el entorno debe crear un plan de recuperación de bosque que es específico para ese entorno y, a continuación, automatizar las secciones que se pueden automatizar correctamente.  
  
 Puede realizar la mayoría de los pasos de recuperación de bosque mediante herramientas de línea de comandos. Por lo tanto, la mayoría de los pasos son que permite ejecutar script. Por ejemplo, Ntdsutil.exe es una de las herramientas usadas con más frecuencia en el proceso de recuperación del bosque.  
  
 Aunque scripts pueden aumentar la velocidad de recuperación, debes probar minuciosamente estos scripts antes de aplicar en un entorno real. Además, debes actualizar según los cambios en el entorno de Active Directory, por ejemplo, la adición de un dominio o controlador de dominio y una nueva versión de Active Directory.

## <a name="next-steps"></a>Pasos siguientes
-   [Recuperación de bosque de AD - requisitos previos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperación de bosque de AD - diseñar un plan de recuperación personalizada del bosque](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperación de bosque de AD - identificar el problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperación de bosque de AD - determinar cómo recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperación de bosque de AD - realizar una recuperación inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperación de bosque de AD - preguntas más frecuentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperación de bosque de AD - recuperar un solo dominio dentro de un bosque Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Recuperación de bosque de AD - recuperación del bosque con controladores de dominio de Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
