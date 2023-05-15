package br.com.santander.bhs.vjcaml.clientintegration.processor.request;

import java.math.BigDecimal;
import java.util.List;

import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.apache.commons.lang3.StringUtils;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import br.com.santander.bhs.vjcaml.clientintegration.model.VJME074;
import br.com.santander.bhs.vjcaml.clientintegration.router.util.CommonUtils;
import br.com.santander.services.bsb.santanderfinanciamentos.propostacreditosantanderfinanciamentos.v1.ParamsSimularParcelas;

public class SimularParcelasMapRequest implements Processor {
	
	 private Logger logger = LogManager.getLogger(SimularParcelasMapRequest.class);
	
	 private String user;
	 
	 public SimularParcelasMapRequest(String user) {
			this.user = user;
		}
	
  @Override
  public void process(Exchange exchange) throws Exception {
	  VJME074 requestTransacao = new VJME074();
	  ParamsSimularParcelas params = (ParamsSimularParcelas)exchange.getIn().getBody(List.class).get(0);
	  
	  populaRequest(params, requestTransacao);
	  logger.info("VJ74 REQUEST: " + CommonUtils.objectToJson(requestTransacao));
	  
	  exchange.getIn().setBody(requestTransacao);
  }
  
  private void populaRequest(ParamsSimularParcelas params, VJME074 requestTransacao) {
  
	  
	  
	  requestTransacao.setQTDPAFN(Integer.parseInt(params.getParcelas().getQuantidade()));
	  requestTransacao.setQTDCALC(Integer.parseInt(params.getCalculo().getQuantidade()));
	  requestTransacao.setVLFINAN(Integer.parseInt(params.getValorFinanciado()));
	  requestTransacao.setFATORAT(Integer.parseInt(params.getCalculo().getFator()));
	  requestTransacao.setCDPACOF(Integer.parseInt(params.getTabelaFinanciamento().getPacote()));
	  
	  requestTransacao.setCDCOT01(params.getPremios().getPremio().get(1).getCodigoCotacao());
	  requestTransacao.setVLPRE01(new BigDecimal(params.getPremios().getPremio().get(1).getValor()));
	  requestTransacao.setCDCOT02(params.getPremios().getPremio().get(2).getCodigoCotacao());
	  requestTransacao.setVLPRE02(new BigDecimal(params.getPremios().getPremio().get(2).getValor()));
	  requestTransacao.setCDCOT03(params.getPremios().getPremio().get(3).getCodigoCotacao());
	  requestTransacao.setVLPRE03(new BigDecimal(params.getPremios().getPremio().get(3).getValor()));
	  requestTransacao.setCDCOT04(params.getPremios().getPremio().get(4).getCodigoCotacao());
	  requestTransacao.setVLPRE04(new BigDecimal(params.getPremios().getPremio().get(4).getValor()));
	  requestTransacao.setCDCOT05(params.getPremios().getPremio().get(5).getCodigoCotacao());
	  requestTransacao.setVLPRE05(new BigDecimal(params.getPremios().getPremio().get(5).getValor()));
	  requestTransacao.setCDCOT06(params.getPremios().getPremio().get(6).getCodigoCotacao());
	  requestTransacao.setVLPRE06(new BigDecimal(params.getPremios().getPremio().get(6).getValor()));
	  requestTransacao.setCDCOT07(params.getPremios().getPremio().get(7).getCodigoCotacao());
	  requestTransacao.setVLPRE07(new BigDecimal(params.getPremios().getPremio().get(7).getValor()));
	  requestTransacao.setCDCOT08(params.getPremios().getPremio().get(8).getCodigoCotacao());
	  requestTransacao.setVLPRE08(new BigDecimal(params.getPremios().getPremio().get(8).getValor()));
	  requestTransacao.setCDCOT09(params.getPremios().getPremio().get(9).getCodigoCotacao());
	  requestTransacao.setVLPRE09(new BigDecimal(params.getPremios().getPremio().get(9).getValor()));
	  requestTransacao.setCDCOT10(params.getPremios().getPremio().get(10).getCodigoCotacao());
	  requestTransacao.setVLPRE10(new BigDecimal(params.getPremios().getPremio().get(10).getValor()));
	  requestTransacao.setVMUSUAR(user);
  }
}
