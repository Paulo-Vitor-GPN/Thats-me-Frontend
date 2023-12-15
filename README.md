package br.com.santander.yzcaml.clientintegration.aggregation;

import br.com.santander.yzcaml.clientintegration.model.dto.StandardErrorDTO;
import br.com.santander.yzcaml.clientintegration.model.dto.StimulusDTO;
import br.com.santander.yzcaml.clientintegration.util.CommonUtils;
import org.apache.camel.Exchange;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInstance;

import java.time.Instant;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.mock;
import static org.mockito.Mockito.when;

@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class ConsultStimuliParamAggregationStrategyTest {

    private ConsultStimuliParamAggregationStrategy strategy;

    @BeforeAll
    void setUp() {
        strategy = new ConsultStimuliParamAggregationStrategy();
    }

    @Test
    void handleNotFoundResponse() {
        Exchange exchange = mock(Exchange.class);
        Exchange resource = mock(Exchange.class);

        when(exchange.getProperty(CommonUtils.TRACE_ID)).thenReturn("testTraceId");
        when(exchange.getProperty(CommonUtils.STIMULUS_ID)).thenReturn("testStimulusId");

        when(resource.getMessage().getHeader(Exchange.HTTP_RESPONSE_CODE)).thenReturn(404);
        StandardErrorDTO errorResponse = new StandardErrorDTO();
        errorResponse.setMessage("Not Found");
        when(resource.getMessage().getBody()).thenReturn(errorResponse);

        strategy.aggregate(exchange, resource);

        // Add assertions based on the expected behavior for a not found response
        // For example:
        assertEquals(400, exchange.getMessage().getHeader(Exchange.HTTP_RESPONSE_CODE));
        // Add more assertions based on your specific requirements
    }

    @Test
    void handleOkResponse() {
        Exchange exchange = mock(Exchange.class);
        Exchange resource = mock(Exchange.class);

        when(exchange.getProperty(CommonUtils.TRACE_ID)).thenReturn("testTraceId");
        when(exchange.getProperty(CommonUtils.STIMULUS_ID)).thenReturn("testStimulusId");

        when(resource.getMessage().getHeader(Exchange.HTTP_RESPONSE_CODE)).thenReturn(200);
        StimulusDTO responseStimuli = new StimulusDTO();
        responseStimuli.setStimulusId("testStimulusId");
        when(resource.getMessage().getBody()).thenReturn(responseStimuli);

        strategy.aggregate(exchange, resource);

        // Add assertions based on the expected behavior for a successful response
        // For example:
        assertEquals(202, exchange.getMessage().getHeader(Exchange.HTTP_RESPONSE_CODE));
        // Add more assertions based on your specific requirements
    }
}
