import java.io.FileInputStream;
import java.io.InputStream;
import java.security.KeyStore;

import javax.net.ssl.SSLContext;

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.protocol.HttpClientContext;
import org.apache.http.conn.ssl.NoopHostnameVerifier;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.conn.ssl.TrustSelfSignedStrategy;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.ssl.SSLContexts;
import org.apache.http.ssl.SSLContextBuilder;

public class HttpClientExample {
    
    public static void main(String[] args) throws Exception {
        // Chame o método abaixo com as informações adequadas
        makeRequestWithCertificateAndKey("https://api.example.com/endpoint", "path/to/certificate.cer", "path/to/privatekey.key");
    }
    
    public static void makeRequestWithCertificateAndKey(String url, String certificatePath, String privateKeyPath) throws Exception {
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        InputStream keyStoreInputStream = new FileInputStream(certificatePath);
        keyStore.load(keyStoreInputStream, "password".toCharArray());
        
        SSLContext sslContext = SSLContextBuilder.create()
                .loadKeyMaterial(keyStore, "password".toCharArray())
                .loadTrustMaterial(new TrustSelfSignedStrategy())
                .build();
        
        SSLConnectionSocketFactory socketFactory = new SSLConnectionSocketFactory(
                sslContext, new String[] { "TLSv1.2" }, null, NoopHostnameVerifier.INSTANCE);
        
        try (CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(socketFactory).build()) {
            HttpClientContext context = HttpClientContext.create();
            
            HttpGet httpGet = new HttpGet(url);
            try (CloseableHttpResponse response = httpClient.execute(httpGet, context)) {
                HttpEntity entity = response.getEntity();
                // Processar a resposta conforme necessário
            }
        }
    }
}
