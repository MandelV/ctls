# CTLS

Ctls is a fork of go `crypto/tls` to add the TLS Extension into ClientHello info as implemented there : https://go-review.googlesource.com/c/go/+/471396

You can then retrieve it by GetCertificat or GetConfigForClient or directly in tls.Con  callback from tls.Config : 

```go
package main 

func main(){


 	cer, err := tls.LoadX509KeyPair("cert.crt", "key.key")
	if err != nil {
		return nil, err
	}   

    tlsConfig := &tls.Config{
		Certificates: []tls.Certificate{cer},
	}

    tlsConfig.GetConfigForClient = func(chi *tls.ClientHelloInfo) (*tls.Config, error) {
		    CipherSuites        := chi.CipherSuites
		    ServerName          := chi.ServerName
		    SupportedCurves     := chi.SupportedCurves
		    SupportedPoints     := chi.SupportedPoints
		    SupportedVersions   := chi.SupportedVersions
		    Extensions          := chi.Extensions

            /* ... */
        }
}
```

in tls.Con

```go

if tlsCon, ok := con.(*tls.Conn); ok && tlsCon != nil {
	chi := tlsCon.ClientHelloInfo

	/* ... */
}

```