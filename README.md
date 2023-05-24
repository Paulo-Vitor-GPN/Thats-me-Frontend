import org.apache.camel.Exchange;
import org.apache.camel.Processor;

public class SSLExampleProcessor implements Processor {

    @Override
    public void process(Exchange exchange) throws Exception {
        // Carregar as propriedades de configuração
        Properties properties = new Properties();
        try (InputStream inputStream = getClass().getResourceAsStream("/ssl.properties")) {
            properties.load(inputStream);
        }

        // Configurar o SSLContext com o certificado e chave privada
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        try (InputStream keyStoreInputStream = new FileInputStream(properties.getProperty("keyStorePath"))) {
            keyStore.load(keyStoreInputStream, properties.getProperty("keyStorePassword").toCharArray());
        }

        KeyManagerFactory kmf = KeyManagerFactory.getInstance(KeyManagerFactory.getDefaultAlgorithm());
        kmf.init(keyStore, properties.getProperty("keyStorePassword").toCharArray());

        TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        KeyStore trustStore = KeyStore.getInstance("JKS");
        try (InputStream trustStoreInputStream = new FileInputStream(properties.getProperty("trustStorePath"))) {
            trustStore.load(trustStoreInputStream, properties.getProperty("trustStorePassword").toCharArray());
        }
        tmf.init(trustStore);

        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(kmf.getKeyManagers(), tmf.getTrustManagers(), new SecureRandom());

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
                exchange.getIn().setBody(responseBody);
            }
        }
    }
}
