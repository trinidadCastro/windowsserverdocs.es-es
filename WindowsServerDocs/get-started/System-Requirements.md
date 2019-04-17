---
title: Requisitos del sistema
description: ¿Cuáles son los requisitos mínimos para almacenamiento, CPU, red, memoria y RAM en una instalación limpia de cada opción de instalación?
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131fe068
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 29183c62830cbe9e26cce4e0ce4543b554f0ed65
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783747"
---
# Requisitos del sistema

>Se aplica a: Windows Server (canal semianual), Windows Server 2016 

En este tema se tratan los requisitos mínimos del sistema para ejecutar Windows Server&reg; 2016 o Windows Server, versión 1709.


> [!Note]  
> En esta versión, se recomiendan las instalaciones limpias.  
>   

> [!NOTE]  
> Si en el momento de la instalación opta por la opción Server Core, debe tener en cuenta que no se instala ningún componente de la interfaz gráfica de usuario y que no podrá instalarlos o desinstalarlos con el Administrador del servidor. Si necesita características de la interfaz gráfica de usuario, asegúrese de elegir la opción "Servidor con Experiencia de escritorio" al instalar Windows Server 2016. Para obtener más información, consulte [Instalación de Nano Server](Getting-Started-with-Nano-Server.md).  


## Repasar los requisitos del sistema  
A continuación se incluyen los requisitos del sistema aproximados para Windows Server 2016. Si su equipo no cumple con los requisitos mínimos, no podrá instalar el producto correctamente. Los requisitos reales variarán según la configuración del sistema y las aplicaciones y características que instale.

A menos que se especifique lo contrario, estos requisitos mínimos del sistema se aplican a todas las opciones de instalación (Server Core, Server con Experiencia de escritorio y Nano Server) y a las ediciones Standard y Datacenter.  

> [!IMPORTANT]  
> Dado a la gran diversidad de implementaciones posibles, sería irreal que se declararan requisitos del sistema "recomendados" de aplicación general. Consulte la documentación específica de los roles de servidor que intenta implementar para obtener más detalles sobre los recursos que se necesitan para cada uno de ellos. Podrá obtener mejores resultados con implementaciones de prueba que le ayuden a determinar los requisitos del sistema apropiados para sus propios escenarios.  


## Procesador  
El rendimiento del procesador depende no solo de su frecuencia de reloj, sino también de su número de núcleos y tamaño de la caché. A continuación, se detallan los requisitos relativos al procesador para este producto:  

**Mínimo**:  
- Procesador de 64 bits a 1,4 GHz  
- Compatible con el conjunto de instrucciones x64  
- Admite DEP y NX  
- Admite CMPXCHG16b, LAHF/SAHF y PrefetchW  
- Admite la traducción de direcciones de segundo nivel (EPT o NPT)  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx) es una herramienta que puedes usar para confirmar cuáles de estas funciones tiene tu CPU.

## RAM  
A continuación, se detallan los requisitos estimados relativos a la memoria RAM para este producto:  

**Mínimo**:  
- 512 MB (2 GB para la opción de instalación Servidor con Experiencia de escritorio)
- Tipo ECC (código de corrección de errores) o tecnología similar  

> [!IMPORTANT]  
> El programa de instalación dará error si ha creado una máquina virtual con el mínimo de parámetros de hardware admitidos (procesador de 1núcleo y 512MB de memoria RAM) y, luego, trata de instalar esta versión en dicha máquina virtual.  
>   
> Para evitarlo, realice una de las acciones siguientes:  
>   
> -   Asigne más de 800 MB de memoria RAM a la máquina virtual en la que quiera instalar esta versión. Cuando el programa de instalación se haya completado, podrá cambiar la asignación de nuevo a 512 MB de RAM, según cuál sea la configuración de servidor real.  
> -   Interrumpa el proceso de arranque de esta versión en la máquina virtual usando MAYÚS+F10. En el símbolo del sistema que se abre, use Diskpart.exe para crear una partición de instalación y darle formato. Ejecute **Wpeutil createpagefile /path=C:\pf.sys** (si es que la partición de instalación se ha creado en C:). Cierre el símbolo del sistema y continúe con el programa de instalación.  

## Requisitos de espacio en disco y del controlador de almacenamiento  
Los equipos que ejecutan Windows Server 2016 deben incluir un adaptador de almacenamiento que sea compatible con la especificación de arquitectura PCI Express. Los dispositivos de almacenamiento persistente en servidores clasificados como unidades de disco duro no deben ser PATA. Windows Server 2016 no admite ATA, PATA, IDE y EIDE para unidades de arranque, página o datos.  

A continuación se detallan los requisitos **mínimos** de espacio en disco estimados para la partición del sistema.  

**Mínimo**: 32 GB  

   > [!NOTE]  
    > Tenga en cuenta que 32 GB debe considerarse como el valor *mínimo absoluto* para una instalación correcta. Con este valor mínimo debería poder instalar Windows Server 2016 en modo Server Core con el rol del servidor de servicios web (IIS). Un servidor en modo Server Core es unos 4 GB más pequeño que el mismo servidor en modo Servidor con una GUI. 
    >   
    > La partición del sistema requerirá más espacio en cualquiera de las siguientes circunstancias:  
    >   
    > -   Si se instala el sistema en una red.  
    > -   Los equipos con más de 16 GB de RAM necesitarán más espacio en disco para los archivos de paginación, hibernación y volcado.  

## Requisitos del adaptador de red  

Los adaptadores de red utilizados con esta versión deberían incluir estas características:  

**Mínimo**:  
- Un adaptador Ethernet con capacidad de rendimiento de al menos gigabit.  
- Compatible con la especificación de arquitectura PCI Express.  
- Admite el entorno de ejecución previo al arranque (PXE).  

Un adaptador de red que admite la depuración de red (KDNet) es útil, pero no es un requisito mínimo.   



## Otros requisitos  
Los equipos que ejecutan esta versión también deben tener lo siguiente:  


-   Unidad de DVD (si necesita instalar el sistema operativo por medio de DVD)  

Los siguientes elementos no son estrictamente obligatorios, pero sí necesarios para algunas características:  

- Sistema basado en UEFI 2.3.1c y firmware que admita el arranque seguro  
- Módulo de plataforma segura  

-   Dispositivo de gráficos y monitor que admita Super VGA (1024 x 768) o una mayor resolución  

-   Teclado y mouse de Microsoft&reg; (u otro dispositivo señalador compatible)  

-   Acceso a Internet (pueden aplicarse las tarifas correspondientes)  

>[!NOTE]  
> Un chip de Módulo de plataforma segura (TPM) no es estrictamente necesario para instalar esta versión, aunque es necesario para poder utilizar determinadas características (como el Cifrado de unidad BitLocker). Si el equipo usa TPM, debe cumplir estos requisitos:  
>  
>- Los TPM basados en hardware deben implementar la versión 2.0 de la especificación de TPM.  
>- Los TPM que implementan la versión 2.0 deben tener un certificado EK que sea suministrado previamente al TPM por el proveedor de hardware o que se puedan recuperar del dispositivo durante el primer inicio.  
>- Los TPM que implementan la versión 2.0 deben suministrarse con bancos SHA-256 PCR e implementar PCR de 0 a 23 para SHA-256. Es aceptable suministrar los TPM con un solo banco PCR intercambiable que se puede usar para las mediciones de SHA-1 y SHA-256.  
>- La opción de UEFI para desactivar TPM no es un requisito.  

## Instalación de Nano Server  
Para obtener instrucciones detalladas para instalar Windows Server 2016 como Nano Server, consulta [Instalación de Nano Server](Getting-Started-with-Nano-Server.md).

## Recursos adicionales
- [Requisitos de procesador de Windows](https://docs.microsoft.com/windows-hardware/design/minimum/windows-processor-requirements)
- [Comparación de las ediciones Standard y Datacenter de Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/2016-edition-comparison)
- [Requisitos del sistema de Windows 10 ](https://www.microsoft.com/windows/windows-10-specifications#system-specifications)
- [Descargar la hoja de datos de licencias de Windows Server 2016](http://download.microsoft.com/download/7/2/9/7290EA05-DC56-4BED-9400-138C5701F174/WS2016LicensingDatasheet.pdf)
