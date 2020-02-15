---
ms.assetid: ''
title: Configuración de sistemas para alta precisión
description: La sincronización de la hora en Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, los sistemas se pueden configurar para mantener una precisión de 1 ms (milisegundos) o mejor (con respecto a la hora UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405196"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configuración de sistemas para alta precisión
>Se aplica a: Windows Server 2016 y Windows 10, versión 1607 o superior

La sincronización de la hora en Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, los sistemas se pueden configurar para mantener una precisión de 1 ms (milisegundos) o mejor (con respecto a la hora UTC).

La siguiente guía te ayudará a configurar los sistemas para lograr una alta precisión.  En este artículo se tratan los siguientes requisitos:

- Sistemas operativos compatibles
- Configuración del sistema 

> [!WARNING]
> **Objetivos de precisión de los sistemas operativos anteriores**<br>
>Windows Server 2012 R2 y versiones anteriores no pueden cumplir los mismos objetivos de alta precisión. Estos sistemas operativos no son compatibles con una alta precisión.
>
>En estas versiones, el servicio de hora de Windows cumplía los siguientes requisitos:
>
> - Proporcionaba la precisión de hora necesaria para satisfacer los requisitos de autenticación de Kerberos, versión 5.
> - Proporcionaba una hora algo preciso para los clientes y servidores de Windows unidos a un bosque común de Active Directory.
>
>Las mayores tolerancias en 2012 R2 y versiones anteriores están fuera de la especificación de diseño del servicio de hora de Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configuración predeterminada de Windows 10 y Windows Server 2016

Si bien se admite la precisión de hasta 1 ms en Windows 10 o Windows Server 2016, la mayoría de los clientes no requiere una hora sumamente precisa.

Por tanto, la **configuración predeterminada** está diseñada para cumplir con los mismos requisitos que los sistemas operativos anteriores, es decir:

- Proporcionar la precisión de hora necesaria para satisfacer los requisitos de autenticación de Kerberos, versión 5.
- Proporcionar una hora algo preciso para los clientes y servidores de Windows unidos a un bosque común de Active Directory.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Procedimiento: Configuración de sistemas para alta precisión

>[!IMPORTANT]
>**Nota sobre la compatibilidad de los sistemas altamente precisos**<br>
> La precisión de la hora conlleva la distribución de un extremo a otro de la hora precisa desde el origen de la hora de autoridad hasta el dispositivo final.  Todo lo que agregue asimetría en las medidas a lo largo de este camino influirá negativamente en la precisión y afectará a la precisión que puedan alcanzar los dispositivos.
>
>Por esta razón, hemos documentado el [Límite de compatibilidad para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md), que describe los requisitos del entorno que también deben cumplirse para alcanzar objetivos de alta precisión.

### <a name="operating-system-requirements"></a>Requisitos del sistema operativo

Las configuraciones de alta precisión requieren Windows 10 o Windows Server 2016.  Todos los dispositivos Windows en la topología de hora deben cumplir este requisito, incluidos los servidores de hora de Windows de estrato superior y, en escenarios virtualizados, los hosts de Hyper-V que ejecutan las máquinas virtuales sujetas a limitaciones temporales. Todos estos dispositivos deben ejecutar al menos Windows 10 o Windows Server 2016.

En la ilustración que se muestra a continuación, las máquinas virtuales que requieren alta precisión ejecutan Windows 10 o Windows Server 2016.  Del mismo modo, el host de Hyper-V en el que residen las máquinas virtuales y el servidor de hora de Windows de nivel superior también deben ejecutar Windows Server 2016.

![Topología de hora - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Comprobación de la versión de Windows**<br>
> Puede ejecutar el comando `winver` en un símbolo del sistema para verificar si la versión del sistema operativo es 1607 (o posterior) y que la compilación del sistema operativo es 14393 (o superior), como se muestra a continuación:
>
> ![Winver - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuración del sistema

Alcanzar destinos de alta precisión requiere la configuración del sistema.  Hay varias maneras de realizar esta configuración, entre otras, directamente en el registro o a través de la directiva de grupo.  Puedes encontrar más información para cada una de estas opciones en la Referencia técnica del servicio de hora de Windows: [Herramientas de servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo de inicio del servicio de hora de Windows

El servicio de hora de Windows (W32Time) debe ejecutarse de manera continua.  Para ello, configure el tipo de inicio del servicio de hora de Windows en "Automático".

![Configuración automática](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latencia de red unidireccional acumulativa

La incertidumbre de medición y el "ruido" se incrementan a medida que aumenta la latencia de red.  Por tanto, es de vital importancia que la latencia de red esté dentro de un límite razonable.  Los requisitos específicos dependen de la precisión de destino y se describen en el artículo [Límite de compatibilidad para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md).

Para calcular la latencia de red unidireccional acumulativa, agrega los retrasos unidireccionales individuales entre pares de nodos cliente-servidor NTP en la topología de hora, empezando por el destino y finalizando en el origen de la hora de estrato 1 de alta precisión.

Por ejemplo: Considera la posibilidad de una jerarquía de sincronización de hora con un origen muy preciso, dos servidores NTP intermedios (A y B), y la máquina de destino, en ese orden. Para obtener la latencia de red acumulativa entre el destino y el origen, mide el promedio de tiempo de ida y vuelta (RTT) de NTP individual entre:

- El destino y el servidor de hora B
- El servidor de hora B y el servidor de hora A
- El servidor de hora A y el origen

Esta medición puede obtenerse mediante la herramienta de bandeja de entrada w32tm.exe.  Para ello:

1. Haz el cálculo desde el destino y servidor de hora B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Haz el cálculo desde el servidor de hora B frente a (apuntando a) el servidor de hora A.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Haz el cálculo desde el servidor de hora A frente al origen.
 
4. Luego, agrega el promedio de RoundTripDelay medido en el paso anterior y divide entre 2 para obtener el retraso de red acumulativo entre el destino y el origen.

#### <a name="registry-settings"></a>Configuración del Registro

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura el intervalo más pequeño en log2 segundos permitidos para el sondeo del sistema.

|  |  | 
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Valor    | 6        |
|Resultado | El intervalo de sondeo mínimo es ahora de 64 segundos. |

El comando siguiente indica a la hora de Windows que recoja la configuración actualizada:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura el intervalo más grande en log2 segundos permitidos para el sondeo del sistema.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Valor    | 6        |
|Resultado | El intervalo de sondeo máximo es ahora de 64 segundos.  |

El comando siguiente indica a la hora de Windows que recoja la configuración actualizada:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
El número de tics del reloj entre los ajustes de corrección de fase.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Valor    | 100        |
|Resultado | El número de tics del reloj entre los ajustes de corrección de fase es ahora de 100 tics. |

El comando siguiente indica a la hora de Windows que recoja la configuración actualizada:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura el intervalo de sondeo en segundos cuando la marca SpecialInterval 0x1 está habilitada.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Valor    | 64        |
|Resultado | El intervalo de sondeo es ahora de 64 segundos. |

El comando siguiente reinicia la hora de Windows para recoger la configuración actualizada:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Valor    | 2        |


---
