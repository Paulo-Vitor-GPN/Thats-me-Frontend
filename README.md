package br.com.santander.yzcaml.clientintegration.aggregation;

import br.com.santander.yzcaml.clientintegration.model.dto.*;
import br.com.santander.yzcaml.clientintegration.util.CommonUtils;
import com.google.gson.Gson;
import org.apache.camel.AggregationStrategy;
import org.apache.camel.Exchange;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import java.time.Instant;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class PostAllocationAggregationStrategy implements AggregationStrategy {
    private static final Logger LOGGER = LogManager.getLogger(PostAllocationAggregationStrategy.class);
    private Gson gson = new Gson();

    @Override
    public Exchange aggregate(Exchange exchange, Exchange resource) {

        PostAllocatedQuotas responsePostAllocation = Optional
                .ofNullable(resource.getMessage().getBody(PostAllocatedQuotas.class))
                .orElse(new PostAllocatedQuotas());

        String uuid = (String) exchange.getProperty(CommonUtils.TRACE_ID);
        String stimulusid = (String) exchange.getProperty(CommonUtils.STIMULUS_ID);

        StandardErrorDTO errorDTO = new StandardErrorDTO();

        LOGGER.info("TRACE ID: {}, Stimulu ID: {}, Stage: Payload response [Newcon - POST ALLOCATION]:{} ", uuid, stimulusid, gson.toJson(responsePostAllocation));

        if (responsePostAllocation.getData() == null || responsePostAllocation.getData().isEmpty()){
            List<StandardErrorDTO> errors = new ArrayList<>();
            for (PostAllocatedQuotaError notification : responsePostAllocation.getNotifications()){
                errorDTO.setMessage(notification.getMessage());
                errorDTO.setStatus(notification.getCode());
                errors.add(errorDTO);
            }
            errorDTO.setTimestamp(Instant.now().toEpochMilli());
            resource.getMessage().setBody(errors);
            LOGGER.info("TRACE ID: {}, Stimulu ID: {}, Stage: Payload response front [POST ALLOCATION]:{} ", uuid, stimulusid, gson.toJson(responsePostAllocation));
            return resource;
        }
        if (responsePostAllocation.getData() != null && !responsePostAllocation.getData().isEmpty() && responsePostAllocation.getNotifications().isEmpty()){
            List<PostAllocatedQuota> quotas = new ArrayList<PostAllocatedQuota>();
            for (PostAllocatedQuota quota : responsePostAllocation.getData()) {
                quotas.addAll(responsePostAllocation.getData());
            }
            resource.getMessage().setBody(gson.toJson(quotas));
            resource.setProperty(CommonUtils.LIST_QUOATAS, quotas);
            exchange.setProperty(CommonUtils.BODY, responsePostAllocation);
            LOGGER.info("TRACE ID: {}, Stimulu ID: {}, Stage: Payload request [INSERT Oracle - POST ALLOCATION]:{} ", uuid, stimulusid, gson.toJson(quotas));
            return resource;
        }
        return resource;
    }
}




package br.com.santander.yzcaml.clientintegration.aggregation;

import br.com.santander.yzcaml.clientintegration.model.dto.PostAllocatedQuota;
import br.com.santander.yzcaml.clientintegration.model.dto.PostAllocatedQuotaError;
import br.com.santander.yzcaml.clientintegration.model.dto.PostAllocatedQuotas;
import br.com.santander.yzcaml.clientintegration.model.dto.StandardErrorDTO;
import br.com.santander.yzcaml.clientintegration.util.CommonUtils;
import org.apache.camel.Exchange;
import org.apache.camel.Message;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestInstance;
import org.junit.jupiter.api.TestInstance.Lifecycle;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.fail;
import static org.mockito.Mockito.when;

@TestInstance(Lifecycle.PER_CLASS)
class PostAllocationAggregationStrategyTest {

    @Mock
    Exchange ex;

    @Mock
    Message msg;

    @Mock
    Exchange ex2;

    @Mock
    Message msg2;

    @SuppressWarnings("deprecation")
    @BeforeAll
    public void init() {
        MockitoAnnotations.initMocks(this);
    }

    @Test
    void responseDataNotNull() {
        PostAllocationAggregationStrategy strategy = new PostAllocationAggregationStrategy();
        StandardErrorDTO errorDTO = new StandardErrorDTO();
        PostAllocatedQuotas responsePostAllocation = new PostAllocatedQuotas();
        List<PostAllocatedQuota> listData = new ArrayList<PostAllocatedQuota>();
        PostAllocatedQuota data = new PostAllocatedQuota();
        data.setProductValue(100.0);
        data.setFirstInstallmentValue(100.0);
        data.setEmail("teste@email.com");
        data.setDdd("11");
        data.setCellPhone("999999999");
        data.setPenumper("99999999");
        data.setConsortiumName("teste");
        data.setF1rstName("teste da silva");
        data.setTypeclient("t");
        data.setDocumentNumber("07084680802");
        data.setCodeGroup("testyz");
        data.setCodeQuota(0);
        data.setVersion(0);
        data.setModality("teste");
        data.setChannel("teste");

        listData.add(data);
        responsePostAllocation.setData(listData);

        when(msg2.getBody(PostAllocatedQuotas.class)).thenReturn(responsePostAllocation);
        when(msg2.getBody()).thenReturn(responsePostAllocation);
        when(ex2.getMessage()).thenReturn(msg2);
        when(ex.getMessage()).thenReturn(msg);
        when(ex.getProperty(CommonUtils.TRACE_ID)).thenReturn(UUID.randomUUID().toString());
        when(ex.getProperty(CommonUtils.STIMULUS_ID)).thenReturn("teste");

        try {
            strategy.aggregate(ex, ex2);
        } catch(Exception e) {
            e.printStackTrace();
            fail("Exception nao esperada");
        }
        assertNotNull(strategy);
    }

    @Test
    void responseDataNull() {

        PostAllocationAggregationStrategy strategy = new PostAllocationAggregationStrategy();

        when(msg2.getBody(PostAllocatedQuotas.class)).thenReturn(null);
        when(msg2.getBody()).thenReturn(null);
        when(ex2.getMessage()).thenReturn(msg2);
        when(ex.getMessage()).thenReturn(msg);
        when(ex.getProperty(CommonUtils.TRACE_ID)).thenReturn(UUID.randomUUID().toString());
        when(ex.getProperty(CommonUtils.STIMULUS_ID)).thenReturn("teste");

        try {
            strategy.aggregate(ex, ex2);
        } catch(Exception e) {
            e.printStackTrace();
            fail("Exception nao esperada");
        }
        assertNotNull(strategy);
    }

    @Test
    void responseNotificationNotNull() {
        PostAllocationAggregationStrategy strategy = new PostAllocationAggregationStrategy();
        PostAllocatedQuotas response = new PostAllocatedQuotas();
        List<PostAllocatedQuotaError> notifications = new ArrayList<>();
        PostAllocatedQuotaError notification = new PostAllocatedQuotaError();

        notification.setCode("0");
        notification.setMessage("teste");
        notifications.add(notification);

        StandardErrorDTO errorDTO = new StandardErrorDTO();

        response.setNotifications(notifications);
        errorDTO.setStatus(notification.getCode());
        errorDTO.setMessage(notification.getMessage());

        List<PostAllocatedQuota> listData = new ArrayList<PostAllocatedQuota>();
        response.setData(listData);
        when(msg2.getBody(PostAllocatedQuotas.class)).thenReturn(response);
        when(msg2.getBody()).thenReturn(response);
        when(ex2.getMessage()).thenReturn(msg2);
        when(ex.getMessage()).thenReturn(msg);
        when(ex.getProperty(CommonUtils.TRACE_ID)).thenReturn(UUID.randomUUID().toString());
        when(ex.getProperty(CommonUtils.STIMULUS_ID)).thenReturn("teste");

        try {
            strategy.aggregate(ex, ex2);
        } catch(Exception e) {
            e.printStackTrace();
            fail("Exception nao esperada");
        }
        assertNotNull(strategy);
    }
}
