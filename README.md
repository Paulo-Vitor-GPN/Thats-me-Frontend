import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.FileInputStream;
import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.nio.file.Paths;
import java.security.KeyStore;
import java.security.SecureRandom;
import java.util.Properties;

public class SSLExample {
    public static void main(String[] args) {
        // Carregar as propriedades de configuração
        Properties properties = new Properties();
        try (FileInputStream fis = new FileInputStream("ssl.properties")) {
            properties.load(fis);
        } catch (IOException e) {
            e.printStackTrace();
            return;
        }

        // Configurar o SSLContext com o certificado e chave privada
        SSLContext sslContext;
        try {
            KeyStore keyStore = KeyStore.getInstance("PKCS12");
            keyStore.load(new FileInputStream(properties.getProperty("keyStorePath")),
                    properties.getProperty("keyStorePassword").toCharArray());

            KeyManagerFactory kmf = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
            kmf.init(keyStore, properties.getProperty("keyStorePassword").toCharArray());

            TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
            KeyStore trustStore = KeyStore.getInstance("JKS");
            trustStore.load(new FileInputStream(properties.getProperty("trustStorePath")),
                    properties.getProperty("trustStorePassword").toCharArray());
            tmf.init(trustStore);

            sslContext = SSLContext.getInstance("TLS");
            sslContext.init(kmf.getKeyManagers(), tmf.getTrustManagers(), new SecureRandom());
        } catch (Exception e) {
            e.printStackTrace();
            return;
        }

        // Configurar o HttpClient com o SSLContext personalizado
        SSLConnectionSocketFactory sslSocketFactory = new SSLConnectionSocketFactory(sslContext,
                new String[]{"TLSv1.2"}, null, SSLConnectionSocketFactory.getDefaultHostnameVerifier());

        try (CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(sslSocketFactory).build()) {
            // Fazer uma requisição HTTPS
            URIBuilder uriBuilder = new URIBuilder("https://exemplo.com/recurso");
            URI uri = uriBuilder.build();

            HttpGet httpGet = new HttpGet(uri);
            HttpResponse response = httpClient.execute(httpGet);
            HttpEntity entity = response.getEntity();

            if (entity != null) {
                String responseBody = EntityUtils.toString(entity);
                System.out.println(responseBody);
            }
        } catch (IOException | URISyntaxException e) {
            e.printStackTrace();
        }
    }
}
