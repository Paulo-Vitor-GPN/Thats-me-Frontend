import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.net.ssl.KeyManagerFactory;
import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManagerFactory;
import java.io.FileInputStream;
import java.io.InputStream;
import java.net.URI;
import java.security.KeyStore;
import java.security.SecureRandom;
import java.util.Properties;

@Component
public class SSLExampleProcessor implements Processor {

    @Value("${auth.keyStorePath}")
    private String keyStorePath;

    @Value("${auth.keyStorePassword}")
    private String keyStorePassword;

    @Value("${auth.trustStorePath}")
    private String trustStorePath;

    @Value("${auth.trustStorePassword}")
    private String trustStorePassword;

    @Override
    public void process(Exchange exchange) throws Exception {
        // Configurar o SSLContext com as propriedades injetadas
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        try (InputStream keyStoreInputStream = new FileInputStream(keyStorePath)) {
            keyStore.load(keyStoreInputStream, keyStorePassword.toCharArray());
        }

        KeyManagerFactory keyManagerFactory = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
        keyManagerFactory.init(keyStore, keyStorePassword.toCharArray());

        KeyStore trustStore = KeyStore.getInstance("PKCS12");
        try (InputStream trustStoreInputStream = new FileInputStream(trustStorePath)) {
            trustStore.load(trustStoreInputStream, trustStorePassword.toCharArray());
        }

        TrustManagerFactory trustManagerFactory = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        trustManagerFactory.init(trustStore);

        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(keyManagerFactory.getKeyManagers(), trustManagerFactory.getTrustManagers(), new SecureRandom());

        // Resto do código permanece o mesmo...
    }
}

        
        
        auth:
  keyStorePath: "/caminho/do/keyStore"
  keyStorePassword: "senhaDoKeyStore"
  trustStorePath: "/caminho/do/trustStore"
  trustStorePassword: "senhaDoTrustStore"
