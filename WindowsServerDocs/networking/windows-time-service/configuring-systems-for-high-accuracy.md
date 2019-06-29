---
ms.assetid: ''
title: Configuración de sistemas de alta precisión
description: Sincronización de hora de Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, se pueden configurar los sistemas para mantener 1 ms (milisegundos) precisión o superior (con respecto a UTC).
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 9bfa4e7d4f8777f8fef299cf3991238e31564ace
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469594"
---
# <a name="configuring-systems-for-high-accuracy"></a>Configuración de sistemas de alta precisión
>Se aplica a: Windows Server 2016 y Windows 10 versión 1607 o posterior

Sincronización de hora de Windows 10 y Windows Server 2016 se ha mejorado considerablemente.  En condiciones de funcionamiento razonables, se pueden configurar los sistemas para mantener 1 ms (milisegundos) precisión o superior (con respecto a UTC).

Las instrucciones siguientes le ayudarán a configurar los sistemas para lograr una alta precisión.  En este artículo se trata los siguientes requisitos:

- Sistemas operativos admitidos
- Configuración del sistema 

> [!WARNING]
> **Objetivos de la precisión de los sistemas operativos anteriores**<br>
>Windows Server 2012 R2 y a continuación no puede cumplir con los mismos objetivos de alta precisión. No se admiten estos sistemas operativos de alta precisión.
>
>En estas versiones, el servicio de hora de Windows cumple los requisitos siguientes:
>
> - Proporciona la precisión de tiempo necesario para satisfacer los requisitos de autenticación Kerberos versión 5.
> - Proporciona la hora exacta de acoplamiento flexible para los clientes de Windows y servidores unidos a un bosque de Active Directory.
>
>Las tolerancias mayores en 2012 R2 y versiones inferiores están fuera de la especificación de diseño del servicio de hora de Windows.

## <a name="windows-10-and-windows-server-2016-default-configuration"></a>Windows 10 y la configuración predeterminada de Windows Server 2016

Aunque se admite la precisión de hasta 1 ms en Windows 10 o Windows Server 2016, la mayoría de los clientes no requieren un tiempo muy preciso.

Por lo tanto, el **configuración predeterminada** está diseñado para cumplir con los mismos requisitos que los sistemas operativos anteriores que son:

- Proporcione la precisión de tiempo necesario para satisfacer los requisitos de autenticación Kerberos versión 5.
- Proporcionar una hora exacta de acoplamiento flexible para los clientes de Windows y servidores unidos a un bosque de Active Directory.

## <a name="how-to-configure-systems-for-high-accuracy"></a>Cómo configurar los sistemas de alta precisión

>[!IMPORTANT]
>**Tenga en cuenta con respecto a la compatibilidad de sistemas muy precisos**<br>
> Precisión de tiempo conlleva la distribución to-end de la hora exacta desde el origen autorizado de la hora en el dispositivo final.  Todo lo que agrega assymetry en las medidas a lo largo de esta ruta de acceso influirán negativamente precisión afectará a la precisión alcanzable en sus dispositivos.
>
>Por este motivo, hemos documentado el [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) describir los requisitos del entorno que deben cumplirse para alcanzar los objetivos de alta precisión.

### <a name="operating-system-requirements"></a>Requisitos del sistema operativo

Las configuraciones de alta precisión requieren Windows 10 o Windows Server 2016.  Todos los dispositivos de Windows en la topología de tiempo deben cumplir este requisito incluidos los servidores de hora de Windows los satélites superior y en virtualizado escenarios, los Hosts de Hyper-V que se ejecutan las máquinas virtuales de sujetos a limitación temporal. Todos estos dispositivos deben ser al menos Windows 10 o Windows Server 2016.

En la ilustración que se muestra a continuación, las máquinas virtuales que requieren alta precisión ejecutan Windows 10 o Windows Server 2016.  Del mismo modo, el Host de Hyper-V en el que residen las máquinas virtuales y el servidor que precede Windows tiempo también deben ejecutar Windows Server 2016.

![Topología de tiempo - 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/Topology2016.png)


>[!TIP] 
>**Determinar la versión de Windows**<br>
> Puede ejecutar el comando `winver` en un símbolo del sistema para comprobar el sistema operativo es versión 1607 (o superior) y compilación del sistema operativo es 14393 (o posterior) como se muestra a continuación:
>
> ![WINVER - 2016 1607](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/winver2016.png)

### <a name="system-configuration"></a>Configuración del sistema

Alcanzar los objetivos de alta precisión requiere la configuración del sistema.  Hay varias formas para realizar esta configuración, incluido directamente en el registro o mediante la directiva de grupo.  Puede encontrar más información para cada una de estas opciones en Windows Time Service Technical Reference: [herramientas del servicio de hora de Windows](Windows-Time-Service-Tools-and-Settings.md#windows-time-service-tools).

#### <a name="windows-time-service-startup-type"></a>Tipo de inicio de servicio de hora de Windows

El servicio de hora de Windows (W32Time) debe ejecutar continuamente.  Para ello, configure el tipo de inicio del servicio de hora de Windows para el inicio "Automático".

![Configuración automática](../media/Windows-Time-Service/Configuring-Systems-for-High-Accuracy/AutomaticService.PNG)

#### <a name="cumulative-one-way-network-latency"></a>Latencia de red unidireccional acumulativa

Incertidumbre de medición y el "ruido" cuelan como aumentos de latencia de red.  Por lo tanto, es imperativo que sea una latencia de red dentro de los límites razonables.  Los requisitos específicos dependen de la precisión de destino y se describen en la [límite de soporte técnico para configurar el servicio de hora de Windows para entornos de alta precisión](support-boundary.md) artículo.

Para calcular la latencia de red unidireccional acumulativa, agregue los retrasos individuales unidireccionales entre los pares de nodos de cliente / servidor NTP en la topología de tiempo, empezando con el destino y terminando en la capa de alta precisión 1 origen de hora.

Por ejemplo: Considere la posibilidad de una jerarquía de sincronización de hora con un origen de muy preciso, dos servidores NTP intermediarios A y B y el equipo de destino en ese orden. Para obtener la latencia de red acumulativa entre el origen y destino, mide el promedio (RTTs) el tiempo de ida y vuelta NTP individual entre:

- El servidor de destino y la hora B
- Servidor horario B y el servidor de tiempo A
- Servidor de tiempo A y el origen

Esta medida se puede obtener mediante la herramienta w32tm.exe de bandeja de entrada.  Para ello:

1. Realizar el cálculo desde el servidor de destino y la hora B.
    
    `w32tm /stripchart /computer:TimeServerB /rdtsc /samples:450 > c:\temp\Target_TsB.csv`

2. Realiza el cálculo del tiempo servidor b contra (señala a) servidor horario una.
    
    `w32tm /stripchart /computer:TimeServerA /rdtsc /samples:450 > c:\temp\Target_TsA.csv`

3. Realizar el cálculo desde el servidor de hora de una en el origen.
 
4. A continuación, agregue que el RoundTripDelay medio se mide en el paso anterior y dividido entre 2 para obtener el retraso de red acumulativa entre el origen y destino.

#### <a name="registry-settings"></a>Configuración del Registro

# <a name="minpollintervaltabminpollinterval"></a>[MinPollInterval](#tab/MinPollInterval)
Configura el intervalo más pequeño en segundos de log2 permitido para el sondeo del sistema.

|  |  | 
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Parámetro    | 6        |
|Resultado | Ahora, el intervalo de sondeo mínimo es 64 segundos. |

El comando siguiente indica la hora de Windows para recoger la configuración actualizada:

`w32tm /config /update`


# <a name="maxpollintervaltabmaxpollinterval"></a>[MaxPollInterval](#tab/MaxPollInterval)
Configura el intervalo mayor en segundos de log2 permitido para el sondeo del sistema.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config        |
|Parámetro    | 6        |
|Resultado | Ahora, el intervalo de sondeo máximo es 64 segundos.  |

El comando siguiente indica la hora de Windows para recoger la configuración actualizada:

`w32tm /config /update`

# <a name="updateintervaltabupdateinterval"></a>[UpdateInterval](#tab/UpdateInterval)
El número de ciclos de reloj entre los ajustes de corrección de fase.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config       |
|Parámetro    | 100        |
|Resultado | El número de ciclos de reloj entre los ajustes de corrección de fase es ahora 100 pasos. |

El comando siguiente indica la hora de Windows para recoger la configuración actualizada:

`w32tm /config /update`

# <a name="specialpollintervaltabspecialpollinterval"></a>[SpecialPollInterval](#tab/SpecialPollInterval)
Configura el intervalo de sondeo en segundos cuando se habilita la marca SpecialInterval 0 x 1.

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient        |
|Parámetro    | 64        |
|Resultado | Ahora, el intervalo de sondeo es 64 segundos. |

El siguiente comando reinicia Windows tiempo para seleccionar la configuración actualizada:

`net stop w32time && net start w32time`

# <a name="frequencycorrectratetabfrequencycorrectrate"></a>[FrequencyCorrectRate](#tab/FrequencyCorrectRate)

|  |  |  
|---------|---------|
|Ubicación de la clave     | HKLM:\SYSTEM\CurrentControlSet\Services\W32Time\Config      |
|Parámetro    | 2        |


---
