package br.com.santander.yzcaml.clientintegration.aggregation;

import br.com.santander.yzcaml.clientintegration.model.dto.StandardErrorDTO;
import br.com.santander.yzcaml.clientintegration.model.dto.StimulusDTO;
import br.com.santander.yzcaml.clientintegration.util.CommonUtils;
import org.apache.camel.AggregationStrategy;
import org.apache.camel.Exchange;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;
import org.springframework.http.HttpStatus;

import java.time.Instant;

public class ConsultStimuliParamAggregationStrategy implements AggregationStrategy {

    private static final Logger LOGGER = LogManager.getLogger(ConsultStimuliParamAggregationStrategy.class);

    @Override
    public Exchange aggregate(Exchange exchange, Exchange resource) {

        String uuid = (String) exchange.getProperty(CommonUtils.TRACE_ID);
        String stimulusid = (String) exchange.getProperty(CommonUtils.STIMULUS_ID);
        int responseCode = (int) resource.getMessage().getHeader(Exchange.HTTP_RESPONSE_CODE);
        StandardErrorDTO errorResponse = new StandardErrorDTO();
        Object resourceResponse = resource.getMessage().getBody();

        LOGGER.info("Trace ID: {}, stimulu ID: {}, Stage: Consulta Arsenal [PARAMETRIZADOR], rotorno oracle: {}", uuid, stimulusid, resource.getMessage().getBody());

        try {
            if (responseCode == HttpStatus.NOT_FOUND.value()) {
                errorResponse = (StandardErrorDTO) resourceResponse;
                errorResponse.setMessage(errorResponse.getMessage() != null ? errorResponse.getMessage() : "Houve um erro inesperado no processamento da mensagem");
                errorResponse.setStatus(errorResponse.getStatus() != null ? errorResponse.getStatus() : "400");
                errorResponse.setTimestamp(errorResponse.getTimestamp() != null ? errorResponse.getTimestamp() : Instant.now().toEpochMilli());
                errorResponse.setUrl(errorResponse.getUrl() != null ? errorResponse.getUrl() : "");
                resource.getMessage().setBody(errorResponse);

                exchange.getMessage().setHeader(Exchange.HTTP_RESPONSE_CODE, HttpStatus.BAD_REQUEST.value());
                LOGGER.error("TRACE ID: {}, Stimulu ID: {}, Stage: Consulta Arsenal [PARAMETRIZADOR]: {} ", uuid, stimulusid, errorResponse);
                return resource;
            }
            if (responseCode == HttpStatus.OK.value()) {
                StimulusDTO responseConsultStimuli = (StimulusDTO) resourceResponse;
                if (StimulusDTO.isValidFields(responseConsultStimuli) && responseConsultStimuli.getStimulusId().equals(exchange.getProperty(CommonUtils.STIMULUS_ID))) {
                    exchange.getMessage().setHeader(Exchange.HTTP_RESPONSE_CODE, HttpStatus.ACCEPTED.value());
                    resource.setProperty(CommonUtils.BODY, responseConsultStimuli);
                    resource.getMessage().setBody(false);
                }
                LOGGER.info("TRACE ID: {}, Stimulu ID: {}, Stage: Consulta Arsenal [PARAMETRIZADOR]: {} ", uuid, stimulusid, responseConsultStimuli.toString());
            }
            return resource;
        } catch (Exception e) {
            errorResponse.setMessage("Houve um erro inesperado no processamento da mensagem");
            errorResponse.setStatus("400");
            errorResponse.setTimestamp(Instant.now().toEpochMilli());
            errorResponse.setUrl("");
            resource.getMessage().setBody(errorResponse);
            LOGGER.error("TRACE ID: {}, Stimulu ID: {}, Stage: Consulta Arsenal - erro, cause by: {} ", uuid, stimulusid, e.getMessage());
            return resource;
        }
    }
}
