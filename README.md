package br.com.santander.afccaml.clientintegration.util;

import static org.mockito.Mockito.when;

import org.junit.Before;
import org.junit.Test;
import org.mockito.MockitoAnnotations;
import org.springframework.context.annotation.Bean;

import br.com.santander.dlb.pemsignature.PEMSignature;

public class CreateReqGetSigntureTest {
	@Before
	public void init() {
		MockitoAnnotations.initMocks(this);
	}
	
	 @Bean 
	 public String environmentVariable() { 
		 
		 return environmentVariable();  
	 }
	@Test
	public void createReqGetSigntureTest() {
		String pathToPem = "";
		String systemAcronym = "EM";
		when(PEMSignature.readPemFile(pathToPem)).thenReturn(PEMSignature.readPemFile(""));
		try {
			String str = CommonUtils.createReqGetSignture(pathToPem, systemAcronym);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}

}
