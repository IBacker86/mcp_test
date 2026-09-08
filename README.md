# mcp_test

mvn dependency:tree -Dverbose \


  
SSLContext sslContext = SSLContextBuilder.create()
        .loadKeyMaterial(
                keyStore,
                parametrosCyberArk.keyStorePassword().toCharArray()
        )
        .loadTrustMaterial(null, (chain, authType) -> true)
        .build();

var tlsStrategy = ClientTlsStrategyBuilder.create()
        .setSslContext(sslContext)
        .setHostnameVerifier(NoopHostnameVerifier.INSTANCE)
        .build();


return Feign.builder()
        .client(new ApacheHttp5Client(httpClient))
        .target(clientClass, generaRequestCyberArk(parametrosCyberArk));

var connectionManager = org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManagerBuilder
        .create()
        .setTlsSocketStrategy(tlsStrategy)
        .build();

CloseableHttpClient httpClient = HttpClients.custom()
        .setConnectionManager(connectionManager)
        .build();




