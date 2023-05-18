import br.com.santander.bhs.vjcaml.clientintegration.router.util.CommonUtils;
import org.apache.camel.Exchange;
import org.apache.camel.Processor;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class LoggerProcessor implements Processor{

    private Logger logger = LogManager.getLogger("CONSOLE_JSON_APPENDER");
    private String etapa;

    public LoggerProcessor(String etapa) {
        this.etapa = etapa;
    }

    @Override
    public void process(Exchange exchange) throws Exception {
        String uuid = (String) exchange.getProperty(CommonUtils.TRACE_ID);

        logger.info("Trace ID: {}, Etapa: {}", uuid, etapa);
    }
}
