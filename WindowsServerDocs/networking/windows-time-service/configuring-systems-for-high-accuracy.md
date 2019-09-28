---
ms.assetid: ''
title: Configuración de sistemas para alta precisión
description: La sincronización de la hora en Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, los sistemas se pueden configurar para mantener una precisión de 1 ms (milisegundos) o superior (con respecto a la hora UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b7cd256fdbbdbe7432e5b5d5b16254314132560f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405196"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configuración de sistemas para alta precisión
>Se aplica a: Windows Server 2016 y Windows 10 versión 1607 o posterior

La sincronización de la hora en Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, los sistemas se pueden configurar para mantener una precisión de 1 ms (milisegundos) o superior (con respecto a la hora UTC).

La siguiente guía le ayudará a configurar los sistemas para lograr una gran precisión.  En este artículo se describen los siguientes requisitos:

- Sistemas operativos admitidos
- Configuración del sistema 

> [!WARNING]
> **Objetivos de precisión de sistemas operativos anteriores**<br>
>Windows Server 2012 R2 y versiones anteriores no pueden cumplir los mismos objetivos de precisión alta. Estos sistemas operativos no se admiten para una precisión alta.
>
>En estas versiones, el servicio de hora de Windows cumple los siguientes requisitos:
>
> - Proporcionó la precisión de tiempo necesaria para satisfacer los requisitos de autenticación Kerberos versión 5.
> - Se proporcionó un tiempo muy preciso para los clientes y servidores de Windows Unidos a un bosque de Active Directory común.
>
>Las tolerancias mayores en 2012 R2 y versiones posteriores están fuera de la especificación de diseño del servicio de hora de Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Configuración predeterminada de Windows 10 y Windows Server 2016

Aunque se admite la precisión de hasta 1 ms en Windows 10 o Windows Server 2016, la mayoría de los clientes no requieren un tiempo muy preciso.

Como tal, la **configuración predeterminada** está pensada para cumplir los mismos requisitos que los sistemas operativos anteriores, que son:

- Proporcione la precisión de tiempo necesaria para satisfacer los requisitos de autenticación Kerberos versión 5.
- Proporcione tiempo muy preciso para los clientes y servidores de Windows Unidos a un bosque de Active Directory común.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Configuración de sistemas para alta precisión

>[!IMPORTANT]
>**Nota sobre la compatibilidad de sistemas muy precisos**<br>
> La precisión temporal conlleva la distribución de un extremo a otro de la hora exacta desde el origen de hora autoritativo hasta el dispositivo final.  Cualquier cosa que agregue assymetry en las medidas en esta ruta influirá negativamente en la precisión afectará a la precisión que se alcanza en los dispositivos.
>
>Por este motivo, hemos documentado el [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) que describen los requisitos del entorno que también deben cumplirse para alcanzar objetivos de alta precisión.

### <a name="operating-system-requirements"></a>Requisitos del sistema operativo

Las configuraciones de alta precisión requieren Windows 10 o Windows Server 2016.  Todos los dispositivos de Windows en la topología de tiempo deben cumplir este requisito, incluidos los servidores de hora de Windows de estrato superior y, en escenarios virtualizados, los hosts de Hyper-V que ejecutan las máquinas virtuales dependientes del tiempo. Todos estos dispositivos deben ser, como mínimo, Windows 10 o Windows Server 2016.

En la ilustración que se muestra a continuación, las máquinas virtuales que requieren alta precisión ejecutan Windows 10 o Windows Server 2016.  Del mismo modo, el host de Hyper-V en el que residen las máquinas virtuales y el servidor de hora de Windows de nivel superior también debe ejecutar Windows Server 2016.

![Topología de tiempo: 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinar la versión de Windows**<br>
> Puede ejecutar el comando `winver` en un símbolo del sistema para comprobar que la versión del sistema operativo es 1607 (o posterior) y que la compilación del sistema operativo es 14393 (o superior), como se muestra a continuación:
>
> ![Winver-2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuración del sistema

Alcanzar los destinos de alta precisión requiere la configuración del sistema.  Hay varias maneras de realizar esta configuración, entre las que se incluyen directamente en el registro o a través de la Directiva de grupo.  Puede encontrar más información para cada una de estas opciones en la referencia técnica del servicio de hora de Windows: [herramientas de servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo de inicio del servicio de hora de Windows

El servicio de hora de Windows (W32Time) debe ejecutarse continuamente.  Para ello, configure el tipo de inicio del servicio de hora de Windows en Inicio automático.

![Configuración automática](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latencia de red unidireccional acumulativa

La incertidumbre de la medición y el "ruido" se incrementan a medida que aumenta la latencia de red.  Como tal, es imperativo que una latencia de red esté dentro de un límite razonable.  Los requisitos específicos dependen de la precisión de destino y se describen en el artículo [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) .

Para calcular la latencia de red unidireccional acumulativa, agregue los retrasos unidireccionales individuales entre pares de nodos cliente-servidor NTP en la topología de tiempo, empezando por el destino y finalizando en el origen de la hora de la estrato de alta precisión.

Por ejemplo: Considere la posibilidad de una jerarquía de sincronización de tiempo con un origen muy preciso, dos servidores NTP intermedios A y B, y el equipo de destino en ese orden. Para obtener la latencia de red acumulativa entre el destino y el origen, mida el promedio de tiempo de ida y vuelta de NTP individual (RTTs) entre:

- El servidor de destino y hora B
- Servidor horario B y servidor horario A
- El servidor horario A y el origen

Esta medida puede obtenerse mediante la herramienta de bandeja de entrada W32tm. exe.  Para ello:

1. Realice el cálculo desde el servidor de destino y hora B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Realice el cálculo desde el servidor de hora b contra (señalado) el servidor de hora a.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Realice el cálculo del servidor horario a en el origen.
 
4. A continuación, agregue el promedio de RoundTripDelay medido en el paso anterior y divida en 2 para obtener el retraso de red acumulativo entre el destino y el origen.

#### <a name="registry-settings"></a>Configuración del Registro

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura el intervalo más pequeño en LOG2 segundos permitido para el sondeo del sistema.

|  |  | 
|---------|---------|
|Ubicación de la clave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Parámetro    | 6        |
|Resultado | El intervalo de sondeo mínimo es ahora de 64 segundos. |

El comando siguiente indica a la hora de Windows que seleccione la configuración actualizada:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura el intervalo más grande en LOG2 segundos permitido para el sondeo del sistema.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Parámetro    | 6        |
|Resultado | El intervalo de sondeo máximo es ahora de 64 segundos.  |

El comando siguiente indica a la hora de Windows que seleccione la configuración actualizada:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
El número de TICs de reloj entre los ajustes de corrección de fase.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Parámetro    | 100        |
|Resultado | El número de TICs de reloj entre ajustes de corrección de fase es ahora 100 TICs. |

El comando siguiente indica a la hora de Windows que seleccione la configuración actualizada:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura el intervalo de sondeo en segundos cuando se habilita la marca SpecialInterval 0x1.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Parámetro    | 64        |
|Resultado | El intervalo de sondeo es ahora de 64 segundos. |

El siguiente comando reinicia la hora de Windows para seleccionar la configuración actualizada:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM: \ SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Parámetro    | 2        |


---
