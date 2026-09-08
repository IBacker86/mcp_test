# mcp_test

mvn dependency:tree -Dverbose \
  

import feign.Feign;
import feign.hc5.ApacheHttp5Client;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URI;
import java.net.URL;
import java.security.KeyStore;
import javax.net.ssl.SSLContext;
import lombok.AccessLevel;
import lombok.NoArgsConstructor;
import org.apache.hc.client5.http.impl.classic.CloseableHttpClient;
import org.apache.hc.client5.http.impl.classic.HttpClients;
import org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManager;
import org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManagerBuilder;
import org.apache.hc.client5.http.ssl.ClientTlsStrategyBuilder;
import org.apache.hc.client5.http.ssl.NoopHostnameVerifier;
import org.apache.hc.core5.ssl.SSLContextBuilder;

/**
 * Clase que ayuda a las multiples conexiones de CyberArk.
 */
@NoArgsConstructor(access = AccessLevel.PRIVATE)
public class FeignClientFactory {

    /**
     * Metodo para crear un cliente con una clase generica.
     *
     * @param clientClass        Clase a la cual va a castear.
     * @param parametrosCyberArk datos de la conexion de CyberArk.
     * @param <T>                Class generica.
     * @return Clase que se quiere obtener.
     */
    public static <T> T createClient(
            Class<T> clientClass,
            DatabaseConfig parametrosCyberArk) {

        try {
            URL url = URI.create(parametrosCyberArk.keyStore()).toURL();

            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod(Constants.GET_METHOD);
            conn.setConnectTimeout(Constants.CONNECT_TIMEOUT);
            conn.setReadTimeout(Constants.READ_TIMEOUT);

            KeyStore keyStore = KeyStore.getInstance(
                    KeyStore.getDefaultType());

            try (InputStream inputStream = conn.getInputStream()) {
                keyStore.load(
                        inputStream,
                        parametrosCyberArk.keyStorePassword().toCharArray());
            }

            SSLContext sslContext = SSLContextBuilder.create()
                    .loadKeyMaterial(
                            keyStore,
                            parametrosCyberArk.keyStorePassword().toCharArray())
                    .loadTrustMaterial(
                            null,
                            (chain, authType) -> true)
                    .build();

            var tlsStrategy = ClientTlsStrategyBuilder.create()
                    .setSslContext(sslContext)
                    .setHostnameVerifier(NoopHostnameVerifier.INSTANCE)
                    .build();

            PoolingHttpClientConnectionManager connectionManager =
                    PoolingHttpClientConnectionManagerBuilder.create()
                            .setTlsSocketStrategy(tlsStrategy)
                            .build();

            CloseableHttpClient httpClient = HttpClients.custom()
                    .setConnectionManager(connectionManager)
                    .build();

            return Feign.builder()
                    .client(new ApacheHttp5Client(httpClient))
                    .target(
                            clientClass,
                            generaRequestCyberArk(parametrosCyberArk));

        } catch (Exception e) {
            throw new IllegalStateException(
                    Constants.ERROR_CREAR_CLIENTE_FEIGN,
                    e);
        }
    }

    /**
     * Metodo que se encarga de generar el request.
     *
     * @param parametrosCyberArk parametros.
     * @return request.
     */
    private static String generaRequestCyberArk(
            final DatabaseConfig parametrosCyberArk) {

        StringBuilder request =
                new StringBuilder(parametrosCyberArk.urlCyberark());

        request.append(parametrosCyberArk.resource())
                .append(Constants.SIGNO_INTERROGACION);

        request.append(Constants.APP_ID)
                .append(Constants.SIGNO_IGUAL)
                .append(parametrosCyberArk.appId());

        request.append(Constants.SIGNO_AMPERSAND);

        request.append(Constants.SAFE)
                .append(Constants.SIGNO_IGUAL)
                .append(parametrosCyberArk.safe());

        request.append(Constants.SIGNO_AMPERSAND);

        request.append(Constants.OBJECT)
                .append(Constants.SIGNO_IGUAL)
                .append(parametrosCyberArk.object());

        return request.toString();
    }
}



package com.banamex.dcmt.dsv.o.account.app.notif.util;

import com.banamex.dcmt.dsv.o.account.app.notif.ao.connector.vo.DatabaseConfig;
import com.banamex.dcmt.dsv.o.account.app.notif.constants.Constants;
import feign.Feign;
import feign.hc5.ApacheHttp5Client;
import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URI;
import java.net.URL;
import java.security.KeyStore;
import javax.net.ssl.SSLContext;
import lombok.AccessLevel;
import lombok.NoArgsConstructor;
import org.apache.hc.client5.http.impl.classic.CloseableHttpClient;
import org.apache.hc.client5.http.impl.classic.HttpClients;
import org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManager;
import org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManagerBuilder;
import org.apache.hc.client5.http.ssl.ClientTlsStrategyBuilder;
import org.apache.hc.client5.http.ssl.NoopHostnameVerifier;
import org.apache.hc.core5.ssl.SSLContextBuilder;

/**
 * Clase que ayuda a las multiples conexiones de CyberArk.
 */
@NoArgsConstructor(access = AccessLevel.PRIVATE)
public class FeignClientFactory {

    /**
     * Metodo para crear un cliente con una clase generica.
     *
     * @param clientClass        Clase a la cual va a castear.
     * @param parametrosCyberArk datos de la conexion de CyberArk.
     * @param <T>                Class generica.
     * @return Clase que se quiere obtener.
     */
    public static <T> T createClient(
            Class<T> clientClass,
            DatabaseConfig parametrosCyberArk) {

        try {
            URL url = URI.create(
                    parametrosCyberArk.keyStore()).toURL();

            HttpURLConnection conn =
                    (HttpURLConnection) url.openConnection();

            conn.setRequestMethod(Constants.GET_METHOD);
            conn.setConnectTimeout(Constants.CONNECT_TIMEOUT);
            conn.setReadTimeout(Constants.READ_TIMEOUT);

            KeyStore keyStore = KeyStore.getInstance(
                    KeyStore.getDefaultType());

            try (InputStream inputStream = conn.getInputStream()) {
                keyStore.load(
                        inputStream,
                        parametrosCyberArk
                                .keyStorePassword()
                                .toCharArray());
            }

            SSLContext sslContext = SSLContextBuilder.create()
                    .loadKeyMaterial(
                            keyStore,
                            parametrosCyberArk
                                    .keyStorePassword()
                                    .toCharArray())
                    .loadTrustMaterial(
                            null,
                            (chain, authType) -> true)
                    .build();

            var tlsStrategy = ClientTlsStrategyBuilder.create()
                    .setSslContext(sslContext)
                    .setHostnameVerifier(
                            NoopHostnameVerifier.INSTANCE)
                    .build();

            PoolingHttpClientConnectionManager connectionManager =
                    PoolingHttpClientConnectionManagerBuilder
                            .create()
                            .setTlsSocketStrategy(tlsStrategy)
                            .build();

            CloseableHttpClient httpClient =
                    HttpClients.custom()
                            .setConnectionManager(connectionManager)
                            .build();

            return Feign.builder()
                    .client(new ApacheHttp5Client(httpClient))
                    .target(
                            clientClass,
                            generaRequestCyberArk(
                                    parametrosCyberArk));

        } catch (Exception e) {
            throw new IllegalStateException(
                    Constants.ERROR_CREAR_CLIENTE_FEIGN,
                    e);
        }
    }

    /**
     * Metodo que se encarga de generar el request.
     *
     * @param parametrosCyberArk parametros.
     * @return request.
     */
    private static String generaRequestCyberArk(
            final DatabaseConfig parametrosCyberArk) {

        StringBuilder request =
                new StringBuilder(
                        parametrosCyberArk.urlCyberark());

        request.append(parametrosCyberArk.resource())
                .append(Constants.SIGNO_INTERROGACION);

        request.append(Constants.APP_ID)
                .append(Constants.SIGNO_IGUAL)
                .append(parametrosCyberArk.appId());

        request.append(Constants.SIGNO_AMPERSAND);

        request.append(Constants.SAFE)
                .append(Constants.SIGNO_IGUAL)
                .append(parametrosCyberArk.safe());

        request.append(Constants.SIGNO_AMPERSAND);

        request.append(Constants.OBJECT)
                .append(Constants.SIGNO_IGUAL)
                .append(parametrosCyberArk.object());

        return request.toString();
    }
}