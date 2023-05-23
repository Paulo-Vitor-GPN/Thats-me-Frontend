
import org.apache.camel.builder.RouteBuilder;

import org.springframework.beans.factory.annotation.Value;

import org.springframework.boot.SpringApplication;

import org.springframework.boot.autoconfigure.SpringBootApplication;

import org.springframework.context.annotation.Bean;

@SpringBootApplication

public class Application {

    

    @Value("${api.host}")

    private String apiHost;

    

    @Value("${api.cert.path}")

    private String certPath;

    

    @Value("${api.key.path}")

    private String keyPath;

    public static void main(String[] args) {

        SpringApplication.run(Application.class, args);

    }

    

    @Bean

    public RouteBuilder myRouteBuilder() {

        return new RouteBuilder() {

            @Override

            public void configure() throws Exception {

                from("direct:apiAuth")

                    .setHeader("Content-Type", constant("application/x-www-form-urlencoded"))

                    .setHeader("Chave", constant("X"))

                    .setHeader("Host", constant(apiHost))

                    .setHeader("Authorization", constant("Basic " + Base64.encodeBase64String("X:X".getBytes())))

                    .to("https4://" + apiHost + "/oauth/token?sslContextParameters=#sslContextParameters")

                    .log("${body}");

            }

        };

    }

    

    @Bean

    public SSLContextParameters sslContextParameters() throws Exception {

        KeyStoreParameters keyStoreParameters = new KeyStoreParameters();

        keyStoreParameters.setResource(certPath);

        keyStoreParameters.setPassword("changeit");

        KeyManagersParameters keyManagersParameters = new KeyManagersParameters();

        keyManagersParameters.setKeyStore(keyStoreParameters);

        keyManagersParameters.setKeyPassword("changeit");

        SSLContextParameters sslContextParameters = new SSLContextParameters();

        sslContextParameters.setKeyManagers(keyManagersParameters);

        return sslContextParameters;

    }

}

