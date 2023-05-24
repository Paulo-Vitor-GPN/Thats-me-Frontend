import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.conn.ssl.SSLContexts;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.springframework.stereotype.Component;

import javax.net.ssl.SSLContext;
import java.io.FileInputStream;
import java.security.KeyStore;

@Component
public class MyProcessor implements Processor {

    private static final String CERTIFICATE_PATH = "caminho/para/o/certificado.p12";
    private static final String CERTIFICATE_PASSWORD = "senha-do-certificado";
    private static final String HOST = "https://api.example.com";
    private static final String REQUEST_BODY = "Corpo da requisição";

    @Override
    public void process(Exchange exchange) throws Exception {
        // Carregar o certificado .p12
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        try (FileInputStream fis = new FileInputStream(CERTIFICATE_PATH)) {
            keyStore.load(fis, CERTIFICATE_PASSWORD.toCharArray());
        }

        // Configurar o contexto SSL com o certificado
        SSLContext sslContext = SSLContexts.custom()
                .loadKeyMaterial(keyStore, CERTIFICATE_PASSWORD.toCharArray())
                .build();

        // Criar o cliente HTTP com o contexto SSL configurado
        try (CloseableHttpClient httpClient = HttpClients.custom().setSSLContext(sslContext).build()) {
            // Criar a solicitação HTTP POST
            HttpPost httpPost = new HttpPost(HOST);
            httpPost.setHeader("Content-Type", "application/json");
            httpPost.setEntity(new StringEntity(REQUEST_BODY));

            // Executar a solicitação
            try (CloseableHttpResponse response = httpClient.execute(httpPost)) {
                // Obter a resposta
                HttpEntity responseEntity = response.getEntity();
                String responseBody = EntityUtils.toString(responseEntity);

                // Fazer algo com a resposta, como definir o corpo da resposta no Exchange
                exchange.getIn().setBody(responseBody);
            }
        }
    }
}
