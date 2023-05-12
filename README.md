package br.com.santander.afccaml.clientintegration.predicate;

import org.apache.camel.Exchange;
import org.apache.camel.Predicate;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import br.com.santander.afccaml.clientintegration.util.CommonUtils;

public class RetryPredicate implements Predicate {

	private Logger logger = LogManager.getLogger("CONSOLE_JSON_APPENDER");

	@Override
	public boolean matches(Exchange exchange) {
		
		String uuid = (String) exchange.getProperty(CommonUtils.TRACE_ID);
		String cpf = (String) exchange.getProperty(CommonUtils.CPF);

		logger.info("Trace ID: {}, CPF: {}, Retry: {}", uuid, cpf, !(boolean) exchange.getProperty(CommonUtils.RETRY));
		
		return !(boolean) exchange.getProperty(CommonUtils.RETRY);
	}

}



package br.com.santander.bhs.vjcaml.clientintegration.processor.request;

import java.util.List;
import java.util.Optional;

import br.com.santander.bhs.vjcaml.clientintegration.router.util.CommonUtils;
import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import br.com.santander.services.bsb.santanderfinanciamentos.propostacreditosantanderfinanciamentos.v1.ParamsRemoverGarantiaB3;


public class RemoverGarantiaPreLogin implements Processor {

	@Override
	public void process(Exchange exchange) throws Exception {

		ParamsRemoverGarantiaB3 RemoverGarantiaB3 = Optional.ofNullable(exchange.getIn().getBody(ParamsRemoverGarantiaB3.class))
				.orElse(new ParamsRemoverGarantiaB3());

//		ParamsRemoverGarantiaB3 RemoverGarantiaB3 = (ParamsRemoverGarantiaB3) exchange.getIn().getBody(List.class).get(0);

		exchange.setProperty(CommonUtils.HYUNDAI, Boolean.FALSE);

		if(RemoverGarantiaB3.getCodigoMontadora().equals("47")) {
			exchange.setProperty(CommonUtils.HYUNDAI, Boolean.TRUE);
		}
		exchange.setProperty(CommonUtils.PAYLOAD_ENTRADA, RemoverGarantiaB3);
	}
}
