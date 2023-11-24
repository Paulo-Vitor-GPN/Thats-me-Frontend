{
    "codigoGrupo": "ABC123",
    "codigoCota": 12345,
    "versao": 00,
    "nomeConsorciado": "João da Silva",
    "primeiroNome": "João",
    "email": "joao@example.com",
    "dddCelular": "011",
    "numeroCelular": "987654321",
    "cpfCnpj": "123.456.789-01",
    "tipoPessoa": "F",
    "penumper": "PEN123",
    "nomeSubProduto": "Consórcio de Automóveis",
    "FormaAcesso": "web",
    "valorBem": 25000.50,
    "valorPrimeiraParcela": 1500.75
}

package br.com.santander.yzcaml.clientintegration.route.integration;

import br.com.santander.yzcaml.clientintegration.model.dto.PostAllocatedQuota;
import br.com.santander.yzcaml.clientintegration.model.dto.PostAllocatedQuotas;
import br.com.santander.yzcaml.clientintegration.processor.HttpResponseProcessor;
import br.com.santander.yzcaml.clientintegration.processor.LoggerProcessor;
import br.com.santander.yzcaml.clientintegration.processor.PostAllocationProcessor;
import br.com.santander.yzcaml.clientintegration.util.CommonUtils;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.apache.camel.Exchange;
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.model.dataformat.JsonLibrary;
import org.json.JSONObject;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.MediaType;
import org.springframework.stereotype.Component;

import java.io.InputStream;
import java.lang.reflect.Type;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;

@Component
public class PostAllocationRoute extends RouteBuilder {

    @Value("${http.auth.appkey}")
    private String appkey;

    @Override
    public void configure() throws Exception {
        from("direct:post_allocation")
                .routeId("post_allocation_Id")
                .doTry()
                    .process(new PostAllocationProcessor())
                    .setHeader(Exchange.HTTP_METHOD, constant("GET"))
                    .setHeader(Exchange.CONTENT_TYPE, constant(MediaType.APPLICATION_JSON_VALUE))
                    .setHeader(CommonUtils.APPLICATION_KEY, constant(appkey))
                    .to("http://localhost:8083/mock/post-allocation" + CommonUtils.URL_PARAMETERS)
                .unmarshal().json(JsonLibrary.Jackson, PostAllocatedQuotas.class)
                .endDoTry()
                .doCatch(Exception.class)
                    .process(new HttpResponseProcessor())
                    .process(new LoggerProcessor("Connection-oracle", "Error: post allocation"))
                .end();
    }
}
