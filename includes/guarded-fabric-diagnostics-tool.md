> [!Note] 
> Cuando ejecutes la herramienta de tejido protegido (Get-HgsTrace - RunDiagnostics), es posible que se devuelva un estado incorrecto que indica que la configuración HTTPS está averiada, pero en realidad no es así o resulta que la configuración no está siendo usada. Este error puede producirse independientemente del modo de atestación de HGS. Existen muchas causas posibles, tal como se explica a continuación:
>
> - Que el protocolo HTTPS está realmente mal configurado o averiado<br>
> - Que estás usando la atestación de administración de confianza y la relación de confianza está rota<br>
> &nbsp;&nbsp;&nbsp;&nbsp;-Se trata con independencia de si HTTPS está configurado correctamente, incorrectamente, o no esté en uso en absoluto.<br>
>
> Ten en cuenta que los diagnósticos solo devolverán este estado incorrecto cuando tengan como destino un host de Hyper-V. Si los diagnósticos tienen como destino el Servicio de protección de host, el estado devuelto será correcto.

<!-- Appears in guarded-fabric-setting-up-the-host-guardian-service-hgs.md and guarded-fabric-troubleshoot-diagnostics.md
-->
