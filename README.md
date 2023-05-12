import br.com.santander.bhs.vjcaml.clientintegration.router.util.CommonUtils;
import br.com.santander.services.bsb.santanderfinanciamentos.propostacreditosantanderfinanciamentos.v1.ParamsRemoverGarantiaB3;
import org.apache.camel.Exchange;
import org.apache.camel.Predicate;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import java.util.Optional;

public class CheckManufacturerCodePredicate implements Predicate {
    private Logger logger = LogManager.getLogger(CheckManufacturerCodePredicate.class);

    @Override
    public boolean matches(Exchange exchange) {
        ParamsRemoverGarantiaB3 paramsRemoverGarantiaB3 = Optional.ofNullable(exchange.getIn().getBody(ParamsRemoverGarantiaB3.class))
                .orElse(new ParamsRemoverGarantiaB3());

        String uuid = (String) exchange.getProperty(CommonUtils.TRACE_ID);
        exchange.setProperty(CommonUtils.PAYLOAD_ENTRADA, paramsRemoverGarantiaB3);
        logger.info("Trace ID: {}, Payload: {}", uuid, paramsRemoverGarantiaB3);

        return !paramsRemoverGarantiaB3.getCodigoMontadora().equals("15");
    }
}
