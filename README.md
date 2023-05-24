
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
        
        
        auth:
  keyStorePath: "/caminho/do/keyStore"
  keyStorePassword: "senhaDoKeyStore"
  trustStorePath: "/caminho/do/trustStore"
  trustStorePassword: "senhaDoTrustStore"
