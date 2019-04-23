Como alternativa, puede especificar una huella digital si desea usar su propio certificado. Esto puede ser útil si desea compartir un certificado en varias máquinas, o use un certificado enlazado a un HSM o de un TPM. Este es un ejemplo de cómo crear un certificado enlazado a TPM (que impide tener la clave privada robado y usarse en otra máquina y requiere un TPM 1.2):

```powersehll
$tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
```


<!-- Appears in set-up-hgs-for-always-encrypted-in-sql-server.md and guarded-fabric-create-host-key.md
-->