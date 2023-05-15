package br.com.santander.afccaml.clientintegration.processor.altair;

import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.apache.commons.lang3.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import br.com.santander.afccaml.clientintegration.model.altair.dto.VJME940;
import br.com.santander.afccaml.clientintegration.model.dto.NewInsurances;
import br.com.santander.afccaml.clientintegration.model.dto.Proposal;
import br.com.santander.afccaml.clientintegration.util.CommonUtils;

public class VJ94AltairProcessor implements Processor {

    private static final Logger LOGGER = LoggerFactory.getLogger(VJ94AltairProcessor.class);
    public static final String TRANSACAO = "VJ94";
    
    @Override
    public void process(Exchange exchange) {

        Proposal proposal = exchange.getIn().getBody(Proposal.class);
        VJME940 request = new VJME940();

        String userPayload = StringUtils.defaultString(proposal.getUserCode());
		userPayload = CommonUtils.recuperaUserVJorPNR(userPayload, exchange);
        exchange.getIn().setHeader(CommonUtils.USER_VJ_PNR, userPayload.trim());

        LOGGER.info("VJ94 (Usuario Altair) : " + exchange.getIn().getHeader(CommonUtils.USER_VJ_PNR));	

        request.setUsuario(userPayload);
		request.setTransacao(TRANSACAO);

        request.setCodigoEmpresa(StringUtils.leftPad(StringUtils.defaultString(proposal.getCredit().getCompany()), 5, "0"));
		request.setFilial(StringUtils.leftPad(StringUtils.defaultString(proposal.getCredit().getBranch()), 5, "0"));
		request.setCanal(StringUtils.leftPad(StringUtils.defaultString(proposal.getCredit().getChannel()), 4, "0"));
		request.setIntermediario(StringUtils.leftPad(StringUtils.defaultString(proposal.getCredit().getIntermediaryNumber()), 7, "0"));
		request.setTipoEntrada(StringUtils.leftPad(StringUtils.defaultString(proposal.getCredit().getInputChannel()), 3, "0"));
		request.setSubsegmento(StringUtils.leftPad(StringUtils.defaultString(proposal.getCredit().getSubSegmentCode()), 5, "0"));

        request.setInsuranceAmount(StringUtils.leftPad(proposal.getNewProductsInsurances().getNewProductsAmount(), 2, "0"));
        request.setInsuranceTotalValue(StringUtils.leftPad(proposal.getNewProductsInsurances().getNewProductsTotalValue(), 13, "0"));
        
        listFillment(proposal, request);        

        LOGGER.info("VJ94 REQUEST: " + CommonUtils.objectToJson(request));

        exchange.getIn().setBody(request);
    }

    static public void listFillment(Proposal proposal, VJME940 request) {

        String serviceCode = "", partnerCode = "", insuranceValue = "";
        int occurs = 0;

        for(NewInsurances newInsurance : proposal.getNewProductsInsurances().getNewInsurances()) {

            serviceCode = StringUtils.leftPad(StringUtils.defaultString(newInsurance.getServiceCode()), 3, "0");
            partnerCode = StringUtils.leftPad(StringUtils.defaultString(newInsurance.getPartnerCode()), 11, "0");
            insuranceValue = StringUtils.leftPad(StringUtils.defaultString(newInsurance.getInsuranceValue()), 10, "0");

            if(occurs < 5) {
                request.setNewProductsTable1(StringUtils.join(request.getNewProductsTable1(), serviceCode, partnerCode, insuranceValue));
            } else {
                request.setNewProductsTable2(StringUtils.join(request.getNewProductsTable2(), serviceCode, partnerCode, insuranceValue));
            }

            occurs++;
        }

        request.setNewProductsTable1(StringUtils.rightPad(request.getNewProductsTable1(), 120, "0"));
        request.setNewProductsTable2(StringUtils.rightPad(request.getNewProductsTable2(), 120, "0"));
    }
}
