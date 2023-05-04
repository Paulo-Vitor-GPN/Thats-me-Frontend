package br.com.santander.vjcaml.clientintegration;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import br.com.santander.bhs.component.altair.annotation.EnableCamelIntegrationAltair;
import br.com.santander.bhs.integration.annotation.EnableIntegrationCore;

@EnableCamelIntegrationAltair
@EnableIntegrationCore
@SpringBootApplication(scanBasePackages = { 
    "br.com.santander.vjcaml.clientintegration", 
    "br.com.santander.tecnico.dlbcripto", 
    "br.com.santander.component" 
})
public class IntegrationApplication {

	public static void main(String[] args) {
		SpringApplication.run(IntegrationApplication.class, args);
	}
}
