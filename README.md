import java.util.ArrayList;

import org.apache.camel.Exchange;
import org.apache.camel.Processor;

import br.com.santander.bhs.vjcaml.clientintegration.dto.removerGarantiaB3.RemoverGarantiaB3_Request_DTO;
import br.com.santander.services.bsb.santanderfinanciamentos.propostacreditosantanderfinanciamentos.v1.ParamsRemoverGarantiaB3;

public class RemoverGarantiaMapRequest implements Processor {
	@Override
	public void process(Exchange exchange) throws Exception {
		
		ParamsRemoverGarantiaB3 payloadEntrada = (ParamsRemoverGarantiaB3) exchange.getProperty("PAYLOADENTRADA");
		RemoverGarantiaB3_Request_DTO outRef = new RemoverGarantiaB3_Request_DTO();
		
		
		String uri = "";
		uri = uri.concat(payloadEntrada.getContrato().concat("/baixa"));
		
		exchange.setProperty("URIB3", uri);
		
		outRef.setChassis(new ArrayList<String>());
		
		for(String chassi : payloadEntrada.getChassiLista().getChassi()) {
			
			outRef.getChassis().add(chassi);
		}
		
		exchange.getIn().setBody(outRef);
		
	}
