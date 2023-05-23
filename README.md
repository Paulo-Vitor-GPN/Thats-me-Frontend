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



<dependencies dependencies >
    <!-- ... outras dependências do Spring Boot ... -->
    
    <dependency>
        <groupId>org.apache.camel</groupId>
        <artifactId>camel-spring-boot-starter</artifactId>
        <version>3.15.0</version>
    </dependency>
    
    <dependency>
        <groupId>org.apache.camel</groupId>
        <artifactId>camel-http4</artifactId>
        <version>3.15.0</version>
    </dependency>
    
    <dependency>
        <groupId>org.apache.camel</groupId>
        <artifactId>camel-jackson</artifactId>
        <version>3.15.0</version>
    </dependency>
</dependencies>



import org.apache.camel.CamelContext;

import org.apache.camel.ProducerTemplate;

import org.springframework.beans.factory.annotation.Autowired;

import org.springframework.web.bind.annotation.GetMapping;

import


