import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.apache.camel.Exchange;
import org.apache.camel.Processor;
import org.apache.camel.builder.RouteBuilder;
import org.apache.camel.spring.boot.CamelSpringBootApplicationController;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.stereotype.Component;
import java.util.Iterator;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@Component
class MyRoute extends RouteBuilder {
    @Override
    public void configure() throws Exception {
        from("direct:validateJson")
            .process(new JsonValidatorProcessor())
            .to("direct:nextRoute");
        
        from("direct:nextRoute")
            .log("JSON is valid")
            .end();
    }
}

class JsonValidatorProcessor implements Processor {
    @Override
    public void process(Exchange exchange) throws Exception {
        String jsonString = exchange.getIn().getBody(String.class);
        
        ObjectMapper objectMapper = new ObjectMapper();
        JsonNode jsonNode = objectMapper.readTree(jsonString);
        
        if (hasNullOrBlankField(jsonNode)) {
            throw new IllegalArgumentException("O JSON contém pelo menos um campo nulo ou vazio");
        }
        
        // O JSON não possui campos nulos ou vazios, faça o processamento necessário
    }
    
    private boolean hasNullOrBlankField(JsonNode jsonNode) {
        Iterator<String> fieldNames = jsonNode.fieldNames();
        
        while (fieldNames.hasNext()) {
            String fieldName = fieldNames.next();
            JsonNode fieldValue = jsonNode.get(fieldName);
            
            if (fieldValue == null || fieldValue.isNull() || fieldValue.isTextual() && fieldValue.asText().isEmpty()) {
                return true;
            }
        }
        
        return false;
    }
}
