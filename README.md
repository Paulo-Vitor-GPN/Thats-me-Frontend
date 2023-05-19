import org.apache.camel.CamelContext;
import org.apache.camel.Exchange;
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.impl.DefaultCamelContext;
import org.apache.camel.support.jsse.KeyStoreParameters;
import org.apache.camel.support.jsse.SSLContextParameters;
import org.apache.camel.support.jsse.TrustManagersParameters;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.security.KeyStore;

public class ApiAuthenticationExample {

    public static void main(String[] args) throws Exception {
        // Crie um contexto Camel
        CamelContext camelContext = new DefaultCamelContext();

        // Defina as configurações do certificado
        SSLContextParameters sslContextParameters = new SSLContextParameters();
        KeyStoreParameters keyStoreParameters = new KeyStoreParameters();
        TrustManagersParameters trustManagersParameters = new TrustManagersParameters();

        // Caminho para o arquivo .cer
        String certificatePath = "/caminho/para/certificado.cer";
        // Caminho para o arquivo .key
        String keyPath = "/caminho/para/chave.key";

        // Carregue o arquivo de certificado
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        keyStore.load(null, null);
        Path certificateFile = Paths.get(certificatePath);
        keyStore.load(Files.newInputStream(certificateFile), null);
        keyStoreParameters.setResource(certificatePath);
        keyStoreParameters.setPassword(""); // Senha do certificado, se aplicável

        // Carregue o arquivo de chave privada
        Path keyFile = Paths.get(keyPath);
        sslContextParameters.setKeyManagers(keyStore, null, null);
        sslContextParameters.setKeyStoreParameters(keyStoreParameters);
        sslContextParameters.setUseInsecureTrustManager(true); // Permitir confiar em certificados não confiáveis

        // Defina a rota Camel para fazer a chamada
        camelContext.addRoutes(new RouteBuilder() {
            @Override
            public void configure() throws Exception {
                from("direct:start")
                        .to("https://" + HOST + "?sslContextParameters=#sslContextParameters")
                        .process(exchange -> {
                            // Obtenha a resposta da chamada
                            String responseBody = exchange.getMessage().getBody(String.class);
                            // Faça algo com a resposta
                            System.out.println("Response Body: " + responseBody);
                        });
            }
        });

        // Inicie o contexto Camel
        camelContext.start();

        // Execute a chamada
        camelContext.createProducerTemplate().sendBody("direct:start", "");

        // Aguarde um tempo para a resposta ser processada
        Thread.sleep(2000);

        // Pare o contexto Camel
        camelContext.stop();
    }
}
