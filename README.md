import org.bouncycastle.asn1.pkcs.PrivateKeyInfo;
import org.bouncycastle.cert.X509CertificateHolder;
import org.bouncycastle.openssl.PEMParser;
import org.bouncycastle.openssl.jcajce.JcaPEMKeyConverter;
import org.bouncycastle.openssl.jcajce.JcaPEMWriter;

import java.io.FileReader;
import java.io.StringWriter;
import java.security.PrivateKey;
import java.security.cert.X509Certificate;

public class CertificateUtils {

    public static X509Certificate readCertificate(String pathToCert) throws Exception {
        FileReader fileReader = new FileReader(pathToCert);
        PEMParser pemParser = new PEMParser(fileReader);
        X509CertificateHolder certificateHolder = (X509CertificateHolder) pemParser.readObject();
        pemParser.close();

        return new JcaX509CertificateConverter().getCertificate(certificateHolder);
    }

    public static PrivateKey readPrivateKey(String pathToKey) throws Exception {
        FileReader fileReader = new FileReader(pathToKey);
        PEMParser pemParser = new PEMParser(fileReader);
        PrivateKeyInfo privateKeyInfo = (PrivateKeyInfo) pemParser.readObject();
        pemParser.close();

        return new JcaPEMKeyConverter().getPrivateKey(privateKeyInfo);
    }

    public static String convertCertificateToPEM(X509Certificate certificate) throws Exception {
        StringWriter stringWriter = new StringWriter();
        JcaPEMWriter pemWriter = new JcaPEMWriter(stringWriter);
        pemWriter.writeObject(certificate);
        pemWriter.close();

        return stringWriter.toString();
    }

    public static String convertPrivateKeyToPEM(PrivateKey privateKey) throws Exception {
        StringWriter stringWriter = new StringWriter();
        JcaPEMWriter pemWriter = new JcaPEMWriter(stringWriter);
        pemWriter.writeObject(privateKey);
        pemWriter.close();

        return stringWriter.toString();
    }

    public static String createReqGetSignature(String pathToCert, String pathToKey, String systemAcronym) throws Exception {
        X509Certificate certificate = readCertificate(pathToCert);
        PrivateKey privateKey = readPrivateKey(pathToKey);
        
        // Realize as operações necessárias com o certificado e a chave privada
        
        return json.toString();
    }
}
