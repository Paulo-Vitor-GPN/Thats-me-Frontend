import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

import javax.net.ssl.KeyManagerFactory;
import javax.net.ssl.SSLContext;
import java.io.FileInputStream;
import java.io.InputStream;
import java.security.KeyStore;

@Component
public class SSLExampleProcessor implements Processor {

    @Value("${auth.keyStorePath}")
    private String keyStorePath;

    @Value("${auth.keyStorePassword}")
    private String keyStorePassword;

    @Override
    public void process(Exchange exchange) throws Exception {
        // Carregar o arquivo de certificado P12
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        try (InputStream keyStoreInputStream = new FileInputStream(keyStorePath)) {
            keyStore.load(keyStoreInputStream, keyStorePassword.toCharArray());
        }

        // Criar o KeyManagerFactory com o certificado
        KeyManagerFactory keyManagerFactory = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
        keyManagerFactory.init(keyStore, keyStorePassword.toCharArray());

        // Criar o SSLContext e inicializá-lo com o KeyManagerFactory
        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(keyManagerFactory.getKeyManagers(), null, null);

        // Configurar o contexto SSL no exchange para uso posterior
        exchange.setProperty("sslContext", sslContext);
    }
}
