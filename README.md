import br.com.santander.bhs.vjcaml.clientintegration.router.util.CommonUtils;

import br.com.santander.services.bsb.santanderfinanciamentos.propostacreditosantanderfinanciamentos.v1.ParamsRemoverGarantiaB3;

import org.apache.camel.Exchange;
import org.apache.camel.Predicate;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import java.util.Optional;

public class CheckManufacturerCodePredicate implements Predicate {
    private Logger logger = LogManager.getLogger("CONSOLE_JSON_APPENDER");

    @Override
    public boolean matches(Exchange exchange) {

        ParamsRemoverGarantiaB3 removerGarantiaB3 = Optional.ofNullable(exchange.getIn().getBody(ParamsRemoverGarantiaB3.class))
                .orElse(new ParamsRemoverGarantiaB3());
        
        String uuid = (String) exchange.getProperty(CommonUtils.TRACE_ID);
        exchange.setProperty(CommonUtils.PAYLOAD_ENTRADA, removerGarantiaB3);
        logger.info("Trace ID: {}, Payload {}", uuid, exchange.getProperty(CommonUtils.PAYLOAD_ENTRADA));

        return !removerGarantiaB3.getCodigoMontadora().equals("15");
    }
}
